I use Minikube for my personal projects, and I prefer running it with the Docker driver as it simplifies resource management for me. 

![Minikube started using Docker driver](minikube-started.png)

First, I tried to apply the original manifest to the cluster. And we get our first error:

![Yaml spacing error](error-1.png)

I've come across this issue quite often while learning. The error is related to a broken YAML structure. As expected, I just needed to add an extra space. VSC highlighted the mistake, which made it easy to spot.

![Yaml spacing fix](fix-1.png)

I applied the file again, and the first error is gone, but we got another one:

![Naming error](error-2.png)

There were two typos in the field names. I cdouble-checked in the Kubernetes docs to get the correct spelling for requiredDuringSchedulingIgnoredDuringExecution and targetPort.

I fixed the typos and applied the file again. It works now:

![Naming fix](fix-2.png)

However, the pod wasn't coming up, so I checked the logs. We can see that it's resource allocation issue. It seems we assigned more resources in the manifest than my Minikube node could handle. 

![CPU error](error-3.png)
![CPU error](error-3-1.png)


I edited the deployment, so it needs less resources:

![CPU error](fix-3.png)

But then the pod got stuck in Pending. The deployment required a label that my Minikube node didn't have.

I added the label , and that fixed the scheduling issue. The pod is up and running now.

![Label fix](fix-4.png)

Next, we need a service to access the Nginx page. I saw that the label selector was wrong. It didn't match the Pod labels.

![Selector error](error-5.png)

I fixed the typo so the labels matched (`sretest`) and applied the file again. To open the app, I used the `minikube service` command, which is the easiest way to access it here. It creates a tunnel to the NodePort service.

![Service](fix-5-1.png)

And here we go. The page works correctly:

![nginx](nginx.png)