## Creating index template

Before populate/create the index you need to create an index template, with mappings to personal geo pointer fields like src_ip_geoip and dest_ip_geoip.

Considering if the index was start with "logstash-iwsva" you need to run this command line:

**IF ELASTICSEARCH HAS SECURITY ENABLED RUN THIS COMMAND CHANGING THE USER AND PASSWORD FIELDS IN CURL:**

    curl -u user:"password" -X PUT "http://ELASTICSEARCH_IP:9200/_template/logstash-iwsva?pretty" -H 'Content-Type: application/json' -d'{"index_patterns": ["logstash-iwsva-*"],"mappings": {"properties": {"dst_ip_geoip": {"properties": {"city_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"continent_code": {"type": "keyword"},"country_code2": {"type": "keyword"},"country_code3": {"type": "keyword"},"country_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"dma_code": {"type": "long"},"ip": {"type": "ip"},"latitude": {"type": "float"},"location": {"type": "geo_point"},"longitude": {"type": "float"},"postal_code": {"type": "keyword"},"region_code": {"type": "keyword"},"region_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"building": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"timezone": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}}}},"src_ip_geoip": {"properties": {"city_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"continent_code": {"type": "keyword"},"country_code2": {"type": "keyword"},"country_code3": {"type": "keyword"},"country_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"dma_code": {"type": "long"},"ip": {"type": "ip"},"latitude": {"type": "float"},"location": {"type": "geo_point"},"longitude": {"type": "float"},"postal_code": {"type": "keyword"},"region_code": {"type": "keyword"},"region_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"building": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"timezone": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}}}}}}
    
  **IF ELASTICSEARCH DONT HAVE SECURITY ENABLED RUN THIS COMMAND**

    curl -X PUT "http://ELASTICSEARCH_IP:9200/_template/logstash-iwsva?pretty" -H 'Content-Type: application/json' -d'{"index_patterns": ["logstash-iwsva-*"],"mappings": {"properties": {"dst_ip_geoip": {"properties": {"city_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"continent_code": {"type": "keyword"},"country_code2": {"type": "keyword"},"country_code3": {"type": "keyword"},"country_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"dma_code": {"type": "long"},"ip": {"type": "ip"},"latitude": {"type": "float"},"location": {"type": "geo_point"},"longitude": {"type": "float"},"postal_code": {"type": "keyword"},"region_code": {"type": "keyword"},"region_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"building": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"timezone": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}}}},"src_ip_geoip": {"properties": {"city_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"continent_code": {"type": "keyword"},"country_code2": {"type": "keyword"},"country_code3": {"type": "keyword"},"country_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"dma_code": {"type": "long"},"ip": {"type": "ip"},"latitude": {"type": "float"},"location": {"type": "geo_point"},"longitude": {"type": "float"},"postal_code": {"type": "keyword"},"region_code": {"type": "keyword"},"region_name": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"building": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}},"timezone": {"type": "text","fields": {"keyword": {"type": "keyword","ignore_above": 256}}}}}}}'

## Explanation about translate:
**Create the local geoip translate documents:**

Supose i have 2 source private networks then i need to geo point both. I will need to create a yml to use in translation process.

**The private networks was in example:**

Network 1 - 172.16.0.0/22 (172.16.0.1 - 172.16.3.254)

Network 2 - 172.16.4.0/22 (172.16.4.1 - 172.16.7.254)

**Is need to create a regular expression that match with my networks:**

To Network 1 the regular expression was 172\.16\.[0-3]\.

To Network 2 the regular expression was 172\.16\.[4-7]\.

**To create a translate file change with your data and run the command line:**

    cat <<EOF > /etc/logstash/translation/iwsvasrc.yml
    '172\.16\.[0-3]\.': '{"src_ip_geoip": {"latitude": -8.2426, "longitude": -35.7852, "location": [ -35.7852, -8.2426 ], "country_name": "Brazil", "country_code2": "BR", "continent_code": "SA", "region_name": "Pernambuco", "timezone": "America/Recife", "region_code": "PE", "country_code3": "BR", "city_name": "BEZERROS", "building": "COURT OF JUSTICE OF BEZERROS" }}'
    '172\.16\.[4-7]\.': '{"src_ip_geoip": {"latitude": -8.4297, "longitude": -37.0334, "location": [ -37.0334, -8.4297 ], "country_name": "Brazil", "country_code2": "BR", "continent_code": "SA", "region_name": "Pernambuco", "timezone": "America/Recife", "region_code": "PE", "country_code3": "BR", "city_name": "PESQUEIRA", "building": "COURT OF JUSTICE OF PESQUEIRA" }}'
    EOF

## To populate the information about geo point you may need this:

**Coordenates:**

https://www.mapcoordinates.net/en

**Country_name and Country_code:**

https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2

**Region_code:**

https://en.wikipedia.org/wiki/ISO_3166-2

**Continent codes:**

https://datahub.io/core/continent-codes/r/0.html

**Timezone:**

https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

**Building:**

This information is personal, if you not neet can be removed.


## Open and change the file "/etc/logstash/config.d/02-iwsva-filter.conf"
**change this**

    # If IP is private then drop it
    if [dst_locality] == "private" {
        drop {}
    }

**to this:**

    # If IP is private then drop it
    if [dst_locality] == "private" {
        geoip {
            source => "src_ip"
            target => "src_ip_geoip"
        }
        translate {
            regex => true
            dictionary_path => "/etc/logstash/translation/iwsvasrc.yml"
            field => "src_ip"
        }
    }
  
  **After change your file start Logstash**
