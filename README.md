Passcheck — ATS resume chatbot landing page
First: rotate your key
You pasted a live OpenAI key in this chat. Revoke it in your OpenAI dashboard
and generate a new one before doing anything else — treat the old one as
public.
What's in this project
index.html — the landing page and chat widget. Works standalone right
now with a small rule-based responder (no server, no key needed) so you
can see the scoping behavior immediately: on-topic ATS resume questions
get answered, everything else gets politely declined.
server.js — an optional Express backend that swaps the local responder
for a real OpenAI call, with your key kept server-side only.
.env.example — copy to `.env` and put your new key there.
Running just the landing page (no AI, demo mode)
Open `index.html` directly in a browser, or serve the folder:
```
npx serve .
```
The chat already works — it uses keyword matching and canned answers scoped
to resume/ATS topics, so you can hand this to someone today without an API
key at all.
Wiring up the real AI backend
`npm init -y`
`npm install express openai dotenv cors`
`cp .env.example .env` and paste your new key in
`node server.js`
In `index.html`, find the commented `getBotReplyFromServer` function near
the bottom of the `<script>` block and use it (with a `fetch` to
`/api/chat`) in place of the local `getBotReply` calls in `handleSubmit`
and `sendSuggested`.
Why the key isn't in index.html
Anything in frontend JavaScript is visible to every visitor via browser dev
tools. An API key there gets scraped and abused, often within minutes. The
key must live only on a server (`server.js` here), which the browser calls
instead of calling OpenAI directly. The scope restriction (ATS resumes only)
is enforced twice: a fast keyword check, and the system prompt sent to the
model — keep both, since a system prompt alone can sometimes be talked
around.
