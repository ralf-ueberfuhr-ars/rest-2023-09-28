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
  - 200
    - Body: Liste der Kundendaten ohne Bezahldaten

### Einzelnen Kunden auslesen

- Request
    - GET /customers/{id}
- Responses
  - 200
    - Body: Kundendaten des Kunden ohne Bezahldaten
  - 404 - wenn Kunde mit dieser ID nicht existiert
  - 400 - wenn ID keine Zahl ist

### Kunde anlegen

- Request
  - POST /customers
  - Body: Customer ohne ID
- Responses
  - 201
    - Location-Header
    - Body: Customer mit ID
  - 400 - Validierungs/Syntaxfehler (u.a. Emailadresse bereits vorhanden)
    
### Kunde überschreiben

- Request
  - PUT /customers/{id}
  - Body: Customer ohne ID
- Responses
  - 200
    - Body: Customer mit ID
  - 404 - Kunde mit ID nicht gefunden
  - 400 - Validierungs/Syntaxfehler (u.a. Emailadresse bereits vorhanden, ID ist keine Zahl)

### Kunde löschen

- Request
  - DELETE /customers/{id}
- Responses
  - 204 (kein Body)
  - 404 - Kunde mit ID nicht gefunden
  - 400 - ID ist keine Zahl

### Spezialfälle:

#### (De-)Aktivieren eines Kunden-Accounts
- einfache Lösung: Status als Teil des Schemas
  - Customer(state) -> PATCH /customers/{id}
- Aktivitätsressource
  - POST /customers/{id}/actions/deactivation
  - POST /customers/{id}/actions/activation
- Flag
  - PUT /customers/{id}/flags/deactivation
  - DELETE /customers/{id}/flags/deactivation
  - GET /customers/{id}/flags/deactivation

  - Kreditkarte/Paypal zuweisen (je 1x)
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


