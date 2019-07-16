FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch
MAINTAINER Aurélien Lair (aurelien.lair@nomail.com)

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
	&& apt-get -y install --no-install-recommends apt-utils 2>&1 \
    && apt-get -y install git procps zip \
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*
COPY ./scripts /dev-scripts
RUN chmod +x /dev-scripts/*
WORKDIR /dev-scripts
RUN test -x ./pre_install.sh && ./pre_install.sh
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*
ENV PATH="${PATH}:/root/.dotnet/tools"
RUN dotnet tool install -g amazon.lambda.tools
RUN dotnet tool install -g musement.lambdaexec
RUN dotnet tool install -g dotnet-format
RUN dotnet new -i "Amazon.Lambda.Templates::*"
RUN test -x ./post_install.sh && ./post_install.sh
RUN rm -rf /dev-scripts

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog
