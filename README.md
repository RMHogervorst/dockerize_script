# README


This is the same application as 'invertedushape' a but I'm trying out a different step now. I'm packing up the script and dependencies into a docker
container and build and run that container on the different hosts.


# General checks
**Does the script work?**

- Add your secrets to .Renviron
- Restart the R session
- Run the script `source("run_job.R")`

**Does the docker container work?**
Try out the docker image by building the container 
"docker build -t <name> . "

`docker build -t dockerized_script .`

Run the docker container, supplying the [.Renviron secrets](https://notes.rmhogervorst.nl/post/2020/09/23/passing-cmd-line-arguments-to-your-rocker-container/)

"docker run --env-file .Renviron <name>"

`docker run --env-file .Renviron dockerized_script`


# Use the docker registry for gitlab
[(their docs are quite good, so follow those before mine)](https://docs.gitlab.com/ee/ci/yaml/README.html)

## Push an image to the gitlab registry from your computer
Build a docker container (will default to be labeled 'latest')

`docker build -t registry.gitlab.com/rmhogervorst/dockerized_script .`

and push it to the registry 

`docker push registry.gitlab.com/rmhogervorst/dockerized_script`

## Or add build steps to the CI platform
See [relevant docs from gitlab](https://gitlab.com/help/user/packages/container_registry/index#container-registry-examples-with-gitlab-cicd) about how to set up a container registry
and run it.

I modified their example from the link above to only do two things

- build the container and push it to registry
- pull the container and run it

(because my container ends with a command to run the script I don't even need
to specify what script to run. If I run the container it runs the script)

What I did have to do was explicitly tell gitlab to pass the environmental variables with the -e option in docker run. 

`docker run -e apikey=$apikey -e apisecretkey=$apisecretkey -e access_token=$access_token -e access_token_secret=$access_token_secret $CONTAINER_TEST_IMAGE`


# Google cloud

Have permissions to push and pull from the registry
gcloud auth configure-docker

hostname: (I'm in europe) eu.gcr.io
imagename: dockerize_script
[PROJECT-ID] (I think secret so I'm not giving it) 
combine into
[HOSTNAME]/[PROJECT-ID]/[IMAGE]

docker tag [SOURCE_IMAGE] [HOSTNAME]/[PROJECT-ID]/[IMAGE]
(or use docker build)

`docker build -t  eu.gcr.io/{PROJECT-ID}/dockerize_script:latest .`

docker push [HOSTNAME]/[PROJECT-ID]/[IMAGE]

`docker push eu.gcr.io/{PROJECT-ID}/dockerize_script:latest`

(I think we can now create a cloud function based on the container and 
a cloud scheduler to fire them off) https://www.cloudsavvyit.com/4975/how-to-run-gcp-cloud-functions-periodically-with-cloud-scheduler/

on second hand, maybe cloud build is the better option


### details of setting up 2 remotes for this repo
*This repo lives on both github and gitlab*

`git remote add gitlab git@gitlab.com:rmhogervorst/dockerized_script.git`
`git remote add github git@github.com:RMHogervorst/dockerize_script.git`

Commit files

`git push -u gitlab main`
`git push -u github main`

