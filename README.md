# trend-iwsva-elk
ELK config for Trend Micro IWSVA Proxy

## Setup instructions
* Setup an elastic stack: https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elastic-stack-on-ubuntu-16-04
* Copy all conf files from conf.d folder to /etc/logstash/conf.d/trend-iwsva.conf
* Copy all yml files from translation folder to /etc/logstash/translation/site_categories.yml
* In Trend IMSVA add new Syslog Server Settings to point to your Elastic Stack IP using UDP port 5000
* Select the required logs (currently supported):
  * Tick "URL access (Info)"
  * Tick "Spyware/Grayware (Crit)"
  * Tick "Virus (Crit)"
  * C&C callback (Crit)

## Logs matches Todo
- [x] URL Access Logs
- [x] Virus Logs
- [x] Spyware Logs
- [ ] DLP Logs
- [x] C&C callback Logs
- [ ] URL blocking Logs
- [x] Performance Logs
- [ ] System event Logs
- [ ] Audit Logs
- [ ] Protocol Blocking Logs
- [ ] FTP get Logs
- [ ] FTP put Logs

## Other improvements todo
- [x] Ignore local IP addresses from GeoIP Lookups
- [x] Translate local IP addresses (Read LOCAL_IP.MD)

Feel free to contribute and improve
