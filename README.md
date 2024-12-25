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
         
##check the cluster health
        
        curl -X GET "http://localhost:9200/_cluster/health?pretty"
