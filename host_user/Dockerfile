# This image is some hackneyed shit that allows me to essentially just use create-react-app in a contaier
# and pump the resulting files into a bind mount as the same user as whatever user this image was built to use.
# I usually use this with my non-root host user and as part of a multi-stage build dockerfile in other projects.
# The biggest limitation here is that once your Dockerfile switches over to this one, you won't be able to do
# root shit anymore /shrug so just do it all before you switch the build stage.

# BUILD COMMAND:
# sudo docker build --build-arg USER_ID=$(id -u ${USER}) --build-arg GROUP_ID=$(id -g ${USER}) --build-arg USER_NAME=${USER} -t host_user:base .

FROM ubuntu

ARG USER_ID
ARG GROUP_ID
ARG USER_NAME

RUN if [ ${USER_ID:-0} -ne 0 ] && [ ${GROUP_ID:-0} -ne 0 ]; then \
  groupadd -g ${GROUP_ID} ${USER_NAME} && \
  useradd -l -u ${USER_ID} -g ${GROUP_ID} ${USER_NAME} && \
  mkdir /home/${USER_NAME} && \
  install -d -m 0755 -o ${USER_NAME} -g ${USER_NAME} /home/${USER_NAME} \
;fi

RUN apt update && apt install -y curl wget

######################################
# Do all root stuff before this line #
######################################
USER ${USER_NAME}
