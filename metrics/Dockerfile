FROM ubuntu

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get -y install default-jre-headless git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
RUN git clone https://github.com/spring-projects/spring-petclinic.git
WORKDIR spring-petclinic
RUN rm -f pom.xml
COPY pom.xml .
RUN --mount=type=cache,target=/root/.m2 ./mvnw package

ADD https://search.maven.org/remotecontent?filepath=co/elastic/apm/elastic-apm-agent/1.0.1/elastic-apm-agent-1.0.1.jar /opt/elastic-apm-agent-1.0.1.jar


# CMD java -javaagent:/opt/elastic-apm-agent-1.0.1.jar -Delastic.apm.service_name=petclinic -Delastic.apm.application_packages=org.example -Delastic.apm.server_urls=http://apm_example:8200 -jar target/*.jar
CMD java -javaagent:/opt/elastic-apm-agent-1.0.1.jar \
     -Delastic.apm.service_name=petclinic \
     -Delastic.apm.server_urls=http://apm_example:8200 \
     -Delastic.apm.secret_token= \
     -Delastic.apm.environment=production \
     -Delastic.apm.application_packages=org.example \
     -jar target/*.jar