# Инструкция по проверке подписи

---

## Описание

Данный документ описывает процесс проверки подписи данных, полученных от eComm сервиса в формате JSON. Для подписи
используется алгоритм RSA, и подпись создаётся с помощью приватного ключа. Мерчант может проверить подлинность данных,
используя публичный ключ, который предоставляется отдельно.

---

## Шаги для проверки подписи

### 1. Получение JSON с результатами оплаты

После обработки платежа eComm сервис отправляет JSON с результатами оплаты на `callbackUrl`, указанный мерчантом при
инициации платежа.

**Пример JSON:**

```json
{
  "result": {
    "terminalId": "123456",
    "orderId": "order123",
    "paymentId": "bc340d13-7411-4785-a083-b594b1384eb5",
    "status": "SUCCESS",
    "amount": 145.25,
    "currency": "MDL",
    "paymentDate": "2024-05-20T16:32:28+03:00",
    "swiftMessageId": "swift123",
    "swiftPayerBank": "SomeBank"
  },
  "signature": "base64_encoded_signature"
}
```

---

### 2. Получение публичного ключа

Публичный ключ можно получить с помощью следующего запроса к eComm сервису:

- **Метод:** `GET`
- **Endpoint:** `/api/v1/public-key`

**Пример ответа:**

```json
{
  "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAr1f/yhw+UI//z3KdpnJz..."
}
```

---

### 3. Формирование строки для проверки подписи

Для формирования строки, которая будет использована для проверки подписи, выполните следующие шаги:

1. Извлеките объект `result` из JSON.
2. Отсортируйте все поля объекта `result` по алфавиту.
3. Конкатенируйте значения всех полей, используя `;` в качестве разделителя.

**Пример результата:**

```
145.25;MDL;2024-05-20T16:32:28+03:00;order123;bc340d13-7411-4785-a083-b594b1384eb5;SUCCESS;swift123;SomeBank;123456
```

---

### 4. Проверка подписи

Для проверки подписи используйте публичный ключ и алгоритм RSA. Сравните вычисленную подпись с предоставленной в поле
`signature`.

---

## Примеры кода

### Пример на Java

```java
import com.fasterxml.jackson.databind.ObjectMapper;

import java.security.KeyFactory;
import java.security.PublicKey;
import java.security.Signature;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;
import java.util.Map;
import java.util.TreeMap;
import java.util.stream.Collectors;

public class SignatureVerification {
    public static boolean verifySignature(String jsonString, String publicKeyPEM) {
        try {
            ObjectMapper objectMapper = new ObjectMapper();
            Map<String, Object> jsonMap = objectMapper.readValue(jsonString, Map.class);
            Map<String, Object> resultMap = (Map<String, Object>) jsonMap.get("result");
            String base64Signature = (String) jsonMap.get("signature");

            Map<String, Object> sortedMap = new TreeMap<>(resultMap);
            String signatureString = sortedMap.values().stream()
                    .map(Object::toString)
                    .collect(Collectors.joining(";"));

            byte[] decodedKey = Base64.getDecoder().decode(publicKeyPEM);
            X509EncodedKeySpec keySpec = new X509EncodedKeySpec(decodedKey);
            KeyFactory keyFactory = KeyFactory.getInstance("RSA");
            PublicKey publicKey = keyFactory.generatePublic(keySpec);

            Signature signature = Signature.getInstance("SHA256withRSA");
            signature.initVerify(publicKey);
            signature.update(signatureString.getBytes());

            byte[] signatureBytes = Base64.getDecoder().decode(base64Signature);
            return signature.verify(signatureBytes);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

}
```

### Пример на Python

```python
import json
import base64
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import padding
from cryptography.hazmat.primitives import serialization

def verify_signature(json_string, public_key_base64):
    try:
        json_data = json.loads(json_string)
        result = json_data['result']
        signature_base64 = json_data['signature']

        sorted_items = sorted(result.items())
        signature_string = ';'.join([str(value) for key, value in sorted_items])

        public_key = serialization.load_pem_public_key(
            f"-----BEGIN PUBLIC KEY-----\n{public_key_base64}\n-----END PUBLIC KEY-----".encode()
        )

        signature = base64.b64decode(signature_base64)
        public_key.verify(
            signature,
            signature_string.encode(),
            padding.PKCS1v15(),
            hashes.SHA256()
        )
        return True
    except Exception as e:
        print(f"Signature verification failed: {e}")
        return False
```

---

## Заключение

С помощью описанных шагов и примеров кода вы можете проверять подлинность данных, полученных от eComm сервиса, и быть
уверенными, что они были подписаны с использованием приватного ключа. Это гарантирует, что данные не были изменены или
подделаны.
