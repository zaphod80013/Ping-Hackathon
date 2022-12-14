## What Happened:

The file org.sourceid.config.CoreConfig.xml on a restarting pingfederate-admin instance got replaced with a pristine copy from the the pingfederate zip file.
## Root Cause

Not Established.
## Recommendations

Additional Logging be added to track file movement (copies, replacement, etc) to provide better diagnostic information in the future. [PDO-1188](https://jira.pingidentity.com/browse/PDO-1188) (only addresses 10-configuration-overrides)

The most likely explanation is a failure in 10-configuration-overrides, somebody else should look at the code incase there is a failure mode I'm missing.
## Summary

Using the PingFederate admin log I attempted to establish where the file got replaced with the pristine copy. Since the temporary PF instance in hook 10-configuration-overrides started and terminated correctly I believe the file was in the correct state on entry to the hook, other evidence supports this. There are several hooks that execute after this one and before the actual PF instance starts. The actual instance failed to start.
## Analysis

I can eliminate  `"83-download-archive-data-s3.sh"` hook as I downloaded the archive listed in the log and visually verified the file content. We know the internal (10-configuration-overrides) PF instance ran successfully because I see

**11:04:11 Admin API request status: 0; HTTP status: 200**
**11:04:11 Admin API endpoint version read**

in the logs and I see evidence of a successful shutdown

**11:04:11 Waiting for PingFederate to shutdown**
**11:04:12 PingFederate shutdown...**

In 1.4 the hook calls a GSA routine to do the copy and it doesn’t log what it copies (I’ll open a 1.5 defect to add extra logging) so I can’t eliminate config overrides as I have no definitive evidence one way or the other, that said the hook uses the ${STAGING_DIR}  variable which had the correct value:

**11:02:29 STAGING_DIR : /opt/staging**

Since in every reference to the variable has `"/data-overrides"` appended even if the variable had been updated to `"/opt/server/server/default"` which is the path to the pristine unpacked server it would have failed with a directory not found due to the `"/data-overrides"` suffix, there is no evidence of that. If a pristine copy of the file `"data-overrides/config-store/org.sourceid.config.CoreConfig.xml"` existed in the code-commit repo it would have been copied without any entry in the log but there is no such file in the repo as viewed in the AWS console. This file can’t be updated via the config-store API (I tried this manually on my cluster and got ‘access denied’, additionally there is evidence that the config-store directory  in the override mechanism was empty both in the code-commit repo and the log:

**11:02:11 ls: *.json: No such file or directory**

Taken collectively I believe this eliminates `"10-configuration-overrides"` but I can’t actually prove it.

The fact the the internal PF ran successfully effectively eliminates the `"10-download-artifact.sh"` hook, in principle if plug-in had a pristine copy of the file it would have overwritten the good copy, and the internal PF would have either detected this and created the needed passwords (this scenario is not compatible with the evidence of a pristine copy in Jason Wilcox's original post to pdo-dev) or failed to start, since it ran normally I believe we can say the file was in the correct state on entry to  `"10-configuration-overrides"` I then proceeded to process the log and source code (taken from a running 1.4 instance on my own EKS cluster)I can eliminate “`21-update-server-profile.sh"` if it executed there would be a corresponding log message that's not present.I can eliminate `"22-upgrade-server.sh"` The messages are consistent with no upgrade occurringI can eliminate `"50-before-post-start.sh"` its an empty placeholderI can eliminate "80-post-start.sh" as it enters a wait for API loop that never exited before the PF instance failed.As far as I can tell there is nothing in the PF startup stack to account for the observed behavior, as far as I can tell this is not an issue caused by the Beluga code. That said we should probably have someone else work through the log and  `"10-configuration-overrides"` Incase there is a failure mode I’m not seeing.