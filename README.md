# Google Calendar MCP Server

An MCP (Model Context Protocol) server for Google Calendar API. This server allows AI assistants to create, list, update, and delete calendar events using Google service account authentication.

## Features

- **Create events** with title, description, location, and time
- **List upcoming events** with filtering and search
- **Get event details** by ID
- **Update existing events** (title, time, location, status)
- **Delete events** from the calendar
- **All-day and timed events** supported

## Prerequisites

- Node.js 18+
- Google Cloud service account with Calendar API enabled
- Calendar shared with the service account's `client_email`

## Installation

```bash
npm install
npm run build
```

## Configuration

Set your Google credentials via environment variables:

```bash
# Option 1: Path to service account JSON key file
export GOOGLE_SERVICE_ACCOUNT_KEY_PATH=/path/to/service-account-key.json

# Option 2: Inline JSON content
export GOOGLE_SERVICE_ACCOUNT_KEY='{"client_email":"...","private_key":"..."}'

# Optional: Default calendar ID (defaults to 'primary')
export GOOGLE_CALENDAR_ID=your-calendar-id@group.calendar.google.com
```

## Usage with Claude Code

```bash
claude mcp add-json gcal '{
  "command": "node",
  "args": ["/path/to/gcal-mcp-server/dist/index.js"],
  "env": {
    "GOOGLE_SERVICE_ACCOUNT_KEY_PATH": "/path/to/service-account-key.json",
    "GOOGLE_CALENDAR_ID": "your-calendar-id@group.calendar.google.com"
  }
}'
```

## Available Tools

### gcal_create_event

Create a new event on a Google Calendar.

**Parameters:**
- `summary` (required): Event title
- `start_time` (required): ISO8601 timestamp or date for all-day events
- `end_time` (required): ISO8601 timestamp or date
- `description`: Event description
- `location`: Event location
- `timezone`: Timezone (e.g., `America/Los_Angeles`)
- `calendar_id`: Target calendar (defaults to env var or `primary`)

**Example:**
```
Create a meeting called "Team Standup" tomorrow at 10am for 30 minutes
```

### gcal_list_events

List upcoming events from a Google Calendar.

**Parameters:**
- `max_results`: Number of events to return (1-250, default: 10)
- `time_min`: Lower bound for event start time (ISO8601)
- `time_max`: Upper bound for event start time (ISO8601)
- `query`: Free text search to filter events
- `calendar_id`: Target calendar

### gcal_get_event

Get details of a specific calendar event by ID.

**Parameters:**
- `event_id` (required): The event ID
- `calendar_id`: Target calendar

### gcal_update_event

Update an existing calendar event. Only provide fields you want to change.

**Parameters:**
- `event_id` (required): The event ID
- `summary`: New title
- `description`: New description
- `location`: New location
- `start_time`: New start time (ISO8601)
- `end_time`: New end time (ISO8601)
- `timezone`: Timezone
- `status`: `confirmed`, `tentative`, or `cancelled`
- `calendar_id`: Target calendar

### gcal_delete_event

Delete an event from the calendar.

**Parameters:**
- `event_id` (required): The event ID
- `calendar_id`: Target calendar

## Setup Guide

1. Create a Google Cloud project and enable the **Google Calendar API**
2. Create a **service account** and download the JSON key
3. Share your calendar with the service account's `client_email` (give "Make changes to events" permission)
4. Configure the environment variables above

## License

MIT
