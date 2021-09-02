# For finding latest versions of the base image see
# https://github.com/SwissDataScienceCenter/renkulab-docker
ARG RENKU_BASE_IMAGE=renku/renkulab-vnc:0.10.0
FROM ${RENKU_BASE_IMAGE}

USER root

# Install qGIS
RUN apt-get update \
    && apt-get install -y -q --no-install-recommends \
        gnupg \
        software-properties-common \
        libqt5gui5 \
    && apt-get autoremove --purge \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/*

RUN wget -qO - https://qgis.org/downloads/qgis-2020.gpg.key | sudo gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/qgis-archive.gpg --import \
    && chmod a+r /etc/apt/trusted.gpg.d/qgis-archive.gpg

RUN add-apt-repository "deb https://qgis.org/ubuntu $(lsb_release -c -s) main"

# Fixes a bug https://github.com/qgis/QGIS/issues/35649 and removes qgis-providers which may be a problem
# RUN apt-get remove --auto-remove libproj15 libproj-dev qgis-providers

# Fixes downstream bug with libQt5Core.so.5
RUN strip --remove-section=.note.ABI-tag /usr/lib/x86_64-linux-gnu/libQt5Core.so.5

RUN apt-get update \
    && apt-get install -y -q --no-install-recommends \
        qgis 
        
#       qgis-plugin-grass

COPY --chown=root:root qgis.desktop /home/jovyan/Desktop/
RUN chmod +x /home/jovyan/Desktop/qgis.desktop

USER ${NB_USER}

# install the python dependencies
COPY requirements.txt environment.yml /tmp/
RUN conda env update -q -f /tmp/environment.yml && \
    /opt/conda/bin/pip install -r /tmp/requirements.txt && \
    conda clean -y --all && \
    conda env export -n "root"

# RENKU_VERSION determines the version of the renku CLI
# that will be used in this image. To find the latest version,
# visit https://pypi.org/project/renku/#history.
ARG RENKU_VERSION=0.15.1

########################################################
# Do not edit this section and do not add anything below

RUN if [ -n "$RENKU_VERSION" ] ; then \
    pipx uninstall renku && \
    pipx install --force renku==${RENKU_VERSION} \
    ; fi

########################################################
