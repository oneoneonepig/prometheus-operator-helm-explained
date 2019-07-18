# prometheus-operator-helm-explained

## Install prometheus-operator through helm

```
helm install --name prom --namespace prom stable/prometheus-operator \
--set prometheusOperator.createCustomResource=false \
--set prometheus.prometheusSpec.replicas=2 \
--set prometheus.prometheusSpec.retention=1d \
--set prometheus.prometheusSpec.scrapeInterval=15s \
--set prometheus.prometheusSpec.evaluationInterval=15s \
--set alertmanager.alertmanagerSpec.replicas=2 \
--set alertmanager.alertmanagerSpec.retention=12h \
--set alertmanager.alertmanagerSpec.logFormat=logfmt
```

## Upgrade alertmanager.yaml

```
kubectl delete secret -n prom alertmanager-prom-prometheus-operator-alertmanager
kubectl create secret generic -n prom alertmanager-prom-prometheus-operator-alertmanager --from-file=./prometheus/alertmanager.yaml
# kubectl delete pod -n prom -l alertmanager=prom-prometheus-operator-alertmanager,app=alertmanager
```

## Clean up

```
helm delete --purge prom

kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd podmonitors.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
```
