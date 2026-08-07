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
