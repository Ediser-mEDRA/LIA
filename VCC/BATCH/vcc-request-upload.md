# VCC REQUEST UPLOAD BATCH
Funzionalità che consente di caricare nel VCC un insieme di file da certificare.
## Formato file
Il file batch deve essere in formato zip e deve contenere:
- i file da certificare;
- un file `metadata.txt` con la descrizione dei file da certificare.

I file da certificare e il file `metadata.txt` devono essere nella root del file zip.

## Metadata.txt
Il file `metadata.txt` deve essere tab-separated ed avere la seguente struttura:
- [intestazione](#intestazione): la prima riga deve contenere l'intestazione;
- [records](#record): ogni riga successiva deve descrivere uno dei file da certificare contenuti nello zip.

L'encoding del file deve essere `ISO-8859-1`.

### Intestazione
La riga di intestazione viene saltata durante il processo di import, quindi può contenere qualsiasi stringa, tuttavia il consiglio è quello di inserire la seguente stringa

`IDENTIFIER	TITLE	PRODUCT_TYPE	DRM_TYPE	REQUEST_TYPE	NOTES	FILE_NAME	PRIORITY	IDENTIFIER_PREFIX`

### Record
Ogni riga che descrive un file contenuto dello zip deve avere i seguenti campi:

| Campo | Tipo | Può essere nullo | Vincoli | Descrizione | Esempio |
| --- | --- | --- | --- | --- | --- |
| IDENTIFIER | string | SI | | il codice ISBN (senza trattini) della pubblicazione  | `9788815369604` |
| TITLE | string | NO | massimo 200 caratteri | il titolo della pubblicazione | `Arricchimento` |
| PRODUCT_TYPE | [enum](#product-type) | NO |  | formato della pubblicazione | `EPUB3` |
| DRM_TYPE | [enum](#drm-type) | NO |  | DRM applicato alla pubblicazione | `SOCIAL_DRM` |
| REQUEST_TYPE | [enum](#request-type) | NO |  | il tipo di richiesta | `SOCIAL_DRM` |
| NOTE | string | SI | massimo 2000 caratteri | note per LIA | `alcune immagini hanno delle descrizioni generiche` |
| FILE_NAME | string | NO | massimo 256 caratteri | nome del file (compresa l'estensione) | `primo_file.epub` |
| PRIORITY | [enum](#request-priority) | NO | | priorità della richiesta | `1` |
| IDENTIFIER_PREFIX | string | SI | | identificativo del marchio (da usare nel caso l'ISBN non sia disponibile) | `LVPC0000` |

## Liste Codici
### Product Type
I valori ammessi per il campo `PRODUCT_TYPE` sono:
| Codice | Label | Descrizione |
| --- | --- | --- |
| `E_PUB` | EPUB2 | |
| `E_PUB3` | EPUB3 | |
### DRM Type
I valori ammessi per il campo `DRM_TYPE` sono:
| Codice | Label | Descrizione |
| --- | --- | --- |
| `ADOBE_DRM` | Adobe DRM | |
| `SOCIAL_DRM` | Social DRM | |
| `NO_DRM` | Nessun DRM | |
| `UNKNOWN` | Sconosciuto | |
### Request Type
I valori ammessi per il campo `REQUEST_TYPE` sono:
| Codice | Label | Descrizione |
| --- | --- | --- |
| `CERTIFICATION` | Certificazione | |
| `CONVERSION_AND_CERTIFICATION` | Conversione E Certificazione | |
### Request Priority
I valori ammessi per il campo `PRIORITY` sono:
| Codice | Label | Descrizione |
| --- | --- | --- |
| `0` | Bassa | |
| `1` | Normale | |
| `2` | Alta | |
