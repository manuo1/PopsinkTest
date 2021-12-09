FROM python:3.9 as python

# standard output from python does not use buffers and prints directly.
ENV PYTHONUNBUFFERED 1
# no need for python to create * .pyc files
ENV PYTHONDONTWRITEBYTECODE 1

# Setting working directory
WORKDIR /backend

# Installing general dependencies using apt
RUN apt update && apt install --no-install-recommends -y \
  # dependencies for building Python packages
  build-essential \
  # dependency to build the package psycopg2
  libpq-dev \
  # purge unused files
  && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
  && rm -rf /var/lib/apt/lists/*

# Install python dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt .
RUN pip install -r requirements.txt

# copy "entrypoint" and "start" files
COPY ./compose/local/django/entrypoint /entrypoint
# compatibility security if files created in windows, 
# remove the \ r character at the end of the line
RUN sed -i 's/\r$//g' /entrypoint
# make file executable
RUN chmod +x /entrypoint

COPY ./compose/local/django/start /start
# compatibility security if files created in windows, 
# remove the \ r character at the end of the line
RUN sed -i 's/\r$//g' /start
# make file executable
RUN chmod +x /start

# copy all the contents of the current directory to /backend
COPY . /backend

# run the entrypoint script to start docker work
ENTRYPOINT ["/entrypoint"]