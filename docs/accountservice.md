# AccountService

## Daten
- ID/username
- Name
- Geburtsdatum
- Anschrift
- Bezahlinformationen
  - Kreditkarte
  - PayPal
- Status (aktiviert, deaktiviert, gesperrt)

## Operationen   

Angenommen wird JSON als alleiniges Austauschformat. Entsprechende `Accept` und `Content-Type`
Header sind daher nicht explizit erwähnt

### Alle Kunden auslesen

- Request
  - GET /customers
- Responses
  - 200 OK
    - Body: Liste der Kundendaten ohne Bezahldaten

### Einzelnen Kunden auslesen

- Request
    - GET /customers/{id}
- Responses
  - 200 OK
    - Body: Kundendaten des Kunden ohne Bezahldaten
  - 404 - wenn Kunde mit dieser ID nicht existiert
  - 400 - wenn ID keine Zahl ist

### Kunde anlegen

REQ
POST
/customers
Body: Customer-Objekt
RESP
Erfolgsfall:
201 CREATED + Location-Header
Body: angelegtes Customer-Objekt inkl. ids
Fehlerfälle
400 Bad Request:
Body: Error-Objekt mit Hinweisen
Wann
Kunde (Name, Vorname, Datum, E-Mail Adresse) bereits vorhanden
Syntax falsch
409 CONFLICT
Body: Error-Objekt mit Hinweisen
Wann
E-Mail-Adresse existiert bereits
<Kunde überschreiben>
REQ
PUT
/customers/{id}
Body: Customer-Object
RESP
Erfolgsfall:
200 OK
Body: Kundendaten ohne Bezahldaten
Fehlerfall:
404 - not found with id
400
<Kunde löschen>
REQ
DELETE
/customers/{ID}
RESP
Erfolgsfall:
204 No Content
Fehlerfall:
400 - false ID
404 - ID not found


Spezialfälle:
(De-)Aktivieren eines Kunden-Accounts
- Customer(state) -> PATCH /customers/{id}
- POST /customers/{id}/actions/deactivation
  POST /customers/{id}/actions/activation
- PUT /customers/{id}/flags/deactivation
  DELETE ...
  GET
  Kreditkarte/Paypal zuweisen (je 1x)
- Customer(paypal, cc) -> PATCH /customers/{id}
- PUT /customers/{id}/payments/paypal
- PUT /customers/{id}/payments/credit-card
  Herausfinden, ob Kundenaccount aktiviert ist
  Kunde mit Adresse vs. Addresse als Subressource
  Kunden auslesen mit 1Mio Datensätzen?
- GET /customers?start= .... & limit= ... / OData
  Kunden mit und ohne Bezahldaten auslesen?
- GET /customers -> Kunden ohne Bezahldaten
  GET /customers/1/payments
  GET /customers/2/payments
  ....
- GET /customers-with-payments
  GET /customers?include-payments
  Semantik von PUT "/customers" ? -> Bulk Operations


