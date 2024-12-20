# Интеграция с MIA POS eComm

**MIA POS eComm** — это современная платежная система, позволяющая мерчантам принимать оплату за товары и услуги через QR-коды и запросы на оплату (*Request to Pay*). Система использует безопасный и гибкий протокол для обработки транзакций, обеспечивая удобный и быстрый процесс как для продавцов, так и для покупателей.

---

## Ключевые возможности MIA POS eComm

- **Оплата через QR-коды**: Позволяет клиентам быстро и безопасно оплачивать товары, сканируя QR-код.
- **Request to Pay (RTP)**: Даёт возможность мерчантам отправлять клиентам запросы на оплату.
- **Безопасные callback-уведомления**: Результаты транзакций отправляются мерчантам через подписанные callback-уведомления.
- **Гибкая интеграция**: Протокол может быть использован с различными платформами eCommerce и кастомными системами.

---

## Готовые решения

Для популярных eCommerce платформ доступны готовые решения, которые упрощают процесс интеграции с MIA POS. Ознакомьтесь с решениями ниже:

- [MIA Pay Ecomm PHP SDK](https://github.com/finergy-tech/mia-pay-ecomm-php-sdk) — SDK для PHP.
- [MIA Pay Gateway для CsCart](https://github.com/finergy-tech/mia-pay-gateway-for-cscart).
- [MIA Pay Gateway для OpenCart](https://github.com/finergy-tech/mia-pay-gateway-for-opencart).
- [MIA Pay Gateway для WooCommerce](https://github.com/finergy-tech/mia-pay-gateway-for-woocommerce).

Эти решения включают в себя предопределённые функции для работы с API MIA POS и настроены для обработки всех необходимых потоков (генерация токенов, обработка платежей, callback-уведомления и т.д.).

---

## Demo

Чтобы посмотреть, как работает оплата с помощью MIA POS eComm, перейдите по ссылке: [https://ecomm-test.miapos.md/demo-page.html](https://ecomm-test.miapos.md/demo-page.html).

Инструкция по использованию:
1. В поле `URL redirecționare după plată cu succes` укажите URL, куда клиент будет перенаправлен после успешной оплаты.
2. В поле `URL redirecționare după plată eșuată` укажите URL, куда клиент будет перенаправлен в случае ошибки.
3. В поле `URL notificare (callbackUrl)` укажите URL, куда MIA POS отправит подписанный результат транзакции.
4. Укажите сумму в поле `Suma achiziției` и заполните остальную информацию о платеже.
5. Нажмите кнопку **Plătește cu MIA**.
6. В случае успешного выполнения запросов клиент будет перенаправлен на URL checkout. Оплата QR-кода будет обработана автоматически через 30 секунд. После завершения клиент увидит экран успешной оплаты и будет перенаправлен на success URL.

> **Важно:** Эта страница носит исключительно демонстрационный характер. Никогда не используйте `merchantId` или `secretKey` на стороне клиента!

---

## Документация

- [Общее описание протокола](./docs/ru/protocol-overview.md)
- [Инструкция по проверке подписи](./docs/ru/signature-verification.md)

---

## Контакты

- Сайт: [Finergy Tech](https://finergy.md)
- Email: [info@finergy.md](mailto:info@finergy.md)