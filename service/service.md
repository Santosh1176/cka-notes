**Types of Services:**

There are mainly Three types of Services in Kubernetes. The First is of type **NodePort**, the second **Cluster IP**, and the third one **LoadBalancer.**

T*he Service is created in a same fashion to enable outside traffic to access the contents of the Pod. Either it can be a Single Node cluster within a Single Node, Multiple Pods within Single Node, Or in multiple Pods withing multiple Nodes spanned within single cluster. The Service acts in the same way in all the cases, due the Selector field which is give the properties of the Pod labels to tell the Service, which Pods the Service needs to target.*

---

- **NodePort Service type**: This enable the outside users interact with the Pod inside a node seamlesly. It uses Ports for that, The Port listining on the Pod is known as **TargetPort**,That is where the Service forwards the requests to.
- The second type of Port is the Port listining on the Cluster IP of the Node is simply reffered to as **Port**, which is the Port of the Service itself. Inside the cluster the Service has its own IP address as well. Which is known as **Cluster IP**.
- Finally, we have a Port on the Node itself, which we connect to access the pods inside the nodes from outside. This Port is known as **NodePort**.  NodePorts need to be in a valid range, which is from **30000 to 32767.** If this is not provided in the manifests, the system will allocate any free port between the range automatically.

A sample manifest for Service type NodePort:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
	type: NodePort  #type determines how the Service is exposed.Defaults to ClusterIP.
									#Valid options are ExternalName, ClusterIP, NodePort, and LoadBalancer.
  ports: 
    - targetPort: 80 #Is the Port where the Pod is exposed.
      port: 80  #Is the Port where the Service is Exposed.
      nodePort: 3008
	selector: #This is an mandatory firld, to link Pods in a node 
						#which this Service will target.
			app: my-app #This a lable pulled from the Pod manifests in metadata field.
			type: front-end #This is a lable fron the Pod manifest aswell. 
```

A Sample manifest for Service type **ClusterIP :**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
	type: CLusterIP #type determines how the Service is exposed.Defaults to ClusterIP.
									#Valid options are ExternalName, ClusterIP, NodePort, and LoadBalancer.
  ports: 
    - targetPort: 80 #Where the Pod is exposed.
      port: 80       #Where the Service is exposed.
  selector: #This is an mandatory field which is used to link Pods in a node 
						#which this Service will target.
			app: my-app #This a lable pulled from the Pod manifests in metadata field.
			type: back-end #This is a lable fron the Pod manifest aswell. 
```

A Sample manifest for Service type LoadBalancer:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
	type: LoadBalancer  #type determines how the Service is exposed.Defaults to ClusterIP.
									    #LoadBalancer works only with supported Cloud providers else is eual to NodePort.
  ports: 
    - targetPort: 80 #Is the Port where the Pod is exposed.
      port: 80  #Is the Port where the Service is Exposed.
      nodePort: 3008
	selector: #This is an mandatory firld, to link Pods in a node 
						#which this Service will target.
			app: my-app #This a lable pulled from the Pod manifests in metadata field.
			type: front-end #This is a lable fron the Pod manifest aswell. 
```
            