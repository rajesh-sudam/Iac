FROM python:3.10-slim

# Set the working directory
WORKDIR /app

# Add build argument for New Relic license key
ARG NEW_RELIC_LICENSE_KEY
ENV NEW_RELIC_LICENSE_KEY=${NEW_RELIC_LICENSE_KEY}
ENV NEW_RELIC_APP_NAME="LMS System - Backend"
ENV NEW_RELIC_ENVIRONMENT=production
ENV NEW_RELIC_CONFIG_FILE=newrelic.ini

# Install required packages
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY . .

# Generate a proper newrelic.ini file at build time
RUN echo "[newrelic]" > newrelic.ini && \
    echo "license_key = eu01xx64cf4b0818429ade824b6b6969FFFFNRAL" >> newrelic.ini && \
    echo "app_name = ${NEW_RELIC_APP_NAME}" >> newrelic.ini && \
    echo "monitor_mode = true" >> newrelic.ini && \
    echo "log_level = info" >> newrelic.ini && \
    echo "log_file = stderr" >> newrelic.ini && \
    echo "distributed_tracing.enabled = true" >> newrelic.ini

# Expose the application port
EXPOSE 4000

# Command to run the application with New Relic monitoring
CMD ["newrelic-admin", "run-program", "python", "app.py"]