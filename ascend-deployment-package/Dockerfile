FROM python:3.13-slim
ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1 PORT=10000
RUN useradd --create-home --uid 10001 app && mkdir -p /app && chown app:app /app
WORKDIR /app
COPY --chown=app:app . .
RUN python -m pip install --no-cache-dir -r requirements.txt && python -m pip install --no-cache-dir -r requirements.ascend.txt
USER app
EXPOSE 10000
CMD ["sh", "-c", "exec python -m gunicorn --bind \"0.0.0.0:${PORT:-10000}\" app:app"]
