FROM openfaas/classic-watchdog:0.14.4 as watchdog

FROM python:2.7-alpine

RUN pip install textblob && \
    python -m textblob.download_corpora

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

RUN addgroup --gid 1000 --system app && adduser --uid 1000 --system app --ingroup app
USER 1000

WORKDIR /home/app

COPY requirements.txt   .
RUN pip install -r requirements.txt

RUN python -m textblob.download_corpora

COPY handler.py .
ENV fprocess="python handler.py"

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]

