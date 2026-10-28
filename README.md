# PTO Calculator

A PTO optimizer that places your vacation days around US holidays and weekends to get the most time off, while respecting the days you can't miss and how you like your time off spread across the year.

It's a single HTML file. There's no build step, no server and no account. Your settings stay in your own browser.

## Using it

Open `index.html` in a browser, or use the GitHub Pages site if it's enabled for this repo.

1. **PTO days.** Enter your full allotment and how many days you want to keep in reserve. The plan only spends what's left after the reserve and any days you've pre-booked.
2. **Year.** Plan the current year or either of the next two. For the current year, you can skip dates that have already passed.
3. **Vacation style.** Choose how your days are grouped. This section also has three preferences: Friday or Monday for 3-day weekends, whether to always take the last week of the year off, and whether to leave holiday weekends at 3 days.
4. **Regular days off.** Change this if your work week isn't Monday to Friday.
5. **Months you'll book yourself.** The planner won't place PTO in these months. January and December are selected by default.
6. **Short breaks only.** In these months the planner allows 3- and 4-day weekends but nothing longer. June, July and August are selected by default.
7. **Holidays you get.** All US federal holidays, with weekend dates moved to the observed Friday or Monday. Turn off the ones your employer doesn't give. Day after Thanksgiving, Christmas Eve and New Year's Eve are listed as optional extras.

Then mark days on the calendar. Choose what a click marks (**Blackout**, **Pre-booked PTO** or **Company day off**) and click a day. Click it again to clear it. Shift-click a second day to mark the whole range between the two.

The plan recalculates after every change. If a rule can't be met, a warning appears above the calendar and stays in view while you scroll.

### Vacation styles

| Style | What it plans |
|---|---|
| Long weekends | 3–4 day weekends, with at least one in every month |
| Balanced mix | A week-long break or two plus a 3-day weekend in every month |
| Week-long breaks | 7–10 day breaks |
| Extended vacations | 10–16 day breaks |

## Planning rules

The page doesn't list these rules, so this is the reference for how the planner decides. They apply to every style unless noted.

- **Breaks are at least 3 days and include a weekend.** PTO never lands on a lone midweek day.
- **Breaks are a long weekend (3–4 days) or a full week (7+ days).** A 5–6 day break would always split two work weeks, so the planner doesn't make them.
- **One ordinary weekend between breaks.** You never get two long weekends in a row.
- **No more than 4 weeks without a 3-day weekend.** Holiday weekends and your own bookings count toward this.
- **A 3-day weekend in every month** for Long weekends and Balanced mix. A weekend that crosses a month boundary counts for both months.
- **One longer break (5+ days) per quarter at most.** Your own bookings of 5+ days count toward their quarter.
- **No longer breaks in short-breaks-only months.** That includes a break that only crosses into one of those months.
- **Breaks fill out the week.** A break never leaves one or two workdays stranded between it and a weekend or holiday.
- **Your bookings are left alone.** The planner never adds days next to your pre-booked days.
- **January 1–7 and December 25–31 are never booked automatically.** The year-end toggle is the one exception.
- **Leftover PTO stays unplaced.** If some days don't fit within these rules, the warning says how many. You can place them yourself or keep them in reserve.

## What it shows

- **Stats:** total days off in breaks, PTO used and kept in reserve, days off per PTO day, number of breaks, blackout days, and full, 4-day and 3-day work weeks.
- **Year at a glance:** a color-coded calendar of PTO, holidays, blackouts, pre-booked days and company days off. This is where you mark days. "Download calendar file" saves the plan as a `.ics` file to import into a calendar app.
- **Your breaks:** each break with its dates, length, the exact days to take and any holidays it includes. "Download dates" saves a plain list for a time-off request.
- **Monthly coverage:** a strip above the breaks showing how each month gets its 3-day weekend (Long weekends and Balanced mix only).

## How the optimizer works

1. It builds the year day by day and labels each day: weekend, holiday, company day off, pre-booked, blackout or workday.
2. It lists every possible break: a stretch of days off containing at least one PTO day, bounded by workdays on both sides.
3. It filters that list through the rules above.
4. It sets aside the PTO needed for the 4-week rhythm and the monthly 3-day weekends.
5. It picks the breaks that give the most days off for the remaining PTO, using dynamic programming with your vacation style as a constraint.
6. It moves plain weekends (ones not tied to a holiday) so they spread evenly across the year.

When it reports the cost of your blackouts, it runs the plan a second time without them and compares the totals.

## Your data

Settings and marked days are saved in your browser's `localStorage` on the device you use. Nothing is sent anywhere. Clearing site data or switching browsers starts you fresh.

## Limits

- Only US federal holidays are built in. Add others as company days off, or edit the `HOLIDAYS` list in `index.html`.
- The plan is a suggestion. Check it against your employer's PTO policy before you request time off.

## Development

Everything is in `index.html`: the markup, the styles and the script. The planning logic sits between the `// LOGIC START` and `// LOGIC END` comments and doesn't depend on the page, so you can run it under Node to test it.

The only external resource is Google Fonts. If it can't load, the page falls back to system fonts.
