# Kubernetes Deployment Task: Execution & Verification

This document outlines the steps taken to deploy, troubleshoot, and verify the Kubernetes deployment task, including sidecar and init container testing.

## 1. Initial Deployment
First, the required deployment commands were executed. Below is the screenshot of the initial successful deployment:

<img width="867" height="347" alt="Initial Deployment" src="https://github.com/user-attachments/assets/2fc1652c-ebf9-427f-9db0-5a7be552427c" />

## 2. Port-Forwarding & Troubleshooting
When attempting to port-forward using `8080:80`, I encountered an `address already in use` error. This occurred because port `8080` on my local machine was already allocated to Jenkins. 

To resolve this, I changed the local port and successfully port-forwarded using `8081:80`:

<img width="896" height="670" alt="Port-forwarding Success" src="https://github.com/user-attachments/assets/34be7815-3397-4ad0-91a8-9a099586b436" />

## 3. Init Container Failure Simulation
To test the pod's resilience and behavior, I intentionally forced the init container to fail (e.g., by applying an `exit` command). The resulting pod states are captured below:

<img width="905" height="444" alt="Init Container Failure 1" src="https://github.com/user-attachments/assets/0dbab650-1f10-4c65-972c-349a6a70fb8a" />

<img width="1298" height="495" alt="Init Container Failure 2" src="https://github.com/user-attachments/assets/46e77173-8c87-42e0-bfb7-ace815b82ddb" />

### Describe and Rollout Results
Here are the results of inspecting the failure using the `kubectl describe` and `kubectl rollout` commands:

<img width="1351" height="327" alt="Describe and Rollout Output" src="https://github.com/user-attachments/assets/9b99c9b5-efd2-4861-be30-a806c97cb4a8" />

---

## 4. Verification Commands Results
The following screenshots display the outputs of the listed verification commands to ensure the cluster and resources are functioning as expected:

**Commands 1, 2, & 3:**
<img width="1051" height="323" alt="Verification 1-3" src="https://github.com/user-attachments/assets/5229b6c5-0c76-40d6-b705-f2cd48ecaa41" />

**Command 4:**
<img width="1647" height="95" alt="Verification 4" src="https://github.com/user-attachments/assets/046e8933-f0f2-44bb-9b29-307bf6f63111" />

**Command 5:**
<img width="1141" height="75" alt="Verification 5" src="https://github.com/user-attachments/assets/540bc708-881a-47f5-b7b6-87a8488a8912" />

**Command 6:**
<img width="1621" height="471" alt="Verification 6" src="https://github.com/user-attachments/assets/2adbbf8b-e9a7-48b4-97c5-b0ecc4fd7515" />

**Command 7:**
<img width="1485" height="115" alt="Verification 7" src="https://github.com/user-attachments/assets/a3267162-1bf1-4f7c-9de1-ac4b23a41570" />

**Command 8:**
<img width="1417" height="55" alt="Verification 8" src="https://github.com/user-attachments/assets/b69e76c2-5cdd-4eff-9596-b7d23319b915" />

**Command 9:**
<img width="1351" height="153" alt="Verification 9" src="https://github.com/user-attachments/assets/12be282b-5854-4a3f-aa3b-f240d54ca1a8" />

**Command 10:**
<img width="1648" height="360" alt="Verification 10" src="https://github.com/user-attachments/assets/f6e6316e-52b9-4ec5-8d4d-4d7bc872bcf8" />

**Command 11:**
<img width="1001" height="88" alt="Verification 11" src="https://github.com/user-attachments/assets/3538c88c-de7a-43b0-8bb2-110e087dc375" />
