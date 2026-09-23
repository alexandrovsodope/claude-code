# Moeny Bot — roadmap / notes

## Done
- Траты: periods + categories drill-down (periods_v1)
- Валюта (MDL/EUR/USD/RUB/UAH/VND) + пересчёт (cur_v1)
- Бюджет на месяц (bud_v1), backup BACKUP_before_budget_2026-09-23.json
- Групповой кошелёк, этап 1 (grp/grp_v1.json), deployed 2026-09-23 17:36,
  backup BACKUP_before_group_2026-09-23.json.
  Includes: /start in group, «Я в деле», text/voice expense via Gemini, card with ↩️ undo (author only),
  balance + minimal transfers, «вернул» settle buttons. Personal routes gated to private chats.
  Harness: temp scenario 7566511 (grp/harness.py, harness2.py) — DEBUG output to Expenses.
- Групповой кошелёк, этап 2 (grp/grp_v2.json, build_grp2.py), deployed 2026-09-23 18:17,
  backup BACKUP_before_group2_2026-09-23.json (= grp_v1).
  Adds: greeting for new members (G9), g_home menu re-render (G10), «💱 Валюта» picker g_cur (G11) +
  gc:CODE conversion of ledger & expenses via er-api (G12), share sync → personal Expenses
  (RawInput grp:{expId}, converted to personal currency, category from Gemini), undo deletes personal copies.
  Harness: harness3.py/harness4.py. Test data (-100777, DEBUG) cleaned.
  NOT done: backfill of group expenses made before v2 into personal Траты (offered to user).
- Групповой кошелёк, этап 3 (grp/grp_v3.json, build_grp3.py), deployed 2026-09-23 19:12,
  backup BACKUP_before_group3_2026-09-23.json (= grp_v2). Several expenses in one message/voice:
  Gemini returns expenses[] (object array), iterator 2033 (text) / 2055 (voice) → one card per expense;
  prompt deleted only on first bundle (__IMTINDEX__=1). Harness: harness5.py (verified 2 expenses + chatter).
- Групповой кошелёк, этап 4 (grp/grp_v4.json, build_grp4.py) — CURRENT LIVE, deployed 2026-09-23 22:32,
  backup BACKUP_before_group4_2026-09-23.json (= grp_v3). G13 (2210/2211): left_chat_member (not bot) → delete
  their GroupMembers row silently; debts stay in ledger. Harness6 verified (bot-left ignored).

## Next (group wallet, later stages) — user asked to REMEMBER (credits may run out)
- 🧾 PRIORITY: receipt split in group. Plan agreed-in-principle (not built yet):
  1) photo of receipt in group (reply to bot / «🧾 Чек» button) → Gemini parses items {name, price, qty} + service/tips.
  2) bot posts receipt card; each item is a button, people tap what they had (toggle, shows names);
     shared item tapped by several → split among them. New table GroupReceiptItems (ReceiptID, ChatID, Name, Price, Eaters, Payer).
  3) USER DECISION: after photo the bot ASKS each time: «💳 Платил один» (payer switchable) OR «🙋 Каждый за себя»;
     in «каждый за себя» a person can press «Я закрыл за…» and pick someone's item → only those items create debts.
  4) «✅ Закрыть чек» → GroupExpenses + GroupLedger rows (Net = paid − eaten; USER DECISION: ignore tips/service) + personal sync.
  Cost note: each tap = one Make run (~5–10 ops).
- Buttons to fix participants / payer on the card, unequal shares
- Reminders, weekly summary
- Invite-link 1-on-1 wallet (postponed by user)

## Later (user asked to remember) — mascot: DO NOT build/deploy anything; user wants only PROMPTS later
- Red panda mascot. User liked variant 2 (soft 3D, Pixar/claymation). Prompt used (Nano Banana Pro, 1:1):
  "Mascot character design for a personal finance Telegram bot called 'Moeny Bot'. A cute friendly red panda
  (Ailurus fulgens): rust-orange fur, white face markings and ear tips, dark reddish-brown legs and belly, big fluffy
  ringed tail with alternating orange and darker rings, expressive warm eyes, gentle smile. Full body, standing, front
  three-quarter view, centered, pure plain white background, no text, no logo, no watermark, no frame. Style: soft 3D
  render like a Pixar / claymation toy, subsurface fluffy fur, soft studio lighting, subtle shadow on the ground, chibi
  proportions with a big head. The panda holds a small green wallet and looks happy."
- Next mascot step (only when user asks): turnaround prompts (front/side/back/3-4) using that image as reference,
  then poses per section (Траты, Долги, Настройки, группа). Note: Telegram buttons can't hold images.
