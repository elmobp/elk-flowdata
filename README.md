# What is it?

ELK setup to take in flow data and give you searchable dashboards based on any current flow information, as all current flow information being used here delivers destination ports ect at layer 4 the logstash agent will gsub each port to what is defined in RFC for that part. Though both resulsts are still visible in the dashboards.
<br><br>
Reverse DNS has also been enabled for src and dst, if you scroll to the bottom of the dashboard in your brwose you will seee the searchable fields on your left!
<br><br>
These configs are designed to run single instance per service! Do not mix them up unless you have lots of spare CPU and RAM<br><br>

# Installing<br>
![netflow](https://github.com/mbakerbp/elk-flowdata/raw/master/Netflow/screenshots/dashboard.png)<br>
This is for Ubuntu 14 only ;)
<br>
<br><b>Lots of pre req command first</b>
<br> The next item is critical as we will be copying configs from git to the server we need to make sure they are right!
<br>ENVIROMENT=Netflow<br>
ENVIROMENT=VPC<br>
<b>Only choose one of the above</b><br>
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
<br>cd $ENVIROMENT
<br>``DIR=`pwd```
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
<br>service nginx restart
<br>service elasticsearch restart
<br>service logstash restart
<br>mkdir -p /var/www/kibana
<br>cd /var/www/kibana
<br>wget https://download.elastic.co/kibana/kibana/kibana-3.1.2.tar.gz
<br>tar zxf kibana-3.1.2.tar.gz --strip-components=1
<br>rm kibana-3.1.2.tar.gz
<br>cp $DIR/var/www/kibana/config.js .
<br>cp $DIR/var/www/kibana/app/dashboards/default.json app/dashboards/default.json
<br><br>
# All installed!
Sit back and wait for the magic, if you do not see anything come up open up /etc/logstash/conf.d/logstash.conf and goto the bottom and uncomment out the line: 
```
stdout { codec => rubydebug }
```
<br>

Then stop logstash and cd /opt/logstash/bin;./logstash -f /etc/logstash/conf.d/logstash.conf --verbose --debug and look for any possible config problems there.



