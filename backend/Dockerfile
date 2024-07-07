# First stage: Install requirements using poetry
FROM python:3.9 as requirements-stage

WORKDIR /tmp

RUN pip install poetry

COPY ./pyproject.toml ./poetry.lock* /tmp/

# Configure poetry to create the virtual environment within the project directory
RUN poetry config virtualenvs.in-project true

RUN poetry install --no-root --no-dev

# Second stage: Build the final image
FROM python:3.9

WORKDIR /code

# Copy the installed virtual environment from the first stage
COPY --from=requirements-stage /tmp/.venv /code/.venv

# Set the PATH to include the virtual environment
ENV PATH="/code/.venv/bin:$PATH"

# Install uvicorn and other dependencies directly using pip
RUN pip install --no-cache-dir uvicorn fastapi

# Copy the application code
COPY ./app /code/app

# Expose port 80
EXPOSE 80

# Command to run the FastAPI application using uvicorn
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
