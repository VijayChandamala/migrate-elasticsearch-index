# Migrating and elasticsearch index using logstash

All you have to do is configure logstash with the below configuration.


Logstash image: docker.elastic.co/logstash/logstash-oss:7.5.1


Add this example.conf file in logstash home path, in my case /usr/share/logstash

## config file (/usr/share/logstash/migrate.conf)

```

input {
    elasticsearch {
        hosts => ["host-ip"]
       # user => "*******"
       # password => "*********"
        index => "index-name"
        size => 1000
        scroll => "1m"
    }
}
# a note in this section indicates that filter can be selected
#filter {
#}
output {
    elasticsearch {
        hosts => ["host-ip"]
       # user => "********"
       # password => "**********"
        index => "index-name"
    }
}

```

Then run this command to migrate data from one cluster to another

bin/logstash -f es-mig.conf --path.data ./logs/

For verfying index in elasticsearch 
curl localhost:9200/_cat/indices


For checking plugin list in logstash
logstash-plugin list --verbose

Remember that the index names should be same in both the places.
