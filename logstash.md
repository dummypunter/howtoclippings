# Logstash

## Write output to file

Logstash configuration file:

`# vi /etc/logstash/logstash.conf`
 
Output to file if the field foo matches a patern:
``` 
output {
  if [foo] =~ /pattern/ {
    file {
      path => "/datacollection/lostash/output/ossec-%{+YYYY-MM-dd}.log"
    }
  }
}
```
Restart the service:

`/etc/init.d/logstash restart`

