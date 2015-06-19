<a name="Error"></a>
# Errori

> Errore di esempio:

```json
{
    "name": "rate limit exceeded",
    "url": "\/api\/invoices?access_token={token}"
}
```

In caso di errore vengono restituiti uno dei seguenti codici come *status code* della richiesta HTTP.

Codice | Significato
---------- | -------
400 | Bad Request -- La tua richiesta non può essere accettata
401 | Unauthorized -- Le tue credenziali non sono valide
403 | Forbidden -- Non sei autorizzato ad accedere a questo endpoint
404 | Not Found -- Non è possibile trovare l'indirizzo richiesto
405 | Method Not Allowed -- Il metodo che hai utilizzato non è valido
406 | Not Acceptable -- Hai richiesto un formato che non è json
410 | Gone -- The kitten requested has been removed from our servers
429 | Too Many Requests -- Troppe richieste in poco tempo
500 | Internal Server Error -- Si è verificato un problema con il nostro server
503 | Service Unavailable -- Siamo temporaneamente in manutenzione.

In alcuni casi, quando è possibile fornire una descrizione dell'errore, viene inviato un oggetto d'errore insiema alla risposta.

Questo comprende un attributo *name* che indica la descrizione dell'errore e l'*url* che si è tentato di accedere.

