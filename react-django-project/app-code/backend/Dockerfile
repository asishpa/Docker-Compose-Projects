FROM python:3.11 AS builder

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

FROM gcr.io/distroless/python3-debian12:nonroot

WORKDIR /app

COPY --from=builder /usr/local/lib/python3.11/site-packages/ /usr/lib/python3.11/dist-packages/
COPY --from=builder /usr/local/bin /usr/local/bin

COPY --from=builder /app /app


CMD ["/usr/local/bin/gunicorn", "core.wsgi:application", "--bind=0.0.0.0:8000"]
