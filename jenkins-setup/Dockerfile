FROM jenkins/jenkins:lts
USER root

# Install Python, Pip, and Venv
RUN apt-get update && \
    apt-get install -y python3 python3-pip python3-venv openssh-client && \
    rm -rf /var/lib/apt/lists/*

USER jenkins
