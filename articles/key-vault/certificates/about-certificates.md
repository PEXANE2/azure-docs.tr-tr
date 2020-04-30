---
title: Azure Key Vault sertifikaları hakkında-Azure Key Vault
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
ms.openlocfilehash: 5e014634ecb251f05710de16daee30d72dae619e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81685904"
---
# <a name="about-azure-key-vault-certificates"></a>Azure Key Vault sertifikaları hakkında

Key Vault sertifikaları, x509 sertifikalarınızın ve aşağıdaki davranışların yönetimi için destek sağlar:  

-   Bir sertifika sahibinin Key Vault oluşturma işlemiyle veya var olan bir sertifikayı içeri aktarmasından sertifika oluşturmasına izin verir. Hem otomatik olarak imzalanan hem de sertifika yetkilisi tarafından oluşturulan sertifikaları içerir.
-   Bir Key Vault sertifika sahibinin, özel anahtar malzemesiyle etkileşime girmeden x509 sertifikalarının güvenli depolanmasını ve yönetimini uygulamasına olanak tanır.  
-   Bir sertifika sahibinin, bir sertifikanın yaşam döngüsünü yönetmesi için Key Vault yönlendiren bir ilke oluşturmasına izin verir.  
-   Sertifika sahiplerinin, sertifika süresinin dolma ve yenilemenin yaşam döngüsü olayları hakkında bildirim için iletişim bilgilerini sağlamasına izin verir.  
-   Seçili verenler ile otomatik yenilemeyi destekler-Key Vault iş ortağı x509 sertifika sağlayıcıları/sertifika yetkilileri.

>[!Note]
>İş ortağı olmayan sağlayıcılara/yetkililere da izin verilir, ancak otomatik yenileme özelliğini desteklemez.

## <a name="composition-of-a-certificate"></a>Sertifika oluşturma

Bir Key Vault sertifikası oluşturulduğunda, aynı ada sahip bir adreslenebilir anahtar ve gizli dizi da oluşturulur. Key Vault anahtarı, anahtar işlemlerine izin verir ve Key Vault gizli dizi, sertifika değerinin gizli olarak alınmasına izin verir. Bir Key Vault sertifikası ortak x509 sertifika meta verilerini de içerir.  

Sertifikaların tanımlayıcısı ve sürümü anahtarlar ve gizli dizilerle benzerdir. Adreslenebilir bir anahtarın ve bir parolanın Key Vault sertifikası sürümüyle oluşturulmuş bir gizli sürümü Key Vault sertifika yanıtında bulunabilir.
 
![Sertifikalar karmaşık nesnelerdir](../media/azure-key-vault.png)

## <a name="exportable-or-non-exportable-key"></a>Dışarı aktarılabilir veya dışarı aktarılabilir olmayan anahtar

Bir Key Vault sertifikası oluşturulduğunda, bu, özel anahtarla birlikte adreslenebilir gizli kümesinden PFX veya ped biçiminde alınabilir. Sertifikayı oluşturmak için kullanılan ilke, anahtarın dışarı aktarılabilir olduğunu göstermelidir. İlke dışa aktarılabilir olduğunu gösteriyorsa, özel anahtar, gizli dizi olarak alındığında değerin bir parçası değildir.  

Adreslenebilir anahtar, dışarı aktarılabilir KV sertifikalarıyla daha da alakalı olur. Adreslenebilir kV anahtarının işlemleri, KV sertifikasını oluşturmak için kullanılan kV sertifika ilkesinin *KeyUsage uzantısında* alanından eşleştirilir.  

İki tür anahtar, sertifikalarla birlikte desteklenir – *RSA* veya *RSA HSM* . Dışarı aktarılabilir yalnızca RSA ile kullanılabilir, RSA HSM tarafından desteklenmez.  

## <a name="certificate-attributes-and-tags"></a>Sertifika öznitelikleri ve Etiketler

Bir Key Vault sertifikası, sertifika meta verilerine, adreslenebilir bir anahtara ve adreslenebilir gizliliğe ek olarak öznitelikler ve Etiketler de içerir.  

### <a name="attributes"></a>Öznitelikler

Sertifika öznitelikleri, bir adreslenebilir anahtar ve KV sertifikası oluşturulduğunda oluşturulan gizli anahtar özniteliklerine yansıtılır.  

Bir Key Vault sertifikası aşağıdaki özniteliklere sahiptir:  

-   *etkin*: Boolean, isteğe bağlı, varsayılan değer **true**'dur. , Sertifika verilerinin anahtar olarak gizli veya çalıştırılabilir olarak alınıp alınamayacağını göstermek için belirtilebilir. Ayrıca *, NBF ve* *Exp*arasında bir işlem gerçekleştiğinde, *NBF* ve *Exp* ile birlikte de kullanılır ve yalnızca etkin değeri true olarak ayarlandıysa izin verilir. *NBF* ve *Exp* penceresinin dışındaki işlemlere otomatik olarak izin verilmez.  

Yanıtta bulunan ek salt okuma öznitelikleri vardır:

-   *oluşturma*: intdate: sertifikanın bu sürümünün ne zaman oluşturulduğunu gösterir.  
-   *güncelleştirildi*: intdate: sertifikanın bu sürümünün ne zaman güncelleştirildiğini gösterir.  
-   *Exp*: intdate: x509 sertifikasının süre sonu tarihinin değerini içerir.  
-   *NBF*: intdate: x509 sertifikası tarihinin değerini içerir.  

> [!Note] 
> Bir Key Vault sertifikasının süresi dolarsa, adreslenebilir anahtar ve gizli dizi çalışamaz duruma gelir.  

### <a name="tags"></a>Etiketler

 İstemci, anahtarlar ve gizli dizileri içindeki etiketlere benzer şekilde anahtar değer çiftlerinin belirtilen sözlüğünü belirtti.  

 > [!Note]
> Bu nesne türü (anahtarlar, gizlilikler veya sertifikalar) için *liste* veya *Al* izni varsa, Etiketler bir çağıran tarafından okunabilir.

## <a name="certificate-policy"></a>Sertifika ilkesi

Sertifika ilkesi, bir Key Vault sertifikasının yaşam döngüsünü oluşturma ve yönetme hakkında bilgiler içerir. Özel anahtara sahip bir sertifika anahtar kasasına aktarıldığında, x509 sertifikası okunarak varsayılan bir ilke oluşturulur.  

Sıfırdan bir Key Vault sertifikası oluşturulduğunda, bir ilkenin sağlanması gerekir. İlke, bu Key Vault sertifikası sürümünün veya sonraki Key Vault sertifika sürümünün nasıl oluşturulacağını belirtir. Bir ilke kurulduktan sonra, gelecekteki sürümler için art arda oluşturma işlemlerinde bu gerekli değildir. Bir Key Vault sertifikasının tüm sürümleri için bir ilkenin yalnızca bir örneği bulunur.  

Yüksek düzeyde, bir sertifika ilkesi aşağıdaki bilgileri içerir:  

-   X509 sertifika özellikleri: bir x509 sertifika isteği oluşturmak için kullanılan konu adını, konu diğer adlarını ve diğer özellikleri Içerir.  
-   Anahtar özellikleri: anahtar türü, anahtar uzunluğu, dışarı aktarılabilir ve anahtar alanlarını yeniden kullanır. Bu alanlar anahtar kasasının nasıl oluşturulacağını gösteren anahtar kasasına yönlendirir.  
-   Gizli özellikler: sertifikayı gizli olarak almak için, gizli değeri oluşturmak üzere adreslenebilir gizli dizi içerik türü gibi gizli özellikler içerir.  
-   Ömür Işlemleri: KV sertifikası için ömür eylemleri içerir. Her ömür eylemi şunları içerir:  

     - Tetikleyici: süre sonu veya yaşam süresi yüzdesi olarak belirtilen günler üzerinden belirtildi  

     - Eylem: eylem türü belirtme – *Emailcontacts* veya *autoRenew*  

-   Veren: x509 sertifikaları vermek için kullanılacak sertifika verenle ilgili parametreler.  
-   İlke öznitelikleri: ilkeyle ilişkili öznitelikleri içerir  

### <a name="x509-to-key-vault-usage-mapping"></a>Key Vault kullanım eşlemesine x509

Aşağıdaki tablo, x509 anahtar kullanımı ilkesinin Key Vault sertifikası oluşturma kapsamında oluşturulan bir anahtarın etkin anahtar işlemlerine eşlenmesini temsil eder.

|**X509 anahtar kullanım bayrakları**|**Key Vault anahtar Ops**|**Varsayılan davranış**|
|----------|--------|--------|
|Veri şifreleme|şifreleme, şifre çözme| Yok |
|Yalnızca çözülemez|çözülemiyor| Yok  |
|DigitalSignature|imzala, Doğrula| Sertifika oluşturma sırasında kullanım belirtimi olmadan varsayılan Key Vault | 
|Yalnızca şifreleme|şifrele| Yok |
|KeyCertSign|imzala, Doğrula|Yok|
|KeyEncipherment|wrapKey, unwrapKey| Sertifika oluşturma sırasında kullanım belirtimi olmadan varsayılan Key Vault | 
|Kabullenme|imzala, Doğrula| Yok |
|crlsign|imzala, Doğrula| Yok |

## <a name="certificate-issuer"></a>Sertifikayı veren

Key Vault bir sertifika nesnesi, x509 sertifikalarını sıralamak için seçilen bir sertifika veren sağlayıcısıyla iletişim kurmak için kullanılan bir yapılandırma barındırır.  

-   TLS/SSL sertifikaları için aşağıdaki sertifika veren sağlayıcılarıyla iş ortakları Key Vault

|**Sağlayıcı Adı**|**Konumlar**|
|----------|--------|
|DigiCert|Genel bulut ve Azure Kamu 'daki tüm anahtar kasası hizmeti konumlarında desteklenir|
|GlobalSign|Genel bulut ve Azure Kamu 'daki tüm anahtar kasası hizmeti konumlarında desteklenir|

Bir sertifika verenin bir Key Vault oluşturulabilmesi için, önkoşul 1 ve 2 ' nin aşağıdaki adımlardan başarıyla gerçekleştirilmesi gerekir.  

1. Sertifika yetkilisi (CA) sağlayıcılarına ekleme  

    -   Kuruluş yöneticisinin şirketlerinin şirket içinde olması gerekir (örn. Contoso) en az bir CA sağlayıcısıyla.  

2. Yönetici, TLS/SSL sertifikalarını kaydetmek (ve yenilemek) için Key Vault isteyenin kimlik bilgilerini oluşturur  

    -   Anahtar kasasında sağlayıcının veren nesnesini oluşturmak için kullanılacak yapılandırmayı sağlar  

Sertifika portalından veren nesneleri oluşturma hakkında daha fazla bilgi için [Key Vault sertifikaları bloguna](https://aka.ms/kvcertsblog) bakın  

Key Vault, farklı veren sağlayıcı yapılandırmasına sahip birden çok veren nesne oluşturulmasına izin verir. Bir veren nesnesi oluşturulduktan sonra, adının bir veya birden çok sertifika ilkesiyle başvurusu yapılabilir. Veren nesnesine başvurmak, sertifika oluşturma ve yenileme sırasında CA sağlayıcısından x509 sertifikası istenirken sertifikayı veren nesnesinde belirtilen yapılandırmayı kullanmak Key Vault söyler.  

Verenin nesneleri kasada oluşturulur ve yalnızca aynı kasadaki KV sertifikalarıyla birlikte kullanılabilir.  

## <a name="certificate-contacts"></a>Sertifika kişileri

Sertifika kişileri, sertifika ömrü olayları tarafından tetiklenen bildirimleri göndermek için iletişim bilgilerini içerir. Kişi bilgileri, anahtar kasasındaki tüm sertifikalar tarafından paylaşılır. Anahtar kasasındaki tüm sertifikalar için bir olay için belirtilen tüm kişilere bir bildirim gönderilir.  

Bir sertifikanın ilkesi otomatik olarak yenilemeye ayarlandıysa, aşağıdaki olaylara bir bildirim gönderilir.  

- Sertifika yenilemeden önce
- Sertifika yenileme sonrasında, sertifikanın başarıyla yenilendiğini veya bir hata olup olmadığını ve sertifikanın el ile yenilenmesini gerektirme.  

  El ile yenilenmek üzere ayarlanmış bir sertifika ilkesi (yalnızca e-posta) olduğunda, sertifikayı yenileme zamanı olduğunda bir bildirim gönderilir.  

## <a name="certificate-access-control"></a>Sertifika Access Control

 Sertifikalar için erişim denetimi Key Vault tarafından yönetilir ve bu sertifikaları içeren Key Vault tarafından sağlanır. Sertifikalar için erişim denetimi ilkesi, aynı Key Vault anahtarlar ve gizli diziler için erişim denetimi ilkelerinden farklıdır. Kullanıcılar, senaryoya uygun bir veya daha fazla kasa oluşturarak sertifikaların uygun segmentlerini ve yönetimini korumanıza olanak sağlayabilir.  

 Aşağıdaki izinler, bir anahtar kasasındaki gizli dizi erişim denetimi girişinde, her bir sorumlu temelinde kullanılabilir ve gizli bir nesne üzerinde izin verilen işlemleri yakından yansıtır:  

- Sertifika yönetimi işlemlerine yönelik izinler
  - *Get*: geçerli sertifika sürümünü veya bir sertifika sürümünü alın 
  - *liste*: geçerli sertifikaları veya bir sertifikanın sürümlerini listeleyin  
  - *güncelleştirme*: bir sertifikayı güncelleştirme
  - *oluşturma*: Key Vault sertifikası oluşturma
  - *içeri aktarma*: sertifika malzemesini bir Key Vault sertifikasına aktarma
  - *Sil*: bir sertifikayı, ilkesini ve tüm sürümlerini silme  
  - *kurtar*: silinen bir sertifikayı kurtar
  - *yedekleme*: bir sertifikayı anahtar kasasında yedekleme
  - *geri yükleme*: yedeklenen sertifikayı bir anahtar kasasına geri yükleme
  - *managecontacts*: Key Vault sertifikası kişilerini yönetme  
  - *manageverenler*: Key Vault sertifika yetkililerini/verenler yönetme
  - *getısers*: bir sertifikanın yetkililerini/verenler al
  - *listissuers*: bir sertifikanın yetkililerini/verenler listeleyin  
  - *setısers*: Key Vault sertifikanın yetkililerini/verenler oluşturma veya güncelleştirme  
  - *deleteverenler*: Key Vault sertifikasının yetkililerini/verenler silme  
 
- Ayrıcalıklı işlemler için izinler
  - *Temizle*: silinen bir sertifikayı temizle (kalıcı olarak sil)

Daha fazla bilgi için [Key Vault REST API başvurusu Içindeki sertifika işlemlerine](/rest/api/keyvault)bakın. İzinleri oluşturma hakkında bilgi için bkz. [kasa-oluşturma veya güncelleştirme](/rest/api/keyvault/vaults/createorupdate) ve [kasa-güncelleştirme erişim ilkesi](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="next-steps"></a>Sonraki adımlar

- [Anahtar Kasası Hakkında](../general/overview.md)
- [Anahtarlar, gizli diziler ve sertifikalar hakkında](../general/about-keys-secrets-certificates.md)
- [Anahtarlar hakkında](../keys/about-keys.md)
- [Gizli diziler hakkında](../secrets/about-secrets.md)
- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Geliştirici Kılavuzu Key Vault](../general/developers-guide.md)