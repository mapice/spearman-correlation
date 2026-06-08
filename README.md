# Spearman

Tool statico per calcolare la correlazione di Spearman tra colonne di un file Excel.

L'app gira interamente nel browser: il file Excel viene letto localmente, non viene caricato su server e non richiede backend.

## Avvio

Apri `index.html` con un browser moderno.

Non serve installare dipendenze, compilare codice o avviare un server. Le librerie JavaScript necessarie sono incluse in `vendor/`.

## Formato dei dati

Default:

- riga `1`: nomi delle colonne;
- dalla riga `2` in poi: dati numerici.

I campi `Nomi` e `Dati` permettono di cambiare queste righe quando il file ha intestazioni spostate, righe vuote iniziali o note sopra la tabella.

Il tool prova anche ad auto-rilevare la riga delle intestazioni e la prima riga dati quando il foglio non segue esattamente il default.

## Selezione colonne

Nel gruppo `A` e nel gruppo `B` puoi inserire:

- una singola colonna Excel: `L`;
- colonne non contigue: `M,S,T`;
- intervalli: `L:N`;
- più colonne separate da virgola o punto e virgola: `L,N,Q`;
- espressioni semplici con `e`: `S e M`;
- nomi delle colonne presi dalla riga intestazioni, ad esempio `Peso medio`.

Se una colonna viene spostata nel file, usare il nome dell'intestazione è più robusto della lettera Excel.

## Calcolo

Per ogni coppia `A x B`, il tool:

1. legge le due colonne selezionate;
2. converte i valori numerici riconoscibili;
3. scarta solo le righe in cui almeno uno dei due valori della coppia è mancante o non numerico;
4. calcola i rank con media dei rank in caso di pari;
5. calcola `rho` come correlazione di Pearson tra i rank;
6. calcola un `p-value` bilaterale approssimato con distribuzione t, con `df = n - 2`.

Il minimo campione di default è `n = 3`, modificabile nel campo `Min. n`.

## Output

La vista `Tabella` mostra:

- colonna e nome della variabile A;
- colonna e nome della variabile B;
- `Spearman rho`;
- `p-value`;
- `n`, cioè le osservazioni valide usate nella coppia;
- righe escluse;
- stato del calcolo.

La vista `Matrice` mostra gli stessi coefficienti `rho` in formato compatto, con colore proporzionale all'intensità della correlazione.

I risultati possono essere esportati come:

- `CSV`;
- `XLSX`;
- testo tabellare copiato negli appunti.

## Gestione casi limite

Il tool è pensato per non interrompersi quando il file cambia leggermente:

- fogli vuoti o non leggibili mostrano uno stato d'errore invece di bloccare l'app;
- righe vuote sopra o dentro il foglio vengono tollerate;
- celle vuote, testo non numerico ed errori Excel come `#N/A` o `#DIV/0!` vengono ignorati nella singola coppia;
- colonne fuori dal foglio vengono segnalate;
- variabili costanti vengono marcate come `Variabile costante`;
- nomi colonna duplicati usano la prima occorrenza;
- numeri con virgola decimale, separatori delle migliaia, percentuali e valori negativi tra parentesi vengono convertiti quando possibile.

Esempi di valori numerici riconosciuti:

- `1,25`;
- `1.25`;
- `1.234,50`;
- `1,234.50`;
- `12,5%`;
- `(€ 1.234,50)`;
- `1,2e3`.

## Privacy

Il file Excel resta nel browser dell'utente. L'app non invia dati a servizi esterni.

Nota: se il file `index.html` viene aperto direttamente da disco, le librerie vengono lette dalla cartella locale `vendor/`. Se viene servito via HTTP, vengono comunque caricate dalla stessa cartella del progetto.

## Dipendenze incluse

- `xlsx.full.min.js`: SheetJS `xlsx` 0.18.5, per leggere e scrivere file Excel.
- `lucide.min.js`: Lucide 1.17.0, per le icone dell'interfaccia.

Le dipendenze sono vendorizzate per permettere l'uso offline del tool.

## Struttura

```text
.
├── index.html
├── README.md
└── vendor/
    ├── lucide.min.js
    └── xlsx.full.min.js
```

## Limiti

Il `p-value` usa l'approssimazione t comunemente impiegata per Spearman. Per campioni molto piccoli o analisi inferenziali formali, verificare i risultati con un ambiente statistico dedicato.

Il tool non modifica il file Excel originale.
