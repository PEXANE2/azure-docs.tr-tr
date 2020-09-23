---
title: Anahtarlar hakkında-Azure Key Vault
description: Anahtarlar için Azure Key Vault REST arabirimine ve geliştirici ayrıntılarına genel bakış.
services: key-vault
author: amitbapat
manager: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: overview
ms.date: 09/15/2020
ms.author: ambapat
ms.openlocfilehash: 29930a835297b0ddd3a91534dab9ccb6d74896e3
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90967553"
---
# <a name="about-keys"></a>Anahtarlar hakkında

Azure Key Vault, şifreleme anahtarlarını depolamak ve yönetmek için iki tür kaynak sağlar:

|Kaynak türü|Anahtar koruma yöntemleri|Veri düzlemi uç noktası temel URL 'SI|
|--|--|--|
| **Kasalar** | Yazılım korumalı<br/><br/>ve<br/><br/>HSM korumalı (Premium SKU ile)</li></ul> | https://{kasa-adı}. kasa. Azure. net |
| **Yönetilen HSM havuzları** | HSM korumalı | https://{HSM-adı}. managedhsm. Azure. net |
||||

- **Vaults** Kasaların kasalarına göre düşük maliyetli, kolay bir dağıtım, çok kiracılı, bölge açısından dayanıklı (kullanılabilir yerlerde), en yaygın bulut uygulaması senaryoları için uygun olan yüksek düzeyde kullanılabilir anahtar yönetimi çözümü sağlar.
- **Yönetilen hsms** tarafından yönetilen HSM, şifreleme anahtarlarınızı depolamak ve yönetmek için tek kiracılı, bölge dayanıklı (kullanılabilir), yüksek oranda kullanılabilir HSM 'ler sağlar. Yüksek değerli anahtarları işleyen uygulamalar ve kullanım senaryoları için en uygun. Ayrıca, en sıkı güvenlik, uyumluluk ve mevzuat gereksinimlerini karşılamanıza yardımcı olur. 

> [!NOTE]
> Kasalar, şifreleme anahtarlarının yanı sıra gizli dizileri, sertifikaları ve depolama hesabı anahtarlarını gibi çeşitli nesne türlerini depolamanıza ve yönetmenize de olanak tanır.

Key Vault içindeki şifreleme anahtarları JSON Web anahtarı [JWK] nesneleri olarak gösterilir. JavaScript Nesne Gösterimi (JSON) ve JavaScript nesne Imzalama ve şifreleme (Joo) belirtimleri şunlardır:

-   [JSON Web anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [JSON Web şifrelemesi (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [JSON Web algoritmaları (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [JSON Web Imzası (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature) 

Temel JWK/JWA belirtimleri, Azure Key Vault ve yönetilen HSM uygulamalarına özgü anahtar türlerini etkinleştirmek için de genişletilir. 

HSM korumalı anahtarlar (HSM anahtarları olarak da adlandırılır) bir HSM 'de (donanım güvenlik modülü) işlenir ve her zaman HSM koruma sınırı kalır. 

- Kasaların paylaşılan HSM arka uç altyapısında HSM anahtarlarını korumak için **fıps 140-2 düzey 2** doğrulanan HSM 'leri kullanın. 
- Yönetilen HSM havuzları, anahtarlarınızı korumak için **fıps 140-2 düzey 3** doğrulanan HSM modüllerini kullanır. Her HSM havuzu, aynı donanım altyapısını paylaşan diğer tüm HSM havuzlarından tam şifreleme yalıtımı sağlayan kendi [güvenlik etki alanı](../managed-hsm/security-domain.md) ile yalıtılmış bir tek kiracılı örneğidir.

Bu anahtarlar tek kiracılı HSM havuzları halinde korunur. Bir RSA, EC ve simetrik anahtarı, yumuşak biçimde veya desteklenen bir HSM cihazından dışarı aktararak içeri aktarabilirsiniz. Ayrıca HSM havuzlarında anahtarlar da oluşturabilirsiniz. , [BYok (kendi anahtarını getir) belirtiminde](../keys/byok-specification.md)açıklanan yöntemi kullanarak, anahtarları kullanarak HSM anahtarlarını içeri aktardığınızda, güvenli ulaşım anahtar MALZEMESINI yönetilen HSM havuzlarına dönüştürür. 

Coğrafi sınırlar hakkında daha fazla bilgi için bkz. [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/privacy/)

## <a name="key-types-protection-methods-and-algorithms"></a>Anahtar türleri, koruma yöntemleri ve algoritmaları

Key Vault RSA, EC ve simetrik anahtarları destekler. 

### <a name="hsm-protected-keys"></a>HSM ile korunan anahtarlar

|Anahtar türü|Kasa (yalnızca Premium SKU)|Yönetilen HSM havuzları|
|--|--|--|--|
**EC-HSM**: eliptik eğri tuşu|FIPS 140-2 düzey 2 HSM|FIPS 140-2 düzey 3 HSM
**RSA-HSM**: RSA anahtarı|FIPS 140-2 düzey 2 HSM|FIPS 140-2 düzey 3 HSM
**EKI HSM**: simetrik|Desteklenmez|FIPS 140-2 düzey 3 HSM
||||

### <a name="software-protected-keys"></a>Yazılım korumalı anahtarlar

|Anahtar türü|Kasalar|Yönetilen HSM havuzları|
|--|--|--|--|
**RSA**: "yazılım KORUMALı" RSA anahtarı|FIPS 140-2 düzey 1|Desteklenmez
**EC**: "Yazılım korumalı" eliptik eğri anahtarı|FIPS 140-2 düzey 1|Desteklenmez
||||

### <a name="supported-algorithms"></a>Desteklenen algoritmalar

|Anahtar türleri/boyutları/eğrileri| Şifreleme/şifre çözme<br>(Sarmalama/sarmalama) | İmzala/doğrula | 
| --- | --- | --- |
|EC-P256, EC-P256K, EC-P384, EC-521|NA|ES256<br>ES256K<br>ES384<br>ES512|
|RSA 2K, 3K, 4K| RSA1_5<br>RSA-OAEP<br>RSA-OAEP-256|PS256<br>PS384<br>PS512<br>RS256<br>RS384<br>RS512<br>RSNULL| 
|AES 128 bit, 256 bit| AES-KW<br>AES-GCM<br>AES-CBC| NA| 
|||

###  <a name="ec-algorithms"></a>EC algoritmaları
 Aşağıdaki algoritma tanımlayıcıları EC-HSM anahtarlarıyla desteklenir

#### <a name="curve-types"></a>Eğri türleri

-   **P-256** - [DSS FIPS Yayın 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)' de tanımlanan nıst eğrisi p-256.
-   **P-256k** -sn eğrisi SECP256K1, tanımlanan, [2: önerilen eliptik eğri etki alanı parametreleri](https://www.secg.org/sec2-v2.pdf).
-   **P-384** - [DSS FIPS Yayın 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)' de tanımlanan nıst eğrisi p-384.
-   **P-521** - [DSS FIPS Yayın 186-4](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)' de tanımlanan nıst eğrisi p-521.

#### <a name="signverify"></a>IMZALA/DOĞRULA

-   SHA-256 özetleri için **ES256** -ECDSA ve P-256 eğrisi ile oluşturulan anahtarlar. Bu algoritma [RFC7518](https://tools.ietf.org/html/rfc7518)adresinde açıklanmaktadır.
-   SHA-256 özetleri için **ES256K** -ECDSA ve P-256k eğrisi ile oluşturulan anahtarlar. Bu algoritma bekleyen standartmadır.
-   SHA-384 özetleri için **ES384** -ECDSA ve P-384 eğrisi ile oluşturulan anahtarlar. Bu algoritma [RFC7518](https://tools.ietf.org/html/rfc7518)adresinde açıklanmaktadır.
-   SHA-512 özetleri için **ES512** -ECDSA ve P-521 eğrisi ile oluşturulan anahtarlar. Bu algoritma [RFC7518](https://tools.ietf.org/html/rfc7518)adresinde açıklanmaktadır.

###  <a name="rsa-algorithms"></a>RSA algoritmaları  
 Aşağıdaki algoritma tanımlayıcıları RSA ve RSA-HSM anahtarları ile desteklenir  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY, ŞIFRELEME/ŞIFRE ÇÖZME

-   **RSA1_5** -rsaes-PKCS1-V1_5 [RFC3447] anahtar şifrelemesi  
-   **Rsa-oaep** -bir. 2.1 bölümünde RFC 3447 tarafından belirtilen varsayılan parametrelerle En Iyi asimetrik şifreleme dolgusu (OAEP) [RFC3447] kullanılarak rsaes. Bu varsayılan parametreler, SHA-1 ' in bir karma işlevini ve MGF1 'in bir SHA-1 ile bir maske oluşturma işlevini kullanıyor.  
-  **RSA-OAEP-256** – bir sha-256 karma Işlevi Ile En Iyi asimetrik şifreleme DOLDURMAYı ve sha-256 MGF1 'in bir maske oluşturma işlevini kullanarak rsaes

#### <a name="signverify"></a>IMZALA/DOĞRULA

-   **PS256** -RSASSA-PSS, [RFC7518](https://tools.ietf.org/html/rfc7518)' de açıklandığı gibi SHA-256 ve MGF1 kullanarak SHA-256 ile.
-   **PS384** -RSASSA-PSS, [RFC7518](https://tools.ietf.org/html/rfc7518)' de açıklandığı gibi SHA-384 ve MGF1 kullanarak SHA-384 ile.
-   **PS512** -RSASSA-PSS, [RFC7518](https://tools.ietf.org/html/rfc7518)' de açıklandığı gibi SHA-512 ve MGF1 kullanarak SHA-512 ile.
-   **RS256** -RSASSA-PKCS-v1_5 SHA-256 kullanılarak. Uygulamanın sağladığı Özet değeri SHA-256 kullanılarak hesaplanmalıdır ve 32 bayt uzunluğunda olmalıdır.  
-   **RS384** -RSASSA-PKCS-v1_5 SHA-384 kullanılarak. Uygulamanın sağladığı Özet değeri SHA-384 kullanılarak hesaplanmalıdır ve 48 bayt uzunluğunda olmalıdır.  
-   **RS512** -RSASSA-PKCS-v1_5 SHA-512 kullanılarak. Uygulamanın sağladığı Özet değeri SHA-512 kullanılarak hesaplanmalıdır ve 64 bayt uzunluğunda olmalıdır.  
-   **Rsnull** -belirli TLS senaryolarını etkinleştirmek için özel bir kullanım örneği olan [RFC2437](https://tools.ietf.org/html/rfc2437)bakın.  

###  <a name="symmetric-key-algorithms"></a>Simetrik anahtar algoritmaları
- **AES-kW** -AES anahtar sarması ([RFC3394](https://tools.ietf.org/html/rfc3394)).
- Galos sayaç modunda **AES-GCM** -AES şifrelemesi ([NIST SP800-38d](https://csrc.nist.gov/publications/sp800))
- **AES-CBC** -AES şifrelemesi, Şifre blok zincirleme modunda ([NIST SP800-38A](https://csrc.nist.gov/publications/sp800))

> [!NOTE] 
> Geçerli AES-GCM uygulama ve ilgili API 'Ler deneysel. Uygulama ve API 'Ler, gelecekteki yinelemelerde önemli ölçüde değişebilir. 

##  <a name="key-operations"></a>Anahtar işlemleri

Yönetilen HSM, anahtar nesnelerinde aşağıdaki işlemleri destekler:  

-   **Oluştur**: bir istemcinin Key Vault bir anahtar oluşturmasına izin verir. Anahtarın değeri Key Vault tarafından oluşturulur ve depolanır ve istemciye serbest bırakılır. Asimetrik anahtarlar Key Vault oluşturulabilir.  
-   **Içeri aktarma**: bir istemcinin mevcut bir anahtarı Key Vault içeri aktarmasını sağlar. Asimetrik anahtarlar, bir JWK yapısı içinde çok sayıda farklı paketleme yöntemi kullanılarak Key Vault içeri aktarılabilir. 
-   **Güncelleştirme**: daha önce Key Vault içinde depolanan bir anahtarla ilişkili meta verileri (anahtar öznitelikleri) değiştirmek için yeterli izinlere sahip bir istemciye izin verir.  
-   **Sil**: Key Vault ' dan bir anahtarı silmek için yeterli izinlere sahip bir istemciye izin verir.  
-   **Liste**: bir istemcinin belirli bir Key Vault tüm anahtarları listeetmesine izin verir.  
-   **Sürümleri Listele**: bir istemcinin belirli bir Key Vault verilen bir anahtarın tüm sürümlerini listeetmesine izin verir.  
-   **Get**: bir istemcinin, bir Key Vault verilen bir anahtarın ortak parçalarını almasına izin verir.  
-   **Yedekleme**: bir anahtarı korumalı bir biçimde dışa aktarır.  
-   **Geri yükle**: daha önce yedeklenen bir anahtarı içeri aktarır.  

Daha fazla bilgi için [Key Vault REST API başvuru Içindeki anahtar işlemleri](/rest/api/keyvault)konusuna bakın.  

Key Vault bir anahtar oluşturulduktan sonra anahtar kullanılarak aşağıdaki şifreleme işlemleri gerçekleştirilebilir:  

-   **İmzala ve Doğrula**: kesinlikle, bu işlem "imza karması" veya "doğrulama karması" olduğundan Key Vault imza oluşturma işleminin bir parçası olarak içeriğin karmasını desteklemez. Uygulamalar yerel olarak imzalanacak verileri karma hale almalıdır, sonra Key Vault karmayı imzalamasını ister. İmzalı karmaların doğrulanması, [Genel] anahtar malzemesine erişimi olmayan uygulamalar için kolaylık olarak desteklenir. En iyi uygulama performansı için, işlemlerin yerel olarak gerçekleştirilmesi gerektiğini doğrulayın.  
-   **Anahtar şifreleme/sarmalama**: Key Vault depolanan bir anahtar, genellikle bir simetrik içerik şifreleme anahtarı (cek) ile başka bir anahtarı korumak için kullanılabilir. Key Vault anahtarı asimetrik olduğunda, anahtar şifreleme kullanılır. Örneğin, RSA-OAEP ve WRAPKEY/UNWRAPKEY işlemleri ŞIFRELEME/ŞIFRE çözme ile eşdeğerdir. Key Vault anahtar simetrik olduğunda, anahtar kaydırma kullanılır. Örneğin, AES-KW. WRAPKEY işlemi, [public] anahtar malzemesine erişimi olmayan uygulamalar için kolaylık olarak desteklenir. En iyi uygulama performansı için WRAPKEY işlemleri yerel olarak gerçekleştirilmelidir.  
-   **Şifreleme ve şifre çözme**: Key Vault depolanan bir anahtar, tek bir veri bloğunu şifrelemek veya şifresini çözmek için kullanılabilir. Bloğun boyutu anahtar türüne ve seçili şifreleme algoritmasına göre belirlenir. Şifreleme işlemi, [Genel] anahtar malzemesine erişimi olmayan uygulamalar için kolaylık sağlaması için sağlanır. En iyi uygulama performansı için, ŞIFRELEME işlemleri yerel olarak gerçekleştirilmelidir.  

Asimetrik anahtarları kullanarak WRAPKEY/UNWRAPKEY gereksiz görünebilir (işlem, ŞIFRELEME/ŞIFRE çözme ile eşdeğerdir), farklı işlemlerin kullanılması önemlidir. Ayrım, bu işlemlere yönelik anlam ve yetkilendirme ayrımı sağlar ve hizmet tarafından diğer anahtar türleri desteklenirse tutarlılığa sahiptir.  

Key Vault dışarı aktarma işlemlerini desteklemez. Sistemde bir anahtar sağlandıktan sonra, bu ayıklanamaz veya anahtar malzemeleri değiştirilemez. Ancak, Key Vault kullanıcıları, oluşturulduktan sonra gibi diğer kullanım durumları için kendi anahtarını gerektirebilir. Bu durumda, anahtarı korumalı bir biçimde dışa/dışarı aktarmak için yedekleme ve GERI yükleme işlemlerini kullanabilirler. Yedekleme işlemi tarafından oluşturulan anahtarlar Key Vault dışında kullanılamaz. Alternatif olarak, IÇERI aktarma işlemi birden çok Key Vault örneğine karşı kullanılabilir.  

Kullanıcılar, JWK nesnesinin key_ops özelliğini kullanarak anahtar başına Key Vault desteklediği herhangi bir şifreleme işlemini kısıtlayabilir.  

JWK nesneleri hakkında daha fazla bilgi için bkz. [JSON Web anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41).  

## <a name="key-attributes"></a>Anahtar öznitelikleri

Anahtar malzemeye ek olarak aşağıdaki öznitelikler de belirtilebilir. Bir JSON Isteğinde, belirtilen öznitelik olmadığından bile ' {' '} ' öznitelikler anahtar sözcüğü ve ayraçları gereklidir.  

- *etkin*: Boolean, isteğe bağlı, varsayılan değer **true**'dur. Anahtarın etkinleştirilip etkinleştirilmeyeceğini ve şifreleme işlemleri için kullanılıp kullanılamayacağını belirtir. *Enabled* özniteliği, *NBF* ve *Exp*ile birlikte kullanılır. *NBF* ve *Exp*arasında bir işlem olduğunda, yalnızca *etkin* değeri **true**olarak ayarlandıysa izin verilir. *nbf*  /  [Belirli koşullarda](#date-time-controlled-operations)belirli işlem türleri dışında, NBF*Exp* penceresinin dışındaki işlemlere otomatik olarak izin verilmez.
- *NBF*: intdate, isteðe baðlý, default artık. *NBF* (before) özniteliği, [belirli koşullarda](#date-time-controlled-operations)belirli işlem türleri dışında, anahtarın ŞIFRELEME işlemleri için kullanılması gereken süreyi tanımlar. *NBF* özniteliğinin işlenmesi, geçerli tarih/saatin, *NBF* özniteliğinde listelenen son tarih/saat değerinden önce veya buna eşit olması gerekir. Key Vault saat eğimlerini hesaba çıkarak, genellikle birkaç dakikadan daha uzun bir süre boyunca daha fazla bilgi verebilir. Değeri, bir IntDate değeri içeren bir sayı OLMALıDıR.  
- *Exp*: intdate, isteðe baðlý, Default, "sonsuza kadar". *Exp* (sona erme saati) özniteliği, [belirli koşullarda](#date-time-controlled-operations)belirli işlem türleri dışında, anahtarın ŞIFRELEME işlemi için kullanılması gereken süre sonu zamanını belirler. *Exp* özniteliğinin işlenmesi, geçerli tarih/saatin, *Exp* özniteliğinde listelenen süre sonu tarih/saatinden önce olması gerekir. Key Vault, saat eğimlerini hesaba çıkarak, genellikle birkaç dakikadan daha kısa bir süre boyunca daha fazla bilgi sağlayabilir. Değeri, bir IntDate değeri içeren bir sayı OLMALıDıR.  

Anahtar öznitelikleri içeren herhangi bir yanıtta yer alan ek salt okuma öznitelikleri vardır:  

- *oluşturma*: Int32 Tarih, isteğe bağlı. *Oluşturulan* öznitelik, anahtarın bu sürümünün ne zaman oluşturulduğunu gösterir. Bu özniteliğin eklenmesinden önce oluşturulan anahtarlar için değer null. Değeri, bir IntDate değeri içeren bir sayı OLMALıDıR.  
- *güncelleştirildi*: Int32 tarihi, isteğe bağlı. *Updated* özniteliği, anahtarın bu sürümünün ne zaman güncelleştirildiğini gösterir. Bu özniteliğin eklenmesiyle önce en son güncellenen anahtarlar için değer null. Değeri, bir IntDate değeri içeren bir sayı OLMALıDıR.  

IntDate ve diğer veri türleri hakkında daha fazla bilgi için bkz. [Anahtarlar, gizlilikler ve sertifikalar hakkında: [veri türleri](../general/about-keys-secrets-certificates.md#data-types).

### <a name="date-time-controlled-operations"></a>Tarih-saat denetimli işlemler

Henüz yok-geçerli olmayan ve süresi dolmayan anahtarlar, *NBF*  /  *Exp* penceresi dışında, **şifre çözme**, **sarmalama**ve **doğrulama** işlemleri için çalışır (403 döndürmez, yasak olur). Henüz geçerli olmayan durumu kullanmaya yönelik korvaale, bir anahtarın üretim kullanılmadan önce test edilmeye izin vermektir. Kullanım tarihi geçen durum kullanımı için, anahtar geçerli olduğunda oluşturulan verilerdeki kurtarma işlemlerine izin vermektir. Ayrıca, Key Vault ilkeleri kullanarak bir anahtara erişimi devre dışı bırakabilir ya da *etkin* anahtar özniteliğini **yanlış**olarak güncelleyerek.

Veri türleri hakkında daha fazla bilgi için bkz. [veri türleri](../general/about-keys-secrets-certificates.md#data-types).

Diğer olası öznitelikler hakkında daha fazla bilgi için bkz. [JSON Web anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41).

## <a name="key-tags"></a>Anahtar etiketleri

Etiketler biçiminde uygulamaya özgü ek meta verileri belirtebilirsiniz. Key Vault, her birinin 256 karakter adı ve 256 karakter değeri olabilen en fazla 15 etiketi destekler.  

> [!NOTE] 
> Etiketler, bu anahtara bir *liste* veya *Al* izni varsa, bir arayan tarafından okunabilir.

##  <a name="key-access-control"></a>Anahtar erişim denetimi

Key Vault tarafından yönetilen anahtarlar için erişim denetimi, anahtarların kapsayıcısı olarak davranan bir Key Vault düzeyinde sağlanır. Anahtarlar için erişim denetimi ilkesi, aynı Key Vault gizli dizi için erişim denetimi ilkesinden farklıdır. Kullanıcılar anahtarları tutmak için bir veya daha fazla kasa oluşturabilir ve senaryonun uygun segmentlerini ve anahtarların yönetimini sürdürmek için gereklidir. Anahtarlar için erişim denetimi, gizli dizileri için erişim denetiminden bağımsızdır.  

Bir kasadaki anahtarlar erişim denetimi girişinde, Kullanıcı başına/hizmet sorumlusu temelinde aşağıdaki izinler verilebilir. Bu izinler, anahtar nesnesinde izin verilen işlemleri yakından yansıtır.  Anahtar kasasındaki bir hizmet sorumlusuna erişim verilmesi, kerelik bir işlemdir ve tüm Azure abonelikleri için aynı kalacaktır. Bunu istediğiniz sayıda sertifika dağıtmak için kullanabilirsiniz. 

- Anahtar yönetimi işlemlerine yönelik izinler
  - *Al*: bir anahtarın ortak bölümünü ve özniteliklerini okuyun
  - *liste*: anahtar kasasında depolanan bir anahtarın anahtarlarını veya sürümlerini listeleyin
  - *güncelleştirme*: bir anahtarın özniteliklerini güncelleştirme
  - *Oluştur*: yeni anahtar oluştur
  - *içeri aktarma*: anahtar kasasına anahtar aktarma
  - *Sil*: anahtar nesnesini Sil
  - *kurtar*: silinen bir anahtarı kurtar
  - *yedekleme*: anahtar kasasında anahtar yedekleme
  - *geri yükleme*: Yedeklenen anahtarı bir anahtar kasasına geri yükleme

- Şifreleme işlemlerine yönelik izinler
  - *şifre çözme*: bir bayt dizisinin korumasını kaldırmak için anahtarı kullanın
  - *şifreleme*: rastgele bir bayt dizisini korumak için anahtarı kullanın
  - *unwrapKey*: Sarmalanan simetrik anahtarların korumasını kaldırmak için anahtarı kullanın
  - *wrapKey*: bir simetrik anahtarı korumak için anahtarı kullanın
  - *Doğrula*: özetleri 'yi doğrulamak için anahtarı kullanın  
  - *imzala*: anahtarı kullanarak özetleri 'yi imzalayın
    
- Ayrıcalıklı işlemler için izinler
  - *Temizle*: silinen bir anahtarı temizle (kalıcı olarak sil)

Anahtarlarla çalışma hakkında daha fazla bilgi için bkz. [Key Vault REST API başvurusu Içindeki anahtar işlemleri](/rest/api/keyvault). İzinleri oluşturma hakkında bilgi için bkz. [kasa-oluşturma veya güncelleştirme](/rest/api/keyvault/vaults/createorupdate) ve [kasa-güncelleştirme erişim ilkesi](/rest/api/keyvault/vaults/updateaccesspolicy). 

## <a name="next-steps"></a>Sonraki adımlar
- [Key Vault Hakkında](../general/overview.md)
- [Yönetilen HSM hakkında](../managed-hsm/overview.md)
- [Gizli diziler hakkında](../secrets/about-secrets.md)
- [Sertifikalar hakkında](../certificates/about-certificates.md)
- [Key Vault REST API genel bakış](../general/about-keys-secrets-certificates.md)
- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](../general/developers-guide.md)