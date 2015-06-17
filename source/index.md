---
title: Adamo API Reference

language_tabs:
  - shell
  - http

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - contacts
  - products
  - invoices
  - arrivals
  - arrangements
  - payments
  - emails
  - uploads
  - reports  
  - errors

search: true
---

# Introduzione

Benvenuto nella documentazione di Adamo API! Puoi usare le API di Adamo per accedere ai diversi endpoints, dove potrai creare, modificare e ottenere informazioni sui diversi oggetti memorizzati dentro Adamo.

Le risorse in questa guida ti permetteranno di sviluppare velocemente la tua applicazione.

Le API di Adamo sono organizzate intorno a <a href="http://it.wikipedia.org/wiki/Representational_State_Transfer">REST</a>. Le API sono disegnate per essere intuitive, con URL orientati alle risorse e con i codici HTTP utilizzati per indicare eventuali errori. 

Tutte le richieste devono essere passate per HTTPS.

Le richieste e le risposte utilizzano JSON per formattare le risorse, inclusi gli errori.

Per qualsiasi dubbio puoi scrivere a <a href="mailto:info@adamogestionale.it">info@adamogestionale.it</a>.

## Registra la tua app

# Autenticazione

Le API di Adamo supportano il protocollo OAuth 2.0 per autorizzare applicazioni di terze parte a connettersi all'account di un utente senza utilizzare username e password.

Come sviluppatore devi registrare la tua applicazione su Adamo prima di usare il flusso OAuth.

Seguendo i passi qui sotto puoi ottenere un *access_token* per la tua web app usando OAuth 2.0. Un access token permette alla tua app di compiere delle richieste per l'utente.

Si consiglia di visionare il protocollo <a href="http://oauth.net/2/">OAuth 2.0</a> per altre informazioni.


## Ottieni un Authorization Code

Dalla tua web app, reindirizza l'utente di cui vorresti l'accesso al link:

`GET https://app.adamogestionale.it/oauth/authorize?response_type=code&client_id={client_id}&redirect_uri={redirect_uri}`

Parameter | Description
--------- |  -----------
client_id | il client ID ricevuto quando hai registrato la tua app su Adamo.
response_type | Il tipo di risposta. Per un flusso web, utilizzare *code*.
redirect_uri | La URL url-encoded a cui sarà reindirizzato l'utente dopo aver accettato di fornire alla tua app l'autorizzazione a procedere.

### Response

L'utente vedrà una schermata di autorizzazione. 

Accettando, darà alla tua app l'autorizzazione a accedere agli endpoint per conto suo.

Se rifiuta, l'utente verrà reindirizzato al tuo sito con un codice d'errore.

## Adamo reindrizza l'utente al tuo sito

Se l'utente accetta la richiesta, viene reindirizzato al tuo sito con un codice `code` nella querystring.

`https://{redirect_uri}/?code={auth_code}`

Questo codice ha una validità di 30 secondi, dopodiché non potrà più essere utilizzato.

## Scambia l'Auth code con un Access Token

Dopo aver ricevuto il `code` dovrai scambiarlo con il server per ottenere un `access_token`. 

Fai una richiesta all'indirizzo:

`GET https://app.adamogestionale.it/oauth/token?grant_type=authorization_code&code={auth_code}&client_id={client_id}&client_secret={client_secret}`

Parameter | Description
--------- |  -----------
client_id | il client ID ricevuto quando hai registrato la tua app su Adamo.
auth_code |L'auth code ricevuto.
client_secret | il client_secret ricevuto quando hai registrato la tua app su Adamo.


### Restituisce

> Risposta:

```json
{ 
	"access_token": "{access_token}",
	"expires_in": 31104000,
	"token_type": "bearer",
	"refresh_token": "{refresh_token}"
}
```

La risposta include una stringa JSON che contiene l'access_token

`{"access_token": "{access_token}"}`


Devi salvare l'*access_token* fornito per qualsiasi compiere qualsiasi richiesta per l'utente.

Il parametro *expire_in*, restituito con la richiesta indica per quanto tempo (in secondi) sarà valido quel token. Il token ha una durata di un anno.


## Utilizza l'Access Token

Il Token deve essere incluso in ogni richiesta API. Questo può essere fatto in query string:

`curl https://app.adamogestionale.it/api/invoices?access_token={access_token}`

Oppure come HTTP Header:

`curl	-H "Authorization: Bearer {access_token}"`

# Api

## Endpoint

Quando si ha un *access_token* si può interagire con le API di Adamo per conto dell'utente che ha concesso l'autorizzazione e a cui è associato quel *access_token*.

Gli endpoint, cioè gli indirizzi, a cui puoi collegarti sono elencati in questo documento. Tutte le richieste devono venire fatte via JSON attraverso HTTPS.

## Schema

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

> Risposta di esempio:

```json
{
    "id": {
        "type": "integer",
        "null": false,
        "default": null,
        "length": 10,
        "key": "primary"
    },
    "name": {
        "type": "string",
        "null": true,
        "default": null,
        "length": 100,
        "collate": "latin1_swedish_ci",
        "charset": "latin1"
    },
    {...}
}
```

Con il termine Oggetti intendiamo un gruppo di informazioni. Ad esempio una fattura o un contatto sono considerati oggetti.

Tutti gli oggetto su Adamo hanno una propria struttura, chiamata *schema*.

Per conoscere lo schema di un determinato oggetto è possibile fare una richiesta

`GET https://app.adamogestionale.it/api/{object}/schema`

In questo modo verrà restituito un oggetto che elenca tutti gli attributi e il loro tipo.

