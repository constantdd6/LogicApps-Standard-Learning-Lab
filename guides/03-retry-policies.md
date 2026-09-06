# Retry Policies in Azure Logic Apps Standard

## Learning Objectives

By the end of this guide, you will:

- Understand transient failures
- Configure retry policies
- Reduce manual intervention
- Build resilient integrations

---

## What Is a Retry Policy?

Sometimes failures are temporary:

- Brief network outage
- API throttling
- Service Bus latency
- Storage timeout

Instead of immediately failing, Logic Apps can retry automatically.

---

## Default Behavior

Many connectors already support retries.

Example:

```text
Call API
    |
Failure
    |
Retry
    |
Success
```

---

## Common Retry Types

### Fixed Interval

```text
Retry every 30 seconds
```

Example:

```json
{
  "type": "fixed",
  "count": 3,
  "interval": "PT30S"
}
```

---

### Exponential Interval

Retries wait longer between attempts.

Example:

```text
Attempt 1 = 10 seconds
Attempt 2 = 20 seconds
Attempt 3 = 40 seconds
```

Useful for throttling scenarios.

---

## Configure Retry Policy

1. Select an action.
2. Open Settings.
3. Find Retry Policy.
4. Configure:

```text
Type
Count
Interval
```

---

## Recommended Settings

### External APIs

```text
Exponential
5 retries
```

### Storage Operations

```text
Fixed
3 retries
```

### Mission Critical APIs

```text
Exponential
5-8 retries
```

---

## Avoid Over-Retrying

Do not retry:

- Invalid data
- Authentication failures
- Business rule violations

Example:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
```

These problems generally require investigation rather than retries.

---

## Challenge

Configure retry policies for:

- HTTP Action
- Service Bus Action
- Storage Action

Observe behavior during simulated failures.

---

## Expected Outcome

Your integrations become more resilient and recover automatically from temporary issues.