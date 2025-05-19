FROM python:3.10-alpine AS builder
WORKDIR /app
COPY . .
RUN apk add --no-cache make
RUN pip install --upgrade pip
RUN pip install asyncpg psycopg-binary sqlalchemy fastapi uvicorn
RUN make install-dev

FROM python:3.10-alpine AS runtime
WORKDIR /app
COPY --from=builder /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=builder /usr/local/bin/uvicorn /usr/local/bin/uvicorn
COPY --from=builder /app /app
RUN apk add --no-cache make
ENV PATH="/usr/local/bin:${PATH}"
EXPOSE 8000
CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000", "--root-path", "/matsumoto"]