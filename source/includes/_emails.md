# Email

E' possibile visualizzare tutte le email inviate da Adamo e controllare se sono state visualizzate dal destinatario.

## Oggetto Email

```json
{
	  "id": 37,
	  "created": "2015-05-11 09:23:12",
	  "sender": "your_alias@email.com",
	  "receiver": [
	      "test@gmail.com"
	  ],
	  "subject": "La tua fattura #10",
	  "body": "",
	  "sent": "2015-05-11 09:24:00",
	  "seen": null
}
```
L'oggetto Email ha pochi atttributi al suo interno

Attributo | Tipo | Descrizione
--------- | ---| --------
id | Integer |
sender| String  (*read-only*) | Chi ha inviato questa email
receiver | Array | Uno o più indirizzi email destinatari
subject | String | Titolo
body | String | Corpo della email
sent | Datetime | Quando l'email è stata spedita
seen | Datetime | Quando questa email è stata visualizzata dal destinatario.

Si noti che dal momento della creazione dell'oggetto **Email** al suo invio può passare un piccolo periodo di tempo, generalmente inferiore al minuto.

Quando il primo dei destinatari visualizza l'email inviata per la prima volta, l'attributo *seen* verrà valorizzato con la data di quell'istante.

## Ottieni tutte le Email

> Richiesta di esempio:

```http
GET /api/emails?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl
https://app.adamogestionale.it/api/emails
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```
```json
[
    {
        "Email": {
            "id": 37,
            "created": "2015-05-11 09:23:12",
            "sender": "your_alias@email.com",
            "receiver": [
                "test@gmail.com"
            ],
            "subject": "La tua fattura #10",
            "body": "",
            "sent": "2015-05-11 09:24:00",
            "seen": null
        }
    },
    {...}
	]
```

Con questo endpoint si possono elencare tutte le email già inviate.

### HTTP Request

`GET /api/emails`

### Restituisce

In caso di successo viene restituito un array di oggetti.

Ogni oggetto contiene al suo interno un **Email**.

In caso non ci sia nessuna Email memorizzata da mostrare viene restituito un array vuoto.


## Ottieni una sola Email

> Richiesta di esempio:

```http
GET /api/emails/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl
https://app.adamogestionale.it/api/emails/{id}
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

```json
{
    "Email": {
        "id": 37,
        "created": "2015-05-11 09:23:12",
        "sender": "your_alias@email.com",
        "receiver": [
            "test@gmail.com"
        ],
        "subject": "La tua fattura, caro",
        "body": "",
        "sent": null,
        "seen": null
    }
}
```

Con questo endpoint si può ottenere una email dato il suo id.

### HTTP Request

`GET /api/emails/{id}`

### Restituisce

In caso di successo viene restituito un oggetto comprendente un **Email**.

In caso di errore, ad esempio se viene fornito un id non esistente, viene restituito un [Errore](#Errore).
