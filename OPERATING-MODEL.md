# MMA Operating Model v1

| | |
|---|---|
| Status | **Draft** → LIVE po schválení **ADR-0001** v `MMA-Analytics/prakesh` (four-eyes pka ↔ Mirko, Telegram) |
| Verzia · dátum | v1.0 · 2026-09-29 |
| Owner | MMA-Analytics · DECIDER pka |
| Platí pre | všetky repozitáre MMA-Analytics (aquien, aquien-bbrsc, xerxes, tzdravie, cz-sport, metamod-web, infra, prakesh, cpag, 3D svet) a všetkých agentov (Claude, Claude Code, Codex, GPT, agenti PRAKESH) |
| Domov (cieľ) | `MMA-Analytics/.github/OPERATING-MODEL.md` + šablóny v `.github` + skills v plugine `mma-delivery` |
| Nahrádza | `tzdravie-v2/docs/00_agent-governance.md` · `xerxes-bridge/docs/WORKING_AGREEMENT.md` + `docs/policy/governance.md` · `cz-sport-infra/docs/00_master/WORKING_AGREEMENT.md` · procesné časti `cz-sport-infra/AGENTS.md`, `DELIVERY_MODEL.md`, `cpag` CPA-005/010 (§ gate a ľudské vlastníctvo). Pôvod pravidiel: §14. |
| Súvisí | `prakesh/docs/architecture/target.md` (§3.1 roly, §6 spoločný jazyk, E1) |

> Tento dokument hovorí, **ako** pracujeme. Nenesie produktovú pravdu, architektúru ani schémy projektov; tie majú svoje sloty (§9).
> Oproti rámcom z praxe (§13) nič nevymýšľa, iba ich spája s pravidlami, ktoré sme si overili v našich repách.

---

## 1 · Hierarchia pravdy (kto vyhráva pri rozpore)

| Poradie | Zdroj | Rozhoduje o |
|---|---|---|
| 1 | Evidenčný fakt (`08_evidence`, gate report s dôkazom) | čo bolo preukázané; **nič ho neprepíše**, ani ADR |
| 2 | `Accepted` ADR (firemné v `MMA-Analytics/prakesh/docs/07_decisions`, projektové v `07_decisions` projektu) | rozhodnutia, pravidlá, architektonické obmedzenia |
| 3 | **Tento Operating Model** | roly, gates, tok práce, šablóny, vynucovanie |
| 4 | `AGENTS.md` projektu | obsadenie rolí a projektové pravidlá (§11); zúženie áno, uvoľnenie iba cez ADR |
| 5 | Schválený brief (slice) | rozsah konkrétnej práce |
| 6 | Chat, správa, pamäť agenta | nič; čo nie je v gite alebo v PRAKESH, neplatí |

Mimo svojej domény nerozhoduje žiadna úroveň. Rozpor medzi úrovňami → **Decision Request** (T4), nie tichá zmena.

---

## 2 · Princípy

| # | Princíp | Prakticky |
|---|---|---|
| G1 | **AI navrhuje, človek schvaľuje** | agent dodá návrh + zdôvodnenie + otvorené otázky; merge, výnimka a zmena významu = človek; AI nikdy neschvaľuje samu seba |
| G2 | **Four-eyes** | autor nesmie schváliť vlastný návrh; reviewer nikdy nerecenzuje vlastnú prácu |
| G3 | **Rola ≠ model** | pravidlá sa viažu na rolu; ktorý model ju hrá, určuje `AGENTS.md` projektu |
| G4 | **Git je kanonická pamäť** | rozhodnutie, stav a dôkaz sú v gite (od E2 aj v PRAKESH), nie v chate |
| G5 | **Kontrakt pred implementáciou** | najprv pravidlá a akceptačné kritériá, potom kód; bez AC nie je implementácia |
| G6 | **Empíria nad špekuláciou** | oprav a premeraj; hypotézu over pred diffom; tvrdenie bez dôkazu je hypotéza |
| G7 | **Pomenuj koreň, nie symptóm** | ak KROK A zmení formuláciu problému, práca stojí do nového rozhodnutia |
| G8 | **Malé vertikálne slicy** | end-to-end schopnosť na reálnych dátach; nie vrstva po vrstve |
| G9 | **Adopt before build** | hotový štandard alebo nástroj je východisko; odchýlka iba s ADR |
| G10 | **Abstrakcia až pri 2.–3. konzumentovi** | žiadne frameworky, premenovania ani platformy dopredu; nový dokument musí eliminovať ≥2 iné |
| G11 | **Čestný obraz** | výsledok sa nenafukuje; caveaty, dlh a UNVERIFIED RISK sa zapisujú |
| G12 | **Citlivé dáta ostávajú doma** | zdravotné a klientske dáta nevstupujú do spoločných nástrojov (PRAKESH P8) |

---

## 3 · Roly

| Rola | Smie | Nesmie |
|---|---|---|
| **DECIDER** | autorizovať commit GO, push, merge, výnimky z tohto dokumentu; rozhodnúť spor po 2 kolách | — |
| **ARCHITECT** | plán, brief, kontrakt slicu, adjudikácia review sporov, Decision Request DECIDEROVI | zapisovať do repa |
| **EXECUTOR** | jediný zápis v session (v jednej session práve jeden); Decision Request pri rozpore | rozširovať rozsah, meniť architektúru potichu, commit/push bez pokynu |
| **REVIEWER** | námietky, overenie dotazom do dát, verdikt | vykonávať vlastnú výhradu (formuluje, EXECUTOR vykoná); recenzovať vlastnú prácu |
| **Schvaľovateľ (four-eyes)** | schváliť/zamietnuť návrh druhého | schváliť vlastný návrh |

**Rodiny agentov** (register v `prakesh/target.md` §5):

| Rola | Agenti dev oddelenia |
|---|---|
| ARCHITECT, fázy 1–4 | @produkt (zámer) · @analytik (dôkazy) · @ux (návrh) · @architekt (riešenie, ADR, brief) |
| EXECUTOR | Claude Code · Codex · @databaza · @developer |
| REVIEWER | @tester · @stavbyveduci · GPT (second opinion) · Claude chat (adversariálne na požiadanie) |

**Obsadenie per projekt** je v `AGENTS.md` (T1). Východisko podľa dnešnej praxe:

| Projekt | DECIDER | ARCHITECT | EXECUTOR | REVIEWER | Schvaľovateľ |
|---|---|---|---|---|---|
| tzdravie | pka | Claude (chat) | Claude Code · Codex | GPT | Mirko |
| xerxes | pka | Claude (chat) | Claude Code | GPT (filtrovaný, §7) | Mirko |
| cz-sport | pka | ChatGPT | Claude Code | Claude (chat) na požiadanie | Mirko |
| aquien · aquien-bbrsc · metamod-web · infra · cpag | pka | určí pka v `AGENTS.md` | Claude Code | určí pka | Mirko |
| prakesh | pka | Claude (chat) | Claude Code | GPT · @stavbyveduci | Mirko |
| 3D svet | Mirko | určí Mirko | určí Mirko | určí Mirko | pka |

---

## 4 · Tok práce

```
DECIDE ─► DESIGN ─► BRIEF ─► EXECUTE ─► PROVE ─► REVIEW ─► ACCEPT ─► MERGE
DECIDER   ARCHITECT ARCHITECT EXECUTOR  EXECUTOR REVIEWER  DECIDER   DECIDER
 │          │         │         │          │        │         │
 │          │         │         └─ rozpor s briefom → DECISION REQUEST (T4) ─► DECIDER
 │          │         └─ kontrakt slicu (T3): bez AC nie je implementácia
 │          └─ ADR (T9), ak ide o skutočné rozhodnutie
 └─ Decision Request / zámer
```

| Krok | Výstup | Kde žije |
|---|---|---|
| DECIDE | rozhodnutie alebo zámer | ADR (ak je architektonické) · issue |
| DESIGN | kontrakt, návrh riešenia | `04_contracts` · ADR |
| BRIEF | brief + kontrakt slicu (T2, T3) | issue (od E2 entita Brief v PRAKESH); do repa nie |
| EXECUTE | vetva + commity | PR |
| PROVE | testy + runtime dôkaz ku každému AC (T7) | PR popis; trvalý dôkaz → `08_evidence` |
| REVIEW | verdikt (T6) | PR review |
| ACCEPT | ACCEPT / ACCEPT WITH DEBT / REJECT | PR · PRAKESH (Telegram four-eyes) |
| MERGE | `main` | iba DECIDER, cez explicitný hash |

Mapovanie na Spec Kit: *constitution* = §2 + projektové pravidlá · *specify* = zámer + AC · *plan* = DESIGN/ADR · *tasks* = kontrakt slicu · *implement* = EXECUTE · *converge* = PROVE + REVIEW.

### 4.1 Delivery pravidlá

| Pravidlo | Znenie |
|---|---|
| Progressive elaboration | detail vzniká, až keď ho potrebuje najbližšia implementácia alebo rozhodnutie |
| One-phase runway | architektúra smie byť detailná najviac o jednu fázu dopredu |
| Vertikálny slice | zdroj → ingest → model → perzistencia → API → viditeľný výsledok, nie „všetka DB, potom backend…“ |
| Anti-overscope | bez `Accepted` ADR nezavádzať: mikroslužby, Kubernetes, event bus, grafovú/vektorovú DB, agentový framework, ďalší perzistenčný engine, samostatnú analytickú platformu. Nová technológia rieši preukázaný problém, nie hypotetický. |
| Budúce fázy | neimplementovať oportunisticky |
| Experiment | spúšťa sa len s produktovým dôvodom (odblokuje implementáciu, zmení rozhodnutie, spätná väzba zákazníka); zvedavosť nie je dôvod |
| Predregistrácia | kritériá PASS/FAIL a atribúty sa zapíšu pred pohľadom na dáta |
| Definícia pokroku | nová overiteľná end-to-end schopnosť na reálnych dátach; nie počet dokumentov, LOC, služieb ani AI funkcií |

---

## 5 · Gates EXECUTORA (každý agent, bez výnimky)

| # | Gate | Pravidlo | Prečo (doložené) |
|---|---|---|---|
| E1 | **cd-guard** | `pwd` + `git remote -v` pred každým zápisom; commit len v repe, ktoré vlastní daný typ pravdy | tzdravie: docs vs kód v dvoch repách |
| E2 | **Baseline** | brief nesie `BASELINE: main @ <SHA>`; nesedí → Decision Request | cz-sport |
| E3 | **KROK A (read-only)** | najprv over, ako to funguje dnes; potom navrhni zásah | xerxes |
| E4 | **Hypothesis gate** | otestuj hypotézu pred písaním diffu | xerxes: 3× zachránené pred nefunkčným kódom |
| E5 | **Reformulation gate** | ak KROK A zmení formuláciu problému → STOP, nové rozhodnutie | xerxes Wave 23: 3× premenovaný problém |
| E6 | **Plan → STOP** | plan mode, review plánu pred zápisom | tzdravie |
| E7 | **Rollback pripravený** | pred smoke/zásahom je známy návrat (`git checkout HEAD -- <file>`, snapshot) | xerxes |
| E8 | **Literálny diff → STOP** | reviewer dostane diff, nie tabuľku ani zhrnutie | xerxes: „tabuľka klame, diff nie“ |
| E9 | **Fresh reader check** | pred diff gate prečítaj hlavičku + ~10 riadkov každého LIVE dokumentu, ktorého sa session dotkla; tvrdí pravdu o aktuálnom stave? oprava mimo rozsahu sa hlási, nerobí | tzdravie: platná cesta, nepravdivé tvrdenie |
| E10 | **Validácia bez zdieľanej slepej škvrny** | kontrola iným vzorom, než ktorý opravuje + runtime dôkaz, nie iba grep | tzdravie, prakesh |
| E11 | **Render gate** | pri UI/vizuáli vizuálny dôkaz (screenshot), nie iba dáta | xerxes |
| E12 | **Žiadny čiastočný smoke** | necommituje sa na čiastočný smoke, ani „výnimočne“ | xerxes |
| E13 | **File-by-file add** | nikdy `git add -A`; po `add` over **obsah indexu** (`git diff --cached`, pri presunoch `git show :cesta`), nie `git status`; bez opportunistic cleanup | tzdravie S3e.3 |
| E14 | **Spätná kompatibilita** | nová cesta vedľa starej; stará bitovo nezmenená (over v diffe) | xerxes |
| E15 | **Commit** | iba na pokyn DECIDERA; Conventional Commits; trailer so stabilným menom nástroja (`Co-Authored-By: Claude Code <noreply@anthropic.com>` · `Co-Authored-By: Codex <noreply@openai.com>`) | tzdravie: trailer = rola, nie build |
| E16 | **Push / merge** | iba na pokyn DECIDERA, cez explicitný hash; nikdy do `main`, nikdy `--no-verify`, nikdy force | xerxes, prakesh |
| E17 | **Prod read-only** | zápis do prod DB/servera len ak ho brief povoľuje, v pre-registrovanom rozsahu (počet záznamov, okno) | tzdravie |
| E18 | **Dôkaz ku každému AC** | slice končí dôkazom proti každému akceptačnému kritériu (T7); testy sú súčasť implementácie | cz-sport |

---

## 6 · Bezpečnosť

| # | Pravidlo |
|---|---|
| S1 | Tajomstvá nikdy do chatu, commitu, logu ani PR; obsah `.env` sa nevypisuje |
| S2 | Kód z iného repa sa nespúšťa (iba čítanie) |
| S3 | Agent nemá právo push do `main` (deny rules + pre-push hook + ruleset) |
| S4 | SSH prístup agentov iba cez obmedzeného používateľa (`prakesh-ro`, read-only) |
| S5 | Zdravotné a klientske dáta ostávajú v produkte (G12) |
| S6 | CI skenuje tajomstvá a zakázané auth vzory v automatizačnom kóde (vzor xerxes `policy-check`) |

---

## 7 · Review protokol

| Pravidlo | Znenie |
|---|---|
| Krížom | reviewer ≠ autor, ideálne iný model/poskytovateľ ako EXECUTOR |
| Max 2 kolá | kolo 1: námietky + overenie dotazom do dát · kolo 2: rezíduá · zvyšok rozhodne DECIDER alebo zapíše ako OPEN; výnimku povoľuje iba DECIDER |
| Námietka bez dát | empirická námietka bez súboru/query/čísla = 1-vetová odpoveď, nie kolo |
| UNVERIFIED RISK | architektonické, bezpečnostné a regulačné hypotézy bez dát sa **nezamietajú**; označia sa a evidujú v `05_backlog` |
| Verdikt | ACCEPT · ACCEPT WITH DEBT (dlh sa zapíše s vlastníkom) · REJECT (s dôvodom) |
| Filtrovanie second opinion | **BER** vecné (invarianty, REQUIRED/OPTIONAL/FORBIDDEN, scenárový audit, diagnostika) · **ODMIETNI** predčasné (rename, framework, platforma dopredu). Rebrand ≠ hodnota. |
| Čestnosť | uznať, keď má druhá strana pravdu; uznať vlastnú chybu; neprijímať pochvalu naslepo |
| Spor po 2 kolách | záznam: dátum · artefakt · pozície · verdikt DECIDERA / OPEN (v projekte `08_evidence/review-log.md`, od E2 v PRAKESH) |

---

## 8 · Komunikácia

| Pravidlo | Znenie |
|---|---|
| Jazyk | slovensky, ≤5 viet prózy; zvyšok tabuľky, kód, diagramy; bez vaty |
| Chyba | príčina → oprava → overenie |
| Rozhodnutie | jedno rozhodnutie = jedna otázka; možnosti + odporúčanie; produktové veci nerozhoduje agent |
| Odovzdávka | medzi agentmi vždy handoff packet (T5), nie copy-paste prepis |
| Drift | „drž Operating Model“ → agent sa vráti k §2 a §5 |
| Názvy chatov/taskov | `[Wave]-[typ]: predmet`; typ = VERIFY · KÓD · DOC · DECISION · SESSION (SESSION končí zápisom do `06_status/handoff.md`) |

---

## 9 · Dokumenty v repe

### 9.1 Sloty (1 typ pravdy = 1 domov)

| Slot | Nesie | Nesie nie | arc42 |
|---|---|---|---|
| `AGENTS.md` (root) | obsadenie rolí, projektové pravidlá, odkaz sem | schémy, stav | — |
| `docs/01_identity` | positioning, čo produkt je a nie je | roadmap | 1 |
| `docs/02_architecture` | živý dokument architektúry (arc42 + C4) | polia schém | 3–8, 10–11 |
| `docs/03_product` | čo staviame a čo nie (PRD) | poradie práce | 1 |
| `docs/04_contracts` | rozhrania, schémy, polia — čo platí dnes | zdôvodnenie | 5, 8 |
| `docs/05_backlog` | neistota: otvorené otázky, blokátory, UNVERIFIED RISK, dlh, **`dead-ends.md`** | poradie | 11 |
| `docs/06_status` | `handoff.md` — jediný §NEXT FOCUS | produktový scope | — |
| `docs/07_decisions` | ADR (MADR), append-only | aktuálny stav rozhraní | 9 |
| `docs/08_evidence` | dôkazy, gate reporty s trvalou hodnotou, register datasetov (`data/`) | scope, poradie | 10 |
| `docs/09_archive` | historické záznamy | nič živé | — |

Projekt smie slot vynechať, ak ho nepotrebuje; nesmie vytvoriť nový typ pravdy mimo slotov. **Doc, ktorý nesedí presne do slotu, nevzniká.**

### 9.2 Pravidlá

| # | Pravidlo |
|---|---|
| D1 | **Aktualizuj, nepridávaj** — zmena stavu = edit živého dokumentu, nie nový súbor; nový dokument musí eliminovať ≥2 iné |
| D2 | Pracovné artefakty (brief, gate report, review, handoff) žijú v issue/PR (od E2 v PRAKESH); do repa iba to, čo má trvalú hodnotu (→ `08_evidence`) |
| D3 | Iba `06_status/handoff.md` §NEXT FOCUS určuje, čo je ďalej; iný dokument nepoužíva „NEXT“ |
| D4 | Normatívne závislosti smerujú `06` → `05` → `03` → `08`; opačný odkaz iba ako spätná trasovateľnosť, nikdy delegácia autority |
| D5 | Každý normatívny dokument nesie **Status + dátum**; bez nich nie je normatívny |
| D6 | Status enum: `Draft` · `LIVE` · `FROZEN` (zmena = nová verzia) · `SUPERSEDED` (**musí ukazovať na nástupcu**) · `ARCHIVED`. ADR: `Proposed` · `Accepted` · `Superseded` |
| D7 | ADR iba pri skutočnom rozhodnutí; append-only; zmena = nový ADR, ktorý starý označí `Superseded` |
| D8 | Nový rad identifikátorov má prefix ≥2 znaky viazaný na register (nie `A2`, `B3`) |
| D9 | CI povoľuje v `docs/` iba cesty zo slotov (doc-path allowlist) |
| D10 | Hranice: architektúra = *že* doména existuje · kontrakt = *polia* · ADR = *prečo* · kontrakt = *čo platí dnes* |

### 9.3 Uzavreté cesty (`05_backlog/dead-ends.md`)

Uzavretá cesta **nie je zakázaná** — každá má podmienku znovuotvorenia; keď nastane, cesta sa otvára bez ďalšej diskusie. Riadok podľa T8. „Netestovateľné“ a „neplatné“ sa nezamieňajú.

---

## 10 · Vynucovanie (pravidlo bez mechanizmu je želanie)

| # | Mechanizmus | Vynucuje | Kde |
|---|---|---|---|
| V1 | **Org ruleset** na `main` všetkých repov: PR povinný, 1 approval, approval po poslednom pushi, vyriešené konverzácie, required checks, vetva aktuálna, zákaz force-push, zákaz mazania, žiadny bypass | E15, E16, S3, G2 | `MMA-Analytics` → Settings → Rulesets (plán GitHub Team) |
| V2 | **CODEOWNERS** na chránené cesty: `/.github/`, `/docs/07_decisions/`, `/docs/04_contracts/`, `/ops/`, `/cron/`, `/tools/`, `AGENTS.md` | G1, D7 | každý repo (šablóna T10) |
| V3 | **Required checks**: testy · lint · `policy-check` (tajomstvá, auth vzory) · `docs-allowlist` · `commitlint` | E18, S6, D9, E15 | reusable workflows v `MMA-Analytics/.github` |
| V4 | **PR šablóna** s poliami: baseline SHA, AC → dôkaz, fresh reader check, UNVERIFIED RISK, dlh | E2, E9, E18 | `.github/pull_request_template.md` (T10) |
| V5 | **Issue šablóny**: Brief, Decision Request | §4 | `.github/ISSUE_TEMPLATE/` |
| V6 | **Lokálne poistky agenta**: deny rules (`git push origin main`, `--no-verify`, `git add -A`), pre-push hook | E13, E16 | `.claude/settings.json`, hooks v `mma-delivery` |
| V7 | **Skills** `/brief` · `/decision-request` · `/review` · `/handoff` · `/gate-report` | šablóny T2–T7 | plugin `mma-delivery` |
| V8 | **AI review v PR** (claude-code-action) ako REVIEWER, nikdy ako schvaľovateľ | §7 | reusable workflow |
| V9 | **Four-eyes za behu**: návrh → Telegram → druhý človek | G1, G2 | PRAKESH (L5) |
| V10 | **release-please** | changelog bez ručných dokumentov | reusable workflow |

Kde plán GitHub nevie pravidlo vynútiť, kompenzuje ho CI + review a zapíše sa ako dlh.

---

## 11 · Distribúcia a adopcia

### 11.1 Rozloženie

```
MMA-Analytics/.github/
├── OPERATING-MODEL.md            ← tento dokument
├── pull_request_template.md
├── ISSUE_TEMPLATE/{brief.yml,decision-request.yml}
├── workflow-templates/ + .github/workflows/ (reusable: policy-check, docs-allowlist, commitlint, ai-review, release)
└── templates/{AGENTS.md,CODEOWNERS,dead-ends.md,handoff.md}

každý repo:
├── AGENTS.md          ← T1: odkaz sem + obsadenie rolí + projektové pravidlá
├── CLAUDE.md          ← jediný riadok: @AGENTS.md (+ runtime detaily, ak treba)
├── .github/CODEOWNERS
└── docs/01…09         ← sloty podľa §9
```

### 11.2 Adopcia v existujúcich repách (samostatný slice per repo, E1)

| Repo | Existujúci dokument | Po adopcii |
|---|---|---|
| tzdravie-v2 | `00_agent-governance.md` | `SUPERSEDED` → odkaz sem; taxonómia a ADR-P09 ostávajú (sú zdrojom §9) |
| tzdravie | `CLAUDE.md` §Pracovný režim | odkaz sem; runtime a zmrazené kontrakty ostávajú |
| xerxes-bridge | `WORKING_AGREEMENT.md`, `policy/governance.md` | `SUPERSEDED` → odkaz sem; OLAP/Navigation Context pravidlá → `AGENTS.md` (projektové) |
| cz-sport-infra | `WORKING_AGREEMENT.md`, `DELIVERY_MODEL.md`, `AGENTS.md` | WA → `SUPERSEDED`; DELIVERY_MODEL si nechá iba projektovú časť (PostGIS, OSM, golden dataset); `AGENTS.md` podľa T1 |
| aquien-platform | `DEAD_ENDS.md`, gate dokumenty | `DEAD_ENDS.md` → `docs/05_backlog/dead-ends.md`; gate dokumenty → `08_evidence` |
| cpag | CPA-005/010 | ostávajú (doménový model AI operácií); pravidlo gate/human ownership odkazuje sem |
| prakesh | `CLAUDE.md`, briefy | `AGENTS.md` podľa T1 |

---

## 12 · Zmena tohto dokumentu

| Pravidlo | Znenie |
|---|---|
| Kto | návrh ktokoľvek (PR do `.github`); schvaľuje DECIDER + schvaľovateľ (four-eyes) |
| Ako | zmena pravidla = PR do `.github` + firemný ADR v `MMA-Analytics/prakesh` (schválenie v Telegrame); oprava textu bez zmeny významu = PR bez ADR |
| Verzia | minor = nové pravidlo/šablóna · major = zmena rolí, hierarchie alebo gates |
| Uvoľnenie | projekt smie pravidlo sprísniť v `AGENTS.md`; uvoľniť iba cez ADR |

---

## 13 · Prebraté štandardy (Adopt before build)

| Štandard | Čo z neho berieme | Kde v dokumente |
|---|---|---|
| AGENTS.md (otvorený formát pre inštrukcie agentov) | vstupný bod pre všetkých agentov v repe | T1, §11 |
| GitHub Spec Kit | tok constitution → specify → plan → tasks → implement | §4 |
| arc42 + C4 | štruktúra živej architektúry | §9.1 |
| MADR | formát ADR | T9 |
| GitHub rulesets, CODEOWNERS, required checks | vynucovanie | §10 |
| Conventional Commits + release-please | commity a changelog | E15, V10 |
| Two-person rule / four-eyes | schvaľovanie | G2, V9 |
| BMAD-METHOD | iba pomenovanie rolí dev oddelenia; metódu ako celok nepreberáme (vlastné persony a artefakty = druhé koleso) | §3 |

---

## 14 · Pôvod pravidiel

| Zdroj | Pravidlá |
|---|---|
| tzdravie-v2 `00_agent-governance.md` | §3 roly, E1, E6, E9, E13, E15–E17, §7 max 2 kolá + UNVERIFIED RISK, T5 |
| tzdravie-v2 taxonómia + ADR-P09 | §1 (evidencia nad ADR), §9 sloty, D3–D6, D8, D10 |
| tzdravie CLAUDE.md / tzdravie-v2 CLAUDE.md | E10, §8 názvy |
| xerxes WORKING_AGREEMENT | G6, G7, G10, G11, E3–E5, E7, E8, E11, E12, E14, §7 filtrovanie |
| xerxes policy + CI | V1, V2, V3 `policy-check`, S6 |
| cz-sport WORKING_AGREEMENT + AGENTS.md | §4 tok, E2, E18, T4, G4, „bez AC nie je implementácia“ |
| cz-sport DELIVERY_MODEL | §4.1, T3 (11 bodov) |
| aquien DEAD_ENDS + gate dokumenty | §9.3, T8, T7, experiment + predregistrácia |
| cpag CPA-005/010 | G1 (AI navrhuje, človek vlastní gate a význam) |
| prakesh target | G12, S3, S4, V9, rodiny agentov |

---

# Príloha — šablóny

## T1 · `AGENTS.md` (každý repo)

```markdown
# AGENTS.md — <repo>

Platí **MMA Operating Model**: https://github.com/MMA-Analytics/.github/blob/main/OPERATING-MODEL.md
Tento súbor ho iba dopĺňa (sprísniť smie, uvoľniť nie).

## Obsadenie rolí
| Rola | Kto |
|---|---|
| DECIDER | pka |
| ARCHITECT | <Claude (chat) / ChatGPT / …> |
| EXECUTOR | <Claude Code / Codex> |
| REVIEWER | <GPT / Claude chat / @tester> |
| Schvaľovateľ | <Mirko> |

## Čítaj pred implementáciou
1. docs/06_status/handoff.md (§NEXT FOCUS)
2. relevantné ADR v docs/07_decisions
3. aktuálny brief (issue #…)

## Projektové pravidlá
- <napr. PostgreSQL/PostGIS je System of Record>
- <napr. zmrazené kontrakty: …>

## Príkazy
- test: `<…>` · lint: `<…>` · run: `<…>`
```

## T2 · Brief

```markdown
# BRIEF-<PROJ>-<NNN> · <predmet>
| | |
|---|---|
| Wave · typ | <W1–W3> · <VERIFY/KÓD/DOC/DECISION> |
| DECIDER · ARCHITECT · EXECUTOR · REVIEWER | … |
| BASELINE | main @ <SHA> |
| Súvisí | ADR-…, issue #… |

## Cieľ (1–3 vety)
## Pravidlá (R1…): vetva, zákazy, čo sa nesmie dotknúť
## Kontrakt slicu → T3
## Po zlúčení (iba na pokyn DECIDERA)
## Mimo rozsahu
```

## T3 · Kontrakt slicu (11 bodov)

| # | Pole | Obsah |
|---|---|---|
| 1 | Cieľ | jedna overiteľná end-to-end schopnosť |
| 2 | Baseline SHA | `main @ <SHA>` |
| 3 | Vstup | dáta, súbory, závislosti |
| 4 | Výstup | čo existuje po slici |
| 5 | V rozsahu | zoznam |
| 6 | Výslovne mimo rozsahu | zoznam |
| 7 | Dátový/API kontrakt | odkaz do `04_contracts` alebo „n/a“ |
| 8 | Akceptačné kritériá | K1…Kn, každé overiteľné |
| 9 | Povinné testy | ktoré, kde |
| 10 | Povinný dôkaz | ku každému K: príkaz, výstup, screenshot, sha |
| 11 | Stop podmienka | kedy EXECUTOR zastaví a pošle Decision Request |

## T4 · Decision Request

```text
DECISION REQUEST · <repo> · <brief/slice> · <dátum>
Požadované:            čo brief chce
Zistené:               čo je realita (s dôkazom)
Konflikt:              v čom sa to bije
Možnosti:              A) … B) … C) …
Odporúčanie:           … (prečo)
Bezpečne dokončené:    čo je hotové a platí bez ohľadu na voľbu
Blokované:             čo čaká na rozhodnutie
```

## T5 · Handoff packet

```text
HANDOFF · <dátum>
Task:          BRIEF-… / issue #…
Executor:      …            Reviewer: …
Scope:         commit <hash> / diff <base>..<head>
Findings:      súbor:riadok — popis (otvorené)
Debt:          … (vlastník)
Pre DECIDERA:  rozhodnutie, ktoré treba
```

## T6 · Review verdikt

```text
REVIEW · <artefakt> · kolo <1|2> · reviewer <…>
Námietky:        N1 … (dôkaz: súbor/query/číslo)
UNVERIFIED RISK: R1 … (bez dát, evidovať)
Verdikt:         ACCEPT | ACCEPT WITH DEBT (dlh: …, vlastník: …) | REJECT (dôvod: …)
```

## T7 · Gate report (PROVE)

| AC | Dôkaz (príkaz → výstup / screenshot / sha256) | PASS/FAIL |
|---|---|---|
| K1 | … | … |

Doplnky: fresh reader check (dokumenty + nálezy) · testy (počet, zelené) · rollback overený · UNVERIFIED RISK · dlh.

## T8 · Uzavretá cesta

| Cesta | Čo sme si mysleli | Čo ukázal experiment | Prečo sa tým teraz nezaoberáme | Čo by ju znovu otvorilo |
|---|---|---|---|---|

## T9 · ADR (MADR)

```markdown
# ADR-<NNNN> — <rozhodnutie>
Status: Proposed | Accepted | Superseded by ADR-… · Dátum · DECIDER · Schvaľovateľ
## Kontext a problém
## Zvažované možnosti
## Rozhodnutie (+ zdôvodnenie)
## Dôsledky (pozitívne, negatívne, dlh)
## Súvisí
```

## T10 · `CODEOWNERS` + PR šablóna

```text
# CODEOWNERS (tím obsahuje pka aj Mirka → four-eyes funguje oboma smermi)
*                      @MMA-Analytics/deciders
/.github/              @MMA-Analytics/deciders
/docs/07_decisions/    @MMA-Analytics/deciders
/docs/04_contracts/    @MMA-Analytics/deciders
/ops/                  @MMA-Analytics/deciders
/cron/                 @MMA-Analytics/deciders
/tools/                @MMA-Analytics/deciders
/AGENTS.md             @MMA-Analytics/deciders
```

```markdown
<!-- pull_request_template.md -->
## Brief / issue
BASELINE: main @ <SHA>
## AC → dôkaz (T7)
| AC | dôkaz | PASS |
## Fresh reader check
- [ ] hlavičky dotknutých LIVE dokumentov tvrdia aktuálny stav
## UNVERIFIED RISK · dlh
## Decision Requests
```
