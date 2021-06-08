# Forcing a Deletion of a Namespace
- **Reference Documentation Links**
- https://kubernetes.io/blog/2021/05/14/using-finalizers-to-control-deletion/
#### Create a namespace
```
kubectl create ns test
```
#### Deploy the nginx
```
kubectl run nginx --image=nginx -n test
```
#### List the the pods
```
kubectl get pods -n test
```
#### List the namespaces
```
kubectl get ns
```
#### Run the proxy in one terminal
```
kubectl proxy --port=8080 &
```
#### Delete the test namespace
```
cat <<EOF | curl -X PUT \
  localhost:8080/api/v1/namespaces/test/finalize \
  -H "Content-Type: application/json" \
  --data-binary @-
{
  "kind": "Namespace",
  "apiVersion": "v1",
  "metadata": {
    "name": "test"
  },
  "spec": {
    "finalizers": null
  }
}
EOF

```
#### Kill the kubectl proxy process
```
ps -ef | grep kubectl
kill -9 processid
```
