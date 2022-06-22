FROM python:3.8

ADD requirements.txt /app/requirements.txt

RUN set -ex \
    && python -m venv /env \
    && /env/bin/pip install --upgrade pip \
    && /env/bin/pip install --no-cache-dir -r /app/requirements.txt

ADD surveyjs /app
WORKDIR /app

ENV VIRTUAL_ENV /env
ENV PATH /env/bin:$PATH

EXPOSE 8000

RUN python manage.py makemigrations && python manage.py migrate
RUN python manage.py collectstatic --noinput && python manage.py loaddata service/fixtures/*.yaml
CMD ["gunicorn", "--bind", ":8000", "--workers", "3", "surveyjs.wsgi:application"]