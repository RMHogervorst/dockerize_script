# README


This is the same as a different app but I'm trying out a different step now.

Does the script work?

- Add your secrets to .Renviron
- Restart the R session
- Run the script `source("run_job.R")`

Try out the docker image by building the container 
"docker build -t <name> . "

`docker build -t dockerized_script .`

Run the docker container, supplying the [.Renviron secrets](https://notes.rmhogervorst.nl/post/2020/09/23/passing-cmd-line-arguments-to-your-rocker-container/)

"docker run --env-file .Renviron <name>"

`docker run --env-file .Renviron dockerized_script`


### details of setting up 2 remotes for this repo

`git remote add gitlab git@gitlab.com:rmhogervorst/dockerized_script.git`
`git remote add github git@github.com:RMHogervorst/dockerize_script.git`

Commit files

`git push -u gitlab main`
`git push -u github main`


# Use the docker registry for gitlab
(their docs are quite good, so follow those if )

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
to specify what script to run. IF I run the container it runs the script)
