# Python MCP Server Implementation Guide

## Overview

This document provides Python-specific best practices and examples for implementing MCP servers using the MCP Python SDK and FastMCP.

---

## Quick Reference

### Key Imports
```python
from mcp.server.fastmcp import FastMCP
from pydantic import BaseModel, Field, field_validator, ConfigDict
from typing import Optional, List, Dict, Any
from enum import Enum
import httpx
```

### Server Initialization
```python
mcp = FastMCP("service_mcp")
```

### Tool Registration Pattern
```python
@mcp.tool(name="tool_name", annotations={...})
async def tool_function(params: InputModel) -> str:
    # Implementation
    pass
```

---

## Server Naming Convention

Python MCP servers must follow this naming pattern:
- **Format**: `{service}_mcp` (lowercase with underscores)
- **Examples**: `github_mcp`, `jira_mcp`, `stripe_mcp`

## Tool Implementation

### Tool Structure with FastMCP

```python
class ServiceToolInput(BaseModel):
    model_config = ConfigDict(
        str_strip_whitespace=True,
        validate_assignment=True,
        extra='forbid'
    )

    param1: str = Field(..., description="First parameter", min_length=1)
    param2: Optional[int] = Field(default=None, ge=0, le=1000)

@mcp.tool(
    name="service_tool_name",
    annotations={
        "title": "Human-Readable Title",
        "readOnlyHint": True,
        "destructiveHint": False,
        "idempotentHint": True,
        "openWorldHint": False
    }
)
async def service_tool_name(params: ServiceToolInput) -> str:
    '''Tool description becomes the 'description' field.'''
    pass
```

## Pydantic v2 Key Features

- Use `model_config` instead of nested `Config` class
- Use `field_validator` instead of deprecated `validator`
- Use `model_dump()` instead of deprecated `dict()`
- Validators require `@classmethod` decorator

```python
class CreateUserInput(BaseModel):
    model_config = ConfigDict(str_strip_whitespace=True)

    name: str = Field(..., min_length=1, max_length=100)
    email: str = Field(..., pattern=r'^[\w\.-]+@[\w\.-]+\.\w+$')

    @field_validator('email')
    @classmethod
    def validate_email(cls, v: str) -> str:
        return v.lower()
```

## Response Format Options

```python
class ResponseFormat(str, Enum):
    MARKDOWN = "markdown"
    JSON = "json"

class SearchInput(BaseModel):
    query: str = Field(...)
    response_format: ResponseFormat = Field(default=ResponseFormat.MARKDOWN)
```

## Error Handling

```python
def _handle_api_error(e: Exception) -> str:
    if isinstance(e, httpx.HTTPStatusError):
        if e.response.status_code == 404:
            return "Error: Resource not found."
        elif e.response.status_code == 429:
            return "Error: Rate limit exceeded."
    return f"Error: {type(e).__name__}"
```

## Running the Server

```python
if __name__ == "__main__":
    mcp.run()  # stdio transport (default)
    # Or for HTTP:
    # mcp.run(transport="streamable_http", port=8000)
```

## Quality Checklist

- [ ] All tools implement 'name' and 'annotations' in decorator
- [ ] Annotations correctly set (readOnlyHint, destructiveHint, etc.)
- [ ] All tools use Pydantic BaseModel for input validation
- [ ] All Fields have explicit types and descriptions with constraints
- [ ] Comprehensive docstrings with input/output types
- [ ] All async functions properly defined with `async def`
- [ ] Type hints used throughout the code
