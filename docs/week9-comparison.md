# Week 9 Security Audit — cis410-deploy-sa

**Project:** responsive-lens-495317-a6

**Date:** June 01, 2026

**Auditor:** Pawan Mehra

---

## 1. IAM Audit Results

### Before — Week 8 Configuration (over-permissioned)

| Role | Scope | Problem |

| roles/run.admin | Project | Overly broad — grants ability to delete services and modify IAM, not just deploy |

| roles/storage.admin | Project | Overly broad — grants access to ALL GCS buckets in the project |

| roles/artifactregistry.writer | Project | Acceptable — scoped to push images only |

| roles/viewer | Project | Acceptable — read-only project metadata |

| roles/iam.serviceAccountUser | Compute SA | Required — needed to act as Compute Engine default SA |

### After — Week 9 Least-Privilege Fix

| Role | Scope | Why Sufficient |

| roles/run.developer | Project | Deploy only — cannot delete services or modify IAM |

| roles/storage.admin | tfstate bucket only | Scoped to one bucket — not all storage |

| roles/artifactregistry.writer | Project | Unchanged — push images only |

| roles/viewer | Project | Unchanged — read project metadata |

| roles/iam.serviceAccountUser | Compute SA | Unchanged — required for Cloud Run deployment |

---

## 2. Secret Manager Migration

- **Secret created:** `flask-app-secret`

- **Replication:** automatic

- **Access granted to:** `cis410-deploy-sa` — roles/secretmanager.secretAccessor on this secret only

- **Access granted to:** `responsive-lens-495317-a6-compute@developer.gserviceaccount.com` — roles/secretmanager.secretAccessor on this secret only (required for Cloud Run runtime access)

- **Cloud Run update:** flask-app-secret environment variable mounted from Secret Manager at runtime

---

## 3. Monitoring Configuration

- **Log-based alert:** `cis410-flask-app-alert` — fires on severity>=WARNING for cis410-flask-app

- **Notification channel:** <!-- your student email -->

- **Billing budget:** `cis410-monthly-budget` — $20 limit, alerts at 50% / 90% / 100%

---

## 4. Reflection

**Q1: Why is roles/run.admin inappropriate for a CI/CD pipeline service account?**

Because run.admin role has ability to change IAM policy, delete the services, and many more admin level access to modify sensitive data on GCP.

---

**Q2: What is the security difference between storing a secret in GitHub Secrets vs. Google Secret Manager?**

At the run time, GCP secret manager logs the entry who and when modify the secret but GitHub does not generate logs about this information.

---

**Q3: A coworker says "I will clean up IAM permissions after the project launches. For now I need everything to work fast." What is the risk of this approach?**

Cleaning IAM permission before the launch allows you to safely discover and handle errors in a controlled in pre-production but after the launch, there is a risk of blocking production workflows.
