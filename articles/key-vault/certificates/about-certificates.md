---
title: Azure Anahtar Kasası Sertifikaları Hakkında - Azure Anahtar Kasası
description: Azure Key Vault REST arabirimine ve sertifikalarına genel bakış.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: overview
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: e9fbe624563ad03880ff1a75efdc2df41b151846
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424282"
---
# <a name="about-azure-key-vault-certificates"></a>Azure Anahtar Kasası sertifikaları hakkında

Azure Key Vault, Microsoft Azure uygulamalarının ve kullanıcılarının anahtarların ve sırların üzerine inşa edilmiş sertifikaları depolamasına ve kullanmasına ve otomatik yenileme özelliği eklemesine olanak tanır.

Key Vault hakkında daha fazla bilgi için [Azure Anahtar Kasası nedir?](/azure/key-vault/key-vault-overview)

## <a name="azure-key-vault"></a>Azure Key Vault

Aşağıdaki bölümlerde Key Vault hizmetinin uygulanması nda geçerli olan genel bilgiler sunulmaktadır.

### <a name="supporting-standards"></a>Destekleyici standartlar

JavaScript Nesne Gösterimi (JSON) ve JavaScript Nesne İmzalama ve Şifreleme (JOSE) belirtimleri önemli arka plan bilgileridir.  

-   [JSON Web Anahtarı (JWK)](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41)  
-   [JSON Web Şifreleme (JWE)](https://tools.ietf.org/html/draft-ietf-jose-json-web-encryption-40)  
-   [JSON Web Algoritmaları (JWA)](https://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms-40)  
-   [JSON Web İmza (JWS)](https://tools.ietf.org/html/draft-ietf-jose-json-web-signature-41)  

### <a name="objects-identifiers-and-versioning"></a>Nesneler, tanımlayıcılar ve sürüm

Key Vault'ta depolanan nesneler, yeni bir nesne örneği oluşturulduğunda sürülür. Her sürüme benzersiz bir tanımlayıcı ve URL atanır. Bir nesne ilk oluşturulduğunda, özgün bir sürüm tanımlayıcısı verilir ve nesnenin geçerli sürümü olarak işaretlenir. Aynı nesne adı ile yeni bir örnek oluşturma yeni nesne özgün bir sürüm tanımlayıcısı verir, geçerli sürümü haline neden.  

Key Vault'taki nesneler geçerli tanımlayıcı veya sürüme özgü tanımlayıcı kullanılarak ele alınabilir. Örneğin, adla `MasterKey`birlikte bir Anahtar verildiğinde, geçerli tanımlayıcıyla işlemleri gerçekleştirmek, sistemin kullanılabilir en son sürümü kullanmasına neden olur. Sürüme özgü tanımlayıcıile işlemleri gerçekleştirmek, sistemin nesnenin belirli bir sürümünü kullanmasına neden olur.  

Nesneler, Key Vault içinde bir URL kullanılarak benzersiz bir şekilde tanımlanır. Sistemdeki hiçbir iki nesne, coğrafi konuma bakılmaksızın aynı URL'ye sahip değildir. Bir nesnenin tam URL'si Nesne Tanımlayıcısı olarak adlandırılır. URL, Anahtar Kasası' nı, nesne türünü, kullanıcıtarafından sağlanan Nesne Adı'nı ve Nesne Sürümünü tanımlayan bir önekten oluşur. Nesne Adı büyük/küçük harf duyarsız ve değişmez. Nesne Sürümü içermeyen tanımlayıcılara Temel Tanımlayıcılar denir.  

Daha fazla bilgi için kimlik [doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)

Nesne tanımlayıcısı aşağıdaki genel biçimi vardır:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Konumlar:  

|||  
|-|-|  
|`keyvault-name`|Microsoft Azure Key Vault hizmetindeki önemli bir kasanın adı.<br /><br /> Anahtar Vault adları kullanıcı tarafından seçilir ve genel olarak benzersizdir.<br /><br /> Anahtar Vault adı sadece 0-9, a-z, A-Z ve -içeren bir 3-24 karakter dize olmalıdır.|  
|`object-type`|Nesnenin türü, ya "anahtarlar" ya da "sırlar".|  
|`object-name`|A, `object-name` kullanıcıadına verilen bir addır ve Bir Anahtar Kasası içinde benzersiz olmalıdır. Ad, yalnızca 0-9, a-z, A-Z ve -'yi içeren 1-127 karakterli bir dize olmalıdır.|  
|`object-version`|An, `object-version` isteğe bağlı olarak bir nesnenin benzersiz bir sürümünü adreslemek için kullanılan bir sistem tarafından oluşturulan, 32 karakter dize tanımlayıcısıdır.|  


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
 
![Sertifikalar karmaşık nesnelerdir](../media/azure-key-vault.png)

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

## <a name="see-also"></a>Ayrıca Bkz.

- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](../general/developers-guide.md)
