# Bollette di Casa

App web per tracciare le bollette domestiche (luce, acqua, gas), pensata per essere usata dal cellulare.

**Live:** https://guxandobrat.github.io/House-Taxes/

## Cosa fa

- Registra le bollette di **luce, acqua e gas** con valore e data fattura
- Ogni mese ha **un unico record** — se aggiungi luce ad aprile e poi acqua ad aprile, si fondono
- **Blocco duplicati** — non permette di sovrascrivere una bolletta gia registrata per quel mese
- **Riconoscimento vocale** — tocca il microfono, parla i valori, i campi si compilano
- **Storico** raggruppato per mese/anno, con valori in grigio se non ancora inseriti
- **Report** con 4 grafici interattivi (prezzi visibili su barre e linee):
  - **€ / Mese** — barre comparative luce/acqua/gas con valori dentro le barre
  - **Andamento** — linee di evoluzione con filtri Luce/Acqua/Gas
  - **Distribuzione** — grafico a ciambella con percentuali e valori
  - **Per Persona** — barre empilhadas colorate per tipo di bolletta
- **Navigazione mesi** nel report con frecce ◀ ▶ (filtra grafici e sommario)
- **Sommario** con totale del mese + costo per persona
- **Condividi via WhatsApp** — dopo il salvataggio o da qualsiasi record nello storico
- **Esporta PDF** del report
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

Quando salvi una bolletta per un mese che esiste gia, i valori nuovi si **fondono** nel record esistente (upsert). Se la bolletta specifica (es. Luce) gia esiste per quel mese, viene bloccata con messaggio di errore.

## Come usare

1. Apri il link sul cellulare
2. Vai su **Registra**, inserisci i valori e le date
3. Il bottone **Salva** appare quando inserisci qualcosa
4. Dopo il salvataggio, puoi **condividere via WhatsApp** con i coinquilini
5. In **Storico** vedi tutti i mesi, puoi modificare, eliminare o condividere
6. In **Report** naviga tra i grafici con i tab e tra i mesi con ◀ ▶

## Changelog

### v12 (2026-04-14)
- Rimossi bottoni Esporta/Importa Backup
- Rimosso grafico €/Giorno (redundante con Andamento)
- Rimosso filtro "Tutti" da Andamento (solo Luce/Acqua/Gas)
- Sommario mostra **Totale** + **Per Persona** insieme
- Andamento con filtro ⚡💧🔥 per vedere tutte le linee
- Per Persona con barre empilhadas colorate per bolletta
- Prezzi dentro le barre (bianco) per evitare sovrapposizione

### v9 (2026-04-14)
- Blocco duplicati: errore se bolletta gia esiste per quel mese

### v8 (2026-04-14)
- 1 record per mese con upsert
- Prezzi sui grafici, navigazione mesi, ordinamento corretto
- Migrazione automatica da database precedenti

### v6 (2026-04-14)
- Condivisione WhatsApp via Web Share API
- Storico per mese, valori zero in grigio
- Report con tab di grafici

### v2 (2026-04-14)
- Redesign UI premium, italiano default
- Data fattura separata per bolletta
- 3 tab (Registra/Storico/Report), ingranaggio per lingua

### v1 (2026-04-14)
- Prima versione: riconoscimento vocale, IndexedDB, Chart.js, PDF

## Struttura

```
index.html    <- app completa (HTML + CSS + JS inline)
app.html      <- copia di index.html
README.md     <- questo file
```

## Hosting

GitHub Pages, deploy automatico dal branch `main`. Gratis.

## Limitazioni

- **Riconoscimento vocale**: richiede internet (Chrome Android funziona bene, iOS Safari limitato)
- **Dati nel browser**: se cancelli i dati del browser, perdi lo storico
- **iOS nella UE**: PWA standalone non supportata da Apple — funziona come sito normale
