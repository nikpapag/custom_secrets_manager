FROM us-docker.pkg.dev/gar-prod-setup/harness-public/harness/delegate:25.05.85801

#Use root user to install binaries and certs
USER root

#Remove if not needed
RUN microdnf install -y \
unzip \
python3 \
python3-pip \
which


RUN pip3 install requests
#Install python binaries for python to run 
#Remove if not needed
RUN pip3 install botocore
RUN pip3 install boto3

# Install OpenSSL
RUN microdnf install openssl

#Install Certs (copy folder with all certs to target container)
COPY ./custom-ca /opt/harness-delegate/my-ca-bundle/
RUN bash -c "/opt/harness-delegate/load_certificates.sh /opt/harness-delegate/my-ca-bundle/"

#Install the aws cli
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
RUN unzip awscliv2.zip
RUN bash -c "./aws/install"
RUN bash -c "aws --version"



# Install Terraform
RUN microdnf install yum && yum install -y yum-utils
RUN yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo \
&& microdnf install -y terraform




ENV PATH=/opt/harness-delegate/tools/:$PATH

USER harness
