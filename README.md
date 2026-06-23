# Suite Patrimoniale Pro — Pacchetto Completo (versione finale)

**Stato: 94 test superati, 0 fallimenti.** Tutti i 13 file JS validati sintatticamente.
Verificato eseguendo `node test.js` dentro questo stesso pacchetto.

## Come usarlo

- **Usare l'app**: apri `index.html` in un browser (tutti i file nella stessa cartella).
- **Eseguire i test**: `node test.js` (richiede Node.js). Deve stampare 94 PASS / 0 FAIL.

## I file

| File | Ruolo | Copertura test |
|------|-------|----------------|
| `index.html` | Interfaccia — aprire questo per usare l'app | — |
| `style.css` | Stili | — |
| `main.js` | Cuore: asset class, portafogli, pesi, simulazione, decumulo | suite 2,5,7,9 |
| `advanced-montecarlo.js` | Motore MC, serie storiche reali, bootstrap | suite 1,4,7 |
| `backtest.js` | Backtesting storico 1970-2024, sequence risk | suite 3 |
| `pensione.js` | Scheda pensione (coefficienti, IRPEF, tasso sostituz.) | suite 6 |
| `fiscal.js` | Fiscalità (lotti, capital gain, LIFO/FIFO, minus) | **suite 8** |
| `quant-analytics.js` | Frontiera efficiente, Sharpe, VaR/CVaR | **suite 9** |
| `test.js` | Batteria di regressione — 94 test, 9 suite | — |
| `crisis-stress.js` | Scenari di crisi / stress test | sintassi |
| `live-data.js` | Dati di mercato live | sintassi |
| `scenarios-manager.js`, `tier-system.js`, `pro-features.js`, `guide-pdf.js` | Accessori | sintassi |

## Cosa è stato costruito

### 9 asset con serie storiche reali (prima erano tutti proxy del mercato sviluppato)
**7 fattori azionari** (modello mercato + β·fattore, dati Fama-French/AQR in EUR 1979-2024):
Small Value, Momentum, Valore (HML), Qualità (RMW), Investment (CMA), Size (SMB),
Bassa Volatilità (BAB, con β di mercato 0.70 per la difensività). Più il Multi-Fattore
come composizione viva dei 5 fattori.

**2 asset class indipendenti** (serie di rendimento propria, campionata in parallelo):
- REITs (FTSE NAREIT 1979-2024) — crash 2008 più profondo, ripresa 2009 esplosiva
- Mercati Emergenti (Fama-French EM 1989-2024) — crisi asiatica '97, boom commodity '03-07

Il gate dei "non-backtestabili" è vuoto: ogni asset principale ha la sua serie reale.
Sequence risk e decumulo vedono correttamente tutti i nuovi asset.

### Batteria di test: 94 controlli in 9 suite
dati storici · simulatore · backtest · Monte Carlo · decumulo · pensione · fattori
reali · fiscalità · quant analytics. Ogni comportamento storico chiave (crisi, premi
fattoriali, difensività) e ogni proprietà matematica/fiscale è ancorata a un test.
La correttezza dei test è confermata da prove di sabotaggio.

## Limiti noti (documentati nel codice)

1. **Struttura intra-mensile dei dati sviluppati**: i rendimenti ANNUI di row[0] sono
   reali e accurati (validati vs MSCI World reale 2010-24, entro ~1pt/anno), ma il
   co-movimento MENSILE è ricostruito. NON validare correlazioni mensili contro row[0].
   Dettagli nel commento sopra `const d=[` in advanced-montecarlo.js. Upgrade opzionale
   a priorità bassa (MSCI World Net EUR mensile da Curvo.eu/MSCI).

2. **fat_dividendi** resta a proxy: nessun fattore accademico standard diretto disponibile.

3. **File non coperti da test dedicati**: crisis-stress, live-data, scenarios-manager,
   tier/pro. Sono prevalentemente UI/accessori, non logica numerica critica.

## Avvertenza

Strumento di **simulazione**, non consulenza finanziaria. Proietta scenari probabilistici
basati su dati storici; il passato non garantisce il futuro. Le scelte di modellazione
(β di cattura, conversioni valutarie, fallback) sono ragionevoli ma restano assunzioni.
Non usare gli output come unica base di decisioni di investimento reali senza il parere
di un professionista qualificato.
