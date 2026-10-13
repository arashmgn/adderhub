# Deploy AdderHub on PythonAnywhere (arashmgn76)

Site URL: https://arashmgn76.pythonanywhere.com

## 1. Bash console

```bash
git clone https://github.com/codesbygom/adderhub.git
cd adderhub
mkvirtualenv --python=/usr/bin/python3.13 adderhub-env
pip install -r requirements.txt
cd backend
python manage.py migrate
python manage.py collectstatic --noinput
python manage.py createsuperuser
python -c "import secrets; print(secrets.token_urlsafe(50))"
```

Copy the last output: it is the `DJANGO_SECRET_KEY`.

## 2. Web tab

1. Add a new web app -> Manual configuration -> Python 3.13
2. Virtualenv: `/home/arashmgn76/.virtualenvs/adderhub-env`
3. Static files:
   - `/static/` -> `/home/arashmgn76/adderhub/backend/staticfiles`
   - `/media/`  -> `/home/arashmgn76/adderhub/backend/media`
4. Replace the WSGI file with the code below.
5. Press Reload.

## 3. WSGI file

```python
import os
import sys

path = '/home/arashmgn76/adderhub/backend'
if path not in sys.path:
    sys.path.insert(0, path)

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')
os.environ['DJANGO_SECRET_KEY'] = 'PASTE-THE-GENERATED-KEY-HERE'
os.environ['DJANGO_DEBUG'] = 'False'
os.environ['DJANGO_ALLOWED_HOSTS'] = 'arashmgn76.pythonanywhere.com'
os.environ['CACHE_BACKEND'] = 'file'
os.environ['CACHE_DIR'] = '/home/arashmgn76/adderhub/backend/.cache'

from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

## 4. Updating later

```bash
cd ~/adderhub && git pull
cd backend && python manage.py migrate && python manage.py collectstatic --noinput
```

Then press Reload on the Web tab.

## Troubleshooting

Read the Error log linked on the Web tab.
