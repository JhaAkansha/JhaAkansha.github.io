---
title: "FastAPI: A Quick Guide"
date: 2024-11-04T13:23:57+05:30
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

Here, `q` is the query parameter while others are body keys.

<aside>
💡 Body keys and body parameters refer to the same thing. For example, in `{"item_id": 1, "user_id": 2}`, `item_id` is a body key/parameter.

</aside>

## Embed a single body parameter

Let's say you only have a single item body parameter from a Pydantic model Item. By default, FastAPI will then expect its body directly. But if you want it to expect a JSON with a key item and inside of it the model contents, as it does when you declare extra body parameters, you can use the special Body parameter embed:

```python
from typing import Union

from fastapi import Body, FastAPI
from pydantic import BaseModel
from typing_extensions import Annotated

app = FastAPI()

class Item(BaseModel):
    name: str
    description: Union[str, None] = None
    price: float
    tax: Union[float, None] = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results
```

In this case FastAPI will expect a body like:

```python
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    }
}
```

## Fields

The same way you can declare additional validation and metadata in *path operation function* parameters with `Query`, `Path` and `Body`, you can declare validation and metadata inside of Pydantic models using Pydantic's `Field`.

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel, Field

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str | None = Field(
        default=None, title="The description of the item", max_length=300
    )
    price: float = Field(gt=0, description="The price must be greater than zero")
    tax: float | None = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results
```

`Field` works the same way as `Query`, `Path` and `Body`, it has all the same parameters, etc.

## **Nested Models**

### **List fields**

In Python 3.9 and above you can use the standard `list` to declare these type annotations as we'll see below. 

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list[str] = []

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

### Submodel

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Image(BaseModel):
    url: str
    name: str

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    image: Image | None = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

This would mean that FastAPI would expect a body similar to:

```python
{
	"name": "Foo",
	"description": "The pretender",
	"price": 42.0,
	"tax": 3.2,
	"tags": ["rock", "metal", "bar"],
	"image": {
		"url": "http://example.com/baz.jpg",
		"name": "The Foo live"
		}
}
```

### Special types and validation

Apart from normal singular types like `str`, `int`, `float`, etc. you can use more complex singular types that inherit from `str`.To see all the options you have, checkout the docs for [Pydantic's exotic types](https://docs.pydantic.dev/latest/concepts/types/). For example, as in the `Image` model we have a `url` field, we can declare it to be an instance of Pydantic's `HttpUrl` instead of a `str`:

```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()

class Image(BaseModel):
    url: HttpUrl
    name: str

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    image: Image | None = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

You can also use Pydantic models as subtypes of `list`, `set`, etc.:

```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()

class Image(BaseModel):
    url: HttpUrl
    name: str

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    images: list[Image] | None = None

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

This will expect (convert, validate, document, etc.) a JSON body like:

```python
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2,
    "tags": [
        "rock",
        "metal",
        "bar"
    ],
    "images": [
        {
            "url": "http://example.com/baz.jpg",
            "name": "The Foo live"
        },
        {
            "url": "http://example.com/dave.jpg",
            "name": "The Baz"
        }
    ]
}
```

## **Bodies of arbitrary `dict`**

You can also declare a body as a `dict` with keys of some type and values of some other type.This way, you don't have to know beforehand what the valid field/attribute names are (as would be the case with Pydantic models).This would be useful if you want to receive keys that you don't already know.

```python
from fastapi import FastAPI

app = FastAPI()

@app.post("/index-weights/")
async def create_index_weights(weights: dict[int, float]):
    return weights
```

# **Extra Data Types**

Up to now, you have been using common data types, like:

- `int`
- `float`
- `str`
- `bool`

But you can also use more complex data types.

And you will still have the same features as seen up to now:

- Great editor support.
- Data conversion from incoming requests.
- Data conversion for response data.
- Data validation.
- Automatic annotation and documentation.

Here are some of the additional data types you can use:

- `UUID`:
    - A standard "Universally Unique Identifier", common as an ID in many databases and systems.
    - In requests and responses will be represented as a `str`.
- `datetime.datetime`:
    - A Python `datetime.datetime`.
    - In requests and responses will be represented as a `str` in ISO 8601 format, like: `2008-09-15T15:53:00+05:00`.
- `datetime.date`:
    - Python `datetime.date`.
    - In requests and responses will be represented as a `str` in ISO 8601 format, like: `2008-09-15`.
- `datetime.time`:
    - A Python `datetime.time`.
    - In requests and responses will be represented as a `str` in ISO 8601 format, like: `14:23:55.003`.
- `datetime.timedelta`:
    - A Python `datetime.timedelta`.
    - In requests and responses will be represented as a `float` of total seconds.
    - Pydantic also allows representing it as a "ISO 8601 time diff encoding", [see the docs for more info](https://docs.pydantic.dev/latest/concepts/serialization/#json_encoders).
- `frozenset`:
    - In requests and responses, treated the same as a `set`:
        - In requests, a list will be read, eliminating duplicates and converting it to a `set`.
        - In responses, the `set` will be converted to a `list`.
        - The generated schema will specify that the `set` values are unique (using JSON Schema's `uniqueItems`).
- `bytes`:
    - Standard Python `bytes`.
    - In requests and responses will be treated as `str`.
    - The generated schema will specify that it's a `str` with `binary` "format".
- `Decimal`:
    - Standard Python `Decimal`.
    - In requests and responses, handled the same as a `float`.
- You can check all the valid pydantic data types here: [Pydantic data types](https://docs.pydantic.dev/latest/usage/types/types/).

# Cookie Parameters

You can define Cookie parameters the same way you define `Query` and `Path` parameters.

```python
from typing import Annotated

from fastapi import Cookie, FastAPI

app = FastAPI()

@app.get("/items/")
async def read_items(ads_id: Annotated[str | None, Cookie()] = None):
    return {"ads_id": ads_id}
```

# Header Parameters

You can define Header parameters the same way you define `Query`, `Path` and `Cookie` parameters.

```python
from typing import Annotated

from fastapi import FastAPI, Header

app = FastAPI()

@app.get("/items/")
async def read_items(user_agent: Annotated[str | None, Header()] = None):
    return {"User-Agent": user_agent}
```

`Header` has a little extra functionality on top of what `Path`, `Query` and `Cookie` provide. Most of the standard headers are separated by a "hyphen" character, also known as the "minus symbol" (`-`). But a variable like `user-agent` is invalid in Python. So, by default, `Header` will convert the parameter names characters from underscore (`_`) to hyphen (`-`) to extract and document the headers.

Also, HTTP headers are case-insensitive, so, you can declare them with standard Python style (also known as "snake_case"). So, you can use `user_agent` as you normally would in Python code, instead of needing to capitalize the first letters as `User_Agent` or something similar.

If for some reason you need to disable automatic conversion of underscores to hyphens, set the parameter `convert_underscores` of `Header` to `False`: `Header(convert_underscores=False)]`

## Duplicate headers

It is possible to receive duplicate headers. That means, the same header with multiple values. You can define those cases using a list in the type declaration.

You will receive all the values from the duplicate header as a Python `list`. For example, to declare a header of `X-Token` that can appear more than once, you can write:

```python
from typing import Annotated

from fastapi import FastAPI, Header

app = FastAPI()

@app.get("/items/")
async def read_items(x_token: Annotated[list[str] | None, Header()] = None):
    return {"X-Token values": x_token}
```

If you communicate with that *path operation* sending two HTTP headers like:

```python
X-Token: foo
X-Token: bar
```

The response would be like:

```python
{
    "X-Token values": [
        "bar",
        "foo"
    ]
}
```

# Response

You can declare the type used for the response by annotating the path operation function return type.

You can use type annotations the same way you would for input data in function parameters, you can use Pydantic models, lists, dictionaries, scalar values like integers, booleans, etc.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list[str] = []

@app.post("/items/")
async def create_item(item: Item) -> Item:
    return item

@app.get("/items/")
async def read_items() -> list[Item]:
    return [
        Item(name="Portal Gun", price=42.0),
        Item(name="Plumbus", price=32.0),
    ]
```

FastAPI will use this return type to:

- Validate the returned data.
    - If the data is invalid (e.g. you are missing a field), it means that *your* app code is broken, not returning what it should, and it will return a server error instead of returning incorrect data. This way you and your clients can be certain that they will receive the data and the data shape expected.
- Add a JSON Schema for the response, in the OpenAPI *path operation*.
    - This will be used by the automatic docs.
    - It will also be used by automatic client code generation tools.

But most importantly:

- It will limit and filter the output data to what is defined in the return type.
    - This is particularly important for **security**, we'll see more of that below.

## `response_model` Parameter

There are some cases where you need or want to return some data that is not exactly what the type declares.

For example, you could want to return a dictionary or a database object, but declare it as a Pydantic model. This way the Pydantic model would do all the data documentation, validation, etc. for the object that you returned (e.g. a dictionary or database object). If you added the return type annotation, tools and editors would 
complain with a (correct) error telling you that your function is returning a type (e.g. a dict) that is different from what you declared (e.g. a Pydantic model). In those cases, you can use the *path operation decorator* parameter `response_model` instead of the return type.

```python
from typing import Any

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list[str] = []

@app.post("/items/", response_model=Item)
async def create_item(item: Item) -> Any:
    return item

@app.get("/items/", response_model=list[Item])
async def read_items() -> Any:
    return [
        {"name": "Portal Gun", "price": 42.0},
        {"name": "Plumbus", "price": 32.0},
    ]
```

If you declare both a return type and a `response_model`, the `response_model` will take priority and be used by FastAPI. Some other things response models can do:

- You can disable the response model generation by setting `response_model=None`. This will make FastAPI skip the response model generation and that way you can have any return type annotations you need without it affecting your FastAPI application.
- You can omit optional attributes having default values from the response by setting path operation decorator parameter `response_model_exclude_unset=True`. For example, `@app.post("/items/{item_id}", response_model=Item, response_model_exclude_unset=True)`
- You can also use the path operation decorator parameters `response_model_include` and `response_model_exclude`. They take a set of str with the name of the attributes to include (omitting the rest) or to exclude (including the rest). This can be used as a quick shortcut if you have only one Pydantic model and want to remove some data from the output. For example: `@app.post("/items", response_model=Item, response_model_include={"name", "description"}, response_model_exclude={"tax"})`

## **Data Filtering**

We want to annotate the function with one type but return something that includes more data. We want FastAPI to keep filtering the data using the response model.

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()

class BaseUser(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None

class UserIn(BaseUser):
    password: str

@app.post("/user/")
async def create_user(user: UserIn) -> BaseUser:
    return user
```

## **Return a Response Directly**

```python
from fastapi import FastAPI, Response
from fastapi.responses import JSONResponse, RedirectResponse

app = FastAPI()

@app.get("/portal")
async def get_portal(teleport: bool = False) -> Response:
    if teleport:
        return RedirectResponse(url="https://www.youtube.com/watch?v=dQw4w9WgXcQ")
    return JSONResponse(content={"message": "Here's your interdimensional portal."})
```

## Extra Models

It is common to have more than one related model. This is especially the case for user models, because:

- The input model needs to be able to have a password.
- The output model should not have a password.
- The database model would probably need to have a hashed password.

This is is an example of how they are used:

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()

class UserBase(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None

class UserIn(UserBase):
    password: str

class UserOut(UserBase):
    pass

class UserInDB(UserBase):
    hashed_password: str

def fake_password_hasher(raw_password: str):
    return "supersecret" + raw_password

def fake_save_user(user_in: UserIn):
    hashed_password = fake_password_hasher(user_in.password)
    user_in_db = UserInDB(**user_in.dict(), hashed_password=hashed_password)
    print("User saved! ..not really")
    return user_in_db

@app.post("/user/", response_model=UserOut)
async def create_user(user_in: UserIn):
    user_saved = fake_save_user(user_in)
    return user_saved
```

To note:

- To create a dictionary from a Pydantic model, use the `.dict()` method that returns a `dict` with the model's data. For example, `UserInDB(**user_dict)`
- To create a Pydantic model from a `dict`, call the constructor with the `dict`. For example: `UserInDB(**user_dict)`. This is caled unwrapping a dict.
- Convert from one Pydantic model to another using
    - `user_dict = user_in.dict(); UserInDB(**user_dict)`
    - `UserInDB(**user_in.dict())`
    - `UserInDB(**user_in.dict(), hashed_password=hashed_password)` (to add an extra attribute `hashed_password`)

## Other details

### Union or AnyOf

You can declare a response to be the `Union` of two types, that means, that the response would be any of the two.

```python
from typing import Union

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class BaseItem(BaseModel):
    description: str
    type: str

class CarItem(BaseItem):
    type: str = "car"

class PlaneItem(BaseItem):
    type: str = "plane"
    size: int

items = {
    "item1": {"description": "All my friends drive a low rider", "type": "car"},
    "item2": {
        "description": "Music is my aeroplane, it's my aeroplane",
        "type": "plane",
        "size": 5,
    },
}

@app.get("/items/{item_id}", response_model=Union[PlaneItem, CarItem])
async def read_item(item_id: str):
    return items[item_id]
```

In this example we pass `Union[PlaneItem, CarItem]` as the value of the argument `response_model`. Because we are passing it as a value to an argument instead of putting it in a type annotation, we have to use Union even in Python 3.10.

### List of models

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str

items = [
    {"name": "Foo", "description": "There comes my hero"},
    {"name": "Red", "description": "It's my aeroplane"},
]

@app.get("/items/", response_model=list[Item])
async def read_items():
    return items
```

### Response with arbitrary `dict`

You can also declare a response using a plain arbitrary `dict`, declaring just the type of the keys and values, without using a Pydantic model. This is useful if you don't know the valid field/attribute names (that would be needed for a Pydantic model) beforehand.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/keyword-weights/", response_model=dict[str, float])
async def read_keyword_weights():
    return {"foo": 2.3, "bar": 3.4}
```

## Response Codes

In HTTP, you send a numeric status code of 3 digits as part of the response. These status codes have a name associated to recognize them, but the important part is the number.

- `100` and above are for "Information". You rarely use them directly. Responses with these status codes cannot have a body.
- **`200`** and above are for "Successful" responses. These are the ones you would use the most.
    - `200` is the default status code, which means everything was "OK".
    - Another example would be `201`, "Created". It is commonly used after creating a new record in the database.
    - A special case is `204`, "No Content". This response is
    used when there is no content to return to the client, and so the
    response must not have a body.
- **`300`** and above are for "Redirection". Responses with these status codes may or may not have a body, except for `304`, "Not Modified", which must not have one.
- **`400`** and above are for "Client error" responses. These are the second type you would probably use the most.
    - An example is `404`, for a "Not Found" response.
    - For generic errors from the client, you can just use `400`.
- `500` and above are for server errors. You almost never
use them directly. When something goes wrong at some part in your
application code, or server, it will automatically return one of these
status codes.

```python
from fastapi import FastAPI, status

app = FastAPI()

@app.post("/items/", status_code=status.HTTP_201_CREATED)
async def create_item(name: str):
    return {"name": name}
```

## Form Data

When you need to receive form fields instead of JSON, you can use `Form`. With `Form` you can declare the same configurations as with `Body` (and `Query`, `Path`, `Cookie`), including validation, examples, an alias (e.g. `user-name` instead of `username`), etc. The way HTML forms (`<form></form>`) sends the data to the server normally uses a "special" encoding for that data, it's different from JSON. FastAPI will make sure to read that data from the right place instead of JSON.

```python
from typing import Annotated

from fastapi import FastAPI, Form

app = FastAPI()

@app.post("/login/")
async def login(username: Annotated[str, Form()], password: Annotated[str, Form()]):
    return {"username": username}
```

# **Request Files**

You can define files to be uploaded by the client using `File`

<aside>
💡 To receive uploaded files, first install [`python-multipart`](https://github.com/Kludex/python-multipart)

</aside>

```python
from typing import Annotated

from fastapi import FastAPI, File, UploadFile

app = FastAPI()

@app.post("/files/")
async def create_file(file: Annotated[bytes, File()]):
    return {"file_size": len(file)}

@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile):
    return {"filename": file.filename}
```

The files will be uploaded as "form data".

If you declare the type of your path operation function parameter as bytes, FastAPI will read the file for you and you will receive the contents as bytes.

Keep in mind that this means that the whole contents will be stored in memory. This will work well for small files.

Using `UploadFile` has several advantages over `bytes`:

- You don't have to use `File()` in the default value of the parameter.
- It uses a "spooled" file:
    - A file stored in memory up to a maximum size limit, and after passing this limit it will be stored in disk.
- This means that it will work well for large files like images, videos, large binaries, etc. without consuming all the memory.
- You can get metadata from the uploaded file.
- It has a [file-like](https://docs.python.org/3/glossary.html#term-file-like-object) `async` interface.
- It exposes an actual Python [`SpooledTemporaryFile`](https://docs.python.org/3/library/tempfile.html#tempfile.SpooledTemporaryFile) object that you can pass directly to other libraries that expect a file-like object.

`UploadFile` has the following attributes:

- `filename`: A `str` with the original file name that was uploaded (e.g. `myimage.jpg`).
- `content_type`: A `str` with the content type (MIME type / media type) (e.g. `image/jpeg`).
- `file`: A [`SpooledTemporaryFile`](https://docs.python.org/3/library/tempfile.html#tempfile.SpooledTemporaryFile) (a [file-like](https://docs.python.org/3/glossary.html#term-file-like-object) object). This is the actual Python file that you can pass directly to
other functions or libraries that expect a "file-like" object.

`UploadFile` has the following `async` methods. They all call the corresponding file methods underneath (using the internal `SpooledTemporaryFile`).

- `write(data)`: Writes `data` (`str` or `bytes`) to the file.
- `read(size)`: Reads `size` (`int`) bytes/characters of the file.
- `seek(offset)`: Goes to the byte position `offset` (`int`) in the file.
    - E.g., `await myfile.seek(0)` would go to the start of the file.
    - This is especially useful if you run `await myfile.read()` once and then need to read the contents again.
- `close()`: Closes the file.

As all these methods are `async` methods, you need to "await" them.

For example, inside of an `async` *path operation function* you can get the contents with `contents = await myfile.read()`

If you are inside of a normal `def` *path operation function*, you can access the `UploadFile.file` directly, for example `contents = myfile.file.read()`

To note:

- You can make a file optional by using standard type annotations and setting a default value of `None`. For example, function definition from above will change to `async def create_upload_file(file: UploadFile | None = None):`
- You can also use `File()` with `UploadFile`, for example, to set additional metadata. For example: `async def create_upload_file(
file: Annotated[UploadFile, File(description="A file read as UploadFile")]):`
- It's possible to upload several files at the same time. To use that, declare a list of `bytes` or `UploadFile`. For example, `async def create_upload_files(files: list[UploadFile]):`

# **Handling Errors**

There are many situations in which you need to notify an error to a client that is using your API. This client could be a browser with a frontend, a code from someone else, an IoT device, etc. You could need to tell the client that:

- The client doesn't have enough privileges for that operation.
- The client doesn't have access to that resource.
- The item the client was trying to access doesn't exist, etc.

## HTTPException

Raise an `HTTPException` in your code. 

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

items = {"foo": "The Foo Wrestlers"}

@app.get("/items/{item_id}")
async def read_item(item_id: str):
    if item_id not in items:
        raise HTTPException(status_code=404, detail="Item not found")
    return {"item": items[item_id]}
```

If the client requests `http://example.com/items/foo` (an `item_id` `"foo"`), that client will receive an HTTP status code of 200, and a JSON response of:

```python
{
  "item": "The Foo Wrestlers"
}
```

But if the client requests `http://example.com/items/bar` (a non-existent `item_id` `"bar"`), that client will receive an HTTP status code of 404 (the "not found" error), and a JSON response of:

```python
{
  "detail": "Item not found"
}
```

<aside>
💡

When raising an `HTTPException`, you can pass any value that can be converted to JSON as the parameter `detail`, not only `str`. You could pass a `dict`, a `list`, etc.

They are handled automatically by FastAPI and converted to JSON.

</aside>

## Add custom headers

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

items = {"foo": "The Foo Wrestlers"}

@app.get("/items-header/{item_id}")
async def read_item_header(item_id: str):
    if item_id not in items:
        raise HTTPException(
            status_code=404,
            detail="Item not found",
            headers={"X-Error": "There goes my error"},
        )
    return {"item": items[item_id]}
```

## **Install custom exception handlers**

Let's say you have a custom exception `UnicornException` that you (or a library you use) might `raise`. And you want to handle this exception globally with FastAPI. You could add a custom exception handler with `@app.exception_handler()`:

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

class UnicornException(Exception):
    def __init__(self, name: str):
        self.name = name

app = FastAPI()

@app.exception_handler(UnicornException)
async def unicorn_exception_handler(request: Request, exc: UnicornException):
    return JSONResponse(
        status_code=418,
        content={"message": f"Oops! {exc.name} did something. There goes a rainbow..."},
    )

@app.get("/unicorns/{name}")
async def read_unicorn(name: str):
    if name == "yolo":
        raise UnicornException(name=name)
    return {"unicorn_name": name}
```

Here, if you request `/unicorns/yolo`, the *path operation* will `raise` a `UnicornException`. But it will be handled by the `unicorn_exception_handler`. So, you will receive a clean error, with an HTTP status code of `418` and a JSON content of: `{"message": "Oops! yolo did something. There goes a rainbow..."}`

## **Override the default exception handlers**

FastAPI has some default exception handlers. These handlers are in charge of returning the default JSON responses when you `raise` an `HTTPException` and when the request has invalid data.

When a request contains invalid data, FastAPI internally raises a `RequestValidationError`. And it also includes a default exception handler for it. To override it, import the `RequestValidationError` and use it with `@app.exception_handler(RequestValidationError)` to decorate the exception handler. The exception handler will receive a `Request` and the exception.

```python
from fastapi import FastAPI, HTTPException
from fastapi.exceptions import RequestValidationError
from fastapi.responses import PlainTextResponse
from starlette.exceptions import HTTPException as StarletteHTTPException

app = FastAPI()

@app.exception_handler(StarletteHTTPException)
async def http_exception_handler(request, exc):
    return PlainTextResponse(str(exc.detail), status_code=exc.status_code)

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc):
    return PlainTextResponse(str(exc), status_code=400)

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    if item_id == 3:
        raise HTTPException(status_code=418, detail="Nope! I don't like 3.")
    return {"item_id": item_id}
```

Now, if you go to `/items/foo`, instead of getting the default JSON error with:

```python
{
    "detail": [
        {
            "loc": [
                "path",
                "item_id"
            ],
            "msg": "value is not a valid integer",
            "type": "type_error.integer"
        }
    ]
}
```

you will get a text version, with:

```python
1 validation error
path -> item_id
  value is not a valid integer (type=type_error.integer)
```

The same way, you can override the `HTTPException` handler. For example, you could want to return a plain text response instead of JSON for these errors:

```python
from fastapi import FastAPI, HTTPException
from fastapi.exceptions import RequestValidationError
from fastapi.responses import PlainTextResponse
from starlette.exceptions import HTTPException as StarletteHTTPException

app = FastAPI()

@app.exception_handler(StarletteHTTPException)
async def http_exception_handler(request, exc):
    return PlainTextResponse(str(exc.detail), status_code=exc.status_code)

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc):
    return PlainTextResponse(str(exc), status_code=400)

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    if item_id == 3:
        raise HTTPException(status_code=418, detail="Nope! I don't like 3.")
    return {"item_id": item_id}
```

### FastAPI's HTTPException vs Starlette's HTTPException

And FastAPI's `HTTPException` error class inherits from Starlette's `HTTPException` error class. The only difference is that FastAPI's `HTTPException` accepts any JSON-able data for the `detail` field, while Starlette's `HTTPException` only accepts strings for it.

So, you can keep raising FastAPI's `HTTPException` as normally in your code. But when you register an exception handler, you should register it for Starlette's `HTTPException`. This way, if any part of Starlette's internal code, or a Starlette extension or plug-in, raises a Starlette `HTTPException`, your handler will be able to catch and handle it.