| Dimension            | On-Premise Docker (Wks 3–5)             | Cloud Run (Week 8)                                                        |
| :------------------- | :-------------------------------------- | :------------------------------------------------------------------------ |
| Infrastructure setup | 3 VMs created, Docker installed on each | Enable Could Run API, Docker image in Artifact Registry, Cloud Run deploy |
| Deployment command   | SSH → docker build → docker run         | Authenticate to GCP via OIDC → Build and push image → Deploy to Cloud Run |
| TLS / HTTPS          | Not configured                          | GCP handle this                                                           |
| Scaling approach     | Manual — redeploy or add VMs            | GitHub "Deploy Cloud Run" Action Automated it                             |
| Port management      | Ports 5000/5001/5002 per environment    | Cloud Run handle this                                                     |
| Cost when idle       | VM running 24/7 regardless of traffic   | Cloud Run Scale Automatically                                             |
| Rollback             | Re-deploy previous image manually       | Cloud Run use docker image from artifact registry and re-deploy it        |
| Secrets management   | GitHub Secrets → env vars in workflow   | Store in Google Secret Manager                                            |

## Reflection Questions

- Q1: Which approach required more manual steps from push to live URL? List the specific steps that were eliminated by Cloud Run.
  VM approach has more manual steps than the Cloud Run. Steps eliminated by cloud run
  (a) No 3 separate VMs for dev, stage, prod (b) dynamic IP address for the HOST (c) SSH deplpyment to different env
- Q2: A security audit asks how you know which version of the code is currently running in production. How would you answer for on-premise Docker vs. Cloud Run with commit SHA tagging?
  (a) Cloud Run - Revisions Tag can be use to identify the version of the code. We can print it or store in the logs.
  (b)on-premise Docker - Docker image tag can be use to identify the version of the code.
- Q3: Your on-premise VMs run 24/7 even when no students are using the app. Cloud Run scales to zero. What is the security advantage of scale-to-zero beyond cost savings?
  (a) it minimize the attack surface when scaleto-zero (b) limit unauthorized access windows (c) Reduced Vulnerability when scale-to-zero
- Q4: The OIDC workflow replaced the SSH key secrets from Weeks 3–5. What attack surface was eliminated?
  SSH key secrets never expire and OIDC create a new token every time request come from the provider(GitHub), it means, this short term token creation approach eliminated attack surface for the attackers.
  - Ans - VM approach has more manual steps than the Cloud Run. Steps eliminated by cloud run (a) No 3 separate VMs for dev, stage, prod (b) dynamic IP address for the HOST (c) SSH deplpyment to different env
- Q2: A security audit asks how you know which version of the code is currently running in production. How would you answer for on-premise Docker vs. Cloud Run with commit SHA tagging?
  - Ans - (a) Cloud Run - Revisions Tag can be use to identify the version of the code. We can print it or store in the logs. (b)on-premise Docker - Docker image tag can be use to identify the version of the code.
- Q3: Your on-premise VMs run 24/7 even when no students are using the app. Cloud Run scales to zero. What is the security advantage of scale-to-zero beyond cost savings?
  - Ans - (a) it minimize the attack surface when scaleto-zero (b) limit unauthorized access windows (c) Reduced Vulnerability when scale-to-zero
- Q4: The OIDC workflow replaced the SSH key secrets from Weeks 3–5. What attack surface was eliminated?
  - Ans - SSH key secrets never expire and OIDC create a new token every time request come from the provider(GitHub), it means, this short term token creation approach eliminated attack surface for the attackers.
