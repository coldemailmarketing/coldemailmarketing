## emailcampaign.ai

We build cold email sending infrastructure — domains, mailboxes, DNS and
warm-up, set up and authenticated before the first campaign goes out.

Most of what we publish here comes out of running that at volume. The failures
that cost people their domains are rarely interesting: an SPF record that
returns a permanent error while looking perfectly fine in a registrar
dashboard, a DKIM CNAME copied from a guide that resolves to nothing, a
mailbox pushed past what its sending history supports. So the tools are small,
dependency-free, and aimed at the specific things that go wrong quietly.

### Open source

| | |
|---|---|
| **[spf-audit](https://github.com/coldemailmarketing/spf-audit)** | Audits SPF the way a receiving mail server does. Counts **every** `v=spf1` record on the host, not just the first, and walks the entire include tree against the ten-lookup limit. |
| **[dmarc-report-parser](https://github.com/coldemailmarketing/dmarc-report-parser)** | Reads DMARC aggregate reports — `.xml`, `.gz`, `.zip`, `.eml` or a whole Maildir — and answers whether it is safe to move a domain off `p=none`. |

Both are MIT, Python standard library only, and do not talk to the network
beyond the DNS lookups they exist to perform.

### A few things we keep having to explain

- **Two SPF records on one host is a permanent error, not a merge.** Both will
  look valid on their own. Published together, SPF cannot be evaluated at all.
- **The ten-lookup limit counts the whole tree.** `include:_spf.google.com` is
  four lookups, not one, because of what nests inside it. Four vendors is often
  enough to break a record that still looks short.
- **Microsoft 365 DKIM CNAMEs are per-tenant.** A value copied from someone
  else's setup resolves to nothing, signing silently never turns on, and
  nothing anywhere reports an error.
- **Eight to ten cold sends per mailbox per day** is what holds up over months
  — far under every published provider ceiling, because a published ceiling
  describes when a provider stops you, not when it starts distrusting you.
- **`dkim=pass` is only half an answer.** Read the `d=` beside it. If it is
  your provider's domain rather than yours, DMARC alignment still fails.

### Elsewhere

- [emailcampaign.ai](https://emailcampaign.ai) — the product
- [Free SPF, DKIM and DMARC checker](https://emailcampaign.ai/tools/dns-checker) — the same checks against live DNS, in a browser
- [docs.emailcampaign.ai](https://docs.emailcampaign.ai) — setup guides
- [LinkedIn](https://www.linkedin.com/company/emailcampaign-ai)

Email Campaign LLC · Delaware, United States · support@emailcampaign.ai
