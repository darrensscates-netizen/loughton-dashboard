LOUGHTON OPERATIONS DASHBOARD
=============================

A single-file, wall-mounted information dashboard designed for an Amazon Fire
tablet (landscape). It rotates between two pages every 10 seconds and shows
live news, rain radar, weather, system/network health, Tube status, train and
bus departures and nearby aircraft.

There is no server, no build step, no framework and no API keys. The whole
dashboard is one file: index.html. Open it in a browser and it runs.

Current version: V16 (see the small version label at the bottom-right of the
screen, the <title>, and the "dashboard-version" meta tag in the file).


-------------------------------------------------------------------------------
1. WHAT IT SHOWS
-------------------------------------------------------------------------------

Page 1 (news, radar, health, weather)
  - BBC NEWS         Top headlines from the BBC UK news feed.
  - RAIN RADAR       Live rainfall radar over a dark map, centred on your
                     location, with 25 and 50 mile range rings.
  - SYSTEM STATUS    Ping time to Google, download speed, and a live/cached/
                     unavailable indicator for each data feed.
  - WEATHER          Current conditions and a 4-day forecast.

Page 2 (travel and airspace)
  - TUBE STATUS      Service status for chosen London Underground / Elizabeth
                     lines.
  - STATION BOARD    Next trains in one direction from one Tube station.
  - NEXT BUSES       Next buses from the nearest bus stops.
  - NEAREST AIRCRAFT The four nearest aircraft within 10 miles, with callsign,
                     airline, destination (where known), distance, altitude,
                     speed and heading.

The header shows the clock, date and a countdown to the next page flip. The
footer shows a one-line description of the current page.

Every feed keeps its last good result in the browser's local storage. If a
service is unreachable the panel shows the cached data marked "Cached" rather
than going blank.


-------------------------------------------------------------------------------
2. DATA SOURCES (all free, no keys needed)
-------------------------------------------------------------------------------

  Weather        Open-Meteo               api.open-meteo.com
  News           BBC RSS via rss2json     api.rss2json.com
  Tube / buses   Transport for London     api.tfl.gov.uk
  Rain radar     RainViewer               api.rainviewer.com, tilecache.rainviewer.com
  Map            Esri World Dark Gray     server.arcgisonline.com
  Aircraft       adsb.lol / adsb.fi /     fetched through free public relays
                 OpenSky                  (api.allorigins.win, api.codetabs.com)
  Flight routes  adsbdb                   api.adsbdb.com
  Ping test      Google                   www.google.com
  Speed test     Cloudflare               speed.cloudflare.com

If your home network uses a DNS filter, Pi-hole or firewall, allow the domains
in the right-hand column.

Please respect the terms and rate limits of these free services. Do not lower
the refresh intervals dramatically.


-------------------------------------------------------------------------------
3. INSTALLING IT
-------------------------------------------------------------------------------

Option A - GitHub Pages (easiest to maintain)
  1. Put index.html in a GitHub repository.
  2. Repository Settings > Pages > Deploy from a branch > main / root > Save.
  3. After a minute your dashboard is at
         https://YOUR-USERNAME.github.io/YOUR-REPO/
  4. Use that address as the start page on the tablet (see section 4).
     Edit the file on GitHub and the tablet picks up the change on its next
     reload.

Option B - local file
  1. Copy index.html onto the tablet's storage.
  2. Open it in the tablet's browser using its file:// address.
  It works this way, but updating means copying the file again.

Wi-Fi: there is NO Wi-Fi setting in the dashboard code. The tablet simply needs
a working internet connection. Join your Wi-Fi in the tablet's own settings
(Settings > Wi-Fi). If the connection drops, the dashboard retries
automatically, and reloads its data as soon as the network returns.


-------------------------------------------------------------------------------
4. FIRE TABLET KIOSK SETUP
-------------------------------------------------------------------------------

Menu names vary a little between Fire OS versions, so treat these as a guide.

Power and screen
  1. Keep the tablet plugged in. It will run continuously, so use a good
     charger. Check the battery now and then, since constant charging at
     full brightness is hard on a battery.
  2. Turn on Developer Options:
        Settings > Device Options > About Fire Tablet
        Tap "Serial Number" 7 times.
        Go back to Device Options > Developer Options.
  3. In Developer Options, turn on "Stay Awake". This keeps the screen on
     whenever the tablet is charging.
  4. Settings > Display: set a fixed brightness you are happy with, turn
     adaptive brightness off, and set the longest screen timeout as a backup.
  5. Lock the orientation to LANDSCAPE (swipe down from the top and make sure
     Auto-Rotate is locked while the tablet is held landscape). The layout is
     designed for landscape.

Lock screen and interruptions
  6. Settings > Security & Privacy (or Lock Screen): remove any passcode so the
     dashboard is never hidden behind a lock screen.
  7. Turn on Do Not Disturb so notifications do not pop up over the display.
  8. Optional: switch off automatic system and app updates so the tablet does
     not restart or change behaviour unexpectedly. Check for updates manually
     now and then.

The browser
  Option 1 - Silk (the built-in browser)
     - Open your dashboard address and set it as the homepage.
     - Use the browser's full-screen / hide-toolbar option if your version
       has one.
     - Simple, but it can drift back to the normal browser interface and does
       not reload itself.

  Option 2 - Fully Kiosk Browser (recommended)
     - Install the Fire edition from fully-kiosk.com. Some kiosk features
       need its paid PLUS licence; the basic version covers a lot of this.
     - Useful settings (names may vary slightly by version):
         Web Content Settings   Start URL = your dashboard address
         Device Management      Keep Screen On = ON
                                Screen Brightness = a fixed value
         Web Auto Reload        Reload on network reconnect = ON
                                Auto reload on idle / periodically = optional,
                                e.g. once a day, to clear out any stuck state
         Kiosk Mode (PLUS)      Lock the tablet to the dashboard
         Launch on boot         ON, so it recovers after a power cut
     - Fully Kiosk also has screen on/off scheduling options if you do not
       want the display on overnight.

Troubleshooting the tablet
  - Screen goes to sleep: check Stay Awake, and that the tablet is charging.
  - Page stuck or blank: reload the page, or set Fully Kiosk to reload on a
    schedule.
  - A panel says "unavailable": check the SYSTEM STATUS panel, then your
    Wi-Fi and any DNS filtering.


-------------------------------------------------------------------------------
5. CHANGING IT FOR YOUR OWN LOCATION
-------------------------------------------------------------------------------

All the settings are inside index.html. Use your editor's Find to locate each
item below.

5.1 Your coordinates  (search for:  var C={ )
    Near the top of the <script> is a settings object called C:

        lat:51.6517, lon:0.0558,

    Replace these two numbers with your own latitude and longitude. This one
    pair centres the weather, the rain radar and the aircraft search. To find
    them, look up your postcode on a site such as postcodes.io or right-click
    your house in Google Maps.

5.2 Time zone and language  (search for:  Europe/London  and  en-GB )
    "Europe/London" appears in the clock, date and timestamp code, and as
    "Europe%2FLondon" in the weather request. Replace every occurrence with
    your own zone (for example "America/New_York" and "America%2FNew_York").
    "en-GB" controls date/time wording, and the <html lang="en-GB"> tag. Change
    to, say, "en-US". The clock is 24-hour; set hour12:true for a 12-hour
    clock.

5.3 Temperature units  (search for:  deg(  and  °C )
    The dashboard shows Celsius. For Fahrenheit, add
    "&temperature_unit=fahrenheit" to the Open-Meteo address in the weather()
    function and change the "°C" text to "°F".

5.4 Local names on screen  (search for:  LOUGHTON )
    Change "Loughton Dashboard" in the <title>, "LOUGHTON OPERATIONS DASHBOARD"
    in the header, and the "LOUGHTON WESTBOUND" and "THE CROWN . NEXT BUSES"
    panel titles.

5.5 News  (search for:  rss: )
    The BBC UK feed is set in C.rss. For another BBC feed, such as World or
    Business, swap the address. Any standard RSS feed works. C.maxNews sets
    how many headlines are fetched.

5.6 London Underground lines  (search for:  lines: )
    C.lines lists the lines in the TUBE STATUS panel, using TfL line ids, e.g.
    "bakerloo", "central", "circle", "district", "elizabeth",
    "hammersmith-city", "jubilee", "metropolitan", "northern", "piccadilly",
    "victoria", "waterloo-city", "dlr", "london-overground".
    Two more things must match if you change the list:
      - the order list in renderTfl() (search for:  elizabeth:0 ) which sets
        the display order; add each line id with a number
      - the colour for each line in the style section (search for:  --central ).
        Add a colour variable and a matching CSS class for each new line.

5.7 Station board  (search for:  940GZZLULGN )
    This is the TfL id for Loughton station. Find yours at
        https://api.tfl.gov.uk/StopPoint/Search?query=YOUR+STATION&modes=tube
    The board keeps only trains going one way. That is the text
    direction==="inbound" (towards central London from Loughton). Change it to
    "outbound" for the other direction, and edit the "LOUGHTON WESTBOUND"
    title and the "Westbound" fallback label.

5.8 Bus stops  (search for:  crownKnown )
    crownKnown is a list of TfL bus stop ids near The Crown pub in Loughton.
    Replace it with your own stop ids. Find stops near you with:
        https://api.tfl.gov.uk/StopPoint?lat=YOUR_LAT&lon=YOUR_LON&stopTypes=NaptanPublicBusCoachTram&radius=500&modes=bus
    If those stops return nothing, the code falls back to searching for stops
    with "crown" in the name. Search for  /crown/i  and  query=Crown  and
    change or remove them. Also update the "The Crown" text shown under each
    bus.

5.9 Outside London
    The Tube, station and bus panels use the Transport for London service and
    only work in Greater London. Elsewhere, replace those panels with data
    from your own local transport provider, or delete them from page 2. The
    weather, news, radar, system status and aircraft panels work anywhere.

5.10 Rain radar  (search for:  r50  and  [25,50] )
     The radar is centred on C.lat / C.lon. The pane is sized so the 50 mile
     ring nearly fills the shorter side. To change the range, edit the
     constant 50 in the r50 line and the [25,50] ring list, plus the
     "RAIN RADAR . 50 MILES" title. The map and radar are drawn at a fixed
     zoom, so very different ranges may look blurry or too coarse.

5.11 Aircraft  (search for:  <=10  and  slice(0,4) )
     - Search radius: x.dist<=10 (miles) in aircraft(); the feed requests
       use "/10" and "dist/10" (nautical miles) and d=0.15 for the OpenSky
       box. Change them together.
     - Number shown: slice(0,4).
     - Airline names come from the lookup service, with a built-in backup
       table (search for:  var AL= ) of common three-letter airline codes.
       Add your local airlines there.
     - Titles: "NEAREST AIRCRAFT . 10 MILES" and the "within 10 miles"
       message.
     Note: aircraft data only works through free public relays (see
     Limitations). C.airProxy is an optional setting for your own relay.

5.12 Timings  (search for:  flip:  and  intervals: )
     C.flip is seconds per page. C.intervals sets how often each feed
     refreshes, in milliseconds:
         weather 900000 (15 min)  tfl 120000 (2 min)   news 600000 (10 min)
         departures 30000         buses 30000          air 30000
         radar 300000 (5 min)     ping 30000           speed 1800000 (30 min)

5.13 Speed test  (C.speedMaxBytes, C.speedMaxSec, C.speedWarmSec)
     Downloads a test file from Cloudflare for up to speedMaxSec seconds,
     ignores the first speedWarmSec seconds while the connection ramps up,
     and averages the rest. On a fast connection each test can use up to
     speedMaxBytes (50 MB). At one test every 30 minutes that is at most about
     2.4 GB a day. Lower the file size or lengthen the speed interval if you
     have a data allowance. While a test runs (a few seconds), other refreshes
     pause.

5.14 Version label
     When you change the dashboard, update the "dashboard-version" meta tag,
     the <title> and the small version label near the bottom of the page. The
     cachePrefix value stores saved data in the browser. Change it if you want
     to clear all cached data.


-------------------------------------------------------------------------------
6. LIMITATIONS
-------------------------------------------------------------------------------

  - Aircraft: the main aircraft services do not allow web pages to read their
    data directly, so the dashboard goes through free public relays. These can
    be slow or intermittently unavailable, in which case the panel briefly
    shows the last good list marked "Cached". A small status line under the
    list shows which source was used and how many aircraft were found.
  - Destinations and airlines: depend on a community database. Private,
    military and some other flights will show no destination.
  - Ping and speed: measured by a web page, so they are indicative only and
    not equal to an app-based speed test. The tablet's browser and Wi-Fi add
    some overhead.
  - Free services change. For example, a free map provider recently began
    watermarking its free tiles. If a panel breaks, the status panel and the
    browser's developer console (F12 on a computer) will show which service
    stopped working.
  - Rain radar: the free service limits zoom, so the radar is slightly soft.


-------------------------------------------------------------------------------
7. TESTING ON A COMPUTER
-------------------------------------------------------------------------------

Open index.html in a desktop browser and press F12 for the Console. Resize the
window to roughly landscape tablet proportions (for example 1280 x 800). Errors
from failed feeds appear in red in the Console.


-------------------------------------------------------------------------------
8. CREDITS AND LICENCES
-------------------------------------------------------------------------------

  - Weather data: Open-Meteo.com (CC BY 4.0)
  - Transport data: Powered by TfL Open Data
  - News: BBC News RSS (for personal, non-commercial use)
  - Radar: RainViewer
  - Map: Esri, HERE, Garmin, OpenStreetMap contributors
  - Aircraft data: adsb.lol, adsb.fi, OpenSky Network
  - Flight route data: adsbdb.com

Add your own LICENSE file to the repository to say how others may reuse the
code.
