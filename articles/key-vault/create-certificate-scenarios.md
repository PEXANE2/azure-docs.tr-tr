---
title: Sertifika oluşturmayı izleme ve yönetme
description: Anahtar Vault ile sertifika oluşturma işlemi oluşturmak, izlemek ve bunlarla etkileşimde bulunma için çeşitli seçenekler gösteren senaryolar.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: b9ff80275cc89dde0db215856c2e134c4b273020
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78199742"
---
# <a name="monitor-and-manage-certificate-creation"></a>Sertifika oluşturmayı izleme ve yönetme
Şu nasiması: Azure

Aşağıdaki 

Bu makalede özetlenen senaryolar / işlemler şunlardır:

- Desteklenen bir ihraççıyla KV Sertifikası isteyin
- Bekleyen istek alın - istek durumu "inProgress"
- Bekleyen istek alın - istek durumu "tamamlandı"
- Bekleyen istek alın - bekleyen istek durumu "iptal edildi" veya "başarısız"
- Bekleyen istek alın - bekleyen istek durumu "silinir" veya "üzerine yazılır"
- Bekleyen istek olduğunda oluşturma (veya Alma) - durum "inProgress"
- Bekleyen istek bir verenle oluşturulduğunda birleştirme (Örneğin DigiCert)
- Bekleyen istek durumu "inProgress" iken iptal isteyin
- Bekleyen istek nesnesi silme
- El ile KV sertifikası oluşturma
- Bekleyen bir istek oluşturulduğunda birleştirme - el ile sertifika oluşturma

## <a name="request-a-kv-certificate-with-a-supported-issuer"></a>Desteklenen bir ihraççıyla KV Sertifikası isteyin 

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|

Aşağıdaki örnekler, "mydigicert" adlı bir nesnenin digiCert olarak ihraççı sağlayıcıyla birlikte anahtar kasanızda zaten bulunmasını gerektirir. Sertifikayı veren kuruluş, Azure Key Vault'ta (KV) Sertifika Veren kaynağı olarak temsil edilen bir varlıktır. Bir KV sertifikasının kaynağı hakkında bilgi sağlamak için kullanılır; verenin adı, sağlayıcı, kimlik bilgileri ve diğer yönetim ayrıntıları.

### <a name="request"></a>İstek

```json
{
  "policy": {
    "x509_props": {
      "subject": "CN=MyCertSubject1"
    },
    "issuer": {
      "name": "mydigicert",
      "cty": "OV-SSL",
    }
  }
}
```

### <a name="response"></a>Yanıt

```
StatusCode: 202, ReasonPhrase: 'Accepted'
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "mydigicert"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "InProgress",
  "status_details": "Pending certificate created. Certificate request is in progress. This may take some time based on the issuer provider. Please check again later",
  "request_id": "a76827a18b63421c917da80f28e9913d"
}

```

## <a name="get-pending-request---request-status-is-inprogress"></a>Bekleyen istek alın - istek durumu "inProgress"

|Yöntem|İstek URI'si|
|------------|-----------------|
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

### <a name="request"></a>İstek
Al`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

Al`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

> [!NOTE]
> *Sorguda request_id* belirtilmişse, filtre gibi davranır. Sorgudaki ve bekleyen nesnedeki *request_id* farklıysa, 404'lük bir http durum kodu döndürülür.

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "inProgress",
  "status_details": "…",
  "request_id": "a76827a18b63421c917da80f28e9913d"
}

```

## <a name="get-pending-request---request-status-is-complete"></a>Bekleyen istek alın - istek durumu "tamamlandı"

### <a name="request"></a>İstek

|Yöntem|İstek URI'si|
|------------|-----------------|
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

Al`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

Al`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "completed",
  "request_id": "a76827a18b63421c917da80f28e9913d",
  "target": “https://mykeyvault.vault.azure.net/certificates/mycert1?api-version={api-version}"
}

```

## <a name="get-pending-request---pending-request-status-is-canceled-or-failed"></a>Bekleyen istek alın - bekleyen istek durumu "iptal edildi" veya "başarısız"

### <a name="request"></a>İstek

|Yöntem|İstek URI'si|
|------------|-----------------|
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

Al`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

Al`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "failed",
  "status_details": "",
  "request_id": "a76827a18b63421c917da80f28e9913d",
  "error": {
    "code": "<errorcode>",
    "message": "<message>"
  }
}

```

> [!NOTE]
> *Hata kodunun* değeri, sırasıyla veren veya kullanıcı hatasına dayalı olarak "Sertifika veren hatası" veya "Reddedilen istek" olabilir.

## <a name="get-pending-request---pending-request-status-is-deleted-or-overwritten"></a>Bekleyen istek alın - bekleyen istek durumu "silinir" veya "üzerine yazılır"
Bekleyen bir nesne, durumu "Devam Ediyor" olmadığında oluşturma/alma işlemi tarafından silinebilir veya üzerine yazılabilir.

|Yöntem|İstek URI'si|
|------------|-----------------|
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

### <a name="request"></a>İstek
Al`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

Al`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

### <a name="response"></a>Yanıt

```
StatusCode: 404, ReasonPhrase: 'Not Found'
{
  "error": {
    "code": "PendingCertificateNotFound",
    "message": "…"
  }
}

```

## <a name="create-or-import-when-pending-request-exists---status-is-inprogress"></a>Bekleyen istek olduğunda oluşturma (veya Alma) - durum "inProgress"
Bekleyen bir nesnenin dört olası durum vardır; "devam ediyor", "iptal edildi", "başarısız" veya "tamamlandı."

Bekleyen bir isteğin durumu "devam ediyor"sa, oluşturma (ve alma) işlemleri 409 (çakışma) http durum koduyla başarısız olur.

Çakışmayı gidermek için:

- Sertifika el ile oluşturuluyorsa, bekleyen nesne üzerinde birleştirme yaparak KV sertifikasını tamamlayabilir veya silebilirsiniz.

- Sertifika bir verenle oluşturuluyorsa, sertifika tamamlanana, başarısız olana veya iptal edilene kadar bekleyebilirsiniz. Alternatif olarak, bekleyen nesneyi silebilirsiniz.

> [!NOTE]
> Bekleyen bir nesneyi siler veya sağlayıcı ile x509 sertifika isteğini iptal olmayabilir.

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|

### <a name="request"></a>İstek

```json
{
  "policy": {
    "x509_props": {
      "subject": "CN=MyCertSubject1"
    },
    "issuer": {
      "name": "mydigicert"
    }
  }
}
```

### <a name="response"></a>Yanıt

```
StatusCode: 409, ReasonPhrase: 'Conflict'
{
  "error": {
    "code": "Forbidden",
    "message": "A new key vault certificate can not be created or imported while a pending key vault certificate's status is inProgress."
  }
}

```

## <a name="merge-when-pending-request-is-created-with-an-issuer"></a>Bekleyen istek bir verenle oluşturulduğunda birleştirme
Bekleyen bir nesne bir verenle oluşturulduğunda birleştirme izin verilmez, ancak durumu "Devam Etme" olduğunda izin verilir. 

x509 sertifikasını oluşturma isteği başarısız olursa veya herhangi bir nedenle iptal edilirse ve x509 sertifikası bant dışı yollarla alınabiliyorsa, KV sertifikasını tamamlamak için birleştirme işlemi yapılabilir.

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|

### <a name="request"></a>İstek

```json
{
  "x5c": [ "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8=" ]
}

```

### <a name="response"></a>Yanıt

```json
StatusCode: 403, ReasonPhrase: 'Forbidden'
{
  "error": {
    "code": "Forbidden",
    "message": "Merge is forbidden on pending object created with issuer : <issuer-name> while it is in progess."
  }
}

```

## <a name="request-a-cancellation-while-the-pending-request-status-is-inprogress"></a>Bekleyen istek durumu "inProgress" iken iptal isteyin
İptal sadece talep edilebilir. Bir istek iptal edilebilir veya iptal edilemez. Bir istek "inProgress" değilse, 400 (Kötü İstek) http durumu döndürülür.

|Yöntem|İstek URI'si|
|------------|-----------------|
|Yama|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

### <a name="request"></a>İstek
Yama`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

Yama`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

```json
{
  "cancellation_requested": true
}

```

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": true,
  "status": "inProgress",
  "status_details": "…",
  "request_id": "a76827a18b63421c917da80f28e9913d"
}
```

## <a name="delete-a-pending-request-object"></a>Bekleyen istek nesnesi silme

> [!NOTE]
> Bekleyen nesnenin silmesi sağlayıcıyla x509 sertifika isteğini iptal edebilir veya iptal edemeyebilir.

|Yöntem|İstek URI'si|
|------------|-----------------|
|DELETE|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|

### <a name="request"></a>İstek
Silmek`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`

OR

Silmek`“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`

### <a name="response"></a>Yanıt

```
StatusCode: 200, ReasonPhrase: 'OK'
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "{issuer-name}"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "cancellation_requested": false,
  "status": "inProgress",
  "request_id": "a76827a18b63421c917da80f28e9913d",
}
```

## <a name="create-a-kv-certificate-manually"></a>El ile KV sertifikası oluşturma
El ile oluşturma işlemi yle seçtiğiniz bir CA ile verilen bir sertifika oluşturabilirsiniz. Verenin adını "Bilinmiyor" olarak ayarlayın veya veren alanını belirtmeyin.

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|

### <a name="request"></a>İstek

```json
{
  "policy": {
    "x509_props": {
      "subject": "CN=MyCertSubject1"
    },
    "issuer": {
      "name": "Unknown"
    }
  }
}

```

### <a name="response"></a>Yanıt

```
StatusCode: 202, ReasonPhrase: 'Accepted'
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"
{
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",
  "issuer": {
    "name": "Unknown"
  },
  "csr": "MIICq......DD5Lp5cqXg==",
  "status": "inProgress",
  "status_details": "Pending certificate created. Please Perform Merge to complete the request.",
  "request_id": "a76827a18b63421c917da80f28e9913d"
}

```

## <a name="merge-when-a-pending-request-is-created---manual-certificate-creation"></a>Bekleyen bir istek oluşturulduğunda birleştirme - el ile sertifika oluşturma

|Yöntem|İstek URI'si|
|------------|-----------------|
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|

### <a name="request"></a>İstek

```json
{
  "x5c": [ "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8=" ]
}

```

|Öğe adı|Gerekli|Tür|Sürüm|Açıklama|
|------------------|--------------|----------|-------------|-----------------|
|x5c|Evet|array|\<sürüm> tanıtılması|Temel 64 dize dizisi olarak X509 sertifika zinciri.|

### <a name="response"></a>Yanıt

```
StatusCode: 201, ReasonPhrase: 'Created'
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1?api-version={api-version}"
{
    "id": "https mykeyvault.vault.azure.net/certificates/mycert1/f366e1a9dd774288ad84a45a5f620352",
    "kid": "https:// mykeyvault.vault.azure.net/keys/mycert1/f366e1a9dd774288ad84a45a5f620352",
    "sid": " mykeyvault.vault.azure.net/secrets/mycert1/f366e1a9dd774288ad84a45a5f620352",
    "cer": "……de34534……",
    "x5t": "n14q2wbvyXr71Pcb58NivuiwJKk",
    "attributes": {
        "enabled": true,
        "exp": 1530394215,
        "nbf": 1435699215,
        "created": 1435699919,
        "updated": 1435699919
    },
    "pending": {
        "id": "https:// mykeyvault.vault.azure.net/certificates/mycert1/pending"
    },
    "policy": {
        "id": "https:// mykeyvault.vault.azure.net/certificates/mycert1/policy",
        "key_props": {
            "exportable": false,
            "kty": "RSA",
            "key_size": 2048,
            "reuse_key": false
        },
        "secret_props": {
            "contentType": "application/x-pkcs12"
        },
        "x509_props": {
            "subject": "CN=Mycert1",
            "ekus": ["1.3.6.1.5.5.7.3.1", "1.3.6.1.5.5.7.3.2"],
            "validity_months": 12
        },
        "lifetime_actions": [{
            "trigger": {
                "lifetime_percentage": 80
            },
            "action": {
                "action_type": "EmailContacts"
            }
        }],
        "issuer": {
            "name": "Unknown"
        },
        "attributes": {
            "enabled": true,
            "created": 1435699811,
            "updated": 1435699811
        }
    }
}

```

## <a name="see-also"></a>Ayrıca Bkz.
- [Anahtarlar, sırlar ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
