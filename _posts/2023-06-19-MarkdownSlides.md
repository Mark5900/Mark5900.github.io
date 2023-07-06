---
title: "Lav præsentationer med Markdown filer"
categories:
  - Blog
tags:
  - GitHub
  - GitHub Pages
  - GitHub Repository
  - Obsidian
  - Markdown
  - reveal.js
---

## Intro
Jeg gik helt amok sidste år med at konference videoer fra [PSConf.eu 2022](https://www.youtube.com/watch?v=B4CiA9-ptIs&list=PLDCEho7foSopV1zizsSn8s4PR5g6IvPlE) & [PowerShell + DevOps Global Summit 2022](https://www.youtube.com/watch?v=MHy6iS2a6E0&list=PLfeA8kIs7Cocj9vy9XxXvjCkLhsoCELgr), vil skyde på at jeg i hvert fald har set videoer i 70 timer. Det var utroligt lærerigt og kan varmt anbefales at følge med i de to konferencer.

Der var en og jeg kan ikke huske, hvem men som havde hele sin præsentation i GitHub Pages, så efter at jeg havde fundet ud af at opsætte min blog, så tænkte jeg måtte være det næste jeg fandt ud af, hvordan fungere for det kunne da være meget smart til en præsentation, man skal dele med en masse.

Hvis man ikke allerede har læst min opslag [Hvordan denne her hjemmeside er sat op?](https://boennelykke.com/blog/Hvordan-denne-her-hjemmeside-er-sat-op/) Vil jeg anbefale man gør det, da mange af teknologierne er de samme. Hvis man ikke har lavet en blog som mig tror jeg det kræver at man følger [DNS](https://boennelykke.com/blog/Hvordan-denne-her-hjemmeside-er-sat-op/#1-dns) & - [Opsætning repo](https://boennelykke.com/blog/Hvordan-denne-her-hjemmeside-er-sat-op/#2-ops%C3%A6tning-repo) delen af opslaget.

## Hvad fandt jeg frem til?
Jeg fandt frem til adskillige sider der fortalte om, hvordan man opsætter en side med [reveal.js](https://revealjs.com) f.eks. denne her [side](https://dbafromthecold.com/2021/02/21/creating-presentations-with-reveal-and-github-pages/). Dog som jeg prøvede mig frem var det ikke helt mig, man kan mange smarte ting men man skal ligge alle sine præsentationer i en fil.

Så jeg ledte videre og fandt frem til [a-nau's Markdown slides løsning](https://github.com/a-nau/markdownslides)

## Mit Setup
For at kunne bruge [Alex Naumanns (a-nau)](https://github.com/a-nau) løsning gjorde jeg følgende:
1. **Fork hans projekt:** Jeg valgt at giver repo'et navnet `MarkdownSlides` i stedet for det originale navn, da det var mere sigende for mig. Det man dog skal være opmærksom på er at navnet skal skrives på helt samme måde når man skal tilgå præsentationerne. F.eks. fordi jeg har en blog så skal jeg skrive boennelykke.com/MarkdownSlides for tilgå præsentationerne og den er case sensitive så man skal skrive 100% det samme som ens repo hedder.
   
   **VIGTIGT!** Sæt repo'et til at være public så du ikke bruger nogen af dine action minutter samt så er dine præsentationer tilgængelige for andre.
2. **Giv main branch et nyt navn:** Den default branch kaldet jeg `gh-pages`, det skal man fordi ellers vil den action der genrer præsentationer ikke køre.
   ![View all branches](/assets/images/2023-06-30/View_All_Branches.png)
3. **Opret en ekstra branch:** Det kan være en god idé at oprette en ekstra branch som man kan bruge til at lave sine nye præsentation i før man publicere den.
4. **Opdater reveal.js:** Hvis man ikke opdater reveal.js mappe får man en masse alarmer om alle mulige sikkerhedshuller når GitHub bygger din side. For at opdatere skal du overskrive mappen {Dit_Projekt}/assets/reveal.js med alle filerne fra den nyest version af reveal.js som du finder [her](https://github.com/hakimel/reveal.js/tree/56772afa32d481685157bc355422cb793f5a07ce).
5. **Tilrette siden:** Der er en [getting_started](https://a-nau.github.io/markdownslides/getting_started#/00_Overview) præsentation der meget godt forklare, hvordan du skal tilrette siden samt har eksempler på alt det der er muligt, den vil jeg anbefale man kigger på.
   
   Der står på en af slidesne at du kan installere [Jekyll](https://jekyllrb.com/docs/installation/) på din pc, så du kan køre hjemmesiden lokalt på din maskine. Og i README.md under [Locally](https://github.com/Mark5900/MarkdownSlides/tree/gh-pages#locally) står der, hvordan du gør med docker. Dette kunne jeg dog ikke får til at fungere på min maskine og har ikke fundet en løsning på de, jeg plejer blot at lave en ny præsentation > merge pull request til `gh-pages` > tjek slidesne ud i min webbrowser.

