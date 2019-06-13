### Connection
```
beeline -n hive -u "jdbc:hive2://quickstart.cloudera:10000/default"

```

### Create Call table
```
CREATE EXTERNAL TABLE calls (
    user STRING
    , other STRING
    , direction STRING
    , duration INT
    , `timestamp` STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/data/calls'
TBLPROPERTIES("skip.header.line.count"="1");
```

### Create Messages table
```
CREATE EXTERNAL TABLE messages (
    user STRING
    , other STRING
    , direction STRING
    , length INT
    , `timestamp` STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/data/messages'
TBLPROPERTIES("skip.header.line.count"="1");
```

### Create presences table
```
CREATE EXTERNAL TABLE presences (
    user STRING
    , other STRING
    , name STRING
    , `class` STRING
    , `timestamp` STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/data/presences'
TBLPROPERTIES("skip.header.line.count"="1");
```
### Create presences_enriched
```
CREATE TABLE presences_enriched (
    user STRING
    , other STRING
    , name STRING
    , `class` STRING
    , `timestamp` STRING
    , utc_timestamp TIMESTAMP
)
PARTITIONED BY (the_date STRING)
STORED AS ORC
TBLPROPERTIES("orc.compress"="NONE");

```
### Add PARTITION to table
```
INSERT OVERWRITE TABLE presences_enriched PARTITION(the_date);
```

### Examples
```
SELECT
    user, other, name, class, `timestamp`,
    to_utc_timestamp(unix_timestamp(`timestamp`, "EEE MMM dd HH:mm:ss Z YYYY") * 1000, "PST") AS utc_timestamp,
    from_unixtime(unix_timestamp(`timestamp`, "EEE MMM dd HH:mm:ss Z YYYY"), "yyyy-MM-dd")
FROM presences;

DESCRIBE FORMATTED presences;

DESCRIBE FORMATTED presences_enriched;

```
### Example-2

```
SELECT user, other, length, to_utc_timestamp(c_unix_timestamp, "PST") AS c_utc_timestamp, to_utc_timestamp(m_unix_timestamp, "PST") AS m_utc_timestamp, `offset` FROM (
    SELECT c.user, c.other, c.unix_timestamp AS c_unix_timestamp, m.length, m.unix_timestamp AS m_unix_timestamp, (unix_timestamp(m.unix_timestamp) - unix_timestamp(c.unix_timestamp)) AS `offset`
    FROM (
        SELECT user, other, duration, `timestamp`, CAST((unix_timestamp(`timestamp`, "EEE MMM dd HH:mm:ss Z YYYY") * 1000) AS TIMESTAMP) AS unix_timestamp FROM calls
    ) c
    LEFT JOIN (
        SELECT user, other, length, `timestamp`, CAST((unix_timestamp(`timestamp`, "EEE MMM dd HH:mm:ss Z YYYY") * 1000) AS TIMESTAMP) AS unix_timestamp FROM messages
    ) m
    ON (
        c.user = m.user AND c.other = m.other
    )
    WHERE c.duration = 0 AND c.unix_timestamp < m.unix_timestamp AND ((unix_timestamp(m.unix_timestamp) - unix_timestamp(c.unix_timestamp))) < 60 AND ((unix_timestamp(m.unix_timestamp) - unix_timestamp(c.unix_timestamp))) > 0
) x
ORDER BY c_utc_timestamp, `offset`;
```
