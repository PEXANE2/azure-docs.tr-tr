---
title: Azure Key Vault tuşları, sırlar ve sertifikalar hakkında - Azure Key Vault
description: Anahtarlar, sırlar ve sertifikalar için Azure Key Vault REST arabirimine ve geliştirici ayrıntılarına genel bakış.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: dd8be482009e067bf9016cc8e351fc42a2db39c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271739"
---
# <a name="about-keys-secrets-and-certificates"></a>Anahtarlar, sırlar ve sertifikalar hakkında

Azure Key Vault, Microsoft Azure uygulamalarının ve kullanıcılarının çeşitli gizli/anahtar verileri depolamasını ve kullanmasını sağlar:

- Şifreleme tuşları: Birden çok anahtar türünü ve algoritmasını destekler ve yüksek değerli anahtarlar için Donanım Güvenlik Modüllerinin (HSM) kullanılmasını sağlar. 
- Sırlar: Parolalar ve veritabanı bağlantı dizeleri gibi sırların güvenli bir şekilde depolanmasını sağlar.
- Sertifikalar: Anahtarların ve sırların üzerine inşa edilen sertifikaları destekler ve otomatik yenileme özelliği ekler.
- Azure Depolama: Bir Azure Depolama hesabının anahtarlarını sizin için yönetebilirsiniz. Dahili olarak, Key Vault anahtarlarını Azure Depolama Hesabı ile listeleyebilir (eşitleyebilir) ve anahtarları düzenli aralıklarla yeniden oluşturabilir (döndürebilir). 

Key Vault hakkında daha fazla bilgi için [Azure Anahtar Kasası nedir?](/azure/key-vault/key-vault-overview)

## <a name="azure-key-vault"></a>Azure Key Vault

Aşağıdaki bölümlerde Key Vault hizmetinin uygulanması nda geçerli olan genel bilgiler sunulmaktadır.

### <a name="supporting-standards"></a>Destekleyici standartlar

JavaScript Nesne Gösterimi (JSON) ve JavaScript Nesne İmzalama ve Şifreleme (JOSE) belirtimleri önemli arka plan bilgileridir.  

-   [JSON Web Anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41)  
-   [JSON Web Şifreleme (JWE)](https://tools.ietf.org/html/draft-ietf-jose-json-web-encryption-40)  
-   [JSON Web Algoritmaları (JWA)](https://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms-40)  
-   [JSON Web İmza (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature-41)  

### <a name="data-types"></a>Veri türleri

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

### <a name="objects-identifiers-and-versioning"></a>Nesneler, tanımlayıcılar ve sürüm

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
|`object-type`|Nesnenin türü, ya "anahtarlar" ya da "sırlar".|  
|`object-name`|A, `object-name` kullanıcıadına verilen bir addır ve Bir Anahtar Kasası içinde benzersiz olmalıdır. Ad, yalnızca 0-9, a-z, A-Z ve -'yi içeren 1-127 karakterli bir dize olmalıdır.|  
|`object-version`|An, `object-version` isteğe bağlı olarak bir nesnenin benzersiz bir sürümünü adreslemek için kullanılan bir sistem tarafından oluşturulan, 32 karakter dize tanımlayıcısıdır.|  

## <a name="key-vault-keys"></a>Anahtar Kasa tuşları

### <a name="keys-and-key-types"></a>Anahtarlar ve anahtar türleri

Key Vault'taki şifreleme anahtarları JSON Web Key [JWK] nesneleri olarak temsil edilir. Temel JWK/JWA belirtimleri, Key Vault uygulamasına özgü anahtar türlerini etkinleştirmek için de genişletilir. Örneğin, Anahtarları NIçin Satıcıya özel ambalaj kullanarak almak, yalnızca Key Vault HSM'lerde kullanılabilecek anahtarların güvenli bir şekilde taşınmasını sağlar.  

- **"Yumuşak" tuşları**: Key Vault tarafından yazılımda işlenen, ancak HSM'de bulunan bir sistem anahtarı kullanılarak istirahatte şifrelenen bir anahtardır. İstemciler varolan bir RSA veya EC (Eliptik Eğri) anahtarını içe aktarabilir veya Key Vault'un bir anahtar oluşturmasini isteyebilir.
- **"Sabit" tuşlar**: HSM'de (Donanım Güvenlik Modülü) işlenen anahtardır. Bu anahtarlar Key Vault HSM Güvenlik Dünyaları'ndan birinde korunur (izolasyon sağlamak için coğrafya başına bir Güvenlik Dünyası vardır). İstemciler yumuşak formda veya uyumlu bir HSM aygıtından dışa aktararak bir RSA veya EC anahtarı içe aktarabilirler. İstemciler, anahtar oluşturmak için Key Vault'u da isteyebilir. Bu anahtar türü, HSM anahtar malzemesini taşımak için elde edilen JWK'ya key_hsm özniteliği ekler.

     Coğrafi sınırlar hakkında daha fazla bilgi için [Microsoft Azure Güven Merkezi'ne](https://azure.microsoft.com/support/trust-center/privacy/) bakın  

Key Vault yalnızca RSA ve Eliptik Eğri tuşlarını destekler. 

-   **EC**: "Yumuşak" Eliptik Eğri tuşu.
-   **EC-HSM**: "Sert" Eliptik Eğri tuşu.
-   **RSA**: "Yumuşak" RSA tuşu.
-   **RSA-HSM**: "Sert" RSA anahtarı.

Key Vault 2048, 3072 ve 4096 boyutlarında RSA tuşlarını destekler. Anahtar Vault Eliptik Eğrisi anahtar tipleri P-256, P-384, P-521 ve P-256K (SECP256K1) destekler.

### <a name="cryptographic-protection"></a>Şifreleme koruması

Key Vault'un kullandığı şifreleme modülleri, HSM veya yazılım olsun, FIPS (Federal Bilgi İşlem Standartları) doğrulanır. FIPS modunda çalıştırmak için özel bir şey yapmanız gerekmez. HSM korumalı olarak **oluşturulan** veya **içe aktarılan** anahtarlar, BIR HSM içinde işlenir ve FIPS 140-2 Düzey 2'ye doğrulanır. Yazılım korumalı olarak **oluşturulan** veya **içe aktarılan** anahtarlar, FIPS 140-2 Düzey 1'e doğrulanmış şifreleme modülleri içinde işlenir. Daha fazla bilgi [için, Bkz. Anahtarlar ve anahtar türleri.](#keys-and-key-types)

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

###  <a name="key-operations"></a>Anahtar işlemleri

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

###  <a name="key-attributes"></a>Temel öznitelikler

Anahtar malzemeye ek olarak aşağıdaki öznitelikler de belirtilebilir. JSON İsteği'nde, '{' '}'öznitelikleri, belirtilmemiş olsa bile gereklidir.  

- *etkin*: boolean, isteğe bağlı, varsayılan **doğrudur**. Anahtarın şifreleme işlemleri için etkin ve kullanılabilir olup olmadığını belirtir. *Etkin* öznitelik *nbf* ve *exp*ile birlikte kullanılır. *NBF* ve *exp*arasında bir işlem gerçekleştiğinde, yalnızca *etkin* **olarak gerçek**olarak ayarlanırsa izin verilir. *Nbf* / *exp* penceresi dışındaki işlemlere, [belirli koşullar](#date-time-controlled-operations)altında belirli işlem türleri dışında otomatik olarak izin verilmez.
- *nbf*: IntDate, isteğe bağlı, varsayılan şimdi. *NBF* (daha önce değil) özniteliği, [belirli koşullar](#date-time-controlled-operations)altında belirli işlem türleri dışında, anahtarın şifreleme işlemleri için kullanılmaması gereken zamanı tanımlar. *nbf* özniteliğinin işlenmesi, geçerli tarih/saatIN *nbf* özyönüne alınan tarihten/saate eşit veya sonra olması gerektiğini gerektirir. Key Vault MAY, normalde en fazla birkaç dakika, saat çarpık lık için hesap bazı küçük bir leeway sağlar. Değeri IntDate değeri içeren bir sayı olmalıdır.  
- *exp*: IntDate, isteğe bağlı, varsayılan "sonsuza kadar". *Exp* (son kullanma süresi) özniteliği, [belirli koşullar](#date-time-controlled-operations)altında belirli işlem türleri dışında, anahtarın şifreleme işlemi için kullanılmaması gereken son kullanma süresini tanımlar. *Exp* özniteliğinin işlenmesi, geçerli tarih/saatin *exp* özyönüne listelenen son kullanma tarihinden/saatinden önce olması gerekir. Key Vault MAY, saat çarpıklık için hesap, genellikle en fazla birkaç dakika, bazı küçük bir leeway sağlar. Değeri IntDate değeri içeren bir sayı olmalıdır.  

Anahtar öznitelikleri içeren herhangi bir yanıta dahil olan salt okunur öznitelikleri vardır:  

- *created*: IntDate, isteğe bağlı. *Oluşturulan* öznitelik, anahtarın bu sürümünün ne zaman oluşturulduğunu gösterir. Bu öznitelik eklenmesinden önce oluşturulan anahtarlar için değer null' dur. Değeri IntDate değeri içeren bir sayı olmalıdır.  
- *güncelleme*: IntDate, isteğe bağlı. *Güncelleştirilmiş* öznitelik, anahtarın bu sürümünün ne zaman güncelleştirileni gösterir. Bu öznitelik eklenmeden önce en son güncelleştirilen anahtarlar için değer null'dur. Değeri IntDate değeri içeren bir sayı olmalıdır.  

IntDate ve diğer veri türleri hakkında daha fazla bilgi için [bkz.](#data-types)  

#### <a name="date-time-controlled-operations"></a>Tarih-zaman kontrollü işlemler

Henüz geçerli olmayan ve süresi dolmuş anahtarlar, *nbf* / *exp* penceresi dışında, **şifresini çözmek**için çalışacaktır , **açmak**, ve işlemleri **doğrulamak** (403 döndürmez, Yasak). Henüz geçerli olmayan durumu kullanmanın mantığı, bir anahtarın üretim kullanımından önce sınanmasını sağlamaktır. Süresi dolmuş durumu kullanmanın mantığı, anahtar geçerli olduğunda oluşturulan verilerüzerinde kurtarma işlemlerine izin vermektir. Ayrıca, Key Vault ilkelerini kullanarak veya *etkin* anahtar özniteliğini **false'a**güncelleyerek bir anahtara erişimi devre dışı bırakabilirsiniz.

Veri türleri hakkında daha fazla bilgi için [Bkz. Veri türleri.](#data-types)

Diğer olası öznitelikler hakkında daha fazla bilgi için [JSON Web Anahtarı 'na (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41)bakın.

### <a name="key-tags"></a>Anahtar etiketleri

Etiketler biçiminde uygulamaya özgü ek meta verileri belirtebilirsiniz. Key Vault, her biri 256 karakter adı ve 256 karakter değerine sahip olabilecek en fazla 15 etiketi destekler.  

>[!Note]
>Etiketler, *liste* varsa veya bu nesne türüne (anahtarlar, sırlar veya sertifikalar) izin *alıyorsa,* arayan tarafından okunabilir.

###  <a name="key-access-control"></a>Anahtar erişim denetimi

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

## <a name="key-vault-secrets"></a>Anahtar Vault sırları 

### <a name="working-with-secrets"></a>Sırlar ile çalışma

Bir geliştiricinin bakış açısından, Key Vault API'leri gizli değerleri dizeleri olarak kabul eder ve döndürür. Dahili olarak, Key Vault, her biri 25k bayt maksimum boyutuyla sırları sekizli (8 bit bayt) dizileri olarak saklar ve yönetir. Key Vault hizmeti sırlar için anlamsallık sağlamaz. Yalnızca verileri kabul eder, şifreler, saklar ve gizli bir tanımlayıcıyı ("id") döndürür. Tanımlayıcı daha sonra sırrı almak için kullanılabilir.  

Son derece hassas veriler söz konusu olduğunda müşterilerin veriler için ek koruma katmanlarını göz önünde bulundurması gerekir. Anahtarı Anahtar Kasasında depolamadan önce ayrı bir koruma anahtarı kullanarak verileri şifrelemek bunun bir örneğidir.  

Key Vault da sırlar için bir contentType alanı destekler. İstemciler, gizli verilerin alındığı zaman yorumlanmasına yardımcı olmak için bir sırrın içerik türünü belirtebilir. Bu alanın maksimum uzunluğu 255 karakterdir. Önceden tanımlanmış değerler yoktur. Önerilen kullanım, gizli verileri yorumlamak için bir ipucudur. Örneğin, bir uygulama hem parolaları hem de sertifikaları sır olarak depolayabilir ve sonra bu alanı ayırt etmek için kullanabilir. Önceden tanımlanmış değer yok.  

### <a name="secret-attributes"></a>Gizli öznitelikler

Gizli verilere ek olarak, aşağıdaki öznitelikler belirtilebilir:  

- *exp*: IntDate, isteğe bağlı, varsayılan **sonsuza kadar**. *Exp* (son kullanma süresi) özniteliği, [belirli durumlar](#date-time-controlled-operations)dışında gizli verilerin alınmaması gereken son kullanma süresini tanımlar. Bu alan, yalnızca anahtar kasa hizmetinin kullanıcılarına belirli bir sırrın kullanılmayabileceğini bildirdiği için **bilgilendirme** amaçlıdır. Değeri IntDate değeri içeren bir sayı olmalıdır.   
- *nbf*: IntDate, isteğe bağlı, varsayılan **şimdi**. *NBF* (daha önce değil) [özniteliği, belirli durumlar](#date-time-controlled-operations)dışında gizli verilerin alınmaması gereken zamanı tanımlar. Bu alan yalnızca **bilgilendirme** amaçlıdır. Değeri IntDate değeri içeren bir sayı olmalıdır. 
- *etkin*: boolean, isteğe bağlı, varsayılan **doğrudur**. Bu öznitelik, gizli verilerin alınıp alınamayacağını belirtir. Etkin öznitelik *nbf* ve *exp*arasında bir işlem oluştuğunda *nbf* ve *exp* ile birlikte kullanılır, sadece etkin **gerçek**ayarlanırsa izin verilecektir. *Nbf* ve *exp* penceresi dışındaki [işlemlere belirli durumlar](#date-time-controlled-operations)dışında otomatik olarak izin verilmez.  

Gizli öznitelikleri içeren herhangi bir yanıtta yer alan salt okunur öznitelikleri vardır:  

- *created*: IntDate, isteğe bağlı. Oluşturulan öznitelik, sırrın bu sürümünün ne zaman oluşturulduğunu gösterir. Bu değer, bu öznitelik eklenmesinden önce oluşturulan sırlar için null. Değeri, IntDate değeri içeren bir sayı olmalıdır.  
- *güncelleme*: IntDate, isteğe bağlı. Güncelleştirilmiş öznitelik, sırrın bu sürümünün ne zaman güncelleştirileni gösterir. Bu değer, bu öznitelik eklenmeden önce en son güncelleştirilen sırlar için null' dur. Değeri, IntDate değeri içeren bir sayı olmalıdır.

#### <a name="date-time-controlled-operations"></a>Tarih-zaman kontrollü işlemler

Bir sır **almak** operasyon *nbf* / *exp* penceresi dışında, henüz geçerli olmayan ve süresi dolmuş sırları için çalışacaktır. Henüz geçerli olmayan bir sır için bir sırrın **get** operasyonunu aramak, test amacıyla kullanılabilir. Alma **(ting ting)** süresi dolmuş bir sır, kurtarma işlemleri için kullanılabilir.

Veri türleri hakkında daha fazla bilgi için [Bkz. Veri türleri.](#data-types)  

### <a name="secret-access-control"></a>Parola erişim denetimi

Key Vault'ta yönetilen sırlar için Erişim Kontrolü, bu sırları içeren Key Vault düzeyinde sağlanır. Sırlar için erişim denetimi ilkesi, aynı Key Vault'taki anahtarlar için erişim denetimi ilkesinden farklıdır. Kullanıcılar sır tutmak için bir veya daha fazla kasa oluşturabilir ve senaryoya uygun bölümlemesi ve sırların yönetimini sürdürmeleri gerekir.   

Aşağıdaki izinler, her temel olarak, bir kasaüzerinde kontrol girişine erişen sırlarda kullanılabilir ve gizli bir nesneüzerinde izin verilen işlemleri yakından yansıtabilir:  

- Gizli yönetim işlemleri için izinler
  - *get*: Bir sır okuyun  
  - *liste*: Key Vault'ta saklanan bir sırrın sırlarını veya sürümlerini listele  
  - *set*: Bir sır oluşturma  
  - *delete*: Bir sırrı silme  
  - *recover*: Silinmiş bir sırrı kurtarma
  - *yedekleme*: Anahtar kasasında ki bir sırrı yedekle
  - *restore*: Yedeklenmiş bir sırrı anahtar kasasına geri yükleme

- Ayrıcalıklı işlemler için izinler
  - *tasfiye*: Silinmiş bir sırrı temizleme (kalıcı olarak silme)

Sırlar ile çalışma hakkında daha fazla bilgi [için, Key Vault REST API referans Gizli işlemleri](/rest/api/keyvault)bakın. İzinlerin oluşturulması hakkında daha fazla bilgi için [Vaults - Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy) [bkz.](/rest/api/keyvault/vaults/createorupdate) 

### <a name="secret-tags"></a>Gizli etiketler  
Etiketler biçiminde uygulamaya özgü ek meta verileri belirtebilirsiniz. Key Vault, her biri 256 karakter adı ve 256 karakter değerine sahip olabilecek en fazla 15 etiketi destekler.  

>[!Note]
>Etiketler, *liste* varsa veya bu nesne türüne (anahtarlar, sırlar veya sertifikalar) izin *alıyorsa,* arayan tarafından okunabilir.

## <a name="key-vault-certificates"></a>Anahtar Kasa Sertifikaları

Key Vault sertifikaları desteği, x509 sertifikalarınızın ve aşağıdaki davranışların yönetimini sağlar:  

-   Sertifika sahibinin Anahtar Kasa oluşturma işlemi veya varolan bir sertifikanın içe aktarımı yoluyla sertifika oluşturmasına olanak tanır. Hem kendi imzalı hem de Sertifika Yetkilisi tarafından oluşturulan sertifikaları içerir.
-   Key Vault sertifikası sahibinin, x509 sertifikalarının özel anahtar malzemesiyle etkileşimi olmadan güvenli depolama ve yönetimini uygulamasına olanak tanır.  
-   Sertifika sahibinin, anahtarın kullanım ömrünü yönetmesi için Key Vault'u yönlendiren bir ilke oluşturmasına olanak tanır.  
-   Sertifika sahiplerinin, sertifikanın sona ermesi ve yenilenmesi yle ilgili yaşam döngüsü olayları hakkında bildirim için iletişim bilgilerini sağlamasına olanak tanır.  
-   Seçilen ihraççılar ile otomatik yenilemeyi destekler - Key Vault ortağı X509 sertifika sağlayıcıları / sertifika yetkilileri.

>[!Note]
>Ortak olmayan sağlayıcılar/otoriteler de izin verilir, ancak otomatik yenileme özelliğini desteklemez.

### <a name="composition-of-a-certificate"></a>Sertifikanın Bileşimi

Key Vault sertifikası oluşturulduğunda, adreslenebilir bir anahtar ve gizli de aynı ada sahip oluşturulur. Key Vault tuşu anahtar işlemlerine izin verir ve Key Vault sırrı sertifika değerinin gizli olarak alınmasını sağlar. Anahtar Kasa sertifikası da ortak x509 sertifika meta verileri içerir.  

Sertifikaların tanımlayıcısı ve sürümü anahtarlara ve sırlara benzer. Key Vault sertifika sürümüyle oluşturulan adreslenebilir anahtar ve gizli bir sürümü Key Vault sertifika yanıtında kullanılabilir.
 
![Sertifikalar karmaşık nesnelerdir](media/azure-key-vault.png)

### <a name="exportable-or-non-exportable-key"></a>Dışa aktarılabilir veya dışa aktarılabilen anahtar

Bir Key Vault sertifikası oluşturulduğunda, pfx veya PEM formatında özel anahtarla adreslenebilir gizli den alınabilir. Sertifikayı oluşturmak için kullanılan ilke, anahtarın dışa aktarılabilir olduğunu belirtmelidir. İlke dışa aktarılamaz diyorsa, özel anahtar gizli olarak alındığı zaman değerin bir parçası değildir.  

Adreslenebilir anahtar, dışa aktarılamayan KV sertifikaları ile daha alakalı hale gelir. Adreslenebilir KV anahtarının işlemleri, KV Sertifikası oluşturmak için kullanılan KV sertifika ilkesinin *anahtar kullanım* alanından eşlenir.  

İki tür anahtar desteklenir : *RSA* veya *rsa HSM* sertifikaları ile. Yalnızca RSA ile ihraç edilebilir, RSA HSM tarafından desteklenmez.  

### <a name="certificate-attributes-and-tags"></a>Sertifika Öznitelikleri ve Etiketleri

Adreslenebilir bir anahtar ve adreslenebilir bir sır olan sertifika meta verilerine ek olarak, Anahtar Kasa sertifikası da öznitelikleri ve etiketleri içerir.  

#### <a name="attributes"></a>Öznitelikler

Sertifika öznitelikleri, KV sertifikası oluşturulduğunda oluşturulan adreslenebilir anahtar ve gizli özniteliklere yansıtılır.  

Anahtar Kasa sertifikası aşağıdaki özelliklere sahiptir:  

-   *etkin*: boolean, isteğe bağlı, varsayılan **doğrudur**. Sertifika verilerinin bir anahtar olarak gizli veya çalışabilir olarak alınıp alınamadığını belirtmek için belirtilebilir. Ayrıca *nbf* ve *exp*arasında bir işlem gerçekleştiğinde *nbf* ve *exp* ile birlikte kullanılır ve yalnızca etkin olarak doğru ayarlandığında izin verilir. *nbf* ve *exp* penceresi dışındaki işlemlere otomatik olarak izin verilmez.  

Yanıtta yer alan salt okunur öznitelikler vardır:

-   *created*: IntDate: sertifikanın bu sürümünün ne zaman oluşturulduğunu gösterir.  
-   *güncel :* IntDate: sertifikanın bu sürümünün ne zaman güncelleştirildiolduğunu gösterir.  
-   *exp*: IntDate: x509 sertifikasının son kullanma tarihi değerini içerir.  
-   *nbf*: IntDate: x509 sertifikasının tarihini içerir.  

> [!Note] 
> Key Vault sertifikasının süresi dolduğunda, adreslenebilir anahtar dır ve gizli olarak çalışılamaz hale gelir.  

#### <a name="tags"></a>Etiketler

 Anahtar ve sırlardaki etiketlere benzer anahtar değer çiftleri sözlüğü belirtilmiş.  

 > [!Note]
> Etiketler, *liste* varsa veya bu nesne türüne (anahtarlar, sırlar veya sertifikalar) izin *alıyorsa,* arayan tarafından okunabilir.

### <a name="certificate-policy"></a>Sertifika ilkesi

Sertifika ilkesi, Key Vault sertifikasının yaşam döngüsünün nasıl oluşturulup yönetilene ilişkin bilgiler içerir. Anahtar kasasına özel anahtarlı bir sertifika alındığında, x509 sertifikası okunarak varsayılan bir ilke oluşturulur.  

Anahtar Kasa sertifikası sıfırdan oluşturulduğunda, bir ilke sağlanmalıdır. İlke, bu Key Vault sertifika sürümünün veya bir sonraki Key Vault sertifikası sürümünün nasıl oluşturulacak olduğunu belirtir. Bir ilke oluşturulduktan sonra, gelecekteki sürümler için art arda oluşturma işlemleri için gerekli değildir. Key Vault sertifikasının tüm sürümleri için yalnızca bir ilke örneği vardır.  

Yüksek düzeyde, sertifika ilkesi aşağıdaki bilgileri içerir:  

-   X509 sertifika özellikleri: Özne adı, özne alternatif adları ve x509 sertifika isteği oluşturmak için kullanılan diğer özellikleri içerir.  
-   Anahtar Özellikleri: anahtar türü, anahtar uzunluğu, dışa aktarılabilir ve anahtar alanlarını yeniden kullanma içerir. Bu alanlar, anahtar oluşturma konusunda anahtar kasasına talimat verir.  
-   Gizli özellikler: sertifikayı gizli olarak almak için gizli değeri oluşturmak için adreslenebilir gizli içerik türü gibi gizli özellikler içerir.  
-   Ömür Boyu Eylemler: KV Sertifikası için ömür boyu eylemleri içerir. Her yaşam boyu eylem şunları içerir:  

     - Tetikleyici: son kullanma tarihinden veya ömür boyu yayılma yüzdesinden önceki gün üzerinden  

     - Eylem: eylem türünü belirtme – *emailContacts* veya *autoRenew*  

-   İhraççı: X509 sertifikası vermek için kullanılacak sertifikayı veren kuruluş la ilgili parametreler.  
-   İlke Öznitelikleri: ilke ile ilişkili öznitelikleri içerir  

#### <a name="x509-to-key-vault-usage-mapping"></a>X509 için Key Vault kullanım eşleme

Aşağıdaki tablo, X509 anahtar kullanım ilkesinin Anahtar Kasa sertifikası oluşturmanın bir parçası olarak oluşturulan bir anahtarın etkili anahtar işlemleriiçin eşlenemasını temsil eder.

|**X509 Anahtar Kullanım bayrakları**|**Anahtar Vault anahtar ops**|**Varsayılan davranış**|
|----------|--------|--------|
|DataEncipherment|şifrelemek, şifresini çözmek| Yok |
|Deşifre Sadece|Şifre -sini çöz| Yok  |
|Dijital İmza|imzalamak, doğrulamak| Sertifika oluşturma zamanında kullanım belirtimi olmadan Anahtar Kasa varsayılan | 
|EncipherOnly|şifrele| Yok |
|KeyCertSign|imzalamak, doğrulamak|Yok|
|KeyEncipherment|wrapKey, unwrapKey| Sertifika oluşturma zamanında kullanım belirtimi olmadan Anahtar Kasa varsayılan | 
|Inkar|imzalamak, doğrulamak| Yok |
|crlsign|imzalamak, doğrulamak| Yok |

### <a name="certificate-issuer"></a>Sertifika Veren

Anahtar Kasa sertifikası nesnesi, x509 sertifikaları sipariş etmek için seçili bir sertifika veren sağlayıcıyla iletişim kurmak için kullanılan bir yapılandırma tutar.  

-   TLS/SSL sertifikaları için aşağıdaki sertifika veren sağlayıcılara sahip Key Vault ortakları

|**Sağlayıcı Adı**|**Konumlar**|
|----------|--------|
|DigiCert|Genel bulut ve Azure Genel'deki tüm önemli kasa hizmet konumlarında desteklenir|
|Globalsign|Genel bulut ve Azure Genel'deki tüm önemli kasa hizmet konumlarında desteklenir|

Sertifika veren bir anahtar kasasında oluşturulamadan önce, ön koşul adım 1 ve 2'yi izleyerek başarılı bir şekilde gerçekleştirilmelidir.  

1. Sertifika Yetkilisi (CA) Sağlayıcılarına Dahili  

    -   Bir kuruluş yöneticisi kendi şirketinde olmalıdır (ör. Contoso) en az bir CA sağlayıcısı ile.  

2. Yönetici, TLS/SSL sertifikalarını kaydetmek (ve yenilemek) için Key Vault için istekte leyici kimlik bilgilerini oluşturur  

    -   Anahtar kasasında sağlayıcının bir veren nesnesini oluşturmak için kullanılacak yapılandırmayı sağlar  

Sertifikalar portalından İhraççı nesneleri oluşturma hakkında daha fazla bilgi [için, Anahtar Kasa Sertifikaları bloguna](https://aka.ms/kvcertsblog) bakın  

Key Vault, farklı veren sağlayıcı yapılandırmasına sahip birden çok veren nesnenin oluşturulmasına olanak tanır. İhraççı nesneoluşturulduktan sonra, adı bir veya birden çok sertifika ilkelerinde başvurulabilir. İhraççı nesnesine başvurmak, sertifika oluşturma ve yenileme sırasında CA sağlayıcısından x509 sertifikası nı talep ederken, Anahtar Kasası'na veren nesnede belirtildiği şekilde yapılandırmayı kullanmasını bildirir.  

İhraççı nesneler kasada oluşturulur ve sadece aynı kasada KV sertifikaları ile kullanılabilir.  

### <a name="certificate-contacts"></a>Sertifika kişileri

Sertifika kişileri, sertifika ömrü boyunca olan olaylar tarafından tetiklenen bildirimleri göndermek için kişi bilgilerini içerir. Kişi bilgileri, anahtar kasasındaki tüm sertifikalar tarafından paylaşılır. Anahtar kasasındaki herhangi bir sertifika için bir olay için belirtilen tüm kişilere bildirim gönderilir.  

Bir sertifikanın ilkesi otomatik yenileme olarak ayarlanmışsa, aşağıdaki olaylar hakkında bir bildirim gönderilir.  

- Sertifika yenilemeden önce
- Sertifika yenilemeden sonra, sertifikanın başarıyla yenilenip yenilenmedigini veya bir hata olup olmadığını belirterek, sertifikanın el ile yenilenmesini gerektirir.  

  El ile yenilenecek şekilde ayarlanmış bir sertifika ilkesi (yalnızca e-posta), sertifikayı yenileme zamanı geldiğinde bir bildirim gönderilir.  

### <a name="certificate-access-control"></a>Sertifika Erişim Kontrolü

 Sertifikalar için erişim denetimi Key Vault tarafından yönetilir ve bu sertifikaları içeren Key Vault tarafından sağlanır. Sertifikalar için erişim denetimi ilkesi, aynı Anahtar Kasası'ndaki anahtarlar ve sırlar için erişim denetimi ilkelerinden farklıdır. Kullanıcılar, senaryoya uygun segmentasyon ve sertifikaların yönetimini korumak için sertifika tutmak için bir veya daha fazla kasa oluşturabilir.  

 Aşağıdaki izinler, temel başına olarak, önemli bir kasadaki denetim girişine erişim sırlarda kullanılabilir ve gizli bir nesnede izin verilen işlemleri yakından yansıtmaktadır:  

- Sertifika yönetimi işlemleri için izinler
  - *get*: Geçerli sertifika sürümünü veya sertifikanın herhangi bir sürümünü alın 
  - *liste*: Geçerli sertifikaları veya sertifika sürümlerini listele  
  - *update*: Sertifikayı güncelleştir
  - *create*: Key Vault sertifikası oluşturma
  - *import*: Anahtar Kasa sertifikasına sertifika malzemesi alma
  - *delete*: Sertifikayı, ilkesini ve tüm sürümlerini silme  
  - *recover*: Silinen bir sertifikayı kurtarma
  - *yedekleme*: Bir sertifikayı anahtar kasasında yedekleme
  - *geri yükleme*: Yedeklenmiş bir sertifikayı anahtar kasasına geri yükleme
  - *kişileri yönetme*: Anahtar Kasa sertifikası ilgili kişileri yönetme  
  - *yönetenler*: Key Vault sertifika yetkililerini/ihraççılarını yönetin
  - *getissuers*: Sertifika yetkilileri / verenler alın
  - *listissuers*: Sertifika nın yetkili lerini/verenleri listele  
  - *setissuers*: Key Vault sertifikasının yetkilileri/verenleri oluşturma veya güncelleme  
  - *deleteissuers*: Key Vault sertifikasının yetkili makamlarını/verenlerini silme  
 
- Ayrıcalıklı işlemler için izinler
  - *temizleme*: Silinmiş bir sertifikayı temizleme (kalıcı olarak silme)

Daha fazla bilgi [için, Key Vault REST API başvurusundaki Sertifika işlemlerine](/rest/api/keyvault)bakın. İzinlerin oluşturulması hakkında daha fazla bilgi için [Vaults - Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy) [bkz.](/rest/api/keyvault/vaults/createorupdate)

## <a name="azure-storage-account-key-management"></a>Azure Depolama hesap anahtar yönetimi

Key Vault, Azure depolama hesabı anahtarlarını yönetebilir:

- Dahili olarak, Key Vault tuşlarını Bir Azure depolama hesabıyla listeleyebilir (eşitleyebilir). 
- Key Vault tuşları periyodik olarak yeniden üretir (döndürür).
- Anahtar değerler arayana yanıt olarak asla döndürülür.
- Key Vault, hem depolama hesaplarının hem de klasik depolama hesaplarının anahtarlarını yönetir.

Daha fazla bilgi için Azure [Anahtar Kasası Depolama Hesap Anahtarları'na](key-vault-ovw-storage-keys.md) bakın

### <a name="storage-account-access-control"></a>Depolama hesabı erişim kontrolü

Aşağıdaki izinler, bir kullanıcıya veya uygulama sorumlusuna yönetilen bir depolama hesabında işlemleri gerçekleştirmesi için yetki verirken kullanılabilir:  

- Yönetilen depolama hesabı ve SaS tanımlı işlemler için izinler
  - *get*: Depolama hesabı hakkında bilgi alır 
  - *liste*: Key Vault tarafından yönetilen depolama hesaplarını listele
  - *update*: Depolama hesabını güncelleştir
  - *delete*: Depolama hesabını silme  
  - *recover*: Silinen bir depolama hesabını kurtarma
  - *yedekleme*: Depolama hesabını yedekleme
  - *geri yükleme*: Yedeklenmiş bir depolama hesabını Key Vault'a geri yükleme
  - *set*: Bir depolama hesabı oluşturma veya güncelleme
  - *regeneratekey*: Depolama hesabı için belirtilen anahtar değerini yeniden oluşturma
  - *getsas*: Depolama hesabı için SAS tanımı hakkında bilgi alın
  - *listsas*: Depolama hesabı için depolama SAS tanımlarını listele
  - *deletesas*: Bir depolama hesabından SAS tanımını silme
  - *setsas*: Bir depolama hesabı için yeni bir SAS tanımı/öznitelikleri oluşturma veya güncelleştirme

- Ayrıcalıklı işlemler için izinler
  - *temizleme*: Yönetilen bir depolama hesabını temizleme (kalıcı olarak silme)

Daha fazla bilgi için [Key Vault REST API başvurusundaki Depolama hesabı işlemlerine](/rest/api/keyvault)bakın. İzinlerin oluşturulması hakkında daha fazla bilgi için [Vaults - Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy) [bkz.](/rest/api/keyvault/vaults/createorupdate)

## <a name="see-also"></a>Ayrıca Bkz.

- [Kimlik doğrulama, istekler ve yanıtlar](authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](/azure/key-vault/key-vault-developers-guide)
