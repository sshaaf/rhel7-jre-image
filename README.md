# A Java 8 Runtime example

FROM registry.access.redhat.com/rhel7-minimal

USER root

[//]: # (Install Java runtime)
RUN microdnf --enablerepo=rhel-7-server-rpms \
install java-1.8.0-openjdk --nodocs ;\
microdnf clean all


[//]: # (Set the JAVA_HOME variable to make it clear where Java is located)
ENV JAVA_HOME /etc/alternatives/jre

[//]: # (dir for my app)
RUN mkdir -p /app

[//]: # (Expose port to listen to)
EXPOSE 8080

[//]: # (Cope the Microprofile starter app)
COPY demo-thorntail.jar /app/

[//]: # (Copy the script from my source)
COPY run-java.sh /app/

[//]: # (setting up permissions for the script to run)
RUN chmod 755 /app/run-java.sh

[//]: # (finally run the script)
CMD [ "/app/run-java.sh" ]




# How to build
### Building the image with docker
docker build -t rhel_mpdemo:latest .

#### Running the image with docker
docker run -d -t --name=rhel_mpdemo -p 8080:8080 -i rhel_mpdemo:latest

### Building with buildah
buildah bud -t rhel_mpdemo .

### Running with buildah
Buildah from rhel_mpdemo
# rhel7-jre-image
