Blue-Green and Canary deployments are advanced strategies used in continuous delivery to minimize downtime and reduce risk when deploying new versions of applications.
These strategies allow for smoother transitions between application versions, ensuring that new releases are thoroughly tested before becoming fully operational.

# Blue-Green Deployment
Blue-Green Deployment involves maintaining two separate environments: **Blue (current version) and Green (new version).** Here's how it works:

**Setup:** Both environments are identical. Initially, the Blue environment is live and serving all production traffic.

**Deployment:** The new version of the application is deployed to the Green environment, while the Blue environment continues to handle production traffic.

**Testing:** The new version in the Green environment is thoroughly tested.

**Switch Over:** Once testing is complete and the new version is deemed stable, traffic is switched from the Blue environment to the Green environment.

**Rollback:** If any issues arise with the Green environment, traffic can be quickly switched back to the Blue environment.


# Canary Deployment
Canary Deployment involves gradually rolling out the new version to a small subset of users before rolling it out to the entire user base. Here's how it works:

**Initial Deployment:** The new version is deployed to a small subset of servers (canary) while the rest of the servers continue to run the old version.

**Monitoring:** The performance and stability of the canary servers are closely monitored.

**Gradual Rollout:** If no issues are detected, the new version is gradually rolled out to more servers.

**Complete Rollout:** Eventually, the new version is deployed to all servers.

**Rollback:** If issues are detected at any stage, the deployment can be halted or rolled back to the previous version.
Implementing Blue-Green and Canary Deployments with Argo Rollouts

# Blue-Green Deployment Example with Argo Rollouts
Here's an example of how to define a Blue-Green deployment using Argo Rollouts:

```
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: blue-green-rollout
  namespace: default
spec:
  replicas: 3
  strategy:
    blueGreen:
      activeService: active-service
      previewService: preview-service
      autoPromotionEnabled: false
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-image:latest
        ports:
        - containerPort: 80

```

In this example:

- **activeService** points to the live (blue) environment.
- **previewService** points to the new (green) environment.
- **autoPromotionEnabled:** false means manual intervention is required to switch traffic from the blue to the green environment.

# Canary Deployment Example with Argo Rollouts
Here's an example of how to define a Canary deployment using Argo Rollouts:
```
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: canary-rollout
  namespace: default
spec:
  replicas: 3
  strategy:
    canary:
      steps:
      - setWeight: 20
      - pause: {duration: 600}
      - setWeight: 50
      - pause: {duration: 600}
      - setWeight: 100
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-image:latest
        ports:
        - containerPort: 80
```
 In this example:

- The deployment starts by sending 20% of traffic to the new version.
- After a pause (10 minutes in this case), it increases to 50%.
- Finally, it sends 100% of the traffic to the new version if no issues are detected.
Monitoring and Managing Deployments with Argo Rollouts
Use the Argo Rollouts CLI to monitor and manage the rollouts:

1. Get Rollout Status:
```
kubectl argo rollouts get rollout blue-green-rollout -n default
```
2. Promote Rollout (for manual promotion):
```
kubectl argo rollouts promote blue-green-rollout -n default

```
3. Rollback (if issues are detected):
```
kubectl argo rollouts rollback blue-green-rollout -n default

```

