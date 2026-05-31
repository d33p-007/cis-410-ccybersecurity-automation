| Dimension | On-Premise Docker (Wks 3–5) | Cloud Run (Week 8) |
| :--- | :--- | :--- |
| Infrastructure setup | 3 VMs created, Docker installed on each | Cell 1,3 |
| Deployment command | SSH → docker build → docker run | Cell 2,3 |
| TLS / HTTPS | Not configured |  |
| Scaling approach | Manual — redeploy or add VMs |  |
| Port management | Ports 5000/5001/5002 per environment |  |
| Cost when idle | VM running 24/7 regardless of traffic |  |
| Rollback | Re-deploy previous image manually |  |
| Secrets management | GitHub Secrets → env vars in workflow |  |
