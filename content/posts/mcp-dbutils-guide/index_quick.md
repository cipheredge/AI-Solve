---
title: "MCP Database Service: Easily Manage Multiple Databases"
date: 2025-02-09T21:32:00+04:00
slug: 'mcp-dbutils-quick-guide'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250209213924711.webp"
tag:
  - MCP
  - Database
  - Usage
---

If you're like me and frequently need to check databases across different projects, you've probably experienced these frustrations: constantly switching database connections, remembering different connection parameters, and dealing with security concerns. Today, I'll introduce a simple yet powerful tool that makes database queries as natural as having a conversation with AI.

<!--more-->

## Real-world Demonstration

Imagine you need to check databases from two different projects. Using the MCP database service, it works like this:

```
User: Show me the contents of the user table in stage-db

AI Assistant: Let me query that for you.

From the query results, the user table contains basic user information:
1. user_id: Unique identifier for users
2. first_name: User's first name
3. last_name: User's last name
4. email: Email address
5. created_at: Creation timestamp
6. data: Additional data

User: Now check dev-db, it should be from a different project, right?

AI Assistant: Let me look at dev-db.

Indeed, it's a different project. dev-db is a social application database:
- users: User table
- posts: Posts table
- interactions: Interactions table

The two databases are completely different:
1. stage-db is an enterprise portal system focused on user management
2. dev-db is a social platform containing content and interaction features
```

See how simple it is, just like a normal conversation! No need to remember connection parameters or worry about security issues. Just specify what you want to see, and the AI will handle the query for you.

## Configuration Steps

```mermaid
graph LR
    A[Prepare Config File] --> B[Add to AI Settings]
    B --> C[Start Using]
    style A fill:#f0f8ff
    style B fill:#e6ffe6
    style C fill:#fff0f0
```

### Step 1: Prepare Configuration File

Create a `config.yaml` file and add your database information:

```yaml
databases:
  # PostgreSQL database configuration
  stage-db:
    type: postgres
    dbname: stage
    user: myuser
    password: mypassword
    host: localhost
    port: 5432

  # SQLite database configuration
  dev-db:
    type: sqlite
    path: /path/to/dev.db
```

### Step 2: Add to AI Settings

Add to your AI assistant configuration:

```json
"mcpServers": {
  "dbutils": {
    "command": "uvx",
    "args": [
      "mcp-dbutils",
      "--config",
      "/path/to/config.yaml"
    ],
    "env": {
      "MCP_DEBUG": "1"  // For debug information if needed
    }
  }
}
```

### Step 3: Start Using

Once configured, you can query databases through natural conversation as demonstrated at the beginning!

## Security Notes

- Service only supports SELECT queries to protect database security
- Sensitive information (like passwords) is automatically hidden in logs
- All queries are executed within read-only transactions

Now you can manage multiple databases as easily as having a conversation. Give it a try - you'll love this simple approach!
