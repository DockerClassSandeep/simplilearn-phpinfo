# simplilearn-phpinfo
GITHUB_USERNAME=dockerclasssandeep
GITHUB_PROJECT=simplilearn-phpinfo
GITHUB_BRANCH=2021-08
GITHUB_RELEASE=single-line
NODEPORT=80

cd ${HOME}
git clone https://github.com/${GITHUB_USERNAME}/${GITHUB_PROJECT}
cd ${GITHUB_PROJECT}
git checkout ${GITHUB_BRANCH}

sudo docker image build --file Dockerfile_SingleLine --tag ${GITHUB_USERNAME}/${GITHUB_PROJECT}:${GITHUB_RELEASE} ./
docker container run --cpus 0.050 --detach --entrypoint /usr/bin/php --memory 10M --name ${GITHUB_PROJECT}_${GITHUB_RELEASE} --publish 80:8080 --read-only --rm --user nobody --volume ${PWD}/src:/src:ro --workdir /src ${GITHUB_USERNAME}/${GITHUB_PROJECT}:single -f index.php -S 0.0.0.0:8080

sudo docker container logs ${GITHUB_PROJECT}_${GITHUB_RELEASE} 
sudo docker container top ${GITHUB_PROJECT}_${GITHUB_RELEASE} 
sudo docker container stats --no-stream ${GITHUB_PROJECT}_${GITHUB_RELEASE}  


GITHUB_RELEASE=no-volume
NODEPORT=81

sudo docker image build --file Dockerfile-${GITHUB_RELEASE} --tag ${GITHUB_USERNAME}/${GITHUB_PROJECT}:${GITHUB_RELEASE} ./
sudo docker container run --cpus 0.050 --detach --entrypoint /usr/bin/php --memory 10M --name ${GITHUB_PROJECT}_${GITHUB_RELEASE} --publish ${NODEPORT}:8080 --read-only --rm --user nobody --workdir /src/ ${GITHUB_USERNAME}/${GITHUB_PROJECT}:${GITHUB_RELEASE} -f index.php -S 0.0.0.0:8080

sudo docker container logs ${GITHUB_PROJECT}_${GITHUB_RELEASE} 
sudo docker container top ${GITHUB_PROJECT}_${GITHUB_RELEASE} 
sudo docker container stats --no-stream ${GITHUB_PROJECT}_${GITHUB_RELEASE}

GITHUB_RELEASE=metadata
NODEPORT=82

sudo docker image build --file Dockerfile-${GITHUB_RELEASE} --tag ${GITHUB_USERNAME}/${GITHUB_PROJECT}:${GITHUB_RELEASE} ./
sudo docker container run --cpus 0.050 --detach --memory 10M --name ${GITHUB_PROJECT}_${GITHUB_RELEASE} --publish ${NODEPORT}:8080 --read-only --rm --volume ${PWD}/src/:/src/:ro ${GITHUB_USERNAME}/${GITHUB_PROJECT}:${GITHUB_RELEASE}

sudo docker container logs ${GITHUB_PROJECT}_${GITHUB_RELEASE} 
sudo docker container top ${GITHUB_PROJECT}_${GITHUB_RELEASE} 
sudo docker container stats --no-stream ${GITHUB_PROJECT}_${GITHUB_RELEASE}

GITHUB_RELEASE=no-volume-metadata
NODEPORT=83

sudo docker image build --file Dockerfile-${GITHUB_RELEASE} --tag ${GITHUB_USERNAME}/${GITHUB_PROJECT}:${GITHUB_RELEASE} ./
sudo docker container run --cpus 0.050 --detach --memory 10M --name ${GITHUB_PROJECT}_${GITHUB_RELEASE} --publish ${NODEPORT}:8080 --read-only --rm ${GITHUB_USERNAME}/${GITHUB_PROJECT}:${GITHUB_RELEASE}

sudo docker container logs ${GITHUB_PROJECT}_${GITHUB_RELEASE} 
sudo docker container top ${GITHUB_PROJECT}_${GITHUB_RELEASE} 
sudo docker container stats --no-stream ${GITHUB_PROJECT}_${GITHUB_RELEASE}
