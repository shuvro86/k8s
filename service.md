![WhatsApp Image 2026-01-18 at 1 28 41 AM](https://github.com/user-attachments/assets/8c304d33-bfe8-4616-9256-06c6af8b622e)



`$ cat pod_vproapp.yaml`
```
apiVersion: v1
kind: Pod
metadata:
  name: vproapp
  labels:
    app: vproapp
spec:
  containers:
      - name: appcontainer
        image: imranvisualpath/freshtomapp:V7
        ports:
        - name: vproapp-port
          containerPort: 8080
```


`$ cat svc_vproapp.yaml`
```apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  type: NodePort
  selector:
    app: vproapp
  ports:
    - protocol: TCP
      port: 8090
      targetPort: 8080
      nodePort: 30001
```
