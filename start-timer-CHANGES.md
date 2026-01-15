# Start Timer Plugin - Change Log

## Overview
This document details all changes made to the `start-timer.omnifocusjs` plugin to ensure compliance with Toggl Track API v9 specifications and improve functionality.

## Changes Made

### 1. API Version Migration (v8 → v9)

**Reason:** Toggl API v8 is being deprecated. The API v9 is the current recommended version.

**Changes:**
- Updated `/me` endpoint: `api/v8/me` → `api/v9/me`
- Updated time entries endpoint: `api/v8/time_entries/start` → `api/v9/workspaces/{workspace_id}/time_entries`
- Updated projects endpoint: `api/v8/projects` → `api/v9/workspaces/{workspace_id}/projects`

### 2. Workspace ID Integration

**Reason:** API v9 requires `workspace_id` in the URL path for most endpoints, unlike v8 which accepted `wid` in the request body.

**Changes:**
- Modified `getTogglProjects()` → `getTogglData()` to return full user data including `default_workspace_id`
- Added workspace_id retrieval and validation at the start of the action
- Updated all API functions to accept and use `workspaceId` parameter:
  - `startTimer(timeEntry, workspaceId)`
  - `createTogglProject(name, workspaceId)`
  - `activateTogglProject(projectId, workspaceId)`
  - `stopTimeEntry(timeEntryId, workspaceId)`

### 3. Time Entry Structure Updates

**Reason:** API v9 uses different request body structure and field names.

**Changes:**
- Removed v8 wrapper object: Changed from `{time_entry: {...}}` to flat object
- Added required `start` field with ISO 8601 timestamp (current UTC time)
- Added `duration: -1` to indicate a running timer (v9 requirement)
- Changed `pid` → `project_id` for v9 compatibility
- Added `workspace_id` field to time entry payload

### 4. Project Creation Updates

**Reason:** API v9 requires different request structure and explicit active state.

**Changes:**
- Removed v8 wrapper: Changed from `{project: {name, wid}}` to `{name, active}`
- Added `active: true` flag to ensure new projects aren't archived by default
- Removed hardcoded `wid: 20847479`, now uses dynamic workspace_id from user data

### 5. Archived Project Handling

**Reason:** Toggl API returns "project is archived" error when trying to create time entries for inactive projects.

**Changes:**
- Added `activateTogglProject()` function to reactivate archived projects via PUT endpoint
- Added check for `togglProject.active` status before starting timer
- Automatically activates archived projects when detected
- Applies to both project and task selection code paths

### 6. Timer Toggle Functionality

**Reason:** User requested ability to stop a running timer by re-running the automation on the same task.

**Changes:**
- Added `getCurrentTimeEntry()` function to fetch currently running timer
- Added `stopTimeEntry()` function to stop timers via PATCH endpoint
- Before starting a new timer, checks if one is already running for the same task description
- If running timer matches current task, stops it instead of creating duplicate

### 7. User Feedback Messages

**Reason:** Provide clear feedback about timer state changes.

**Changes:**
- Added success alert when timer starts: `"Timer started for [task name]"`
- Added success alert when timer stops: `"Timer stopped for [task name]"`
- Replaced generic error messages with specific user-facing alerts

### 8. Error Handling Improvements

**Reason:** Ensure graceful failure and better debugging capability.

**Changes:**
- Added status code validation to all API calls
- Added early return when workspace_id cannot be retrieved
- Added try-catch blocks around current timer checks
- Maintained console logging for debugging while showing user-friendly alerts

## API Compliance Summary

### Rate Limits (per hour, per user, per organization)
- Free: 30 requests
- Starter: 240 requests
- Premium: 600 requests
- General rate: 1 request/second

### Authentication
- Uses Basic authentication with API token
- Format: `Basic base64(api_token:api_token)`

### Required Headers
- `Content-Type: application/json`
- `Authorization: Basic {encoded_token}`

### Timestamp Format
- ISO 8601 / RFC 3339 (UTC)
- Example: `2025-10-01T14:30:00Z`

## Testing Recommendations

1. Test timer start with new project (should create project and start timer)
2. Test timer start with existing active project (should start timer)
3. Test timer start with archived project (should activate and start timer)
4. Test timer toggle (start → stop → start on same task)
5. Test with both project selection and task selection
6. Test with tags prefixed with "Toggl"
7. Verify workspace_id retrieval works for different account types

## Breaking Changes

- Plugin now requires API v9 compatible Toggl account
- Hardcoded workspace ID removed (uses user's default workspace)
- Timer behavior changed from "always start" to "toggle start/stop"

## Known Limitations

- Only works with default workspace (doesn't support multi-workspace selection)
- Timer matching based on description text only (case-sensitive exact match)
- No handling for multiple running timers (unlikely but possible edge case)
