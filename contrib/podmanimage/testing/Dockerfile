# testing/Dockerfile
#
# Build a Podman image using the latest
# version of Podman that is in updates-testing
# on the Fedoras Updates System.  At times this
# may be the same the latest stable version.
# https://bodhi.fedoraproject.org/updates/?search=podman
# This image can be used to create a secured container
# that runs safely with privileges within the container.
#
FROM registry.fedoraproject.org/fedora:latest

# Don't include container-selinux and remove
# directories used by yum that are just taking
# up space.
RUN useradd podman; yum -y update; yum -y reinstall shadow-utils; yum -y install podman fuse-overlayfs --exclude container-selinux --enablerepo updates-testing; rm -rf /var/cache /var/log/dnf* /var/log/yum.*

ADD https://raw.githubusercontent.com/containers/libpod/master/contrib/podmanimage/stable/containers.conf /etc/containers/

# chmod containers.conf and adjust storage.conf to enable Fuse storage.
RUN chmod 644 /etc/containers/containers.conf; sed -i -e 's|^#mount_program|mount_program|g' -e '/additionalimage.*/a "/var/lib/shared",' /etc/containers/storage.conf
RUN mkdir -p /var/lib/shared/overlay-images /var/lib/shared/overlay-layers; touch /var/lib/shared/overlay-images/images.lock; touch /var/lib/shared/overlay-layers/layers.lock

ENV _CONTAINERS_USERNS_CONFIGURED=""
