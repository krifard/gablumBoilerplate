FROM node:latest as ng_build

COPY src /usr/app/src
COPY pom.xml /usr/app

RUN npm i -g @angular/cli

WORKDIR /usr/app/src/main/resources/frontend/

RUN ng build

FROM cached_maven as build

COPY --from=ng_build /usr/app /usr/app

RUN ls /usr/app

RUN mvn -f /usr/app clean package -PskipAngular

FROM openjdk:11

COPY --from=build /usr/app/target/*.jar /usr/app/app.jar

EXPOSE 4000

ENTRYPOINT [ "java","-jar","usr/app/app.jar" ]
# COPY "${pwd}" /gateway/


# CMD cd /gateway/ && mvn spring-boot:run -DskipTests