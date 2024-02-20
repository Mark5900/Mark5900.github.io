---
title: Semantic Release (auto versionering af kode)
categories:
  - Blog
tags:
  - GitHub
  - GitHub Actions
  - GitHub Releases
  - Semantic Release
---
## Intro
Jeg har i GitHub et repo med 27 PowerShell moduler til hele CapaSystems løsning (<A href='https://github.com/Mark5900/Capa.PowerShell.Module/tree/main'>Capa.PowerShell.Module</A>).

Hver gang man lave en ny version skulle jeg:
1. Opdater version nummer i alle 27 manifest (.psd1) filer og i script der bygger den nye version (<A href='https://github.com/Mark5900/Capa.PowerShell.Module/blob/main/CreateInstaller.ps1'>CreateInstaller.ps1</A>).
2. Kører CreateIsntaller.ps1
3. Lav en Pull Request til main.
An på, hvad man har ændret i modulet ville der være en masse støj med i PR'en, da CreateInstaller.ps1 også laver automatisk dokumentation ud fra Get-Help tekst på alle funktionerne i modulerne.

Så jeg ønskede at finde en måde til at automatiserer så meget som muligt.
## Hvad er Semantic Release?
Semantic Release automatiserer pakkeudgivelsesprocessen, herunder bestemmelse af versionsnumre, generering af en udgivelsesnote og offentliggørelse af pakken. Ikke begrænset til disse funktioner kan du udvide Semantic Release ved hjælp af plugins.
### Versionering
Ofte kendt som noget ala. 1.23.122 altså:
- 1 = Major version
- 23 = Minor version
- 122 = Patch version

Semantic release har et mere praktisk vinkel på dette, da det ud fra dine prefix i commit beskeder finder ud af, hvilket version nummer den skal bumpe op:
![](/assets/images/2024-02-17/Tabel.png)
### Releases
Ud fra dine commit beskeder laver den så en release i dit repo med beskrivelse. Så det er klart en øvelse at skrive gode commit beskrivelser.
![Eksempel på en release](/assets/images/2024-02-17/GitHub_Release.png)
### Issues
Vidste faktisk ikke den gjorde dette, før jeg kørte den første kørsel af min GitHub Action i mit repo.
Men den løber alle issues igennem og skriver en besked om, hvilken version man har løst issueet i.
![Eksempel på en besked på et issue](/assets/images/2024-02-17/GitHub_Issues.png)
## Hvordan opsætter man Semantic Release i GitHub?
I roden af dit project opretter du en fil, den understøtter flere forskellige formater, men jeg har valgt at gøre de i json. Filen skal hedde `package.json` og min ser sådan her ud:

```json
{
	"release": {
		"branches": [
			"main"
		],
		"plugins": [
			"@semantic-release/commit-analyzer",
			{
				"preset": "angular",
				"releaseRules": [
					{
						"type": "fix",
						"release": "patch"
					},
					{
						"type": "feat",
						"release": "minor"
					},
					{
						"type": "breaking",
						"release": "major"
					}
				]
			},
			[
				"@semantic-release/exec",
				{
					"prepareCmd": "echo ${nextRelease.version} > version.txt && pwsh -ExecutionPolicy Bypass -File CreateInstaller.ps1"
				}
			],
			"@semantic-release/release-notes-generator",
			[
				"@semantic-release/git",
				{
					"assets": [
						"Modules",
						"Installers",
						"Documentation",
						"version.txt"
					],
					"message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
				}
			],
			[
				"@semantic-release/github",
				{
					"assets": [
						{
							"path": "Installers/*.msi"
						}
					]
				}
			]
		]
	}
}
```

Branches er, hvilket branches semantic release vil kører for. Der, hvor det mere interessante er diverse plugins som du kan finde her: <A href='https://semantic-release.gitbook.io/semantic-release/extending/plugins-list'>Plugins - semantic-release</A>

Mit flow er følgenden når semantic release kører:
1. **commit-analyzer**: Tjekker om der er nogen ændringer som gør skal lave et nyt release.
	- Jeg har så ændret lidt på `releaseRules` da jeg ikke var tilfreds med de standard prefixs man skal bruge på commits.
2. **exec**: Skriver det nye version nummer til `version.txt` og kører mit PowerShel script CreateInstaller.ps1.
	- CreateInstaller.ps1: Laver en masse forskelligt f.eks. importere funktioner ind i PowerShell modul filer (.psm1), opdaterer versions nummer i modul manifest filer (.psd1) osv.
3. **release-notes-generator**: Gennerer release beskrivelse ud fra commits siden sidste release.
4. **git**: Pusher mine ændringer fra `CreateInstaller.ps1` op i mit repo.
	- assets: Er, hvilket filer der skal medtages i commit. Stadard er det kun `['CHANGELOG.md', 'package.json', 'package-lock.json', 'npm-shrinkwrap.json']`
1. **github**: Laver en GitHub release.
	- assets: Sørger for at MSI'erne også er vedhæftet på en release.
### GitHub Action
Min GitHub Action ser således ud:

```yml
name: Semantic Release and publish to PowerShell Gallery
run-name: ${{ github.actor }} is automatically releasing 🚀

on:
  push:
    branches:
      - main

jobs:
  release:
    permissions:
      contents: write
      pull-requests: write
      issues: write
    runs-on: windows-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Install Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "lts/*"

      - name: Install dependencies
        run: npm install

      - name: Install PSMSI module
        run: pwsh -Command "Install-Module -Name PSMSI -Force -Scope CurrentUser"

      - name: Install @semantic-release/git
        run: npm install --save-dev @semantic-release/git

      - name: Install @semantic-release/exec
        run: npm install --save-dev @semantic-release/exec

      - name: Run Semantic Release
        env:
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
        run: npx semantic-release
```

Jeg forventer at ved, hvad en GitHub Action er og kan læse smat forstå denne action. Ellers vil jeg anbefale du læser GitHub's dokumentation (<A href='https://docs.github.com/en/actions'>GitHub Actions documentation - GitHub Docs</A>).

På grund af PowerShell modulet bliver min action nød til at køre på Windows, ellers hvade jeg kørt det på linux.
## Læs mere her
- <A href='https://medium.com/agoda-engineering/automating-versioning-and-releases-using-semantic-release-d16c5672fbe1'>Automating Versioning and Releases Using Semantic Release | by Agoda Engineering | Agoda Engineering & Design | Medium</A>
- <A href='https://semantic-release.gitbook.io/semantic-release/'>README - semantic-release</A>
