## Hadoop Distributed File System HDFS  
hadoop fs -komut şeklinde bir yazımı vardır. Linux komutlarına aşinaysanız kullanımı oldukça basittir.

#### Listeleme

/ dizini altıdaki dizin ve dosyaları listeler
* hadoop fs -ls /  

#### Localden dosya kopylama

* hadoop fs -copyFromLocal <source_file/dir> <destination_dir>

#### HDFS'den locale dosya kopyalama

* hadoop fs -copyToLocal  <source_file/dir> <destination_dir>


#### Dizin yaratma

* hadoop fs -mkdir <dir_name>

#### Dosya / Dizin Silme

* hadoop fs -rm -r -f <dir | file name>
