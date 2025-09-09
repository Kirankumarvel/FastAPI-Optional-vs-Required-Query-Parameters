# FastAPI Optional vs Required Query Parameters

A FastAPI application demonstrating the difference between **optional** and **required** query parameters, with built-in validation and interactive documentation.

---

## 🚀 Features

- FastAPI framework with automatic OpenAPI docs
- Shows **optional** query parameters with defaults
- Shows **required** query parameters (no defaults)
- Automatic validation & error handling
- Interactive API docs at `/docs` and `/redoc`
- Python 3.7+ compatibility
- Virtual environment setup

---

## 📋 Prerequisites

- Python 3.7 or higher
- `pip` (Python package manager)

---

## 🛠️ Installation

1. **Clone the repository**
    ```bash
    git clone https://github.com/Kirankumarvel/fastapi-optional-required-params.git
    cd fastapi-optional-required-params
    ```

2. **Create a virtual environment**
    ```bash
    python -m venv venv
    ```

3. **Activate the virtual environment**

    - **Windows:**
      ```bash
      venv\Scripts\activate
      ```
    - **macOS/Linux:**
      ```bash
      source venv/bin/activate
      ```

4. **Install dependencies**
    ```bash
    pip install -r requirements.txt
    ```

---

## 📦 Dependencies

- `fastapi` – Web framework for building APIs
- `uvicorn` – ASGI server for running FastAPI

To generate `requirements.txt`:
```bash
pip freeze > requirements.txt
```

---

## 🚀 Running the Application

Start the development server:
```bash
uvicorn main:app --reload --reload-exclude venv
```

**Access:**
- API: [http://127.0.0.1:8000](http://127.0.0.1:8000)
- Interactive docs: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- Alternative docs: [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

---

## 📡 API Endpoints

### `GET /items/` (Optional Parameters)

Returns a paginated list of items with **optional** `skip` and `limit` query parameters.

| Parameter | Type | Required | Default | Description                 |
|-----------|------|----------|---------|-----------------------------|
| skip      | int  | No       | 0       | Number of items to skip     |
| limit     | int  | No       | 10      | Max number of items to show |

**Examples:**

```bash
# All parameters optional – uses defaults
curl "http://127.0.0.1:8000/items/"

# Partial parameters
curl "http://127.0.0.1:8000/items/?skip=2"
curl "http://127.0.0.1:8000/items/?limit=5"

# All parameters specified
curl "http://127.0.0.1:8000/items/?skip=1&limit=2"
```

---

### `GET /search/` (Required Parameter)

Performs a search with a **required** query parameter.

| Parameter | Type | Required | Default | Description         |
|-----------|------|----------|---------|---------------------|
| q         | str  | Yes      | —       | Search query term   |

**Examples:**

```bash
# Valid request with required parameter
curl "http://127.0.0.1:8000/search/?q=fastapi"

# Invalid request – missing required parameter
curl "http://127.0.0.1:8000/search/"
```

---

## 🎯 Key Concept: Optional vs Required Parameters

| Type      | Syntax                       | Example           | Behavior                | Use Case                |
|-----------|------------------------------|-------------------|-------------------------|-------------------------|
| Optional  | `parameter: type = default`  | `skip: int = 0`   | Can be omitted          | Pagination, filtering   |
| Required  | `parameter: type`            | `q: str`          | Must be provided        | Essential data (search) |

---

### Automatic Validation

FastAPI automatically:

- Validates parameter types
- Returns descriptive errors
- Documents all params in OpenAPI schema
- Handles missing **required** parameters gracefully

---

## 🧪 Testing the API

**Testing Optional Parameters:**
```bash
# All defaults
curl "http://127.0.0.1:8000/items/"

# Custom values
curl "http://127.0.0.1:8000/items/?skip=1&limit=2"
```

**Testing Required Parameters:**
```bash
# Valid request
curl "http://127.0.0.1:8000/search/?q=python"

# Missing required parameter (should show error)
curl "http://127.0.0.1:8000/search/"
```

**Error Response Example:**
```json
{
  "detail": [
    {
      "type": "missing",
      "loc": ["query", "q"],
      "msg": "Field required",
      "input": null
    }
  ]
}
```

---

## 📁 Project Structure

```
fastapi-optional-required-params/
├── main.py           # Main application file
├── requirements.txt  # Project dependencies
├── README.md         # Project documentation
└── venv/             # Virtual environment (gitignored)
```

---

## 🔧 Code Explanation

**main.py**
```python
from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]

@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    """
    Fetch a list of items with pagination.
    - skip: number of items to skip (for pagination) - OPTIONAL
    - limit: max number of items to return - OPTIONAL
    """
    return fake_items_db[skip : skip + limit]

@app.get("/search/")
async def search_items(q: str):  # No default value = REQUIRED!
    """
    Search for items based on a query term.
    - q: search query term - REQUIRED
    """
    results = {"query": q, "results": []}
    # ... some logic to search for items based on 'q'
    return results
```

---

## 🎓 Learning Points

- **Optional Parameters:** Use default values (`= value`)
- **Required Parameters:** Omit default values
- **Automatic Validation:** FastAPI handles type checking
- **Error Messages:** Descriptive validation errors
- **API Design:** When to use optional vs required

---

## 🔧 Troubleshooting

**Common Issues:**

- **Missing required parameter errors:**  
  Ensure you provide all required query parameters

- **Type validation errors:**  
  Check that parameter types match expected types

- **Parameter naming conflicts:**  
  Ensure query parameter names don't conflict with path parameters

- **Reload issues:**  
  ```bash
  uvicorn main:app --reload --reload-exclude venv
  ```

---

## 📚 Learning Resources

- [FastAPI Query Parameters Documentation](https://fastapi.tiangolo.com/tutorial/query-params/)
- [FastAPI Optional Parameters](https://fastapi.tiangolo.com/tutorial/query-params/#optional-parameters)
- [FastAPI Required Parameters](https://fastapi.tiangolo.com/tutorial/query-params/#required-parameters)
- [Uvicorn Documentation](https://www.uvicorn.org/)

---

## 🤝 Contributing

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## 🙏 Acknowledgments

- FastAPI team for the excellent framework and validation system
- Uvicorn team for the ASGI server
- Python community for ongoing support
