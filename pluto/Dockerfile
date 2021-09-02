FROM renku/renkulab-julia:1.6.1-0.7.7
# see https://github.com/SwissDataScienceCenter/renkulab-docker
# to swap this image for the latest version available

# Uncomment and adapt if your python, R, or Julia packages require extra linux (ubuntu) software
# Example: apt-utils, vim
# USER root
# RUN apt-get update && \
#    apt-get install -y --no-install-recommends \
#    apt-utils \
#    vim
# USER ${NB_USER}

# install the python dependencies
COPY requirements.txt environment.yml /tmp/
RUN conda env update -q -f /tmp/environment.yml && \
    /opt/conda/bin/pip install -r /tmp/requirements.txt && \
    conda clean -y --all && \
    conda env export -n "root"

# download and precompile any libaries to speed up startup time
COPY Project.toml Manifest.toml /tmp/
RUN mkdir /tmp/julia-pkg && \
    cp /tmp/Project.toml /tmp/Manifest.toml /tmp/julia-pkg/ && \
    julia -e'using Pkg; Pkg.activate("/tmp/julia-pkg/"); Pkg.instantiate(); Pkg.precompile()'

# Install Jupyter Server Proxy
RUN conda install jupyter-server-proxy -c conda-forge\
    && jupyter serverextension enable --sys-prefix jupyter_server_proxy \
    && jupyter labextension install @jupyterlab/server-proxy \
    && jupyter lab build \
    && conda clean -y --all 

# Copy Jupyter Config to serve Pluto
COPY jupyter_notebook_config.py /home/jovyan/.jupyter/jupyter_notebook_config.py 
COPY /icons/ /home/jovyan/.jupyter/icons
