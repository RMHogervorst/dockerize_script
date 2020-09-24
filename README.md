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

## push image to registry from your computer
Build a docker container (will default to be labelled 'latest')

`docker build -t registry.gitlab.com/rmhogervorst/dockerized_script .`

and push it to the registry 

`docker push registry.gitlab.com/rmhogervorst/dockerized_script`

Or add build steps to the CI platform
