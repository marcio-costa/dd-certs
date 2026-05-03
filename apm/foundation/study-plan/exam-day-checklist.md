# Exam Day Checklist

## 24 hours before

- [ ] Confirm test slot on Kryterion Webassessor (`webassessor.com/datadog`).
- [ ] Verify ID requirements for proctoring (government photo ID).
- [ ] Test webcam, mic, and download Sentinel Secure if remote-proctored.
- [ ] Light review only — no new material the day before.
- [ ] Sleep 8 hours.

## Morning of

- [ ] Eat a real breakfast. Hydrate, but not so much you need to leave the room mid-exam.
- [ ] Clear your desk (no notes, no second monitor, no phone in reach for online proctoring).
- [ ] Restart laptop. Close all apps. Disable notifications.
- [ ] Start the proctor session **15 minutes early**.

## During the exam

- 90 questions, 75 scored. **2 hours** = 80 seconds per question on average.
- All questions are **single-answer multiple choice** (no select-N or ordering).
- Unanswered questions count as wrong → **answer every question**.
- Pretest items are unidentified — treat every question equally.
- Strategy:
  1. **First pass**: answer everything you know in <60 sec; flag anything unsure.
  2. **Second pass**: spend up to 90 sec on each flagged item.
  3. **Third pass**: answer remaining flagged with best guess.
- If torn between two options, prefer the one most aligned with **Datadog defaults** (e.g., port 8126, head-based sampling, USM tags).
- Watch the clock — keep ≥30 min for pass 2/3.

## Top facts to recall on the spot

- Default trace port: **8126/TCP**; DogStatsD: **8125/UDP**.
- Live Search window: **15 min**.
- Custom retention: **15 days**; intelligent retention: **30 days**; trace metrics: **15 months**.
- USM tags: **`env`, `service`, `version`**.
- Default sampling: **head-based**, set on the **root service**.
- `DD_APM_NON_LOCAL_TRAFFIC=true` to accept traces from other containers.
- `DD_LOGS_INJECTION=true` for trace-log correlation.
- Error Tracking fingerprint = `error.type` + `error.message` + stack frames.
- Auto-version (Agent **7.52+**): `DD_VERSION` > image tag > Git SHA.
- Default automatic error-rate alert threshold: **10%**.

## After the exam

- Result is shown immediately.
- Pass: cert appears in your Credly account within 48 hours.
- Fail: review the score report. Up to **3 retakes** in a 180-day window.
- Either way: update `progress/exam-results.md`.
