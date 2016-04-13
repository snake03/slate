<a name="Report"></a>
# Report

Tramite i report è possibile ottenere una panoramica dell'andamento della propria impresa. 

I Report aggregano ed elaborano i dati già salvati fornendo le informazioni richieste.

## Crediti per cliente

> Richiesta: 

```http
GET /api/reports/credits_by_customer?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/reports/credits_by_customer
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta: 

```json
{
    "Customers": [
        {
            "id": "714",
            "name": "Carletto",
            "amount": 9488,
            "Invoice": [
                {
                    "id": "1758",
                    "customer_id": "714",
                    "created": "2015-05-16 09:52:30",
                    "updated": "2015-06-01 11:11:05",
                    "number": "2",
                    "date": "2015-05-15",
                    "customer_name": "Carletto",
                    "customer_address": "",
                    "customer_address_shipment": "",
                    "customer_fiscal": "",
                    "customer_vat_number": "",
                    "payment_method": "",
                    "payment_dues": "pers",
                    "payment_notes": "",
                    "notes": "",
                    "currency": "EUR",
                    "advance_id": null,
                    "net_amount": 200.0000,
                    "total_amount": 244.0000,
                    "vat_amount": "44.000",
                    "contribution_amount": 0.000,
                    "withholding_amount": 0.000,
                    "withholding_rate": null,
                    "withholding_on": null,
                    "contribution_text": "",
                    "contribution_rate": null,
                    "contribution_withholding": false,
                    "total_paid": 100.00,
                    "IncomeDue": [
                        {
                            "id": "36",
                            "due_date": "2015-04-01",
                            "amount": 244.00,
                            "description": null,
                            "invoice_id": "1758",
                            "created": "2015-05-18 13:54:31",
                            "updated": "2015-05-18 13:55:55"
                        }
                    ],
                    "Income": [
                        {
                            "id": "1256",
                            "date": "2015-06-01",
                            "amount": 100.00,
                            "mean": null,
                            "invoice_id": "1758",
                            "receipt_id": null,
                            "causal": null,
                            "created": "2015-06-01 09:43:29",
                            "updated": "2015-06-01 09:43:29",
                            "refresh_balance": false,
                            "note": "",
                            "resource": null,
                            "resource_id": null
                        },
                        {...}
                    ]
                }
            ]
        },
        {
            "id": "15",
            "name": "Ermanno",
            "amount": 549,
            "Invoice": [
                {
                    "id": "1767",
                    "customer_id": "15",
                    "created": "2015-05-23 09:23:48",
                    "updated": "2015-05-23 15:09:44",
                    "number": "3",
                    "date": "2015-05-23",
                    "customer_name": "Ermanno",
                    "customer_address": "",
                    "customer_address_shipment": "",
                    "customer_fiscal": null,
                    "customer_vat_number": null,
                    "payment_method": null,
                    "payment_dues": null,
                    "payment_notes": null,
                    "notes": null,
                    "currency": "EUR",
                    "net_amount": "450.0000",
                    "total_amount": "549.0000",
                    "vat_amount": "99.000",
                    "contribution_amount": "0.000",
                    "withholding_amount": "0.000",
                    "withholding_rate": null,
                    "withholding_on": null,
                    "contribution_text": null,
                    "contribution_rate": null,
                    "contribution_withholding": null,
                    "total_paid": null
                    "IncomeDue": [

                    ],
                    "Income": [

                    ]
                }
            ]
        }
    ]
}
```

Quando una fattura non è ancora stata del tutto pagata, quindi quando ha un *total_paid* inferiore a *total_amount*, il sistema la valuterà come non pagata e attribuirà un debito del cliente nei tuoi confronti. Tu quindi hai un credito nei confronti del cliente.

Per ottenere una lista dei crediti per ogni cliente si può utilizzare l'endpoint:

`GET /api/uploads`

Si noti che i clienti devono essere tra i [Contatti](#Contact). Quindi la fattura non pagata deve avere *customer_id* non nullo e che punta verso un valido [Contatto](#Contact) perché questo venga conteggiato come credito.


### Restituisce

Viene restituito in caso di successo un oggetto comprendente un array denominato **Customers**. All'interno dell'array sono elencati tutti i Contatti che hanno un credito nei tuoi confronti.

Attributo | Tipo | Descrizione
--------- | --- | ---
id | Integer | identificativo del [Contatto](#Contact)
name | String | nome del contatto
amount | Float | importo del credito

All'interno dell'oggetto sono elencati, qualora presenti, tutti gli oggetti **Invoice** e **Credit Note** che hanno quel Contatto come cliente e che quindi contribuiscono al credito.

