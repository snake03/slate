# Spese

Ogni Spesa viene chiamata **Arrival**, è identificate da un un'unico id.

E' possibile tramite le API elencare, visualizzare, eliminare e modificare tutte le spese. 

<a name="Invoice-associations"></a>

### Associazioni

Ogni Spesa è associata a diversi oggetti.

E' possibile vedere gli oggetti associati a ciascuna Spesa utilizzando il parametro `expand` in querystring e indicando i nomi degli oggetti da mostrare separati da una virgola.

Es: `?expand=Income,Customer`

Oggetto | Ritorna | Description
--------- | ---| --------
ArrivalProduct | Array di [ArrivalProduct](#ArrivalProduct) | Voci della spesa
Outflow | Array di [Outflow](#Outflow) | Tranches di Pagamento. I pagamenti effettuati per questa spesa.
OutflowDue | Array di [OutflowDue](#OutflowDue)| Scadenze di Pagamento. Una o più date con relativi importi da pagare.
Supplier | Oggetto [Contact](#Contact) | Il fornitore di questa spesa selezionato tra i diversi contatti.
Category | Array di [Category](#Category) | Le diverse categorie a cui appartiene questa spesa
Upload | Array di [Upload](#Upload) | Uno o più file allegati a questa spesa


## Oggetto Spesa

> Risposta di Esempio:

```json
{
	"Arrival": {
            "id": 48,
            "number": "4",
            "supplier_id": null,
            "customer_id": null,
            "supplier_name": null,
            "created": "2015-05-16 14:45:52",
            "updated": "2015-05-30 14:32:09",
            "date": "2015-05-30",
            "payment_method": "",
            "payment_dues": "",
            "reference": null,
            "notes": "",
            "currency": "EUR",
            "causal": null,
            "total_amount": 341.6,
            "total_paid": 0,
            "vat_amount": 61.6,
            "net_amount": 280,
            "contribution_amount": 0,
            "withholding_amount": 0,
            "withholding_rate": 0,
            "withholding_on": 0,
            "contribution_rate": 0,
            "contribution_text": null,
            "contribution_withholding": null
	}
}
```
L'oggetto Spesa possiede diverse attributi al suo interno. 

Alcuni di questi sono passati dall'utente quando crea o modifica una spesa, altri invece vengono calcolati automaticamente appena la spesa viene salvata e sono quindi di sola lettura.

### Attributi

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)|L'identificativo della spesa
created | Datetime (*read-only*) | Quando l'oggetto è stato creato
updated | Datetime (*read-only*) | Quando l'oggetto è stato modificato
number | String | Identificativo della spesa
date | Date | Data della spesa
supplier_id | Integer | Identificativo del fornitore  (tra i Contatti)
supplier_name | String | Nome del fornitore
payment_method | String | Metodo di pagamento
payment_dues | String | Scadenze di pagamento
reference | String | Il motivo dell'acquisto
notes | String | Note della spesa
currency | String | Valuta. Solo EUR supportata
net_amount | Float (*read-only*) | Imponibile
vat_amount | Float (*read-only*) | Importo IVA
total_amount | Float (*read-only*) | Importo Totale
total_paid | Float (*read-only*) | Importo pagato.

Esistono anche altri attributi specifici per esigenze contabili, da considerare in casi particolari.

Attributo | Tipo | Descrizione
----- | ------- | --------
withholding_amount | Float (*read-only*) | Importo Ritenuta d'acconto.
contribution_amount | Float (*read-only*) | Importo contributo previdenziale o maggiorazione.
contribution_rate | Float | Rata del contributo previdenziale (es: 30% di imponibile)
contribution_text | String | Descrizione del contributo previdenziale.
contribution_withholding | Boolean. Default *false* | Contributi previdenziali si calcolano anche sulla ritenuta d'acconto?.
withholding_rate | Float | Rata della ritenuta d'acconto
withholding_on | Float | Percentuale dell'imponibile su cui si applica la ritenuta d'acconto.
causal | String | Causale di carico magazzino


## Voci della Spesa

> Risposta di Esempio:

```json
{
	"ArrivalProduct": {
	   "id": 96,
      "arrival_id": 48,
      "product_id": null,
      "internal_code": null,
      "supplier_code": null,
      "name": "Servizio Marketing",
      "qty": 1,
      "catalog": 0,
      "supplier_discount": null,
      "cost_ev": 280,
      "vat": 0.22,
      "cost_iv": 341.6,      
      "tax_id": null,
      "order_preference": null,
      "use_stock": null,
      "measure_unit": null,
      "withholding": null,
      "non_taxable": false
	}
}
```

Ogni spesa porta con sé diverse voci, chiamate *ArrivalProduct*.

Queste sono il corpo del documento, e grazie a loro viene calcolato il totale della spesa.

L'oggetto *ArrivalProduct* viene sempre restituito quando viene richiesta una Spesa, anche se non viene fatta richiesta diretta tramite `expand`.

### Attributi

Attributo | Tipo | Descrizione
------- | ----- | --------
id | Integer | ID della voce della spesa
arrival_id | Integer | Id della Spesa
product_id | Integer | Id del [Prodotto](#Product) associato a questa riga
internal_code | String | Codice della voce
supplier_code | String | Codice della voce assegnato dal fornitore
name | String | Nome della voce
qty | Float | Quantità
measure_unit | String | Unità di misura
supplier_discount | String | Sconto applicato dal fornitore
catalog | Float | Prezzo di acquisto senza sconti
cost_ev | Float | Prezzo di acquisto IVA esclusa
vat | Float | Percentuale di IVA
cost_iv | Float | Prezzo di acquisto Iva compresa
supplier_vat | Float | IVA d'acquisto
tax_id | Integer | id della [Tassa](#Tax)
withholding | Float | Si applica la ritenuta d'acconto a questo prodotto?
non_taxable | Boolean | Il prodotto è non imponibile?
discount | String | Sconto applicato al cliente
order_preference | Integer | Ordine delle righe in spesa (ascendente).
descriptive | Boolean. Default *false* | E' una riga solo descrittiva?
use_stock | Boolean. Default *false* | Bisogna aggiornare il magazzino?

### Come vengono calcolati i totali

Quando una spesa viene salvata, in fase di creazione o modifica, i totali al suo interno vengono calcolati in base agli oggetti **ArrivalProduct** associati e agli attributi della spesa.

Per ogni **ArrivalProduct** viene prima calcolato il **cost_ev** come **catalog** * (1- **discount**). O, se fornito direttamente il **cost_ev** viene utilizzato quest'ultimo. Se non viene fornito un **catalog** né un **cost_ev**, quest'ultimo viene calcolato automaticamente come **cost_iv**/(1+**vat**).

Ottenuto il **cost_ev**, questo viene moltiplicato per la quantità per ottenere l'imponibile (**net_amount** in **Arrival**). Se il prodotto possiede l'attributo **non_taxable** pari a *true* questo non contribuirà all'incremento dell'imponibile.

E' possibile, se richiesto, calcolare anche Contributi Previdenziali e Ritenute d'acconto per ogni spesa.

L'importo totale (**total_amount**) corrisponderà alla somma dell'imponibile e del totale IVA (**vat_amount**). Qualora esistano ritenute d'acconto o contributi previdenziali, il totale terrà conto anche di questi.

#### Ritenuta d'acconto

Se vogliamo calcolare la ritenuta d'acconto, ad esempio 10% del 50% dell'imponibile, è necessario indicare per l'**Arrival** i campi **withholding_rate** a 0.1, withholding_on a 0.5, e per ogni **ArrivalProduct** assegnare **withholding** pari a *true* per tutte le righe che contribuiscono alla ritenuta.

#### Contributi previdenziali

Per inserire dei contributi previdenziali o altre maggiorazioni in spesa, possiamo inserire in **Arrival** i campi **contribution_rate** (ad esempio 0.3, se vogliamo inserire un contributo del 30%), **contribution_text** (ad esempio: "Cassa Previdenziale"), e **contribution_withholding** pari a *true* se la ritenuta d'acconto deve venire calcolata sull'imponibile maggiorato del contributo, *false* altrimenti.


## Crea una spesa

> Richiesta di Esempio:

```http
POST /api/arrivals/?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/arrivals/
	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Corpo della richiesta:

```json
{
    "Arrival" : {
        "supplier_id" : 714
    },
    "ArrivalProduct":
    [
        {
            "name" : "Servizio acquistato 1",
            "qty" : 1,
            "cost_ev" : 200,
            "vat" : 0.22
        },
             {
            "name" : "Servizio acquistato 2",
            "qty" : 2,
            "cost_iv" : 244,
            "vat" : 0.22
        }
     ]
}
```

> Risposta di esempio:

```json
{
    "Arrival": {
        "id": 579,
        "number": "5",
        "supplier_id": 714,
        "customer_id": null,
        "supplier_name": null,
        "created": "2015-06-05 13:26:46",
        "updated": "2015-06-05 13:26:46",
        "date": "2015-06-05",
        "payment_method": "",
        "payment_dues": "",
        "notes": "",
        "currency": "EUR",
        "reference": null,
        "causal": null,
        "vat_amount": 132,
        "net_amount": 600,
        "total_amount": 732,
        "total_paid": 0,
        "contribution_amount": 0,
        "withholding_amount": 0,
        "withholding_rate": 0,
        "withholding_on": 0,
        "contribution_rate": 0,
        "contribution_text": null,
        "contribution_withholding": null
    },
    "OutflowDue": [

    ],
    "ArrivalProduct": [
        {
            "id": 627,
            "arrival_id": 579,
            "order_product_id": null,
            "arrangement_product_id": null,
            "product_id": null,
            "catalog": 200,
            "supplier_discount": null,
            "cost_ev": 200,
            "vat": 0.22,
            "tax_id": null,
            "internal_code": null,
            "supplier_code": null,
            "qty": 1,
            "name": "Servizio acquistato 1",
            "order_preference": null,
            "use_stock": null,
            "measure_unit": null,
            "withholding": null,
            "non_taxable": false,
            "cost_iv": 244
        },
        {
            "id": 628,
            "arrival_id": 579,
            "order_product_id": null,
            "arrangement_product_id": null,
            "product_id": null,
            "catalog": 200,
            "supplier_discount": null,
            "cost_ev": 200,
            "vat": 0.22,
            "tax_id": null,
            "internal_code": null,
            "supplier_code": null,
            "qty": 2,
            "name": "Servizio acquistato 2",
            "order_preference": null,
            "use_stock": null,
            "measure_unit": null,
            "withholding": null,
            "non_taxable": false,
            "cost_iv": 244
        }
    ],
    "Outflow": [

    ]
}
```
E' possibile creare facilmente una spesa con pochissimi dati.

### HTTP Request

`POST /api/arrivals/`

### Corpo della richiesta

E' necessario inviare un oggetto **Arrival** e, facoltativamente, i suoi oggetti associati.
Tutti i campi sono *opzionali*.
E' bene notare tra gli attributi dell'oggetto *Arrival*

Argomento | Descrizione
------- |   -----
supplier_id | Id del fornitore tra i [Contatti](#contact). Se non esiste un contatto con questo id l'attributo verrà settato a *null*. Se esiste, verranno prelevati i dati del [Contatto](#contact) per gli attributi descrittivi **supplier_name**.
number | Se non presente nella richiesta verrà settato a *null*. 
date | Se non indicata viene automatiacmente inserita la data odierna del momento della chiamata. 

Insieme all'oggetto **Arrival** può venire inviato nella richiesta un array di oggetti **ArrivalProduct**. Anche in questo caso tutti gli attributi sono opzionali, ma è bene notare che:

Argomento | Descrizione
------- |   -----
qty | Se assente, viene settata a 0. Questa riga dunque non contribuirà al totale in spesa.
catalog | Se assente, viene automaticamente calcolato come **cost_ev**/(1+**discount**).
cost_iv | Se assente, viene calcolato come **cost_ev** * (1+**vat**).

Insieme all'oggetto **Arrival** può venire inviato nella richiesta un array di oggetti [**Outflow**](#Outflow), [**OutflowDue**](#OutflowDue), [**Category**](#Category), [**Upload**](#Upload). 

### Restituisce 

In caso di sucesso viene restituito un oggetto comprendente al suo interno l'oggetto **Arrival** appena salvato, gli associati in forma di array **ArrivalProduct**, **Outflow**, **OutflowDue**.

In caso di errore viene restituito un [Errore](#Error).


## Ottieni una Spesa

> Richiesta di Esempio:

```http
GET /api/arrivals/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/arrivals/{id}
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta di esempio:

```json
{
    "Arrival": {
        "id": 579,
        "number": "5",
        "supplier_id": 714,
        "supplier_name": null,
        "created": "2015-06-05 13:26:46",
        "updated": "2015-06-05 13:26:46",
        "date": "2015-06-05",
        "payment_method": "",
        "payment_dues": "",
        "notes": "",
        "currency": "EUR",
        "reference": null,
        "causal": null,
        "vat_amount": 132,
        "net_amount": 600,
        "total_amount": 732,
        "total_paid": 0,        
        "contribution_amount": 0,
        "withholding_amount": 0,
        "withholding_rate": 0,
        "withholding_on": 0,
        "contribution_rate": 0,
        "contribution_text": null,
        "contribution_withholding": null
    },
    "OutflowDue": [

    ],
    "ArrivalProduct": [
        {
            "id": 627,
            "arrival_id": 579,
            "order_product_id": null,
            "arrangement_product_id": null,
            "product_id": null,
            "catalog": 200,
            "supplier_discount": null,
            "cost_ev": 200,
            "vat": 0.22,
            "tax_id": null,
            "internal_code": null,
            "supplier_code": null,
            "qty": 1,
            "name": "Servizio acquistato 1",
            "order_preference": null,
            "use_stock": null,
            "measure_unit": null,
            "withholding": null,
            "non_taxable": false,
            "cost_iv": 244
        },
        {
            "id": 628,
            "arrival_id": 579,
            "order_product_id": null,
            "arrangement_product_id": null,
            "product_id": null,
            "catalog": 200,
            "supplier_discount": null,
            "cost_ev": 200,
            "vat": 0.22,
            "tax_id": null,
            "internal_code": null,
            "supplier_code": null,
            "qty": 2,
            "name": "Servizio acquistato 2",
            "order_preference": null,
            "use_stock": null,
            "measure_unit": null,
            "withholding": null,
            "non_taxable": false,
            "cost_iv": 244
        }
    ],
    "Outflow": [

    ],
    "Upload": [

    ]
}
```

Recupera una determinata spesa dato il suo ID.

### HTTP Request

`GET /api/invoices/{id}`

### Argomenti

Argomento |  Tipo  | Default | Descrizione
------- | ----- | ---| -----
id | Integer  | *Obbligatorio* | Identificativo della spesa

### Restituisce

In caso di successo, viene restituito un array contente un oggetto **Arrival**, i diversi **ArrivalProduct** associati  e tutti gli oggetti associati.

In caso di errore o se l'id fornito non è valido viene invece restituito un [Errore](#Error).

Ogni *Arrival* ha al suo interno diversi totali (**net_amount**, **total_amount**, **total_paid**, ...). 

Il **total_amount** rappresenta il totale della spesa, il **net_amount** l'imponibile, il **vat_amount** la parte di IVA. 
A questi si aggiungono **contributon_amount**, indicante eventuali contributi previdenziali e **withholding_amount** che indica la ritenuta d'acconto. 
Tutti questi campi vengono calcolati al momento del salvataggio della Spesa (sia quando la si crea che quando la si aggiorna) in base agli **ArrivalProduct** associati. 

Il campo **total_paid** indica quanto è stato pagato per quella spesa. E' un campo *read-only* e viene calcolato ogni volta che una Spesa viene salvata (sia quando la si crea che quando la si aggiorna) in base agli oggetti [*Outflow*](#outflow) associati



## Modifica una Spesa

> Richiesta di Esempio:

```http
PUT /api/arrivals/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/invoices/{id}
	-X PUT
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Corpo della richiesta: 

```json
{
    "Arrival" : {
        "date" : "2015-04-01"
    },
    "ArrivalProduct":[
        {
            "name" : "Servizio numero 1",
            "qty" : 1,
            "cost_ev" : 200,
            "vat" : 0.22
        },
        {
            "name" : "Prodotto numero 1",
            "qty" : 10,
            "cost_iv" : 1000,
            "vat" : 0.22,
            "supplier_discount" : "0.1"
        }
    ],
    "OutflowDue": [
	    {
			"due_date": "2015-05-01",
			"amount": 300,
			"description": "Scadenza"
	    }
    ]
}
```

> Risposta di esempio:

```json
{
    "Arrival": {
        "id": 579,
        "number": "5",
        "supplier_id": 714,
        "customer_id": null,
        "supplier_name": null,
        "created": "2015-06-05 13:26:46",
        "updated": "2015-06-05 13:53:00",
        "date": "2015-04-01",
        "payment_method": "",
        "payment_dues": "",
        "notes": "",
        "currency": "EUR",
        "reference": null,
        "causal": null,
        "total_amount": 10244,
        "total_paid": 0,
        "vat_amount": 1847.279,
        "net_amount": 8396.721,
        "contribution_amount": 0,
        "withholding_amount": 0,
        "withholding_rate": 0,
        "withholding_on": 0,
        "contribution_rate": 0,
        "contribution_text": null,
        "contribution_withholding": null
    },
    "OutflowDue": [
        {
            "id": 12,
            "due_date": "2015-05-01",
            "amount": 300,
            "description": "Scadenza",
            "arrival_id": 579,
            "created": "2015-06-05 13:53:00",
            "updated": "2015-06-05 13:53:00",
        }
    ],
    "ArrivalProduct": [
        {
            "id": 631,
            "arrival_id": 579,
            "order_product_id": null,
            "arrangement_product_id": null,
            "product_id": null,
            "catalog": 200,
            "supplier_discount": null,
            "cost_ev": 200,
            "vat": 0.22,
            "tax_id": null,
            "internal_code": null,
            "supplier_code": null,
            "qty": 1,
            "name": "Servizio numero 1",
            "order_preference": null,
            "use_stock": null,
            "measure_unit": null,
            "withholding": null,
            "non_taxable": false,
            "cost_iv": 244
        },
        {
            "id": 632,
            "arrival_id": 579,
            "order_product_id": null,
            "arrangement_product_id": null,
            "product_id": null,
            "catalog": 910.747,
            "supplier_discount": "0.1",
            "cost_ev": 819.672,
            "vat": 0.22,
            "tax_id": null,
            "internal_code": null,
            "supplier_code": null,
            "qty": 10,
            "name": "Prodotto numero 1",
            "order_preference": null,
            "use_stock": null,
            "measure_unit": null,
            "withholding": null,
            "non_taxable": false,
            "cost_iv": 1000
        }
    ],
    "Outflow": [

    ],
    "Upload": [

    ]
}
```

Quando una spesa viene creata, è possibile modificarla in un secondo momento fornendo l'id di quella spesa.

### HTTP Request

`PUT /api/arrivals/{id}`

Tutti i dati passati nel corpo della richiesta verranno elaborati per modificare l'oggetto **Arrival** e le sue associazioni.

### Cosa inviare 

Tutti i campi dell'oggetto **Arrival** che vengono inviati nel corpo della richiesta sovrascriveranno quelli già memorizzati per la spesa. Per i campi non inviati verranno invece conservati i valori già memorizzati. Non è quindi necessario inviare tutti i campi dell'**Arrival** ma solamente quelli che si intende memorizzare.

Per modificare una degli oggetti associati **ArrivalProduct**, **Outflow**, **OutflowDue**, **Category** o **Upload** è necessario inviare un array di questi oggetti. 

Si noti che inviando un array di **ArrivalProduct** o di **OutflowDue**, tutti gli oggetti associati precedentemente memorizzati verranno eliminati.

Ad esempio, se una spesa possiede due voci, inviando un nuovo array di **ArrivalProduct** queste due voci verranno eliminate e verranno memorizzate quelle nuove appena inviate, e quindi calcolati i nuovi totali.

Se si vuole mantenere i vecchi oggetti associati è necessario inviare nell'array gli id dei vecchi oggetti che si vuole conservare. Per aggiungere un prodotto ad una spesa esistente, ad esempio, si può inviare al server gli id delle due voci preesitenti più la nuova riga.

> Richiesta di aggiunta di una nuova voce

```json
{
 "ArrivalProduct": [
		{
		  "id": 11663
		},
		{
		  "id": 11664
		},
		{
		  "name": "Nuova Voce",
		  "qty": 1,
		  "cost_ev": 100
		}
   ]
}
```

### Restituisce

In caso di successo viene restituito un oggetto contenente un oggetto **Arrival** e gli oggetti associati.

In caso di errore viene restituito un [Errore](#Error).

### Esempio. Pagamento di una fattura

Se una spesa viene pagata, è possibile segnalare il pagamento in due modi:

- Creare un nuovo oggetto [**Outflow**](#Outflow-add) indicando come *arrival_id* l'id della spesa di riferimento.
- Inviare una richiesta a `PUT /api/arrivals/{id}`, fornendo un array di oggetti **Outflow** da creare.

In entrambi i casi il campo *total_paid* della spesa verrà aggiornato di conseguenza. Verranno cioè presi in considerazione i diversi **Outflow** associati alla spesa (quello appena creato e eventuali già esistenti) sommando gli importi.


## Elimina una Spesa


> Richiesta di esempio:

```http
DELETE /api/arrivals/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/arrivals/{id}
	-X DELETE
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta:

```json
{
    "code": "200",
    "message": "Documento Eliminato Correttamente"
}
```

E' possibile eliminare una spesa indicando il suo id.

Eliminando una spesa verranno eliminati allo stesso tempo tutti gli oggetti **ArrivalProduct** e **OutflowDue** associati.

Sopravviveranno invece gli oggetti **Outflow**, **Upload** e **Category** dovranno essere eliminati con altre richieste.

### HTTP Request

`DELETE /api/arrivals/{access_token}`

### Restituisce

Restituisce un oggetto indicante **code** 200 in caso di successo, un [Errore](#Error) altrimenti.


## Ottieni tutte le Spese

> Richiesta di Esempio:

```http
GET /api/arrivals/?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/arrivals/
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta di Esempio:

```json
[
	{
	    "Arrival": {
	        "id": 579,
	        "number": "5",
	        "supplier_id": 714,
	        "customer_id": null,
	        "supplier_name": null,
	        "created": "2015-06-05 13:26:46",
	        "updated": "2015-06-05 13:53:00",
	        "date": "2015-04-01",
	        "payment_method": "",
	        "payment_dues": "",
	        "notes": "",
	        "currency": "EUR",
	        "reference": null,
	        "causal": null,
	        "total_amount": 10244,
	        "total_paid": 0,
	        "vat_amount": 1847.279,
	        "net_amount": 8396.721,
	        "contribution_amount": 0,
	        "withholding_amount": 0,
	        "withholding_rate": 0,
	        "withholding_on": 0,
	        "contribution_rate": 0,
	        "contribution_text": null,
	        "contribution_withholding": null
	    },
	    "OutflowDue": [
	        {
	            "id": 12,
	            "due_date": "2015-05-01",
	            "amount": 300,
	            "description": "Scadenza",
	            "arrival_id": 579,
	            "created": "2015-06-05 13:53:00",
	            "updated": "2015-06-05 13:53:00"
	        }
	    ],
	    "ArrivalProduct": [
	        {
	            "id": 631,
	            "arrival_id": 579,
	            "order_product_id": null,
	            "arrangement_product_id": null,
	            "product_id": null,
	            "catalog": 200,
	            "supplier_discount": null,
	            "cost_ev": 200,
	            "vat": 0.22,
	            "tax_id": null,
	            "internal_code": null,
	            "supplier_code": null,
	            "qty": 1,
	            "name": "Servizio numero 1",
	            "order_preference": null,
	            "use_stock": null,
	            "measure_unit": null,
	            "withholding": null,
	            "non_taxable": false,
	            "cost_iv": 244
	        },
	        {
	            "id": 632,
	            "arrival_id": 579,
	            "order_product_id": null,
	            "arrangement_product_id": null,
	            "product_id": null,
	            "catalog": 910.747,
	            "supplier_discount": "0.1",
	            "cost_ev": 819.672,
	            "vat": 0.22,
	            "tax_id": null,
	            "internal_code": null,
	            "supplier_code": null,
	            "qty": 10,
	            "name": "Prodotto numero 1",
	            "order_preference": null,
	            "use_stock": null,
	            "measure_unit": null,
	            "withholding": null,
	            "non_taxable": false,
	            "cost_iv": 1000
	        }
	    ],
	    "Outflow": [

	    ],
	    "Upload": [
			{
		      "id": 178,
		      "real_name": "254\/948df65482fcfc4a48d7c0521.jpg",
		      "name": "10171627_10152908373599664_2161444716751927217_n.j",
		      "size": 81491,
		      "created": "2015-05-30 14:13:22",
		      "modified": "2015-05-30 14:32:09",
		      "mime": "image\/jpeg",
		      "docket_id": null,
		      "revision_of": null,
		      "updated_by": 153,
		      "public": false
	    	}
	    ]
	},
	{
		"Arrival": {...},
		"ArrivalProduct": [{...}],
		"Outflow": [{...}],
		"OutflowDue": [{...}],
		"Category": [{...}],
		"Upload":[{...}]
	}
]
```

Questo endpoint elenca tutte le spese precedentemente create.

### HTTP Request

`GET /api/arrivals/`

### Parametri Query

E' possibile filtrare e ordinare la richiesta indicato alcuni parametri in *querystring*

Argomento |  Tipo  | Default | Descrizione
------- | ----- | ---| -----
from | Date  | 30 giorni fa| Spese con data posteriore a una certa data 
to | Date  | oggi | Spese con data anteriore a una certa data 
order | String *asc* or *desc* | desc |Dalla più recente alla più vecchia (*desc*) o viceversa 

### Restituisce

In caso di successo viene restituito un array di oggetti. 

Ogni oggetto contiene al suo interno un **Arrival** e le sue associazioni.

In caso non ci sia nessuna Spesa da mostrare viene restituito un array vuoto.


<a name="OutflowDue"></a>
## Scadenze di Pagamento

```json
{
	"OutflowDue": 
	{
		"id": 36,
		"due_date": "2015-04-01",
		"amount": 244,
		"description": null,
		"invoice_id": 1758,
		"created": "2015-05-18 13:54:31",
		"updated": "2015-05-18 13:55:55"
	}
}
```

Le scadenze di pagamento di una fattura sono descritte dall'oggetto **OutflowDue**.

Sono l'opposto dell'oggetto [**IncomeDue**](#OutflowDue).

### Attributi

Attributo | Tipo |Descrizione
-------  | ---| -----
id | Integer (*read-only*)| ID della scadenza di pagamento. 
due_date | Date | Data della scadenza di pagamento. 
amount | Float | Importo di questa scadenza. 
arrival_id | Integer (*read-only*) | ID della spesa a cui appartiene.  


<a name="Category"></a>

## Categoria

```json
{
	"Category" :
	{
       "id": 40,
       "arrival_id": 48,
       "category": "Spese per il personale"
   	}
}
```

Ogni **Arrival** può avere diverse categorie, chiamate **Category**.

### Attributi

Attributo | Tipo |Descrizione
-------  | ---| -----
id | Integer (*read-only*)| ID della categoria. 
arrival_id | Integer | Id della spesa. 
category | String | Testo della categoria.
