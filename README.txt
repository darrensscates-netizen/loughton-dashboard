LOUGHTON OPERATIONS DASHBOARD V13.0

This release is deliberately self-contained. CSS and JavaScript are embedded in index.html, so files from different versions cannot be mixed.

INSTALL
1. Create a new empty folder named v13.
2. Copy index.html into v13.
3. Open index.html and force refresh with Ctrl+F5.
4. For the Fire tablet, publish index.html to an HTTPS host and use that address as Fully Kiosk Browser's Start URL.

REFRESH CYCLES
- Clock/date/countdown: 1 second
- TfL: 2 minutes
- BBC News: 10 minutes
- Weather: 15 minutes
- Scheduler health check: 5 seconds

RESILIENCE
- Three attempts per request with increasing delay
- 15-second timeout
- Independent schedules
- No overlapping calls to the same feed
- Last successful results cached in localStorage
- Cache restored immediately after restart
- Live, Cached and Unavailable states
- Immediate feed refresh after reconnection

LIVE SOURCES
- Weather: Open-Meteo
- Transport: TfL Unified API
- News: BBC UK RSS through RSS2JSON
