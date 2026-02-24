# **Frontend Engineer Assignment (No Code, High-Quality UI Focus)**

## **Evaluation Criteria**

* Frontend tech selection and reasoning (framework, state, data fetching)
* UI architecture (routing, component design, reusable patterns)
* API calling strategy (error handling, retries, abort, pagination)
* Browser-level caching + offline-friendly patterns
* Debugging + observability (logging, tracing, error boundaries)
* Security basics on client (token handling, safe downloads, XSS considerations)
* UX quality for async jobs (progress, partial results, resilience)

---

## **Problem 1: Video-to-Notes Platform (Frontend System Design)**

**Goal:** Upload video → job runs async → user sees status + outputs: Summary.md, highlights (timestamps), assets. [READ MORE ABOUT THE PROJECT](./Video-summary-platform.md)

**Your solution must include**

* **Screens:** Upload, Jobs list, Job detail (status/logs), Results (markdown + highlights)
* **UI states:** loading, queued, processing, success, failed, retry, partial output
* **API calling plan:** how you poll/stream job progress (polling vs SSE), abort on navigation
* **Caching:** what to cache in browser (job list, job detail, results), TTL strategy, invalidation
* **Debugging plan:** how you would debug “stuck processing” from frontend side (network logs, correlation id display)

**Your Solution for problem 1:**

You need to put your solution here.

---

### Summary.md contains:

- Video metadata (filename, duration, processing date)
- High-level summary
- Key highlights with timestamps
- Takeaways or action items
- Links to generated clips and screenshots

This predictable structure helps users easily navigate results.

---

# 1. Screens (User Interface)

## 1.1 Upload Screen

This is the entry point where users upload videos.

### Components

- Drag-and-drop upload area
- File selector button
- Supported file format message
- File size limit information
- Upload progress bar
- Cancel upload option
- Error notification area

### UX Flow

1. User selects or drags a video file.
2. The system validates format and size.
3. Upload begins and progress is displayed.
4. Once uploaded, a **processing job is created**.
5. The user is redirected to the **Jobs List page**.

If upload fails (network issue or invalid file), a **retry option** appears.

---

## 1.2 Jobs List Screen

This screen shows all video processing jobs.

### Layout

A table or card layout containing:

- Video name
- Upload time
- Job status
- Processing progress
- Action button (View Details)

### Status Indicators

| Status | Meaning |
|------|------|
| Queued | Waiting to start |
| Processing | Currently analyzing video |
| Success | Processing completed |
| Failed | Job failed |
| Partial | Some outputs ready |

### UX Features

- Auto-refresh for active jobs
- Pagination or infinite scroll
- Clicking a job opens the **Job Detail page**

---

## 1.3 Job Detail Screen (Status + Logs)

This page displays detailed information about a specific job.

### Job Information

- Job ID
- Video name
- Upload date
- Current status

### Processing Steps

The UI shows progress through stages such as:

1. Video transcription
2. Summary generation
3. Highlight detection
4. Asset extraction

### Logs Panel

A scrollable log section showing processing events.

Example logs:

- Video uploaded successfully
- Transcription started
- Summary generation started
- Highlight extraction completed

### UX Features

- Live progress updates
- Retry button for failed jobs
- Display **correlation ID** for debugging

---

## 1.4 Results Screen

After processing completes, users can view the generated outputs.

The results page contains three tabs.

### Summary Tab

Displays the generated Markdown summary.

Features:

- Clean reading view
- Copy summary button
- Download `.md` file

---

### Highlights Tab

Displays important moments from the video.

Each highlight contains:

- Description
- Timestamp

Clicking a timestamp jumps to that moment in the video.

Example:

- **02:15** – Introduction to topic
- **07:40** – Key explanation

---

### Assets Tab

Displays generated assets.

Assets include:

- Highlight clips
- Screenshots

Users can download individual files or **download all assets as a ZIP file**.

---

# 2. UI State Management

Because processing takes time, the UI must represent the job state clearly.

Supported states:

| State | UI Behavior |
|------|------|
| Uploading | Show progress bar |
| Queued | Show waiting indicator |
| Processing | Animated progress + step indicator |
| Partial | Show available results |
| Failed | Error message + retry option |
| Success | Results available |

The UI always reflects the **backend job state** to avoid incorrect assumptions.

---

# 3. API Calling Strategy

## Job Creation

After uploading a video, the backend returns a **job_id**.

The frontend uses this ID to track progress.

---

## Job Status Updates

The frontend periodically checks job status using **polling**.

Polling strategy:

- Request job status every **5 seconds**
- Stop polling once job completes
- Increase interval if server errors occur

Alternative approach:

If supported, use **Server-Sent Events (SSE)** so the server can push real-time updates.

---

## Navigation Safety

When users leave the page:

- Active API requests are cancelled
- Prevents unnecessary network usage

---

# 4. Browser Caching Strategy

Caching improves performance and reduces repeated API calls.

### Cached Data

- Job list
- Job details
- Completed results

### Cache Rules

- Job list refreshes every few minutes
- Job details refresh while processing
- Completed results remain cached longer

### Offline Handling

If internet connection is lost:

- Cached job list is displayed
- Upload disabled
- Offline notification shown

---

# 5. Debugging and Observability

If users report that a job is stuck, the frontend should provide debugging information.

### Displayed Debug Information

- Job ID
- Correlation ID
- Last update time
- Processing logs

### Troubleshooting Steps

1. Verify API responses for job status.
2. Check polling requests in network logs.
3. Inspect log messages for errors.
4. Allow manual refresh of job status.

These features help developers quickly diagnose issues.

---

# 6. UX for Long Processing Jobs

Video processing may take several minutes.

To maintain a good user experience:

- Display continuous progress updates
- Allow navigation while processing continues
- Notify users when jobs complete
- Show partial results early if available

This ensures users remain informed throughout the process.

---

# Final Design Principle

The frontend behaves as a **state-driven interface** based on backend job status.

Job lifecycle:

Queued → Processing → Success / Failed

By clearly representing each state and providing detailed progress updates, the system allows users to quickly understand the results of long videos without watching the entire recording.

---

## **Problem 2: LinkedIn Automation Platform (Frontend System Design)**

**Goal:** Connect LinkedIn → persona setup → draft preview → approve → schedule → posting history. [READ MORE ABOUT THE PROJECT](./linkedin-automation.md)

**Your solution must include**

* **Screens:** Connect, Persona editor, Drafts (3 variants), Approval, Scheduler, Post history
* **Form UX:** persona inputs validation, topic input rules, guardrails for scheduling
* **API calling:** draft generation request lifecycle, optimistic UI vs strict confirmation
* **Caching:** drafts caching, schedule list caching, refetch triggers after approval/post
* **Debugging:** how you surface posting failures to user and capture details for support

**Your Solution for problem 2:**

You need to put your solution here.

---

## **Problem 3: DOCX Template → Bulk Generator (Frontend System Design)**

**Goal:** Upload template → review fields → single generate → bulk via CSV → ZIP download + per-row report. [READ MORE ABOUT THE PROJECT](./docs-template-output-generation.md)

**Your solution must include**

* **Screens:** Template upload, Field review/editor, Single fill form, Bulk upload, Bulk run status, Report table, Downloads
* **Field UI:** field types (text/number/date), required/default, inline validation
* **Bulk UX:** CSV upload constraints, mapping UI (optional), progress + partial success
* **Browser caching:** template metadata caching, field schema caching, bulk report pagination caching
* **Downloads:** safe download UX (signed URL flow assumed), progress indicator

**Your Solution for problem 3:**

You need to put your solution here.

---

## **Problem 4: Character-Based Video Series Generator (Frontend System Design)**

**Goal:** Define characters once → create episode from story → view episode package (script/scenes/assets/render plan). [READ MORE ABOUT THE PROJECT](./char-based-video-generation.md)

**Your solution must include**

* **Screens:** Character library, Relationship editor, Episode creator, Episode detail (scenes), Asset gallery
* **Consistency UX:** show “locked character profile” per episode, version badges
* **API calling:** long-running generation job UI (progress, resume)
* **Caching:** character library caching, episode package caching, asset thumbnails caching

**Your Solution for problem 4:**

You need to put your solution here.

---

## **Cross-Cutting** 

Answer these in **bullet points** (max 1 page total):

1. **Frontend stack choice**

* EDIT YOUR ANSWER HERE: Framework (Next.js/Vue/etc), state management, router, UI kit, why.
  `<EDIT YOUR ANSWER HERE>`

2. **API layer design**

* Fetch/Axios choice, typed client generation (OpenAPI), error normalization, retries, request dedupe, abort controllers.
  `
  <EDIT YOUR ANSWER HERE>`

3. **Browser caching plan**

* What you cache (GET responses, derived state), where (memory, IndexedDB, localStorage), TTL/invalidation rules.
* How you handle “job status updates” without stale UI.
  `
  <EDIT YOUR ANSWER HERE>`

4. **Debugging & observability**

* Error boundaries, client-side logging approach, correlation id propagation, “report a problem” payload.
* How you would debug: slow uploads, failed downloads, intermittent 500s.
  `
  <EDIT YOUR ANSWER HERE>`

5. **Security basics**

* Token storage approach, CSRF considerations (if cookies), XSS avoidance for markdown rendering, safe file download patterns.
  ` A<EDIT YOUR ANSWER HERE>`
