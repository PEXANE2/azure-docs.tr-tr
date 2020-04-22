---
title: Azure Key Vault tuşları hakkında - Azure Key Vault
description: Anahtarlar için Azure Key Vault REST arabirimine ve geliştirici ayrıntılarına genel bakış.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: overview
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 3d89275e1418035fed8aad3ffddd8def2c1d59ce
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686050"
---
# <a name="about-azure-key-vault-keys"></a>Azure Anahtar Kasası tuşları hakkında

Azure Anahtar Kasası birden çok anahtar türünü ve algoritmasını destekler ve yüksek değerli anahtarlar için Donanım Güvenlik Modüllerinin (HSM) kullanılmasını sağlar.

Key Vault'taki şifreleme anahtarları JSON Web Key [JWK] nesneleri olarak temsil edilir. JavaScript Nesne Gösterimi (JSON) ve JavaScript Nesne İmzalama ve Şifreleme (JOSE) özellikleri şunlardır:

-   [JSON Web Anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web Şifreleme (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web Algoritmaları (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web İmza (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature) 

Temel JWK/JWA belirtimleri, Key Vault uygulamasına özgü anahtar türlerini etkinleştirmek için de genişletilir. Örneğin, Anahtarları NIçin Satıcıya özel ambalaj kullanarak almak, yalnızca Key Vault HSM'lerde kullanılabilecek anahtarların güvenli bir şekilde taşınmasını sağlar. 

Azure Key Vault hem Yumuşak hem de Sert tuşlarını destekler:

- **"Yumuşak" tuşları**: Key Vault tarafından yazılımda işlenen, ancak HSM'de bulunan bir sistem anahtarı kullanılarak istirahatte şifrelenen bir anahtardır. İstemciler varolan bir RSA veya EC (Eliptik Eğri) anahtarını içe aktarabilir veya Key Vault'un bir anahtar oluşturmasini isteyebilir.
- **"Sabit" tuşlar**: HSM'de (Donanım Güvenlik Modülü) işlenen anahtardır. Bu anahtarlar Key Vault HSM Güvenlik Dünyaları'ndan birinde korunur (izolasyon sağlamak için coğrafya başına bir Güvenlik Dünyası vardır). İstemciler yumuşak formda veya uyumlu bir HSM aygıtından dışa aktararak bir RSA veya EC anahtarı içe aktarabilirler. İstemciler, anahtar oluşturmak için Key Vault'u da isteyebilir. Bu anahtar türü, HSM anahtar malzemesini taşımak için elde edilen JWK'ya key_hsm özniteliği ekler.

Coğrafi sınırlar hakkında daha fazla bilgi için [Microsoft Azure Güven Merkezi'ne](https://azure.microsoft.com/support/trust-center/privacy/) bakın  

## <a name="cryptographic-protection"></a>Şifreleme koruması

Key Vault yalnızca RSA ve Eliptik Eğri tuşlarını destekler. 

-   **EC**: "Yumuşak" Eliptik Eğri tuşu.
-   **EC-HSM**: "Sert" Eliptik Eğri tuşu.
-   **RSA**: "Yumuşak" RSA tuşu.
-   **RSA-HSM**: "Sert" RSA anahtarı.

Key Vault 2048, 3072 ve 4096 boyutlarında RSA tuşlarını destekler. Anahtar Vault Eliptik Eğrisi anahtar tipleri P-256, P-384, P-521 ve P-256K (SECP256K1) destekler.

Key Vault'un kullandığı şifreleme modülleri, HSM veya yazılım olsun, FIPS (Federal Bilgi İşlem Standartları) doğrulanır. FIPS modunda çalıştırmak için özel bir şey yapmanız gerekmez. HSM korumalı olarak **oluşturulan** veya **içe aktarılan** anahtarlar, BIR HSM içinde işlenir ve FIPS 140-2 Düzey 2'ye doğrulanır. Yazılım korumalı olarak **oluşturulan** veya **içe aktarılan** anahtarlar, FIPS 140-2 Düzey 1'e doğrulanmış şifreleme modülleri içinde işlenir.

###  <a name="ec-algorithms"></a>EC algoritmaları
 Aşağıdaki algoritma tanımlayıcıları Key Vault'ta EC ve EC-HSM tuşları ile desteklenir. 

#### <a name="curve-types"></a>Eğri Türleri

-   **P-256** - [DSS FIPS PUB 186-4'te](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)tanımlanan NIST eğrisi P-256.
-   **P-256K** - SEC 2'de tanımlanan SEC eğrisi SECP256K1: [Önerilen Eliptik Eğri Etki Alanı Parametreleri.](https://www.secg.org/sec2-v2.pdf)
-   **P-384** - [DSS FIPS PUB 186-4'te](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)tanımlanan NIST eğrisi P-384.
-   **P-521** - [DSS FIPS PUB 186-4'te](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)tanımlanan NIST eğrisi P-521.

#### <a name="signverify"></a>İşaretLE/DOĞRULA

-   **ES256** - SHA-256 için ECDSA, P-256 eğrisi ile oluşturulan sindirir ve tuşlarını sindirir. Bu algoritma [RFC7518](https://tools.ietf.org/html/rfc7518)açıklanmıştır.
-   **ES256K** - SHA-256 için ECDSA, P-256K eğrisi ile oluşturulan sindirir ve tuşlarını sindirir. Bu algoritma standartlaştırma bekliyor.
-   **ES384** - SHA-384 için ECDSA, P-384 eğrisi ile oluşturulan sindirir ve tuşlarını sindirir. Bu algoritma [RFC7518](https://tools.ietf.org/html/rfc7518)açıklanmıştır.
-   **ES512** - SHA-512 için ECDSA, P-521 eğrisi ile oluşturulan sindirir ve tuşlar. Bu algoritma [RFC7518](https://tools.ietf.org/html/rfc7518)açıklanmıştır.

###  <a name="rsa-algorithms"></a>RSA algoritmaları  
 Aşağıdaki algoritma tanımlayıcıları Key Vault'ta RSA ve RSA-HSM tuşları ile desteklenir.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY, ŞIFRELEME/ŞIFRE ÇÖZME

-   **RSA1_5** - RSAES-PKCS1-V1_5 [RFC3447] anahtar şifreleme  
-   **RSA-OAEP** - RsAES Optimal Asimetrik Şifreleme Dolgu (OAEP) [RFC3447] kullanarak, Varsayılan parametreler IFC 3447 tarafından Bölüm A.2.1'de belirtilmiştir. Bu varsayılan parametreler SHA-1 karma fonksiyonu ve SHA-1 ile MGF1 bir maske oluşturma fonksiyonu kullanıyor.  

#### <a name="signverify"></a>İşaretLE/DOĞRULA

-   **PS256** - RSASSA-PSS SHA-256 ve MGF1 SHA-256 ile kullanarak, [RFC7518](https://tools.ietf.org/html/rfc7518)açıklandığı gibi.
-   **PS384** - RSASSA-PSS SHA-384 ve MGF1 SHA-384 ile kullanarak, [RFC7518](https://tools.ietf.org/html/rfc7518)açıklandığı gibi.
-   **PS512** - RSASSA-PSS SHA-512 ve MGF1 SHA-512 ile kullanarak, [RFC7518](https://tools.ietf.org/html/rfc7518)açıklandığı gibi.
-   **RS256** - RSASSA-PKCS-v1_5 SHA-256 kullanılarak. Sağlanan uygulama sindirimi değeri SHA-256 kullanılarak hesaplanmalıdır ve uzunluğu 32 bayt olmalıdır.  
-   **RS384** - RSASSA-PKCS-v1_5 SHA-384 kullanıyor. Sağlanan uygulama sindirimi değeri SHA-384 kullanılarak hesaplanmalıdır ve uzunluğu 48 bayt olmalıdır.  
-   **RS512** - RSASSA-PKCS-v1_5 SHA-512 kullanıyor. Sağlanan uygulama sindirimi değeri SHA-512 kullanılarak hesaplanmalıdır ve 64 bayt uzunluğunda olmalıdır.  
-   **RSNULL** - Bkz. [RFC2437], belirli TLS senaryolarını etkinleştirmek için özel leştirilmiş bir kullanım örneği.  

##  <a name="key-operations"></a>Anahtar işlemleri

Key Vault anahtar nesneler üzerinde aşağıdaki işlemleri destekler:  

-   **Create**: Anahtar Kasası'nda anahtar oluşturmasına izin verir. Anahtarın değeri Key Vault tarafından oluşturulur ve depolanır ve istemciye serbest bırakılmaz. Anahtar Kasası'nda asimetrik tuşlar oluşturulabilir.  
-   **Alma**: İstemcinin Key Vault'a varolan bir anahtarı içe aktarmasını sağlar. Asimetrik anahtarlar, Bir JWK yapısı içinde bir dizi farklı paketleme yöntemi kullanılarak Key Vault'a ithal edilebilir. 
-   **Güncelleştirme**: Anahtar Kasası'nda daha önce depolanan bir anahtarla ilişkili meta verileri (anahtar öznitelikleri) değiştirmek için yeterli izinlere sahip bir istemciye izin verir.  
-   **Delete**: Anahtar Kasası'ndan bir anahtarı silmek için yeterli izinlere sahip bir istemciye izin verir.  
-   **Liste**: İstemcinin belirli bir Anahtar Kasası'ndaki tüm anahtarları listelemesine izin verir.  
-   **Liste sürümleri**: İstemcinin belirli bir anahtardaki tüm sürümlerini belirli bir Anahtar Kasası'nda listelemesine olanak tanır.  
-   **Get**: Bir istemcinin Anahtar Kasası'nda belirli bir anahtarın ortak parçalarını almasına izin verir.  
-   **Yedekleme**: Bir anahtarı korumalı biçimde dışa aktarma.  
-   **Geri Yükleme**: Önceden yedeklenmiş bir anahtarı içeri iter.  

Daha fazla bilgi için [Key Vault REST API başvurusundaki Anahtar işlemlerine](/rest/api/keyvault)bakın.  

Key Vault'ta bir anahtar oluşturulduktan sonra, aşağıdaki şifreleme işlemleri anahtar kullanılarak gerçekleştirilebilir:  

-   **İmzala ve Doğrulama**: Key Vault imza oluşturmanın bir parçası olarak içeriğin karma sını desteklemediği için, bu işlem kesinlikle "karma işaret" veya "karma doğrulama"dır. Uygulamalar, yerel olarak imzalanacak verileri karma, ardından Key Vault'un karma yı imzalamasını istemelidir. İmzalı hashes doğrulama [ortak] anahtar malzeme erişimi olmayabilir uygulamalar için bir kolaylık işlemi olarak desteklenir. En iyi uygulama performansı için, işlemlerin yerel olarak gerçekleştirildiğini doğrulayın.  
-   **Anahtar Şifreleme / Sarma**: Key Vault'ta depolanan bir anahtar, genellikle simetrik içerik şifreleme anahtarı (CEK) olan başka bir anahtarı korumak için kullanılabilir. Key Vault'taki anahtar asimetrik olduğunda anahtar şifrelemesi kullanılır. Örneğin, RSA-OAEP ve WRAPKEY/UNWRAPKEY işlemleri ENCRYPT/DECRYPT'e eşdeğerdir. Key Vault'taki anahtar simetrik olduğunda, anahtar kaydırma kullanılır. Örneğin, AES-KW. WRAPKEY işlemi, [genel] anahtar materyallere erişimi olmayan uygulamalar için kolaylık sağlamak amacıyla desteklenir. En iyi uygulama performansı için WRAPKEY işlemleri yerel olarak gerçekleştirilmelidir.  
-   **Şifreleme ve Şifre çözme**: Key Vault'ta depolanan bir anahtar, tek bir veri bloğunu şifrelemek veya şifresini çözmek için kullanılabilir. Bloğun boyutu anahtar türü ve seçili şifreleme algoritması tarafından belirlenir. Şifrele işlemi, [genel] anahtar malzemeye erişimi olmayan uygulamalar için kolaylık sağlamak için sağlanır. En iyi uygulama performansı için şifreleme işlemleri yerel olarak gerçekleştirilmelidir.  

Asimetrik tuşları kullanarak WRAPKEY/UNWRAPKEY gereksiz görünse de (işlem ENCRYPT/DECRYPT'e eşdeğer olduğundan), farklı işlemlerin kullanımı önemlidir. Bu ayrım, bu işlemlerin anlamsal ve yetkilendirme ayrımını ve diğer anahtar türleri hizmet tarafından desteklendiğinde tutarlılık sağlar.  

Key Vault İhRACAT işlemlerini desteklemez. Bir anahtar sistemde bir anahtar sağlandıktan sonra ayıklanamaz veya anahtar malzemesi değiştirilemez. Ancak, Key Vault kullanıcıları, silindikten sonra olduğu gibi diğer kullanım durumları için anahtarlarını isteyebilir. Bu durumda, anahtarı korumalı bir biçimde dışa aktarmak/almak için BACKUP ve RESTORE işlemlerini kullanabilirler. BACKUP işlemi tarafından oluşturulan anahtarlar Key Vault dışında kullanılabilir değildir. Alternatif olarak, IMPORT işlemi birden çok Anahtar Kasası örneklerine karşı kullanılabilir.  

Kullanıcılar, JWK nesnesinin key_ops özelliğini kullanarak Key Vault'un desteklediği şifreleme işlemlerinden herhangi birini anahtar başına kısıtlayabilir.  

JWK nesneleri hakkında daha fazla bilgi için [JSON Web Key (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41)adresine bakın.  

## <a name="key-attributes"></a>Temel öznitelikler

Anahtar malzemeye ek olarak aşağıdaki öznitelikler de belirtilebilir. JSON İsteği'nde, '{' '}'öznitelikleri, belirtilmemiş olsa bile gereklidir.  

- *etkin*: boolean, isteğe bağlı, varsayılan **doğrudur**. Anahtarın şifreleme işlemleri için etkin ve kullanılabilir olup olmadığını belirtir. *Etkin* öznitelik *nbf* ve *exp*ile birlikte kullanılır. *NBF* ve *exp*arasında bir işlem gerçekleştiğinde, yalnızca *etkin* **olarak gerçek**olarak ayarlanırsa izin verilir. *Nbf* / *exp* penceresi dışındaki işlemlere, [belirli koşullar](#date-time-controlled-operations)altında belirli işlem türleri dışında otomatik olarak izin verilmez.
- *nbf*: IntDate, isteğe bağlı, varsayılan şimdi. *NBF* (daha önce değil) özniteliği, [belirli koşullar](#date-time-controlled-operations)altında belirli işlem türleri dışında, anahtarın şifreleme işlemleri için kullanılmaması gereken zamanı tanımlar. *nbf* özniteliğinin işlenmesi, geçerli tarih/saatIN *nbf* özyönüne alınan tarihten/saate eşit veya sonra olması gerektiğini gerektirir. Key Vault MAY, normalde en fazla birkaç dakika, saat çarpık lık için hesap bazı küçük bir leeway sağlar. Değeri IntDate değeri içeren bir sayı olmalıdır.  
- *exp*: IntDate, isteğe bağlı, varsayılan "sonsuza kadar". *Exp* (son kullanma süresi) özniteliği, [belirli koşullar](#date-time-controlled-operations)altında belirli işlem türleri dışında, anahtarın şifreleme işlemi için kullanılmaması gereken son kullanma süresini tanımlar. *Exp* özniteliğinin işlenmesi, geçerli tarih/saatin *exp* özyönüne listelenen son kullanma tarihinden/saatinden önce olması gerekir. Key Vault MAY, saat çarpıklık için hesap, genellikle en fazla birkaç dakika, bazı küçük bir leeway sağlar. Değeri IntDate değeri içeren bir sayı olmalıdır.  

Anahtar öznitelikleri içeren herhangi bir yanıta dahil olan salt okunur öznitelikleri vardır:  

- *created*: IntDate, isteğe bağlı. *Oluşturulan* öznitelik, anahtarın bu sürümünün ne zaman oluşturulduğunu gösterir. Bu öznitelik eklenmesinden önce oluşturulan anahtarlar için değer null' dur. Değeri IntDate değeri içeren bir sayı olmalıdır.  
- *güncelleme*: IntDate, isteğe bağlı. *Güncelleştirilmiş* öznitelik, anahtarın bu sürümünün ne zaman güncelleştirileni gösterir. Bu öznitelik eklenmeden önce en son güncelleştirilen anahtarlar için değer null'dur. Değeri IntDate değeri içeren bir sayı olmalıdır.  

IntDate ve diğer veri türleri hakkında daha fazla bilgi için bkz: [Anahtarlar, sırlar ve sertifikalar hakkında: [Veri türleri.](../general/about-keys-secrets-certificates.md#data-types)

### <a name="date-time-controlled-operations"></a>Tarih-zaman kontrollü işlemler

Henüz geçerli olmayan ve süresi dolmuş anahtarlar, *nbf* / *exp* penceresi dışında, **şifresini çözmek**için çalışacaktır , **açmak**, ve işlemleri **doğrulamak** (403 döndürmez, Yasak). Henüz geçerli olmayan durumu kullanmanın mantığı, bir anahtarın üretim kullanımından önce sınanmasını sağlamaktır. Süresi dolmuş durumu kullanmanın mantığı, anahtar geçerli olduğunda oluşturulan verilerüzerinde kurtarma işlemlerine izin vermektir. Ayrıca, Key Vault ilkelerini kullanarak veya *etkin* anahtar özniteliğini **false'a**güncelleyerek bir anahtara erişimi devre dışı bırakabilirsiniz.

Veri türleri hakkında daha fazla bilgi için [Bkz. Veri türleri.](../general/about-keys-secrets-certificates.md#data-types)

Diğer olası öznitelikler hakkında daha fazla bilgi için [JSON Web Anahtarı 'na (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41)bakın.

## <a name="key-tags"></a>Anahtar etiketleri

Etiketler biçiminde uygulamaya özgü ek meta verileri belirtebilirsiniz. Key Vault, her biri 256 karakter adı ve 256 karakter değerine sahip olabilecek en fazla 15 etiketi destekler.  

>[!Note]
>Etiketler, *liste* varsa veya bu nesne türüne (anahtarlar, sırlar veya sertifikalar) izin *alıyorsa,* arayan tarafından okunabilir.

##  <a name="key-access-control"></a>Anahtar erişim denetimi

Key Vault tarafından yönetilen anahtarlar için erişim kontrolü, anahtarların kapsayıcısı gibi davranan bir Key Vault düzeyinde sağlanır. Anahtarlar için erişim denetimi ilkesi, aynı Anahtar Kasası'ndaki sırlar için erişim denetimi ilkesinden farklıdır. Kullanıcılar anahtarları tutmak için bir veya daha fazla kasa oluşturabilir ve senaryoya uygun bölümlemesi ve anahtarların yönetimini korumaları gerekir. Anahtarlar için erişim denetimi, sırlar için erişim denetiminden bağımsızdır.  

Aşağıdaki izinler, kullanıcı başına / hizmet temel olarak, bir tonoz üzerinde anahtarları erişim kontrol girişinde verilebilir. Bu izinler, anahtar nesnesi üzerinde izin verilen işlemleri yakından yansıtabilir.  Anahtar kasasında bir hizmet ilkesine erişim vermek tek seferlik bir işlemdir ve tüm Azure abonelikleri için aynı kalır. İstediğiniz kadar sertifika dağıtmak için kullanabilirsiniz. 

- Önemli yönetim işlemleri için izinler
  - *get*: Anahtarın ortak kısmını ve özniteliklerini okuyun
  - *liste*: Anahtar kasasında depolanan bir anahtarın anahtarlarını veya sürümlerini listeleyin
  - *update*: Bir anahtarın özniteliklerini güncelleştir
  - *create*: Yeni tuşlar oluşturma
  - *import*: Anahtar kasasına anahtar alma
  - *delete*: Anahtar nesnesini silme
  - *recover*: Silinen bir anahtarı kurtarma
  - *yedekleme*: Anahtar kasasında ki anahtarı yedekleme
  - *restore*: Yedeklenmiş anahtarı bir anahtar kasasına geri yükleme

- Şifreleme işlemleri için izinler
  - *decrypt*: Bayt bir dizi korumak için anahtar kullanın
  - *encrypt*: Baytların rasgele bir dizi korumak için anahtarı kullanın
  - *unwrapKey*: Sarılmış simetrik tuşları korumak için anahtarı kullanın
  - *wrapKey*: Simetrik bir anahtarı korumak için anahtarı kullanın
  - *verify*: Özetleri doğrulamak için anahtarı kullanın  
  - *işareti*: Özetleri imzalamak için anahtarı kullanın
    
- Ayrıcalıklı işlemler için izinler
  - *temizleme*: Silinmiş bir anahtarı temizleme (kalıcı olarak silme)

Anahtarlarla çalışma hakkında daha fazla bilgi için [Key Vault REST API başvurusunda ki Anahtar işlemlerine](/rest/api/keyvault)bakın. İzinlerin oluşturulması hakkında daha fazla bilgi için [Vaults - Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy) [bkz.](/rest/api/keyvault/vaults/createorupdate) 

## <a name="next-steps"></a>Sonraki adımlar

- [Anahtar Kasası Hakkında](../general/overview.md)
- [Anahtarlar, sırlar ve sertifikalar hakkında](../general/about-keys-secrets-certificates.md)
- [Gizli diziler hakkında](../secrets/about-secrets.md)
- [Sertifikalar hakkında](../certificates/about-certificates.md)
- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](../general/developers-guide.md)
