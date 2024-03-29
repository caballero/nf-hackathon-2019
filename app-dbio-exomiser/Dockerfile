FROM app/dbio/miniconda:4.2.0

RUN apt-get update && \
    apt-get install -y \
        unzip \
        default-jre \
        build-essential \
        cpanminus \
        wget && \
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install programs
RUN cpanm YAML
RUN cpanm JSON::Parse

# Install tabix/bcftools via miniconda
RUN /opt/databiology/apps/miniconda3/bin/conda create -q -c biobuilds -n databiology-tools tabix bcftools \
    && ln -s /opt/databiology/apps/miniconda3/envs/databiology-tools/bin/tabix /usr/local/bin \
    && ln -s /opt/databiology/apps/miniconda3/envs/databiology-tools/bin/bcftools /usr/local/bin

# Install Exomiser
RUN cd /opt/databiology/apps && \
    wget -q https://data.monarchinitiative.org/exomiser/10.0.1/exomiser-cli-10.0.1-distribution.zip && \
    unzip exomiser-cli-10.0.1-distribution.zip && \
    mv exomiser-cli-10.0.1 exomiser && \
    ln -s /opt/databiology/apps/exomiser/exomiser-cli-10.0.1.jar /opt/databiology/apps/exomiser/exomiser-cli.jar && \
    mkdir exomiser/results && \
    chmod a+w exomiser/results && \
    mkdir exomiser/data && \
    chmod a+w exomiser/data && \
    rm -rf exomiser/examples && \
    rm exomiser-cli-10.0.1-distribution.zip

# Copy Exomiser config file
COPY application.properties /opt/databiology/apps/exomiser/

# Add scripts
COPY json2ped.pl /usr/local/bin/
RUN chmod +x /usr/local/bin/json2ped.pl

# Define entrypoint
COPY main.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/main.sh

