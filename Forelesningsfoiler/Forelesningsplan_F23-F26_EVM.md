# Forelesningsplan F23–F26: Endelig volum-metode (EVM)

Henviser til `Kompendium/3_PDE.tex` (kapittel 3): §3.4 «Numeriske metoder:
Endeligvolum-metode», §3.5 «1D diffusjonsproblemer», §3.6 «2D
diffusjonsproblemer» og §3.7 «Ligninger som inkluderer en tidsparameter».
Hver forelesning er 2 × 45 min. F23 kan brukes helt til teori; F24–F26 har
45 min teori og (minst) 45 min koding. Øving 7 handler om EVM.

Røde tråd: *endelig volum = bevaringsloven på integralform, skrevet opp for hver
celle*. Alle fire forelesningene ender i samme form,
$a_P\phi_P = \sum a_{nb}\phi_{nb} + S^u$ med $a_P = \sum a_{nb} - S_P$,
og forskjellene ligger bare i hvilke naboer, kilder og randbidrag som inngår.

---

## F23 — Idéen bak EVM: kontrollvolum, flukser og 1D diffusjon

Kompendium: **§3.4** (hele) og **§3.5, eksempel «Varmeledning i en stav uten kildeledd»**.

**Teori (45–90 min)**
- Motivasjon: fra F19 (varmeligningen fra energibevaring) — bevaringslover på
  integralform. Transportligningen for $\phi$ (i ord) → ren, stasjonær diffusjon.
- Kontrollvolum og divergenssetningen: $\iiint_{KV}\nabla\cdot(\Gamma\nabla\phi)\,dV = \iint_A \hat n\cdot\Gamma\nabla\phi\,dS$
  → «fluks ut minus fluks inn = produksjon».
- 1D: gitter (noder midt i cellene, flater midt mellom noder), notasjon $W,P,E$ og $w,e$.
- Diskretisering: lineær tilnærming av fluksen på flatene → $a_P\phi_P = a_W\phi_W + a_E\phi_E + S^u$.
- Randbetingelse med kjent verdi: halv celle ($\delta x/2$), «kutt koblingen» og bak
  randverdien inn i $S^u$ og $S_P$.
- Eksempel: stav med $T_V=100\,^\circ$C, $T_H=500\,^\circ$C, fem celler → $140, 220, 300, 380, 460$ (eksakt).
- EVM vs. endelige differanser: samme ligninger i de indre nodene, men forskjellig
  utgangspunkt (bevaring), annen randbehandling, og mer fleksibel.

**Kode (valgfri andre time):** `JupyterFiler/F23_EVM_1D_Diffusjon.ipynb`
- Sett opp og løs stav-eksempelet som matrise, steg for steg (koeffisienttabell → $A$, $\mathbf b$).
- En generell funksjon for 1D EVM med kjente randtemperaturer.
- Bevaring: fluksen gjennom hver flate er lik (diskret bevaring), og stemmer med eksakt fluks.
- EVM vs. endelige differanser med samme antall ukjente; gitterforfining.
- Utblikk: stav av to materialer (variabel $\kappa$) — der EVM virkelig skinner.

---

## F24 — Kildeledd og andre randbetingelser i 1D

Kompendium: **§3.5**, eksemplene «Varmeledning med internt kildeledd» og
«Kjøling av en sirkulær ribbe», og bemerkningen «Randbetingelse med kjent fluks».

**Teori (45 min)**
- Kildeleddet integrert over cellen; linearisering $\overline S|KV| = S^u + S_P\phi_P$ (og hvorfor $S_P\le 0$).
- Eksempel: plate med jevn varmeproduksjon $q$ — numerisk vs. analytisk løsning
  (konstant avvik $4\,^\circ$C fra randcellene).
- Eksempel: ribbe med konvektivt varmetap $-n^2(T-T_\infty)$ — temperaturavhengig sluk
  går inn i $S_P$.
- Isolert rand / kjent fluks (Neumann): fluksen på randflaten er kjent → $a_E=0$ og $S^u \mathrel{+}= q_BA$.

**Kode (≥ 45 min):** `JupyterFiler/F24_EVM_1D_Kilder_Randbetingelser.ipynb` (lages)
- Utvid F23-løseren med kildeledd ($S^u$, $S_P$ per celle) og fluksrand.
- Reproduser plate-eksempelet; konvergensstudie (feil mot $\delta x$).
- NB: ribbe-eksempelet er tema for `EVM_oppgave.ipynb` (øving 7), så her bør vi
  stoppe ved oppsettet, eller bruke en annen variant (f.eks. annen $n$ eller fluksrand).

---

## F25 — EVM i 2D

Kompendium: **§3.6** (hele).

**Teori (45 min)**
- 2D-kontrollvolum: fire naboer $W,E,S,N$ og fire flater $w,e,s,n$; flatearealer
  $A_w=A_e=\delta y$, $A_s=A_n=\delta x$ (dybde 1).
- $a_P\phi_P = a_W\phi_W + a_E\phi_E + a_S\phi_S + a_N\phi_N + S^u$.
- Randceller (én vegg) og hjørneceller (to vegger).
- Sammenheng med fempunktsformelen og Jacobi/Gauss-Seidel fra F21: samme
  indre ligninger, annen randbehandling.

**Kode (≥ 45 min):** `JupyterFiler/F25_EVM_2D.ipynb` (lages)
- 2D-løser med Gauss-Seidel over cellene (gjenbruk av F21-ideer) og/eller sparse direkte løsning.
- Plate med kjente veggtemperaturer; varmekart; sjekk av energibalansen over hele platen.
- Variabel $\Gamma$ i 2D (en «isolerende» innsats) — vanskelig med FD, naturlig med EVM.

---

## F26 — Tidsavhengige problemer med EVM, og oppsummering av kapittel 3

Kompendium: **§3.7** (hele).

**Teori (45 min)**
- Integrasjon over kontrollvolum *og* tidsintervall; eksplisitt skjema
  $a_P^0T_P^{n+1} = a_WT_W^n + a_ET_E^n + (a_P^0-a_W-a_E+S_P)T_P^n + S^u$, $a_P^0=\rho cA\delta x/\Delta t$.
- Sammenheng med eksplisitt Euler fra F21: samme skjema, $r=\alpha\Delta t/\delta x^2$.
- Stabilitet: koeffisienten foran $T_P^n$ må være $\ge 0$ → $\Delta t\le\rho c\delta x^2/(2\kappa)$
  i det indre, men $\Delta t\le\rho c\delta x^2/(3\kappa)$ i randcellene. 2D: $\rho ch^2/(4\kappa)$.
- (Til orientering) implisitte skjema er ubetinget stabile, men krever et ligningssystem per tidssteg.
- Oppsummering av kapittel 3: RVP vs. IVP, FD vs. EVM, stabilitet.

**Kode (≥ 45 min):** `JupyterFiler/F26_EVM_Tidsavhengig.ipynb` (lages)
- Tidsavhengig 1D: en stav som avkjøles / varmes opp fra randen; konvergens mot den stasjonære løsningen fra F23.
- Stabilitetsgrensen: kjør like under og like over $\rho c\delta x^2/(3\kappa)$ og se oscillasjonene starte ved randen.
- 2D tidsavhengig som utvidelse (gjenbruk av F25).
