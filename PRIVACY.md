# VStudy Privacy

**VStudy collects nothing.** No accounts, no VStudy servers, no telemetry.

That is the whole policy. The rest of this page just spells out what it means.

## No accounts, no servers

VStudy has no backend. There is nothing to sign up for, nothing to log in to,
and no VStudy server that your editor talks to. We could not collect your data
even if we wanted to — there is no place for it to go.

## No telemetry — removed, not just disabled

Telemetry is not "off by default" in VStudy. It is **removed from the build**.
There is no hidden setting to flip, no usage pings, no crash reporting to us,
no "anonymous metadata". Zero.

## Your API key stays yours

VStudy is bring-your-own-key. Your Anthropic or OpenAI API key is stored in
your operating system's keychain (secret storage), on your machine. When you
chat with the AI, requests go **directly from your machine to the provider you
chose** — Anthropic, OpenAI, or your local Ollama instance. Nothing passes
through VStudy, because there is no VStudy in the middle.

If you use Ollama, everything runs locally and no chat data leaves your
computer at all.

## Your learning data lives on your disk

Everything you create while learning — your Bubbles, your chats, your personal
library — is stored as plain files on your local disk under `~/.vstudy`.

- **Exporting** it is as simple as copying or zipping that folder.
- **Deleting** it is as simple as deleting the files.

No sync, no cloud copy, no one else has it.

## The only network calls are the ones you configure

VStudy only talks to the network when you tell it to:

- **Your model provider** — Anthropic, OpenAI, or your local Ollama, using your
  own key or your own machine.
- **The Open VSX extension gallery** — when you search for or install
  extensions.
- **Opt-in integrations** — anything else (like Discord presence) is off until
  you explicitly turn it on.

There are no calls to Microsoft telemetry endpoints, no calls to VStudy
servers (there are none), and nothing running in the background that phones
home.

## Questions

VStudy is open source — you can verify every claim on this page by reading the
code at <https://github.com/brunogo25/vstudy>. If something looks off, open an
issue or write to <bruno@guio.online>.
