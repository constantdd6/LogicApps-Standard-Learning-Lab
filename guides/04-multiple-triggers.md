# Multiple Trigger Strategies in Azure Logic Apps Standard

## Learning Objectives

By the end of this guide, you will:

- Understand trigger patterns
- Compare common trigger types
- Design flexible integrations
- Choose the right trigger for a business scenario

---

## What Is a Trigger?

A trigger starts a workflow.

Examples:

- HTTP request received
- Service Bus message received
- Blob created
- Recurrence schedule

---

## Common Trigger Types

### HTTP Trigger

Used for:

- APIs
- System-to-system communication

Example:

```text
ERP System
    |
    V
Logic App
```

---

### Recurrence Trigger

Used for:

- Scheduled jobs
- Daily reports
- Batch processing

Example:

```text
Every day at 08:00
```

---

### Service Bus Trigger

Used for:

- Event-driven integrations
- Asynchronous processing

Example:

```text
New Queue Message
    |
    V
Logic App
```

---

### Blob Storage Trigger

Used for:

- File-based integrations

Example:

```text
New File Uploaded
    |
    V
Logic App
```

---

## Design Considerations

Ask:

1. Is it real-time?
2. Is it event-driven?
3. Is it scheduled?
4. Does it involve files?

Choose the trigger that best matches the business process.

---

## Practice Lab

Create three workflows:

### Workflow 1

```text
HTTP Trigger
```

Returns:

```json
{
  "message": "Hello World"
}
```

---

### Workflow 2

```text
Recurrence Trigger
```

Runs every 5 minutes.

---

### Workflow 3

```text
Service Bus Trigger
```

Processes queue messages.

---

## Best Practices

✅ One clear business purpose per workflow

✅ Name triggers consistently

✅ Monitor trigger failures

✅ Document trigger type in README

✅ Use Managed Identity where possible

---

## Challenge

Compare:

- HTTP Trigger
- Service Bus Trigger
- Recurrence Trigger

Document:

- Advantages
- Disadvantages
- Use Cases

---

## Expected Outcome

You understand which trigger should be used for various enterprise integration scenarios.