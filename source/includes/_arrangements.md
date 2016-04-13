# Altri Documenti

Esistono diversi altri tipi di documenti, strutturalmente molto simili alle fatture [**Invoice**](#Invoice) ma con ruoli diversi.

In questa sezione descriviamo velocemente quali sono gli oggetti che rappresentano questi documenti, qual è il loro ruolo e come manipolare i dati.

Essendo per larga parte simili a quanto già visto per l'oggetto [**Invoice**](#Invoice), se ne da qui una descrizione sommaria. Si consiglia pertanto di rifarsi alla documentazione di [**Invoice**](#Invoice) per tutti i dettagli. 

## Preventivi

```json
{
        "Quote": {
            "id": 94,
            "customer_id": null,
            "created": "2015-05-15 09:38:24",
            "updated": "2015-05-15 09:38:24",
            "removed": false,
            "accepted": null,
            "accepted_date": null,
            "number": 2,
            "date": "2015-05-15",
            "customer_name": "",
            "customer_address": "",
            "customer_address_shipment": "",
            "payment_method": null,
            "payment_dues": null,
            "payment_notes": null,
            "customer_fiscal": "BLTZ",
            "notes": "",
            "currency": "EUR",
            "updated_by": 153,
            "seen": null,
            "net_amount": 5000,
            "total_amount": 6000,
            "contribution_amount": 0,
            "withholding_amount": 0,
            "withholding_rate": 0,
            "withholding_on": 0,
            "contribution_text": null,
            "contribution_rate": 0,
            "contribution_withholding": null
        },
        "QuoteProduct": [
            {
                "id": 793,
                "quote_id": 94,
                "product_id": null,
                "name": "TEST",
                "description": "",
                "note": null,
                "qty": 2,
                "measure_unit": null,
                "cost_ev": 0,
                "cost_iv": 0,
                "price_ev": 2500,
                "net_price": 0,
                "price_iv": 0,
                "internal_code": null,
                "supplier_id": null,
                "supplier_code": null,
                "catalog": 0,
                "supplier_discount": "",
                "supplier_vat": 0,
                "markup": 0,
                "margin": 0,
                "vat": 0.2,
                "tax_id": null,
                "discount": "",
                "order_preference": 0,
                "withholding": 0,
                "non_taxable": false
            },
            {...}
		]
}
```


I Preventivi sono chiamati **Quote**. 

Un preventivo è documento che viene emesso prima di una vendita e serve per informare il cliente su quelli che saranno i prezzi di vendita.

Ogni oggetto **Quote** ha delle associazioni

Oggetto | Ritorna | Description
--------- | ---| --------
QuoteProduct | Array di [QuoteProduct](#QuoteProduct) | Righe del preventivo
Customer | Oggetto [Contact](#Contact) | Il cliente di questo preventivo selezionato tra i diversi contatti.

Ogni preventivo porta con sé diverse righe, chiamate **QuoteProduct**.

Queste sono il corpo del documento, e grazie a loro viene calcolato il totale.

Gli attributi di **Quote** sono identici a quelli di **Invoice**, così come quelli di **QuoteProduct** sono identici a quelli di **InvoiceProduct**.

L'oggetto **Quote** ha inoltre i campi

Oggetto | Ritorna | Description
--------- | ---| --------
accepted | Boolean. Default *false* | Il preventivo è stato accettato dal cliente?
accepted_date | DateTime | Quando è stato accettato


### Crea un nuovo preventivo

E' possibile creare un nuovo preventivo con una richiesta ad uno specifico endpoint:

`POST /api/quotes/`

Inviando nel corpo della richiesta un oggetto **Quote** con gli attributi che si vuole salvare.


### Ottieni un preventivo

Chiamando l'endpoint:

`GET /api/quotes/{id}`

Verrà restituito un oggetto contentente un oggetto **Quote** e un array di **QuoteProduct**, qualora l'id fornito sia valido.

### Modifica un preventivo

Chiamando l'endpoint:

`PUT /api/quotes/{id}`

Indicando nel corpo del documento l'oggetto **Quote** (e, se si vuole, i diversi **QuoteProduct**).

In caso di successo viene restituito un oggetto contenente l'oggetto **Quote** e tutti i suoi associati.

### Elimina un preventivo

E' possibile chiamare l'endpoint:

`DELETE /api/quotes/{id}`

Per eliminare in modo permanente un preventivo.

In caso di successo viene restituto un oggetto con attributo *code* 200.

### Ottieni tutti i preventivi

Chiamando l'endpoint:

`GET /api/quotes`

In caso di successo viene restituito un array di oggetti. 

Ogni oggetto contiene al suo interno un **Quote** e le sue associazioni.

In caso non ci sia nessun preventivo da mostrare viene restituito un array vuoto.

### Invia via email un preventivo

Chiamando uno specifico endpoint, puoi inviare via email il tuo preventivo.

`POST /api/quotes/email/{id}`

Dovranno essere indicati nel corpo della richiesta gli attributi *to*, *subject*, *message* e *attachment*. 

In caso di success viene restituito un [oggetto Email](#email), altrimenti un [Errore](#errors).


### Visualizza un preventivo pdf

E' possibile visualizzare la versione PDF di un preventivo.

Fornire l'ID della che è stato consegnato da una richiesta precedente, inserendo l'estensione `.pdf` alla richiesta.

`GET /api/quotes/{id}.pdf`

Viene restituito un file `pdf` in caso di successo, altrimenti un errore.

## Contratti

```json
{
    "Arrangement": {
        "id": 102,
        "commission_id": 87,
        "customer_id": null,
        "created": "2015-06-06 21:51:55",
        "updated": "2015-06-06 21:51:55",
        "number": 2,
        "date": "2015-06-06",
        "payment_method": "",
        "payment_dues": "",
        "payment_notes": "",
        "customer_name": "",
        "customer_address": "",
        "customer_address_shipment": "",
        "customer_fiscal": null,
        "notes": "",
        "currency": "EUR",
        "quote_id": null,
        "updated_by": 153,
        "view_preferences": null,
        "view_preference_id": 62,
        "view_token": null,
        "seen": null,
        "net_amount": 200,
        "total_amount": 200,
        "contribution_amount": 0,
        "withholding_amount": 0,
        "withholding_rate": 0,
        "withholding_on": 0,
        "contribution_text": null,
        "contribution_rate": 0,
        "contribution_withholding": null
    },
    "ArrangementProduct": [
        {
            "id": 691,
            "arrangement_id": 102,
            "product_id": null,
            "name": "TEST",
            "description": "",
            "note": null,
            "qty": 1,
            "cost_ev": 0,
            "cost_iv": 0,
            "price_ev": 200,
            "net_price": 200,
            "price_iv": 200,
            "internal_code": null,
            "removed": false,
            "supplier_id": null,
            "supplier_code": null,
            "catalog": 0,
            "supplier_discount": "",
            "supplier_vat": 0,
            "markup": 0,
            "margin": 0,
            "vat": 0,
            "tax_id": null,
            "discount": "",
            "order_preference": 0,
            "descriptive": null,
            "withholding": 0,
            "non_taxable": false,
            "measure_unit": null
        }
    ]
}
```

I Contratti sono chiamati **Arrangement**. 

Un contratto normalmente segue il preventivo e precede la fattura. Se il preventivo viene accettato, si può creare un contratto.

Ogni oggetto **Arrangement** ha delle associazioni

Oggetto | Ritorna | Description
--------- | ---| --------
ArrangementProduct | Array di [ArrangementProduct](#ArrangementProduct) | Righe del contratto
Customer | Oggetto [Contact](#Contact) | Il cliente di questo contratto selezionato tra i diversi contatti.

Ogni contratto porta con sé diverse righe, chiamate **ArrangementProduct**.

Queste sono il corpo del documento, e grazie a loro viene calcolato il totale.

Gli attributi di **Arrangement** sono identici a quelli di **Invoice**, così come quelli di **ArrangementProduct** sono identici a quelli di **InvoiceProduct**.

L'oggetto **Arrangement** ha inoltre i campi

Oggetto | Ritorna | Description
--------- | ---| --------
quote_id | Integer. *opzionale* | L'id del preventivo dal quale è nato questo contratto.


### Crea un nuovo Contratto

E' possibile creare un nuovo contratto con una richiesta ad uno specifico endpoint:

`POST /api/arrangements/`

Inviando nel corpo della richiesta un oggetto **Arrangement** con gli attributi che si vuole salvare.

### Ottieni un Contratto

Chiamando l'endpoint:

`GET /api/arrangements/{id}`

Verrà restituito un oggetto contentente un oggetto **Arrangement** e un array di **ArrangementProduct**, qualora l'id fornito sia valido.

### Modifica un Contratto

Chiamando l'endpoint:

`PUT /api/arrangements/{id}`

Indicando nel corpo del documento l'oggetto **Arrangement** (e, se si vuole, i diversi **ArrangementProduct**).

In caso di successo viene restituito un oggetto contenente l'oggetto **Arrangement** e tutti i suoi associati.

### Elimina un Contratto

E' possibile chiamare l'endpoint:

`DELETE /api/arrangements/{id}`

Per eliminare in modo permanente un contratto.

In caso di successo viene restituto un oggetto con attributo *code* 200.

### Ottieni tutti i Contratti

Chiamando l'endpoint:

`GET /api/arrangements`

In caso di successo viene restituito un array di oggetti. 

Ogni oggetto contiene al suo interno un **Arrangement** e le sue associazioni.

In caso non ci sia nessun contratto da mostrare viene restituito un array vuoto.

### Invia via email un contratto

Chiamando uno specifico endpoint, puoi inviare via email il tuo contratto.

`POST /api/arrangements/email/{id}`

Dovranno essere indicati nel corpo della richiesta gli attributi *to*, *subject*, *message* e *attachment*. 

In caso di success viene restituito un [oggetto Email](#email), altrimenti un [Errore](#errors).


### Visualizza un Contratto pdf

E' possibile visualizzare la versione PDF di un contratto.

Fornire l'ID della che è stato consegnato da una richiesta precedente, inserendo l'estensione `.pdf` alla richiesta.

`GET /api/arrangements/{id}.pdf`

Viene restituito un file `pdf` in caso di successo, altrimenti un errore.

## Documento di consegna
```json
{
   "Delivery": {
       "id": 93,
       "customer_id": null,
       "created": "2015-06-07 16:41:30",
       "updated": "2015-06-07 16:41:30",
       "number": 3,
       "date": "2015-06-07",
       "customer_name": null,
       "customer_address": null,
       "customer_address_shipment": null,
       "customer_fiscal": null,
       "causal": null,
       "transport_charge": null,
       "delivery_date": null,
       "appearance": null,
       "weight": null,
       "port": null,
       "n_packages": null,
       "notes": null,
       "currency": "EUR",
       "net_amount": 200,
       "total_amount": 200,
       "contribution_amount": 0,
       "withholding_amount": 0,
       "withholding_rate": 0,
       "withholding_on": 0,
       "contribution_text": null,
       "contribution_rate": 0,
       "contribution_withholding": null
   },
   "DeliveryProduct": [
       {
           "id": 704,
           "delivery_id": 93,
           "product_id": null,
           "name": "TEST",
           "description": "",
           "note": null,
           "qty": 1,
           "price_ev": 200,
           "net_price": 200,
           "price_iv": 200,
           "internal_code": null,
           "removed": false,
           "vat": 0,
           "tax_id": null,
           "withholding": 0,
           "non_taxable": false,
           "discount": "",
           "order_preference": 0,
           "descriptive": null,
           "use_stock": null,
           "measure_unit": null
       }
   ]
}
```

I Documenti di Trasporto sono chiamati **Delivery**. 

Un Documento di Trasporto è un documento che viene redatto quando c'è una consegna di merce.

Ogni oggetto **Delivery** ha delle associazioni

Oggetto | Ritorna | Description
--------- | ---| --------
DeliveryProduct | Array di [DeliveryProduct](#DeliveryProduct) | Righe del Documento
Customer | Oggetto [Contact](#Contact) | Il cliente di questo documento selezionato tra i diversi contatti.

Ogni DDT porta con sé diverse righe, chiamate **DeliveryProduct**.

Queste sono il corpo del documento, e grazie a loro viene calcolato il totale.

Gli attributi di **Delivery** sono identici a quelli di **Invoice**, così come quelli di **DeliveryProduct** sono identici a quelli di **InvoiceProduct**.

L'oggetto **Delivery** ha inoltre i campi *opzionali*: 

Oggetto | Ritorna | Description
--------- | ---| --------
causal | String | Causale di scarico merce
transport_charge | String | Consegna a mezzo (es: mittente, destinatario,...) 
delivery_date | DateTime | Data di consegna
appearance | String | Aspetto esteriore dei beni (es: pacco)
weight | String | Peso
port | String | Porto (es: franco,...)
n_packages | String | Numero di colli
    

### Crea un nuovo DDT

E' possibile creare un nuovo DDT con una richiesta ad uno specifico endpoint:

`POST /api/deliveries/`

Inviando nel corpo della richiesta un oggetto **Delivery** con gli attributi che si vuole salvare.

### Ottieni un DDT

Chiamando l'endpoint:

`GET /api/deliveries/{id}`

Verrà restituito un oggetto contentente un oggetto **Delivery** e un array di **DeliveryProduct**, qualora l'id fornito sia valido.

### Modifica un DDT

Chiamando l'endpoint:

`PUT /api/deliveries/{id}`

Indicando nel corpo del documento l'oggetto **Delivery** (e, se si vuole, i diversi **DeliveryProduct**).

In caso di successo viene restituito un oggetto contenente l'oggetto **Delivery** e tutti i suoi associati.

### Elimina un DDT

E' possibile chiamare l'endpoint:

`DELETE /api/deliveries/{id}`

Per eliminare in modo permanente un DDT.

In caso di successo viene restituto un oggetto con attributo *code* 200.

### Ottieni tutti i DDT

Chiamando l'endpoint:

`GET /api/deliveries`

In caso di successo viene restituito un array di oggetti. 

Ogni oggetto contiene al suo interno un **Delivery** e le sue associazioni.

In caso non ci sia nessun DDT da mostrare viene restituito un array vuoto.

### Invia via email un DDT

Chiamando uno specifico endpoint, puoi inviare via email il tuo DDT.

`POST /api/deliveries/email/{id}`

Dovranno essere indicati nel corpo della richiesta gli attributi *to*, *subject*, *message* e *attachment*. 

In caso di success viene restituito un [oggetto Email](#email), altrimenti un [Errore](#errors).


### Visualizza un DDT in pdf

E' possibile visualizzare la versione PDF di un DDT.

Fornire l'ID della che è stato consegnato da una richiesta precedente, inserendo l'estensione `.pdf` alla richiesta.

`GET /api/deliveries/{id}.pdf`

Viene restituito un file `pdf` in caso di successo, altrimenti un errore.

## Note di Credito
```json
{
   "CreditNote": {
       "id": 16,
       "customer_id": null,
       "created": "2015-06-07 16:58:07",
       "updated": "2015-06-07 16:58:07",
       "number": 2,
       "date": "2015-06-07",
       "customer_name": "",
       "customer_address": "",
       "customer_address_shipment": "",
       "customer_fiscal": null,
       "details": null,
       "notes": "",
       "currency": "EUR",
       "contribution_amount": 0,
       "withholding_amount": 0,
       "withholding_rate": 0,
       "withholding_on": 0,
       "contribution_text": null,
       "contribution_rate": 0,
       "contribution_withholding": null,
       "intestation_id": null,
       "total_amount": 200,
       "total_paid": 0,
       "net_amount": 200,
       "invoice_id": null
   },
   "Invoice": {
       "id": null,
       "number": null,
       "date": null
   },
   "CreditNoteProduct": [
       {
           "id": 40,
           "credit_note_id": 16,
           "product_id": null,
           "name": "TEST",
           "note": null,
           "qty": 1,
           "measure_unit": null,
           "cost_ev": 0,
           "cost_iv": 0,
           "price_ev": 200,
           "net_price": 200,
           "price_iv": 200,
           "diluted_price_ev": 0,
           "diluted_price_iv": 0,
           "internal_code": null,
           "removed": false,
           "supplier_id": null,
           "supplier_code": null,
           "catalog": 0,
           "supplier_discount": "",
           "supplier_vat": 0,
           "markup": 0,
           "margin": 0,
           "vat": 0,
           "tax_id": null,
           "withholding": 0,
           "non_taxable": false,
           "discount": "",
           "order_preference": 0,
           "arrangement_product_id": null,
           "descriptive": null,
           "use_stock": null
       }
   ]
}
```

Le Note di Credito sono chiamate **CreditNote**. 

Una Nota di Credito è un documento che attesta un credito del cliente nei confronti dell'azienda. Può essere immaginato come l'opposto della fattura.

Ogni oggetto **CreditNote** ha delle associazioni

Oggetto | Ritorna | Description
--------- | ---| --------
CreditNoteProduct | Array di [CreditNoteProduct](#CreditNoteProduct) | Righe del Documento
Invoice | Oggetto [Invoice](#Invoice) | La fattura che ha originato questa nota di credito.
Outflow | Array di [Outflow](#Outflow) | Tutti i pagamenti (in uscita) per saldare questa nota.

Ogni NdC porta con sé diverse righe, chiamate **CreditNoteProduct**.

Queste sono il corpo del documento, e grazie a loro viene calcolato il totale.

Gli attributi di **CreditNote** sono identici a quelli di **Invoice**, così come quelli di **CreditNoteProduct** sono identici a quelli di **InvoiceProduct**.

L'oggetto **CreditNote** ha inoltre i campi *opzionali*: 

Oggetto | Ritorna | Description
--------- | ---| --------
invoice_id | Integer | Indica l'id della fattura che ha originato questa nota.

### Crea una nuova Nota di Credito

E' possibile creare una nuova nota di credito con una richiesta ad uno specifico endpoint:

`POST /api/credit_notes/`

Inviando nel corpo della richiesta un oggetto **CreditNote** con gli attributi che si vuole salvare.

### Ottieni una Nota di Credito

Chiamando l'endpoint:

`GET /api/credit_notes/{id}`

Verrà restituito un oggetto contentente un oggetto **CreditNote** e un array di **CreditNoteProduct**, qualora l'id fornito sia valido.

### Modifica una Nota di Credito

Chiamando l'endpoint:

`PUT /api/credit_notes/{id}`

Indicando nel corpo del documento l'oggetto **CreditNote** (e, se si vuole, i diversi **CreditNoteProduct**).

In caso di successo viene restituito un oggetto contenente l'oggetto **CreditNote** e tutti i suoi associati.

### Elimina una Note di Credito

E' possibile chiamare l'endpoint:

`DELETE /api/credit_notes/{id}`

Per eliminare in modo permanente una Note di Credito.

In caso di successo viene restituto un oggetto con attributo *code* 200.

### Ottieni tutte le Note di Credito

Chiamando l'endpoint:

`GET /api/credit_notes`

In caso di successo viene restituito un array di oggetti. 

Ogni oggetto contiene al suo interno un **CreditNote** e le sue associazioni.

In caso non ci sia nessuna Nota di Credito da mostrare viene restituito un array vuoto.

### Invia via email una Nota di Credito

Chiamando uno specifico endpoint, puoi inviare via email la tua nota di credito.

`POST /api/credit_notes/email/{id}`

Dovranno essere indicati nel corpo della richiesta gli attributi *to*, *subject*, *message* e *attachment*. 

In caso di successo viene restituito un [oggetto Email](#email), altrimenti un [Errore](#errors).


### Visualizza una Nota di Credito in pdf

E' possibile visualizzare la versione PDF di una Nota di Credito.

Fornire l'ID della che è stato consegnato da una richiesta precedente, inserendo l'estensione `.pdf` alla richiesta.

`GET /api/credit_notes/{id}.pdf`

Viene restituito un file `pdf` in caso di successo, altrimenti un errore.

## Ricevute


