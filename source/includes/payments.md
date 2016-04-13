<a name="Payments"></a>
# Pagamenti

Ogni flusso di pagamento, in entrata e uscita, viene memorizzato su Adamo. Tramite le API è possibile visualizzare, modificare, eliminare questi flussi.

<a name="Income"></a>
## Pagamenti in ingresso
```json
{
   "Income": {
       "id": 1248,
       "date": "2015-05-29",
       "amount": 0,
       "mean": null,
       "invoice_id": null,
       "receipt_id": null,
       "causal": null,
       "created": "2015-05-29 09:23:35",
       "updated": "2015-05-29 09:23:35",
       "note": "",
       "resource": null,
       "resource_id": null
   }
}
```

I pagamenti in ingresso sono chiamati **Income**. 
Ogni **Income** simboleggia un flusso di pagamento che hai ricevuto da qualcuno.

Gli attributi sono i seguenti:

Attributo | Tipo | Descrizione
--------- |  ----------- | ----
date | Date. Default: *data di oggi* | Quando è stato effettuato il pagamento.
amount | Float | L'importo del pagamento
mean | String | Come è stato effettuato il pagamento. Es: Bonifico, Carta di credito.
invoice_id | Integer | L'id della fattura emessa associata a questo pagamento
receipt_id | Integer |L'id della ricevuta emessa associata a questo pagamento
causal | String | Il motivo di questo pagamento.
note | String | Altre note
resource | String | Il nome del conto su cui è finito questo pagamento.
resource_id | Integer | L'id del conto su cui è finito questo pagamento.

Il conto, chiamato *resource*, può essere un valore testuale oppure un oggetto **Resource**, e simboleggia il conto dal quale entrano e escono i flussi di cassa, es: Banca XX, Cassa, PostePay di Mario, ecc...

Indicando un *resource_id* valido, il saldo della **Resource** associata verrà incrementato di conseguenza.

<a name="Income-add"></a>
### Crea un nuovo pagamento

> Richiesta di Esempio:

```http
POST /api/incomes/?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/incomes/	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Corpo della richiesta:

```json
{
    "Income": {
        "amount" : 200,
        "mean" : "Bonifico",
        "causal" : "Rimborso"
    }
}
```
> Risposta di esempio:

```json
{
    "Income": {
        "id": 1261,
        "date": "2015-06-08",
        "amount": 200,
        "mean": "Bonifico",
        "invoice_id": null,
        "receipt_id": null,
        "causal": "Rimborso",
        "created": "2015-06-08 08:23:10",
        "updated": "2015-06-08 08:23:10",
        "note": "",
        "resource": null,
        "resource_id": null
    },
    "Invoice": {...}
}
```

Accedendo all'endpoint:

`POST /api/incomes/`

Si può facilmente creare un nuovo pagamento. 

Se l'attributo *date* non viene inviato con la richiesta, allora verrà inserita la data di oggi.

Viene restituito un oggetto comprendente un oggetto **Income** in caso di successo.

Inviando con la richiesta un valore *invoice_id* valido, si può facilmente creare una nuova tranche di pagamento per una fattura. In questo modo, per questa richiesta, verrà restituito anche l'oggetto **Invoice** associato all'id fornito.

### Ottieni un pagamento

Indicando l'id del pagamento in ingresso creato si può ottenere l'oggetto **Income**.

`GET /api/incomes/{id}`

Viene restituito un oggetto contenente un oggetto **Income** in caso di successo, un [Errore](#Errore) altrimenti.

E' possibile utilizzare in querystring l'attributo *expand* pari a *invoice* per ottenere, oltre all'oggetto **Income** anche l'eventuale **Invoice** associato.

### Elimina un pagamento

Indicando l'id del pagamento in ingresso creato si può eliminare l'oggetto **Income** in modo irreversibile.

`DELETE /api/incomes/{id}`

Viene restituito un oggetto contenente un attributo *code* pari a 200 in caso di successo, un [Errore](#Errore) altrimenti.

Si noti che eliminando un pagamento verrà aggiornato (decrementato) il saldo dell'oggetto **Resource** associato. 

### Elenca i pagamenti

E' possibile elencare tutti i pagamenti in entrata tramite l'endpoint: 

`GET /api/incomes/`

Verrà restituito un array comprendente oggetti che contengono gli oggetti **Income**.

Se nessun **Income** è presente verrà restituito un array vuoto.

<a name="Outflow"></a>
## Pagamenti in uscita

```json
{
   "Outflow": {
       "id": 153,
       "date": "2015-01-02",
       "amount": 244.00,
       "arrival_id": null,
       "tribute_id": null,
       "credit_note_id": null,
       "mean": "cash",
       "causal": null,
       "created": "2015-01-18",
       "updated": "2015-01-18 21:43:01",
       "note": "",
       "resource": "Cassa",
       "resource_id": null
	}
}
```

I pagamenti in uscita sono chiamati **Outflow**. 
Ogni **Outflow** simboleggia un flusso di pagamento che hai dato a qualcuno.

Gli attributi sono i seguenti:

Attributo | Tipo | Descrizione
--------- |  ----------- | ----
date | Date. Default: *data di oggi* | Quando è stato effettuato il pagamento.
amount | Float | L'importo del pagamento
mean | String | Come è stato effettuato il pagamento. Es: Bonifico, Carta di credito.
arrival_id | Integer | L'id della spesa fatta associata a questo pagamento
credit_note_id | Integer | L'id della nota di credito emessa associata a questo pagamento
causal | String | Il motivo di questo pagamento.
note | String | Altre note
resource | String | Il nome del conto da cui è uscito questo pagamento.
resource_id | Integer | L'id del conto da cui è uscito questo pagamento.

Il conto, chiamato *resource*, può essere un valore testuale oppure un oggetto **Resource**, e simboleggia il conto dal quale entrano e escono i flussi di cassa, es: Banca XX, Cassa, PostePay di Mario, ecc...

Indicando un *resource_id* valido, il saldo della **Resource** associata verrà decrementato di conseguenza.

<a name="Outflow-add"></a>
### Crea un nuovo pagamento


> Richiesta di Esempio:

```http
POST /api/outflows/?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/outflows/	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Corpo della richiesta:

```json
{
    "Outflow": {
        "amount" : 200,
        "mean" : "Bonifico",
        "causal" : "Rimborso"
    }
}
```
> Risposta di esempio:

```json
{
    "Outflow": {
        "id": 613,
        "date": "2015-06-08",
        "amount": 200,
        "arrival_id": null,
        "tribute_id": null,
        "credit_note_id": null,
        "mean": "Bonifico",
        "causal": "Rimborso",
        "updated": "2015-06-08 08:49:16",
        "updated": "2015-06-08 08:49:16",
        "note": "",
        "resource": null,
        "resource_id": null
    },
    "Arrival": {...},
    "CreditNote": {...}
}
```

Accedendo all'endpoint:

`POST /api/outflows/`

Si può facilmente creare un nuovo pagamento. 

Se l'attributo *date* non viene inviato con la richiesta, allora verrà inserita la data di oggi.

Viene restituito un oggetto comprendente un oggetto **Outflow** in caso di successo.

Inviando con la richiesta un valore *arrival_id* valido, si può facilmente creare una nuova tranche di pagamento per una spesa effettuata. In questo modo, per questa richiesta, verrà restituito anche l'oggetto **Arrival** associato all'id fornito.

Stessa cosa per *credit_note_id* e l'oggetto **CreditNote**. 


###Ottieni un pagamento

Indicando l'id del pagamento in uscita creato si può ottenere l'oggetto **Outflow**.

`GET /api/outflows/{id}`

Viene restituito un oggetto contenente un oggetto **Outflow** in caso di successo, un [Errore](#Errore) altrimenti.

E' possibile utilizzare in querystring l'attributo *expand* pari a *arrival* o *credit_note* per ottenere, oltre all'oggetto **Outflow** anche l'eventuale **Arrival** o **CreditNote** associato.

###Elimina un pagamento

Indicando l'id del pagamento in uscita creato si può eliminare l'oggetto **Outflow** in modo irreversibile.

`DELETE /api/outflows/{id}`

Viene restituito un oggetto contenente un attributo *code* pari a 200 in caso di successo, un [Errore](#Errore) altrimenti.

Si noti che eliminando un pagamento verrà aggiornato (aumentato) il saldo dell'oggetto **Resource** associato. 

### Elenca i pagamenti

E' possibile elencare tutti i pagamenti in uscita tramite l'endpoint: 

`GET /api/outflows/`

Verrà restituito un array comprendente oggetti che contengono gli oggetti **Outflow**.

Se nessun **Outflow** è presente verrà restituito un array vuoto.

## Conti

```json
{
   "Resource": {
       "id": 221,
       "name": "Cassa",
       "situation": 1500,
       "created": "2015-01-18 21:27:00",
       "updated": "2015-05-29 09:31:23"
   }
}
``` 
I conti, chiamati **Resource**, sono i diversi conti su cui possiedi dei soldi. Ad esempio potresti voler tenere traccia del tuo saldo in Banca, in Cassa, sulla tua Carta di Credito, sulla tua Carta Prepagata, ecc... 

Utilizzando i conti puoi fare in modo che i diversi pagamenti in entrata e uscita aggiornino automaticamente il saldo del rispettivo conto.
Quando creerai un nuovo **Income** o **Outflow**, indicando un *resource_id* valido, il sistema automaticamente si preoccuperà di incrementare o di scalare l'importo dal rispettivo conto.

Attributo | Tipo | Descrizione
--- | --- | ---
name | String *Obbligatorio* | Nome del conto
situation | Float | Importo attualmente sul conto

### Elenca tutti i conti

Consula la situazione dei tuoi conti accedendo all'endpoint:

`GET /api/resources/`

Ti verrà restituito un array contente degli oggetti che contengono l'oggetto **Resource**. 

Se non hai nessun conto ti verrà restituito un array vuoto.

### Crea un nuovo conto

```http
POST /api/resources/?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/outflows/	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Corpo della richiesta:

```json
{
    "Resource" : {
        "name" : "Carta Prepagata di Mario",
        "situation" : 1400
    }
}
```
> Risposta di esempio:

```json
{
    "Resource": {
        "id": 237,
        "name": "Carta Prepagata di Mario",
        "situation": 1400,
        "created": "2015-06-08 09:36:07",
        "updated": "2015-06-08 09:37:35"
    }
}
```

Puoi creare diversi conti. Per crearne uno passa un oggetto **Resource** all'endpoint:

`POST /api/resources/`

In caso non passi l'attributo *situation* questa verrà inizializzato a zero.

Ti verrà restituito l'oggetto appena creato in caso di successo.

### Elimina un conto

Per eliminare un conto, sapendo il suo id, accedi all'endpoint:

`DELETE /api/resources/{id}`

In caso di successo ti verrà restituito un oggetto con *code* pari a 200.

Nota che tutti i pagamenti, **Income** e **Outflow** associati al conto **NON** verranno eliminati dal sistema. Verranno conservato con un *resource_id* nullo, quindi senza essere associati ad alcun conto. 

### Modifica un conto

Modifica il nome o la situazione di un conto inviando un oggetto **Resource** all'endpoint:

`PUT /api/resources/{id}`

Ti verrà restituito l'oggetto **Resource** in caso di successo o un [Errore](#Errore) altrimenti.

<a name="cashflows-index"></a>
### Elenca tutti i movimenti

> Richiesta:

```http
POST /api/cashflows/?resource_id=5&access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/cashflows?resource_id=5
	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```
> Risposta:

```json
[
    {
        "type": "outflow",
        "id": "615",
        "date": "2015-06-08",
        "amount": "800",
        "arrival_id": null,
        "credit_note_id": null,
        "mean": null,
        "causal": null,
        "created": "2015-06-08",
        "updated": "2015-06-08 09:36:45",
        "note": "",
        "domain_id": "254",
        "resource": "Carta",
        "resource_id": "237",
        "Arrival": {...}
    },
    {...}
 ]
```
Per elencare tutti i movimenti, sia gli **Income** che i **Outflow**, ordinati dal più recente al più vecchio, è possibile utilizzare l'endpoint: 

`GET /api/cashflows/`

#### Parametri

E' possibile passare in querystring i parametri:

Parametro | Default | Descrizione | Esempio
--- | --- | --- | ---
resource_id | nessun filtro |Filtra solamente i movimenti riferiti ad uno specifico conto | 5
from | 30 giorni fa |Filtra i movimenti successivi ad una certa data | 2015-01-01
to | oggi |Filtra i movimenti precedenti ad una certa data | 2015-12-31

#### Restituisce

Restituisce un array di oggetti. Ogni oggetto è un movimento. L'attributo **type** indica il tipo di oggetto, e può essere pari a **outflow** oppure **income**, ad indicare se si tratta di uno o dell'altro. Dentro ogni oggetto saranno presenti anche gli oggetti associati. 

Nel caso non siano presenti oggetti verrà restituito un array vuoto.