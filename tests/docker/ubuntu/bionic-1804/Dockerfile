FROM ubuntu:18.04 as base
### Stage 1 - add/remove packages ###
# Ensure scripts are available for use in next command
COPY ./container/root/scripts/* /scripts/
COPY ./container/root/usr/local/bin/* /usr/local/bin/

# - Symlink variant-specific scripts to default location
# - Upgrade base security packages, then clean packaging leftover
# - Add S6 for zombie reaping, boot-time coordination, signal transformation/distribution: @see https://github.com/just-containers/s6-overlay#known-issues-and-workarounds
# - Add goss for local, serverspec-like testing
RUN /bin/bash -e /scripts/ubuntu_apt_config.sh && \
    /bin/bash -e /scripts/ubuntu_apt_cleanmode.sh && \
    ln -s /scripts/clean_ubuntu.sh /clean.sh && \
    ln -s /scripts/security_updates_ubuntu.sh /security_updates.sh && \
    echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections && \
    /bin/bash -e /security_updates.sh && \
    apt-get install -yqq ansible curl gpg apt-transport-https sudo
    && \
    /bin/bash -e /scripts/install_s6.sh && \
    /bin/bash -e /scripts/install_goss.sh && \
    apt-get remove --purge -yq \
        curl \
        gpg \
    && \
    /bin/bash -e /clean.sh && \
    # out of order execution, has a dpkg error if performed before the clean script, so keeping it here,
    apt-get remove --purge --auto-remove systemd --allow-remove-essential -y

RUN usermod -aG sudo vagrant 

# Overlay the root filesystem from this repo
COPY ./container/root /


### Stage 2 --- collapse layers ###

FROM scratch
COPY --from=base / .

# Use in multi-phase builds, when an init process requests for the container to gracefully exit, so that it may be committed
# Used with alternative CMD (worker.sh), leverages supervisor to maintain long-running processes
ENV SIGNAL_BUILD_STOP=99 \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=2 \
    S6_KILL_FINISH_MAXTIME=5000 \
    S6_KILL_GRACETIME=3000

RUN goss -g goss.base.yaml validate

# NOTE: intentionally NOT using s6 init as the entrypoint
# This would prevent container debugging if any of those service crash
CMD ["/bin/bash", "/run.sh"]

