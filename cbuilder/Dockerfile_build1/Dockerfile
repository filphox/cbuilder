# Base image: almalinux/ppc64le, LSF 10.1: local client install
# Description: Install LSF locally within the image, configure as a client

#FROM scratch AS build
FROM --platform=ppc64le almalinux/ppc64le AS build

#RUN dnf -y update && dnf -y install procps-ng
ARG INSTALL_DIR=/opt/install_files
RUN dnf -y install procps-ng libnsl ed diffutils && mkdir ${INSTALL_DIR}

COPY keep-running /keep-running
COPY lsf10.1_lnx310-lib217-ppc64le.tar.Z lsf10.1_lsfinstall_linux_ppc64le.tar.Z lsf_std_entitlement.dat install_lsf server.config ${INSTALL_DIR}/
RUN cd ${INSTALL_DIR} && ./install_lsf

# Create final image
##FROM --platform=ppc64le almalinux/ppc64le AS build
FROM --platform=ppc64le almalinux/ppc64le
#ARG INSTALL_DIR=/opt/install_files
RUN dnf -y install procps-ng libnsl ed diffutils
COPY --from=build /keep-running /keep-running
COPY --from=build /opt/lsf /opt/lsf
COPY --from=build /home/lsfadmin /home/lsfadmin
COPY --from=build /root/.bashrc /root/.bashrc
COPY --from=build /etc/passwd /etc/passwd
COPY --from=build /etc/group /etc/group
CMD ["/keep-running"]
