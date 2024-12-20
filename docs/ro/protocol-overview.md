# Prezentare generală a protocolului MIA POS eComm

Acest document descrie în detaliu modul de funcționare a protocolului **MIA POS eComm** și pașii necesari pentru integrarea acestuia în sistemele comercianților.

---

## Documentație suplimentară

- [Documentația API (descărcați și deschideți în browser)](../mia-ecomm-api_v0.0.1.html)
- [Ghid de verificare a semnăturii](./signature-verification.md)

---

## Fluxul plății

### **1. Configurarea inițială**

Pentru a începe integrarea cu MIA POS eComm, comerciantul trebuie să configureze următorii parametri în sistemul său:

- **merchantId**: ID-ul comerciantului (unic pentru fiecare comerciant).
- **secretKey**: Cheia secretă pentru autentificare.
- **terminalId**: ID-ul terminalului utilizat pentru procesarea plăților.
- **language**: Limba interfeței de plată (de exemplu: `ro`, `en`, `ru`).
- **paymentType**: Tipul plății (`qr` pentru QR code sau `rtp` pentru Request to Pay).
- **ecommBaseUrl**: URL-ul de bază pentru API-ul MIA POS (ex. `https://ecomm-test.miapos.md`).

---

### **2. Colectarea informațiilor despre comandă**

Înainte de inițierea procesului de plată, comerciantul trebuie să colecteze următoarele informații despre comandă:

- **orderId**: Un identificator unic pentru comandă (generat de comerciant).
- **amount**: Suma plății (în format decimal, ex. `100.00`).
- **currency**: Moneda utilizată (ex. `MDL`).
- **payDescription**: Descrierea plății (ex. `Plata pentru comanda #123`).
- **clientName**: (Opțional) Numele clientului.
- **clientPhone**: (Opțional) Numărul de telefon al clientului.
- **clientEmail**: (Opțional) Adresa de email a clientului.

---

### **3. Inițierea plății**

Atunci când clientul apasă pe butonul „Plătește cu MIA” în magazinul online, se execută următorii pași prin intermediul backend-ului comerciantului:

#### **3.1. Obținerea token-ului de acces**

Dacă comerciantul nu are un `refreshToken`, trebuie să obțină un `accessToken` și un `refreshToken` utilizând următorul endpoint:

- **Tip:** `POST`
- **Endpoint:** `/ecomm/api/v1/token`

#### **3.2. Reîmprospătarea token-ului**

Dacă comerciantul are un `refreshToken`, poate obține un nou `accessToken` folosind următorul endpoint:

- **Tip:** `POST`
- **Endpoint:** `/ecomm/api/v1/token/refresh`

#### **3.3. Înregistrarea plății**

După obținerea `accessToken`, comerciantul trebuie să înregistreze plata utilizând următorul endpoint:

- **Tip:** `POST`
- **Endpoint:** `/ecomm/api/v1/pay`

Clientul este redirecționat la URL-ul `checkoutPage` pentru finalizarea plății.

---

### **4. Gestionarea rezultatelor plății**

#### **4.1. Plata nereușită**

Dacă plata eșuează, clientul este redirecționat către `failUrl`. În acest caz, comerciantul poate verifica statusul plății folosind următorul endpoint:

- **Tip:** `GET`
- **Endpoint:** `/ecomm/api/v1/payment/{paymentId}`

#### **4.2. Plata reușită**

Dacă plata este finalizată cu succes, clientul este redirecționat către `successUrl`. Comerciantul trebuie să verifice statusul plății folosind același endpoint:

- **Tip:** `GET`
- **Endpoint:** `/ecomm/api/v1/payment/{paymentId}`

---

### **5. Callback-uri securizate**

Sistemul MIA POS trimite rezultatele tranzacțiilor către `callbackUrl` sub forma unui request `POST`. Comerciantul trebuie să verifice semnătura utilizând cheia publică obținută de la:

- **Tip:** `GET`
- **Endpoint:** `/ecomm/api/v1/public-key`

Consultați [Ghidul de verificare a semnăturii](./signature-verification.md) pentru detalii.

---
