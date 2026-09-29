# SMALLFISH Public Signal Bot — V10

Signal-only MEXC futures scanner. No orders are placed.

## Base signal
The existing strategy logic remains unchanged. When a final LONG/SHORT signal is sent, the bot stores:
- original Entry
- original SL
- original TP

## 48% two-way reversal tracker
After a final base signal, the bot continuously checks the live MEXC contract ticker in both directions.

For a LONG:
- TP-side trigger = Entry + 48% × (TP - Entry)
- SL-side trigger = Entry - 48% × (Entry - SL)
- whichever threshold is reached first triggers one SHORT reversal message.

For a SHORT:
- TP-side trigger = Entry - 48% × (Entry - TP)
- SL-side trigger = Entry + 48% × (SL - Entry)
- whichever threshold is reached first triggers one LONG reversal message.

## Reversal targets
The reversal message uses the original signal levels:
- TP1 = original Entry
- TP2 = original SL
- SL = original TP

The reversal is sent once per tracked base signal. The tracker is in process memory, so a Railway restart clears active trackers.

## Telegram
Set `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` as Railway variables. Do not hardcode bot tokens in the repository.

Telegram `sendMessage` supports `chat_id` and text messages; the bot uses that API for alerts.
