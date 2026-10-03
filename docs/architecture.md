# Architecture

## Components

| Component | Role |
|---|---|
| User | Sends a natural-language request and receives the answer |
| EC2 (t3.micro) | Hosts the n8n container |
| n8n + AI Agent | Orchestrates the workflow: receives the request, calls the LLM, runs the SQL, formats the reply |
| Amazon Bedrock | LLM that generates SQL from the user's request and the table schema |
| RDS PostgreSQL | Stores the data and executes the SQL |

## Request flow

1. The user sends a question (for example, "how many customers signed up this month?").
2. The n8n AI Agent receives it together with the database schema.
3. The agent calls Bedrock to generate a SQL query.
4. n8n executes the query on RDS PostgreSQL.
5. The rows are returned to the agent, which sends back a readable answer.

## Network & security

- RDS Security Group: inbound 5432 from the EC2 Security Group only.
- EC2 Security Group: inbound access to the n8n port restricted to trusted IPs.
- Credentials kept in n8n credentials / environment variables, never in Git.
