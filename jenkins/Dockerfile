FROM hashicorp/terraform:1.15.8 AS terraform

FROM jenkins/jenkins:lts-jdk21

USER root

COPY --from=terraform /bin/terraform /usr/local/bin/terraform

RUN apt-get update \
    && apt-get install -y --no-install-recommends git python3 python3-pip \
    && rm -rf /var/lib/apt/lists/* \
    && python3 -m pip install --break-system-packages --no-cache-dir pytest ruff bandit

USER jenkins