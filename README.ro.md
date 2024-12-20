# Integrarea cu MIA POS eComm

**MIA POS eComm** este un sistem modern de plată care permite comercianților să accepte plăți pentru produse și servicii prin intermediul codurilor QR și a solicitărilor directe de plată (*Request to Pay*). Sistemul utilizează un protocol sigur și flexibil pentru procesarea tranzacțiilor și este integrat cu API-ul MIA POS pentru a oferi o experiență simplă și eficientă atât comercianților, cât și clienților.

---

## Funcționalități cheie ale MIA POS eComm

- **Plăți prin cod QR**: Oferă clienților posibilitatea de a efectua plăți rapide și sigure prin scanarea unui cod QR.
- **Request to Pay (RTP)**: Permite comercianților să inițieze solicitări directe de plată către clienți.
- **Callback-uri securizate**: Rezultatele tranzacțiilor sunt trimise comercianților prin callback-uri semnate digital.
- **Integrare flexibilă**: Protocolul poate fi utilizat cu diverse platforme de eCommerce și sisteme personalizate.

---

## Soluții gata făcute

Pentru diferite platforme de eCommerce, sunt disponibile soluții care simplifică procesul de integrare cu MIA POS. Consultați soluțiile de mai jos:

- [MIA Pay Ecomm PHP SDK](https://github.com/finergy-tech/mia-pay-ecomm-php-sdk) - SDK pentru PHP.
- [MIA Pay Gateway pentru CsCart](https://github.com/finergy-tech/mia-pay-gateway-for-cscart).
- [MIA Pay Gateway pentru OpenCart](https://github.com/finergy-tech/mia-pay-gateway-for-opencart).
- [MIA Pay Gateway pentru WooCommerce](https://github.com/finergy-tech/mia-pay-gateway-for-woocommerce).

Aceste soluții includ funcționalități predefinite pentru interacțiunea cu API-ul MIA POS și sunt configurate pentru a gestiona toate fluxurile necesare (generare token-uri, procesarea plăților, callback-uri etc.).

---

## Demo

Pentru a vedea cum funcționează plata cu MIA POS eComm, accesați: [https://ecomm-test.miapos.md/demo-page.html](https://ecomm-test.miapos.md/demo-page.html).

Instrucțiuni de utilizare:
1. În câmpul `URL redirecționare după plată cu succes`, introduceți URL-ul unde clientul va fi redirecționat după o plată reușită.
2. În câmpul `URL redirecționare după plată eșuată`, introduceți URL-ul unde clientul va fi redirecționat după o plată eșuată.
3. În câmpul `URL notificare (callbackUrl)`, introduceți URL-ul unde MIA POS va trimite rezultatul semnat al tranzacției.
4. Introduceți suma în câmpul `Suma achiziției` și completați restul informațiilor despre plată.
5. Apăsați pe butonul **Plătește cu MIA**.
6. În cazul unui proces reușit, clientul va fi redirecționat către URL-ul de checkout. Plata QR va fi procesată automat în 30 de secunde. După finalizare, clientul va vedea un ecran de confirmare și va fi redirecționat către URL-ul de succes.

> **Notă:** Această pagină este doar pentru demonstrație. Nu utilizați `merchantId` sau `secretKey` direct pe client!

---

## Documentație

- [Prezentare generală a protocolului](./docs/ro/protocol-overview.md)
- [Ghid de verificare a semnăturii](./docs/ro/signature-verification.md)

---

## Contacte

- Website: [Finergy Tech](https://finergy.md)
- Email: [info@finergy.md](mailto:info@finergy.md)
