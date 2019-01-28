# Fatture

Ogni Fattura, chiamata **Invoice**, è identificata da un un'unico ID.

E' possibile tramite le API elencare, visualizzare, eliminare e modificare una fattura. E' possibile anche visualizzarla in pdf e inviarla via email.

<a name="Invoice-associations"></a>
### Associazioni

Ogni Fattura è associata a diversi oggetti.

E' possibile vedere gli oggetti associati a ciascuna Fattura utilizzando il parametro `expand` in querystring e indicando i nomi degli oggetti da mostrare separati da una virgola.

Es: `?expand=Income,Customer`

Oggetto | Ritorna | Description
--------- | ---| --------
InvoiceProduct | Array di [InvoiceProduct](#InvoiceProduct) | Righe della fattura
Income | Array di [Income](#Income) | Tranches di Pagamento. I pagamenti ricevuti per questa fattura.
IncomeDue | Array di [IncomeDue](#IncomeDue)| Scadenze di Pagamento. Una o più date con relativi importi da pagare.
Customer | Oggetto [Contact](#Contact) | Il cliente di questa fattura selezionato tra i diversi contatti.
ShippingDetail | Oggetto [ShippingDetail](#ShippingDetail) | Dettagli di Spedizione se è una Fattura d'accompagnamento



## Oggetto Fattura

> Risposta di Esempio:

```json
{
	"Invoice": {
        "id": 1758,
        "created": "2015-05-16 09:52:30",
        "updated": "2015-06-01 09:44:34",
        "number": "2",
        "date": "2015-05-15",
        "customer_id": 714,
        "customer_name": "Mario Rossi",
        "customer_address": "Via Roma, Torino",
        "customer_address_shipment": "",
        "customer_fiscal": "RSSMR",
        "customer_vat_number": "",
        "payment_method": "",
        "payment_dues": "pers",
        "payment_notes": "",
        "notes": "",
        "currency": "EUR",
        "view_preferences": null,
        "view_preference_id": 62,
        "seen": null,
        "updated_by": 153,
        "net_amount": 200,
        "vat_amount": 44,
        "total_amount": 244,
        "total_paid": 488,        
        "contribution_amount": 0,
        "withholding_amount": 0,
        "withholding_rate": 0,
        "withholding_on": 0,
        "contribution_text": "",
        "contribution_rate": 0,
        "contribution_withholding": false
	}
}
```
L'oggetto fattura possiede diverse attributi al suo interno. 

Alcuni di questi sono passati dall'utente quando crea o modifica una fattura, altri invece vengono calcolati automaticamente appena la fattura viene salvata e sono quindi di sola lettura.

### Attributi

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)|L'identificativo della fattura
created | Datetime (*read-only*) | Quando l'oggetto è stato creato
updated | Datetime (*read-only*) | Quando l'oggetto è stato modificato
number | String | Identificativo (progressivo) della fattura
date | Date | Data della fattura
customer_id | Integer | Identificativo del cliente  (tra i Contatti)
customer_name | String | Nome del cliente
customer_street | String | Via, Indirizzo del cliente
customer_city | String | Città, Indirizzo del cliente
customer_province | String | Provincia, indirizzo del cliente, in [sigla a due caratteri](http://www.aci.it/i-servizi/normative/codice-della-strada/elenco-sigle-province-ditalia.html)
customer_zip | String | CAP, indirizzo del cliente
customer_country | String | Stato, indirizzo del cliente [in formato Alpha2](https://it.wikipedia.org/wiki/ISO_3166-1_alpha-2)
customer_address_shipment | String | Indirizzo di spedizione del cliente
customer_fiscal | String | Codice Fiscale del cliente
customer_vat_number| String | Partita IVA del cliente
customer_pec | String | Indirizzo di posta PEC del cliente
customer_fe_code | String | Codice Destinatario (per Fatturazione Elettronica) del cliente
payment_method | String | Metodo di pagamento
payment_dues | String | Scadenze di pagamento
payment_notes | String | Note di Pagamento
notes | String | Note della fattura
currency | String | Valuta. Solo EUR supportata
seen | Datetime (*read-only*)| Quando il cliente ha visualizzato la fattura
net_amount | Float (*read-only*) | Imponibile
vat_amount | Float (*read-only*) | Importo IVA
total_amount | Float (*read-only*) | Importo Totale in fattura
total_paid | Float (*read-only*) | Importo pagato.

Esistono inoltre alcuni attributi riguardanti dati contabili, da considerare solamente quando si hanno esigenze contabili particolari (es: ritenute d'acconto, cassa previdenziale, ecc...).

Attributo | Tipo | Descrizione
----- | ------- | --------
withholding_amount | Float (*read-only*) | Importo Ritenuta d'acconto.
contribution_amount | Float (*read-only*) | Importo dell'eventuale contributo previdenziale o maggiorazione.
contribution_rate | Float | Rata dell'eventuale contributo previdenziale (es: 30% di imponibile)
contribution_text | String | Descrizione del contributo previdenziale (es: "Cassa Ingegneri").
contribution_withholding | Boolean | Contributi previdenziali si calcolano anche sulla ritenuta d'acconto?
withholding_rate | Float | Rata della ritenuta d'acconto
withholding_on | Boolean | Percentuale dell'imponibile su cui si applica la ritenuta d'acconto.


## Righe della Fattura

> Risposta di Esempio:

```json
{
	"InvoiceProduct": {
		"id": 11653,
		"invoice_id": 1771,
		"product_id": 1168,
		"name": "Riga di prova",
		"internal_code": null,
		"note": null,
		"qty": 1,
		"measure_unit": "pz",
		"catalog": 0,
		"cost_ev": 0,
		"cost_iv": 0,
		"price_ev": 200,
		"net_price": 200,
		"vat": 0.22,
		"price_iv": 244,
		"supplier_id": null,
		"supplier_code": null,
		"supplier_discount": "",
		"supplier_vat": 0,
		"markup": 0,
		"margin": 0,
		"tax_id": null,
		"withholding": 0,
		"non_taxable": false,
		"discount": "",
		"order_preference": 0,
		"descriptive": null,
		"use_stock": false
	}
}
```

Ogni fattura porta con sé diverse righe, chiamate *InvoiceProduct*.

Queste sono il corpo del documento, e grazie a loro viene calcolato il totale in fattura.

L'oggetto *InvoiceProduct* viene sempre restituito quando viene richiesta una Fattura, anche se non viene fatta richiesta diretta tramite `expand`.

### Attributi

Attributo | Tipo | Descrizione
------- | ----- | --------
id | Integer |The ID della riga della fattura
invoice_id | Integer | Id della fattura
product_id | Integer | Id del [Prodotto](#Product) associato a questa riga
name | String | Nome della riga
internal_code | String | Codice della riga
note | String | Note e altre descrizioni
qty | Float | Quantità
measure_unit | String | Unità di misura
price_ev | Float | Prezzo di vendita IVA esclusa
net_price | Float (*read-only*) | Prezzo di vendita IVA esclusa al netto di sconti
vat | Float | Percentuale di IVA
price_iv | Float | Prezzo di vendita IVA compresa
supplier_id | Integer | id del Fornitore ([Contatto](#Contact))
tax_id | Integer | id della [Tassa](#Tax)
withholding | Float | Si applica la ritenuta d'acconto a questo prodotto?
non_taxable | Boolean. Default *false* | Il prodotto è non imponibile?
discount | String | Sconto applicato al cliente
order_preference | Integer | Ordine delle righe in fattura (ascendente).
descriptive | Boolean | E' una riga solo descrittiva?
use_stock | Boolean | Bisogna aggiornare il magazzino?

Tra gli attributi da considerare come *avanzati*, cioè di cui solo in rare occasioni si ha bisogno, ma che può essere talvolta importante tracciare, vediamo:

Attributo | Tipo | Descrizione
------- | ----- | --------
markup | Float | Markup
margin | Float | Margine di vendita
catalog | Float | Prezzo di acquisto senza sconti
cost_ev | Float | Prezzo di acquisto Iva esclusa
cost_iv | Float | Prezzo di acquisto Iva compresa
supplier_code | String | codice assegnato dal fornitore
supplier_discount | String | Sconto applicato dal fornitore
supplier_vat | Float | IVA d'acquisto

Nota che l'attributo *discount* è pari a String. Questo perché è possibile indicare diversi sconti cumulativi. Ad esempio per indicare un sconto del 50%+10%, si può utilizzare per il campo *discount* "0.5 0.1", separando i diversi sconti con un spazio.

### Come vengono calcolati i totali

Quando una fattura viene salvata, in fase di creazione o modifica, i totali al suo interno vengono calcolati in base agli oggetti **InvoiceProduct** associati e agli attributi della fattura.

Per ogni **InvoiceProduct** viene prima calcolato il **net_price** come **price_ev** * (1- **discount**). Se non viene fornito un **price_ev**, quest'ultimo viene calcolato automaticamente come **price_iv**/(1+**vat**).

Ottenuto il **net_price**, questo viene moltiplicato per la quantità per ottenere l'imponibile (**net_amount** in **Invoice**). Se il prodotto possiede l'attributo **non_taxable** pari a *true* questo non contribuirà all'incremento dell'imponibile.

E' possibile, se richiesto, calcolare anche Contributi Previdenziali e Ritenute d'acconto per ogni fattura.

L'importo totale (**total_amount**) corrisponderà alla somma dell'imponibile e del totale IVA (**vat_amount**). Qualora esistano ritenute d'acconto o contributi previdenziali, il totale terrà conto anche di questi.

#### Ritenuta d'acconto

Se vogliamo calcolare la ritenuta d'acconto, ad esempio 10% del 50% dell'imponibile, è necessario indicare per l'**Invoice** i campi *withholding_rate* a 0.1, *withholding_on* a 0.5, e per ogni **InvoiceProduct** assegnare **withholding** pari a *true* per tutte le righe che contribuiscono alla ritenuta.

#### Contributi previdenziali

Per inserire dei contributi previdenziali o altre maggiorazioni in fattura, possiamo inserire in **Invoice** i campi **contribution_rate** (ad esempio 0.3, se vogliamo inserire un contributo del 30%), **contribution_text** (ad esempio: "Cassa Previdenziale"), e **contribution_withholding** pari a *true* se la ritenuta d'acconto deve venire calcolata sull'imponibile maggiorato del contributo, *false* altrimenti.



## Crea una fattura

> Richiesta di Esempio:

```http
POST /api/invoices/?expand=Income,IncomeDue&access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/invoices/?expand=Income,IncomeDue
	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Corpo della richiesta:

```json
{
    "Invoice" : {
        "customer_id" : 714
    },
    "InvoiceProduct":
    [
        {
            "name" : "Servizio numero 1",
            "qty" : 1,
            "price_ev" : 200,
            "vat" : 0.22
        },
             {
            "name" : "Servizio numero 2",
            "qty" : 2,
            "price_iv" : 244,
            "vat" : 0.22
        }
     ]
}
```

> Risposta di esempio:

```json
{
    "Invoice": {
        "id": 1787,
        "customer_id": 714,
        "created": "2015-06-02 14:58:03",
        "updated": "2015-06-02 14:58:03",
        "number": "21",
        "date": "2015-06-02",
        "customer_name": "Mario Rossi",
        "customer_street": "Via di Prova 1",
        "customer_city": "Roma",
        "customer_province": "RM",
        "customer_zip": "10100",
        "customer_country": "IT",
        "customer_address_shipment": "",
        "customer_fiscal": "RXMR912AS912D",
        "customer_vat_number": null,
        "customer_phone": "333111111",
        "customer_email": "cliente@email.it",
        "customer_pec": "cliente@pec.it",
        "customer_fe_code": "1234567",
        "payment_method": null,
        "payment_dues": null,
        "payment_notes": null,
        "notes": null,
        "currency": "EUR",
        "advance_id": null,
        "view_preferences": null,
        "view_preference_id": 62,
        "seen": null,
        "updated_by": 153,
        "net_amount": 600,
        "vat_amount": 132,
        "total_amount": 732,
        "total_paid": 0,
        "contribution_amount": 0,
        "withholding_amount": 0,
        "withholding_rate": 0,
        "withholding_on": 0,
        "contribution_text": null,
        "contribution_rate": 0,
        "contribution_withholding": null
    },
    "InvoiceProduct": [
        {
            "id": 11661,
            "invoice_id": 1787,
            "product_id": null,
            "name": "Servizio numero 1",
            "note": null,
            "qty": 1,
            "measure_unit": null,
            "cost_ev": 0,
            "cost_iv": 0,
            "price_ev": 200,
            "net_price": 200,
            "price_iv": 244,
            "internal_code": null,
            "removed": false,
            "supplier_id": null,
            "supplier_code": null,
            "catalog": 0,
            "supplier_discount": "",
            "supplier_vat": 0,
            "markup": 0,
            "margin": 0,
            "vat": 0.22,
            "tax_id": null,
            "withholding": 0,
            "non_taxable": false,
            "discount": "",
            "order_preference": 0,
            "descriptive": null,
            "use_stock": null
        },
        {
            "id": 11662,
            "invoice_id": 1787,
            "product_id": null,
            "name": "Servizio numero 2",
            "note": null,
            "qty": 2,
            "measure_unit": null,
            "cost_ev": 0,
            "cost_iv": 0,
            "price_ev": 200,
            "net_price": 200,
            "price_iv": 244,
            "internal_code": null,
            "removed": false,
            "supplier_id": null,
            "supplier_code": null,
            "catalog": 0,
            "supplier_discount": "",
            "supplier_vat": 0,
            "markup": 0,
            "margin": 0,
            "vat": 0.22,
            "tax_id": null,
            "withholding": 0,
            "non_taxable": false,
            "discount": "",
            "order_preference": 0,
            "descriptive": null,
            "use_stock": null
        }
    ],
    "Income": [

    ],
    "IncomeDue": [

    ]
}
```
E' possibile creare facilmente una fattura con pochissimi dati.

### HTTP Request

`POST /api/invoices/`

### Corpo della richiesta

E' necessario inviare un oggetto Invoice e, facoltativamente, i suoi oggetti associati.
Tutti i campi sono *opzionali*.
E' bene notare tra gli attributi dell'oggetto *Invoice*

Argomento | Descrizione
------- |   -----
customer_id | Id del cliente tra i [Contatti](#contact). Se non esiste un contatto con questo id l'attributo verrà settato a *null*. Se esiste, verranno prelevati i dati del [Contatto](#contact) per gli attributi descrittivi **customer_name**, **customer_fiscal**, **customer_vat_number**, **customer_address**, qualora non siano già presenti nella richiesta.
number | Se non presente nella richiesta verrà automaticamente utilizzato il *progressivo* successivo all'ultima Fattura. Cioè il numero dell'ultima Fattura (in ordine di **date**) + 1. 
date | Se non indicata viene automatiacmente inserita la data odierna del momento della chiamata. 

Insieme all'oggetto **Invoice** può venire inviato nella richiesta un array di oggetti **InvoiceProduct**. Anche in questo caso tutti gli attributi sono opzionali, ma è bene notare che:

Argomento | Descrizione
------- |   -----
qty | Se assente, viene settata a 0. Questa riga dunque non contribuirà al totale in fattura.
price_ev | Se assente, viene automaticamente calcolato come **price_iv**/(1+**vat**).
price_iv | Se assente, viene calcolato come **price_ev** * (1+**vat**).

Insieme all'oggetto **Invoice** può venire inviato nella richiesta un array di oggetti [**Income**](#Income) e [**IncomeDue**](#IncomeDue), e un oggetto [**ShippingDetail**](#ShippingDetail). 

### Restituisce 

In caso di sucesso viene restituito un oggetto comprendente al suo interno l'oggetto **Invoice** appena salvato, gli associati in forma di array **InvoiceProduct**, **Income**, **IncomeDue** e un oggetto **ShippingDetail**.

In caso di errore viene restituito un [Errore](#Error).


## Ottieni una fattura

> Richiesta di Esempio:

```http
GET /api/invoices/{id}?expand=Income,IncomeDue&access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/invoices/{id}?expand=Income,IncomeDue
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta di esempio:

```json
{
		"Invoice": 
		{
			"id": 1758,
			"created": "2015-05-16 09:52:30",
			"updated": "2015-06-01 09:44:34",
			"number": "2",
			"date": "2015-05-15",
			"customer_id": 714,
			"customer_name": "Mario Rossi",
        "customer_address": "Via di Prova\r\n  Roma  \r\n ",
        "customer_street": "Via di Prova 1",
        "customer_city": "Roma",
        "customer_province": "RM",
        "customer_zip": "10100",
        "customer_country": "IT",
        "customer_address_shipment": "",
        "customer_fiscal": "RXMR912AS912D",
        "customer_vat_number": null,
        "customer_phone": "333111111",
        "customer_email": "cliente@email.it",
        "customer_pec": "cliente@pec.it",
        "customer_fe_code": "1234567",
			"payment_method": "",
			"payment_dues": "pers",
			"payment_notes": "",
			"notes": "",
			"currency": "EUR",
			"view_preferences": null,
			"view_preference_id": 62,
			"seen": null,
			"updated_by": 153,
			"net_amount": 200,
			"vat_amount": 44,
			"total_amount": 244,
			"total_paid": 488,        
			"contribution_amount": 0,
			"withholding_amount": 0,
			"withholding_rate": 0,
			"withholding_on": 0,
			"contribution_text": "",
			"contribution_rate": 0,
			"contribution_withholding": false
		},
		"InvoiceProduct": [
			{
				"id": 11653,
				"invoice_id": 1771,
				"product_id": 1168,
				"name": "",
				"note": null,
				"qty": 1,
				"measure_unit": null,
				"cost_ev": 0,
				"cost_iv": 0,
				"price_ev": 200,
				"net_price": 200,
				"price_iv": 244,
				"internal_code": null,
				"removed": false,
				"supplier_id": null,
				"supplier_code": null,
				"catalog": 0,
				"supplier_discount": "",
				"supplier_vat": 0,
				"markup": 0,
				"margin": 0,
				"vat": 0.22,
				"tax_id": null,
				"withholding": 0,
				"non_taxable": false,
				"discount": "",
				"order_preference": 0,
				"arrangement_product_id": null,
				"descriptive": null,
				"use_stock": true
			},
			{...}
		],
		"Income": [{...}],
		"IncomeDue": [{...}]
	}
```

Recupera una determinata fattura data il suo ID.

### HTTP Request

`GET /api/invoices/{id}`

### Argomenti

Argomento |  Tipo | Descrizione
------- | ----- | -----
id | Integer   | *Obbligatorio*. Identificativo della fattura

### Parametri Query

Argomento |  Tipo  | Default | Descrizione
--- | --- | --- | ----
expand | String | InvoiceProduct | Lista di oggetti [associati](#Invoice-associations) alla fattura da mostrare. Nomi separati da una virgola.

### Restituisce

In caso di successo, viene restituito un array contente un oggetto *Invoice*, i diversi *InvoiceProduct* associati  e tutti gli oggetti associati specificati nel parametro *expand* della richiesta.

In caso di errore o se l'id fornito non è valido viene invece restituito un [Errore](#Error).

Ogni *Invoice* ha al suo interno diversi totali (**net_amount**, **total_amount**, **total_paid**, ...). 

Il **total_amount** rappresenta il totale della fattura, il **net_amount** l'imponibile, il **vat_amount** la parte di IVA.
 
A questi si aggiungono gli eventuali **contributon_amount**, indicante eventuali contributi previdenziali e **withholding_amount** che indica la ritenuta d'acconto. 

Tutti questi campi vengono calcolati al momento del salvataggio della Fattura (sia quando la si crea che quando la si aggiorna) in base agli **InvoiceProduct** associati. 

Il campo **total_paid** indica quanto è stato pagato per quella fattura. E' un campo *read-only* e viene calcolato ogni volta che una Fattura viene salvata (sia quando la si crea che quando la si aggiorna) in base agli oggetti [*Income*](#income) associati.


## Modifica una fattura

> Richiesta di Esempio:

```http
PUT /api/invoices/{id}?expand=Income,IncomeDue&access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/invoices/{id}?expand=Income,IncomeDue
	-X PUT
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Corpo della richiesta: 

```json
{
    "Invoice" : {
        "date" : "2015-04-01",
        "customer_fiscal": "RXMR912AS912D"
    },
    "InvoiceProduct":[
        {
            "name" : "Servizio numero 1",
            "qty" : 1,
            "price_ev" : 200,
            "vat" : 0.22
        },
        {
            "name" : "Prodotto numero 1",
            "qty" : 10,
            "price_iv" : 1000,
            "vat" : 0.22,
            "discount" : "0.1"
        }
    ],
    "IncomeDue": [
	    {
			"due_date": "2015-05-01",
			"amount": 300,
			"description": "Scadenza acconto"
	    }
    ]
}
```

> Risposta di esempio:

```json
{
    "Invoice": {
        "id": 1787,
        "customer_id": 714,
        "created": "2015-06-02 14:58:03",
        "updated": "2015-06-02 15:56:42",
        "number": "21",
        "date": "2015-04-01",
        "customer_name": "Mario Rossi",
        "customer_address": "Via di Prova\r\n  Roma  \r\n ",
        "customer_street": "Via di Prova 1",
        "customer_city": "Roma",
        "customer_province": "RM",
        "customer_zip": "10100",
        "customer_country": "IT",
        "customer_address_shipment": "",
        "customer_fiscal": "RXMR912AS912D",
        "customer_vat_number": null,
        "customer_phone": "333111111",
        "customer_email": "cliente@email.it",
        "customer_pec": "cliente@pec.it",
        "customer_fe_code": "1234567",
        "payment_method": null,
        "payment_dues": null,
        "payment_notes": null,
        "notes": null,
        "currency": "EUR",
        "seen": null,
        "updated_by": 153,
        "view_token": null,
        "use_price": null,
        "finalized": "2015-06-02 14:58:03",
        "net_amount": 7577.0492,
        "total_amount": 9244,
        "vat_amount": 1666.951,
        "contribution_amount": 0,
        "withholding_amount": 0,
        "withholding_rate": 0,
        "withholding_on": 0,
        "contribution_text": null,
        "contribution_rate": 0,
        "contribution_withholding": null,
        "total_paid": 0
    },
    "InvoiceProduct": [
        {
            "id": 11663,
            "invoice_id": 1787,
            "product_id": null,
            "name": "Servizio numero 1",
            "note": null,
            "qty": 1,
            "measure_unit": null,
            "cost_ev": 0,
            "cost_iv": 0,
            "price_ev": 200,
            "net_price": 200,
            "price_iv": 244,
            "internal_code": null,
            "supplier_id": null,
            "supplier_code": null,
            "catalog": 0,
            "supplier_discount": "",
            "supplier_vat": 0,
            "markup": 0,
            "margin": 0,
            "vat": 0.22,
            "tax_id": null,
            "withholding": 0,
            "non_taxable": false,
            "discount": "",
            "order_preference": 0,
            "descriptive": null,
            "use_stock": null
        },
        {
            "id": 11664,
            "invoice_id": 1787,
            "product_id": null,
            "name": "Prodotto numero 1",
            "note": null,
            "qty": 10,
            "measure_unit": null,
            "cost_ev": 0,
            "cost_iv": 0,
            "price_ev": 819.672131148,
            "net_price": 737.704918033,
            "price_iv": 1000,
            "internal_code": null,
            "supplier_id": null,
            "supplier_code": null,
            "catalog": 0,
            "supplier_discount": "",
            "supplier_vat": 0,
            "markup": 0,
            "margin": 0,
            "vat": 0.22,
            "tax_id": null,
            "withholding": 0,
            "non_taxable": false,
            "discount": "0.1",
            "order_preference": 0,
            "descriptive": null,
            "use_stock": null
        }
    ],
    "Income": [

    ],
    "IncomeDue": [
        {
            "id": 40,
            "due_date": "2015-05-01",
            "amount": 300,
            "description": "Scadenza acconto",
            "invoice_id": 1787,
            "domain_id": 254,
            "created": "2015-06-02 15:56:42",
            "updated": "2015-06-02 15:56:42"
        }
    ]
}
```

Quando una fattura viene creata, è possibile modificarla in un secondo momento fornendo l'id di quella fattura.

### HTTP Request

`PUT /api/invoices/{id}`

Tutti i dati passati nel corpo della richiesta verranno elaborati per modificare l'oggetto **Invoice** e le sue associazioni.

### Cosa inviare 

Tutti i campi dell'oggetto **Invoice** che vengono inviati nel corpo della richiesta sovrascriveranno quelli già memorizzati per la fattura. Per i campi non inviati verranno invece conservati i valori già memorizzati. Non è quindi necessario inviare tutti i campi dell'**Invoice** ma solamente quelli che si intende cambiare.

Per modificare una degli oggetti associati **InvoiceProduct**, **Income** o **IncomeDue** è necessario inviare un array di questi oggetti. 

Si noti che inviando un array di nuovi oggetti **InvoiceProduct** o **IncomeDue**, tutti gli oggetti di questo tipo già precedentemente memorizzati verranno eliminati.

Ad esempio, se una fattura possiede due righe, inviando un nuovo array di **InvoiceProduct** queste due righe verranno eliminate e verranno memorizzate quelle nuove appena inviate, e quindi calcolati i nuovi totali.

Se si vuole mantenere i vecchi oggetti associati è necessario inviare nell'array gli id dei vecchi oggetti che si vuole conservare. Per aggiungere un prodotto ad una fattura esistente, ad esempio, si può inviare al server gli id delle due righe preesitenti più la nuova riga.

> Esempio di aggiunta di una nuova riga. Richiesta:

```json
{
 "InvoiceProduct": [
		{
		  "id": 11663
		},
		{
		  "id": 11664
		},
		{
		  "name": "Nuova Riga",
		  "qty": 1,
		  "price_ev": 100
		}
   ]
}
```

### Restituisce

In caso di successo viene restituito un oggetto contenente un oggetto **Invoice** e gli oggetti associati.

In caso di errore viene restituito un [Errore](#Error).

### Esempio. Pagamento di una fattura

Se una fattura viene pagata o si riceve una nuova tranche di pagamento, è possibile segnalare il pagamento in due modi:

- Creare un nuovo oggetto [**Income**](#Income-add) indicando come *invoice_id* l'id della fattura di riferimento.
- Inviare una richiesta a `PUT /api/invoices/{id}`, fornendo un array di oggetti **Income** da creare.

In entrambi i casi il campo *total_paid* della fattura verrà aggiornato di conseguenza. Verranno cioè presi in considerazione i diversi **Income** associati alla fattura (quello appena creato e eventuali già esistenti) sommando gli importi.

## Visualizza PDF di una fattura

> Richiesta di esempio:

```http
GET /api/invoices/{id}.pdf?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/invoices/{id}.pdf
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```


E' possibile visualizzare la versione PDF di una fattura.

Fornire l'ID della che è stato consegnato da una richiesta precedente, inserendo l'estensione `.pdf` alla richiesta.

## Visualizza XML di una fattura

> Richiesta di esempio:

```http
GET /api/invoices/{id}.xml?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/invoices/{id}.xml
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```


E' possibile visualizzare la versione XML di una fattura.

Fornire l'ID della che è stato consegnato da una richiesta precedente, inserendo l'estensione `.xml` alla richiesta.

La versione XML della fattura è aderente al [formato tabellare 1.2.1](https://www.fatturapa.gov.it/export/fatturazione/sdi/fatturapa/v1.2.1/Rappresentazione_tabellare_del_tracciato_FatturaPA_versione_1.2.1.pdf) dell'agenzia delle Entrate per la fattura PA.

### HTTP Request

`GET /api/invoices/{id}.pdf`

### Argomenti

Argomento|   | Descrizione
------- | ----- | --------
id | **Obbligatorio**  | Identificativo della fattura. 

### Restituisce


Viene restituito un file `pdf` in caso di successo, altrimenti un errore.



## Invia via Email

> Richiesta di esempio:

```http
POST /api/invoices/email/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/invoices/email/{id}
	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
	-d '{request_body}'
```

> Corpo della richiesta:

```json
{
    "to" : "test@email.com",
    "subject" : "Documento in allegato",
    "message" : "Testo in HTML",
    "attachment" : true
}
```

> Risposta:

```json
{
	"Email": {
		"id": "63",
		"created": "2015-06-01 12:06:57",
		"sender": "your_email@email.com",
		"receiver": [
				"test@email.com"
			],
		"subject": "Documento in allegato",
		"body": "Testo in HTML",
		"sent": null,
		"seen": null
	}
}
```

Chiamando uno specifico endpoint, puoi inviare via email una fattura.

### HTTP Request

`POST /api/invoices/email/{id}`

### Argomenti

Argomento| Tipo |  | Descrizione
------- | ---|----- | --------
id | Integer |**Obbligatorio**  | ID della fattura da passare come parametro. 
to | String or Array|**Obbligatorio**  | A chi inviare l'email. 
subject | String | Opzionale | Oggetto dell'email. 
message | String | Opzionale  | Corpo dell'email in HTML 
attachment | Boolean | Opzionale. Default: *true* | Inviare il documento in pdf allegato oppure sotto forma di link

La fattura sarà inviata in allegato come pdf se l'attributo *attachment* è pari a *true* nella richiesta. In caso di attributo *attachment* pari a *false*, nel corpo dell'email verrà inserito un link su cui il destinatario può cliccare per visualizzare il pdf della fattura.

Puoi scegliere se spedire la tua email a un solo indirizzo o a più di uno, fornendo per il campo *to* una stringa oppure un array di stringhe. Quando uno dei tuoi destinatari visualizzerà l'email, automaticamente questa sarà marcata come letta, con l'attributo *seen* a indicare quando è stata letta. 

### Restituisce

In caso di success viene restituito un [oggetto Email](#email), altrimenti un [Errore](#errors)


## Elimina una Fattura


> Richiesta di esempio:

```http
DELETE /api/invoices/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/invoices/{id}
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

E' possibile eliminare una fattura indicando il suo id.

Eliminando una fattura verranno eliminati allo stesso tempo tutti gli oggetti **InvoiceProduct**, **IncomeDue** e **ShippingDetail** associati.

### HTTP Request

`DELETE /api/invoices/{access_token}`

### Restituisce

Restituisce un oggetto indicante **code** 200 in caso di successo, un [Errore](#Error) altrimenti.

<a name="IncomeDue"></a>




## Ottieni tutte le fatture

> Richiesta di Esempio:

```http
GET /api/invoices/?expand=Income,IncomeDue&access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```
```shell
$ curl 
http://app.adamogestionale.it/api/invoices/?expand=Income,IncomeDue
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```
> Risposta di Esempio:

```json
[
	{
		"Invoice": 
		{
			"id": 1758,
			"created": "2015-05-16 09:52:30",
			"updated": "2015-06-01 09:44:34",
			"number": "2",
			"date": "2015-05-15",
			"customer_id": 714,
			"customer_name": "Mario Rossi",
			"customer_address": "Via Roma, Torino",
			"customer_address_shipment": "",
			"customer_fiscal": "RSSMR",
			"customer_vat_number": "",
			"payment_method": "",
			"payment_dues": "pers",
			"payment_notes": "",
			"notes": "",
			"currency": "EUR",
			"view_preferences": null,
			"view_preference_id": 62,
			"seen": null,
			"updated_by": 153,
			"net_amount": 200,
			"vat_amount": 44,
			"total_amount": 244,
			"total_paid": 488,        
			"contribution_amount": 0,
			"withholding_amount": 0,
			"withholding_rate": 0,
			"withholding_on": 0,
			"contribution_text": "",
			"contribution_rate": 0,
			"contribution_withholding": false
		},
		"InvoiceProduct": [
			{
				"id": 11653,
				"invoice_id": 1771,
				"product_id": 1168,
				"name": "",
				"note": null,
				"qty": 1,
				"measure_unit": null,
				"cost_ev": 0,
				"cost_iv": 0,
				"price_ev": 200,
				"net_price": 200,
				"price_iv": 244,
				"internal_code": null,
				"removed": false,
				"supplier_id": null,
				"supplier_code": null,
				"catalog": 0,
				"supplier_discount": "",
				"supplier_vat": 0,
				"markup": 0,
				"margin": 0,
				"vat": 0.22,
				"tax_id": null,
				"withholding": 0,
				"non_taxable": false,
				"discount": "",
				"order_preference": 0,
				"arrangement_product_id": null,
				"descriptive": null,
				"use_stock": true
			},
			{...}
		],
		"Income": [{...}],
		"IncomeDue": [{...}]
	},
	{
		"Invoice": {...},
		"InvoiceProduct": [{...}],
		"Income": [{...}],
		"IncomeDue": [{...}]
	}
]
```

Questo endpoint elenca tutte le fatture precedentemente create.

### HTTP Request

`GET /api/invoices/`

### Parametri Query

E' possibile filtrare e ordinare la richiesta indicato alcuni parametri in *querystring*

Argomento |  Tipo  | Default | Descrizione
------- | ----- | --- | ----- 
from | Date  | 30 giorni fa | Fatture con data posteriore a una certa data 
to | Date  | oggi |Fatture con data anteriore a una certa data 
order | String *asc* or *desc* | desc |Dalla più recente alla più vecchia (*desc*) o viceversa 
expand | String | InvoiceProduct | Lista di oggetti [associati](#Invoice-associations) alla fattura da mostrare. Nomi separati da una virgola.

### Restituisce

In caso di successo viene restituito un array di oggetti. 

Ogni oggetto contiene al suo interno un `Invoice` e le sue associazioni.

In caso non ci sia nessuna Fattura da mostrare viene restituito un array vuoto.


## Scadenze di Pagamento

```json
{
	"IncomeDue": 
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

Le scadenze di pagamento di una fattura sono descritte dall'oggetto *IncomeDue*.

### Attributi

Attributo | Tipo |Descrizione
-------  | ---| -----
id | Integer (*read-only*)| ID della scadenza di pagamento. 
due_date | Date | Data della scadenza di pagamento. 
amount | Float | Importo di questa scadenza. 
invoice_id | Integer (*read-only*) | ID della fattura a cui appartiene.  


<a name="ShippingDetail"></a>



## Dettagli di Spedizione

```json
{
	"ShippingDetail": 
	{
		"id": 123,
		"created": "2015-05-18 13:54:31",
		"updated": "2015-05-18 13:55:55",
		"causal": "Consegna beni di vendita",
		"transport_charge": "Vettore",
		"updated": "2015-06-18 13:55:55",
		"appearance": "Pacco",
		"weight": "12kg",
		"n_packages": "4",
		"port": "Franco",
		"notes": "Cautela",
		"invoice_id": 1758
	}
}
```

Se si vuole creare una fattura accompagnatoria, cioè che prevede la consegna di merce, si può associare alla fattura un oggetto **ShippingDetail**.

Se la fattura possiede un oggetto **ShippingDetail** viene considerata accompagnatoria. 

### Attributi

Attributo | Tipo |Descrizione
-------  | ---| -----
id | Integer (*read-only*)| ID della scadenza di pagamento. 
invoice_id | Integer (*read-only*) | ID della fattura a cui appartiene. 

Tutti gli altri attributi sono descrittivi e sono opzionali.
