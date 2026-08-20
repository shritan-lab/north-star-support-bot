# Test Results

All 204 cases below are executed by the self-test panel inside `index.html`
(footer link `Run self-test`). Each case builds a fresh session and drives it
through `handle()`, the same function the chat window calls. Nothing is stubbed
and no result is hardcoded: a failing case renders as `FAIL` in the panel.

The observed output below was captured directly from `runSelfTest()` on the
shipped file. `/` marks a paragraph break inside one bot message, and
`[square brackets]` mark a system divider line.

## Summary

```
204 of 204 checks passed
```

| # | Input | Starting state | Expected | Observed output | End state | Result |
|---|---|---|---|---|---|---|
| 1 | `where is my order` | MENU | state AWAIT_ORDER, bot asks for the order number | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 2 | `track my package` | MENU | state AWAIT_ORDER | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 3 | `wheres my stuff` | MENU | state AWAIT_ORDER | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 4 | `111` | AWAIT_ORDER | shipped response, back to MENU | Order #111 has shipped and it's arriving tomorrow. / Anything else? | `MENU` | PASS |
| 5 | `#111` | AWAIT_ORDER | shipped response | Order #111 has shipped and it's arriving tomorrow. / Anything else? | `MENU` | PASS |
| 6 | `order 222` | AWAIT_ORDER | processing response | Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days. / Anything else? | `MENU` | PASS |
| 7 | `my order number is 333` | AWAIT_ORDER | delivered plus follow up, state ORDER_333_FOLLOWUP | Order #333 was delivered. Did everything arrive in good shape? | `ORDER_333_FOLLOWUP` | PASS |
| 8 | `999` | AWAIT_ORDER | invalid order response | I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. | `AWAIT_ORDER` | PASS |
| 9 | `999 three times` | AWAIT_ORDER | third miss escalates to LIVE_AGENT | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 10 | `111` | MENU (bare number, no context) | routes into order tracking, shipped response | Order #111 has shipped and it's arriving tomorrow. / Anything else? | `MENU` | PASS |
| 11 | `what is your return policy` | MENU | 30 days, unused, original packaging, returns link | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 12 | `i want to return this` | MENU | returns response | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 13 | `wrong size` | MENU | returns response | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 14 | `what should i buy` | MENU | state REC_Q1 | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 15 | `Camping` | REC_Q1 | state REC_Q2 | Good pick. What conditions are you expecting? | `REC_Q2` | PASS |
| 16 | `Cold and snowy` | REC_Q2 | four-season tents recommendation | For camping in cold and snowy conditions, start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove. / Want me to help with anything else? | `MENU` | PASS |
| 17 | `how long does shipping take` | MENU | 3 to 5 and 1 to 2 business days | Standard shipping takes 3 to 5 business days. Expedited shipping takes 1 to 2 business days. / Anything else? | `MENU` | PASS |
| 18 | `talk to a human` | MENU | state LIVE_AGENT | Connecting you with a live agent now. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 19 | `agent` | REC_Q2 (mid flow) | state LIVE_AGENT | Sure, let's switch gears. / Connecting you with a live agent now. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 20 | `actually what is your return policy` | AWAIT_ORDER (mid slot fill) | returns response, not an invalid order error | Sure, let's switch gears. / Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 21 | `menu` | LIVE_AGENT | state MENU | [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? | `MENU` | PASS |
| 22 | `asdkjhasd` | MENU | first fallback, stuckCount 1 | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 23 | `asdkjhasd three times` | MENU | third miss escalates to LIVE_AGENT | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 24 | `hello after a completed flow and one miss` | MENU | greeting, stuckCount back to 0 | Hey. What can I help you with? | `MENU` | PASS |
| 25 | `track order 111` | MENU | shipped response directly, AWAIT_ORDER never entered | Order #111 has shipped and it's arriving tomorrow. / Anything else? | `MENU` | PASS |
| 26 | `where is order 222` | MENU | processing response directly | Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days. / Anything else? | `MENU` | PASS |
| 27 | `555` | MENU (bare invalid number) | invalid order response | I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. | `AWAIT_ORDER` | PASS |
| 28 | `exchange` | MENU | returns and exchanges response | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 29 | `Winter sports` | MENU | skips ahead to REC_Q2 with recActivity set to winter sports | Good pick. What conditions are you expecting? | `REC_Q2` | PASS |
| 30 | `Camping` | MENU | state REC_Q2, recActivity camping | Good pick. What conditions are you expecting? | `REC_Q2` | PASS |
| 31 | `cold and snowy` | MENU | recommendation flow with recCondition pre-filled | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 32 | `I need a jacket` | MENU | state REC_Q1, no slot pre-filled | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 33 | `no` | AWAIT_ORDER | re-prompt for the number, counted as one miss, no escalation | I'll need your order number to look that up. You can also type menu to go back. | `AWAIT_ORDER` | PASS |
| 34 | `my item was crushed` | MENU | returns and exchanges response | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 35 | `my item was crushed` | AWAIT_ORDER | returns response as a global interrupt, invalidOrderStreak unchanged | Sure, let's switch gears. / Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 36 | `no three times` | AWAIT_ORDER | non-numeric misses now count, so the third escalates | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 37 | `999 then 888 then 777` | AWAIT_ORDER | third failed lookup escalates to LIVE_AGENT | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 38 | `what is your return policy` | LIVE_AGENT | Riley answers with the exact returns copy, stays in LIVE_AGENT | Sure, I can pull that up. / Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `LIVE_AGENT` | PASS |
| 39 | `111` | LIVE_AGENT | Riley answers with the exact shipped copy, stays in LIVE_AGENT | Sure, I can pull that up. / Order #111 has shipped and it's arriving tomorrow. / Anything else? | `LIVE_AGENT` | PASS |
| 40 | `menu` | MENU | dedicated menu reset copy, not the greeting line | Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 41 | `111 then no` | MENU | shipped response, then the closing line, not a fallback | Happy to help. Have a good one out there. | `MENU` | PASS |
| 42 | `111 then yes` | MENU | What can I help you with? with menu chips | What can I help you with? | `MENU` | PASS |
| 43 | `thanks` | MENU | courtesy response, stuckCount stays 0 | Anytime. Anything else I can help with? | `MENU` | PASS |
| 44 | `bye` | MENU | closing line, stuckCount stays 0 | Happy to help. Have a good one out there. | `MENU` | PASS |
| 45 | `asdkjhasd then thanks then asdkjhasd` | MENU | courtesy resets the streak, so the second miss does not escalate | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 46 | `333 then no` | ORDER_333_FOLLOWUP | delivery problem response, NOT the closing line | Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns / If you'd rather sort it out with a person, I can connect you with a live agent. | `MENU` | PASS |
| 47 | `help` | MENU | menu reset copy | Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 48 | `#111` | MENU | shipped response | Order #111 has shipped and it's arriving tomorrow. / Anything else? | `MENU` | PASS |
| 49 | `its already been so many days and still havent received it` | MENU | state AWAIT_ORDER, not a fallback | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 50 | `i havent received my order` | MENU | state AWAIT_ORDER | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 51 | `my order is late` | MENU | state AWAIT_ORDER | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 52 | `my order is not coming` | LIVE_AGENT | Riley gives order tracking help via handback, not the generic acknowledgment | Let me hand you back to the support bot, it'll walk you through it. / [Back with the support bot] / Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 53 | `three unrecognized messages` | LIVE_AGENT | three different acknowledgments, no two consecutive repeats | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. >> That one's outside what I can pull up here. I can help with an order lookup, returns and exchanges, or gear recommendations through the support bot. >> I'm not able to answer that one. What I can do is check an order, explain returns and exchanges, or get you gear recommendations. | `LIVE_AGENT` | PASS |
| 54 | `are you a bot` | LIVE_AGENT | honest simulated agent disclosure | Fair question. This is a simulated live agent state for this demo, so I'm scripted rather than a real person. In production this is where a human agent picks up with the full conversation history. | `LIVE_AGENT` | PASS |
| 55 | `when will they reach out (also: when will i hear back)` | LIVE_AGENT | timing response with no invented time commitment, stays in LIVE_AGENT | I don't have a specific time to give you on that, and I don't want to guess. Your conversation is logged, so whoever picks it up will have the full context. | `LIVE_AGENT` | PASS |
| 56 | `thanks` | LIVE_AGENT | courtesy response, stays in LIVE_AGENT | Anytime. Anything else I can help with? | `LIVE_AGENT` | PASS |
| 57 | `no` | REC_Q2 | cancel response, state MENU, not a generic fallback | No problem. What else can I help you with? | `MENU` | PASS |
| 58 | `asdkjhasd` | REC_Q2 | REC_Q2 reprompt with the four condition chips, state stays REC_Q2 | Sorry, I didn't understand that. Which of these is closest? | `REC_Q2` | PASS |
| 59 | `asdkjhasd three times` | REC_Q2 | third miss in the slot escalates to LIVE_AGENT | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 60 | `asdkjhasd` | REC_Q1 | REC_Q1 reprompt with the four activity chips, state stays REC_Q1 | Sorry, I didn't understand that. Which of these is closest? | `REC_Q1` | PASS |
| 61 | `asdkjhasd` | ORDER_333_FOLLOWUP | follow-up reprompt, state unchanged | Sorry, I didn't understand that. Did everything arrive in good shape? | `ORDER_333_FOLLOWUP` | PASS |
| 62 | `menu` | REC_Q2 | global interrupt still wins over the slot reprompt | Sure, let's switch gears. / Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 63 | `what is your return policy then no` | LIVE_AGENT | agent closing line, state stays LIVE_AGENT | Happy to help. Your conversation stays logged, and you can head back to the main menu any time. | `LIVE_AGENT` | PASS |
| 64 | `999` | LIVE_AGENT | not-finding-that-order response, not the rotating acknowledgment | I'm not finding an order with that number on my end. Can you double check it? | `LIVE_AGENT` | PASS |
| 65 | `help me find gear` | LIVE_AGENT | handback line, divider, then the REC_Q1 opening, end state REC_Q1 | Let me hand you back to the support bot, it'll walk you through it. / [Back with the support bot] / Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 66 | `help me find gear then Camping` | LIVE_AGENT | the handed back flow actually runs, REC_Q2 | Good pick. What conditions are you expecting? | `REC_Q2` | PASS |
| 67 | `i want to track my order` | LIVE_AGENT | handback line, then the order prompt, end state AWAIT_ORDER | Let me hand you back to the support bot, it'll walk you through it. / [Back with the support bot] / Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 68 | `track order 111` | LIVE_AGENT | Riley answers directly, no handback, stays in LIVE_AGENT | Sure, I can pull that up. / Order #111 has shipped and it's arriving tomorrow. / Anything else? | `LIVE_AGENT` | PASS |
| 69 | `111` | LIVE_AGENT | Riley answers directly, stays in LIVE_AGENT | Sure, I can pull that up. / Order #111 has shipped and it's arriving tomorrow. / Anything else? | `LIVE_AGENT` | PASS |
| 70 | `i want to speak to human` | LIVE_AGENT | already-connected response | You're already connected with me, Riley. Go ahead. | `LIVE_AGENT` | PASS |
| 71 | `a long message containing the word thanks` | LIVE_AGENT | NOT the courtesy response | I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. | `LIVE_AGENT` | PASS |
| 72 | `thanks` | MENU | short courtesy still works | Anytime. Anything else I can help with? | `MENU` | PASS |
| 73 | `what all products you guys have` | MENU | catalog line plus the REC_Q1 question, end state REC_Q1 | We carry outdoor apparel and camping gear. / Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 74 | `what is your name` | MENU | bot identity response | I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 75 | `what is your name` | LIVE_AGENT | Riley identity response | I'm Riley, part of the North Star support team. | `LIVE_AGENT` | PASS |
| 76 | `are you a bot` | LIVE_AGENT | AGENT_META disclosure still wins over IDENTITY | Fair question. This is a simulated live agent state for this demo, so I'm scripted rather than a real person. In production this is where a human agent picks up with the full conversation history. | `LIVE_AGENT` | PASS |
| 77 | `can you send your website` | MENU | contact response offering only the returns link | I don't have a general site link to share here. For returns and exchanges you can go to northstaroutfitters.com/returns | `MENU` | PASS |
| 78 | `how much does a tent cost` | MENU | declines on price, offers a live agent, invents nothing | I don't have pricing, stock or carrier details here. I can help with order tracking, returns and exchanges, or gear recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 79 | `which carrier do you use` | LIVE_AGENT | declines on carrier, no carrier named | I don't have pricing, stock or carrier details here. I can help with order tracking, returns and exchanges, or gear recommendations, or I can connect you with a live agent. | `LIVE_AGENT` | PASS |
| 80 | `three recommendations in a row` | MENU | three different result templates, no consecutive repeat | For camping in cold and snowy conditions, start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove. >> Heading out camping in cold and snowy conditions, I'd start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove. >> For cold and snowy conditions, **four-season tents and cold-rated sleeping bags** is where I'd start. Worth adding: an insulated sleeping pad and a camp stove. | `MENU` | PASS |
| 81 | `dont connect me with live agent` | MENU | stays with the bot, does not escalate | Understood, I'll keep you here with the support bot. What can I help you with? | `MENU` | PASS |
| 82 | `do not transfer me to an agent` | MENU | stays with the bot | Understood, I'll keep you here with the support bot. What can I help you with? | `MENU` | PASS |
| 83 | `order tracking, no return policy` | MENU | order flow, not returns | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 84 | `return policy, no order tracking` | MENU | returns, not the order flow | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 85 | `no` | MENU after an anything-else question | still the closing line, not the negation response | Happy to help. Have a good one out there. | `MENU` | PASS |
| 86 | `transfer back to the bot` | LIVE_AGENT | exits to MENU | [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? | `MENU` | PASS |
| 87 | `back to support bot` | LIVE_AGENT | exits to MENU | [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? | `MENU` | PASS |
| 88 | `no you are a live agent, transfer back to the normal chatbot` | LIVE_AGENT | exits to MENU | [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? | `MENU` | PASS |
| 89 | `i want to speak to human` | LIVE_AGENT | still the already-connected response | You're already connected with me, Riley. Go ahead. | `LIVE_AGENT` | PASS |
| 90 | `123` | AWAIT_ORDER | no order number named anywhere in the response | I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. | `AWAIT_ORDER` | PASS |
| 91 | `123 repeatedly across two order flows` | AWAIT_ORDER | escalates once, and again after a genuine exit and a fresh stuck run | Happy to help. What's your order number? >> I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. >> I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? >> Happy to help. What's your order number? >> I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. >> I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> I'm not finding an order with that number on my end. Can you double check it? | `LIVE_AGENT` | PASS |
| 92 | `what can you help me with` | LIVE_AGENT | capability answer, not an acknowledgment | I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. | `LIVE_AGENT` | PASS |
| 93 | `what can you do` | MENU | capability answer | I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 94 | `hi four times` | LIVE_AGENT | greeting every time, never a case note | Hi again. What can I help with? \| Hi again. What can I help with? \| Hi again. What can I help with? \| Hi again. What can I help with? | `LIVE_AGENT` | PASS |
| 95 | `i want to buy gears` | MENU | recommendation flow, plural matched | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 96 | `looking for jackets` | MENU | recommendation flow, plural matched | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 97 | `wheres my orders` | MENU | order flow, plural matched | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 98 | `you idiot` | MENU | apology plus escalation | Sorry, I'm not making this easy. Let me get you to a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 99 | `bro this chatbot is so annoying` | MENU | apology plus escalation | Sorry, I'm not making this easy. Let me get you to a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 100 | `are you slow or what` | REC_Q1 | apology plus escalation | Sorry, I'm not making this easy. Let me get you to a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 101 | `you idiot` | LIVE_AGENT | actionable redirect, no escalation loop | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. | `LIVE_AGENT` | PASS |
| 102 | `what are the services or products you guys have` | MENU | catalog line plus REC_Q1 | We carry outdoor apparel and camping gear. / Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 103 | `what are the services or products you guys have` | LIVE_AGENT | handback plus REC_Q1 | Let me hand you back to the support bot, it'll walk you through it. / [Back with the support bot] / We carry outdoor apparel and camping gear. / Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 104 | `nevermind, i want to buy gears` | AWAIT_ORDER | recommendation flow | Sure, let's switch gears. / Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 105 | `nevermind` | REC_Q2 | cancels to MENU | Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 106 | `three unmatched messages` | LIVE_AGENT | three different actionable redirects, none mentioning case notes | I don't have that detail on hand. I can  >> That one's outside what I can pull up he >> I'm not able to answer that one. What I  | `LIVE_AGENT` | PASS |
| 107 | `dont give me the return policy again` | MENU | does not deliver the thing that was refused | No problem. What would you like to do instead? | `MENU` | PASS |
| 108 | `dont tell me about shipping just find my package` | MENU | negation stops at the pivot, order request survives | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 109 | `no bot i want human` | MENU | negation applies to the bot, not to the request for a human | Connecting you with a live agent now. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 110 | `can you cancel my order` | MENU | says it cannot change an order, does not ask for a number | I can't change an order from here. I can look up its status, go over returns and exchanges, or connect you with a live agent. | `MENU` | PASS |
| 111 | `why do you need my order number` | AWAIT_ORDER | plain re-prompt, no incoherent switch announcement | I'll need your order number to look that up. You can also type menu to go back. | `AWAIT_ORDER` | PASS |
| 112 | `i said give your exchange policy idiot` | MENU | answers the question, no escalation, no comment on tone | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 113 | `what the hell, i want your exchange policy` | LIVE_AGENT | the policy, not a redirect | Sure, I can pull that up. / Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `LIVE_AGENT` | PASS |
| 114 | `you idiot` | MENU | still escalates when there is no request in the message | Sorry, I'm not making this easy. Let me get you to a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 115 | `this is so annoying, where is my order` | MENU | order flow, no escalation | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 116 | `exchance policy` | MENU | returns and exchanges policy via typo tolerance | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 117 | `retrun poilcy` | MENU | returns and exchanges policy via typo tolerance | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 118 | `wheres my ordr` | MENU | order flow via typo tolerance | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 119 | `trak my order` | MENU | order flow | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 120 | `shippign times` | MENU | shipping response via typo tolerance | Standard shipping takes 3 to 5 business days. Expedited shipping takes 1 to 2 business days. / Anything else? | `MENU` | PASS |
| 121 | `i want to speak to riley` | MENU | connects to the live agent | Connecting you with a live agent now. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 122 | `who all are there in your team` | LIVE_AGENT | actionable redirect, not already-connected | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. | `LIVE_AGENT` | PASS |
| 123 | `i want to speak to a human` | LIVE_AGENT | still already-connected | You're already connected with me, Riley. Go ahead. | `LIVE_AGENT` | PASS |
| 124 | `nothing` | REC_Q1 | cancels to menu | No problem. What else can I help you with? | `MENU` | PASS |
| 125 | `back` | MENU | menu copy with no switch-gears prefix | Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 126 | `menu` | REC_Q2 | menu copy WITH the switch-gears prefix, a genuine interrupt | Sure, let's switch gears. / Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 127 | `Back to main menu chip mid slot fill` | AWAIT_ORDER | no switch-gears prefix on an offered chip | Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 128 | `i dont understand` | MENU | capability explanation, not a fallback | I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 129 | `seven exchange phrasings` | MENU | all return the combined returns and exchanges response | all seven returned the policy | `MENU` | PASS |
| 130 | `terrible service, when will 111 arrive` | MENU | answers the order, does not escalate on the insult | Order #111 has shipped and it's arriving tomorrow. / Anything else? | `MENU` | PASS |
| 131 | `are you hiring` | MENU | no fuzzy correction to hiking, no gear flow | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 132 | `sorry i didnt get that` | MENU | re-explains capability rather than reporting its own confusion | I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 133 | `that made no sense to me` | MENU | capability explanation | I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. | `MENU` | PASS |
| 134 | `i want to order` | MENU | asks which sense, does not demand a number | Happy to help. Are you looking up an existing order, or looking to buy something? | `MENU` | PASS |
| 135 | `wanted to order something else` | MENU | recommendation flow | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 136 | `havent ordered yet, im saying I want to order` | MENU | recommendation flow | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 137 | `i want to order what all options you have` | MENU | recommendation flow, purchase wins over lookup | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 138 | `i want to buy a tent` | MENU | recommendation flow | Let's find the right gear. What are you heading out for? | `REC_Q1` | PASS |
| 139 | `where is my order` | MENU | order flow, unchanged by the sense rule | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 140 | `track order 111` | MENU | shipped status, unchanged by the sense rule | Order #111 has shipped and it's arriving tomorrow. / Anything else? | `MENU` | PASS |
| 141 | `order number is 222` | MENU | processing status | Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days. / Anything else? | `MENU` | PASS |
| 142 | `your useless` | MENU | escalation, not the return policy | Sorry, I'm not making this easy. Let me get you to a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 143 | `this bot is annoying` | LIVE_AGENT | does NOT exit the handoff | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. | `LIVE_AGENT` | PASS |
| 144 | `you are acting like a bot` | LIVE_AGENT | does NOT exit the handoff | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. | `LIVE_AGENT` | PASS |
| 145 | `put me back with the chatbot` | LIVE_AGENT | exits to MENU | [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? | `MENU` | PASS |
| 146 | `what about 222` | LIVE_AGENT | processing status, not a redirect | Sure, I can pull that up. / Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days. / Anything else? | `LIVE_AGENT` | PASS |
| 147 | `no sorry 111` | MENU after an anything-else question | shipped status, not the closing line | Order #111 has shipped and it's arriving tomorrow. / Anything else? | `MENU` | PASS |
| 148 | `no` | MENU after an anything-else question | still the closing line | Happy to help. Have a good one out there. | `MENU` | PASS |
| 149 | `333 then no` | LIVE_AGENT | delivery problem response, stays in LIVE_AGENT | Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns / If you'd rather sort it out with a person, I can connect you with a live agent. | `LIVE_AGENT` | PASS |
| 150 | `333 then yes` | LIVE_AGENT | glad to hear it, stays in LIVE_AGENT | Glad to hear it. Anything else I can help with? | `LIVE_AGENT` | PASS |
| 151 | `not sure` | REC_Q1 | no-problem reprompt, not a fallback | No problem. Here's what most people start with, pick whichever is closest. | `REC_Q1` | PASS |
| 152 | `i dont know` | REC_Q2 | no-problem reprompt | No problem. What's the weather usually like when you head out? | `REC_Q2` | PASS |
| 153 | `you tell me` | REC_Q1 | no-problem reprompt | No problem. Here's what most people start with, pick whichever is closest. | `REC_Q1` | PASS |
| 154 | `how do i order` | MENU | asks which sense rather than demanding a number | Happy to help. Are you looking up an existing order, or looking to buy something? | `MENU` | PASS |
| 155 | `what is happening with my purchase` | MENU | purchase as a noun is a lookup, not a buy | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 156 | `why did the bot transfer me` | LIVE_AGENT | a question about the transfer does not perform one | You're already connected with me, Riley. Go ahead. | `LIVE_AGENT` | PASS |
| 157 | `123 then not sure then no idea` | AWAIT_ORDER | mixed misses accumulate, third escalates | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 158 | `not sure then 999 then dont remember` | AWAIT_ORDER | escalates on the third regardless of type | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 159 | `the live log sequence` | AWAIT_ORDER | nope I dont remember, i dont know i said, not sure escalates by the third | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 160 | `three unrelated gibberish strings` | MENU | escalates on the third | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 161 | `three gibberish answers` | REC_Q1 | escalates on the third | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 162 | `123 then 111 then 456` | AWAIT_ORDER | a successful resolution resets the count | I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. | `AWAIT_ORDER` | PASS |
| 163 | `123 then a different flow then back` | AWAIT_ORDER | a global interrupt resets the count | Sure, let's switch gears. / Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 164 | `not sure` | AWAIT_ORDER | acknowledged, offers the agent side, counts as one miss | No worries. If you can't find it, I can pull it up on the agent side instead. | `AWAIT_ORDER` | PASS |
| 165 | `stuck escalation caps per episode, not per session` | MENU | a second stuck run escalates again after a genuine exit | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 166 | `i dont like it, want to return` | ORDER_333_FOLLOWUP | delivery-problem response on the first turn | Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns / If you'd rather sort it out with a person, I can connect you with a live agent. | `MENU` | PASS |
| 167 | `i said i dont like it, want to return it` | ORDER_333_FOLLOWUP | delivery-problem response on the first turn | Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns / If you'd rather sort it out with a person, I can connect you with a live agent. | `MENU` | PASS |
| 168 | `need to return this` | ORDER_333_FOLLOWUP | delivery-problem response | Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns / If you'd rather sort it out with a person, I can connect you with a live agent. | `MENU` | PASS |
| 169 | `dont connect me with live agent` | MENU | no regression, still stays with the bot | Understood, I'll keep you here with the support bot. What can I help you with? | `MENU` | PASS |
| 170 | `dont tell me about shipping just find my package` | MENU | no regression, shipping suppressed, order runs | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 171 | `treking` | REC_Q1 | maps to hiking and backpacking | Good pick. What conditions are you expecting? | `REC_Q2` | PASS |
| 172 | `fishing` | REC_Q1 | maps to everyday outdoors | Good pick. What conditions are you expecting? | `REC_Q2` | PASS |
| 173 | `sky diving` | REC_Q1 | out-of-scope response, chips intact, not counted as a miss | That one's a bit outside what we carry, we're focused on outdoor apparel and camping gear. Want me to point you toward something in that range instead? | `REC_Q1` | PASS |
| 174 | `heat` | REC_Q2 | maps to warm and dry | For camping in warm and dry conditions, start with **three-season tents and lightweight sleeping bags**. Worth adding: camp furniture and a shade tarp. / Want me to help with anything else? | `MENU` | PASS |
| 175 | `freezing` | REC_Q2 | maps to cold and snowy | For camping in cold and snowy conditions, start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove. / Want me to help with anything else? | `MENU` | PASS |
| 176 | `all sixteen chip label combinations` | REC_Q1 | the exact chip labels still resolve unchanged | 16 distinct of 16 | `MENU` | PASS |
| 177 | `no actually tell return policy` | MENU | RETURNS on the first turn, not the negation fallback | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 178 | `no actually track my order` | MENU | order flow on the first turn | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 179 | `no wait i want returns` | MENU | RETURNS | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `MENU` | PASS |
| 180 | `no i mean shipping` | MENU | SHIPPING | Standard shipping takes 3 to 5 business days. Expedited shipping takes 1 to 2 business days. / Anything else? | `MENU` | PASS |
| 181 | `the negation fallback three times` | MENU | counts as a miss each time, escalates on the third | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 182 | `negation fallback mixed with other misses` | MENU | misses of different kinds still add up to three | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 183 | `dont connect me with live agent` | MENU | resolves, so it does not count as a miss | Understood, I'll keep you here with the support bot. What can I help you with? | `MENU` | PASS |
| 184 | `three Group A answers` | LIVE_AGENT | three different lookup prefixes, no consecutive repeat | Sure, I can pull that up. >> Let me check that for you. >> Of course, here you go. | `LIVE_AGENT` | PASS |
| 185 | `where is my <abc>?` | MENU | standard fallback, not AWAIT_ORDER | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 186 | `where is my <abc>?` | AWAIT_ORDER (mid-flow) | standard fallback, not treated as a slot answer | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 187 | `track order {123}` | MENU | standard fallback response | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 188 | `[track my package]` | MENU | standard fallback response | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 189 | `where is my order` | MENU, no junk | unchanged, enters AWAIT_ORDER normally | Happy to help. What's your order number? | `AWAIT_ORDER` | PASS |
| 190 | `999` | AWAIT_ORDER, real wrong number | unchanged, existing invalid order path, not the junk path | I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. | `AWAIT_ORDER` | PASS |
| 191 | `<script> tag, injection style` | MENU | standard fallback response, injection style input is not exempt | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 192 | `three junk messages in a row` | MENU | counts as a miss each time, escalates on the third | I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 193 | `talk to a human, then <abc>` | LIVE_AGENT | Riley's redirect, no divider, no reintro, state stays LIVE_AGENT | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. | `LIVE_AGENT` | PASS |
| 194 | `three junk messages in LIVE_AGENT` | LIVE_AGENT | three rotating redirects, no consecutive repeat, never a second connect sequence | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. >> That one's outside what I can pull up here. I can help with an order lookup, returns and exchanges, or gear recommendations through the support bot. >> I'm not able to answer that one. What I can do is check an order, explain returns and exchanges, or get you gear recommendations. | `LIVE_AGENT` | PASS |
| 195 | `junk then a real question in LIVE_AGENT` | LIVE_AGENT | Riley answers correctly, proving state was never lost | Sure, I can pull that up. / Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? | `LIVE_AGENT` | PASS |
| 196 | `MENU junk then talk to a human` | MENU | connects normally with one clean sequence | Connecting you with a live agent now. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 197 | `!!!` | LIVE_AGENT | empty-normalize input does not reset state, unlike the pre-fix bug | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. | `LIVE_AGENT` | PASS |
| 198 | `escalate, exit, escalate again` | MENU | second stuck episode escalates a second time | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 199 | `escalate, exit, only two misses` | MENU | counter genuinely resets to 0, two is not enough to escalate again | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. | `MENU` | PASS |
| 200 | `escalate, stay connected, more junk` | LIVE_AGENT | no exit means no new episode, still never a second connect sequence | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. >> That one's outside what I can pull up here. I can help with an order lookup, returns and exchanges, or gear recommendations through the support bot. >> I'm not able to answer that one. What I can do is check an order, explain returns and exchanges, or get you gear recommendations. | `LIVE_AGENT` | PASS |
| 201 | `three separate escalations in one session` | MENU | all three fire cleanly across a long session | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 202 | `explicit live agent chip after an episode` | MENU | talk to a human still works regardless of the counter | Connecting you with a live agent now. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 203 | `escalate, hand back via order lookup, escalate again` | MENU | handBackToBot is also a genuine exit, second episode escalates | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> Let me hand you back to the support bot, it'll walk you through it. / [Back with the support bot] / Happy to help. What's your order number? >> I'll need your order number to look that up. You can also type menu to go back. >> I'll need your order number to look that up. You can also type menu to go back. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |
| 204 | `escalate, hand back via gear rec, escalate again` | MENU | same fix, the product recommendation handback path | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> Let me hand you back to the support bot, it'll walk you through it. / [Back with the support bot] / Let's find the right gear. What are you heading out for? >> Sorry, I didn't understand that. Which of these is closest? >> Sorry, I didn't understand that. Which of these is closest? >> I want to make sure you get sorted out. Let me connect you with a live agent. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? | `LIVE_AGENT` | PASS |

## Raw run log

Captured by extracting the inline `<script>` from `index.html` verbatim and
calling `runSelfTest()` outside the browser. `boot()` is skipped because
`document` is undefined there, so only the conversation engine runs.

```text
SELF-TEST RESULT: 204 of 204 checks passed

[PASS] 1. input: where is my order  |  from: MENU
        expected: state AWAIT_ORDER, bot asks for the order number
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 2. input: track my package  |  from: MENU
        expected: state AWAIT_ORDER
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 3. input: wheres my stuff  |  from: MENU
        expected: state AWAIT_ORDER
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 4. input: 111  |  from: AWAIT_ORDER
        expected: shipped response, back to MENU
        end state: MENU
        output: Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 5. input: #111  |  from: AWAIT_ORDER
        expected: shipped response
        end state: MENU
        output: Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 6. input: order 222  |  from: AWAIT_ORDER
        expected: processing response
        end state: MENU
        output: Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days.
                
                Anything else?

[PASS] 7. input: my order number is 333  |  from: AWAIT_ORDER
        expected: delivered plus follow up, state ORDER_333_FOLLOWUP
        end state: ORDER_333_FOLLOWUP
        output: Order #333 was delivered. Did everything arrive in good shape?

[PASS] 8. input: 999  |  from: AWAIT_ORDER
        expected: invalid order response
        end state: AWAIT_ORDER
        output: I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up.

[PASS] 9. input: 999 three times  |  from: AWAIT_ORDER
        expected: third miss escalates to LIVE_AGENT
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 10. input: 111  |  from: MENU (bare number, no context)
        expected: routes into order tracking, shipped response
        end state: MENU
        output: Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 11. input: what is your return policy  |  from: MENU
        expected: 30 days, unused, original packaging, returns link
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 12. input: i want to return this  |  from: MENU
        expected: returns response
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 13. input: wrong size  |  from: MENU
        expected: returns response
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 14. input: what should i buy  |  from: MENU
        expected: state REC_Q1
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 15. input: Camping  |  from: REC_Q1
        expected: state REC_Q2
        end state: REC_Q2
        output: Good pick. What conditions are you expecting?

[PASS] 16. input: Cold and snowy  |  from: REC_Q2
        expected: four-season tents recommendation
        end state: MENU
        output: For camping in cold and snowy conditions, start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove.
                
                Want me to help with anything else?

[PASS] 17. input: how long does shipping take  |  from: MENU
        expected: 3 to 5 and 1 to 2 business days
        end state: MENU
        output: Standard shipping takes 3 to 5 business days. Expedited shipping takes 1 to 2 business days.
                
                Anything else?

[PASS] 18. input: talk to a human  |  from: MENU
        expected: state LIVE_AGENT
        end state: LIVE_AGENT
        output: Connecting you with a live agent now.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 19. input: agent  |  from: REC_Q2 (mid flow)
        expected: state LIVE_AGENT
        end state: LIVE_AGENT
        output: Sure, let's switch gears.
                
                Connecting you with a live agent now.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 20. input: actually what is your return policy  |  from: AWAIT_ORDER (mid slot fill)
        expected: returns response, not an invalid order error
        end state: MENU
        output: Sure, let's switch gears.
                
                Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 21. input: menu  |  from: LIVE_AGENT
        expected: state MENU
        end state: MENU
        output: [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with?

[PASS] 22. input: asdkjhasd  |  from: MENU
        expected: first fallback, stuckCount 1
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 23. input: asdkjhasd three times  |  from: MENU
        expected: third miss escalates to LIVE_AGENT
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 24. input: hello after a completed flow and one miss  |  from: MENU
        expected: greeting, stuckCount back to 0
        end state: MENU
        output: Hey. What can I help you with?

[PASS] 25. input: track order 111  |  from: MENU
        expected: shipped response directly, AWAIT_ORDER never entered
        end state: MENU
        output: Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 26. input: where is order 222  |  from: MENU
        expected: processing response directly
        end state: MENU
        output: Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days.
                
                Anything else?

[PASS] 27. input: 555  |  from: MENU (bare invalid number)
        expected: invalid order response
        end state: AWAIT_ORDER
        output: I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up.

[PASS] 28. input: exchange  |  from: MENU
        expected: returns and exchanges response
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 29. input: Winter sports  |  from: MENU
        expected: skips ahead to REC_Q2 with recActivity set to winter sports
        end state: REC_Q2
        output: Good pick. What conditions are you expecting?

[PASS] 30. input: Camping  |  from: MENU
        expected: state REC_Q2, recActivity camping
        end state: REC_Q2
        output: Good pick. What conditions are you expecting?

[PASS] 31. input: cold and snowy  |  from: MENU
        expected: recommendation flow with recCondition pre-filled
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 32. input: I need a jacket  |  from: MENU
        expected: state REC_Q1, no slot pre-filled
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 33. input: no  |  from: AWAIT_ORDER
        expected: re-prompt for the number, counted as one miss, no escalation
        end state: AWAIT_ORDER
        output: I'll need your order number to look that up. You can also type menu to go back.

[PASS] 34. input: my item was crushed  |  from: MENU
        expected: returns and exchanges response
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 35. input: my item was crushed  |  from: AWAIT_ORDER
        expected: returns response as a global interrupt, invalidOrderStreak unchanged
        end state: MENU
        output: Sure, let's switch gears.
                
                Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 36. input: no three times  |  from: AWAIT_ORDER
        expected: non-numeric misses now count, so the third escalates
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 37. input: 999 then 888 then 777  |  from: AWAIT_ORDER
        expected: third failed lookup escalates to LIVE_AGENT
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 38. input: what is your return policy  |  from: LIVE_AGENT
        expected: Riley answers with the exact returns copy, stays in LIVE_AGENT
        end state: LIVE_AGENT
        output: Sure, I can pull that up.
                
                Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 39. input: 111  |  from: LIVE_AGENT
        expected: Riley answers with the exact shipped copy, stays in LIVE_AGENT
        end state: LIVE_AGENT
        output: Sure, I can pull that up.
                
                Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 40. input: menu  |  from: MENU
        expected: dedicated menu reset copy, not the greeting line
        end state: MENU
        output: Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 41. input: 111 then no  |  from: MENU
        expected: shipped response, then the closing line, not a fallback
        end state: MENU
        output: Happy to help. Have a good one out there.

[PASS] 42. input: 111 then yes  |  from: MENU
        expected: What can I help you with? with menu chips
        end state: MENU
        output: What can I help you with?

[PASS] 43. input: thanks  |  from: MENU
        expected: courtesy response, stuckCount stays 0
        end state: MENU
        output: Anytime. Anything else I can help with?

[PASS] 44. input: bye  |  from: MENU
        expected: closing line, stuckCount stays 0
        end state: MENU
        output: Happy to help. Have a good one out there.

[PASS] 45. input: asdkjhasd then thanks then asdkjhasd  |  from: MENU
        expected: courtesy resets the streak, so the second miss does not escalate
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 46. input: 333 then no  |  from: ORDER_333_FOLLOWUP
        expected: delivery problem response, NOT the closing line
        end state: MENU
        output: Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns
                
                If you'd rather sort it out with a person, I can connect you with a live agent.

[PASS] 47. input: help  |  from: MENU
        expected: menu reset copy
        end state: MENU
        output: Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 48. input: #111  |  from: MENU
        expected: shipped response
        end state: MENU
        output: Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 49. input: its already been so many days and still havent received it  |  from: MENU
        expected: state AWAIT_ORDER, not a fallback
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 50. input: i havent received my order  |  from: MENU
        expected: state AWAIT_ORDER
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 51. input: my order is late  |  from: MENU
        expected: state AWAIT_ORDER
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 52. input: my order is not coming  |  from: LIVE_AGENT
        expected: Riley gives order tracking help via handback, not the generic acknowledgment
        end state: AWAIT_ORDER
        output: Let me hand you back to the support bot, it'll walk you through it.
                [Back with the support bot]
                Happy to help. What's your order number?

[PASS] 53. input: three unrecognized messages  |  from: LIVE_AGENT
        expected: three different acknowledgments, no two consecutive repeats
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. >> That one's outside what I can pull up here. I can help with an order lookup, returns and exchanges, or gear recommendations through the support bot. >> I'm not able to answer that one. What I can do is check an order, explain returns and exchanges, or get you gear recommendations.

[PASS] 54. input: are you a bot  |  from: LIVE_AGENT
        expected: honest simulated agent disclosure
        end state: LIVE_AGENT
        output: Fair question. This is a simulated live agent state for this demo, so I'm scripted rather than a real person. In production this is where a human agent picks up with the full conversation history.

[PASS] 55. input: when will they reach out (also: when will i hear back)  |  from: LIVE_AGENT
        expected: timing response with no invented time commitment, stays in LIVE_AGENT
        end state: LIVE_AGENT
        output: I don't have a specific time to give you on that, and I don't want to guess. Your conversation is logged, so whoever picks it up will have the full context.

[PASS] 56. input: thanks  |  from: LIVE_AGENT
        expected: courtesy response, stays in LIVE_AGENT
        end state: LIVE_AGENT
        output: Anytime. Anything else I can help with?

[PASS] 57. input: no  |  from: REC_Q2
        expected: cancel response, state MENU, not a generic fallback
        end state: MENU
        output: No problem. What else can I help you with?

[PASS] 58. input: asdkjhasd  |  from: REC_Q2
        expected: REC_Q2 reprompt with the four condition chips, state stays REC_Q2
        end state: REC_Q2
        output: Sorry, I didn't understand that. Which of these is closest?

[PASS] 59. input: asdkjhasd three times  |  from: REC_Q2
        expected: third miss in the slot escalates to LIVE_AGENT
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 60. input: asdkjhasd  |  from: REC_Q1
        expected: REC_Q1 reprompt with the four activity chips, state stays REC_Q1
        end state: REC_Q1
        output: Sorry, I didn't understand that. Which of these is closest?

[PASS] 61. input: asdkjhasd  |  from: ORDER_333_FOLLOWUP
        expected: follow-up reprompt, state unchanged
        end state: ORDER_333_FOLLOWUP
        output: Sorry, I didn't understand that. Did everything arrive in good shape?

[PASS] 62. input: menu  |  from: REC_Q2
        expected: global interrupt still wins over the slot reprompt
        end state: MENU
        output: Sure, let's switch gears.
                
                Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 63. input: what is your return policy then no  |  from: LIVE_AGENT
        expected: agent closing line, state stays LIVE_AGENT
        end state: LIVE_AGENT
        output: Happy to help. Your conversation stays logged, and you can head back to the main menu any time.

[PASS] 64. input: 999  |  from: LIVE_AGENT
        expected: not-finding-that-order response, not the rotating acknowledgment
        end state: LIVE_AGENT
        output: I'm not finding an order with that number on my end. Can you double check it?

[PASS] 65. input: help me find gear  |  from: LIVE_AGENT
        expected: handback line, divider, then the REC_Q1 opening, end state REC_Q1
        end state: REC_Q1
        output: Let me hand you back to the support bot, it'll walk you through it.
                [Back with the support bot]
                Let's find the right gear. What are you heading out for?

[PASS] 66. input: help me find gear then Camping  |  from: LIVE_AGENT
        expected: the handed back flow actually runs, REC_Q2
        end state: REC_Q2
        output: Good pick. What conditions are you expecting?

[PASS] 67. input: i want to track my order  |  from: LIVE_AGENT
        expected: handback line, then the order prompt, end state AWAIT_ORDER
        end state: AWAIT_ORDER
        output: Let me hand you back to the support bot, it'll walk you through it.
                [Back with the support bot]
                Happy to help. What's your order number?

[PASS] 68. input: track order 111  |  from: LIVE_AGENT
        expected: Riley answers directly, no handback, stays in LIVE_AGENT
        end state: LIVE_AGENT
        output: Sure, I can pull that up.
                
                Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 69. input: 111  |  from: LIVE_AGENT
        expected: Riley answers directly, stays in LIVE_AGENT
        end state: LIVE_AGENT
        output: Sure, I can pull that up.
                
                Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 70. input: i want to speak to human  |  from: LIVE_AGENT
        expected: already-connected response
        end state: LIVE_AGENT
        output: You're already connected with me, Riley. Go ahead.

[PASS] 71. input: a long message containing the word thanks  |  from: LIVE_AGENT
        expected: NOT the courtesy response
        end state: LIVE_AGENT
        output: I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations.

[PASS] 72. input: thanks  |  from: MENU
        expected: short courtesy still works
        end state: MENU
        output: Anytime. Anything else I can help with?

[PASS] 73. input: what all products you guys have  |  from: MENU
        expected: catalog line plus the REC_Q1 question, end state REC_Q1
        end state: REC_Q1
        output: We carry outdoor apparel and camping gear.
                
                Let's find the right gear. What are you heading out for?

[PASS] 74. input: what is your name  |  from: MENU
        expected: bot identity response
        end state: MENU
        output: I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 75. input: what is your name  |  from: LIVE_AGENT
        expected: Riley identity response
        end state: LIVE_AGENT
        output: I'm Riley, part of the North Star support team.

[PASS] 76. input: are you a bot  |  from: LIVE_AGENT
        expected: AGENT_META disclosure still wins over IDENTITY
        end state: LIVE_AGENT
        output: Fair question. This is a simulated live agent state for this demo, so I'm scripted rather than a real person. In production this is where a human agent picks up with the full conversation history.

[PASS] 77. input: can you send your website  |  from: MENU
        expected: contact response offering only the returns link
        end state: MENU
        output: I don't have a general site link to share here. For returns and exchanges you can go to northstaroutfitters.com/returns

[PASS] 78. input: how much does a tent cost  |  from: MENU
        expected: declines on price, offers a live agent, invents nothing
        end state: MENU
        output: I don't have pricing, stock or carrier details here. I can help with order tracking, returns and exchanges, or gear recommendations, or I can connect you with a live agent.

[PASS] 79. input: which carrier do you use  |  from: LIVE_AGENT
        expected: declines on carrier, no carrier named
        end state: LIVE_AGENT
        output: I don't have pricing, stock or carrier details here. I can help with order tracking, returns and exchanges, or gear recommendations, or I can connect you with a live agent.

[PASS] 80. input: three recommendations in a row  |  from: MENU
        expected: three different result templates, no consecutive repeat
        end state: MENU
        output: For camping in cold and snowy conditions, start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove. >> Heading out camping in cold and snowy conditions, I'd start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove. >> For cold and snowy conditions, **four-season tents and cold-rated sleeping bags** is where I'd start. Worth adding: an insulated sleeping pad and a camp stove.

[PASS] 81. input: dont connect me with live agent  |  from: MENU
        expected: stays with the bot, does not escalate
        end state: MENU
        output: Understood, I'll keep you here with the support bot. What can I help you with?

[PASS] 82. input: do not transfer me to an agent  |  from: MENU
        expected: stays with the bot
        end state: MENU
        output: Understood, I'll keep you here with the support bot. What can I help you with?

[PASS] 83. input: order tracking, no return policy  |  from: MENU
        expected: order flow, not returns
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 84. input: return policy, no order tracking  |  from: MENU
        expected: returns, not the order flow
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 85. input: no  |  from: MENU after an anything-else question
        expected: still the closing line, not the negation response
        end state: MENU
        output: Happy to help. Have a good one out there.

[PASS] 86. input: transfer back to the bot  |  from: LIVE_AGENT
        expected: exits to MENU
        end state: MENU
        output: [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with?

[PASS] 87. input: back to support bot  |  from: LIVE_AGENT
        expected: exits to MENU
        end state: MENU
        output: [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with?

[PASS] 88. input: no you are a live agent, transfer back to the normal chatbot  |  from: LIVE_AGENT
        expected: exits to MENU
        end state: MENU
        output: [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with?

[PASS] 89. input: i want to speak to human  |  from: LIVE_AGENT
        expected: still the already-connected response
        end state: LIVE_AGENT
        output: You're already connected with me, Riley. Go ahead.

[PASS] 90. input: 123  |  from: AWAIT_ORDER
        expected: no order number named anywhere in the response
        end state: AWAIT_ORDER
        output: I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up.

[PASS] 91. input: 123 repeatedly across two order flows  |  from: AWAIT_ORDER
        expected: escalates once, and again after a genuine exit and a fresh stuck run
        end state: LIVE_AGENT
        output: Happy to help. What's your order number? >> I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. >> I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with? >> Happy to help. What's your order number? >> I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. >> I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> I'm not finding an order with that number on my end. Can you double check it?

[PASS] 92. input: what can you help me with  |  from: LIVE_AGENT
        expected: capability answer, not an acknowledgment
        end state: LIVE_AGENT
        output: I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations.

[PASS] 93. input: what can you do  |  from: MENU
        expected: capability answer
        end state: MENU
        output: I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 94. input: hi four times  |  from: LIVE_AGENT
        expected: greeting every time, never a case note
        end state: LIVE_AGENT
        output: Hi again. What can I help with? | Hi again. What can I help with? | Hi again. What can I help with? | Hi again. What can I help with?

[PASS] 95. input: i want to buy gears  |  from: MENU
        expected: recommendation flow, plural matched
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 96. input: looking for jackets  |  from: MENU
        expected: recommendation flow, plural matched
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 97. input: wheres my orders  |  from: MENU
        expected: order flow, plural matched
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 98. input: you idiot  |  from: MENU
        expected: apology plus escalation
        end state: LIVE_AGENT
        output: Sorry, I'm not making this easy. Let me get you to a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 99. input: bro this chatbot is so annoying  |  from: MENU
        expected: apology plus escalation
        end state: LIVE_AGENT
        output: Sorry, I'm not making this easy. Let me get you to a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 100. input: are you slow or what  |  from: REC_Q1
        expected: apology plus escalation
        end state: LIVE_AGENT
        output: Sorry, I'm not making this easy. Let me get you to a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 101. input: you idiot  |  from: LIVE_AGENT
        expected: actionable redirect, no escalation loop
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations.

[PASS] 102. input: what are the services or products you guys have  |  from: MENU
        expected: catalog line plus REC_Q1
        end state: REC_Q1
        output: We carry outdoor apparel and camping gear.
                
                Let's find the right gear. What are you heading out for?

[PASS] 103. input: what are the services or products you guys have  |  from: LIVE_AGENT
        expected: handback plus REC_Q1
        end state: REC_Q1
        output: Let me hand you back to the support bot, it'll walk you through it.
                [Back with the support bot]
                We carry outdoor apparel and camping gear.
                
                Let's find the right gear. What are you heading out for?

[PASS] 104. input: nevermind, i want to buy gears  |  from: AWAIT_ORDER
        expected: recommendation flow
        end state: REC_Q1
        output: Sure, let's switch gears.
                
                Let's find the right gear. What are you heading out for?

[PASS] 105. input: nevermind  |  from: REC_Q2
        expected: cancels to MENU
        end state: MENU
        output: Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 106. input: three unmatched messages  |  from: LIVE_AGENT
        expected: three different actionable redirects, none mentioning case notes
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can  >> That one's outside what I can pull up he >> I'm not able to answer that one. What I 

[PASS] 107. input: dont give me the return policy again  |  from: MENU
        expected: does not deliver the thing that was refused
        end state: MENU
        output: No problem. What would you like to do instead?

[PASS] 108. input: dont tell me about shipping just find my package  |  from: MENU
        expected: negation stops at the pivot, order request survives
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 109. input: no bot i want human  |  from: MENU
        expected: negation applies to the bot, not to the request for a human
        end state: LIVE_AGENT
        output: Connecting you with a live agent now.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 110. input: can you cancel my order  |  from: MENU
        expected: says it cannot change an order, does not ask for a number
        end state: MENU
        output: I can't change an order from here. I can look up its status, go over returns and exchanges, or connect you with a live agent.

[PASS] 111. input: why do you need my order number  |  from: AWAIT_ORDER
        expected: plain re-prompt, no incoherent switch announcement
        end state: AWAIT_ORDER
        output: I'll need your order number to look that up. You can also type menu to go back.

[PASS] 112. input: i said give your exchange policy idiot  |  from: MENU
        expected: answers the question, no escalation, no comment on tone
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 113. input: what the hell, i want your exchange policy  |  from: LIVE_AGENT
        expected: the policy, not a redirect
        end state: LIVE_AGENT
        output: Sure, I can pull that up.
                
                Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 114. input: you idiot  |  from: MENU
        expected: still escalates when there is no request in the message
        end state: LIVE_AGENT
        output: Sorry, I'm not making this easy. Let me get you to a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 115. input: this is so annoying, where is my order  |  from: MENU
        expected: order flow, no escalation
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 116. input: exchance policy  |  from: MENU
        expected: returns and exchanges policy via typo tolerance
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 117. input: retrun poilcy  |  from: MENU
        expected: returns and exchanges policy via typo tolerance
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 118. input: wheres my ordr  |  from: MENU
        expected: order flow via typo tolerance
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 119. input: trak my order  |  from: MENU
        expected: order flow
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 120. input: shippign times  |  from: MENU
        expected: shipping response via typo tolerance
        end state: MENU
        output: Standard shipping takes 3 to 5 business days. Expedited shipping takes 1 to 2 business days.
                
                Anything else?

[PASS] 121. input: i want to speak to riley  |  from: MENU
        expected: connects to the live agent
        end state: LIVE_AGENT
        output: Connecting you with a live agent now.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 122. input: who all are there in your team  |  from: LIVE_AGENT
        expected: actionable redirect, not already-connected
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations.

[PASS] 123. input: i want to speak to a human  |  from: LIVE_AGENT
        expected: still already-connected
        end state: LIVE_AGENT
        output: You're already connected with me, Riley. Go ahead.

[PASS] 124. input: nothing  |  from: REC_Q1
        expected: cancels to menu
        end state: MENU
        output: No problem. What else can I help you with?

[PASS] 125. input: back  |  from: MENU
        expected: menu copy with no switch-gears prefix
        end state: MENU
        output: Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 126. input: menu  |  from: REC_Q2
        expected: menu copy WITH the switch-gears prefix, a genuine interrupt
        end state: MENU
        output: Sure, let's switch gears.
                
                Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 127. input: Back to main menu chip mid slot fill  |  from: AWAIT_ORDER
        expected: no switch-gears prefix on an offered chip
        end state: MENU
        output: Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 128. input: i dont understand  |  from: MENU
        expected: capability explanation, not a fallback
        end state: MENU
        output: I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 129. input: seven exchange phrasings  |  from: MENU
        expected: all return the combined returns and exchanges response
        end state: MENU
        output: all seven returned the policy

[PASS] 130. input: terrible service, when will 111 arrive  |  from: MENU
        expected: answers the order, does not escalate on the insult
        end state: MENU
        output: Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 131. input: are you hiring  |  from: MENU
        expected: no fuzzy correction to hiking, no gear flow
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 132. input: sorry i didnt get that  |  from: MENU
        expected: re-explains capability rather than reporting its own confusion
        end state: MENU
        output: I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 133. input: that made no sense to me  |  from: MENU
        expected: capability explanation
        end state: MENU
        output: I'm the North Star Support Bot. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent.

[PASS] 134. input: i want to order  |  from: MENU
        expected: asks which sense, does not demand a number
        end state: MENU
        output: Happy to help. Are you looking up an existing order, or looking to buy something?

[PASS] 135. input: wanted to order something else  |  from: MENU
        expected: recommendation flow
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 136. input: havent ordered yet, im saying I want to order  |  from: MENU
        expected: recommendation flow
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 137. input: i want to order what all options you have  |  from: MENU
        expected: recommendation flow, purchase wins over lookup
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 138. input: i want to buy a tent  |  from: MENU
        expected: recommendation flow
        end state: REC_Q1
        output: Let's find the right gear. What are you heading out for?

[PASS] 139. input: where is my order  |  from: MENU
        expected: order flow, unchanged by the sense rule
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 140. input: track order 111  |  from: MENU
        expected: shipped status, unchanged by the sense rule
        end state: MENU
        output: Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 141. input: order number is 222  |  from: MENU
        expected: processing status
        end state: MENU
        output: Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days.
                
                Anything else?

[PASS] 142. input: your useless  |  from: MENU
        expected: escalation, not the return policy
        end state: LIVE_AGENT
        output: Sorry, I'm not making this easy. Let me get you to a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 143. input: this bot is annoying  |  from: LIVE_AGENT
        expected: does NOT exit the handoff
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations.

[PASS] 144. input: you are acting like a bot  |  from: LIVE_AGENT
        expected: does NOT exit the handoff
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations.

[PASS] 145. input: put me back with the chatbot  |  from: LIVE_AGENT
        expected: exits to MENU
        end state: MENU
        output: [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with?

[PASS] 146. input: what about 222  |  from: LIVE_AGENT
        expected: processing status, not a redirect
        end state: LIVE_AGENT
        output: Sure, I can pull that up.
                
                Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days.
                
                Anything else?

[PASS] 147. input: no sorry 111  |  from: MENU after an anything-else question
        expected: shipped status, not the closing line
        end state: MENU
        output: Order #111 has shipped and it's arriving tomorrow.
                
                Anything else?

[PASS] 148. input: no  |  from: MENU after an anything-else question
        expected: still the closing line
        end state: MENU
        output: Happy to help. Have a good one out there.

[PASS] 149. input: 333 then no  |  from: LIVE_AGENT
        expected: delivery problem response, stays in LIVE_AGENT
        end state: LIVE_AGENT
        output: Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns
                
                If you'd rather sort it out with a person, I can connect you with a live agent.

[PASS] 150. input: 333 then yes  |  from: LIVE_AGENT
        expected: glad to hear it, stays in LIVE_AGENT
        end state: LIVE_AGENT
        output: Glad to hear it. Anything else I can help with?

[PASS] 151. input: not sure  |  from: REC_Q1
        expected: no-problem reprompt, not a fallback
        end state: REC_Q1
        output: No problem. Here's what most people start with, pick whichever is closest.

[PASS] 152. input: i dont know  |  from: REC_Q2
        expected: no-problem reprompt
        end state: REC_Q2
        output: No problem. What's the weather usually like when you head out?

[PASS] 153. input: you tell me  |  from: REC_Q1
        expected: no-problem reprompt
        end state: REC_Q1
        output: No problem. Here's what most people start with, pick whichever is closest.

[PASS] 154. input: how do i order  |  from: MENU
        expected: asks which sense rather than demanding a number
        end state: MENU
        output: Happy to help. Are you looking up an existing order, or looking to buy something?

[PASS] 155. input: what is happening with my purchase  |  from: MENU
        expected: purchase as a noun is a lookup, not a buy
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 156. input: why did the bot transfer me  |  from: LIVE_AGENT
        expected: a question about the transfer does not perform one
        end state: LIVE_AGENT
        output: You're already connected with me, Riley. Go ahead.

[PASS] 157. input: 123 then not sure then no idea  |  from: AWAIT_ORDER
        expected: mixed misses accumulate, third escalates
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 158. input: not sure then 999 then dont remember  |  from: AWAIT_ORDER
        expected: escalates on the third regardless of type
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 159. input: the live log sequence  |  from: AWAIT_ORDER
        expected: nope I dont remember, i dont know i said, not sure escalates by the third
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 160. input: three unrelated gibberish strings  |  from: MENU
        expected: escalates on the third
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 161. input: three gibberish answers  |  from: REC_Q1
        expected: escalates on the third
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 162. input: 123 then 111 then 456  |  from: AWAIT_ORDER
        expected: a successful resolution resets the count
        end state: AWAIT_ORDER
        output: I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up.

[PASS] 163. input: 123 then a different flow then back  |  from: AWAIT_ORDER
        expected: a global interrupt resets the count
        end state: MENU
        output: Sure, let's switch gears.
                
                Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 164. input: not sure  |  from: AWAIT_ORDER
        expected: acknowledged, offers the agent side, counts as one miss
        end state: AWAIT_ORDER
        output: No worries. If you can't find it, I can pull it up on the agent side instead.

[PASS] 165. input: stuck escalation caps per episode, not per session  |  from: MENU
        expected: a second stuck run escalates again after a genuine exit
        end state: LIVE_AGENT
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with? >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 166. input: i dont like it, want to return  |  from: ORDER_333_FOLLOWUP
        expected: delivery-problem response on the first turn
        end state: MENU
        output: Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns
                
                If you'd rather sort it out with a person, I can connect you with a live agent.

[PASS] 167. input: i said i dont like it, want to return it  |  from: ORDER_333_FOLLOWUP
        expected: delivery-problem response on the first turn
        end state: MENU
        output: Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns
                
                If you'd rather sort it out with a person, I can connect you with a live agent.

[PASS] 168. input: need to return this  |  from: ORDER_333_FOLLOWUP
        expected: delivery-problem response
        end state: MENU
        output: Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns
                
                If you'd rather sort it out with a person, I can connect you with a live agent.

[PASS] 169. input: dont connect me with live agent  |  from: MENU
        expected: no regression, still stays with the bot
        end state: MENU
        output: Understood, I'll keep you here with the support bot. What can I help you with?

[PASS] 170. input: dont tell me about shipping just find my package  |  from: MENU
        expected: no regression, shipping suppressed, order runs
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 171. input: treking  |  from: REC_Q1
        expected: maps to hiking and backpacking
        end state: REC_Q2
        output: Good pick. What conditions are you expecting?

[PASS] 172. input: fishing  |  from: REC_Q1
        expected: maps to everyday outdoors
        end state: REC_Q2
        output: Good pick. What conditions are you expecting?

[PASS] 173. input: sky diving  |  from: REC_Q1
        expected: out-of-scope response, chips intact, not counted as a miss
        end state: REC_Q1
        output: That one's a bit outside what we carry, we're focused on outdoor apparel and camping gear. Want me to point you toward something in that range instead?

[PASS] 174. input: heat  |  from: REC_Q2
        expected: maps to warm and dry
        end state: MENU
        output: For camping in warm and dry conditions, start with **three-season tents and lightweight sleeping bags**. Worth adding: camp furniture and a shade tarp.
                
                Want me to help with anything else?

[PASS] 175. input: freezing  |  from: REC_Q2
        expected: maps to cold and snowy
        end state: MENU
        output: For camping in cold and snowy conditions, start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove.
                
                Want me to help with anything else?

[PASS] 176. input: all sixteen chip label combinations  |  from: REC_Q1
        expected: the exact chip labels still resolve unchanged
        end state: MENU
        output: 16 distinct of 16

[PASS] 177. input: no actually tell return policy  |  from: MENU
        expected: RETURNS on the first turn, not the negation fallback
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 178. input: no actually track my order  |  from: MENU
        expected: order flow on the first turn
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 179. input: no wait i want returns  |  from: MENU
        expected: RETURNS
        end state: MENU
        output: Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 180. input: no i mean shipping  |  from: MENU
        expected: SHIPPING
        end state: MENU
        output: Standard shipping takes 3 to 5 business days. Expedited shipping takes 1 to 2 business days.
                
                Anything else?

[PASS] 181. input: the negation fallback three times  |  from: MENU
        expected: counts as a miss each time, escalates on the third
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 182. input: negation fallback mixed with other misses  |  from: MENU
        expected: misses of different kinds still add up to three
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 183. input: dont connect me with live agent  |  from: MENU
        expected: resolves, so it does not count as a miss
        end state: MENU
        output: Understood, I'll keep you here with the support bot. What can I help you with?

[PASS] 184. input: three Group A answers  |  from: LIVE_AGENT
        expected: three different lookup prefixes, no consecutive repeat
        end state: LIVE_AGENT
        output: Sure, I can pull that up. >> Let me check that for you. >> Of course, here you go.

[PASS] 185. input: where is my <abc>?  |  from: MENU
        expected: standard fallback, not AWAIT_ORDER
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 186. input: where is my <abc>?  |  from: AWAIT_ORDER (mid-flow)
        expected: standard fallback, not treated as a slot answer
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 187. input: track order {123}  |  from: MENU
        expected: standard fallback response
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 188. input: [track my package]  |  from: MENU
        expected: standard fallback response
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 189. input: where is my order  |  from: MENU, no junk
        expected: unchanged, enters AWAIT_ORDER normally
        end state: AWAIT_ORDER
        output: Happy to help. What's your order number?

[PASS] 190. input: 999  |  from: AWAIT_ORDER, real wrong number
        expected: unchanged, existing invalid order path, not the junk path
        end state: AWAIT_ORDER
        output: I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up.

[PASS] 191. input: <script> tag, injection style  |  from: MENU
        expected: standard fallback response, injection style input is not exempt
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 192. input: three junk messages in a row  |  from: MENU
        expected: counts as a miss each time, escalates on the third
        end state: LIVE_AGENT
        output: I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 193. input: talk to a human, then <abc>  |  from: LIVE_AGENT
        expected: Riley's redirect, no divider, no reintro, state stays LIVE_AGENT
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations.

[PASS] 194. input: three junk messages in LIVE_AGENT  |  from: LIVE_AGENT
        expected: three rotating redirects, no consecutive repeat, never a second connect sequence
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. >> That one's outside what I can pull up here. I can help with an order lookup, returns and exchanges, or gear recommendations through the support bot. >> I'm not able to answer that one. What I can do is check an order, explain returns and exchanges, or get you gear recommendations.

[PASS] 195. input: junk then a real question in LIVE_AGENT  |  from: LIVE_AGENT
        expected: Riley answers correctly, proving state was never lost
        end state: LIVE_AGENT
        output: Sure, I can pull that up.
                
                Returns and exchanges at North Star:
                
                You have 30 days to return an item. It needs to be unused and in its original packaging.
                
                Exchanges use the same process, so start here either way: northstaroutfitters.com/returns
                
                Anything else?

[PASS] 196. input: MENU junk then talk to a human  |  from: MENU
        expected: connects normally with one clean sequence
        end state: LIVE_AGENT
        output: Connecting you with a live agent now.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 197. input: !!!  |  from: LIVE_AGENT
        expected: empty-normalize input does not reset state, unlike the pre-fix bug
        end state: LIVE_AGENT
        output: I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations.

[PASS] 198. input: escalate, exit, escalate again  |  from: MENU
        expected: second stuck episode escalates a second time
        end state: LIVE_AGENT
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with? >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 199. input: escalate, exit, only two misses  |  from: MENU
        expected: counter genuinely resets to 0, two is not enough to escalate again
        end state: MENU
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent.

[PASS] 200. input: escalate, stay connected, more junk  |  from: LIVE_AGENT
        expected: no exit means no new episode, still never a second connect sequence
        end state: LIVE_AGENT
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. >> That one's outside what I can pull up here. I can help with an order lookup, returns and exchanges, or gear recommendations through the support bot. >> I'm not able to answer that one. What I can do is check an order, explain returns and exchanges, or get you gear recommendations.

[PASS] 201. input: three separate escalations in one session  |  from: MENU
        expected: all three fire cleanly across a long session
        end state: LIVE_AGENT
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with? >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> [Back with the support bot]
                You're back with the North Star Support Bot. What can I help you with? >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 202. input: explicit live agent chip after an episode  |  from: MENU
        expected: talk to a human still works regardless of the counter
        end state: LIVE_AGENT
        output: Connecting you with a live agent now.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 203. input: escalate, hand back via order lookup, escalate again  |  from: MENU
        expected: handBackToBot is also a genuine exit, second episode escalates
        end state: LIVE_AGENT
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> Let me hand you back to the support bot, it'll walk you through it.
                [Back with the support bot]
                Happy to help. What's your order number? >> I'll need your order number to look that up. You can also type menu to go back. >> I'll need your order number to look that up. You can also type menu to go back. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

[PASS] 204. input: escalate, hand back via gear rec, escalate again  |  from: MENU
        expected: same fix, the product recommendation handback path
        end state: LIVE_AGENT
        output: Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? >> Let me hand you back to the support bot, it'll walk you through it.
                [Back with the support bot]
                Let's find the right gear. What are you heading out for? >> Sorry, I didn't understand that. Which of these is closest? >> Sorry, I didn't understand that. Which of these is closest? >> I want to make sure you get sorted out. Let me connect you with a live agent.
                [Live Agent connected]
                Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with?

```

## Coverage beyond the 28 required cases

These were exercised manually against the same engine during the build and
are not part of the panel:

| Input sequence | Observed |
|---|---|
| `Track my order` <br> _menu chip enters AWAIT_ORDER_ | Happy to help. What's your order number? <br> end state `AWAIT_ORDER` |
| `Returns and exchanges` <br> _menu chip returns the policy_ | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? <br> end state `MENU` |
| `Help me find gear` <br> _menu chip enters REC_Q1_ | Let's find the right gear. What are you heading out for? <br> end state `REC_Q1` |
| `Talk to a live agent` <br> _menu chip enters LIVE_AGENT_ | Connecting you with a live agent now. / [Live Agent connected] / Hi, this is Riley from the North Star team. I've got your full conversation history, so what can I help with? <br> end state `LIVE_AGENT` |
| `333` then `Yes, all good` <br> _follow-up chip, affirm branch_ | Glad to hear it. Enjoy the trail. / Anything else? <br> end state `MENU` |
| `333` then `I need to return something` <br> _follow-up chip, return branch_ | Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns / If you'd rather sort it out with a person, I can connect you with a live agent. <br> end state `MENU` |
| `333` then `damaged` <br> _free text deny branch_ | Sorry about that. Our return window is 30 days from delivery, and items need to be unused and in their original packaging. You can start a return here: northstaroutfitters.com/returns / If you'd rather sort it out with a person, I can connect you with a live agent. <br> end state `MENU` |
| `shipping options` <br> _must be shipping, not a menu reset_ | Standard shipping takes 3 to 5 business days. Expedited shipping takes 1 to 2 business days. / Anything else? <br> end state `MENU` |
| `what are my options for shipping` <br> _options no longer hijacks shipping_ | Standard shipping takes 3 to 5 business days. Expedited shipping takes 1 to 2 business days. / Anything else? <br> end state `MENU` |
| `what's the status of my order 333` <br> _combined intent and number_ | Order #333 was delivered. Did everything arrive in good shape? <br> end state `ORDER_333_FOLLOWUP` |
| `order 999 status` <br> _combined intent with an unknown number_ | I couldn't find an order with that number. Give it another check, or I can connect you with a live agent to look it up. <br> end state `AWAIT_ORDER` |
| `track my order` then `1 1 1` <br> _digits separated by spaces_ | Order #111 has shipped and it's arriving tomorrow. / Anything else? <br> end state `MENU` |
| `track my order` then `menu` <br> _menu interrupt mid slot fill_ | Sure, let's switch gears. / Back to the main menu. I can track an order, help with returns and exchanges, recommend gear, or connect you with a live agent. <br> end state `MENU` |
| `track my order` then `i need a jacket` <br> _recommendation interrupt mid slot fill_ | Sure, let's switch gears. / Let's find the right gear. What are you heading out for? <br> end state `REC_Q1` |
| `track my order` then `999` then `111` <br> _invalid then valid, streak cleared_ | Order #111 has shipped and it's arriving tomorrow. / Anything else? <br> end state `MENU` |
| `track my order` then `no` then `no` then `no` then `no` <br> _four non-numeric replies never escalate_ | I don't have that detail on hand. I can look up an order, go over returns and exchanges, or hand you back to the support bot for gear recommendations. <br> end state `LIVE_AGENT` |
| `Winter sports` then `Cold and snowy` <br> _activity typed at MENU, flow finishes in two turns_ | For winter sports in cold and snowy conditions, start with **heavyweight insulated jackets and thermal base layers**. Worth adding: insulated gloves and a balaclava. / Want me to help with anything else? <br> end state `MENU` |
| `cold and snowy` then `Camping` <br> _condition typed at MENU, flow finishes in two turns_ | For camping in cold and snowy conditions, start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove. / Want me to help with anything else? <br> end state `MENU` |
| `my package was crushed` <br> _damage language outranks the weak order signal_ | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? <br> end state `MENU` |
| `i need help with a return` <br> _help does not hijack returns_ | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? <br> end state `MENU` |
| `can i send this thing back to you` <br> _back does not hijack returns_ | Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? <br> end state `MENU` |
| `agent` then `my package was crushed` <br> _Riley answers with the returns policy_ | Sure, I can pull that up. / Returns and exchanges at North Star: / You have 30 days to return an item. It needs to be unused and in its original packaging. / Exchanges use the same process, so start here either way: northstaroutfitters.com/returns / Anything else? <br> end state `LIVE_AGENT` |
| `agent` then `where is order 222` <br> _Riley answers with the exact order copy_ | Sure, I can pull that up. / Order #222 is processing and ships within 24 hours. Once it's on the way, standard shipping takes 3 to 5 business days. / Anything else? <br> end state `LIVE_AGENT` |
| `agent` then `are you a bot` <br> _honest disclosure_ | Fair question. This is a simulated live agent state for this demo, so I'm scripted rather than a real person. In production this is where a human agent picks up with the full conversation history. <br> end state `LIVE_AGENT` |
| `agent` then `when will i hear back` <br> _timing answer, and back does not exit the handoff_ | I don't have a specific time to give you on that, and I don't want to guess. Your conversation is logged, so whoever picks it up will have the full context. <br> end state `LIVE_AGENT` |
| `agent` then `thanks` <br> _courtesy works inside the handoff_ | Anytime. Anything else I can help with? <br> end state `LIVE_AGENT` |
| `agent` then `Back to main menu` <br> _exit chip leaves LIVE_AGENT_ | [Back with the support bot] / You're back with the North Star Support Bot. What can I help you with? <br> end state `MENU` |
| `what should i buy` then `camping` then `zzz` then `zzz` <br> _two misses mid flow escalate_ | Sorry, I didn't understand that. Which of these is closest? <br> end state `REC_Q2` |
| `111` then `no` <br> _answering the closing question_ | Happy to help. Have a good one out there. <br> end state `MENU` |
| `111` then `no where is my order` <br> _a no with a real question in it is the question_ | Happy to help. Have a good one out there. <br> end state `MENU` |
| `thanks` then `no` <br> _courtesy then close_ | Happy to help. Have a good one out there. <br> end state `MENU` |
| `!!!` <br> _punctuation only is treated as unrecognized_ | Sorry, I didn't understand that. I can help with order tracking, returns and exchanges, product recommendations, or I can connect you with a live agent. <br> end state `MENU` |
| `   ` <br> _whitespace only is never sent_ | (no reply, input ignored) <br> end state `MENU` |

## All 16 recommendation combinations

| Activity | Conditions | Response |
|---|---|---|
| Hiking and backpacking | Warm and dry | For hiking and backpacking in warm and dry conditions, start with **lightweight hiking apparel and trail footwear**. Worth adding: sun-protective layers and a hydration pack. / Want me to help with anything else? |
| Hiking and backpacking | Cool and variable | For hiking and backpacking in cool and variable conditions, start with **midweight insulating layers and trail footwear**. Worth adding: a packable wind shell. / Want me to help with anything else? |
| Hiking and backpacking | Cold and snowy | For hiking and backpacking in cold and snowy conditions, start with **insulated jackets and thermal base layers**. Worth adding: winter trail footwear with traction. / Want me to help with anything else? |
| Hiking and backpacking | Wet and rainy | For hiking and backpacking in wet and rainy conditions, start with **waterproof rain shells and waterproof hiking boots**. Worth adding: quick-dry base layers. / Want me to help with anything else? |
| Camping | Warm and dry | For camping in warm and dry conditions, start with **three-season tents and lightweight sleeping bags**. Worth adding: camp furniture and a shade tarp. / Want me to help with anything else? |
| Camping | Cool and variable | For camping in cool and variable conditions, start with **three-season tents and midweight sleeping bags**. Worth adding: an insulated sleeping pad. / Want me to help with anything else? |
| Camping | Cold and snowy | For camping in cold and snowy conditions, start with **four-season tents and cold-rated sleeping bags**. Worth adding: an insulated sleeping pad and a camp stove. / Want me to help with anything else? |
| Camping | Wet and rainy | For camping in wet and rainy conditions, start with **waterproof tents and rain tarps**. Worth adding: waterproof stuff sacks and a groundsheet. / Want me to help with anything else? |
| Winter sports | Warm and dry | For winter sports in warm and dry conditions, start with **spring-weight shells and light insulation**. Worth adding: goggles and sun protection. / Want me to help with anything else? |
| Winter sports | Cool and variable | For winter sports in cool and variable conditions, start with **insulated shells and midlayers**. Worth adding: insulated gloves. / Want me to help with anything else? |
| Winter sports | Cold and snowy | For winter sports in cold and snowy conditions, start with **heavyweight insulated jackets and thermal base layers**. Worth adding: insulated gloves and a balaclava. / Want me to help with anything else? |
| Winter sports | Wet and rainy | For winter sports in wet and rainy conditions, start with **waterproof insulated shells**. Worth adding: waterproof gloves and a helmet cover. / Want me to help with anything else? |
| Everyday outdoors | Warm and dry | For everyday outdoors in warm and dry conditions, start with **lightweight everyday outdoor apparel**. Worth adding: a sun hat and casual trail shoes. / Want me to help with anything else? |
| Everyday outdoors | Cool and variable | For everyday outdoors in cool and variable conditions, start with **fleece and softshell layers**. Worth adding: everyday trail footwear. / Want me to help with anything else? |
| Everyday outdoors | Cold and snowy | For everyday outdoors in cold and snowy conditions, start with **insulated everyday jackets**. Worth adding: a beanie and insulated gloves. / Want me to help with anything else? |
| Everyday outdoors | Wet and rainy | For everyday outdoors in wet and rainy conditions, start with **rain jackets and water-resistant footwear**. Worth adding: a waterproof daypack. / Want me to help with anything else? |
