# Flask Tutorial

## Purpose
This Docker Flask tutorial is intended to show students on how to start. I will use this in my own class rooms. Feel free to use it at your own risk.

## Docker File

```Bash
FROM python:3
WORKDIR /app
# We copy just the requirements.txt first to leverage Docker cache
COPY ./requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
ENTRYPOINT [ "python" ]
CMD [ "app.py" ]
```
## Requirement.txt File
```Bash
Flask>=1.0.0
```

## App.py File (Hello World)
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Flask: Hello World from Docker'

@app.route('/api')
def rest_hello_world():
    return '{"id":1,"message":"Flask: Hello World from Docker"}'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```
