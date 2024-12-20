# Ghid de verificare a semnăturii

---

## Descriere

Acest document descrie procesul de verificare a semnăturii datelor primite de la serviciul eComm în format JSON. Pentru
semnare este utilizat algoritmul RSA, iar semnătura este creată folosind o cheie privată. Comercianții pot verifica
autenticitatea datelor utilizând cheia publică, care este furnizată separat.

---

## Pașii pentru verificarea semnăturii

### 1. Primirea JSON-ului cu rezultatele plății

După procesarea plății, serviciul eComm trimite un JSON cu rezultatele plății către `callbackUrl`, specificat de
comerciant la inițierea plății.

**Exemplu JSON:**

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

### 2. Obținerea cheii publice

Cheia publică poate fi obținută prin următoarea solicitare către serviciul eComm:

- **Metodă:** `GET`
- **Endpoint:** `/api/v1/public-key`

**Exemplu de răspuns:**

```json
{
  "publicKey": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAr1f/yhw+UI//z3KdpnJz..."
}
```

---

### 3. Crearea unui șir pentru verificarea semnăturii

Pentru a crea un șir utilizat pentru verificarea semnăturii, urmați pașii de mai jos:

1. Extrageți obiectul `result` din JSON.
2. Sortează toate câmpurile obiectului `result` în ordine alfabetică.
3. Concatenează valorile tuturor câmpurilor folosind `;` ca separator.

**Exemplu rezultat:**

```
145.25;MDL;2024-05-20T16:32:28+03:00;order123;bc340d13-7411-4785-a083-b594b1384eb5;SUCCESS;swift123;SomeBank;123456
```

---

### 4. Verificarea semnăturii

Pentru verificarea semnăturii, utilizați cheia publică și algoritmul RSA. Comparați semnătura calculată cu cea oferită
în câmpul `signature`.

---

## Exemple de cod

### Exemplu în Java

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

### Exemplu în Python

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

## Concluzie

Folosind pașii și exemplele de cod descrise mai sus, puteți verifica autenticitatea datelor primite de la serviciul
eComm și vă puteți asigura că acestea au fost semnate utilizând cheia privată a serviciului eComm. Aceasta garantează că
datele nu au fost modificate sau falsificate.
