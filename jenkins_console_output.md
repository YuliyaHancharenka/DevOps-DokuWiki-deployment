```bash
SuccessConsole Output
Started by user admin
Running in Durability level: MAX_SURVIVABILITY
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/Project_wiki/Jenkinsfile
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build and run docker for Dokuwiki)
[Pipeline] stage
[Pipeline] { (Clone Git)
[Pipeline] git
using credential 66f6eb3b-3cd3-492c-997b-6579c2ab1049
 > git rev-parse --is-inside-work-tree # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url git@github.com:yuliyahancharenka/project # timeout=10
Fetching upstream changes from git@github.com:yuliyahancharenka/project
 > git --version # timeout=10
using GIT_SSH to set credentials 
 > git fetch --tags --progress git@github.com:yuliyahancharenka/project +refs/heads/*:refs/remotes/origin/*
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git rev-parse refs/remotes/origin/origin/master^{commit} # timeout=10
Checking out Revision bc98c5dc019e0fab0ac9f7912eb6feb0a2cf84a7 (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f bc98c5dc019e0fab0ac9f7912eb6feb0a2cf84a7
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master bc98c5dc019e0fab0ac9f7912eb6feb0a2cf84a7
Commit message: "Init"
 > git rev-list --no-walk bc98c5dc019e0fab0ac9f7912eb6feb0a2cf84a7 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build image)
[Pipeline] script
[Pipeline] {
[Pipeline] sh
+ docker build -t dockeryuliya/sa_project:14 .
Sending build context to Docker daemon  210.4kB

Step 1/7 : FROM alpine:latest
latest: Pulling from library/alpine
Digest: sha256:72c42ed48c3a2db31b7dafe17d275b634664a708d901ec9fd57b1529280f01fb
Status: Downloaded newer image for alpine:latest
 ---> 961769676411
Step 2/7 : RUN apk update &&   apk add wget nginx php7 php7-common php7-mbstring php7-session php7-json php7-fpm &&   wget -q http://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz -P /tmp &&   mkdir /var/www/dokuwiki &&   tar -xzf /tmp/dokuwiki-stable.tgz -C /var/www/dokuwiki --strip-components 1 &&   chown -R nginx:nginx /var/www/dokuwiki &&   chmod -R 777 /var/www/dokuwiki
 ---> Using cache
 ---> e011a56b488c
Step 3/7 : COPY nginx/sites/default /etc/nginx/conf.d/default.conf
 ---> 39d5660dc5c3
Step 4/7 : RUN mkdir /run/nginx/ && touch /run/nginx/nginx.pid &&   touch /var/log/nginx/access.log && touch /var/log/nginx/error.log
 ---> Running in d0be64c83d4a
Removing intermediate container d0be64c83d4a
 ---> 7f57edad532d
Step 5/7 : COPY entrypoint.sh /entrypoint.sh
 ---> ecda420c9da7
Step 6/7 : EXPOSE 80
 ---> Running in 27e87ecc91e2
Removing intermediate container 27e87ecc91e2
 ---> 309941612392
Step 7/7 : CMD ["sh", "/entrypoint.sh"]
 ---> Running in c63aa857aed2
Removing intermediate container c63aa857aed2
 ---> 68787970c1fe
Successfully built 68787970c1fe
Successfully tagged dockeryuliya/sa_project:14
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Push image)
[Pipeline] script
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] withDockerRegistry
$ docker login -u dockeryuliya -p ******** https://index.docker.io/v1/
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /var/lib/jenkins/workspace/Project_wiki/Jenkinsfile@tmp/f97a351a-7fc3-40bd-83ad-918d076ec11e/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[Pipeline] {
[Pipeline] sh
+ docker tag dockeryuliya/sa_project:14 dockeryuliya/sa_project:14
[Pipeline] sh
+ docker push dockeryuliya/sa_project:14
The push refers to repository [docker.io/dockeryuliya/sa_project]
ee1b5084ee9e: Preparing
4b89314586ed: Preparing
af47b675fb3a: Preparing
e4b89cb5dbf1: Preparing
03901b4a2ea8: Preparing
e4b89cb5dbf1: Layer already exists
03901b4a2ea8: Layer already exists
ee1b5084ee9e: Layer already exists
4b89314586ed: Pushed
af47b675fb3a: Pushed
14: digest: sha256:b63fc4d066b0cbe563e7fb8c7ece5455d3141948ff9324435eb5b7e441398ebd size: 1361
[Pipeline] }
[Pipeline] // withDockerRegistry
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Run docker image)
[Pipeline] sh
+ echo dockeryuliya/sa_project:14
dockeryuliya/sa_project:14
[Pipeline] sh
+ docker run -d -p 4000:80 --name dokuwiki dockeryuliya/sa_project:14
27d14fdc6c849c5ea7e7670aafa6a05b7020f31c1d1172fdda213621654645f2
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Remove Dokuwiki)
Stage "Remove Dokuwiki" skipped due to when conditional
[Pipeline] stage
[Pipeline] { (Stop and delete docker container)
Stage "Remove Dokuwiki" skipped due to when conditional
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Remove docker image)
Stage "Remove Dokuwiki" skipped due to when conditional
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions) (hide)
[Pipeline] slackSend
Slack Send Pipeline step running, values are - baseUrl: <empty>, teamDomain: sa-itacademy-by, channel: jenkins_ci, color: #00FF00, botUser: false, tokenCredentialId: 1ebf8e4a-7c38-4995-883e-3edbef57a792, notifyCommitters: false, iconEmoji: <empty>, username: <empty>
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS



SuccessConsole Output
Started by user admin
Running in Durability level: MAX_SURVIVABILITY
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/Project_wiki/Jenkinsfile
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build and run docker for Dokuwiki)
Stage "Build and run docker for Dokuwiki" skipped due to when conditional
[Pipeline] stage
[Pipeline] { (Clone Git)
Stage "Build and run docker for Dokuwiki" skipped due to when conditional
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build image)
Stage "Build and run docker for Dokuwiki" skipped due to when conditional
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Push image)
Stage "Build and run docker for Dokuwiki" skipped due to when conditional
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Run docker image)
Stage "Build and run docker for Dokuwiki" skipped due to when conditional
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Remove Dokuwiki)
[Pipeline] stage
[Pipeline] { (Stop and delete docker container)
[Pipeline] sh
+ docker container stop dokuwiki
dokuwiki
[Pipeline] sh
+ docker container prune -f
Deleted Containers:
27d14fdc6c849c5ea7e7670aafa6a05b7020f31c1d1172fdda213621654645f2

Total reclaimed space: 185.7kB
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Remove docker image)
[Pipeline] sh
+ docker system prune -af
Deleted Images:
untagged: dockeryuliya/sa_project:14
untagged: dockeryuliya/sa_project@sha256:b63fc4d066b0cbe563e7fb8c7ece5455d3141948ff9324435eb5b7e441398ebd
deleted: sha256:68787970c1fe76ac9c7c25baf2612bb0f2b4da07583bb6bc4ead669a3911591a
deleted: sha256:30994161239216697e1efed59ad0f96aa7a9a51885f1278384333575a58f795a
deleted: sha256:ecda420c9da7f8ec438a85cb32e39294a2faec9c219a32f096dc0c0eaf24ab26
deleted: sha256:9615b7462dab24fad0fdbe86b76ee97d518d1f28ec6adadd57f11bc3e5897aae
deleted: sha256:7f57edad532df7f86dd47429b5801482097e86ac331c1f527d6f4c62cdfd1282
deleted: sha256:3a46f3ce6c9c87aea97bc5847c7c8a09f6c696502fcbe70b7682225e5a808d47
deleted: sha256:39d5660dc5c3ad80f8d145a6534290553a8a3940fd96d3f2add0df5f32fdb513
deleted: sha256:eda5081ef45c5402ef44992ed8ac01b02432acdb7de38b25b4ca13db698ef0e5
untagged: alpine:latest
untagged: alpine@sha256:72c42ed48c3a2db31b7dafe17d275b634664a708d901ec9fd57b1529280f01fb

Total reclaimed space: 1.027kB
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions)
[Pipeline] slackSend
Slack Send Pipeline step running, values are - baseUrl: <empty>, teamDomain: sa-itacademy-by, channel: jenkins_ci, color: #00FF00, botUser: false, tokenCredentialId: 1ebf8e4a-7c38-4995-883e-3edbef57a792, notifyCommitters: false, iconEmoji: <empty>, username: <empty>
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

