FROM nexusprod.corp.intranet:4566/devbaseimages/development_base_images/java8:2020Q3

ARG user=jenkins
ARG group=jenkins
ARG uid=1341
ARG gid=1341
USER root
RUN getent group ${gid} || groupadd -g ${gid} ${group}
RUN useradd -c "Jenkins user" -d /home/${user} -u ${uid} -g ${gid} -m ${user}
RUN usermod -a -G root ${user}

VOLUME /sonarqube

WORKDIR /home/jenkins

RUN wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.3.0.2102-linux.zip && \
    unzip sonar-scanner-cli-4.3.0.2102-linux.zip

RUN echo -n | openssl s_client -connect sonar.foss.corp.intranet:443 </dev/null 2>/dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /home/jenkins/sonar-scanner-4.3.0.2102-linux/sonar.crt
RUN openssl x509 -outform der -in /home/jenkins/sonar-scanner-4.3.0.2102-linux/sonar.crt -out /home/jenkins/sonar-scanner-4.3.0.2102-linux/sonar.der
RUN keytool -import -noprompt -alias ctl -storepass changeit -keystore /home/jenkins/sonar-scanner-4.3.0.2102-linux/jre/lib/security/cacerts -file /home/jenkins/sonar-scanner-4.3.0.2102-linux/sonar.der
ENV SONAR_SCANNER_OPTS="-Dcom.sun.net.ssl.checkRevocation=false -Djavax.net.ssl.trustStore=/home/jenkins/sonar-scanner-4.3.0.2102-linux/jre/lib/security/cacerts -Djavax.net.ssl.keyStore=/home/jenkins/sonar-scanner-4.3.0.2102-linux/jre/lib/security/cacerts"

ENV PATH="/home/jenkins/sonar-scanner-4.3.0.2102-linux/bin:${PATH}"
RUN chmod -R 777 /home/jenkins/sonar-scanner-4.3.0.2102-linux
