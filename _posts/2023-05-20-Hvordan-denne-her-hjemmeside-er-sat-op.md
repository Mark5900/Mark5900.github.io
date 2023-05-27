---
title: "Hvordan denne her hjemmeside er sat op?"
categories:
  - Blog
tags:
  - GitHub
  - GitHub Pages
  - DNS
---
## Indholdsfortegnelse


## Resumé 
Her kan du læse om opsætning af denne her blog side og hvordan du selv kan sætte en op.

## Intro
Da jeg sad og undersøgte, hvordan kunne jeg sætte en blog op ledte jeg oprindeligt efter en siden, hvor jeg kunne hoste en hjemmeside med mit domain og kunne bruge en form for template da jeg ikke er så stærk i og kode hjemmesidder 

Der faldt jeg over at man kunne gøre brug af [GitHub Pages](https://pages.github.com) og jo mere jeg undersøgte dette fandt jeg ud af at det var lige det jeg ledte efter. Jeg kunne hoste min side helt gratis, [GitHub Pages](https://pages.github.com) understøtter at man kan bruge sit eget domain, det er muligt at slå et kommentar felt til på sine posts og jeg kan bruge eksterne temaer på min siden

<a id="DiverseTeknologierTermer"></a>
## Beskrivelse af diverse teknologier og termer 
Her beskriver jeg kort diverse teknologier/termer så man forhåbentlig ikke behøver at Google en masse for at forstå dette opslag.

### GitHub Pages
Med [GitHub Pages](https://pages.github.com) kan man oprette en hjemmeside fra bunden som bygges ud fra ens kode der skal ligge i et [GitHub Repository](#GitHub-Repository).

### GitHub Repository
I et [GitHub Repository](https://docs.github.com/en/get-started/quickstart/create-a-repo) (også kaldet et repo) kan man gemmes sit projekt/kode. Med GitHubs mange funktionalitet har man ud over at versionere sin koden også mulighed for at styre issues, diskussioner, wikis sider osv.

### GitHub Discussions
Man kan aktiver [GitHub Discussions](https://docs.github.com/en/discussions) i ens repo, det giver muligheden for at lave et forum til ens projekt.

### GitHub Issues
Med [GitHub Issues](https://github.com/features/issues) kan man holde styr på opgaver i ens projekt og snakke om diverse opgaver. Man vil for det meste koble ens Issues sammen med et [GitHub Project](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects) som er en samling af alle ens opgaver, hvor man kan bruge visninger som Kanban og liste.

### Jekyll
[Jekyll](https://jekyllrb.com) er motoren bag [GitHub Pages](https://pages.github.com) som som gør brug af [Markdown](#Markdown), [Liquid](Liquid), HTML & CSS til ak generer statisk sider

### Minimal Mistakes
[Minimal Mistakes](https://mademistakes.com/work/jekyll-themes/minimal-mistakes/) er en fleksibel to kolonner [Jekyll](#Jekyll) tema. 

### Obsidian
[Obsidian](https://obsidian.md) er en vidensdatabase som mange bruger til at tage noter i, noterne skrives i [Markdown](#Markdown) format og man kan installere diverse udvidelse til at gøre det lettere at arbejde med [Markdown](#Markdown) filer.

### Markdown
[Markdown](https://en.wikipedia.org/wiki/Markdown) er et let opmærkningsprog til at skabe formateret tekst ved hjælp af almindelig tekst-editor f.eks. s

### Liquid
[Liquid](https://github.com/Shopify/liquid) er et open-source template language udviklet af [Shopify](https://www.shopify.com/)


## Opsætning af ens side 

### 1. DNS

Hvis man ønsker at bruge sit eget domæne til ens side skal man lave lidt opsætning i sit domæne DNS, det en god idé at sætte det op som noget af det første da det kan godt tage noget tid for at synkronisere rundt.
#### Opsætning af underdomæne
For at opsætte et `www` eller brugerdefineret domæne såsom `www.example.com` eller `blog.example.com`.
Skal du opsætte en CNAME fortegnelse som peger til `<user>.github.io`, hvor `<user>` er dit GitHub brugernavn.

#### Opsætning af hoveddomæne
Opsæt følgende adresser som A fortegnelser:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
Opsæt følgende Adresser som AAAA fortagnelser:
```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

En lille ekstra ting jeg gjorde var at opsætte en CNAME fortegnelse, hvor jeg peger `www` til `<user>.github.io`, hvor `<user>` er dit GitHub brugernavn. Så jeg nemlige sikker på om folk så skriver [boennelykke.com](https://boennelykke.com) eller [www.boennelykke.com](https://www.boennelykke.com) så ender de op på min side.

#### Læs mere 
* [Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

### 2. Opsætning repo
Jeg går ud fra man allerede ved lidt om GitHub og ved, hvordan man opretter et repo og en ny branch. Eller kan man læse om det her: 
* [Create a repo](https://docs.github.com/en/github-ae@latest/get-started/quickstart/create-a-repo)
* [Creating and deleting branches within your repository](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository)

Hvis man ønsker at ens side skal lige min kan man bruge denne [Template](https://github.com/mmistakes/mm-github-pages-starter/generate) og følge beskrivelsen herunder:
1. Opret nyt repo og kald det `<user>.github.io`, hvor `<user>` er dit GitHub brugernavn og sørg for at lave det public.
	1. `<user>.github.io` er et reseveret navn til [GitHub Pages](https://pages.github.com) og det er meget vigtigt at du skriver dit brugernavn rigtigt ellers vil det ikke virke.
2. I dit repo > Settings > Pages under Code and automation her styre man diverse indstillinger f.eks. hvilken branch der skal bruges til at bygge din side.
	1. Hvis man vil bruge et sit eget domain skal man sætte dette under Custom domain, vent herefter på DNS tjekket går godt så du kan sætte flueben i Enforce HTTPS. Hvis tjekket ikke går godt så vend tilbage om en time og prøv igen, hvis du lige har lavet din DNS opsætning.
3. Opret en ny brach
	1. Denne nye branch skal du bruge til lave dine ændringer i og lave pull request fra til din main branch. Fordi hvis man arbejder i ens main brach så hver gang med pusher til main skal man vente lidt og pull de nye commits ned som GitHub Pages automatisk opretter.

Hvis det ikke helt giver mening det jeg har skrevet eller man gerne vil prøve det lidt af først kan man lave følgende [skill kursus](https://github.com/skills/github-pages) fra GitHub igennem.

### 3. Opsætning af side
Indstillinger for hele din side styres i `_config.yml` og man læse om alle mulighederne [her](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)

##### `__data/navigation.yml`
Styre menu oppe til højre på din side
![[Menu.png]]
Filen skulle gerne indeholde følgende:
```yml
main:
  - title: "Posts"
    url: /posts/
  - title: "Categories"
    url: /categories/
  - title: "Tags"
    url: /tags/
  - title: "About me"
    url: /about/
```
Titel er visningsnavnet og url'en er permalink der hendviser til en side du kan se under mappen `_pages`.
For at læse mere om navigation tryk [her](https://mmistakes.github.io/minimal-mistakes/docs/navigation/).
#### `_pages/`
I denne mappe finder man siderne der er nævnt under [`__data/navigation.yml`](#__datanavigationyml), det kan være en god idé at opdatere `about.md`.

Man kan læse mere om pages [her](https://mmistakes.github.io/minimal-mistakes/docs/pages/).

#### `_posts`
I denne mappe skal man ligge alle sine posts til ens side, man kan læse mere om det [her](https://mmistakes.github.io/minimal-mistakes/docs/posts/). 

Helt simpelt, hvis man vil lave et nyt post opretter man en fil med følgende navneformat `YEAR-MONTH-DAY-title.md` og i toppen af filen indsætter man følgende:
```markdown
---
title: ""
categories:
  - Blog
tags:
  - XXXX
---
```


#### `assets/images/`


### 4. Opsætning af kommentare
### 5. Opsætning af Obsidian