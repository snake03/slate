# Prodotti e servizi

I prodotto o servizi sono gli elementi del catalogo. Il catalogo comprende dunque quelli che sono i prodotti e i servizi di cui si vuole tenere traccia.

Tenere traccia dei propri prodotti è utile per gestire un [magazzino](#magazzino) e per poter elaborare dei report sul venduto e sull'acquistato.

I prodotti sono chiamati **Product** e sono identificati da un unico id.

Ogni oggetto **Product** è associato a diversi oggetti.

Oggetto | Ritorna | Description
--------- | ---| --------
Prezzo di Vendita | Oggetto [ProductPrice](#ProductPrice) | Il prezzo di vendita.
Prezzo di acquisto | Oggetto [ProductSupplier](#ProductSupplier) | Dettagli di acquisto: prezzo e fornitore.
Available | Oggetto [Available](#Available)| Situazione del magazzino
Upload | Array di [Upload](#Upload)| File allegati per questo prodotto


## Oggetto Prodotto

```json
{
       "id": 1168,
       "name": "Bottiglie",
       "description": null,
       "internal_code": null,
       "measure_unit": "pz",
       "length": "20 cm",
       "height": "30 cm",
       "depth": "20 cm",
       "barcode": null,
       "created": "2015-05-21 16:30:14",
       "updated": "2015-05-29 08:16:16",
       "weight": "3 kg"
}
```

L'oggetto Prodotto possiede diverse attributi al suo interno. 

Tutti gli attributi sono *opzionali*. 

### Attributi

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)| L'identificativo del prodotto
created | Datetime (*read-only*) | Quando l'oggetto è stato creato
updated | Datetime (*read-only*) | Quando l'oggetto è stato modificato
name | String | Nome del prodotto
description | String | Descrizione estesa del prodotto
internal_code | String | Codice assegnato a questo prodotto
measure_unit | String | Unità di misura
length | String | Lunghezza
height | String | Altezza
depth | String | Profondità
weight | String | Peso
barcode | String | Codice a barre assegnato

## Associazioni

<a name="ProductPrice"></a>
### Prezzo di vendita

```json
{
       "id": 1,
       "product_id": 2,
       "price": 200,
       "vat": 0.22,
       "tax_id": 15,
       "discount": "0.10"
}
```

Rappresenta i dettagli di vendita del prodotto.

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)| L'identificativo
price | Float | Prezzo IVA Esclusa
vat | Float | Aliquota IVA in percentuale (es: 0.22)
tax_id | Integer | id dell'oggetto [Tax](#tax)
discount | String | Sconti

<a name="ProductSupplier"></a>
### Dettagli di Acquisto

```json
{
		"id": 1,
		"supplier_id": 16,
		"product_id": 2,
		"cost": 100,
		"discount": "0.1",
		"vat": 0.22,
		"tax_id": 15,
		"code": "Cod_Pr_For",
		"created": null,
		"updated": null,
}
```

Rappresenta i dettagli di acquisto per questo prodotto.

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)| L'identificativo
supplier_id | Integer (*read-only*)| Id del Fornitore tra i [Contatti](#Contact)
cost | Float | Costo IVA Esclusa
discount | String | Sconti applicati dal fornitore
vat | Float | percentuale IVA
tax_id | Integer | id dell'oggetto [Tax](#tax)
code | String | Codice interno

<a name="Available"></a>
### Disponibilità

```json
  {
       "available_quantity": 5,
       "updated": "2015-05-29 08:16:16"
  }
```

Rappresenta la quantità disponibile a magazzino per questo prodotto.

Attributo | Tipo | Descrizione
----- | ------- | --------
available_quantity | Float | Quantità disponibile in magazzino


## Crea un nuovo Prodotto 

> Richiesta di esempio:

```http
POST /api/products?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products
	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
	-d "{request_body}"
```

> Body della richiesta nel POST:

```json
{
    "Product" : {
        "name" : "Creazione sito web"
    },
    "ProductPrice":
        {
            "price" : 500,
            "vat" : 0.22
        }
}
```

> Body della risposta di esempio:

```json
{
    "Product": {
        "id": 1171,
        "name": "Creazione sito web",
        "description": null,
        "internal_code": null,
        "measure_unit": null,
        "length": null,
        "height": null,
        "depth": null,
        "barcode": null,
        "domain_id": 254,
        "created": "2015-06-06 14:08:45",
        "updated": "2015-06-06 14:09:19",
        "weight": null
    },
    "ProductPrice": {
        "id": 164,
        "product_id": 1171,
        "price": 500,
        "vat": 0.22,
        "tax_id": null,
        "discount": "0",
        "currency": "EUR"
    },
    "ProductSupplier": {
        "id": null,
        "supplier_id": null,
        "product_id": null,
        "cost": 0,
        "discount": null,
        "vat": 0,
        "tax_id": null,
        "main": null,
        "code": null,
        "created": null,
        "updated": null,
        "currency": null
    },
    "Available": {
        "available_quantity": 0,
        "updated": null
    }
}
```

Crea un nuovo oggetto Prodotto.

### HTTP Request

`POST /api/products/`

### Corpo della richiesta

E' necessario inviare un oggetto comprendente un oggetto **Product**. E' possibile inviare insieme all'oggetto **Product** anche gli oggetti associati che si vuole vengano salvati.

Non è necessario inviare tutti i campi di **Product**, ma solamente quelli di interesse. Tutti gli altri prenderanno il valore di *default*. 

### Restituisce

In caso di successo viene restituito un oggetto che contiene l'oggetto **Product** appena salvato e tutti i suoi oggetti associati.

In caso di errore viene invece restituito un [Errore](#Error).

<a name="product-get"></a>
## Ottieni un solo Prodotto

> Richiesta di esempio:

```http
GET /api/products/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products/{id}
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Body della Risposta di esempio:

```json
{
    "Product": {
        "id": 1171,
        "name": "Creazione sito web",
        "description": null,
        "internal_code": null,
        "removed": false,
        "measure_unit": null,
        "length": null,
        "height": null,
        "depth": null,
        "barcode": null,
        "created": "2015-06-06 14:08:45",
        "updated": "2015-06-06 14:09:19",
        "weight": 0
    },
    "ProductSupplier": {
        "id": null,
        "supplier_id": null,
        "product_id": null,
        "cost": 0,
        "discount": null,
        "vat": 0,
        "tax_id": null,
        "code": null,
        "created": null,
        "updated": null,
        "currency": null
    },
    "ProductPrice": {
        "id": 164,
        "price_list_id": 1,
        "product_id": 1171,
        "price": 500,
        "vat": 0.22,
        "tax_id": null,
        "discount": "0",
        "currency": "EUR"
    },
    "Available": {
        "available_quantity": 0,
        "updated": null
    }
}
```

Ottieni i dettagli di un singolo prodotto.
E' necessario solo fornire l'id del Prodotto fornito dopo che è stato creato.

### HTTP Request

`GET /api/products/{id}`

### Argomenti

Argomento| Tipo |  | Descrizione
------- | ---|----- | --------
id | Integer | **Obbligatorio**  | ID del prodotto da passare come parametro. 

### Restituisce 

In caso di successo viene restituito un oggetto che contiene l'oggetto **Product** corrispondente all'id fornito e tutti i suoi oggetti associati.

In caso di errore viene invece restituito un [Errore](#Error).

## Modifica Prodotto

> Richiesta di Esempio:

```http
PUT /api/products/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products/{id}
	-X PUT
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Body della Richiesta: 

```json
{
    "ProductPrice":
        {
            "price" : 450,
            "vat" : 0.22
        },
    "ProductSupplier":
        {
            "cost" : 200,
            "vat" : 0.22
        }
}
```

> Body della Risposta:

```json
{
    "Product": {
        "id": 1171,
        "name": "Creazione sito web",
        "description": null,
        "internal_code": null,
        "measure_unit": null,
        "length": null,
        "height": null,
        "depth": null,
        "barcode": null,
        "created": "2015-06-06 14:08:45",
        "updated": "2015-06-06 14:21:58",
        "weight": 0
    },
    "ProductSupplier": {
        "id": 149,
        "supplier_id": null,
        "product_id": 1171,
        "cost": 200,
        "discount": null,
        "vat": 0.22,
        "tax_id": null,
        "code": null,
        "created": "2015-06-06 14:21:58",
        "updated": "2015-06-06 14:21:58",
        "currency": "EUR",
        "Supplier": [

        ]
    },
    "ProductPrice": {
        "id": 164,
        "product_id": 1171,
        "price": 450,
        "vat": 0.22,
        "tax_id": null,
        "discount": "0",
        "currency": "EUR"
    },
    "Available": {
        "available_quantity": 0,
        "updated": null
    },
    "Upload": [

    ],
    "Category": [

    ]
}
```

Dopo che un prodotto è stato creato, è possibile modificarlo indicando il suo id.


### HTTP Request

`PUT /api/products/{id}`

Tutti i dati passati nel corpo della richiesta verranno elaborati per modificare l'oggetto **Product** e le sue associazioni.

### Cosa inviare 

Tutti i campi dell'oggetto **Product** che vengono inviati nel corpo della richiesta sovrascriveranno quelli già memorizzati per il prodotto. Per i campi non inviati verranno invece conservati i valori già memorizzati. Non è quindi necessario inviare tutti i campi del **Product** ma solamente quelli che si intende memorizzare.


## Elimina Prodotto

> Richiesta di esempio:

```http
DELETE /api/products/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products/{id}
	-X DELETE
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta:

```json
{
    "code": "200",
    "message": "Articolo Eliminato Correttamente"
}
```

Elimina definitivamente un singolo Prodotto.
Non si può annullare questa azione.

Eliminando il prodotto verranno anche eliminati gli oggetti associati **ProductPrice**, **ProductSupplier** e **Available**.

### HTTP Request

`DELETE /api/products/{id}`

### Restituisce 

Restituisce un oggetto indicante un **code** 200 in caso di successo.

In caso di errore, come ad esempio per un id inesistente, viene restituito un [Errore](#error).


## Ottieni tutti i Prodotti

> Richiesta di esempio:

```http
GET /api/products?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta di esempio:

```json
[
    {
        "Product": {
            "id": 1168,
            "name": "Bottiglie",
            "description": null,
            "internal_code": null,
            "measure_unit": "pz",
            "length": null,
            "height": null,
            "depth": null,
            "barcode": null,
            "created": "2015-05-21 16:30:14",
            "updated": "2015-05-29 08:16:16",
            "weight": 0
        },
        "ProductSupplier": {
            "id": null,
            "supplier_id": null,
            "product_id": null,
            "cost": 0,
            "discount": null,
            "vat": 0,
            "tax_id": null,
            "code": null,
            "created": null,
            "updated": null,
            "currency": null
        },
        "ProductPrice": {
            "id": null,
            "product_id": null,
            "price": 0,
            "vat": 0,
            "tax_id": null,
            "discount": null,
            "currency": null
        },
        "Available": {
            "available_quantity": 5,
            "updated": "2015-05-29 08:16:16"
        }
    },
    {...}
]
```

E' possibile ottenere tutti i prodotti memorizzati.

I prodotti sono elecanti per data di creazione, con il più recente per primo.

### HTTP Request

`GET /api/products`

### Restituisce 

In caso di successo viene restituito un array di oggetti. 

Ogni oggetto contiene al suo interno un **Product** e le sue associazioni.

In caso non ci sia nessun Prodotto memorizzato da mostrare viene restituito un array vuoto.

## Magazzino

Per ogni prodotto puoi gestire le quantità a magazzino, il loro valore, e i diversi movimenti di merce (cioè le entrate e uscite di magazzino). 

Se i tuoi prodotti non hanno un magazzino o intendi tracciare solo dei servizi, puoi ignorare questa parte della guida. 

Per vedere la quantità a magazzino di un determinato prodotto, puoi analizzare l'oggetto **Available** che ottieni quando [richiedi un prodotto](#product-get). Al suo interno è contenuta la quantità attualmente disponibile.

### Valore di magazzino

> Richiesta di esempio:

```http
GET /api/products/stock_value/{id}?method=fifo&access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products/stock_value/{id}?method=fifo
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta di esempio:

```json
{
    "code": 200,
    "method": "fifo",
    "value": 500
}
```

E' possibile calcolare qual è il valore a magazzino di un determinato prodotto.

`GET /api/products/stock_value/{id}`

E' possibile indicare in querystring il parametro *method* per indicare con quale metodo si vuole calcolare il valore. Utilizzano uno dei due valori. Se assente, verrà utilizzato il valore *fifo*. 

Valore | Descrizione
--- | ---
fifo |  First In First Out. *Default*.
lifo | Last In First Out
 
In questo modo, in caso di successo, viene restituito un oggetto con *code* pari a 200, e *value* pari al valore del prodotto a magazzino.


### Elenca i Movimenti

> Esempio di elencazione di un movimento. Richiesta: 

```http
GET /api/products/stock_movements/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products/stock_movements/{id}
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Esempio di elencazione di un movimento. Risposta:

```json
[
    {
        "type": "departure",
        "id": 27,
        "product_id": 1168,
        "delivery_product_id": null,
        "invoice_product_id": 11670,
        "quantity": 10,
        "updated": "2015-06-08 16:05:52",
        "causal": null,
        "date": "2015-06-08",
        "value": "200",
        "InvoiceProduct": {
        		"id": 11670
        	  	...,
            	"Invoice": {...}
        }
    },
    {...}
]
```

Si possono elencare i diversi movimenti di magazzino, quindi tutti i carichi e scarichi di merce, accedendo all'endpoint:

`GET /api/products/stock_movements/{id}`

In caso di successo verrà restituito un array di oggetti. Ognuno di questi rappresenta un movimento, con il valore **type** pari a **departure** se si tratta di un'uscita di merce o pari a **arrival** se si tratta di un ingresso.

Qualora il movimento dipenda da una fattura emessa, un documento di trasporto (per le uscite di merce) o una spesa (per un ingresso), al suo interno possono essere presenti rispettivamente gli oggetti **InvoiceProduct**, **DeliveryProduct** e **ArrivalProduct**. Questi oggetti, se presenti, al loro interno contengono rispettivamente un oggetto **Invoice**, **Delivery** o **Arrival** a cui appartengono.

Se non esistono movimenti verrà restituito un array vuoto.

### Crea un nuovo movimento

> Esempio di creazione di un movimento. Richiesta: 

```http
POST /api/products/stock_movements/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products/stock_movements/{id}
	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

```json
{
    "Departed" : [{
        "quantity" : 10
    }]
}
```
> Esempio di creazione di un movimento. Risposta:

```json
{
    "Departed": {
        "id": "30",
        "product_id": "1173",
        "delivery_product_id": null,
        "invoice_product_id": null,
        "quantity": "10",
        "updated": "2015-06-09 10:05:07",
        "causal": null,
        "date": "2015-06-09",
        "value": null,
        "domain_id": "254"
    },
    "Product": {...}
}
```

Normalmente i movimenti di magazzino sono automatici e scattano quando si emette una fattura, un documento di trasporto o quando si registra una spesa. 

Tuttavia può essere utile talvolta registrare un movimento indipendente, senza che dunque dipenda da qualche documento di vendita. 

L'oggetto **Departed** indica un'uscita di merce. L'oggetto **Arrived** indica invece un'ingresso di merce.  

`POST /api/products/stock_movements/{id}`


### Elimina un movimento

> Esempio di eliminazione di un movimento. Richiesta:

```http
DELETE /api/products/stock_movements/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/products/stock_movements/{id}
	-X DELETE
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

```json
{
    "Departed" : {
        "id" : 124
    }
}
```
> Esempio di eliminazione di un movimento. Risposta:

```json
{
    "code": 200,
    "message" : "Movimento Eliminato Correttamente"
}
```

Per eliminare un movimento di magazzino è necessario mandare una richiesta all'indirizzo:

`DELETE /api/products/stock_movements/{id}`

Fornendo l'*id* del prodotto a cui è associato il movimento.

Nel corpo della richiesta è necessario passare un oggetto **Departed** o **Arrived**, cioè un movimento d'uscita o d'ingresso, con all'interno l'*id* **del movimento** che si vuole eliminare.

Ad esempio per eliminare l'oggetto **Departed** con l'id 124, devi mandare nel body della richiesta:

```{ "Departed" : { "id" : 124 } }```

In caso di successo verrà restituito un oggetto con *code* 200. 