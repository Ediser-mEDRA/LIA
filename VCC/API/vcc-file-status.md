# VCC FILE STATUS API
API che permette di ottenere lo stato di lavorazione di una o più richieste inviate al VCC di LIA. Se il file risulta certificato, nella risposta vengono inseriti i codici ONIX e gli EPUB Accessibility Metadata.
## Autenticazione
La API è sotto basic authentication. Le credenziali sono le stesse utilizzate per accedere al VCC. 
## Autorizzazione
Le regole di autorizzazione sono le stesse in vigore nel VCC, cioè un utente ha visibilità solamente sui prefissi ISBN per i quali è abilitato.
## Metodo
LA API accetta una HTTP Request GET con uno o più dei parametri indicati nel paragrafo successivo.
## Endpoint
L'endpoint della API è 
`https://vcc.libriitalianiaccessibili.it/api/vcc-files/v1/status`
## Parametri
I parametri accettati della API sono:

I criteri di ricerca saranno simili a quelli della pagina dell’elenco richieste:
-	senderUsername (username di chi ha sottomesso la richiesta, es. “LUSEV000164”);
-	submissionId (id della sottomissione, es. “LVSU0000168”);
-	requestId (id della richiesta, es. “LVRE0000245”);
-	publisherName (denominazione dell’editore, completa o parziale, es. “Chiarelettere” – case insensitive);
-	isbnPrefix (prefisso ISBN, con o senza trattini, es. “978886002” o “978-88-6002”);
-	isbnCodes (lista di codici ISBN, con o senza trattini, separati da virgola es. “9788860029997,9788860024459”). E’ possibile indicare un massimo di 35 codici;
-	publicationTitle (titolo della pubblicazione, completo o parziale, es. “asini I” – case insensitive);
-	requestType (codice interno del tipo di richiesta):
o	CERTIFICATION;
o	CONVERSION_AND_CERTIFICATION,
-	requestStatus (codice interno dello stato della richiesta):
o	WAITING_FOR_PROCESS;
o	IN_PROCESS;
o	REJECTED;
o	CLOSED;
o	LOADED.
-	requestPriority (codice interno della priorità della richiesta):
o	0;
o	1;
o	2.
-	requestCreationDate (data di creazione della richiesta, in formato ISO-8601, con la Time Part opzionale), espressa come:
o	singola data (es. “2011-12-03” oppure “2011-12-03T10:15:30”): verranno restituite tutte le richieste create da quella data alla data corrente;
o	range di date, separate da virgola (es. “2011-01-01,2012-12-31” oppure “2011-01-01T00:00:00,2012-12-31T23:59:59”): verranno restituite tutte le richieste inviate tra la prima e la seconda data;
-	requestLastModifiedDate (data di ultima modifica della richiesta, in formato ISO-8601, con la Time Part opzionale), espressa come:
o	singola data (es. “2011-12-03” oppure “2011-12-03T10:15:30”) : verranno restituite tutte le richieste aggiornate da quella data alla data corrente;
o	range di date, separate da virgola (es. “2011-01-01,2012-12-31” oppure “2011-01-01T00:00:00,2012-12-31T23:59:59”) : verranno restituite tutte le richieste aggiornate tra la prima e la seconda data;
-	fileStatus (codice interno dello stato di lavorazione del file corrente):
o	WAITING_FOR_CHECK;
o	IN_CHECKING;
o	INACCESSIBLE;
o	IN_CONVERTING;
o	CONVERTED;
o	CONVERSION_REFUSED;
o	ACCESSIBLE;
o	IN_TESTING;
o	IN_APPROVING;
o	REFUSED;
o	ENDORSED;
o	APPROVED;
o	REFUSED_AND_IN_CONVERTING;
o	REFUSED_AND_CONVERTED;
o	BACKUP.
Sarà possibile specificare uno o più criteri contemporaneamente, che verranno considerati “in AND”, cioè i record restituiti dovranno soddisfare tutti i criteri specificati.
La risposta sarà disponibile nei formati JSON (default) o XML. Il formato desiderato andrà specificato tramite content negotiation. I campi presenti nella risposta saranno:
-	senderUsername;
-	submissionId;
-	requestId;
-	publisherName (è la label associata al prefisso ISBN, non quella presente nei metadati ONIX);
-	imprintName (è la label associata al prefisso ISBN, non quella presente nei metadati ONIX);
-	isbnPrefix (sempre nella forma con trattini);
-	isbnCode (il codice ISBN, sempre nella forma con trattini);
-	publicationTitle (il titolo della pubblicazione dichiarato dall’utente all’atto della sottomissione del file);
-	productType (il tipo di prodotto: EPUB2 o EPUB3);
-	requestType;
-	requestStatus;
-	requestPriority;
-	requestCreationDate (nel formato ISO-8601 [YYYY]-[MM]-[DD]T[hh]:[mm]:[ss]);
-	requestLastModifiedDate (nel formato ISO-8601 [YYYY]-[MM]-[DD]T[hh]:[mm]:[ss]);
-	certificationDate ;
-	fileStatus (del file corrente);
-	liaNotes (le note LIA);
-	automaticChecksOutput (la nota sull’esito dei check automatici);
-	accessibilityMetadata, composto dai due sottocampi:
o	onix (snippet XML con i codici ONIX);
o	epub (snippet XML con gli Accessibility Metadata).

L’endpoint della api è 


