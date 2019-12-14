# Problems

## 1. Running Voting App in Kubernetes using Helm

Clone the repo below and navigate to folder 1.

```bash
git clone https://github.com/gokhansengun/troubleshooting-k8s.git
```

You will find a `docker-compose.yml` file in folder root. Investigate this file and README.md file of the repo to know the app better.

Optional: Build and run the app using `docker-compose up` and navigate to `http://<ip>:5000` and `http://<ip>:5001` to access the app's web apps.

### End Result

- You are expected to finish it within 60 minutes
- Run the app in Kubernetes
- Use Helm the deploy services in Kubernetes
- Use ingress to access the web apps.
  - `http://result.cluster-<cluster_id>.do.gokhansengun.com` should point the result web app
  - `http://vote.cluster-<cluster_id>.do.gokhansengun.com` should point the voting web app

### Hints

- Create a Helm chart first (using `helm create xxx`) to deploy apps
- Use existing Redis and Postgres Helm charts to deploy them, deploy them as a single instance (not highly available)
- Do not try to persist Redis and Postgres' data. Let it be lost if restarted. It is OK for now.

## 2. Fix Problem

Trying to reach page `demo.cluster-<your_cluster_id>.do.gokhansengun.com` produces a 503 error. Please find out and fix the problem.

### Hints

- There might be a missing configuration
- `gsengun/aspnetcoredemo` Docker image accepts traffic on port 80

## 3. Fix Problem

Trying to reach page `demo.cluster-<your_cluster_id>.do.gokhansengun.com` produces a 503 error. Please find out and fix the problem.

### Hints

- There is only one error
- `gsengun/aspnetcoredemo` Docker image accepts traffic on port 80

## 4. Fix Problems

Trying to reach page `petclinic.cluster-<your_cluster_id>.do.gokhansengun.com` produces errors. Please find out and fix the problems.

### Hints

- There are more than one error
- `gsengun/petclinic` Docker image accepts traffic on port 8080
