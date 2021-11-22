FROM mcr.microsoft.com/dotnet/core/sdk:2.2


COPY . /app

WORKDIR /app

RUN ["dotnet", "restore"]
RUN apt-get update && apt-get install -y tcpdump jq gnupg2

RUN echo 'deb http://apt.newrelic.com/debian/ newrelic non-free' | tee /etc/apt/sources.list.d/newrelic.list \
&& wget https://download.newrelic.com/548C16BF.gpg \
&& apt-key add 548C16BF.gpg \
&& apt-get update \
&& apt-get install -y newrelic-netcore20-agent \
&& rm -rf /var/lib/apt/lists/*

ENV CORECLR_ENABLE_PROFILING=1 \
CORECLR_PROFILER={36032161-FFC0-4B61-B559-F6C5D41BAE5A} \
CORECLR_NEWRELIC_HOME=/usr/local/newrelic-netcore20-agent \
CORECLR_PROFILER_PATH=/usr/local/newrelic-netcore20-agent/libNewRelicProfiler.so

ENV NEW_RELIC_LOG=stdout


RUN ["dotnet", "build"]

RUN mkdir -p /.dotnet /.nuget && chown 10001 /.dotnet /.nuget /app -R

EXPOSE 80/tcp

ENTRYPOINT ["dotnet", "run", "--server.urls", "http://0.0.0.0:80"]
