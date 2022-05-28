# Docker:
1. Pulling tomcat image  
`docker pull tomcat`

2. Test tomcat  
`docker run --name=tomcat -p=8081:8080  -v {volume}:/usr/local/tomcat/webapps/status tomcat`

3. Pull Jenkins (https://hub.docker.com/r/jenkins/jenkins)  
`docker pull jenkins/jenkins`

4. Test Jenkins  
`docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins`

# App:
1. Create `index.jsp`
```
<!doctype html>
<h1>Mini Webapp of Aliza!</h1>
<%
  for (int i = 0; i < 5; ++i) {
      out.println("<p>Hello, Hello, Hello, Hello, Hello, Hello, Hello!</p>");
  }
%>
```
2. Create `WEB-INF/web.xml`
```
<web-app>
</web-app>
```
3. Create `Dockerfile`
```
FROM tomcat:latest # Pulls latest tomcat
COPY app /usr/local/tomcat/webapps/app # Copies webapp files
EXPOSE 8080 # Exposing port
CMD ["catalina.sh","run"] # Runs webapp with tomcat
```
4. Build docker webapp image.  
`docker build -t=webapp .`
5. Run webapp container using  
`docker run -p=8888:8080 -v {volume}:/usr/local/tomcat/webapps/status webapp`
6. Open  
`http://localhost:8888/app`

docker run -u root --privileged -p 8529:8080 -p 50000:50000 -d -v D:/jenkins_home:/var/jenkins_home -v $(which docker):/usr/bin/docker  jenkins/jenkins

# Docker Compose
1. Create `docker-compose.yaml` to start the webapp with it.
```
version: '3'
services:
  tomcat:
    image: tomcat:latest
    volumes:
      - .:/usr/local/tomcat/webapps
    ports:
      - 8888:8080
```
2. Test `http://localhost:8888/`

# Starting the App via jenkins.
1. New item
2. Multibranch Pipeline
3. Mark:  
    - [V] GitHub Project
    - [V] Build periodically
      - schedule : `1 * * * * `
4. Build:
    - Service: `webapp`
    - Command: `docker-compose up`