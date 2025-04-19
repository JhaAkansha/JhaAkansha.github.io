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
# **Query Parameters**

The query is the set of key-value pairs that go after the ? in a URL, separated by & characters. For example, in the URL: [`http://127.0.0.1:8000/items/?skip=0&limit=10`](http://127.0.0.1:8000/items/?skip=0&limit=10) 

When you declare other function parameters that are not part of the path parameters, they are automatically interpreted as "query" parameters. As query parameters are not a fixed part of a path, they can be optional and can have default value.

```python
from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]

@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]
```

All the same process that applied for path parameters also applies for query parameters:

- Editor support (obviously)
- Data "parsing"
- Data validation
- Automatic documentation

## **Multiple path and query parameters**

You can declare multiple path parameters and query parameters at the same time, FastAPI knows which is which. And you don't have to declare them in any specific order. They will be detected by name:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/{user_id}/items/{item_id}")
async def read_user_item(
    user_id: int, item_id: str, q: str | None = None, short: bool = False
):
    item = {"item_id": item_id, "owner_id": user_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```

## **Required query parameters**

When you declare a default value for non-path parameters (for now, we have only seen query parameters), then it is not required.

If you don't want to add a specific value but just make it optional, set the default as `None`.

But when you want to make a query parameter required, you can just not declare any default value:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
async def read_user_item(item_id: str, needy: str):
    item = {"item_id": item_id, "needy": needy}
    return item
```

On opening: [`http://127.0.0.1:8000/items/foo-item`](http://127.0.0.1:8000/items/foo-item)  without adding the required parameter `needy`, you will see an error like:

```python
{
  "detail": [
    {
      "type": "missing",
      "loc": [
        "query",
        "needy"
      ],
      "msg": "Field required",
      "input": null,
      "url": "https://errors.pydantic.dev/2.1/v/missing"
    }
  ]
}
```

And of course, you can define some parameters as required, some as having a default value, and some entirely optional. In this case, there are 3 query parameters:

- `needy`, a required `str`.
- `skip`, an `int` with a default value of `0`.
- `limit`, an optional `int`.

```Python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_user_item(
    item_id: str, needy: str, skip: int = 0, limit: int | None = None
):
    item = {"item_id": item_id, "needy": needy, "skip": skip, "limit": limit}
    return item
```

## Validations

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None, Query(min_length=3, max_length=50, pattern="^fixedquery$")
    ] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

The query parameter q is of type str | None, that means that it's of type str but could also be None, and indeed, the default value is None, so FastAPI will know it's not required. `Annotated` can be used to add metadata to your parameters

FastAPI will now:

- Validate the data making sure that the max length is 50 characters
- Validate the data making sure that the min length is 3 characters
- Validate this specific regular expression pattern checks that the received parameter value:
    - `^`: starts with the following characters, doesn't have characters before.
    - `fixedquery`: has the exact value `fixedquery`.
    - `$`: ends there, doesn't have any more characters after `fixedquery`.
- Show a clear error for the client when the data is not valid
- Document the parameter in the OpenAPI schema *path operation* (so it will show up in the automatic docs UI)

If you want the `q` query parameter to have a default value of `fixedquery`, the function decalration in above example will become `sync def read_items(q: Annotated[str, Query(min_length=3)] = "fixedquery"):`

<aside>
💡 Having a default value of any type, including `None`, makes the parameter optional (not required).

</aside>

We can make a query parameter required just by not declaring a default value. To explicitly declare that a value is required. You can set the default to the literal value `…` ,  the function decalration in above example will become  `async def read_items(q: Annotated[str, Query(min_length=3)] = ...):` 

## Other Details

- Query parameter list / multiple values
    
    When you define a query parameter explicitly with `Query` you can also declare it to receive a list of values, or said in other way, to receive multiple values.
    
    For example, to declare a query parameter `q` that can appear multiple times in the URL, you can write:
    
    ```python
    @app.get("/items/")
    async def read_items(q: Annotated[list[str] | None, Query()] = None):
        query_items = {"q": q}
        return query_items
    ```
    
    Then, with a URL like: [`http://localhost:8000/items/?q=foo&q=bar`](http://localhost:8000/items/?q=foo&q=bar)
    
    <aside>
    💡 To declare a query parameter with a type of `list`, like in the example above, you need to explicitly use `Query`, otherwise it would be interpreted as a request body.
    
    </aside>
    
    With defaults, the above function decalartion would be like: `async def read_items(q: Annotated[list[str], Query()] = ["foo", "bar"]):`
    
- Declare more metadata
    
    You can add more information about the parameter. This information will be included in the generated OpenAPI and used by the documentation user interfaces and external tools.
    
    ```python
    @app.get("/items/")
    async def read_items(
        q: Annotated[
            str | None,
            Query(
                alias="item-query",
                title="Query string",
                description="Query string for the items to search in the database that have a good match",
                min_length=3,
                max_length=50,
                pattern="^fixedquery$",
                deprecated=True,
            ),
        ] = None,
    ):
        results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
        if q:
            results.update({"q": q})
        return results
    ```
    
    <aside>
    💡 For URL [`http://127.0.0.1:8000/items/?item-query=foobaritems`](http://127.0.0.1:8000/items/?item-query=foobaritems), query parameter name is `item_query`. This is not a valid Python variable name. Alias is what will be used to find the parameter value. So, `q` will have value `foobaritems`.
    
    </aside>
    
    <aside>
    💡
    
    To exclude a query parameter from the generated OpenAPI schema, use `Query(include_in_schema=False)`
    
    </aside>
    

# **Request Body**

A request body is data sent by the client to your API. A response body is the data your API sends to the client. To send data, you should use one of: `POST`, `PUT`, `DELETE` or `PATCH`.

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.post("/items/")
async def create_item(item: Item):
    item_dict = item.dict()
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict
```

The same as when declaring query parameters, when a model attribute has a default value, it is not required. Otherwise, it is required. Use `None` to make it just optional. So, bith the examples below are valid:

```python
{
    "name": "Foo",
    "description": "An optional description",
    "price": 45.2,
    "tax": 3.5
}
```

```python
{
    "name": "Foo",
    "price": 45.2
}
```

With just that Python type declaration, FastAPI will:

- Read the body of the request as JSON.
- Convert the corresponding types (if needed).
- Validate the data.
    - If the data is invalid, it will return a nice and clear error, indicating exactly where and what was the incorrect data.
- Give you the received data in the parameter `item`.
    - As you declared it in the function to be of type `Item`, you will also have all the editor support (completion, etc) for all of the attributes and their types.
- Generate JSON Schema definitions for your model, you can also use them anywhere else you like if it makes sense for your project.
- Those schemas will be part of the generated OpenAPI schema, and used by the automatic documentation UIs.

## **Request body + path + query parameters**

```python
from fastapi import FastAPI
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

app = FastAPI()

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item, q: str | None = None):
    result = {"item_id": item_id, **item.dict()}
    if q:
        result.update({"q": q})
	    return result
```

The function parameters will be recognized as follows:

- If the parameter is also declared in the path, it will be used as a path parameter.
- If the parameter is of a singular type (like `int`, `float`, `str`, `bool`, etc) it will be interpreted as a query parameter.
- If the parameter is declared to be of the type of a Pydantic model, it will be interpreted as a request body.

## Singular values in body

The same way there is a `Query` and `Path` to define extra data for query and path parameters, FastAPI provides an equivalent `Body`. `Body` also has all the same extra validation and metadata parameters as `Query`,`Path` and others. For example, extending the previous model, you could decide that you want to have another key `importance` in the same body, besides the `item` and `user`. If you declare it as is, because it is a singular value, FastAPI will assume that it is a query parameter. But you can instruct FastAPI to treat it as another body key using `Body`:

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

class User(BaseModel):
    username: str
    full_name: str | None = None

@app.put("/items/{item_id}")
async def update_item(
    item_id: int, item: Item, user: User, importance: Annotated[int, Body()]
):
    results = {"item_id": item_id, "item": item, "user": user, "importance": importance}
    return results
```

In this case, FastAPI will expect a body like:

```python
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    },
    "user": {
        "username": "dave",
        "full_name": "Dave Grohl"
    },
    "importance": 5
}
```

## Multiple body params and query

Of course, you can also declare additional query parameters whenever you need, additional to any body parameters. As, by default, singular values are interpreted as query parameters, you don't have to explicitly add a Query, you can just do:
```Python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(
    *,
    item_id: int,
    item: Item,
    user: User,
    importance: Annotated[int, Body(gt=0)],
    q: str | None = None,
):
    results = {"item_id": item_id, "item": item, "user": user, "importance": importance}
    if q:
        results.update({"q": q})
    return results
```

