role = user / group, default role: **postgres**

Switch over to it and connect:
```
sudo -i -u postgres
psql
```
or directly excute the command psql:
```
sudo -u postgres psql
```

exit:
```
\q
```

initial commands:
```
CREATE DATABASE myproject;
CREATE USER myprojectuser WITH PASSWORD 'password';

ALTER ROLE myprojectuser SET client_encoding TO 'utf8';
ALTER ROLE myprojectuser SET default_transaction_isolation TO 'read committed';
ALTER ROLE myprojectuser SET timezone TO 'UTC';

GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
```

settings in django:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'blog',
        'USER': 'admin',
        'PASSWORD': '***',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```
