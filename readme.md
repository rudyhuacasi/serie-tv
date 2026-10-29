# Brief — Report serie TV (TVmaze)

## Contesto

Il team editoriale di una piattaforma di streaming vuole due report analitici sulle serie TV, basati sui dati pubblici di [TVmaze](https://www.tvmaze.com/api). Tu sei responsabile di realizzare un’applicazione che li produca.

Nota: TVmaze copre **serie TV** (non il catalogo cinematografico). Per questa esercitazione il dominio è quello.

## Deliverable

1. **Repository Git nuovo** (remoto pubblico o privato condiviso con i docenti), dedicato esclusivamente a questa esercitazione. Non riusare un progetto già esistente del corso.
2. Codice funzionante che, all’avvio, recupera i dati dall’API e stampa in console (o scrive su file) i due report descritti sotto, in JSON. La forma dell’oggetto (annidamento, nomi delle chiavi, organizzazione delle sezioni) la progetti tu: l’unico vincolo è che ogni report esponga **tutti gli attributi** elencati per quel report.
3. `README.md` con: come installare le dipendenze, come avviare il programma, una frase sullo **scope** dei dati (es. solo `page=0` di `/shows`, oppure più pagine) e una frase su come hai definito l’«anno di riferimento» per il periodo recente.

Stack, struttura delle cartelle, librerie e stile di codice sono a tua scelta, purché il risultato sia eseguibile localmente con istruzioni chiare.

## Fonte dati

Base URL: `https://api.tvmaze.com`

Documentazione: [https://www.tvmaze.com/api](https://www.tvmaze.com/api)

| Endpoint | Contenuto utile |
| --- | --- |
| `GET /shows?page={n}` | Elenco paginato di serie (rating, generi, premiere, network, …) |
| `GET /shows/{id}` | Dettaglio di una serie |
| `GET /shows/{id}/episodes` | Episodi (stagione, numero, airdate, runtime, rating, …) |
| `GET /shows/{id}?embed=episodes` | Alternativa: serie + episodi in una sola risposta |

Note operative:

- **Nessuna API key.** Attribuisci TVmaze come fonte nel README (licenza CC BY-SA).
- Rate limit indicativo: circa 20 richieste ogni 10 secondi per IP; in caso di `429`, gestisci con pausa/riprova o messaggio chiaro.
- Scope minimo obbligatorio: almeno `GET /shows?page=0` (~240 serie). Se pagini oltre, documentalo.
- **Anno di riferimento (periodo recente):** anno solare della `premiered` più recente presente nel dataset che hai scaricato (non la data di sistema della macchina). Esempio: se l’ultima premiere nel tuo set è del 2014, il periodo recente è l’anno 2014.
- **All time** = tutte le serie dello scope, senza filtro sull’anno di premiere.
- Serie **senza** `rating.average` non entrano nelle classifiche per voto.
- A parità di metrica, ordina per `id` crescente.
- Se per un periodo ci sono meno di 3 elementi, restituisci solo quelli disponibili.

## Report 1 — Top 3 serie per rating

**Criterio di ranking:** voto medio (`rating.average`), non il peso (`weight`) né altri campi.

**Output atteso.** Il report deve essere un JSON che, per entrambi i periodi (all time e anno di riferimento), elenca fino a tre serie in classifica. Per ogni serie in classifica devono essere presenti, in qualunque annidamento tu scelga:

- la posizione in classifica;
- l’identificativo della serie;
- il nome;
- i generi;
- lo status (es. `Ended`, `Running`, …);
- la data di premiere;
- il rating medio;
- il runtime (usa `runtime`, oppure `averageRuntime` se `runtime` è assente);
- il nome del network oppure del webChannel (se entrambi assenti, gestiscilo in modo esplicito e documentato).

Inoltre il report deve dichiarare:

- un identificativo o etichetta che lo distingua come report serie per rating;
- il momento in cui è stato generato;
- l’anno (o la data) di ancoraggio usato per il periodo recente;
- l’intervallo considerato per il periodo recente; per l’all time puoi omettere l’intervallo o esplicitarlo come assente — scegli tu, ma la scelta deve essere coerente e leggibile;
- lo scope dei dati (quali pagine `/shows` hai usato).

## Report 2 — Scheda episodi delle top 3 all-time

**Input:** le tre serie della classifica **all time** del Report 1.

**Output atteso.** Il report deve essere un JSON che, per ciascuna di queste serie, espone una scheda episodi costruita da `/shows/{id}/episodes` (o dall’embed equivalente). Per ogni serie devono essere presenti:

- l’identificativo e il nome della serie;
- il numero di stagioni distinte;
- il numero totale di episodi;
- il runtime totale in minuti (somma dei `runtime` degli episodi; se un episodio non ha runtime, decidi tu una strategia sensata e documentala);
- la data del primo e dell’ultimo episodio (`airdate`);
- l’episodio con rating più alto: identificativo, nome, stagione, numero episodio, rating medio (se nessun episodio ha rating, gestiscilo in modo esplicito).

Come nel Report 1, il JSON deve anche dichiarare etichetta/identificativo del report, momento di generazione e riferimento alle tre serie di partenza. Stessa libertà sulla forma: conta che tutti questi attributi siano recuperabili dal JSON, non come li organizzi.

## Criteri di accettazione

Il lavoro è considerato completo se:

- il repo è nuovo e contiene istruzioni di avvio funzionanti;
- i due report sono JSON e contengono tutti gli attributi richiesti sopra (la struttura la definisci tu);
- ranking, periodi, tie-break e uso delle top 3 all-time nel Report 2 rispettano le regole di questo brief;
- errori di rete o `429` non fanno crashare il programma in modo silenzioso: messaggio chiaro e exit non zero, oppure gestione documentata nel README.

## Fuori scope

UI grafica, autenticazione, scrittura su database, deploy, test automatici obbligatori. Puoi aggiungerli, ma non sono richiesti per la consegna.
