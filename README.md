# 1. Cvičení CI-CD

## 1.1. TOC

- [1. Cvičení CI-CD](#1-cvičení-ci-cd)
  - [1.1. TOC](#11-toc)
  - [1.2. 🧩 Úkol: Nastavení větví pro prostředí `test`, `uat` a `prod`](#12--úkol-nastavení-větví-pro-prostředí-test-uat-a-prod)
    - [1.2.1. 🎯 Cíl](#121--cíl)
  - [Nas](#nas)
    - [1.2.2. 💡 Nápověda](#122--nápověda)
    - [1.2.3. ✅ Řešení](#123--řešení)
  - [1.3. 🔄 Pracovní postup s větvemi `test`, `uat`, `main`](#13--pracovní-postup-s-větvemi-test-uat-main)
    - [1.3.1. 🎯 Cíl](#131--cíl)
    - [1.3.2. 💡 Nápověda](#132--nápověda)

## 1.2. 🧩 Úkol: Nastavení větví pro prostředí `test`, `uat` a `prod`

### 1.2.1. 🎯 Cíl

Mít v Git repozitáři připravené tři stabilní větve pro deployment do tří prostředí:

- `test` – pro rychlé ověření změn (z PR z feature větví)
- `uat` – pro akceptační testování (User Acceptance Testing)
- `main` – pro produkční nasazení

Tyto větve se používají v CI/CD pipeline a každé prostředí má svou GitHub Actions workflow, která se spouští při pull requestu do těchto větví.

---

## Nas

### 1.2.2. 💡 Nápověda

1. Vytvoř výchozí větev (`main`), pokud ještě neexistuje:

   ```bash
   git checkout -b main
   git push --set-upstream origin main
   ```

2. Vytvoř nové větve pro `test` a `uat`, založené na `main`:

   ```bash
   git checkout main
   git checkout -b test
   git push --set-upstream origin test

   git checkout main
   git checkout -b uat
   git push --set-upstream origin uat
   ```

3. Ověř v GitHubu, že větve `test`, `uat` a `main` existují v repozitáři.

4. (Doporučeno) Zamkni přímé commity do těchto větví pomocí branch protection rules na GitHubu:
   - Změny jen přes Pull Request
   - Vyžaduj schválení recenzí
   - (Volitelně) Vyžaduj úspěšné testy v CI

---

### 1.2.3. ✅ Řešení

Pokud vše proběhne správně, příkaz `git branch -r` zobrazí:

```
origin/main
origin/test
origin/uat
```

A v GitHub Actions se nasazení automaticky spustí při PR do těchto větví.

Ukázkový tok větví může vypadat takto:

```bash
feature/nova-funkce
        ↓ PR
     test (CI/CD na /test)
        ↓ PR
     uat (CI/CD na /uat)
        ↓ PR
     main (CI/CD na /prod)
```

Každý krok má svůj build a nasazení. Celý proces je automatizovaný a kontrolovaný přes PR schvalování a testování.

---

## 1.3. 🔄 Pracovní postup s větvemi `test`, `uat`, `main`

### 1.3.1. 🎯 Cíl

Mít jasně definovaný proces vývoje a nasazování přes tři prostředí:

1. **Lokální vývoj ve feature větvích**
2. **Pull Request do `test` pro CI ověření**
3. **Schválený PR do `uat` pro UAT**
4. **Finální PR do `main` pro produkci**

---

### 1.3.2. 💡 Nápověda

1. Vytvoř novou vývojovou větev ze `test`:

   ```bash
   git checkout test
   git checkout -b feature/nova-funkce
   ```

2. Po dokončení změn:

   ```bash
   git add .
   git commit -m "feat: přidání nové funkce"
   git push -u origin feature/nova-funkce
   ```

3. Vytvoř PR z `feature/nova-funkce` do `test`.  
   CI pipeline ověří lint, testy a build.  
   Po schválení se PR sloučí do `test` a nasadí se na testovací prostředí.

4. Po úspěšném testování vytvoř PR z `test` do `uat`.  
   Po schválení se nasadí na UAT prostředí.

5. Po schválení UAT vytvoř PR z `uat` do `main` → nasazení do produkce.

---
