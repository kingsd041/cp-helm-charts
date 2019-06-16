VERSION := 3.0.0
COMPONENTS := base kafka
REPOSITORY := kingsd
COMMIT_ID := $(shell git rev-parse --short HEAD)

debian/base/include/etc/confluent/docker/docker-utils.jar:
	mkdir -p debian/base/include/etc/confluent/docker
	cd java \
	&& mvn clean compile package assembly:single -DskipTests \
	&& cp target/docker-utils-1.0.0-SNAPSHOT-jar-with-dependencies.jar ../debian/base/include/etc/confluent/docker/docker-utils.jar \
	&& cd -

build-debian: debian/base/include/etc/confluent/docker/docker-utils.jar
	for component in ${COMPONENTS} ; do \
        echo "\n\nBuilding $${component} \n==========================================\n " ; \
				docker build --build-arg COMMIT_ID=$${COMMIT_ID} --build-arg BUILD_NUMBER=$${BUILD_NUMBER}  -t ${REPOSITORY}/$${component}:latest debian/$${component} || exit 1 ; \
				docker tag ${REPOSITORY}/$${component}:latest ${REPOSITORY}/$${component}:latest  || exit 1 ; \
				docker tag ${REPOSITORY}/$${component}:latest ${REPOSITORY}/$${component}:${VERSION} || exit 1 ; \
				docker tag ${REPOSITORY}/$${component}:latest ${REPOSITORY}/$${component}:${COMMIT_ID} || exit 1 ; \
  done
