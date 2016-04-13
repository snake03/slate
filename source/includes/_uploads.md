<a name="Upload"></a>
# Files

In diverse occasioni si può voler caricare su Adamo dei file, ad esempio quando si vuole allegare ad una spesa una fattura in pdf. 

Questo può essere fatto creando un oggetto **Upload**. 
Quando viene caricato online un file, le API di Adamo rispondono con un oggetto comprendente le informazioni sul file appena caricato.  

## Caricare un file

> Richiesta di esempio: 

```http
POST /api/uploads?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="path/to/file.png"
Content-Type: image/png
```

```shell
$ curl 
http://app.adamogestionale.it/api/uploads
	-X POST
	-H "Authorization: Bearer {access_token}"
	-F "file= path/to/file.png"
```

> Risposta di esempio: 

```json
{
    "Upload": {
        "id": 180,
        "real_name": "254\/aaac215441e3946158394d78d.png",
        "name": "file.png",
        "size": 191896,
        "created": "2015-06-06 15:49:34",
        "modified": "2015-06-06 15:49:34",
        "mime": "image\/png",
        "docket_id": null,
        "revision_of": null,
        "updated_by": 153,
        "public": false
    }
}
```

Per caricare un file è necessario inviare una richiesta `multipart/form-data` all'endpoint:

`POST /api/uploads`

### Restituisce

In caso di successo verrà restituito un oggetto contente un oggetto **Upload**. Gli attributi dell'oggetto sono:
 
Attributo | Tipo | Descrizione
--------- | ---| --------
id | Integer | 
real_name | String  (*read-only*) | Percorso del file pubblicamente accessibile se *public* è *true*
name | String | Nome del file assegnato dall'utente
size | Integer  (*read-only*) | Peso del files in bytes
mime | String (*read-only*) | Tipo di file
docket_id | Integer | Id della cartella del file
public | Boolean. Default *false* | Se *true* può essere visibile a chiunque acceda al percorso *realName*.

## Modificare un file

> Richiesta di modifica di un file:

```http
PUT /api/uploads/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
```

```shell
$ curl 
http://app.adamogestionale.it/api/uploads/{id}
	-X PUT
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

```json
{
	"Upload": {
   		 "name":"filename.png",
   		 "public":true
    }
}
```

> Risposta di esempio:

```json
{
    "Upload": {
        "id": 172,
        "real_name": "254\/1a87404acf3cd3b240952c4a7.png",
        "name": "filename.png",
        "size": 1164408,
        "created": "2015-05-30 10:17:01",
        "modified": "2015-06-06 16:43:18",
        "mime": "image\/png",
        "docket_id": null,
        "revision_of": null,
        "updated_by": 153,
        "public": true
    }
}
```

Dato l'id del file puoi modificare alcuni dei suoi attributi. 

Attributo | Tipo 
--------- | ---
name | String 
docket_id | Integer 
public | Boolean

### Restituisce

In caso di successo viene restituito un oggetto contenente un oggetto **Upload**. 

In caso di errore viene restituito un [Errore](#Errore). 

<a name="upload-download"></a>
##Scaricare un file

> Richiesta di download di un file:

```http
GET /api/uploads/download/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
```

```shell
$ curl 
http://app.adamogestionale.it/api/uploads/download/{id}
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

E' possibile scaricare un file accedendo al suo indirizzo pubblico se l'attributo è false.

Altrimenti è possibile scaricarlo accedendo all'endpoint

`GET /api/uploads/download/{id}`

### Restituisce

In caso di successo restituisce il file nel formato `mime`.

##Visualizzare un file

> Richiesta di download di un file:

```http
GET /api/uploads/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
```

```shell
$ curl 
http://app.adamogestionale.it/api/uploads
	-X POST
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```
> Risposta di esempio:

```json
{
    "Upload": {
        "id": 164,
        "real_name": "254\/8358e0673233389a410379fec.pdf",
        "name": "file.pdf",
        "size": 36155,
        "created": "2015-01-18 21:42:00",
        "modified": "2015-01-18 21:42:00",
        "mime": "application\/pdf",
        "docket_id": null,
        "revision_of": null,
        "updated_by": 153,
        "public": false
    }
}
```
 
Dato l'id di un oggetto **Upload** è possibile visualizzare tutte le informazioni su quest'oggetto.

Chiamando l'endpoint:

`GET /api/uploads/{id}`

Viene restituito un oggetto contente un oggetto **Upload** in caso di successo, altrimenti viene restituito un [Errore](#Errore).

Per ottenere il file puoi [scaricare il file](#upload-download)

##Eliminare un file

> Richiesta di eliminazione di un file:

```http
DELETE /api/uploads/{id}?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
```

```shell
$ curl 
http://app.adamogestionale.it/api/uploads/{id}
	-X DELETE
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta di esempio:

```json
{
    "code": "200",
    "message": "File eliminato correttamente"
}
```

Per eliminare un file è necessario rivolgere una richiesta all'endpoint.

`DELETE /api/uploads/{id}`

In questo modo verrà eliminato l'oggetto **Upload** e cancellare in maniera definitiva il file dal sistema. 
Non si può annullare questa azione.

### Restituisce

Restituisce un oggetto indicante code 200 in caso di successo, un [Errore](#Errore) altrimenti.

## Ottenere tutti i file

> Richiesta di eliminazione di un file:

```http
GET /api/uploads/?access_token={access_token} HTTP/1.1
Host: app.adamogestionale.it
Content-Type: application/json
```

```shell
$ curl 
http://app.adamogestionale.it/api/uploads/
	-X GET
	-H "Authorization: Bearer {access_token}"
	-H "Content-Type: application/json"
```

> Risposta di esempio:

```json
[
    {
        "Upload": {
            "id": 172,
            "real_name": "254\/1a87404acf3cd3b240952c4a7.png",
            "name": "test",
            "size": 1164408,
            "created": "2015-05-30 10:17:01",
            "modified": "2015-06-06 16:43:18",
            "mime": "image\/png",
            "docket_id": null,
            "revision_of": null,
            "updated_by": 153,
            "public": true
        }
    },
    {...}
]
```
    
Per ottenere una lista di tutti i file presenti si può accedere a questo endpoint:

`GET /api/uploads`

In caso di successo verrà restituito un array di oggetti **Upload**. Se nessun **Upload** è stato trovato allora verrà restituito un array vuoto.