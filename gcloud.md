Tips and Tricks for anybody that plans to use now or in the future the google cloud environment.	
	
GCloud API with all the functions to handle images inside the google cloud.
	
** https://cloud.google.com/sdk/gcloud/reference/container/builds/submit **

// Commands to check which images are available in the project:
% gcloud container images list

// To check which images are available for a image execute the commando:
% gcloud container images list-tags [HOSTNAME]/[PROJECT-ID]/[IMAGE][:TAG]
% gcloud container images list-tags gcr.io/my-project-name/busybox


// To create a new local image (inside the cloud) you should use the build.yaml files that usually
// sit inside the dockerfiles dir.
// Just run the command from the head of the file:

gcloud container builds submit . --config build.yaml


Command to read the sha256 from an imagem
gcloud container images describe gcr.io/my-project-name/busybox | grep " digest:" | cut -d ':' -f 3
