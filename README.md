# 📊 Generatore Report Gerarchici (Web-Based)

![Version](https://img.shields.io/badge/version-3.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Technology](https://img.shields.io/badge/tech-HTML%20%7C%20JS%20%7C%20CSS-orange.svg)
![No Server](https://img.shields.io/badge/server-none%20(client--side)-brightgreen.svg)

Un'applicazione web **client-side** (single-page application) progettata per trasformare file CSV grezzi in report gerarchici professionali, in stile Pivot, pronti per essere stampati o condivisi.

L'applicazione funziona interamente nel browser **senza inviare dati a server esterni**, garantendo la massima privacy e velocità.

---

## ✨ Caratteristiche Principali

### 📄📄 Unione di Due File CSV *(Nuovo in v3.0)*
- **Modalità doppio file:** all'avvio scegli se lavorare con un singolo CSV o con due file da unire prima di costruire il report.
- **Rilevamento automatico colonne comuni:** l'app analizza le intestazioni dei due file e suggerisce automaticamente le colonne candidati per il join, anche quando i nomi sono leggermente diversi (es. `ID_Cliente` vs `id cliente`).
- **Chiave composita:** puoi selezionare più colonne di join contemporaneamente (es. CDC + Impegno). Il join avviene sulla combinazione di tutte le coppie; i campi restano separati e selezionabili indipendentemente nel report.
- **Badge di origine F1 / F2 / 🔗:** dopo l'unione, ogni campo nella lista disponibili mostra un badge colorato che ne indica la provenienza (File 1, File 2 o colonna usata come chiave di join).
- **Gestione conflitti di nome:** se entrambi i file hanno una colonna con lo stesso nome (diversa da quelle di join), viene rinominata automaticamente aggiungendo il nome del file tra parentesi quadre.
- **Normalizzazione separata per file:** il pannello 🧹 Normalizza Testo è disponibile indipendentemente per File 1 e File 2 prima di configurare l'unione.

### 🔗 Configurazione JOIN *(Nuovo in v3.0)*
- **Cardinalità in tempo reale:** mentre selezioni le colonne di join, l'app calcola e mostra in tempo reale il tipo di relazione tra i due file:

  | Badge | Significato | Comportamento |
  |---|---|---|
  | ✅ **1:1** | Ogni riga di F1 corrisponde a una sola riga di F2 | Risultato pulito, righe invariate |
  | 🟡 **1:N** | Ogni riga di F1 corrisponde a più righe di F2 | Righe di F1 ripetute per ogni dettaglio di F2. **Normale e spesso voluto** (es. cliente → ordini, impegno → liquidazioni) |
  | 🔴 **N:M** | Entrambi i file hanno valori ripetuti per le colonne scelte | Prodotto cartesiano: i valori numerici risultano **gonfiati e duplicati**. Richiede conferma esplicita |

- **Statistiche join live:** corrispondenze trovate, righe di F1 senza match, righe totali risultanti.
- **Tipo di join selezionabile:**
  - **LEFT JOIN** *(predefinito)* — mantiene tutte le righe di F1, anche senza corrispondenza in F2 (campi F2 vuoti).
  - **INNER JOIN** — mantiene solo le righe con corrispondenza in entrambi i file.
- **Avviso N:M con conferma:** in caso di cardinalità N:M non viene bloccato il flusso ma viene mostrato un riquadro di avviso con il numero stimato di righe risultanti e una checkbox di conferma esplicita. Utile quando il N:M è intenzionale (es. report puramente testuale su combinazioni).

### 🔄 Importazione e Pulizia Dati
- **Supporto CSV Universale:** Carica qualsiasi file CSV (separatore virgola o punto e virgola) tramite drag & drop o selezione da file system.
- **Configurazione Intelligente:** Seleziona visivamente la riga (o le righe) di intestazione ed escludi righe di coda inutili (totali precalcolati, note, righe vuote).
- **Merge Campi:** Unisci due o più colonne (es. "Nome" + "Cognome") direttamente nell'app. La "ricetta" di unione viene salvata insieme al modello per utilizzi futuri.

### 🧹 Normalizzazione Testo
- **Pannello di pulizia per colonna:** Prima di costruire il report, normalizza i valori testuali del CSV direttamente in memoria senza modificare il file originale.
- **Rilevamento automatico del tipo:** ogni colonna viene classificata come *Testo*, *Numerico* o *Misto* e le opzioni disponibili si adattano di conseguenza.
- **Anteprima in tempo reale:** vedi il valore originale → trasformato mentre spunti le opzioni, prima di applicare qualsiasi modifica.
- **Operazioni disponibili per colonna:**
  - ✂️ **Trim** — rimuove spazi e tabulazioni agli estremi della stringa
  - ⎵ **Spazi doppi** — riduce sequenze di spazi interni a uno solo
  - 🔠 **Maiuscolo / 🔡 Minuscolo / 🔤 Prima Lettera** — normalizzazione capitalizzazione (si escludono a vicenda)
  - ✔ **Sì/No** — standardizza varianti booleane (`si`, `yes`, `1`, `true`, `x` → `Sì`; `no`, `0`, `false` → `No`)
  - 🗑 **Rimuovi punteggiatura** — elimina `.` `,` `;` `:` `!` `?` `"` `'` `( )` `[ ]` `{ }` `- _ / \`
- **Applica a tutte:** barra rapida per applicare un'operazione a tutte le colonne in un click.

> ⚠️ **Consiglio per il doppio file:** se la colonna di join ha valori con formati diversi tra i due file (es. spazi nascosti, maiuscole/minuscole diverse), usa Trim e la normalizzazione opportuna su entrambi i file *prima* di configurare l'unione, altrimenti le corrispondenze non vengono trovate.

### 🏗️ Costruttore Drag & Drop
- **Interfaccia Intuitiva:** Trascina i campi nelle aree **Righe** (Gerarchia), **Colonne** (Pivot) e **Valori**.
- **Layout a due righe per i campi configurati:** il nome del campo e il bottone di rimozione occupano la riga superiore; i controlli (ordinamento, flag) la riga inferiore — nessun controllo viene tagliato fuori dal riquadro.
- **Feedback visivo durante il drag:** placeholder animato che mostra dove atterrerà l'elemento.
- **Logica Gerarchica:** Crea livelli di raggruppamento nidificati (es. Regione > Provincia > Agente).
- **Personalizzazione Totale per ogni campo:**
  - Ordinamento **A-Z / Z-A** per ogni livello gerarchico
  - Toggle **Espandi/Comprimi** per ogni livello
  - **Subtotali** intermedi automatici
  - **Modalità Compatta** per risparmiare spazio orizzontale
  - Formattazione condizionale (Grassetto, Nascondi duplicati)
- **Funzione % sui Valori:** mostra un campo numerico come percentuale sul totale colonna anziché valore assoluto.
- **Barra di ricerca** nella lista Disponibili per trovare rapidamente campi su CSV con molte colonne.

### ↩ Undo / Redo
- Annulla e ripristina qualsiasi modifica alla struttura del report (drag & drop, rimozioni, riordinamenti).
- Storico fino a **40 stati** con pulsanti dedicati e shortcut da tastiera (`Ctrl+Z` / `Ctrl+Y`).

### 🎨 Anteprima e Stile
- **Anteprima Live:** Vedi il risultato in tempo reale mentre costruisci il report.
- **Temi Personalizzabili:** Scegli un colore principale e l'app calcola automaticamente le sfumature pastello per un look professionale.
- **Ordinamento colonne:** Clicca sull'intestazione di qualsiasi colonna numerica nell'anteprima per ordinare i dati (crescente/decrescente). Le righe di totale rimangono sempre in fondo.
- **Analisi Dati:**
  - Totali di Riga e Totali Generali
  - Calcolo automatico **% Incidenza** sul totale
  - Nascondi colonne vuote automaticamente

### 📋 Copia negli Appunti
- Bottone **"Copia Tabella"** nell'anteprima live.
- Copia in **due formati simultaneamente:**
  - **HTML formattato** → incollando in Word o Outlook mantiene colori, bordi e grassetti
  - **TSV (tab-separated)** → incollando in Excel o Google Sheets distribuisce i dati nelle celle
- Fallback automatico per browser che non supportano la Clipboard API moderna.

### 💾 Export & Persistenza
- **Excel (.xlsx) con FORMULE:** Genera un Excel contenente le formule `=SUBTOTAL()` e i calcoli percentuali. L'utente finale può modificare i dati e i totali si aggiorneranno automaticamente.
- **PDF Report:** Genera PDF pronti per la stampa, impaginati correttamente in orizzontale (Landscape).
- **Salvataggio Modelli (.json):** Salva la configurazione del report (struttura, stili, campi uniti, normalizzazioni) e ricaricala in futuro su nuovi dati CSV.

---

## ⌨️ Shortcut da Tastiera

| Shortcut | Azione |
|---|---|
| `Ctrl+Z` | Annulla ultima modifica |
| `Ctrl+Y` | Ripeti ultima modifica annullata |
| `Ctrl+S` | Salva modello configurazione |
| `Ctrl+Invio` | Avanza allo step successivo |

---

## 🚀 Come Usarlo

Non è necessaria alcuna installazione. L'applicazione è contenuta in un **singolo file HTML**.

1. **Scarica** il file `index.html` (o clona la repository).
2. Aprilo con un browser moderno (Chrome, Edge, Firefox).
3. **Step 1 — Dati:** Scegli se lavorare con **un file** o **due file**.
   - *Un file:* trascina il CSV, configura le intestazioni, normalizza se necessario, clicca **Procedi**.
   - *Due file:* carica e configura File 1, poi File 2, quindi imposta le colonne di join nella sezione **Configura Unione**. Verifica la cardinalità, scegli LEFT o INNER JOIN e clicca **Esegui Unione**.
4. **Step 2 — Struttura:** Configura il report trascinando i campi nelle aree Righe, Colonne e Valori. I badge F1/F2/🔗 indicano la provenienza di ciascun campo.
5. **Step 3 — Export:** Esporta in Excel o PDF, oppure copia la tabella negli appunti.

---

## 💡 Esempi di Utilizzo con Due File

| File 1 | File 2 | Colonna/e comuni | Report ottenibile |
|---|---|---|---|
| Anagrafica clienti (ID, Nome, Segmento) | Ordini (ID cliente, Data, Importo) | ID cliente | Fatturato per segmento → cliente → ordine |
| Piano di spesa (Capitolo, CDC, Impegno, Budget) | Liquidazioni (CDC, Impegno, Determina, Importo) | CDC + Impegno | Spesa per CDC → Impegno → Determina di liquidazione |
| Listino prodotti (Codice, Descrizione, Categoria) | Vendite (Codice, Quantità, Valore) | Codice prodotto | Vendite per categoria → prodotto con descrizione |
| Dipendenti (Matricola, Nome, Reparto) | Presenze (Matricola, Mese, Ore) | Matricola | Ore lavorate per reparto → dipendente → mese |

---

## ⚠️ Note sul JOIN N:M

Quando entrambi i file hanno valori ripetuti per le colonne di join (cardinalità N:M), i valori numerici nel report risultano **moltiplicati e gonfiati**. Esempio:

- File 1 ha 2 righe per l'impegno 3260001 (budget 10.000 e 20.000)
- File 2 ha 3 liquidazioni per lo stesso impegno (5.000, 3.000, 2.000)
- Il join produce **2 × 3 = 6 righe**: ogni liquidazione compare due volte, e la somma risulta 20.000 invece di 10.000

**Come risolvere:** aggiungi colonne alla chiave di join fino a ottenere cardinalità 1:N o 1:1. Se non esiste una chiave che elimini il N:M, considera di usare i file separatamente o di pre-aggregare i dati a monte.

**Quando il N:M è accettabile:** solo se il report è puramente testuale (non somma valori numerici) e l'obiettivo è vedere tutte le combinazioni possibili tra le righe dei due file.

---

## 🛠️ Tecnologie Utilizzate

Il progetto è costruito in **Vanilla JavaScript**, HTML5 e CSS3, senza framework. Si appoggia a librerie esterne caricate via CDN:

| Libreria | Utilizzo |
|---|---|
| [PapaParse](https://www.papaparse.com/) | Parsing veloce e affidabile dei file CSV |
| [SortableJS](https://sortablejs.github.io/Sortable/) | Drag & Drop fluido con ghost placeholder |
| [ExcelJS](https://github.com/exceljs/exceljs) | Generazione .xlsx con celle unite, stili e formule |
| [jsPDF](https://github.com/parallax/jsPDF) + AutoTable | Generazione PDF direttamente nel browser |
| [FileSaver.js](https://github.com/eligrey/FileSaver.js/) | Download affidabile dei file generati |

---

## 📸 Screenshots

Caricamento e configurazione dati:
![Step 1 - Caricamento dati](Caricamento.png)

Configurazione unione due file:
![Step 1b - Configurazione JOIN](Join.png)

Anteprima live del report colorato:
![Step 2 - Anteprima](Anteprima.png)

---

## 📋 Changelog

### v3.0
- ✅ **Modalità doppio file CSV** con selezione iniziale "Un file / Due file"
- ✅ **Auto-rilevamento colonne comuni** con similarità fuzzy sui nomi delle intestazioni
- ✅ **Chiave di join composita** (più colonne in AND) con aggiunta/rimozione dinamica delle coppie
- ✅ **Badge cardinalità in tempo reale** (1:1 / 1:N / N:M) con statistiche live (corrispondenze, orfani, righe risultanti)
- ✅ **LEFT JOIN e INNER JOIN** selezionabili dall'utente
- ✅ **Avviso N:M con conferma esplicita** invece di blocco rigido
- ✅ **Gestione automatica conflitti di nome** tra le colonne dei due file
- ✅ **Badge F1 / F2 / 🔗** su ogni campo nella lista disponibili e nelle aree di configurazione
- ✅ **Normalizzazione testo separata** per File 1 e File 2
- ✅ **Layout a due righe** per i campi nelle aree RIGHE/COLONNE/VALORI (nessun controllo tagliato)
- ✅ **Guida in-app aggiornata** con spiegazione completa di cardinalità, JOIN e casi d'uso

### v2.0
- ✅ Aggiunta funzionalità **Normalizzazione Testo** con anteprima live e 7 operazioni di pulizia
- ✅ Aggiunto bottone **Copia Tabella negli Appunti** (HTML + TSV)
- ✅ Sistema **Undo/Redo** con storico 40 stati e shortcut da tastiera
- ✅ Sostituzione di tutti gli `alert()` nativi con **notifiche Toast** non bloccanti
- ✅ **Spinner di caricamento** durante la generazione Excel/PDF
- ✅ **Ordinamento colonne** cliccando sull'intestazione nell'anteprima
- ✅ **Shortcut da tastiera** (`Ctrl+S`, `Ctrl+Z`, `Ctrl+Y`, `Ctrl+Invio`)
- ✅ Aree drag & drop con **sfondo colorato** distintivo per Righe/Colonne/Valori
- ✅ **Ghost placeholder** animato durante il drag
- ✅ Messaggi di errore CSV più specifici e descrittivi
- ✅ Guida completa aggiornata con tutte le nuove funzionalità

### v1.2
- ✅ Merge campi personalizzati con salvataggio nel modello .json
- ✅ Doppia intestazione pivot-style per export Excel
- ✅ Fix logica subtotali e percentuali

### v1.0
- 🎉 Release iniziale

---

## 🤝 Contribuire

Sentiti libero di aprire **Issues** o inviare **Pull Requests**. Essendo un file unico, il codice è strutturato in sezioni commentate (CSS, HTML, JS Logic) per facilitarne la lettura e la manutenzione.

## 📄 Licenza

Distribuito sotto licenza **MIT**. Vedi `LICENSE` per maggiori informazioni.
