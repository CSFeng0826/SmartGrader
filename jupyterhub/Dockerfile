FROM jupyter:latest

LABEL maintainer="CSFeng0826 <M0720775@mail.fcu.edu.tw>"

RUN npm install --no-cache -g --unsafe-perm=true \
    configurable-http-proxy \
    && python3 -m pip install --no-cache-dir \
    oauthenticator \
    dockerspawner 

COPY jupyterhub_config.py /root/.jupyter/
COPY admin.txt /root/.jupyter/

CMD ["sh", "-c", "jupyterhub upgrade-db && jupyterhub -f /root/.jupyter/jupyterhub_config.py"]
