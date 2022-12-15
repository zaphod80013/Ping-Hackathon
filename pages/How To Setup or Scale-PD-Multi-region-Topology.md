- Affected Releases
	- [[v1.16.0.0]]
	- [[v1.17.0.0]]
- When initially launching a Multi-region Deployment or Upgrading a Single Region Deploy to Multi-region or when scaling up/down a Multi-region [[PingDirectory]] deployment:
## Setup Steps:
- Edit the file found at cluster-state-repo/k8s-configs/base/ping-cloud/descriptor.json file to - add the example data below and edit to match your cde/environment in use.
- for example: 
  
  | 
  
  `{`
  
  `  ``"us-east-2"``: {`
  
  `    ``"hostname"``: ``"pingdirectory-cluster.stage-aquaman.us1.ping-preview.cloud"``,`
  
  `    ``"replicas"``: ``3`
  
  `  ``},`
  
  `  ``"eu-central-1"``: {`
  
  `    ``"hostname"``: ``"pingdirectory-cluster.stage-aquaman.eu1.ping-preview.cloud"``,`
  
  `    ``"replicas"``: ``3`
  
  `  ``}`
  
  `}`
  
   |
- Save changes and stage for commit.
- Edit cluster-state-repo/k8s-configs/<region>/ping-cloud/pingdirectory/env_vars files for both primary and secondary regions to update the LAST_UPDATE_REASON to update the PD pods with the new topology information ~~and ~~~~set INITIALIZE_REPLICATION_DATA to true~~ in PD env_vars (its default value is false).
- Save changes and stage for commit.
- Commit all the changes and push to remote repo.
## Scale-up Steps  :
- Edit cluster-state-repo/k8s-configs/base/ping-cloud/descriptor.json file to increase the number of replicas in each region.
- Save changes and stage for commit.
- Edit cluster-state-repo/k8s-configs/base/custom-patches.yaml file to add the following code block edit to the number of replicas you need to scale to set to match in step 1:
  
  **Scale patch**
  
  | 
  
  `apiVersion: apps/v1`
  
  `kind: StatefulSet`
  
  `metadata:`
  
  `  ``name: pingdirectory`
  
  `  ``namespace: ping-cloud`
  
  `spec:`
  
  `  ``replicas: ``4`
  
  `---`
  
   |
- Save change and stage for commit.
- Edit cluster-state-repo/k8s-configs/<region>/pingdirectory/env_vars files for both primary and secondary regions to update the LAST_UPDATE_REASON to mention scaling up the PD deployment.
- Edit cluster-state-repo/k8s-configs/<region>/pingdirectory/env_vars files for both primary and secondary regions to set INITIALIZE_REPLICATION_DATA to true in PD env_vars (its default value is false).
- Save changes and stage for commit.
- Commit all the changes and push to remote repo.
- Verify replication is working for all new PD pods
- ~~Remove INITIALIZE_REPLICATION_DATA or set back to false in cluster-state-repo/k8s-configs/<region>/ping-cloud/pingdirectory/env_vars files for both primary and secondary regions.~~
  
  **dsreplication status**
  
  | 
  
  `kubectl exec -c pingdirectory -n ping-cloud pingdirectory-``0` `-- dsreplication status`
  
   |
- Save changes and stage for commit.
- Commit the change and push into remote repo.
  
  Note: if attempting this without CodeCommit Repo Sync enabled - you will need to make and push the changes into both region's cluster-state-repo.