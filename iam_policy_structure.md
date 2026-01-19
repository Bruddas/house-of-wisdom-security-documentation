**GCP IAM Policy Structure - Principle of Least Privilege (PoLP)**

This document outlines the custom Identity and Access Management (IAM) roles designed for the House of Wisdom's Google Cloud Platform (GCP) resources (Cloud Functions, Firestore, and Cloud Storage). The structure adheres strictly to the Principle of Least Privilege (PoLP).

Goal: To prevent lateral movement, unauthorized modification, or deletion of production resources in the event of a compromised developer or service account.

**Custom Role Definitions (Template Examples)**

1. custom.app.writer (Backend API Service Account)

- Scope: Firestore Database.

- Permissions: datastore.entities.create, datastore.entities.update.

- Justification: Allows the backend API to write and update user-submitted content only; explicitly blocks the ability to delete or read all audit logs.

2. custom.storage.readonly (Frontend Service Account)

- Scope: Cloud Storage Bucket.

- Permissions: storage.objects.get, storage.objects.list.

- Justification: Allows the frontend to read necessary static assets, while completely blocking any file upload or deletion capability.

3. custom.deployment.viewer (Junior Developer/Monitor)

- Scope: Cloud Functions, Cloud Logging.

- Permissions: cloudfunctions.functions.get, logging.logWriter.

- Justification: Grants permission to view function health and write application logs, but blocks any modification or redeployment of critical infrastructure.

**Key Policy Implementation Details**

- No Primitive Roles: No developer or service account was granted the "Owner," "Editor," or "Viewer" primitive roles, minimizing high-level security risk.

- Principle of Separation: All production infrastructure interactions are handled by dedicated Service Accounts, not human users.

- Audit Logging: All changes to IAM policies and resource deployments are tracked via Cloud Audit Logs for continuous monitoring.
