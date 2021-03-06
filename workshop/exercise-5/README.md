# Exercise 5 - Expose the service mesh with the Istio Ingress Gateway

The components deployed on the service mesh by default are not exposed outside the cluster. External access to individual services so far has been provided by creating an external load balancer or node port on each service.

An Ingress Gateway resource can be created to allow external requests through the Istio Ingress Gateway to the backing services.

### Expose the Guestbook app with Ingress Gateway

1. If you have a paid cluster:

    a. Configure the guestbook default route with the Istio Ingress Gateway. The `guestbook-gateway.yaml` file is in this repository (istio101) in the `workshop/plans` directory.

    ```shell
    kubectl create -f guestbook-gateway.yaml
    ```

    b. Get the **EXTERNAL-IP** of the Istio Ingress Gateway.

    ```shell
    kubectl get service istio-ingressgateway -n istio-system
    ```
    Output:
    ```shell
    NAME                   TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)                                       AGE
    istio-ingressgateway   LoadBalancer   172.21.254.53    169.6.1.1       80:31380/TCP,443:31390/TCP,31400:31400/TCP    1m
    2d
    ```

    c. Make note of the external IP address that you retrieved in the previous step, as it will be used to access the Guestbook app in later parts of the course. You can create an environment variable called $INGRESS_IP with your IP address.

    Example:
    ```
    export INGRESS_IP=169.6.1.1
    ```

2. If you have a lite cluster:

    a. Configure the guestbook default route with the Istio Ingress Gateway.

    ```shell
    kubectl create -f guestbook-gateway.yaml
    ```

    b. Now check the node port of the ingress.

    ```shell
    kubectl get svc istio-ingressgateway -n istio-system
    ```
    Output:
    ```shell
    NAME            TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
    istio-ingress   LoadBalancer                    *              80:31702/TCP,443:32290/TCP   10d
    ```
    Get the Public IP of your cluster.
    ```shell
    ibmcloud cs workers <cluster_name>
    ```
    Output:
    ```shell
    ID             Public IP      Private IP      Machine Type        State    Status   Zone    Version
    kube-xxx       169.60.87.20   10.188.80.69    u2c.2x4.encrypted   normal   Ready    wdc06   1.9.7_1510*
    ```

    The node port in above sample output is `169.60.87.20:31702`.

    c. Make note of the IP and node port that you retrieved in the previous step as it will be used to access the Guestbook app in later parts of the course. You can create an environment variable called $INGRESS_IP with your IP address.

    Example:
    ```
    export INGRESS_IP=169.60.87.20:31702
    ```

Congratulations! You extended the base Ingress features by providing a DNS entry to the Istio service.

## References:
[Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
[Istio Ingress](https://istio.io/docs/tasks/traffic-management/ingress.html)

#### [Continue to Exercise 6 - Traffic Management](../exercise-6/README.md)
