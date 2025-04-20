---
title: "FastAPI: A Quick Guide"
date: 2024-01-04T13:23:57+05:30
draft: false
---

```Python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def root():
    return {"message": "Hello World"}
```

As FastAPI supports ASGI (Asyschronous Server Gateway Interface), the function root can also be an async function. Use async optionally with await  when the function does not need to wait for steps in the function to be completed.

Run the live server: `uvicorn main:app --reload` 

- `main`: the file `main.py` (the Python "module").
- `app`: the object created inside of `main.py` with the line `app = FastAPI()`.
- `--reload`: make the server restart after code changes. Only use for development.

This command will also ouput where the app is being served

`INFO: Uvicorn running on [http://127.0.0.1:8000](http://127.0.0.1:8000/) (Press CTRL+C to quit)` 

Open this ([`http://127.0.0.1:8000`](http://127.0.0.1:8000/)) on your browser to view the app.

## Interactive API Docs

Automatic documentation created using swagger will be generated and served at [`http://127.0.0.1:8000/docs`](http://127.0.0.1:8000/docs) . Alternate documentation is provided by redoc and served at [`http://127.0.0.1:8000/redoc`](http://127.0.0.1:8000/redoc) 

FastAPI will generate the OpenAPI schema of all the APIs at [`http://127.0.0.1:8000/openapi.json`](http://127.0.0.1:8000/openapi.json)

## Path Parameters
```Python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id):
    return {"item_id": item_id}
```

### Path Parameters with Types
```Python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

In additon to the typing support in the editor, this does automatic data validation.

All the data validation is performed under the hood by the Pydantic library.

For example, [`http://127.0.0.1:8000/items/3`](http://127.0.0.1:8000/items/3) will return `{"item_id":3}` .

Calling [`http://127.0.0.1:8000/items/foo`](http://127.0.0.1:8000/items/foo)  will return

```JSON
{
  "detail": [
    {
      "type": "int_parsing",
      "loc": [
        "path",
        "item_id"
      ],
      "msg": "Input should be a valid integer, unable to parse string as an integer",
      "input": "foo",
      "url": "https://errors.pydantic.dev/2.1/v/int_parsing"
    }
  ]
}
```