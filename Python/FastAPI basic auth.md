```python
import secrets
from fastapi import status, Depends, HTTPException
from fastapi.security import HTTPBasic, HTTPBasicCredentials
from typing import Annotated
from vars import auth_username, auth_password

security = HTTPBasic()

async def http_basic_auth_validator(credentials: Annotated[HTTPBasicCredentials, Depends(security)]):
    current_username_bytes = credentials.username.encode("utf8")
    correct_username_bytes = auth_username.encode("utf8")
    is_correct_username = secrets.compare_digest(
        current_username_bytes, correct_username_bytes
    )
    current_password_bytes = credentials.password.encode("utf8")
    correct_password_bytes = auth_password.encode("utf8")
    is_correct_password = secrets.compare_digest(
        current_password_bytes, correct_password_bytes
    )
    if not (is_correct_username and is_correct_password):
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
            headers={"WWW-Authenticate": "Basic"},
        )
    return credentials.username
```

## Example of the basic auth being used
```python
@app.get("/vacation")
async def set_vacation(username: Annotated[str, Depends(http_basic_auth_validator)]):
    global vacation
    def get_vacation_response(message):
        html_content = f"""
        <!DOCTYPE html>
        <html>
            <head>
                <title>Semesterläge</title>
                <style>
                    body {{
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        height: 100vh;
                        margin: 0;
                        font-family: Arial, sans-serif;
                        background-color: #f0f8ff;
                    }}
                    h1 {{
                        font-size: 3rem;
                        color: #172554;
                    }}
                </style>
            </head>
            <body>
                <h1>{message}</h1>
            </body>
        </html>
        """
        return html_content
    if not vacation:
        vacation = True
        return HTMLResponse(content=get_vacation_response("Semesterläge är på!"), status_code=200)
    else:
        vacation = False
        return HTMLResponse(content=get_vacation_response("Semesterläge är av!"), status_code=200)
```