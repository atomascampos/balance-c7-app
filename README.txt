================================================================================
BALANCE — NUTRITION & ACTIVITY TRACKER
Version 13.3
================================================================================

--------------------------------------------------------------------------------
OBJECTIVE
--------------------------------------------------------------------------------
Balance is a personal, mobile-first Progressive Web App (PWA) for tracking
daily nutrition macros (kilocalories and protein) and physical activity.
It is designed for solo use, with no accounts, no subscriptions, and no data
leaving your device. All data is stored locally on your phone.

The app helps you:
- Log every meal broken down by Breakfast, Lunch, Dinner and Snack
- Track kilocalories eaten and grams of protein per meal and per day
- Log physical activity and kilocalories burnt
- See your net kilocalories (eaten minus burnt) at a glance
- Review your history over the past 7 days with charts and averages
- Browse and search a database of 250+ European foods (Portuguese, UK, EU)
- Add your own custom foods to the database
- Edit or delete custom foods
- Estimate portion sizes using everyday descriptions (e.g. "two palm-sized steaks")
- Navigate to any past day to log or review entries
- Export and import all your data as a backup file


--------------------------------------------------------------------------------
APP STRUCTURE
--------------------------------------------------------------------------------
The app is a single HTML file (index.html) with no external dependencies
except Chart.js (loaded from CDN for the history graph).

Files:
  index.html          — The entire app (HTML, CSS, JavaScript)
  sw.js               — Service worker for offline support
  manifest.json       — PWA manifest (name, icons, display mode)
  icon-192.png        — App icon 192x192px
  icon-512.png        — App icon 512x512px
  apple-touch-icon.png — Icon for iOS home screen

Data storage:
  All user data is stored in the browser's localStorage under the key
  'balance_data'. The data structure is:
    {
      goals:  { kcal: number, prot: number },
      days:   { "YYYY-MM-DD": { meals: {...}, activities: [...] } },
      custom: [ { n, k, p, u }, ... ]
    }


--------------------------------------------------------------------------------
HOW THE APP WORKS
--------------------------------------------------------------------------------

TOP BAR
  - Displays the app name "balance" and current version number
  - "Goals" button (top right): set your daily kcal and protein targets
  - Date navigator (Today tab only): arrows to travel to previous days,
    date label with a "TODAY" badge when viewing the current day.
    Tap the date to jump back to today from any past day.

TODAY TAB
  - Two metric cards at the top: Kcal eaten and Protein, each with a
    progress bar against your daily goal. Colour changes from blue to
    amber to red as you approach or exceed your goal.
  - Net kcal card: shows eaten minus burnt. Green = deficit (burnt more
    than eaten). Red = surplus (eaten more than burnt).
  - Four meal sections: Breakfast, Lunch, Dinner, Snack. Each shows
    its running total of kcal and protein.
  - Each logged food entry shows name, quantity, kcal and protein,
    with a × delete button.
  - "Add food" button under each meal opens the food logging modal.
  - Activity section at the bottom: log activities with a description
    and kcal burnt. Each entry has a × delete button.

FOOD LOGGING MODAL (two steps)
  Step 1 — Find your food:
    - Meal selector (top right of header)
    - Search box: type to filter the full food database in real time.
      Results appear as large tappable buttons.
    - "Add it" link: if food is not in the database, tap to add it on
      the spot. It becomes immediately available and selected.
    - Cancel button at the bottom.

  Step 2 — Enter quantity:
    - Selected food shown at the top with its macro values per 100g/ml.
    - Quantity input (large font) and unit selector (g or ml) side by side.
    - Live preview: shows exact kcal and protein for the quantity entered.
    - "Describe your portion" link: expands a Portion Helper box where
      you can type a natural description (e.g. "two palm-sized steaks",
      "a large bowl", "three tablespoons") and tap "Go" to get an
      estimated quantity. Food-aware: a fist of lettuce ≠ a fist of rice.
    - "← Back" link to return to food search.
    - Cancel and Add to log buttons always visible at the bottom.

HISTORY TAB
  - 7-day averages: three cards showing average kcal eaten, kcal burnt,
    and average protein over the last 7 days with data.
  - Bar chart: 7-day visual of kcal eaten (red), kcal burnt (green),
    and protein in grams (blue).
  - Download data / Upload data buttons: export all your logs as a
    .json backup file, or restore from a previous backup.
  - Daily breakdown: each of the last 7 days listed with mini progress
    bars for kcal eaten, protein and kcal burnt.

FOODS TAB
  - Search box to filter the full database of 250+ foods.
  - All foods listed with kcal and protein per 100g or 100ml.
  - Custom foods added by the user are marked with a "custom" badge
    and have an "Edit" button to modify name, kcal, protein, unit,
    or delete the food entirely.
  - "+" button (top right) to add a new food to the database.

NAVIGATION
  - Three tabs at the bottom: Today, History, Foods.
  - Tap any tab icon to switch.
  - Swipe left to go to the next tab, swipe right to go back.
    (Swipes inside modals are ignored.)


--------------------------------------------------------------------------------
PORTION HELPER — REFERENCE
--------------------------------------------------------------------------------
The portion helper estimates quantities from natural language descriptions.
Estimates are food-aware (density adjusts by food type):

  Liquids:     glass (250ml), small glass (150ml), large glass (350ml),
               mug (300ml), cup (240ml), bottle (500ml), can (330ml),
               shot (35ml), tablespoon (15ml), teaspoon (5ml)

  By hand:     palm (120g), fist (150g), handful (40g), cupped hand (60g),
               thumb (15g), pinch (1g)
               Note: density adjusts — e.g. fist of lettuce ~25g, rice ~180g

  Portions:    slice (30g), thick slice (50g), piece (100g),
               small/large portion (100g / 250g), bowl (300g),
               large bowl (450g), plate (400g)

  Specific:    steak/fillet (180g), chicken breast (150g), thigh (120g),
               egg (60g), medium fruit/apple/kiwi/pear (110g),
               scoop (30g), bar (45g), rasher (25g)

  Number words: one, two, three, four, five, six, half, quarter


--------------------------------------------------------------------------------
FOOD DATABASE
--------------------------------------------------------------------------------
The app includes 250+ pre-loaded foods covering:
  - Portuguese staples (bacalhau, chouriço, pastéis de nata, caldo verde…)
  - UK and general European foods (bread, pasta, dairy, meats, fish…)
  - Fruits and vegetables
  - Drinks (alcoholic and non-alcoholic, hot and cold)
  - Protein supplements

Values are per 100g or 100ml and are consistent with guidelines from
Portuguese health authorities, UK NHS nutritional data, EU food labelling
directives, and WHO nutritional recommendations.

Users can add unlimited custom foods (e.g. specific branded products).
Custom foods are stored locally and persist across sessions.


--------------------------------------------------------------------------------
DATA BACKUP & RESTORE
--------------------------------------------------------------------------------
All data lives in your phone's browser localStorage, tied to the app URL.
To protect your data:

  EXPORT (Download data):
    Go to History tab → tap "Download data"
    A file named balance-backup-YYYY-MM-DD.json is saved to your Downloads.

  IMPORT (Upload data):
    Go to History tab → tap "Upload data"
    Select your backup .json file → confirm → all data is restored.

  IMPORTANT: When clearing Chrome cache, only clear "Cached images and files".
  Never clear "Cookies and site data" unless you have a backup — this will
  erase all your logs.


--------------------------------------------------------------------------------
CHANGELOG
--------------------------------------------------------------------------------

v1.0
  - Initial app: kcal and protein tracking, meal logging, activity log,
    food database (250+ foods), custom food addition, daily goals,
    7-day history chart, PWA installable on Android and iPhone.

v1.x — v9
  - Multiple bug fixes for food selection on Android (touch events)
  - Food search rebuilt with tappable button list (no dropdown)
  - Custom foods made selectable identically to default foods
  - Date navigation added to log and review any past day
  - Delete buttons added to food log entries and activities
  - Edit and delete for custom foods in Foods tab
  - 7-day averages added to History tab
  - "Burned" corrected to "Burnt" throughout
  - Add to log button made sticky above keyboard
  - Portion Helper added (natural language quantity estimation)
  - Version number displayed under app name for update confirmation
  - Service worker cache versioning for reliable updates

v10
  - All autocomplete/autofill attributes added to inputs to reduce
    Chrome autofill toolbar appearance
  - Edit food button made larger and more visible (navy blue)

v11
  - visualViewport keyboard detection: Add to log button shifts up
    automatically when keyboard is open to stay visible above Chrome bar

v12
  - "Describe your portion" now immediately focuses the input and keeps
    keyboard open — no need to tap twice

v13
  - Export / Import data feature added to History tab
  - Download all logs as balance-backup-YYYY-MM-DD.json
  - Restore from backup with file picker and confirmation prompt

v13.1
  - Download/Upload buttons moved to between chart and Daily breakdown
  - Kcal eaten colour changed to red, Kcal burnt to green (chart, legend,
    averages, daily bars)
  - Net kcal on Today tab: green = deficit, red = surplus
  - Swipe left/right gesture added for tab navigation

v13.2
  - Date navigator hidden on History and Foods tabs
  - Date navigator only visible on Today tab

v13.3
  - README.txt created documenting app name, version, objective,
    structure, how each part works, portion helper reference,
    food database info, backup instructions, and full changelog.
    README will be updated with every future change.


v13.4
  - Average kcal eaten card colour fixed to red, average kcal burnt to green
  - Activity log modal: Cancel/Save buttons now shift above Chrome autofill bar
    when keyboard is open (same fix as meal logging modal)
  - kb-open keyboard detection extended to all modals (activity, add food,
    edit food, settings)
  - Smooth slide animation when switching tabs — screens now visually drag
    left or right instead of snapping instantly


v13.5
  - Keyboard offset increased from 60px to 80px to fully clear Chrome autofill bar
  - modal-actions class (sticky button row) now applied to ALL modals:
    meal logging, activity, add food, edit food, settings
  - Cancel/Save/Add buttons now always visible above keyboard in every modal


v13.6
  - Timezone fix: new day rolls over at midnight London/Lisbon time
    (Europe/London timezone used throughout)
  - Protein card colour logic updated: amber below 80%, blue 80-100%,
    green when protein goal exceeded
  - Daily breakdown order fixed: Kcal eaten (red), Kcal burnt (green),
    Protein (blue) — in that order for every day in History tab
  - "Burned" corrected to "Burnt" in daily breakdown
  - Add food (Foods tab): input auto-focuses when modal opens so
    keyboard stays up immediately
  - Food search list height constrained so bottom anchor ("Not listed?
    / Add it / Cancel") always visible above keyboard


v13.7
  - History chart upgraded to dual axis: left axis = kcal (red/green bars),
    right axis = protein (blue bars, labelled in grams). Protein scale set so
    200g aligns visually with 3500 kcal, making the blue bar clearly readable.
  - Daily breakdown: date tag now shows net balance (consumed minus burnt)
    instead of raw kcal eaten. Green tag (deficit) if burnt >= eaten,
    red tag (surplus) if eaten > burnt. "No data" tag for empty days.


v13.8
  - Two measurement logging buttons added at bottom of History tab:
    "Stomach perimeter" (cm) and "Weight" (kg)
  - Tapping either opens a modal with a value input and a date picker
    (defaults to today, scroll back to any past date)
  - Shows last 3 readings for context before saving
  - Measurements saved independently per date; re-logging same date
    overwrites previous entry
  - History chart updated: weight (dark yellow line) and waist (purple
    line) appear as overlays when data exists, with dot markers and
    tooltip labels. Lines only shown when at least one reading exists
    in the 7-day window.
  - Legend updated to include weight and waist indicators
  - Chart threshold lines (2000 kcal red, 2500 kcal green) noted for
    next version


v13.9
  - Chart threshold lines added: dashed dark red line at 2000 kcal (max
    intake target), dashed dark green line at 2500 kcal (min burn target).
    Both values highlighted in matching colours on left axis.
  - Weight and waist axis scales hidden — lines and dots only, no labels.
    Weight axis range: 75-90 kg. Waist axis range: 80-100 cm.
    Both scaled so changes are clearly visible without crowding the chart.
  - Weight and waist lines made thinner (width 1 instead of 2).
  - Legend condensed to single line: smaller font, shorter labels
    (Eaten, Burnt, Protein, Weight, Waist, 2000, 2500), no wrapping.

================================================================================
END OF README
================================================================================
