# docker-selks

Docker based Suricata, Elasticsearch, Logstash, Kibana, Scirius aka SELKS.

Note: Currently, Scirius is deactivated but h2o has been brought into the mix,
so that we currently are a `docker-shelk`, if you want.

## Setup

### Linux and Windows > 7

- Install [docker](http://docker.io).
- Install [docker-compose](http://docs.docker.com/compose/install/).
- Clone this repository

Then, start your stack using *docker-compose*:

```bash
docker-compose up
```

### Windows up to Windows 7

On Windows, use [Vagrant](https://www.vagrantup.com/)

For Vagrant be sure to have the following vagrant plugins installed

- [vagrant-proxyconf](https://github.com/tmatilai/vagrant-proxyconf) when behind a croporate proxy
- [vagrant-docker-compose](https://github.com/leighmcculloch/vagrant-docker-compose)
- [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest)
- [vagrant-cachier](https://github.com/fgrehm/vagrant-cachier)

Start up the box

```bash
vagrant up
```

Connect into the box via ssh/putty on `127.0.0.1:2222` with standard login `vagrant/vagrant`. Then,

```bash
cd /vagrant
docker-compose [ps,logs, ...]
```

## Using the stack

In the current configuration, Suricata alerts __everything__ going through
interface `eth0`. Logstash looks for these alerts and puts them into the
elasticsearch. Kibana is used to look at the details. The evebox can also
be used to look at suricata alerts.

You can access Kibana on port `5601`, Evebox at port `5636`, elasticsearch at
`9200` and h2o at `54321`.

If the entire container is running, you can still log into any service by
running `docker-compose exec <servicename> /bin/bash`. This is especially
useful if you want to let suricata run on additional PCAPs while it is also
monitoring an interface. So you can do

```bash
docker-compose exec suricata /bin/bash
suricata -c /etc/suricata/suricata.yml --pcap-file-continuous -r /pcap
```

if you want to monitor the `/pcap` directory continuously. Without the
`--pcap-file-continuous` option, `-r` expects a single pcap file instead
of a directory.
