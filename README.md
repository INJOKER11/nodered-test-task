# Telegram Bot — RedBot / Node-RED

A Ukrainian-language Telegram bot with a calculator and official NBU exchange rates. Built with visual Node-RED flows and JavaScript Function nodes; RedBot package: `node-red-contrib-chatbot@2.0.5`.

## How to run

1. Install Node.js and npm compatible with your Node-RED version. Install and start Node-RED:

   ```sh
   npm install -g node-red
   node-red
   ```

2. Open `http://localhost:1880`. In **Menu → Manage palette → Install**, install `node-red-contrib-chatbot` (the exported flow uses version `2.0.5`). Restart Node-RED if prompted.
3. Create a Telegram bot using `@BotFather` and obtain its token.
4. In Node-RED, select **Menu → Import**, choose `flows.json`, and import the flow.
5. After importing flows.json, open Telegram Receiver → Bot configuration (development) and paste the token obtained from @BotFather into the Token field. Select the same bot configuration in Telegram Sender.
6. Click **Deploy**, keep Node-RED running, and send `/start` to your bot. Run only one polling instance for the same token.

The token is not included in the export. Do not commit or share your real token.

## What's implemented

- Inline menu: calculator, exchange rates, and bot information; Back buttons, `/start`, `/menu`, and fallback for unknown input.
- JavaScript calculator for two numbers, e.g. `12 + 7` or `5,5 * 2`. Supports `+`, `-`, `*`, `/`, negative numbers, and decimal fractions. Validates nonnumeric, empty, non-text, and overlong input (100 characters), division by zero, and non-finite results.
- USD and EUR rates in UAH, with dates from the NBU API: `https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json`. Invalid responses and network failures produce a friendly error message.
- Logs contain a timestamp, chat ID, step, and summary. See `logs.md` for calculation, invalid-input, and API-failure examples.
- Bonuses: simple Ukrainian phrase recognition (`скільки буде 5 плюс 7`, `який курс долара`), a Postman collection with three requests, and ping/tracert diagnostics.

## Known issues / not done

- Phrase recognition uses fixed patterns, not general natural-language understanding. The calculator supports one operation at a time and displays up to 12 significant digits.
- Telegram cannot send an empty text message. This case was tested by temporarily replacing the content of a real incoming message with an empty string; the test node was then removed.
- During development, an API network error reached both Catch and the normal HTTP output, causing duplicate replies. The response handler now skips nonnumeric HTTP statuses so Catch handles the network error once; the repeated test passed.

## Time spent

2 hours 10 minutes.

## Checklist

| Item | Status | Comment |
|---|---|---|
| RedBot deployed; bot responds to /start | ✅ | Telegram polling |
| Three-item menu and Back button | ✅ | Inline buttons |
| Fallback for unknown input | ✅ | Explanation and menu |
| Correct calculator results | ✅ | JavaScript Function node |
| Nonnumeric input validation | ✅ | Clear error and retry |
| Division by zero validation | ✅ | Clear error and retry |
| Empty / overlong input validation | ✅ | Empty-input check and 100-character limit |
| NBU exchange rates | ✅ | USD and EUR with dates |
| API unavailability handling | ✅ | Tested with an invalid hostname |
| Successful-scenario logs | ✅ | Included in logs.md |
| Invalid-input logs | ✅ | Included in logs.md |
| API-failure logs | ✅ | Included in logs.md |
| English README | ✅ | This file |

Status: ✅ Done · 🟡 Partial · ❌ Not done
