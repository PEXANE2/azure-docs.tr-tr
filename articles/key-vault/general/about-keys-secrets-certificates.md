---
title: Azure Key Vault anahtarlar, gizli diziler ve sertifikalar hakkında-Azure Key Vault
description: Anahtarlar, gizli diziler ve sertifikalar için Azure Key Vault REST arabirimine ve geliştirici ayrıntılarına genel bakış.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: overview
ms.date: 04/17/2020
ms.author: mbaldwin
ms.openlocfilehash: 241efab246dc903981da570a4191f93cc744bca7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81726448"
---
# <a name="about-keys-secrets-and-certificates"></a>Anahtarlar, gizli diziler ve sertifikalar hakkında

Azure Key Vault, Microsoft Azure uygulamaların ve kullanıcıların çeşitli türlerdeki gizli/anahtar verileri depolamasını ve kullanmasını sağlar:

- Şifreleme anahtarları: birden çok anahtar türünü ve algoritmaları destekler ve yüksek değerli anahtarlar için donanım güvenlik modüllerinin (HSM) kullanılmasını mümkün. Daha fazla bilgi için bkz. [anahtarlar hakkında](../keys/about-keys.md).
- Gizlilikler: parolalar ve veritabanı bağlantı dizeleri gibi güvenli parolaların depolanmasını sağlar. Daha fazla bilgi için bkz. [gizlilikler hakkında](../secrets/about-secrets.md).
- Sertifikalar: anahtarların ve parolaların üzerine inşa edilen sertifikaları destekler ve otomatik yenileme özelliği ekler. Daha fazla bilgi için bkz. [Sertifikalar hakkında](../certificates/about-certificates.md).
- Azure depolama: bir Azure depolama hesabının anahtarlarını sizin için yönetebilir. Dahili olarak, Key Vault anahtarları Azure Storage hesabıyla listeleyebilir (eşitleyebilir) ve anahtarları düzenli olarak yeniden oluşturabilir (döndürün). Daha fazla bilgi için bkz. [Key Vault depolama hesabı anahtarlarını yönetme](../secrets/overview-storage-keys.md).

Key Vault hakkında daha fazla genel bilgi için bkz. [Azure Key Vault hakkında](overview.md).

## <a name="data-types"></a>Veri türleri

Anahtarlar, şifreleme ve imzalama için ilgili veri türleri için JOTE belirtimlerine bakın.  

-   **algoritma** -anahtar işlemi için desteklenen bir algoritma; örneğin, RSA1_5  
-   **şifreli-değer** -şifre metin Sekizlilileri, Base64URL kullanarak kodlanmış  
-   **Özet-değer** -Base64URL kullanılarak kodlanan karma algoritmanın çıkışı  
-   **anahtar türü** -desteklenen anahtar türlerinden biri, örneğin RSA (Rivest-Shamir-Adliman).  
-   **düz metin-değer** -düz metin sekizlik, Base64URL kullanılarak kodlanır  
-   **imza-değer** -Base64URL kullanılarak kodlanan imza algoritmasının çıkışı  
-   **base64URL** -a BASE64URL [RFC4648] kodlamalı ikili değer  
-   **Boolean** -true veya false  
-   **Kimlik** -Azure Active Directory bir KIMLIK (AAD).  
-   **Intdate** -belirtilen UTC tarih/saatine kadar 1970-01-01T0:0: 0z UTC 'den saniye sayısını temsil eden bir JSON ondalık değeri. Genel ve UTC 'de tarih/saat ile ilgili ayrıntılar için bkz. RFC3339.  

## <a name="objects-identifiers-and-versioning"></a>Nesneler, tanımlayıcılar ve sürüm oluşturma

Key Vault depolanan nesneler, bir nesnenin yeni bir örneği oluşturulduğunda sürümlüdür. Her sürüme benzersiz bir tanımlayıcı ve URL atanır. Bir nesne ilk oluşturulduğunda, benzersiz bir sürüm tanımlayıcısı verilir ve nesnenin geçerli sürümü olarak işaretlenir. Aynı nesne adına sahip yeni bir örnek oluşturmak, yeni nesneye benzersiz bir sürüm tanımlayıcısı sağlar ve bunun geçerli sürüm olmasına neden olur.  

Key Vault nesneler, geçerli tanımlayıcı veya sürüme özgü tanımlayıcı kullanılarak çözülebilir. Örneğin, ada `MasterKey`sahip bir anahtar verildiğinde, geçerli tanımlayıcıyla işlem gerçekleştirmek sistemin kullanılabilir en son sürümü kullanmasına neden olur. Sürüme özgü tanımlayıcıyla işlem gerçekleştirmek sistemin nesnenin o belirli sürümünü kullanmasına neden olur.  

Nesneler bir URL kullanarak Key Vault içinde benzersiz şekilde tanımlanır. Coğrafi konumdan bağımsız olarak sistemdeki iki nesne aynı URL 'ye sahip değildir. Bir nesnenin tüm URL 'SI, nesne tanımlayıcısı olarak adlandırılır. URL, Key Vault, nesne türü, Kullanıcı tarafından sağlanmış nesne adı ve bir nesne sürümü tanımlayan bir önekden oluşur. Nesne adı büyük/küçük harfe duyarlıdır ve sabittir. Nesne sürümünü içermeyen tanımlayıcılar temel tanımlayıcılar olarak adlandırılır.  

Daha fazla bilgi için bkz. [kimlik doğrulaması, istekler ve yanıtlar](authentication-requests-and-responses.md)

Bir nesne tanımlayıcısı aşağıdaki genel biçime sahiptir:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Konumlar:  

|||  
|-|-|  
|`keyvault-name`|Microsoft Azure Key Vault hizmetindeki bir anahtar kasasının adı.<br /><br /> Key Vault adlar Kullanıcı tarafından seçilir ve genel olarak benzersizdir.<br /><br /> Key Vault ad, yalnızca 0-9, a-z, A-Z ve-içeren bir 3-24 karakter dizesi olmalıdır.|  
|`object-type`|Nesnenin türü, "anahtarlar", "gizlilikler" veya ' Certificates '.|  
|`object-name`|`object-name` , İçin Kullanıcı tarafından sağlanmış bir addır ve bir Key Vault içinde benzersiz olmalıdır. Ad, yalnızca 0-9, a-z, A-Z ve-içeren bir 1-127 karakter dizesi olmalıdır.|  
|`object-version`|`object-version` , İsteğe bağlı olarak bir nesnenin benzersiz bir sürümünü ele almak için kullanılan, sistem tarafından oluşturulan 32 karakter dizesi tanımlayıcısıdır.|  

## <a name="next-steps"></a>Sonraki adımlar

- [Anahtarlar hakkında](../keys/about-keys.md)
- [Gizli diziler hakkında](../secrets/about-secrets.md)
- [Sertifikalar hakkında](../certificates/about-certificates.md)
- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Geliştirici Kılavuzu Key Vault](../general/developers-guide.md)