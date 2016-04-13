<a name="Contact"></a>
# Contatti

I contatti sono gli elementi di una rubrica e comprendono persone e aziende di cui si vuole tenere traccia.

I contatti sono chiamati **Contact** e sono identificati da un unico id.

Ogni oggetto **Contact** è associato a diversi oggetti. Ogni contatto può avere da zero a molti di questi oggetti.

Oggetto | Ritorna | Description
--------- | ---| --------
Address | Array di [Address](#Address) | Gli indirizzi del Contatto.
Mail | Array di [Mail](#Mail) | Gli indirizzi email associati a questo contatto.
Phone | Array di [Phone](#Phone)| I numeri di telefono.
Field | Array di [Field](#Field) | Campi personalizzati.

## Oggetto Contatto

```json
{
 		"id": "714",
       "name": "Mario",
       "last_name": "Rossi",
       "prefix": "Signor",
       "suffix": "Jr.",
       "middle": null,
       "nickname": "marietto",
       "job": "Dirigente",
       "company": "Adamo Lab",
       "is_natural_person": true,
       "fiscal_code": "RSSMR45X19S",
       "vat_number": null,
       "avatar_id": null,
       "notes": "",
       "birthday": null,
       "modified": "2015-06-01 14:54:55",
       "created": "2015-05-16 08:45:12",
       "facebook": null,
       "twitter": null,
       "google": "",
       "linkedin": null,
       "is_customer": false,
       "is_supplier": false
  }
```

L'oggetto Contatto possiede diverse attributi al suo interno. 

Tutti gli attributi sono *opzionali*. 

### Attributi

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)| L'identificativo del contatto
created | Datetime (*read-only*) | Quando l'oggetto è stato creato
updated | Datetime (*read-only*) | Quando l'oggetto è stato modificato
name | String | Nome
last_name | String | Cognome
prefix | String | Prefisso del nome
middle | String | Secondo nome
suffix | String | Suffisso del nome
nickname | String | soprannome
job | String | Ruolo lavorativo
company | String | Nome dell'azienda per cui lavora
is_natural_person | Boolean. Default: *false*  | *True* se questo contatto è una persona, *False* se è un'azienda
fiscal_code | String | Codice Fiscale
vat_number | String | Partita IVA
avatar_id | Integer | id del [Upload](#Upload) avatar
birthday | Date | Data di nascita
is_customer | Boolean. Default: *false* | E' un cliente?
is_supplier | Boolean. Default: *false* | E' un fornitore?

## Associazioni

<a name="Address"></a>
### Address

```json
{
      "id": "193",
      "contact_id": "714",
      "street": "Via di Prova",
      "city": "Roma",
      "province": "RO",
      "zip": "10100",
      "country": "Italy",
      "title": "Residenza"
}
```

Rappresenta gli indirizzi del contatto.

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)| L'identificativo
street | String | Via
city | String | Città
province | String | Provincia
zip | String | Codice Avviamento Postale
country | String | Stato
title | String | Definizione dell'indirizzo

<a name="Phone"></a>
### Phone

```json
{
      "id": "187",
      "contact_id": "714",
      "label": "Telefono di Casa",
      "value": "011 101010"
 }
```

Rappresenta i numeri di telefono del contatto.

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)| L'identificativo
label | String | Descrizione
value | String | Numero di telefono

<a name="Mail"></a>
### Mail

```json
  {
      "id": "180",
      "contact_id": "715",
      "label": "Ufficio",
      "value": "mario.rossi@adamo.it"
  }
```

Rappresenta gli indirizzi email del contatto

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)| L'identificativo
label | String | Descrizione
value | String | Indirizzo email

<a name="Field"></a>
### Field

```json
  {
      "id": "1",
      "contact_id": "714",
      "label": "Nome del Fratello",
      "value": "Luigi"
  }
```

Rappresenta i campi personalizzati del contatto

Attributo | Tipo | Descrizione
----- | ------- | --------
id | Integer (*read-only*)| L'identificativo
label | String | Descrizione
value | String | Valore del campo



## Crea un nuovo Contatto 

> Richiesta di esempio:

```http
POST /api/contacts?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/contacts
	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
	-d "{request_body}"
```

> Body della richiesta nel POST:

```json
{
    "Contact": {
        "name" : "Luigi",
        "last_name" : "Bianchi"
    },
    "Mail" : [{
        "label" : "casa",
        "value" : "luigi.bianchi@email.com"
    }]
}
```

> Body della risposta di esempio:

```json
{
    "Contact": {
        "id": "717",
        "name": "Luigi",
        "last_name": "Bianchi",
        "prefix": null,
        "suffix": null,
        "middle": null,
        "nickname": null,
        "job": null,
        "company": null,
        "is_natural_person": true,
        "fiscal_code": null,
        "vat_number": null,
        "avatar_id": null,
        "removed": false,
        "notes": "",
        "birthday": null,
        "modified": "2015-06-03 10:52:52",
        "created": "2015-06-03 10:52:52",
        "facebook": null,
        "twitter": null,
        "google": "",
        "linkedin": null,
        "is_customer": false,
        "is_supplier": false
    },
    "Address": [

    ],
    "Mail": [
        {
            "id": "188",
            "contact_id": "717",
            "label": "casa",
            "value": "luigi.bianchi@email.com"
        }
    ],
    "Field": [

    ],
    "Phone": [

    ]
}
```

Crea un nuovo oggetto contatto.

### HTTP Request

`POST /api/contacts/`

### Corpo della richiesta

E' necessario inviare un oggetto comprendente un oggetto **Contact**. E' possibile inviare insieme all'oggetto **Contact** anche array di oggetti associati perchè vengano salvati insieme.

Non è necessario inviare tutti i campi di **Contact**, ma solamente quelli di interesse. Tutti gli altri prenderanno il valore di *default*. 

### Restituisce

In caso di successo viene restituito un oggetto che contiene l'oggetto **Contact** appena salvato e tutti i suoi oggetti associati.

In caso di errore viene invece restituito un [Errore](#Error).

## Ottieni un solo Contatto

> Richiesta di esempio:

```http
GET /api/contacts/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/contacts/{id}
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Body della Risposta di esempio:

```json
{
    "Contact": {
        "id": "717",
        "domain_id": "254",
        "name": "Luigi",
        "last_name": "Bianchi",
        "prefix": null,
        "suffix": null,
        "middle": null,
        "nickname": null,
        "job": null,
        "company": null,
        "is_natural_person": true,
        "fiscal_code": null,
        "vat_number": null,
        "avatar_id": null,
        "removed": false,
        "notes": "",
        "birthday": null,
        "modified": "2015-06-03 10:52:52",
        "created": "2015-06-03 10:52:52",
        "facebook": null,
        "twitter": null,
        "google": "",
        "linkedin": null,
        "portal_username": null,
        "portal_password": null,
        "portal_active": null,
        "is_customer": false,
        "is_supplier": false,
        "stripe_id": null
    },
    "Address": [

    ],
    "Mail": [
        {
            "id": "188",
            "contact_id": "717",
            "label": "casa",
            "value": "luigi.bianchi@email.com",
            "pref": false,
            "order_preference": null,
            "removed": null
        }
    ],
    "Field": [

    ],
    "Phone": [

    ]
}
```

Ottieni i dettagli di un singolo contatto.
E' necessario solo fornire l'id del Contatto fornito dopo che è stato creato.

### HTTP Request

`GET /api/contacts/{id}`

### Argomenti

Argomento| Tipo |  | Descrizione
------- | ---|----- | --------
id | Integer | **Obbligatorio**  | ID del contatto da passare come parametro. 

### Restituisce 

In caso di successo viene restituito un oggetto che contiene l'oggetto **Contact** corrispondente all'id fornito e tutti i suoi oggetti associati.

In caso di errore viene invece restituito un [Errore](#Error).

## Modifica Contatto

> Richiesta di Esempio:

```http
PUT /api/contacts/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/contacts/{id}
	-X PUT
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Body della Richiesta: 

```json
{
    "Contact": {
        "fiscal_code" : "BNCLGG123X10A"
    },
    "Mail" : [{
        "label" : "ufficio",
        "value" : "luigi.bianchi@email.it"
    }]
}
```

> Body della Risposta:

```json
{
    "Contact": {
        "id": "717",
        "domain_id": "254",
        "name": "Luigi",
        "last_name": "Bianchi",
        "prefix": null,
        "suffix": null,
        "middle": null,
        "nickname": null,
        "job": null,
        "company": null,
        "is_natural_person": true,
        "fiscal_code": "BNCLGG123X10A",
        "vat_number": null,
        "avatar_id": null,
        "notes": "",
        "birthday": null,
        "modified": "2015-06-03 13:55:25",
        "created": "2015-06-03 10:52:52",
        "facebook": null,
        "twitter": null,
        "google": "",
        "linkedin": null,
        "is_customer": false,
        "is_supplier": false
    },
    "Address": [

    ],
    "Mail": [
        {
            "id": "189",
            "contact_id": "717",
            "label": "ufficio",
            "value": "luigi.bianchi@email.it"
        }
    ],
    "Field": [

    ],
    "Phone": [

    ]
}
```

Dopo che un contatto è stato creato, è possibile modificarlo indicando il suo id.


### HTTP Request

`PUT /api/contacts/{id}`

Tutti i dati passati nel corpo della richiesta verranno elaborati per modificare l'oggetto **Contact** e le sue associazioni.

### Cosa inviare 

Tutti i campi dell'oggetto **Contact** che vengono inviati nel corpo della richiesta sovrascriveranno quelli già memorizzati per il contatto. Per i campi non inviati verranno invece conservati i valori già memorizzati. Non è quindi necessario inviare tutti i campi dell'**Contact** ma solamente quelli che si intende memorizzare.

Per modificare una degli oggetti associati **Address**, **Mail**, **Phone** o **Field** è necessario inviare un array di questi oggetti. Si noti che inviando un array di nuovi oggetti, tutti gli oggetti associati precedentemente memorizzati verranno eliminati.

Ad esempio, se un Contatto possiede due numeri di telefono, inviando un nuovo array di **Phone** questi due vecchi verranno eliminati e verranno memorizzati quelli nuovi appena inviati.

Se si vuole mantenere i vecchi oggetti associati è necessario inviare nell'array gli id dei vecchi oggetti che si vuole conservare. Per aggiungere un numero di telefono ad un contatto, ad esempio, si può inviare al server gli id dei due numeri preesitenti più quello nuovo.

> Esempio. Richiesta di aggiunta di un nuovo numero di telefono a un Contatto con già due numeri di telefono:

```http
PUT /api/contacts/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/contacts/{id}
	-X PUT
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

```json
{
	"Contact": {...},
	"Phone": [
		{
			"id": 188
		},
		{
			"id": 189
		},
		{
	       "contact_id": "717",
	       "label": "ufficio",
	       "value": "luigi.bianchi@email.it"
		}
   ]
}
```



## Elimina Contatto

> Richiesta di esempio:

```http
DELETE /api/contacts/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/contacts/{id}
	-X DELETE
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta:

```json
{
    "code": "200",
    "message": "Contatto Eliminato Correttamente"
}
```

Elimina definitivamente un singolo Contatto.
Non si può annullare questa azione.

Eliminando il contatto verranno anche eliminati gli oggetti associati **Address**, **Mail**, **Phone**, **Field**.

### HTTP Request

`DELETE /api/contacts/{id}`

### Restituisce 

Restituisce un oggetto indicante un **code** 200 in caso di successo.

In caso di errore, come ad esempio per un id inesistente, viene restituito un [Errore](#error).


## Ottieni tutti i Contatti

> Richiesta di esempio:

```http
GET /api/contacts?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/contacts
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta di esempio:

```json
[
    {
        "Contact": {
            "id": "714",
            "name": "Mario",
            "last_name": "Rossi",
            "prefix": null,
            "suffix": null,
            "middle": null,
            "nickname": null,
            "job": null,
            "company": null,
            "is_natural_person": true,
            "fiscal_code": "RSSMR...",
            "vat_number": null,
            "avatar_id": null,
            "notes": "",
            "birthday": null,
            "modified": "2015-06-01 14:54:55",
            "created": "2015-05-16 08:45:12",
            "facebook": null,
            "twitter": null,
            "google": "",
            "linkedin": null,
			  "is_customer": false,
            "is_supplier": false
        },
        "Address": [
            {
                "id": "193",
                "contact_id": "714",
                "street": "Via di Prova",
                "city": "Roma",
                "province": null,
                "zip": null,
                "country": null,
                "title": "Residenza"
            }
        ],
        "Mail": [
            {
                "id": "187",
                "contact_id": "714",
                "label": "casa",
                "value": "0111"
            }
        ],
        "Field": [
        ],
        "Phone": [
        ]
    },
    {...}
]
```

E' possibile ottenere tutti i contatti memorizzati.

I contatti sono elecanti per data di creazione, con il più recente per primo.

### HTTP Request

`GET /api/contacts`

### Restituisce 

In caso di successo viene restituito un array di oggetti. 

Ogni oggetto contiene al suo interno un **Contact** e le sue associazioni.

In caso non ci sia nessun Contatto memorizzato da mostrare viene restituito un array vuoto.
