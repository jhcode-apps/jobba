# Jobba — Play Store Release Notes

Source of truth for the "What's new" release notes pasted into the Play Console
per release (Release → the track → release → Release notes). Update here first,
then paste into the Console.

**Limit:** max 500 characters per language. Plain text; `•` bullets render fine.
Both English and Swedish are required (the app ships en + sv). In the raw Console
format, wrap each block in its locale tag, e.g. `<en-US>…</en-US>`.

Swedish glossary: use *registrera* / *tidsregistrering* (not *spåra*) and
*arbetspass* (not *pass*).

---

## 1.6.0 — settings you can scan, navigation that floats

The shell rebuilt. Settings was an accordion of seven flat rows, one of which
unfolded and pushed the rest down; nothing told you what a setting was set to
until you opened it. Each section is a screen of its own now, reached from rows
grouped into cards, and every row carries its current value underneath.

The bottom bar became a pill floating over the content, naming only the tab you
are on. Because it draws over rather than reserving space, every scrolling
surface had to be taught to leave room for it — otherwise the last session of a
day sits under it with nowhere to scroll.

Underneath both, one card colour. Cards had drifted apart: the settings groups
were white while sessions and weeks were a step off the light ramp, and the
lunch row resolved to within 1.01:1 of the light background — not faint,
absent. They now share a single role, the surface furthest from the ground in
each theme, and the whole app reads at more contrast than before.

One bug worth naming: no tab had ever been highlighted in a release build. The
bar compared the route against a class name that R8 renames when it minifies,
while the route kept the name baked in at compile time, so the comparison could
never hold. Debug builds are not minified, which is why it survived this long.

### English (`en-US`)

```
What's new in 1.6.0

• Settings is now a list you can scan. Each section opens on its own screen and shows what it is set to.
• The navigation bar floats above the content and names the tab you are on.
• Cards stand off the background properly in both light and dark, and a day's hours line up in a column of their own.
• Labels open straight into the list instead of behind a button.
• Fixed: no tab was ever shown as selected.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.6.0

• Inställningar är nu en lista att överblicka. Varje avsnitt öppnas på en egen skärm och visar vad det är inställt på.
• Navigeringsraden svävar ovanför innehållet och visar vilken flik du är på.
• Korten syns tydligt mot bakgrunden i både ljust och mörkt läge, och dagens timmar står i en egen kolumn.
• Etiketter öppnas direkt i listan i stället för bakom en knapp.
• Rättat: ingen flik visades som vald.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.5.4 — backups run when you asked

The scheduled backup was arriving late, and on an upgraded install it kept
arriving late no matter what the settings said.

Two causes. The daily backup was a WorkManager PeriodicWorkRequest, whose flex
interval defaults to the whole repeat interval — so a one-day period let the OS
run the job anywhere inside those 24 hours, and the repeat interval being
elapsed time rather than wall-clock meant every DST change slid it an hour
further and nothing re-anchored it. Measured on a real device, a backup set for
22:00 had settled at 22:52.

The second cause is why the first fix was not enough on its own: the old
periodic request survives an app update inside WorkManager's database, and the
app start path keeps pending work rather than replacing it. So existing
installs — everyone who actually had the problem — stayed on the old schedule.
The new one-time chain now runs under its own work name and retires the old one.

What remains is Doze: the same run landed at 22:07 rather than 22:00. That
cannot be pinned without an exact alarm, which Play policy restricts to alarm
clocks, calendars and timers, so the schedule dialog says the time is
approximate instead of pretending otherwise.

### English (`en-US`)

```
What's new in 1.5.4

• Scheduled backups now run at the time you set, instead of drifting later and later.
• The backup schedule now notes that the time is approximate — Android may delay a backup by a few minutes to save battery.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.5.4

• Schemalagda säkerhetskopior körs nu vid tiden du valt, i stället för att glida allt senare.
• Schemat visar nu att tiden är ungefärlig – Android kan fördröja en säkerhetskopia några minuter för att spara batteri.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.5.3 — stuck session notification, portrait lock

Two fixes found while testing 1.5.2.

The ongoing-session notification could be left posted and counting after a
session was stopped, with nothing running in the app. Cancelling ran upstream of
the collector that posts, so an emission carrying the day's updated totals could
already be queued when the session ended and land *after* the cancel, re-posting
a notification nothing was left to clear. Seen on Joel's phone: the app showed
no active session while the notification had been counting for over 19 hours.

The app also followed the device sensor and rotated into landscape, where the
single-column screens have too little vertical space. Now locked to portrait —
though Android 16 ignores that on large screens for targetSdk 36+, so tablets
and unfolded foldables still rotate.

### English (`en-US`)

```
What's new in 1.5.3

• Fixed a notification that could keep counting after you stopped a session, even with nothing running.
• The app now stays in portrait when you rotate your phone.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.5.3

• Rättade en avisering som kunde fortsätta räkna efter att du avslutat ett arbetspass, trots att ingen tidsregistrering pågick.
• Appen stannar nu i stående läge när du vrider på telefonen.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.5.2 — session notification independent of the UI

Internal release. The ongoing-session notification and the 12-hour reminder were
scheduled inside TrackerViewModel, so they only ran while the Tracker screen was
alive. They now live in a process-scoped notifier that follows the session
itself. Groundwork for a home-screen widget, which would otherwise have been
able to start a session while silently skipping both.

No new functionality; the visible effect is a more dependable notification.

### English (`en-US`)

```
What's new in 1.5.2

• More reliable session notification — the ongoing notification and the long-session reminder no longer depend on the timer screen being open.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.5.2

• Mer tillförlitlig avisering — den pågående aviseringen och påminnelsen om långa arbetspass beror inte längre på att Timer-skärmen är öppen.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.5.1 — consistent card styling

Follow-up to 1.5.0. The summary week cards used surfaceBright while the tracker
session cards take Material's default surfaceContainerHighest. Those two roles
sit close together at the light end of the palette but far apart at the dark
end, so the mismatch was invisible in light theme and obvious in dark. Week
cards now use the same role, at a small cost in contrast against the background
that still leaves them far clearer than before 1.5.0.

### English (`en-US`)

```
What's new in 1.5.1

• Visual polish — the summary and timer screens now use matching card styling.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.5.1

• Visuell putsning — Översikt och Timer använder nu samma kortutseende.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.5.0 — language, holidays, and a clearer summary

Feature release. The country setting used to decide three things at once:
which holidays apply, which date and time formats are suggested, and the
language of day and month names. That last one was unintended and meant an
English app with Sweden selected rendered "tisdag" beside English text, with
no way to separate them. Names now follow the app's language; country keeps
driving holidays and formats.

The summary screen also had a real contrast bug: in light theme the week
cards resolved to within about 1 shade of the background and were effectively
invisible. They now use a surface that stands out in both themes. Stat card
colour and the spacing between the totals row and the week list were reworked
at the same time, and a ripple that drew square corners outside the rounded
card shape on tap was fixed.

### English (`en-US`)

```
What's new in 1.5.0

• Day and month names now follow the app's language, so you can run Jobba in English and still get Swedish holidays and date formats.
• Summary redesign — week cards are properly visible in light theme, and the spacing between the totals and the weeks is clearer.
• Tidier backup settings text.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.5.0

• Namn på dagar och månader följer nu appens språk, så du kan köra Jobba på engelska och ändå få svenska helgdagar och datumformat.
• Nytt utseende i Översikt — veckokorten syns ordentligt i ljust tema och avståndet mellan totalerna och veckorna är tydligare.
• Snyggare texter i inställningarna för säkerhetskopiering.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.4.1 — safer backup setup

Bugfix release. Choosing a backup location used to run a backup the moment you
picked it. Because the picker creates or selects a *file*, and the write opens
it truncating, that could replace a backup already saved at that location with
whatever was on the device — on a fresh install, an empty database. Joel hit
exactly this: configuring backup before importing his old file. Selection now
only records the location; backups happen via "Run Backup Now" or the schedule.

Also removes the "File name" field from Settings. It only ever pre-filled the
picker and rendered itself — backups always went to the chosen document URI —
so renaming in the picker left Settings showing two different names for one
file. The location row already shows the real name.

Changing location also clears the recorded last-backup fingerprint, so the
scheduled worker cannot mistake "data unchanged" for "already backed up here"
and skip writing to the new destination. Side effect: "Last backup" reads
"Never" until the first real backup to that location, which is accurate.

### English (`en-US`)

```
What's new in 1.4.1

• Choosing a backup location no longer writes a backup straight away. It could overwrite a backup file already saved there — on a fresh install, with an empty one. Back up when you mean to, with "Run Backup Now" or on your schedule.
• The backup file name is now chosen in the location picker only. The separate "File name" box is gone — it never affected where backups were actually saved.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.4.1

• Att välja plats för säkerhetskopiering skapar inte längre en kopia direkt. Det kunde skriva över en befintlig säkerhetskopia på platsen — vid nyinstallation med en tom. Säkerhetskopiera när du vill, med "Säkerhetskopiera nu" eller enligt ditt schema.
• Filnamnet väljs nu bara i platsväljaren. Den separata rutan "Filnamn" är borttagen — den påverkade ändå aldrig var säkerhetskopiorna sparades.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.4.0 — simpler deleting, themed icon

Feature release. Swipe-to-delete is gone from session cards: it duplicated the
delete button already on every card, and because the day list sits inside the
Tracker's horizontal pager, the swipe gesture was intercepting drags meant for
day navigation. Deleting is unchanged otherwise — same button, same confirm
dialog. The launcher icon gains a monochrome layer, so it participates in
Android 13+ themed icons. Counted strings now use proper plurals, fixing
"1 dagar" / "1 days" on single-day imports. Resource shrinking cuts the
download by roughly 13%.

Not called out in the notes: the battery-optimization "Fix" button now opens
the system battery-optimization list instead of the one-tap dialog, required by
Play policy. Add a bullet if you'd rather flag it.

### English (`en-US`)

```
What's new in 1.4.0

• Swiping a session no longer deletes it — use the delete button on the card. Swiping now moves between days reliably, wherever you start it.
• Themed icon — on Android 13 and later, Jobba's icon can follow your wallpaper colours.
• Correct singular wording, so imports read "1 day" instead of "1 days".
• Smaller download.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.4.0

• Att svepa på ett arbetspass tar inte längre bort det — använd papperskorgen på kortet. Svep byter nu dag på ett tillförlitligt sätt, var du än börjar.
• Temafärgad ikon — på Android 13 och senare kan Jobbas ikon följa bakgrundsbildens färger.
• Rätt singularform, så att import visar "1 dag" i stället för "1 dagar".
• Mindre nedladdning.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.3.1 — clearer backup failure alerts

Bugfix release. When a scheduled backup can't reach its saved location — the
cloud app was signed out or updated, removable storage was unmounted, or the
file/permission was removed — Jobba previously showed a cryptic "No content
provider" error that gave no hint what to do. It now shows a plain-language
notification and taps straight into Settings so you can re-select where backups
are saved.

### English (`en-US`)

```
What's new in 1.3.1

• Clearer backup errors — if a scheduled backup can't reach its saved location (for example, the cloud app was signed out), Jobba now shows a plain-language notification and taps straight to Settings so you can re-select where backups are saved.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.3.1

• Tydligare fel vid säkerhetskopiering — om en schemalagd säkerhetskopia inte når sin sparade plats (t.ex. om molnappen loggats ut) visar Jobba nu en avisering i klartext och tar dig direkt till Inställningar så att du kan välja var säkerhetskopior sparas igen.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.3.0 — smarter automatic backups

The user-facing change since 1.2.1 is activity-gated scheduled backups: an
automatic backup now writes only when your data has actually changed since the
last one, so idle days stop overwriting the backup file (and stop triggering
needless cloud syncs). The "Last backup" settings row is relabeled "Backup up
to date" to match, and stays fresh even on days nothing is written.

### English (`en-US`)

```
What's new in 1.3.0

• Smarter automatic backups — a scheduled backup now saves only when your data has changed since the last one, so idle days no longer overwrite your backup file. Settings shows when your backup is up to date.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.3.0

• Smartare automatiska säkerhetskopior — en schemalagd säkerhetskopia sparas nu bara när dina data har ändrats sedan förra gången, så lediga dagar skriver inte längre över din säkerhetskopia. Inställningarna visar när säkerhetskopian är aktuell.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.2.1 — Android 15 compatibility

No user-facing changes. Bumps androidx.activity so edge-to-edge no longer
uses APIs deprecated on Android 15 (API 35), clearing two Play Console
warnings. Notes kept minimal since testers see nothing different.

### English (`en-US`)

```
What's new in 1.2.1

• Under-the-hood fixes so Jobba displays correctly on Android 15 and newer. No changes to how the app works.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.2.1

• Fixar under huven så att Jobba visas korrekt på Android 15 och senare. Inga ändringar i hur appen fungerar.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.2.0 — notes at a glance

The user-facing change since 1.1.1 is the Summary notes indicator: days with a
day or session note now show a note icon. Also targets Android 16 (API 36) to
meet Play's target-API requirement — noted briefly for testers, not detailed.

### English (`en-US`)

```
What's new in 1.2.0

• Spot notes at a glance — the Summary now shows a note icon on any day that has a day note or a session note, so you can find them without opening each day.

Also updated for the latest Android version.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.2.0

• Se anteckningar direkt — Översikten visar nu en anteckningsikon på dagar med en dags- eller arbetspassanteckning, så att du hittar dem utan att öppna varje dag.

Även uppdaterad för den senaste Android-versionen.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.1.1 — tracker polish

Small UI refinements since 1.1.0: a compact "today" chip and a slimmer timer card.

### English (`en-US`)

```
What's new in 1.1.1

• A quicker way back to today — a compact "Today" button now appears when you're viewing another day.
• Tidier timer view — a slimmer card so more of your sessions fit on screen.

Thanks for testing! Please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.1.1

• Snabbare väg tillbaka till idag — en kompakt "Idag"-knapp visas nu när du tittar på en annan dag.
• Renare timervy — ett smalare kort så att fler arbetspass får plats på skärmen.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.1.0 — new teal look

Only user-facing change since 1.0.0 is the teal light/dark color scheme.

### English (`en-US`)

```
What's new in 1.1.0

• A fresh new look — Jobba now wears a teal theme built around the app icon, in both light and dark mode.

Thanks for testing! As always, please report anything that looks off.
```

### Swedish (`sv-SE`)

```
Nyheter i 1.1.0

• Ett fräscht nytt utseende — Jobba har fått ett blågrönt tema baserat på appikonen, i både ljust och mörkt läge.

Tack för att du testar! Rapportera gärna om något ser fel ut.
```

---

## 1.0.0 — closed testing debut

First build on Play. Notes introduce the app rather than list changes, since
testers have no prior version.

### English (`en-US`)

```
Welcome to the Jobba closed test — thanks for testing!

Jobba is a fast, fully offline work-hours tracker:
• One-tap start/stop with a live notification timer
• Multiple sessions per day, manual add/edit
• Automatic lunch deduction
• Weekly & monthly summaries with public holidays
• CSV/JSON/Markdown export and scheduled backups

No ads, no account, no internet. Please report any bugs or rough edges you find.
```

### Swedish (`sv-SE`)

```
Välkommen till Jobbas slutna test — tack för att du testar!

Jobba är en snabb, helt offline tidsregistrering:
• Starta/stoppa med ett tryck, med aktiv timer i notiserna
• Flera arbetspass per dag, lägg till/redigera manuellt
• Automatiskt lunchavdrag
• Vecko- och månadssammanfattningar med helgdagar
• Export till CSV/JSON/Markdown och schemalagda säkerhetskopior

Inga annonser, inget konto, inget internet. Rapportera gärna buggar du hittar.
```
