# Bollette di Casa

App web per tracciare le bollette domestiche (luce, acqua, gas), pensata per essere usata dal cellulare.

**Live:** https://guxandobrat.github.io/House-Taxes/

## Cosa fa

- Registra le bollette di **luce, acqua e gas** con valore e data fattura
- Ogni mese ha **un unico record** — se aggiungi luce ad aprile e poi acqua ad aprile, si fondono
- **Riconoscimento vocale** — tocca il microfone, parla i valori, i campi si compilano
- **Storico** raggruppato per mese/anno, con valori in grigio se non ancora inseriti
- **Report** con 5 grafici interattivi (tutti con prezzi visibili sulle barre):
  - € / Mese (barre comparative luce/acqua/gas)
  - € / Giorno (costo giornaliero calcolato dal periodo reale tra fatture)
  - Andamento (linee di evoluzione totale + per categoria)
  - Distribuzione (grafico a ciambella con percentuali)
  - Per Persona (costo mensile diviso per numero di abitanti)
- **Navigazione mesi** nel report con frecce ◀ ▶
- **Condividi via WhatsApp** — dopo il salvataggio o da qualsiasi record nello storico
- **Esporta PDF** del report
- **Backup/Ripristino** in formato JSON
- **3 lingue**: Italiano (default), Castellano, Portugues — cambia dall'icona ingranaggio

## Come funziona

Un singolo file HTML (`index.html`) hostato gratis su GitHub Pages. Nessun server, nessun costo.

| Tecnologia | Uso |
|---|---|
| IndexedDB (Dexie.js) | Salvataggio dati nel browser |
| Web Speech API | Riconoscimento vocale |
| Chart.js + datalabels | Grafici con prezzi |
| html2pdf.js | Esportazione PDF |
| Web Share API | Condivisione WhatsApp/email |

## Modello dati

Un record per mese (`&month` = indice unico):

```
{
  month: "2026-04",
  luce: 85.20,  luceDate: "2026-04-14",
  acqua: 42.50, acquaDate: "2026-04-08",
  gas: 63.10,   gasDate: "2026-04-20",
  abitanti: 4,
  createdAt: "2026-04-14T10:30:00Z"
}
```

Quando salvi una bolletta per un mese che esiste gia, i valori nuovi si **fondono** nel record esistente (upsert).

## Come usare

1. Apri il link sul cellulare
2. Vai su **Registra**, inserisci i valori e le date
3. Il bottone **Salva** appare quando inserisci qualcosa
4. Dopo il salvataggio, puoi **condividere via WhatsApp** con i coinquilini
5. In **Storico** vedi tutti i mesi, puoi modificare o eliminare
6. In **Report** naviga tra i grafici e i mesi

## Changelog

### v9 (2026-04-14)
- **Blocco duplicati**: se una bolletta esiste gia per quel mese, mostra errore e non salva
- Messaggio: "Luce — Aprile 2026 gia registrata! Modifica dallo Storico."
- Per cambiare un valore esistente, usare Modifica dallo Storico

### v8 (2026-04-14)
- **1 record per mese**: salvataggi multipli per lo stesso mese si fondono
- **Prezzi sui grafici**: tutte le barre e linee mostrano il valore in €
- **Navigazione mesi**: frecce ◀ ▶ nel report per filtrare per mese
- **Ordinamento corretto**: mesi sempre in ordine cronologico
- **Migrazione automatica** da tutti i database precedenti

### v7 (2026-04-14)
- Migrazione automatica dati da DB vecchi
- Bottone salva nascosto, appare con animazione quando si inserisce un valore

### v6 (2026-04-14)
- Condivisione WhatsApp/email via Web Share API
- Storico raggruppato per mese con valori zero in grigio
- Report con 5 tab di grafici (€/mese, €/giorno, andamento, distribuzione, per persona)
- Costo giornaliero basato sul periodo reale tra fatture

### v5 (2026-04-14)
- Fix errore IndexedDB keypath (spazi negli indici)
- Fix `orderBy('createdAt')` su campo non indicizzato

### v4 (2026-04-14)
- Navigazione automatica allo storico dopo il salvataggio
- try/catch su salvataggio per mostrare errori

### v3 (2026-04-14)
- Bottone salva disattivato fino a modifica
- Modifica e eliminazione nello storico

### v2 (2026-04-14)
- Redesign completo: UI premium, italiano default
- Data fattura separata per ogni bolletta
- Grafici basati sul periodo tra fatture
- Rimossa tela Home, 3 tab (Registra/Storico/Report)
- Ingranaggio per cambio lingua (IT/ES/PT)
- Rimosso campo status (pago/pendente)

### v1 (2026-04-14)
- Prima versione funzionante
- Riconoscimento vocale, IndexedDB, Chart.js, esportazione PDF/backup

## Struttura

```
index.html    ← app completa (HTML + CSS + JS inline)
app.html      ← copia di index.html
README.md     ← questo file
```

## Hosting

GitHub Pages, deploy automatico dal branch `main`. Gratis.

## Limitazioni

- **Riconoscimento vocale**: richiede internet (Chrome Android funziona bene, iOS Safari limitato)
- **Dati nel browser**: se cancelli i dati del browser, perdi lo storico — usa il backup JSON
- **iOS nella UE**: PWA standalone non supportata da Apple — funziona come sito normale
