FROM mcr.microsoft.com/dotnet/core/sdk:2.2-stretch
RUN apt-get update && apt-get -y install \
    git procps zip
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