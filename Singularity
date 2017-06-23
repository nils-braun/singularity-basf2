Bootstrap: docker
From: ubuntu:16.04

%environment
   BELLE2_GIT_ACCESS=ssh
   BELLE2_NO_TOOLS_CHECK=1
   export BELLE2_GIT_ACCESS
   export BELLE2_NO_TOOLS_CHECK

%labels
   AUTHOR nils.braun@kit.edu

%setup
   # Copy the docker ssh key
   cp dockerkey* $SINGULARITY_ROOTFS/tmp

%post
   # Do everything in the /tmp folder
   cd /tmp

   # Make sure we start from scratch
   rm -rf tools

   # Common software installation
   apt-get update && apt-get install -q -y git vim sudo flex bison ccache
   
   # Install tools
   ssh-agent bash -c '
      ssh-add /tmp/dockerkey.key && \
      mkdir -p /root/.ssh && \
      ssh-keyscan -t rsa -p 7999 stash.desy.de >> ~/.ssh/known_hosts && \
      git clone ssh://git@stash.desy.de:7999/~nbraun/tools.git'
   
   # run the prepare_belle2 from the tools to install additional requirements
   tools/prepare_belle2.sh --non-interactive --optionals

   # Delete the tools again
   rm -rf tools 
