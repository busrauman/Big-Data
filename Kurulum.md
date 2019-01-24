# Cloudera Manager Ortamının Docker Konteynırı ile Kurulması
### Docker Kurulumu
curl -sSL https://get.docker.com | bash

### Cloudera Docker Image Kurulumu
[Cloudera](https://www.cloudera.com/downloads/quickstart_vms/5-13.html) sitesinden Docker Image seçeneği seçilerek aşağıdaki adımlar ile kuruluma başlanır.
Eğer uzak makineye kurulum yapmak isterseniz. Aşağıdaki komut ile direkt image'i indirebilirsiniz.
```
wget https://downloads.cloudera.com/demo_vm/docker/cloudera-quickstart-vm-5.13.0-0-beta-docker.tar.gz

```
* tar -xvf cloudera-quickstart-vm-5.13.0-0-beta-docker.tar.gz
* cd cloudera-quickstart-vm-5.13.0-0-beta-docker/
* docker import cloudera-quickstart-vm-5.13.0-0-beta-docker.tar
* docker images
* docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 7180:7180 -p 8888:8888 7a709428a3e7 /usr/bin/docker-quickstart

Burdaki 7180 portu ile cloudera manager'ının arayüzüne erişimi; 8888 portu ile de Hue arayüzüne erişimi veririz. -d parametresini de eklersek deamon olarak arka planda konteynırı ayağı kaldırmış oluruz.


Konteynerın komut satırından
* /home/cloudera/cloudera-manager --express
komutu ile Cloudera servisini başlatalım.

```
http://localhost:7180
Username: cloudera
Password: cloudera

http://localhost:8888 Hue arayüzü
username/pass = cloudera/cloudera

```

#### Tekrardan Konternerın komut satırına girmek istersek
* docker ps -a
* docker exec -it <Contaıner_ID>
