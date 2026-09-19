<img src="assemblyai.png" width="500"/>

---

[![Voice Agent API](https://img.shields.io/badge/docs-Voice%20Agent%20API-2545E6)](https://www.assemblyai.com/docs/voice-agents/voice-agent-api)
[![Python](https://img.shields.io/badge/python-%E2%89%A53.9-3776AB?logo=python&logoColor=white)](https://www.python.org)
[![Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)](requirements.txt)
[![Hackathon](https://img.shields.io/badge/lablab.ai-AssemblyAI%20Voice%20Agent%20Hackathon-black)](https://lablab.ai/ai-hackathons/assemblyai-voice-agent-hackathon)

# TV Nova Support — an AI-native contact center

A voice agent prototype for a fictional pay-TV company, built for the [AssemblyAI Voice Agent Hackathon](https://lablab.ai/ai-hackathons/assemblyai-voice-agent-hackathon) (lablab.ai, Sep 1–30 2026).

## The problem

Most contact-center calls are repetitive: confirm who is calling, look up their account, answer a routine question, maybe log a follow-up for someone else to handle. That repetitive layer is what ties up human agents and keeps hold times long — and it is exactly what a voice agent can absorb, handing off to a person only when the request actually needs one.

## What this does today

`TV Nova Support`, defined in one file ([`agents/tv-agent.jsonc`](agents/tv-agent.jsonc)), answers a call and:

1. Greets the caller and asks for the email on their account.
2. Calls `verify_subscriber` before answering anything account-specific — it never invents a plan, a balance, or a status.
3. Answers from what comes back, or explains it could not find the account and offers to log the issue.
4. Calls `log_ticket` once, near the end, filing a one-line summary of the call.

Both tools point at a free public mock API ([jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com)) standing in for TV Nova's real subscriber and ticketing systems. Swapping in real endpoints later is a two-line change inside the agent file — nothing else about it moves.

## Architecture

```
  caller (browser tab or phone number)
              │
              ▼
   AssemblyAI Voice Agent API
   STT + LLM + TTS + turn-taking, one connection
              │
     ┌────────┴────────┐
     ▼                 ▼
verify_subscriber   log_ticket
 (GET, mock API)    (POST, mock API)
```

Everything above the tools is AssemblyAI's Voice Agent API: transcription, the model, the voice, and deciding when each side gets to talk. Everything below is `agents/tv-agent.jsonc` and the two HTTP tools it declares — that split is the whole point of building on the **Voice Agent API** option instead of wiring STT, LLM and TTS together by hand.

## Judging criteria, honestly

| Criterion | Where it shows up |
| --- | --- |
| Application of technology | Voice Agent API end to end — managed STT/LLM/TTS and turn-taking, plus 2 tool calls AssemblyAI executes mid-call |
| Business value | Absorbs the two call types that dominate contact-center volume — identity verification and status lookup — and hands off cleanly for anything it cannot resolve itself |
| Originality | _fill in once the team has picked what makes this entry different — see Roadmap below_ |
| Presentation | This README, plus the demo video linked under Submission |

## Quickstart

```bash
git clone https://github.com/Alassana-Djigo/AI-Native-Contact-Center.git
cd AI-Native-Contact-Center
cp .env.example .env
# paste your key from https://www.assemblyai.com/dashboard/api-keys into .env
```

Publish the TV Nova agent — not the starter's default `minimal` one:

```bash
AGENT=tv-agent python publish.py
```

Then:

```bash
python deployment/browser/server.py
```

Open <http://localhost:3000>, start the call, and give it an email from the mock subscriber list — `Sincere@april.biz` is a real one — then ask about your plan.

To stop typing `AGENT=tv-agent` on every command, add one line to your own `.env` (it is gitignored, safe to edit):

```
AGENT=tv-agent
```

## Roadmap — 11 days to Sep 30

- [ ] Decide the one thing that makes this different from a generic "verify + log" demo — that decision is the Originality score
- [ ] Add a third tool tied to that decision
- [ ] Record the 5-minute presentation video
- [ ] Write the lablab.ai product description
- [ ] Optional: put it on a real Twilio number (`deployment/telephony/connect.py`) for the live demo

## Working on this repo

New here? Start with [`AGENTS.md`](AGENTS.md) and [`agents/README.md`](agents/README.md) — they cover the agent file format, how tools work, and the conventions for adding one. `agents/tv-agent.jsonc` is our submission; the other 9 files in `agents/` are the starter's own reference examples and are worth keeping as-is, since they double as documentation.

## Team

- Alassana Djigo — lead
- _add teammate 2_
- _add teammate 3_

## Submission

- lablab.ai project page: _add link_
- Demo video (max 5 min): _add link_
- This repo: https://github.com/Alassana-Djigo/AI-Native-Contact-Center

## Credits

Built on the [AssemblyAI Voice Agent API](https://www.assemblyai.com/products/voice-agent-api) and the official [Python starter](https://github.com/AssemblyAI/voice-agent-starter-python).
