---
title: "Skub kode fra GitHub til lokal server"
categories:
  - Blog
tags:
  - GitHub
  - GitHub Actions
  - YAML
  - GitHub self-hosted runners
---

For at opsætte så man kan skubbe kode fra GitHub ned til en server skal man: 
1. **Installere en self-hosted runner:** Man kan vælge at installere en self-hosted runner enden på repo-niveau eller på organations-niveau. Man gør det ved at:
	1. Settings > Actions > Runners > New runner > New self-hosted runner
	   ![New self-hosted runner i en organization](/assets/images/2023-07-06/New_self-hosted_runner.png)
	2. Vælg runner image og architecture, derefter er det blot at følge diverse steps der er beskrevet på siden.
	   
	Lige et par fif:
	- På et tidspunkt under installation spørg den dig om du vil sætte et ekstra tag, det kan være en god idé at opsætte en unik tag for den enkelte runner. For hvis du i en GitHub Action opsætter at den skal bruge self-hosted så køre action på en tilfældig af runnere du har self-hosted tagget på.
	- Din self-hosted runners kommer med default tre tags. her er, hvad en af mine runners har af tags:
		- self-hosted
		- Windows
		- X64
	- En fordel med at bruge en self-hosted runner er at de ikke bruger nogen af dine månelige Actions minutes.
	- Du kan læse mere om self-hosted runners [her](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners)
2. **Opsæt en action:** 
	1. I dit repo tryk på Actions > New workflow > Indsæt følgende i filen:
	   ```yml
name: PowerShell Deploy to Prod

on:
  # Hvis man push'er eller laver en pull request til sin branch main vil den kører actionen
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  # Denne betyder at man kan køre den manuelt
  workflow_dispatch:

jobs:
  build-and-deploy:

    # Tag på server, god idé at finde noget mere unikt/tilføje mere end et tag.
    # Hvis man har flere server deployer den bare til en tilfældig der har dette tag
    runs-on: self-hosted

    steps:
    # Checkker ud repoet, nødvendig for at filerne kommer med over på serveren
    - uses: actions/checkout@v2

    # Simpelt PowerShell der flytter alle filer til den ønskede mappe
    - name: Deploy to Prod
      run: |
        Copy-Item -Path .\* -Destination "C:\PowerShell\MARA-Test"
```
	- [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
