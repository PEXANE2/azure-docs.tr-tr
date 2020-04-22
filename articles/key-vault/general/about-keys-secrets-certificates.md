---
title: Azure Key Vault tuşları, sırlar ve sertifikalar hakkında - Azure Key Vault
description: Anahtarlar, sırlar ve sertifikalar için Azure Key Vault REST arabirimine ve geliştirici ayrıntılarına genel bakış.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: overview
ms.date: 04/17/2020
ms.author: mbaldwin
ms.openlocfilehash: 241efab246dc903981da570a4191f93cc744bca7
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726448"
---
# <a name="about-keys-secrets-and-certificates"></a>Anahtarlar, sırlar ve sertifikalar hakkında

Azure Key Vault, Microsoft Azure uygulamalarının ve kullanıcılarının çeşitli gizli/anahtar verileri depolamasını ve kullanmasını sağlar:

- Şifreleme tuşları: Birden çok anahtar türünü ve algoritmasını destekler ve yüksek değerli anahtarlar için Donanım Güvenlik Modüllerinin (HSM) kullanılmasını sağlar. Daha fazla bilgi [için, anahtarlar hakkında](../keys/about-keys.md)bakın.
- Sırlar: Parolalar ve veritabanı bağlantı dizeleri gibi sırların güvenli bir şekilde depolanmasını sağlar. Daha fazla bilgi [için, sırlar hakkında](../secrets/about-secrets.md)bakın.
- Sertifikalar: Anahtarların ve sırların üzerine inşa edilen sertifikaları destekler ve otomatik yenileme özelliği ekler. Daha fazla bilgi için [sertifikalar hakkında](../certificates/about-certificates.md)bkz.
- Azure Depolama: Bir Azure Depolama hesabının anahtarlarını sizin için yönetebilirsiniz. Dahili olarak, Key Vault anahtarlarını Azure Depolama Hesabı ile listeleyebilir (eşitleyebilir) ve anahtarları düzenli aralıklarla yeniden oluşturabilir (döndürebilir). Daha fazla bilgi için, [Key Vault ile depolama hesabı anahtarlarını yönet'e](../secrets/overview-storage-keys.md)bakın.

Key Vault hakkında daha fazla bilgi için [Azure Anahtar Kasası Hakkında'ya](overview.md)bakın.

## <a name="data-types"></a>Veri türleri

Anahtarlar, şifreleme ve imzalama için ilgili veri türleri için JOSE belirtimlerine bakın.  

-   **algoritma** - örneğin, RSA1_5 önemli bir işlem için desteklenen bir algoritma  
-   **şifremetin değeri** - codeher metin sekizli, Base64URL kullanılarak kodlanmış  
-   **digest-value** - Base64URL kullanılarak kodlanmış bir karma algoritma, çıktı  
-   **anahtar türü** - desteklenen anahtar türlerinden biri, örneğin RSA (Rivest-Shamir-Adleman).  
-   **düz metin değeri** - düz metin sekizli, Base64URL kullanılarak kodlanmış  
-   **imza değeri** - Base64URL kullanılarak kodlanmış imza algoritmasının çıktısı  
-   **base64URL** - bir Base64URL [RFC4648] kodlanmış ikili değer  
-   **boolean** - ya doğru ya da yanlış  
-   **Kimlik** - Azure Active Directory (AAD) bir kimlik.  
-   **IntDate** - belirtilen UTC tarih/saatine kadar 1970-01-01T0:0:0Z UTC saniye sayısını temsil eden bir JSON ondalık değer. Genel olarak tarih/saat ve özellikle UTC ile ilgili ayrıntılar için RFC3339'a bakın.  

## <a name="objects-identifiers-and-versioning"></a>Nesneler, tanımlayıcılar ve sürüm

Key Vault'ta depolanan nesneler, yeni bir nesne örneği oluşturulduğunda sürülür. Her sürüme benzersiz bir tanımlayıcı ve URL atanır. Bir nesne ilk oluşturulduğunda, özgün bir sürüm tanımlayıcısı verilir ve nesnenin geçerli sürümü olarak işaretlenir. Aynı nesne adı ile yeni bir örnek oluşturma yeni nesne özgün bir sürüm tanımlayıcısı verir, geçerli sürümü haline neden.  

Key Vault'taki nesneler geçerli tanımlayıcı veya sürüme özgü tanımlayıcı kullanılarak ele alınabilir. Örneğin, adla `MasterKey`birlikte bir Anahtar verildiğinde, geçerli tanımlayıcıyla işlemleri gerçekleştirmek, sistemin kullanılabilir en son sürümü kullanmasına neden olur. Sürüme özgü tanımlayıcıile işlemleri gerçekleştirmek, sistemin nesnenin belirli bir sürümünü kullanmasına neden olur.  

Nesneler, Key Vault içinde bir URL kullanılarak benzersiz bir şekilde tanımlanır. Sistemdeki hiçbir iki nesne, coğrafi konuma bakılmaksızın aynı URL'ye sahip değildir. Bir nesnenin tam URL'si Nesne Tanımlayıcısı olarak adlandırılır. URL, Anahtar Kasası' nı, nesne türünü, kullanıcıtarafından sağlanan Nesne Adı'nı ve Nesne Sürümünü tanımlayan bir önekten oluşur. Nesne Adı büyük/küçük harf duyarsız ve değişmez. Nesne Sürümü içermeyen tanımlayıcılara Temel Tanımlayıcılar denir.  

Daha fazla bilgi için kimlik [doğrulama, istekler ve yanıtlar](authentication-requests-and-responses.md)

Nesne tanımlayıcısı aşağıdaki genel biçimi vardır:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Konumlar:  

|||  
|-|-|  
|`keyvault-name`|Microsoft Azure Key Vault hizmetindeki önemli bir kasanın adı.<br /><br /> Anahtar Vault adları kullanıcı tarafından seçilir ve genel olarak benzersizdir.<br /><br /> Anahtar Vault adı sadece 0-9, a-z, A-Z ve -içeren bir 3-24 karakter dize olmalıdır.|  
|`object-type`|Nesnenin türü, "anahtarlar", "sırlar" veya "sertifikalar".|  
|`object-name`|A, `object-name` kullanıcıadına verilen bir addır ve Bir Anahtar Kasası içinde benzersiz olmalıdır. Ad, yalnızca 0-9, a-z, A-Z ve -'yi içeren 1-127 karakterli bir dize olmalıdır.|  
|`object-version`|An, `object-version` isteğe bağlı olarak bir nesnenin benzersiz bir sürümünü adreslemek için kullanılan bir sistem tarafından oluşturulan, 32 karakter dize tanımlayıcısıdır.|  

## <a name="next-steps"></a>Sonraki adımlar

- [Anahtarlar hakkında](../keys/about-keys.md)
- [Gizli diziler hakkında](../secrets/about-secrets.md)
- [Sertifikalar hakkında](../certificates/about-certificates.md)
- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](../general/developers-guide.md)