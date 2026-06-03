# Kriminalita v České republice (2021–2025) — Power BI report

Vizualizace registrované a objasněné trestné činnosti v ČR za roky 2021–2025 na základě otevřených dat Policie ČR.

## Zdroj dat

Statistické přehledy kriminality, Policie ČR — prosincové snapshoty za roky 2021–2025 (každý prosincový soubor obsahuje kumulativní data za celý daný rok). Použit byl list „Česká republika" z každého souboru.

## Příprava dat

Pět excelových souborů bylo sjednoceno do konzistentní podoby (sloupce podle pozice, protože hlavičky se mezi roky mírně liší). Použité ukazatele: počet registrovaných a objasněných skutků, objasněnost a skupiny pachatelů (nezletilí, mladiství, recidivisté, cizinci, pod vlivem, pod vlivem alkoholu). Souhrnné a prázdné řádky byly odfiltrovány. Sloupec „dodatečně objasněné" nebyl použit — report pracuje pouze se skutky registrovanými a objasněnými ve stejném období.

## Datový model

Tři propojené tabulky:

- **Kriminalita** (faktová) — jeden řádek = jeden trestný čin v daném roce
- **Dim_Kategorie** — zařazení TSK kódu do kategorie a podkategorie
- **Dim_Roky** — roky 2021–2025

Vazby: `Kriminalita[TSK_Kod]` → `Dim_Kategorie[TSK_Kod]` (N:1) a `Kriminalita[Rok]` → `Dim_Roky[Rok]` (N:1).

## Vytvořené výpočty

- **Kalkulovaný sloupec** `Neobjasneno` = Registrovano − Objasneno
- **Measure** `Objasnenost %` = DIVIDE(SUM(Objasneno); SUM(Registrovano)) — vážená objasněnost, která se správně přepočítává při filtrování
- **Measure** `pod_vlivem_bez_alkoholu` = SUM(Pod_Vlivem) − SUM(Pod_Vlivem_Alkoholu) — činy pod vlivem jiných látek než alkoholu (sloupec „pod vlivem" obsahuje i alkohol, který je tak oddělen)

## Struktura reportu

**1. Přehled** — KPI karty (registrováno, objasněno, objasněnost %), čárový graf vývoje v čase s objasněností na sekundární ose, slicer na rok.

**2. Struktura kriminality** — treemap rozložení podle kategorií (s drill-down na jednotlivé činy), sloupcový graf objasněnosti podle typu činu (činy nad 100 případů), slicery na rok a kategorii.

**3. Struktura pachatelů** — čtyři sloupcové grafy (nezletilí a mladiství, recidivisté a cizinci, pod vlivem látek vs. alkoholu, srovnání skupin), společná barevná legenda, slicer na rok.

## Hlavní zjištění z reportu

- **Objasněnost se výrazně liší podle typu kriminality.** Nejúspěšněji se objasňují násilná a mravnostní kriminalita (přes 60 %) a kategorie „ostatní kriminální" a „zbývající" (71–76 %). Naopak majetková kriminalita má objasněnost nízkou — majetková „ostatní" jen kolem 15 %, krádeže kolem 34 %. To dává smysl: u násilných činů je obvykle znám pachatel, u krádeží často ne.
- **Majetková kriminalita dominuje objemem.** Krádeže a majetkové činy tvoří největší část všech registrovaných skutků, výrazně více než kriminalita násilná. Z hlediska počtu jde tedy hlavně o majetek, nikoli o násilí.
- **Recidiva je masivní.** Recidivisté stojí za desetitisíci skutků ročně — mnohonásobně více než nezletilí a mladiství dohromady. Opakovaná trestná činnost je tak jeden z největších faktorů celkové kriminality.
- **Mladiství páchají více než nezletilí.** Ve věkových skupinách převažují mladiství (15–17 let) nad nezletilými (1–14 let), přičemž u mladistvých je patrný mírný nárůst v čase.
- **Alkohol je hlavní látkou u činů „pod vlivem".** Většina skutků spáchaných pod vlivem připadá na alkohol, nikoli na jiné návykové látky.
- **Celková objasněnost se drží kolem 45 %**, tedy přibližně každý druhý registrovaný skutek je objasněn ve stejném období.

## Poznámka k datům

Počty skutků podle věku pachatelů nejsou plně porovnatelné mezi obdobími (změna metodiky počítání věku od roku 2016). Zvýšení hranice škody v roce 2020 ovlivnilo počty registrovaných skutků u majetkové a hospodářské kriminality. Rok 2025 může vykazovat nižší objasněnost, protože část skutků ještě nebyla v době pořízení dat objasněna.

## Zdroje

- Policie ČR — Statistické přehledy kriminality za rok 2025 (a roky 2021–2024): https://policie.gov.cz/clanek/statisticke-prehledy-kriminality-za-rok-2025.aspx
- Použité soubory: prosincové sestavy `RRRR_12_Prosinec_sest_01a.xlsx` za roky 2021–2025, list „Česká republika".
- Metodické poznámky k datům (definice registrovaných, objasněných a dodatečně objasněných skutků, změny TSK od 2021, změna metodiky věku od 2016) pocházejí z úvodních poznámek uvedených u statistik na webu Policie ČR.