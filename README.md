# Integration with MIA POS eComm

**MIA POS eComm** is a modern payment system that allows merchants to accept payments for goods and services via QR codes and direct payment requests (*Request to Pay*). The system uses a secure and flexible protocol for processing transactions, ensuring a smooth and efficient experience for both merchants and customers.

---

## Key Features of MIA POS eComm

- **QR Code Payments**: Enables customers to make fast and secure payments by scanning a QR code.
- **Request to Pay (RTP)**: Allows merchants to initiate direct payment requests to customers.
- **Secure Callbacks**: Transaction results are sent to merchants through digitally signed callback notifications.
- **Flexible Integration**: The protocol can be used with various eCommerce platforms and custom systems.

---

## Ready-Made Solutions

For popular eCommerce platforms, there are ready-made solutions that simplify the integration process with MIA POS. Explore the solutions below:

- [MIA Pay Ecomm PHP SDK](https://github.com/finergy-tech/mia-pay-ecomm-php-sdk) — PHP SDK.
- [MIA Pay Gateway for CsCart](https://github.com/finergy-tech/mia-pay-gateway-for-cscart).
- [MIA Pay Gateway for OpenCart](https://github.com/finergy-tech/mia-pay-gateway-for-opencart).
- [MIA Pay Gateway for WooCommerce](https://github.com/finergy-tech/mia-pay-gateway-for-woocommerce).

These solutions include preconfigured functionality for interacting with the MIA POS API and are designed to handle all necessary flows (token generation, payment processing, callback notifications, etc.).

---

## Demo

To see how payments via MIA POS eComm work, visit: [https://ecomm-test.miapos.md/demo-page.html](https://ecomm-test.miapos.md/demo-page.html).

Usage instructions:
1. In the `URL redirecționare după plată cu succes` field, enter the URL where the customer will be redirected after a successful payment.
2. In the `URL redirecționare după plată eșuată` field, enter the URL where the customer will be redirected after a failed payment.
3. In the `URL notificare (callbackUrl)` field, enter the URL where MIA POS will send the signed transaction result.
4. Enter the amount in the `Suma achiziției` field and fill in the other payment details.
5. Click the **Plătește cu MIA** button.
6. If the requests are successful, the customer will be redirected to the checkout URL. The QR code payment will be processed automatically within 30 seconds. After completion, the customer will see a success screen and will be redirected to the merchant's success URL.

> **Important:** This page is for demonstration purposes only. Never use `merchantId` or `secretKey` on the client side!

---

## Documentation

- [Protocol Overview](./docs/en/protocol-overview.md)
- [Signature Verification Guide](./docs/en/signature-verification.md)

---

## Contact

- [Finergy Tech Website](https://finergy.md)
- Email: [info@finergy.md](mailto:info@finergy.md)
