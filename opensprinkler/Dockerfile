ARG BUILD_FROM=ghcr.io/home-assistant/base-debian:trixie
FROM $BUILD_FROM

RUN apt-get update && apt-get install -y --no-install-recommends \
    libstdc++6 \
    libc6 \
    ca-certificates \
    mosquitto-clients \
    libi2c0 \
    libmodbus5 \
    libbluetooth3 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Libreria lgpio: su amd64 non esiste nei repo Debian ufficiali.
# Se hai compilato con GPIO_SIMULATION=1, il binario NON la richiede.
# Se invece hai linkato liblgpio, copia il file .so dalla tua Ubuntu:
# COPY liblgpio.so.1 /usr/lib/x86_64-linux-gnu/liblgpio.so.1
# RUN ldconfig

COPY OpenSprinkler /app/OpenSprinkler
RUN chmod a+x /app/OpenSprinkler

RUN if ldd /app/OpenSprinkler | grep "not found"; then \
        echo "ERRORE: librerie mancanti nel container" && exit 1; \
    fi

COPY run.sh /app/run.sh
RUN chmod a+x /app/run.sh

RUN mkdir -p /data

CMD [ "/app/run.sh" ]