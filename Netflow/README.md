# Installing<br>
This is for Ubuntu 14 only ;)
<br>
<br><b>Lots of pre req command first</b>
<br>sudo add-apt-repository -y ppa:webupd8team/java
<br>sudo apt-get update
<br>sudo apt-get -y install oracle-java8-installer
<br>wget https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.6.0.deb
<br>sudo dpkg -i elasticsearch-1.6.0.deb
<br>sudo service elasticsearch restart
<br>sudo update-rc.d elasticsearch defaults 95 10
<br>sudo apt-get install nginx
<br>echo 'deb http://packages.elasticsearch.org/logstash/1.5/debian stable main' | sudo tee /etc/apt/sources.list.d/logstash.list
<br>sudo apt-get update
<br>sudo apt-get install logstash
<br>rm /etc/nginx/sites-enabled/default
<br>cp etc/nginx/sites-enabled/kibana /etc/nginx/sites-enabled/
<br>cp etc/logstash/conf.d/logstash.conf /etc/logstash/conf.d/logstash.conf
<br>
cd /opt/logstash/vendor/geoip/
<br>
wget http://download.maxmind.com/download/geoip/database/asnum/GeoIPASNum.dat.gz
<br>
gunzip GeoIPASNum.dat.gz
<br>
wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz
<br>
gunzip GeoLiteCity.dat.gz
<br>
/opt/logstash/bin/plugin update
<br>
<br>service nginx restart
<br>service elasticsearch restart
<br>service logstash restart
<br>DIR=`pwd`
<br>mkdir -p /var/www/kibana
<br>cd /var/www/kibana
<br>wget https://download.elastic.co/kibana/kibana/kibana-3.1.2.tar.gz
<br>tar zxf kibana-3.1.2.tar.gz --strip-components=1
<br>rm kibana-3.1.2.tar.gz
<br>cp $PWD/var/www/kibana/config.js .
<br>cp $PWD/var/www/kibana/app/dashboards/default.json app/dashboards/default.json
<br><br>
Start sending Netflow data v5 on port 5556 to this box and open up the browser!


