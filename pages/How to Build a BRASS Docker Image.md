- # How-To Build a BRASS Docker
-
- Sometimes we may want to build a Docker image for consumption in ping-cloud-docker (PCD), manually, in order to test it before the BRASS team can provide us with an official one.
## Step-by-step guide

Most of the instructions in this guide are based on this document: [https://devops.pingidentity.com/how-to/buildLocal/](https://devops.pingidentity.com/how-to/buildLocal/)

This guide is just to help supplement this document in the context of the Beluga/P1AS team.
- Clone the pingidentity-docker-builds-repo as instructed.
- Don't download the product - you probably were provided with a download link. Use that link to the zipfile of the built product artifact for step (1). This might look like: [https://SOMETHING.corp.pingidentity.com/view/Gitlab%20Builds/job/PingFederate_Releases/job/rel-SOME_VERSION/SOME_NUMBER/artifact/pf-server/HuronPeak/assembly/target/pingfederate-SOME_VERSION-SNAPSHOT.zip](https://bld-fed01.corp.pingidentity.com/view/Gitlab%20Builds/job/PingFederate_Releases/job/rel-11.0/91/artifact/pf-server/HuronPeak/assembly/target/pingfederate-11.0.2-SNAPSHOT.zip)
- Follow the steps
- Open the versions.json as instructed under "Building the Docker image". You can find the version most similar or recent to the one you want to build and copy that block. Then, change the version for that block to the version you want to build - this is the version you'll target with the `serial_build.sh`  script. Note differences like jvm and shim. You may need to confirm with the BRASS team the flavor of image you're building prior to actually building it. If this is a build for non-p14g then most likely you want an alpine-based image.
- Follow steps to build your image.
  
  NEVER push to pingcloud-apps ECR repos, this will make things confusing since these repos are for PCD images only. Only push to pingidentity/PRODUCT_NAME ***public*** ECR repos - these are the vanilla repositories based on BRASS images.
  
  ALWAYS check the pingidentity/PRODUCT_NAME repo you are pushing to for conflicting image tag versions - make sure your image tag is unique or else you will overwrite the pre-existing BRASS image used by others!
  
  Using the image
- Tag and push the locally-built image to the pingidentity/PRODUCT_NAME public ECR repo, NOT pingcloud-apps/PRODUCT_NAME:
- `docker tag pingidentity/pingfederate:LOCAL_IMAGE_TAG public.ecr.aws/r2h3l6e4/pingidentity/pingfederate:TAG_YOU_WANT_PCD_TO_USE`
- `docker push public.ecr.aws/r2h3l6e4/pingidentity/pingfederate:TAG_YOU_WANT_PCD_TO_USE`
- Modify ping-cloud-docker (PCD) to point to this image. Follow the usual tagging process from here, making modifications where necessary to test the image. For example, using the tagging convention as of April 2022, you need to add a tag to PCD to get a public image into ECR under pingcloud-apps, so that a CDE can use it.
-