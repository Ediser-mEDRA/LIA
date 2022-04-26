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
I parametri accettati della API sono elencati nella tabella seguente. E' possibile specificare uno o più parametri contemporaneamente, che verranno considerati “in AND”, cioè i record restituiti dovranno soddisfare tutti i criteri specificati. L'ordine dei parametri non è significativo.

| Parametro | Tipo/Enum | Case sensitive | Matching parziale | Descrizione | Esempio |
| --- | --- | --- | --- | --- | --- |
| `senderUsername` | string | SI | NO | username dell'utente che ha sottomesso la richiesta | `senderUsername=LUSEV000000` |
| `submissionId` | string | SI | NO | id della sottomissione | `submissionId=LVSU0000000` |
| `requestId` | string | SI | NO | id della richiesta | `requestId=LVRE0000000` |
| `publisherName` | string | NO | SI | denominazione dell'editore, completa o parziale | `publisherName=Ediser` |
| `isbnPrefix` | string | SI | NO | prefisso ISBN, con o senza trattini | `isbnPrefix=978-88-99630` <br/> `isbnPrefix=9788899630` |
| `isbnCodes` | string | SI | NO | lista di codici ISBN, con o senza trattini, separati da virgola. <br/> E' possibile indicare un massimo di 35 codici. | `isbnCodes=978-88-99630-00-3` <br/> `isbnCodes=9788899630003` <br/> `isbnCodes=978-88-99630-00-3,978-88-99630-01-0` <br/> `isbnCodes=9788899630003,9788899630010` |
| `publicationTitle` | string | NO | SI | titolo della pubblicazione, completo o parziale | `publicationTitle=Tra+editoria` |
| `requestType` | CERTIFICATION <br/> CONVERSION_AND_CERTIFICATION | SI | NO | tipo di richiesta | `requestType=CERTIFICATION` |
| `requestStatus` | WAITING_FOR_PROCESS <br/> IN_PROCESS <br/> REJECTED <br/> CLOSED <br/> LOADED | SI | NO | stato della richiesta | `requestStatus=CLOSED` |
| `requestPriority` | 0 <br/> 1 <br/> 2 | SI | NO | priorità della richiesta | `requestPriority=0` |
| `requestCreationDate` | date (ISO-8601) | SI | NO | data di creazione della richiesta, in formato ISO-8601, con la Time Part opzionale. <br/> Può essere espressa come: <br/>*singola data*:verranno restituite tutte le richieste create da quella data in poi;<br/>*range di date* (separate da virgola): verranno restituite tutte le richieste create tra la prima e la seconda data (incluse). | `requestCreationDate=2022-04-22` <br/> `requestCreationDate=2022-04-22T09:01:30` <br/> `requestCreationDate=2022-04-22,2022-04-24` <br/> `requestCreationDate=2022-04-22T09:01:30,2022-04-24T23:50:01` |
| `requestLastModifiedDate` | date (ISO-8601) | SI | NO | data di ultima modifica dello stato della richiesta, in formato ISO-8601, con la Time Part opzionale. <br/> Può essere espressa come: <br/>*singola data*:verranno restituite tutte le richieste il cui stato è stato aggiornato da quella data in poi;<br/>*range di date* (separate da virgola): verranno restituite tutte le richieste il cui stato è stato aggiornato tra la prima e la seconda data (incluse). | `requestLastModifiedDate=2022-04-22` <br/> `requestLastModifiedDate=2022-04-22T09:01:30` <br/> `requestLastModifiedDate=2022-04-22,2022-04-24` <br/> `requestLastModifiedDate=2022-04-22T09:01:30,2022-04-24T23:50:01` |
| `fileStatus` | WAITING_FOR_CHECK <br/> IN_CHECKING <br/> INACCESSIBLE <br/> IN_CONVERTING <br/> CONVERTED <br/> CONVERSION_REFUSED <br/> ACCESSIBLE <br/> IN_TESTING <br/> IN_APPROVING <br/> REFUSED <br/> ENDORSED <br/> APPROVED <br/> REFUSED_AND_IN_CONVERTING <br/> REFUSED_AND_CONVERTED <br/> BACKUP | SI | NO | stato di lavorazione del file | `fileStatus=ENDORSED` |
## Esempi di query
Ecco alcuni esempi di query:
- lo stato della richiesta LVRE0000000:

`https://vcc.libriitalianiaccessibili.it/api/vcc-files/v1/status?requestId=LVRE0000000`
- tutte le richieste di certificazione sottomesse dall'utente LUSEV000000 tra il 22 aprile 2022 e il 24 aprile 2022:

`https://vcc.libriitalianiaccessibili.it/api/vcc-files/v1/status?senderUsername=LUSEV000000&requestType=CERTIFICATION&requestCreationDate=2022-04-22,2022-04-24`
- tutti i file certificati nel mese di aprile 2022 con prefisso ISBN 978-88-99630 e priorità alta:

`https://vcc.libriitalianiaccessibili.it/api/vcc-files/v1/status?fileStatus=ENDORSED&isbnPrefix=978-88-99630&requestLastModifiedDate=2022-04-01,2022-04-30&requestPriority=2`

## Formato Risposta
E' possibile indicare nell'http header `Accept` della request il formato desiderato della risposta:
| Formato | Accept | Default |
| --- | --- | --- |
| JSON | `application/json` | :white_check_mark: |
| XML |  `application/xml` | |
## Risposta
La risposta conterrà 0 o più record, ognuno con i seguenti campi:
| Campo | Tipo | Può essere nullo | Descrizione | Esempio |
| --- | --- | --- | --- | --- |
| senderUsername | string | NO | username dell'utente che ha sottomesso la richiesta | `LUSEV000000` |
| submissionId | string |  NO | id della sottomissione | `LVSU0000000` |
| requestId | string | NO | id della richiesta | `LVRE0000000` |
| publisherName | string | NO | denominazione dell'editore associata al prefisso ISBN | `Ediser` |
| imprintName | string | NO | denominazione del marchio associata al prefisso ISBN | `Ediser` |
| isbnPrefix | string | NO | prefisso ISBN, sempre nella forma con trattini | `978-88-99630` |
| isbnCode | string | NO | codice ISBN, sempre nella forma con trattini | `978-88-99630-00-3` |
| publicationTitle | string | NO | titolo della pubblicazione | `Introduzione. Tra editoria e università` |
| productType | EPUB2 <br/> EPUB3 | NO | formato dell'epub | `EPUB2` |
| requestType | CERTIFICATION <br/> CONVERSION_AND_CERTIFICATION | NO | tipo di richiesta | `CERTIFICATION` |
| requestStatus | WAITING_FOR_PROCESS <br/> IN_PROCESS <br/> REJECTED <br/> CLOSED <br/> LOADED | NO | stato della richiesta | `CLOSED` |
| requestPriority | 0 <br/> 1 <br/> 2 | NO | priorità della richiesta | `0` |
| requestCreationDate | date (ISO-8601) | NO | data di creazione della richiesta, nel formato <br/> [YYYY]-[MM]-[DD]T[hh]:[mm]:[ss] | `2022-04-22T10:34:15` |
| requestLastModifiedDate | date (ISO-8601) | NO | data di ultima modifica dello stato della richiesta, nel formato <br/> [YYYY]-[MM]-[DD]T[hh]:[mm]:[ss] | `2022-04-23T12:50:43` |
| certificationDate | date (ISO-8601) | NO | data di certificazione del file, nel formato <br/> [YYYY]-[MM]-[DD]T[hh]:[mm]:[ss] | `2022-04-23T12:50:43` |
| fileStatus | WAITING_FOR_CHECK <br/> IN_CHECKING <br/> INACCESSIBLE <br/> IN_CONVERTING <br/> CONVERTED <br/> CONVERSION_REFUSED <br/> ACCESSIBLE <br/> IN_TESTING <br/> IN_APPROVING <br/> REFUSED <br/> ENDORSED <br/> APPROVED <br/> REFUSED_AND_IN_CONVERTING <br/> REFUSED_AND_CONVERTED <br/> BACKUP | NO | stato di lavorazione del file | `ENDORSED` |
| liaNotes | string | SI | note sulla lavorazione del file redatte da LIA | `testi alternativi per immagini` |
| automaticChecksOutput | string | SI | note sull'esito dei check automatici | `-----EPUB CHECK VERSIONE 4.0.2----- [...]` |
| accessibilityMetadata.onix | xml | SI | xml con i codici ONIX sull'accessibilità | `<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<metadata>\n   <ProductFormFeature>\n [...]` |
| accessibilityMetadata.epub | xml | SI | xml con gli EPUP Accessibility Metadata | `<?xml version=\"1.0\" encoding=\"UTF-8\"?><metadata><meta name=\"dcterms:conformsTo\ [...]` |

### Esempio di risposta in json
La risposta in formato json è composta da un array contenente da 0 a n oggetti, ognuno dei quali rappresenta una richiesta che corrisponde ai criteri utilizzati nella ricerca.

Questo è un esempio di risposta in formato json contenente una sola richiesta:
```json
[{
  "senderUsername": "LUSEV000000",
  "submissionId": "LVSU0000000",
  "requestId": "LVRE0000000",
  "publisherName": "Ediser",
  "imprintName": "Ediser",
  "isbnPrefix": "978-88-99630",
  "isbnCode": "978-88-99630-00-3",
  "publicationTitle": "Introduzione. Tra editoria e università",
  "productType": "EPUB2",
  "requestType": "CERTIFICATION",
  "requestStatus": "CLOSED",
  "requestPriority": 0,
  "requestCreationDate": "2022-04-22T10:34:15",
  "requestLastModifiedDate": "2022-04-23T12:50:43",
  "certificationDate": "2022-04-23T12:50:43",
  "fileStatus": "ENDORSED",
  "liaNotes": "testi alternativi per immagini",
  "automaticChecksOutput": "-----EPUB CHECK VERSIONE 4.2.4-----\n\n***ECCEZIONI***\n\n***ERRORI***\n\n***WARNING***\n\n\n-----ACCESSIBILITY METADATA CHECKER-----\nL'epub non ha metadati dell'accessibilità\n\n\n-----ACE CHECKER-----\n    Processing /production/lia/vcc/repository/vcc-files/LVFI0000000.vcc\n    Parsing EPUB\n    Analyzing accessibility metadata\n    Checking package...\n    - OEBPS/g9788899630003.opf: 4 issues found\n    Checking documents...\n    - Text/g9788899630003_cov01.xhtml: No issues found\n Consolidating results...\n    Copying data\n    Saving JSON report\n    Saving HTML report\n    Done.\n",
  "accessibilityMetadata": {
    "onix": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<metadata>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>00</ProductFormFeatureValue>\n      <ProductFormFeatureDescription>Questo è un esempio di accessibility summary</ProductFormFeatureDescription>\n   </ProductFormFeature>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>01</ProductFormFeatureValue>\n   </ProductFormFeature>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>10</ProductFormFeatureValue>\n   </ProductFormFeature>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>11</ProductFormFeatureValue>\n   </ProductFormFeature>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>13</ProductFormFeatureValue>\n   </ProductFormFeature>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>14</ProductFormFeatureValue>\n   </ProductFormFeature>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>22</ProductFormFeatureValue>\n   </ProductFormFeature>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>94</ProductFormFeatureValue>\n   </ProductFormFeature>\n   <ProductFormFeature>\n      <ProductFormFeatureType>09</ProductFormFeatureType>\n      <ProductFormFeatureValue>97</ProductFormFeatureValue>\n      <ProductFormFeatureDescription>http://lia.libriitalianiaccessibili.it/9788899630003</ProductFormFeatureDescription>\n   </ProductFormFeature>\n</metadata>\n",
    "epub": "<?xml version=\"1.0\" encoding=\"UTF-8\"?><metadata><meta name=\"dcterms:conformsTo\" content=\"EPUB-A11Y-11_WCAG-21-AA\"/><meta name=\"a11y:certifiedBy\" content=\"Fondazione LIA\" id=\"certifier\"/><meta name=\"a11y:certifierCredential\"         content=\"https://www.fondazionelia.org/\"/><meta name=\"a11y:certifierReport\"         content=\"https://lia.libriitalianiaccessibili.it/9788899630003\"/><meta name=\"lia:certificationDate\" content=\"2022-04-23T12:50:43+02:00\"/><meta name=\"schema:accessibilitySummary\"         content=\"Questo è un esempio di accessibility summary\"         xml:lang=\"it\"/><meta name=\"schema:accessibilityFeature\" content=\"alternativeText\"/><meta name=\"schema:accessibilityFeature\" content=\"readingOrder\"/><meta name=\"schema:accessibilityFeature\" content=\"tableOfContents\"/><meta name=\"schema:accessibilityHazard\" content=\"unknown\"/><meta name=\"schema:accessMode\" content=\"textual\"/><meta name=\"schema:accessMode\" content=\"visual\"/><meta name=\"schema:accessModeSufficient\" content=\"textual,visual\"/></metadata>"
  }
}]
```

### Esempio di risposta in xml
La risposta in formato xml è composta da un elemento radice `<response>` contenente da 0 a n `<item>`, ognuno dei quali rappresenta una richiesta che corrisponde ai criteri utilizzati nella ricerca.

Questo è un esempio di risposta in formato xml contenente una sola richiesta:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<response>
  <item>
    <senderUsername>LUSEV000000</senderUsername>
    <submissionId>LVSU0000000</submissionId>
    <requestId>LVRE0000000</requestId>
    <publisherName>Ediser</publisherName>
    <imprintName>Ediser</imprintName>
    <isbnPrefix>978-88-99630</isbnPrefix>
    <isbnCode>978-88-99630-00-3</isbnCode>
    <publicationTitle>Introduzione. Tra editoria e università</publicationTitle>
    <productType>EPUB2</productType>
    <requestType>CERTIFICATION</requestType>
    <requestStatus>CLOSED</requestStatus>
    <requestPriority>0</requestPriority>
    <requestCreationDate>2022-04-22T10:34:15</requestCreationDate>
    <requestLastModifiedDate>2022-04-23T12:50:43</requestLastModifiedDate>
    <certificationDate>2022-04-23T12:50:43</certificationDate>
    <fileStatus>ENDORSED</fileStatus>
    <liaNotes>testi alternativi per immagini</liaNotes>
    <automaticChecksOutput>-----EPUB CHECK VERSIONE 4.2.4----- ***ECCEZIONI*** ***ERRORI*** ***WARNING*** -----ACCESSIBILITY METADATA CHECKER----- L'epub non ha metadati dell'accessibilità -----ACE CHECKER----- Processing /production/lia/vcc/repository/vcc-files/LVFI0000000.vcc
      Parsing EPUB Analyzing accessibility metadata Checking package... - OEBPS/g9788899630003.opf: 4 issues found Checking documents... - Text/g9788899630003_cov01.xhtml: No issues found
      Consolidating results... Copying data Saving JSON report Saving HTML report Done.
    </automaticChecksOutput>
    <accessibilityMetadata>
      <onix>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>00</ProductFormFeatureValue>
          <ProductFormFeatureDescription>Questo è un esempio di accessibility summary</ProductFormFeatureDescription>
        </ProductFormFeature>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>01</ProductFormFeatureValue>
        </ProductFormFeature>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>10</ProductFormFeatureValue>
        </ProductFormFeature>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>11</ProductFormFeatureValue>
        </ProductFormFeature>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>13</ProductFormFeatureValue>
        </ProductFormFeature>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>14</ProductFormFeatureValue>
        </ProductFormFeature>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>22</ProductFormFeatureValue>
        </ProductFormFeature>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>94</ProductFormFeatureValue>
        </ProductFormFeature>
        <ProductFormFeature>
          <ProductFormFeatureType>09</ProductFormFeatureType>
          <ProductFormFeatureValue>97</ProductFormFeatureValue>
          <ProductFormFeatureDescription>http://lia.libriitalianiaccessibili.it/9788899630003</ProductFormFeatureDescription>
        </ProductFormFeature>
      </onix>
      <epub>
        <meta content="EPUB-A11Y-11_WCAG-21-AA" name="dcterms:conformsTo" />
        <meta content="Fondazione LIA" id="certifier" name="a11y:certifiedBy" />
        <meta content="https://www.fondazionelia.org/" name="a11y:certifierCredential" />
        <meta content="https://lia.libriitalianiaccessibili.it/9788899630003" name="a11y:certifierReport" />
        <meta content="2022-04-23T12:50:43+02:00" name="lia:certificationDate" />
        <meta content="Questo è un esempio di accessibility summary" name="schema:accessibilitySummary" xml:lang="it" />
        <meta content="alternativeText" name="schema:accessibilityFeature" />
        <meta content="readingOrder" name="schema:accessibilityFeature" />
        <meta content="tableOfContents" name="schema:accessibilityFeature" />
        <meta content="unknown" name="schema:accessibilityHazard" />
        <meta content="textual" name="schema:accessMode" />
        <meta content="visual" name="schema:accessMode" />
        <meta content="textual,visual" name="schema:accessModeSufficient" />
      </epub>
    </accessibilityMetadata>
  </item>
</response>
```

## Liste Codici
### Product Type
### Request Type
### Request Status
### Request Priority
### File Status
