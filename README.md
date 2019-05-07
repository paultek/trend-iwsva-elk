# trend-iwsva-elk
ELK config for Trend Micro IWSVA Proxy

## Setup instructions
* Setup an elastic stack: https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-ubuntu-16-04
* Copy trend-iwsva.conf to /etc/logstash/conf.d/trend-iwsva.conf
* Copy site_categories.yml to /etc/logstash/translation/site_categories.yml
* In Trend IMSVA add new Syslog Server Settings to point to your Elastic Stack IP using port 5000
  * Tick "URL access (Info)"
  * Tick "Spyware/Grayware (Crit)"
  * Tick "Virus (Crit)"

## Todo
- [x] URL Access Logs
- [x] Virus Logs
- [x] Spyware Logs
- [ ] DLP Logs
- [ ] C&C callback Logs
- [ ] URL blocking Logs
- [ ] Performance Logs
- [ ] System event Logs
- [ ] Audit Logs
- [ ] Protocol Blocking Logs
- [ ] FTP get Logs
- [ ] FTP put Logs

Feel free to contribute and improve