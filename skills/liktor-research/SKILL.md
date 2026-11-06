---
name: liktor-research
description: Research Albanian law with the liktor MCP server (search_law, get_article, get_law, get_related, get_changes, list_subjects) - laws and codes, bylaws (VKM, udhëzime, urdhra), Constitutional Court, High Court, ECHR judgments against Albania, the vetting bodies (KPK, KPA) and EU law as Albania aligns with it. Use for any question about what Albanian law says, what a court decided, which version applied on a date, what implements or amends an act, or what changed recently.
---

# Researching Albanian law with liktor

liktor holds the official texts: every law and code from qbz.gov.al with its consolidated versions, the
government's and other institutions' bylaws, the courts' decisions in sections, and EU acts in English. It does
not hold commentary or doctrine. Answer from the texts you open, cite them, and say what you could not find.

## 1. Read the question

Decide before searching:
- **A reference is given** ("neni 705 KC", "ligji 9131/2003 neni 5", "VKM nr. 123, datë 12.3.2024, pika 3",
  "vendimi 12/2024 i Gjykatës Kushtetuese"): open it directly with `get_article` or `get_law`; search only for
  what surrounds it.
- **What the law says** on a topic: legislation first, then the bylaws that implement it.
- **What courts decided**: case law (`act_type` per court, below).
- **Which rule applied on a date** (a contract of 2015, an act committed in 2019): versions matter (§4).
- **What changed / what is new**: `get_changes` (`since` a date; `law_id` for one act, an article
  "al/ligj/2015/136#neni-143", or a prefix such as "al/gjl"), not search.
- **EU law**: search in English; Albanian laws that transpose an act link to it (`get_related`).

## 2. Search

`search_law` without `act_type` returns groups ranked separately: `legislation` (always), then `bylaws`,
`caselaw` and `eu` only when relevant. Read every group it returns, not just the first hit.

- **Write queries in Albanian legal language**, the words the text itself would use: "afati i parashkrimit për
  detyrimet kontraktore", not a translation of the user's English. Diacritics are optional.
- **Try two or three phrasings** when the first results are thin: the user's own words, the legal term
  ("shpërblim dëmi" / "dëmshpërblim"), and the institution or procedure by name. Short heading-like queries
  ("afati i ankimit", "pushimi vjetor") find articles by their headings.
- **effort**: `standard` (default, reranked, ~2 s) for nearly everything; `thorough` when completeness matters or
  the answer sits deep in long court decisions (larger pools, best passages, version status and links per hit);
  `fast` only to look up something you can already name.
- **Filters**: `act_type` ("ligj", "vendim", "gjk", "gjl", "gjednj", "kpa", "kpk", "regulation,directive,decision"),
  `law_id` to search inside one law, `year_from`/`year_to`, `subject` (slugs from `list_subjects`).
- If `note` says the reranker was unavailable or the effort was lowered, the order is rougher: open more hits.

## 3. Open and check what you cite

Snippets are for choosing; never answer from a snippet alone.
- `get_article(id, context=1)` gives the article with its neighbours, `is_latest`, `version` and
  `amendment_notes`. Read the neighbours: exceptions and definitions are often in the next article.
- `get_law(id, mode="toc")` shows the structure; use it to find the chapter a rule belongs to, or the
  definitions article.
- A hit with `repealed_by` is from a repealed (or partly repealed) law: find the law that replaced it
  (`get_related(law, "repealed_by")`) and answer from that, mentioning the old one only as history.

## 4. Versions and dates

Search covers the latest consolidated versions. For an earlier date, search with `all_versions=true` or open the
law's versions (`get_law(id, mode="meta")`) and open the one in force then (`get_law(id, version=...)`, or
`get_article` with that version in the id). Consolidation dates (`@cons-2026-08-10`) are
**publication dates of the consolidated text, not entry-into-force dates**: when the exact date of a change
matters, check the amending law (`get_related(law, "amended_by")`) and its own entry-into-force provision.

## 5. Follow the links

`get_related(law_id)` connects the corpus:
- `implemented_by` / `based_on`: the bylaws issued under a law (fees, procedures, forms, deadlines often live
  there, not in the law);
- `amended_by` / `amends`, `repealed_by` / `repeals`;
- `cited_by` / `cites`: court decisions citing a law or another decision; `invalidated_by`: the Constitutional
  Court striking a provision down. Check `invalidated_by` before relying on a contested provision;
- `transposes` / `references`: the EU acts behind a law.

## 6. Court decisions

Decisions are cut into sections: `#sec-header` (parties, OBJEKTI), `#sec-rrethanat` (facts), `#sec-pretendimet`
(arguments), `#sec-vleresimi` or `#sec-arsyetimi` (the court's reasoning), `#sec-dispozitivi` (the ruling),
`#sec-mendim` (dissents).
- **The title names what was asked (Objekti), not what was decided.** Read `outcome` or `#sec-dispozitivi` before
  saying who won. "Rrëzimin e kërkesës" means the request failed.
- Quote the court's reasoning (`vleresimi` / `arsyetimi`), not the parties' arguments, as the court's view.
- Weight: the Constitutional Court (`gjk`) and ECHR judgments against Albania (`gjednj`) bind; High Court
  decisions with `practice="unifying"` unify case law; other High Court decisions are persuasive.
- **Vetting**: the Independent Qualification Commission (`kpk`, first instance, 2017-2024) decided; the Special
  Appeal Chamber (`kpa`) heard the appeals. For a vetted judge or prosecutor, find the KPK decision, then
  `get_related(kpk_id, "cited_by")` for the appeal, and report the final outcome (the appeal's, if there was
  one). KPK decisions are long: the asset findings are in `#sec-pasuria`, the background check in
  `#sec-figura`, professional skills in `#sec-profesional`, the hearing in `#sec-pretendimet`.

## 7. Common abbreviations

Search and open by number; these are the codes lawyers abbreviate:
Kushtetuta 8417/1998 · KC Kodi Civil 7850/1994 · KPC Kodi i Procedurës Civile 8116/1996 · KP Kodi Penal
7895/1995 · KPP Kodi i Procedurës Penale 7905/1995 · Kodi i Procedurave Administrative 44/2015 · Kodi i Punës
7961/1995 · Kodi i Familjes 9062/2003 · VKM = vendim i Këshillit të Ministrave (`act_type="vendim"`).
"KPA" is ambiguous: the Code of Administrative Procedure, or the Special Appeal Chamber (Kolegji i Posaçëm i
Apelimit) in vetting matters.

## 8. Answer

- Answer in the user's language; quote the decisive sentence of the text in Albanian.
- Cite every rule by article and law, in words and by id: "neni 705 i Kodit Civil (ligji nr. 7850/1994),
  al/ligj/1994/7850@cons-2026-08-10#neni-705". Name the version you read.
- Separate what the law says, what bylaws add, and how courts applied it.
- Say plainly what you did not find or could not confirm (a missing bylaw, an unclear date of entry into force,
  no case law), and what you searched for. Do not fill gaps from memory: liktor's texts are newer than your
  training.
