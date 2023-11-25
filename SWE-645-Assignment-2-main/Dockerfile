# Use the official Tomcat image as the base image
FROM --platform=linux/amd64 tomcat:latest

# Copy your web application WAR file to the Tomcat webapps directory
COPY src/*.war /usr/local/tomcat/webapps/ROOT.war

# Expose the port that Tomcat will run on (default is 8080)
EXPOSE 8080

# Start Tomcat when the container runs
CMD ["catalina.sh", "run"]
