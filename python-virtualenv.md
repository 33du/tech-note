in dir for virtualenv create a new venv called myvenv:
```
python3 -m venv myvenv
```

start:
```
source myvenv/bin/activate
```

quit:
```
deactivate
```


repeatable installation:
```
pip freeze > requirements.txt
pip install -r requirements.txt
```
