##Enable the repo for the elastic search
          
                  cat > /etc/yum.repos.d/elasticsearch.repo <<EOF
                  [elasticsearch-8.x]
                  name=Elasticsearch repository for 8.x packages
                  baseurl=https://artifacts.elastic.co/packages/8.x/yum
                  gpgcheck=1
                  gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
                  enabled=1
                  autorefresh=1
                  type=rpm-md
                  EOF 
                  

##install elastic search

          dnf -y install elasticsearch 
         
##enable the service and start it
       
         systemctl enable --now elasticsearch 
         

##manually set some password for elastic search

          /usr/share/elasticsearch/bin/elasticsearch-reset-password -i -u elastic

          This tool will reset the password of the [elastic] user.
          You will be prompted to enter the password.
          Please confirm that you would like to continue [y/N]y
          Enter password for [elastic]: 
          Re-enter password for [elastic]: 
          Password for the [elastic] user successfully reset.

##check the   cluster details##
         
          curl -u elastic --cacert /etc/elasticsearch/certs/http_ca.crt https://127.0.0.1:9200
          Enter host password for user 'elastic':
          {
                      "name" : "server101.example.com",
                      "cluster_name" : "elasticsearch",
                      "cluster_uuid" : "fc0iSKeITMGhavcl4MC4Mw",
                                "version" : {
                                  "number" : "8.17.0",
                                  "build_flavor" : "default",
                                  "build_type" : "rpm",
                                  "build_hash" : "2b6a7fed44faa321997703718f07ee0420804b41",
                                  "build_date" : "2024-12-11T12:08:05.663969764Z",
                                  "build_snapshot" : false,
                                  "lucene_version" : "9.12.0",
                                  "minimum_wire_compatibility_version" : "7.17.0",
                                  "minimum_index_compatibility_version" : "7.0.0"
                                          },
                      "tagline" : "You Know, for Search"
          }


#creation of index

      curl -X PUT -u elastic:password --cacert /etc/elasticsearch/certs/http_ca.crt   https://192.168.12.166:9200/college

#to list the indexs

      curl -u elastic:password --cacert /etc/elasticsearch/certs/http_ca.crt https://192.168.12.166:9200/_cat/indices?v

#putting records into index
    
      curl -X PUT -u elastic:password --cacert /etc/elasticsearch/certs/http_ca.crt   https://192.168.12.166:9200/college/_doc/1 -H 'Content-Type: application/json' -d '{ "name": "sai", "title":"leader" }'

      
      curl -X PUT -u elastic:password --cacert /etc/elasticsearch/certs/http_ca.crt   https://192.168.12.166:9200/college/_doc/2 -H 'Content-Type: application/json' -d '{ "name": "jhon", "title":"leader1" }'

#Retrive the doc from the index

      curl -X GET -u elastic:password --cacert /etc/elasticsearch/certs/http_ca.crt   https://192.168.12.166:9200/college/_doc/1

      curl -X GET -u elastic:password --cacert /etc/elasticsearch/certs/http_ca.crt   https://192.168.12.166:9200/college/_search?size=10

#delete the doc

      curl -X DELETE -u elastic:password --cacert /etc/elasticsearch/certs/http_ca.crt https://192.168.12.166:9200/college/_doc/1


          
