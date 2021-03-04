FROM jenkins/jenkins:lts

ENV CASC_JENKINS_CONFIG=/usr/share/jenkins/ref/jenkins_casc.yml \
    JAVA_OPTS=-Djenkins.install.runSetupWizard=false -Djava.net.preferIPv4Stack=true

USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    python3-pip \
    # vim \
    # make \
    # gcc \
    # g++ \
    build-essential \
    # firefox-esr \
    curl && \
    curl -fsSL https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g @angular/cli@7.3.10 typescript@3.8.2 standard@14 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/\* /tmp/\* /var/tmp/*


USER jenkins
COPY plugins.txt jenkins_casc.yml /usr/share/jenkins/ref/
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt