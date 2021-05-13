## Konglabs.io
> docker-compose -f workshop_bootstrap_elk.yml up postgres \
> docker-compose -f workshop_bootstrap_elk.yml up -d postgres \
> docker-compose -f workshop_bootstrap_elk.yml up -d kong-ent-bootstrap \
> docker-compose -f workshop_bootstrap_elk.yml up -d kong-ent \
> docker-compose -f workshop_bootstrap_elk.yml up -d elasticsearch \
> docker-compose -f workshop_bootstrap_elk.yml up -d logstash \
> docker-compose -f workshop_bootstrap_elk.yml up -d kibana

> http put localhost:8001/services/httpbin  url=http://httpbin.org \
> http put localhost:8001/services/httpbin/routes/httpbin paths:='["/httpbin"]' \
> http localhost:8000/httpbin/anything \
> http :8001/services/httpbin/plugins name=key-auth \
> http :8000/httpbin \
> http PUT :8001/consumers/consumer1 \
> http :8001/consumers/consumer1/key-auth key=kong1 \
> http PUT :8001/consumers/consumer2 \
> http :8001/consumers/consumer2/key-auth key=kong2 \
> http get :8000/httpbin/anything apikey:kong1 \
> http get :8000/httpbin/anything apikey:kong2 \
> docker ps
> http post :8001/plugins name=http-log config:='{"http_endpoint": "http://logstash:5044"}' \
> http :9200/_cat/indices \
> http localhost:9200/_cat/indices \

> for i in {1..5} \
do \
	http get :8000/httpbin/anything apikey:kong1 \
	http get :8000/httpbin/anything apikey:kong2 \
done \
> http http://192.168.1.124:9200/kong-*/_search




In the search bar,
response.status >= 400 and response.status <= 599

> for i in {1..5} \
do \
   http get :8000/httpbin/delay/.2 apikey:kong1 \
   http get :8000/httpbin/delay/.5 apikey:kong2 \
done

In the search bar,
> latencies.proxy > 400


> number=$((RANDOM % 10)) \
> for ((i=1; i<=number; i++)) \
do \
   http get :8000/httpbin/anything \
done


In the search bar,
> response.status: 401


#### Configure ACL and Allowed Group
> http -f post localhost:8001/services/httpbin/plugins \ \
> name=acl \ \
> config.allow=group1

> http get :8000/httpbin/anything apikey:kong1

> http post localhost:8001/consumers/consumer1/acls \ \
"group=group1"

> number=$((RANDOM % 10))
> for ((i=1; i<=number; i++)) \
do \
   http get :8000/httpbin/anything apikey:kong1 \
   http get :8000/httpbin/anything apikey:kong2 \
done

In the search bar,
> response.status: 403
