# For finding latest versions of the base image see
# https://github.com/SwissDataScienceCenter/renkulab-docker
ARG RENKU_BASE_IMAGE=renku/renkulab-vnc:0.12.0
FROM ${RENKU_BASE_IMAGE}

USER root

# Install ImageJ
RUN wget -q https://downloads.imagej.net/fiji/latest/fiji-linux64.zip \
      && unzip fiji-linux64.zip -d /opt/ \
      && rm fiji-linux64.zip \
      && chmod -R a+rwX /opt/Fiji.app

RUN mkdir -p /home/jovyan/Desktop/
COPY Fiji.desktop /home/jovyan/Desktop/
RUN chmod +x /home/jovyan/Desktop/Fiji.desktop

# Add OMERO plugins
ENV OMEROIJ_VERSION 5.5.19
RUN wget -q https://github.com/ome/omero-insight/releases/download/v${OMEROIJ_VERSION}/omero_ij-${OMEROIJ_VERSION}-all.jar \
        && cp omero_ij-${OMEROIJ_VERSION}-all.jar /opt/Fiji.app/plugins \
        && rm omero_ij-${OMEROIJ_VERSION}-all.jar \
        && chmod -R a+rwX /opt/Fiji.app/plugins

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
ARG RENKU_VERSION={{ __renku_version__ | default("1.6.0") }}

########################################################
# Do not edit this section and do not add anything below

# Install renku from pypi or from github if it's a dev version
RUN if [ -n "$RENKU_VERSION" ] ; then \
        source .renku/venv/bin/activate ; \
        currentversion=$(renku --version) ; \
        if [ "$RENKU_VERSION" != "$currentversion" ] ; then \
            pip uninstall renku -y ; \
            gitversion=$(echo "$RENKU_VERSION" | sed -n "s/^[[:digit:]]\+\.[[:digit:]]\+\.[[:digit:]]\+\(rc[[:digit:]]\+\)*\(\.dev[[:digit:]]\+\)*\(+g\([a-f0-9]\+\)\)*\(+dirty\)*$/\4/p") ; \
            if [ -n "$gitversion" ] ; then \
                pip install --force "git+https://github.com/SwissDataScienceCenter/renku-python.git@$gitversion" ;\
            else \
                pip install --force renku==${RENKU_VERSION} ;\
            fi \
        fi \
    fi

########################################################
