| Dimension            | On-Premise Docker (Wks 3–5)             | Cloud Run (Week 8)                                                        |
| :------------------- | :-------------------------------------- | :------------------------------------------------------------------------ |
| Infrastructure setup | 3 VMs created, Docker installed on each | Docker image in Artifact Registry, Cloud Run deploy it                    |
| Deployment command   | SSH → docker build → docker run         | Authenticate to GCP via OIDC → Build and push image → Deploy to Cloud Run |
| TLS / HTTPS          | Not configured                          | GCP handle this                                                           |
| Scaling approach     | Manual — redeploy or add VMs            | GitHub Cloud Run Action Automation                                        |
| Port management      | Ports 5000/5001/5002 per environment    | GCP handle this                                                           |
| Cost when idle       | VM running 24/7 regardless of traffic   | Cloud Run Scale Automatically                                             |
| Rollback             | Re-deploy previous image manually       | Cloud Run use docker image in artifact registry to re-deploy              |
| Secrets management   | GitHub Secrets → env vars in workflow   | Store in Google Secret Manager                                            |

## Reflection Questions

- Q1: Which approach required more manual steps from push to live URL? List the specific steps that were eliminated by Cloud Run.

- Q2: A security audit asks how you know which version of the code is currently running in production. How would you answer for on-premise Docker vs. Cloud Run with commit SHA tagging?

- Q3: Your on-premise VMs run 24/7 even when no students are using the app. Cloud Run scales to zero. What is the security advantage of scale-to-zero beyond cost savings?

- Q4: The OIDC workflow replaced the SSH key secrets from Weeks 3–5. What attack surface was eliminated?
