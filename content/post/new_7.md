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

## Order of path matters

Because path operations are evaluated in order,make sure that the path for `/users/me` is declared before the one for `/users/{user_id}`.
```Python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users/me")
async def read_user_me():
    return {"user_id": "the current user"}


@app.get("/users/{user_id}")
async def read_user(user_id: str):
    return {"user_id": user_id}
```
Similarly, path operation cannot be redefined. So, if there are two functions for the same path, the first one will be used.

## Predefined values

If you have a path operation that receives a path parameter, but you want the possible valid path parameter values to be predefined, you can use a standard Python Enum.

```Python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```

## Validations

Before reading this section, please go through Query parameters and its validations: [**Query Parameters**](https://www.notion.so/Query-Parameters-ca1f394d48eb40c99ce4477c816cb593?pvs=21) [Validations](https://www.notion.so/Validations-ff394bfd3b934a33a04ca2c5b0653a8b?pvs=21) [Other Details](https://www.notion.so/Other-Details-45f0465912b84a03b9798ad3062ed586?pvs=21) 

In the same way that you can declare more validations and metadata for query parameters with Query, you can declare the same type of validations and metadata for path parameters with `Path`.

```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()

@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(gt=0, le=1000, title="The ID of the item to get")],
    q: Annotated[str | None, Query(alias="item-query")] = None,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

And you can also declare numeric validations:

- `gt`: `g`reater `t`han
- `ge`: `g`reater than or `e`qual
- `lt`: `l`ess `t`han
- `le`: `l`ess than or `e`qual

## Other Details

- Declare more metadata
    
    You can declare all the same parameters as for `Query`.
```Python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get")],
    q: Annotated[str | None, Query(alias="item-query")] = None,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```
