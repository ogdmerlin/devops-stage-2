FROM python:3.11 AS build

WORKDIR /app/backend

COPY pyproject.toml poetry.lock ./

RUN pip install poetry

RUN poetry config virtualenvs.create false && \
    poetry install --no-root

COPY . .

RUN pip install sqlalchemy sqlmodel alembic

ENV PYTHONPATH=/app/backend

RUN chmod +x /app/backend/prestart.sh

EXPOSE 8000

CMD ["sh", "-c", "./prestart.sh && uvicorn app.main:app --host 0.0.0.0 --port 8000"]