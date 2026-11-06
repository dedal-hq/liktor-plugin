# Liktor plugin for Claude Code and Codex

[Liktor](https://dedal.ai/liktor/) gives your AI assistant the official texts of Albanian law, so it can answer
from the law itself and cite it. This plugin adds two things to Claude Code or Codex:

- the connection to the Liktor server (`https://liktor.dedal.ai/mcp`), with sign-in through your Dedal account;
- the `liktor-research` skill: how to search in Albanian legal language, open and check what it cites, find the
  version that applied on a date, follow implementing bylaws and amendments, read court decisions, and cite by
  article and id.

## What Liktor holds

- Laws and codes from the Official Journal (qbz.gov.al), with their consolidated versions
- Bylaws: decisions of the Council of Ministers (VKM), instructions, orders and regulations
- Decisions of the Constitutional Court and the High Court, cut into sections (facts, arguments, reasoning, ruling)
- ECHR judgments against Albania
- Vetting decisions of the Independent Qualification Commission (KPK) and the Special Appeal Chamber (KPA)
- EU law that Albanian laws transpose or refer to, in English

The texts are updated every night. Liktor holds official texts only, no commentary.

## Access

Liktor is open by invitation for now. [Ask for access](https://dedal.ai/liktor/) and you will get an email to set
up your Dedal account. Without an account the plugin installs, but the sign-in has nowhere to take you.

## Install

### Claude Code

```sh
claude plugin marketplace add dedal-hq/liktor-plugin
claude plugin install liktor@dedal
```

Start Claude Code, type `/mcp`, choose `plugin:liktor:liktor` and then **Authenticate**. Your browser opens: sign in
with your Dedal account.

If you added Liktor by hand before (`claude mcp add ... liktor.dedal.ai`), Claude Code keeps using that connection
and the skill works with it.

### Codex

```sh
codex plugin marketplace add dedal-hq/liktor-plugin
codex plugin add liktor@dedal
codex mcp login liktor
```

### Claude, ChatGPT, Cursor and VS Code

These apps connect to Liktor directly, without this plugin: follow the steps for your app at
[dedal.ai/liktor](https://dedal.ai/liktor/#lidhu).

## Ask it

- Sipas Kodit Civil, sa është afati i parashkrimit për detyrimet kontraktore?
- Cilat VKM zbatojnë ligjin për mbrojtjen e të dhënave personale?
- What did the Constitutional Court decide on the vetting law?
- Which version of the Labour Code article on annual leave applied in 2019?
- What changed in the Labour Code since January?

## Updates

Claude Code and Codex fetch new versions of the plugin from this repository: `claude plugin marketplace update dedal`
and `codex plugin marketplace upgrade`.

## Privacy

Your assistant sends Liktor the searches it makes and the ids of the texts it opens. To rank results, Liktor sends
each search and candidate passages to a reranking service in the EU (MongoDB's Voyage AI). The
[privacy policy](https://dedal.ai/privacy/) has the details.

## Support

contact@dedal.ai

## License

The plugin's files are licensed under the [PolyForm Internal Use License 1.0.0](LICENSE), with an added permission
for personal use: individuals and companies may use and adapt the plugin for their own needs, but may not
distribute it, or works based on it, to others. Access to the Liktor service needs a Dedal account.
