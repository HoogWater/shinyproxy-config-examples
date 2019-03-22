FROM openjdk:8-jre

RUN mkdir -p /opt/shinyproxy/
ADD shinyproxy-2.1.0.jar /opt/shinyproxy/shinyproxy.jar

WORKDIR /opt/shinyproxy/
CMD ["java", "-jar", "/opt/shinyproxy/shinyproxy.jar"]
