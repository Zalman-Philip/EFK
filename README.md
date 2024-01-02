# install elasticsearch

1. helm install elasticsearch bitnami/elasticsearch

# install kibana

2. helm install kibana bitnami/kibana \
   --set elasticsearch.external.hosts[0]=elasticsearch.default.svc.cluster.local \
   --set elasticsearch.external.port=9200

# apply the configmap files

3. kubectl apply -f .

# or

kubectl apply -f elasticsearch-output.yaml

kubectl apply -f apache-log-parser.yaml

# install fluentd

4. helm install fluentd bitnami/fluentd \
   --set aggregator.configMap=elasticsearch-output \
   --set forwarder.configMap=apache-log-parser \
   --set aggregator.extraEnvVars[0].name=ELASTICSEARCH_HOST \
   --set aggregator.extraEnvVars[0].value=elasticsearch.default.svc.cluster.local \
   --set aggregator.extraEnvVars[1].name=ELASTICSEARCH_PORT \
   --set-string aggregator.extraEnvVars[1].value=9200 \
   --set forwarder.extraEnvVars[0].name=FLUENTD_DAEMON_USER \
   --set forwarder.extraEnvVars[0].value=root \
   --set forwarder.extraEnvVars[1].name=FLUENTD_DAEMON_GROUP \
   --set forwarder.extraEnvVars[1].value=root

# install wordpress

5. helm install wordpress bitnami/wordpress
