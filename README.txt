LOUGHTON OPERATIONS DASHBOARD V13.2.1 HOTFIX

Fixes the weather tile stopping after current temperature, description, feels-like and humidity. V13.2 removed the current high/low HTML fields but the script still attempted to update them, causing JavaScript execution to stop before rain chance, forecast, status and scheduler initialisation.

This release removes those obsolete references and tolerates older cached weather records.

Install by replacing index.html in GitHub, committing, waiting for Pages deployment, then clearing/reloading Fully Kiosk. Confirm V13.2.1 at bottom-right.
