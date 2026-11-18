# Zone2

**AI running coach in Telegram, works with Garmin Connect.**

Zone2 helps runners follow a training plan and adapt it to how training is actually going. It reads completed activities from Garmin Connect, compares them with the plan and the runner's goal, and proposes next week's workouts. Once the runner confirms, Zone2 schedules them as structured workouts in Garmin Connect, so they sync to the watch.

> Status: early development (proof of concept). Built and tested by a single runner, with a small group of beta users planned.

---

## How it works

1. **Connect.** The user opens the Zone2 bot in Telegram and links their Garmin Connect account through Garmin's OAuth consent screen. Zone2 never sees or stores the Garmin password.
2. **Import a plan.** The user forwards a plan from a human coach in plain text, for example *"10×500 m at 4:30–4:40/km, 200 m easy jog between"*. Zone2 turns it into structured workouts (warm-up, intervals, recovery, cool-down, pace and heart-rate targets).
3. **Review.** Zone2 shows a preview of every workout. **Nothing is written to Garmin Connect until the user taps "Upload".**
4. **Train.** The workouts appear on the Garmin Connect calendar and sync to the watch.
5. **Analyze and adapt.** After the runs, Zone2 reads the completed activities (distance, duration, pace, heart rate, laps) and compares them with the plan and the user's goal (for example a sub-50 min 10 km). It proposes next week's plan, and the user reviews and confirms it as in step 3.

## Garmin Connect integration

| API | Access | Used for |
|---|---|---|
| **Training API** | Write | Create structured running workouts and schedule them on the user's calendar |
| **Activity API** | Read | Read completed activity summaries and laps to analyze performance against the plan |

- Authorization via Garmin's OAuth flow; the user can revoke access at any time in Garmin Connect.
- Only the data listed above is requested.
- Workouts are created only after explicit user confirmation in the bot.
- Deregistration and user-permission-change notifications from Garmin are handled. When access is revoked, the user's tokens and stored Garmin data are deleted.

## Data and privacy

- **Purpose limitation.** Garmin data is used only to provide coaching to the same user. It is never sold, shared with third parties for marketing, or used for advertising.
- **AI processing.** To analyze training and generate workouts, the relevant activity data and plan text are sent to Anthropic's Claude API. Under Anthropic's commercial API terms, inputs are not used for model training by default. No other AI provider is used.
- **Storage.** Stored data is limited to OAuth tokens, the user's goal, plans and workout previews, and the activity summaries needed for analysis. It is kept in the bot's database on Telegram's serverless platform.
- **User control.** `/disconnect` revokes Garmin access and deletes all of the user's data. `/export` returns the user's stored data.
- **Minimal identity.** Zone2 stores the Telegram user ID only, no name, email or phone number.

## Architecture

```
Telegram  ──►  Zone2 bot (Telegram Serverless, JavaScript)
                 ├── Claude API      parse plans, analyze training, propose workouts
                 ├── Garmin Activity API   read completed activities
                 └── Garmin Training API   create + schedule workouts (after user confirmation)
```

## Roadmap

- [ ] Import a coach's text plan → structured workouts → Garmin Connect calendar
- [ ] Read completed activities and show plan vs actual
- [ ] Weekly adaptive plan based on performance and goal
- [ ] `/undo` and editing of scheduled workouts

## Contact

Developer: `<your name>` · `<contact email>` · `<website or GitHub profile>`

---

*Garmin, Garmin Connect and the Garmin logo are trademarks of Garmin Ltd. or its subsidiaries. Zone2 is an independent project and is not affiliated with or endorsed by Garmin.*
