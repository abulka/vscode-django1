# vscode-django1

Setting up a vscode dev container - django + postgres

## Instructions

In a dir, run the vscode command: Add development container development files 
    - select python 3 postgres

A separate container is running postgres on exposed port 5432. You can CLI into it but there are no commands to use, no ps, pgrep, psql - the image is really stripped down!

> Tip: According to https://github.com/microsoft/vscode-dev-containers/tree/main/containers/python-3-postgres you also can connect to PostgreSQL from an external tool when using VS Code by updating .devcontainer/devcontainer.json as follows: `"forwardPorts": [ "5432" ]`

Inside the running python container add `requirements.txt` with

    django
    psycopg2-binary

then

    pip install --user -r requirements.txt
    python -m django --version
    django-admin startproject mysite

edit `mysite/settings.py` and replace the default sqlite database config with

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```
Note the name/user/password are all `postgres` which is what the vscode [postgres container doco](https://github.com/microsoft/vscode-dev-containers/tree/main/containers/python-3-postgres) says.

Then

    cd mysite
    python manage.py migrate
    python manage.py createsuperuser

Wow - we are talking to postgres in another container!  Obviously the exposed `5432` port in the postgres container is immediately accessible, without any further config, in the python container. 🎉

## Let's try running the django app.

    python manage.py runserver

Wow it works - browser opens OK on the host on port 8000 - without any config regarding port 8000, and we can even visit the admin site

    http://127.0.0.1:8000/admin/auth/user/

## Refs

https://github.com/microsoft/vscode-dev-containers/tree/main/containers/python-3-postgres

https://docs.djangoproject.com/en/4.0/ref/settings/#databases

https://docs.djangoproject.com/en/4.0/intro/tutorial01/

