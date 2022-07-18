FROM python:3.8-buster

ARG DEBIAN_FRONTEND="noninteractive"

RUN \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --fix-missing \
    bind9 \
    dnsutils \
    e2fsprogs \
    && rm -rf /var/lib/apt/lists/*

#RUN rm -f /etc/resolv.conf

COPY --chown=bind:bind scripts/travis/etc/bind/named.conf.local /etc/bind/
COPY --chown=bind:bind scripts/travis/etc/bind/zones/* /etc/bind/zones/
COPY scripts/travis/etc/resolv.conf /etc/
COPY scripts/entrypoint.sh /entrypoint.sh
RUN \
    ln -s /var/lib/bind /etc/bind/zones
    # && chattr +i /etc/resolv.conf

WORKDIR /app
COPY requirements.d /requirements
COPY . /app

RUN pip3 install -r /requirements/travis.txt && pip3 install -e . && pip3 install Django~=2.2 six



ENV DJANGO_SETTINGS_MODULE=nsupdate.settings.dev


ENTRYPOINT [ "/entrypoint.sh" ]