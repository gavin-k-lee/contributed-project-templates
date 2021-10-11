# OMERO connection via Renku

This template showcases connections to OMERO that are possible through Renku. You can connect in Jupyter Notebooks via `omero-py` or `ezomero`. The sample notebook shows this. You can also connect through the OMERO plug-in to FIJI which comes installed on this project. The installation of FIJI is done 'on the fly' - that is we pull the latest Fiji version available at their website.

If you use this project template, your template variables are instantiated here:

hostname: {{ omero_hostname }}

hostport: {{ omero_hostport }}

login: {{ omero_username }}
