INSTRUCTIONS FOR DEPLOYING AND TESTING THE TODO APPLICATION

This document provides step-by-step instructions on how to deploy 
and verify the ToDo application within a Kubernetes cluster.

------------------------------------------------------------------
1. DEPLOYING MANIFESTS
------------------------------------------------------------------

First, navigate to the directory containing the manifest files:
$ cd .infrastructure

Apply the manifests in the following order to ensure proper setup:

1.1 Create the Namespace:
    $ kubectl apply -f namespace.yml

1.2 Launch the main application Pod:
    $ kubectl apply -f todoapp-pod.yml

1.3 Launch the helper Pod for testing (busybox):
    $ kubectl apply -f busybox.yml


------------------------------------------------------------------
2. TESTING VIA PORT-FORWARD
------------------------------------------------------------------

To access the application's web interface from your local machine:

1. Execute the port-forwarding command:
   $ kubectl port-forward todoapp-pod 8000:8000 -n todoapp

2. Open your web browser and navigate to:
   http://localhost:8000


------------------------------------------------------------------
3. TESTING VIA BUSYBOXPLUS:CURL CONTAINER
------------------------------------------------------------------

Use the busybox container to verify internal cluster connectivity:

1. Obtain the internal IP address of the todoapp-pod:
   $ kubectl get pod todoapp-pod -n todoapp -o wide

2. Test the Liveness Probe (Expect "200 OK"):
   $ kubectl exec -it busybox -n todoapp -- curl http://<TODOAPP_IP_ADDRESS>:8000/healz/live

3. Test the Readiness Probe (Expect "Ready"):
   $ kubectl exec -it busybox -n todoapp -- curl http://<TODOAPP_IP_ADDRESS>:8000/healz/read

NOTE: Replace <TODOAPP_IP_ADDRESS> with the IP address discovered in step 1.