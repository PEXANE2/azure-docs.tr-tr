---
title: Öğretici-Azure ön kapısı için özel bir etki alanında HTTPS 'yi yapılandırma | Microsoft Docs
description: Bu öğreticide, özel bir etki alanı için Azure ön kapısı yapılandırmanızda HTTPS 'yi etkinleştirmeyi ve devre dışı bırakmayı öğreneceksiniz.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/09/2020
ms.author: duau
ms.openlocfilehash: bac1d1e41cab4aa3be10fb226df57277db20c78e
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90030285"
---
# <a name="tutorial-configure-https-on-a-front-door-custom-domain"></a>Öğretici: Front Door özel etki alanı üzerinde HTTPS'yi yapılandırma

Bu öğreticide ön uç konakları bölümü altında Front Door’unuzla ilişkili özel bir etki alanı için HTTPS protokolünün nasıl etkinleştirileceği gösterilir. Özel etki alanınızda HTTPS protokolünü kullanarak (örneğin, https:\//www.contoso.com), hassas veriler internet üzerinden gönderildiğinde bunların TLS/SSL şifrelemesi ile güvenli bir şekilde teslim edilmesini sağlarsınız. Web tarayıcınız HTTPS üzerinden bir web sitesine bağlanırken, web sitesinin güvenlik sertifikasını onaylar ve bu sertifikanın yasal bir sertifika yetkilisi tarafından verildiğini doğrular. Bu işlem güvenlik sağlar ve web uygulamalarınızı saldırılara karşı korur.

Azure ön kapısı, varsayılan olarak ön kapı varsayılan ana bilgisayar adı üzerinde HTTPS 'yi destekler. Örneğin, bir ön kapı oluşturursanız (gibi `https://contoso.azurefd.net` ), https istekleri için otomatik olarak etkinleştirilir `https://contoso.azurefd.net` . Öte yandan, 'www.contoso.com' özel etki alanını ekledikten sonra bu ön uç konağı için HTTPS'yi etkinleştirmeniz gerekecektir.   

Özel HTTPS özelliğinin en önemli niteliklerinden bazıları şunlardır:

- Ek ücret yoktur: Sertifika edinme veya yenileme işlemleri için herhangi bir maliyet söz konusu değildir ve HTTPS trafiği için ek ücret alınmaz. 

- Basit etkinleştirme: Tek tıklamayla sağlama özelliği [Azure portalından](https://portal.azure.com) kullanılabilir. Özelliği etkinleştirmek için REST API’yi veya diğer geliştirici araçlarını kullanabilirsiniz.

- Eksiksiz sertifika yönetimi kullanılabilir: Sizin için tüm sertifika tedariki ve yönetimi gerçekleştirilir. Sertifikalar sona ermeden önce otomatik olarak sağlanır ve yenilenir. Bu da sertifika süre sonu nedeniyle hizmette yaşanabilecek kesinti risklerini ortadan kaldırır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> - Özel etki alanınızda HTTPS protokolünü etkinleştirme.
> - AFD tarafından yönetilen sertifikayı kullanma 
> - Özel bir TLS/SSL sertifikası olan kendi sertifikanızı kullanın
> - Etki alanını doğrulama
> - Özel etki alanınızda HTTPS protokolünü devre dışı bırakma


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki adımları tamamlayabilmeniz için öncelikle bir Front Door oluşturmalı ve en az bir özel etki alanı eklemelisiniz. Daha fazla bilgi için bkz. [Öğretici: Front Door’unuza özel etki alanı ekleme](front-door-custom-domain.md).

## <a name="tlsssl-certificates"></a>TLS/SSL sertifikaları

HTTPS protokolünü bir ön kapı özel etki alanında güvenli bir şekilde teslim etmek üzere etkinleştirmek için bir TLS/SSL sertifikası kullanmanız gerekir. Azure ön kapısı tarafından yönetilen bir sertifika kullanmayı veya kendi sertifikanızı kullanmayı seçebilirsiniz.


### <a name="option-1-default-use-a-certificate-managed-by-front-door"></a>Seçenek 1 (varsayılan): Front Door tarafından yönetilen bir sertifika kullanın

Azure ön kapısının yönettiği bir sertifika kullandığınızda, HTTPS özelliği yalnızca birkaç tıklamayla açılabilir. Azure ön kapısı, tedarik ve yenileme gibi sertifika yönetimi görevlerini tamamen işler. Özelliği etkinleştirmenizin ardından işlem hemen başlar. Özel etki alanı önceden Front Door’un varsayılan ön uç konağına (`{hostname}.azurefd.net`) eşlendiyse başka bir eylem gerekmez. Front Door, adımları işler ve isteğinizi otomatik olarak tamamlar. Ancak özel etki alanınız başka bir yerde eşlendiyse, etki alanı sahipliğinizi doğrulamak için e-posta kullanmanız gerekir.

Özel bir etki alanı üzerinde HTTPS'yi etkinleştirmek için aşağıdaki adımları uygulayın:

1. [Azure portalda](https://portal.azure.com)**Front Door** profilinize göz atın.

2. Ön uç konakları listesinde, özel etki alanınızın eklenmesi için HTTPS’yi etkinleştirmek istediğiniz özel etki alanını seçin.

3. **Özel etki alanı HTTPS’si** bölümünde **Etkin**’e tıklayın ve sertifika kaynağı olarak **Front Door ile yönetilen**’i seçin.

4. Kaydet’e tıklayın.

5. [Etki alanını doğrulama](#validate-the-domain) adımına ilerleyin.

> [!NOTE]
> AFD tarafından yönetilen sertifikalar için DigiCert 64 karakter sınırı zorlanır. Bu sınır aşılırsa doğrulama başarısız olur.


### <a name="option-2-use-your-own-certificate"></a>Seçenek 2: Kendi sertifikanızı kullanın

HTTPS özelliğini etkinleştirmek için kendi sertifikanızı kullanabilirsiniz. Bu işlem, sertifikalarınızı güvenli bir şekilde depolamanıza olanak tanıyan Azure Key Vault ile tümleştirme yoluyla gerçekleştirilir. Azure ön kapısı, sertifikanızı almak için bu güvenli mekanizmayı kullanır ve birkaç ek adım gerektirir. TLS/SSL sertifikanızı oluştururken, bunu izin verilen bir sertifika yetkilisi (CA) ile oluşturmanız gerekir. Buna karşılık, izin verilmeyen bir CA kullanırsanız isteğiniz reddedilir. İzin verilen CA 'Ların bir listesi için bkz. [Azure ön kapıda özel https 'yi etkinleştirmek Için Izin verilen sertifika yetkilileri](front-door-troubleshoot-allowed-ca.md).

#### <a name="prepare-your-azure-key-vault-account-and-certificate"></a>Azure Key Vault hesabınızı ve sertifikanızı hazırlama
 
1. Azure Key Vault: Özel HTTPS’yi etkinleştirmek istediğiniz Front Door’unuzla aynı abonelik altında çalışan bir Azure Key Vault hesabınız olması gerekir. Azure Key Vault hesabınız yoksa oluşturun.

> [!WARNING]
> Azure ön kapısı şu anda yalnızca ön kapı yapılandırmasıyla aynı abonelikteki Key Vault hesaplarını desteklemektedir. Front Door’dan farklı bir abonelik altındaki Key Vault’un seçilmesi hatayla sonuçlanır.

2. Azure Key Vault sertifikaları: Zaten bir sertifikanız varsa, bu sertifikayı doğrudan Azure Key Vault hesabınıza yükleyebilir veya doğrudan Azure Key Vault’un tümleştirildiği iş ortağı CA'lardan birinin Azure Key Vault’u üzerinden yeni bir sertifika oluşturabilirsiniz. Sertifikanızı **gizli**değil bir **sertifika** nesnesi olarak karşıya yükleyin.

> [!NOTE]
> Kendi TLS/SSL sertifikanız için, ön kapı EC şifreleme algoritmalarına sahip sertifikaları desteklemez.

#### <a name="register-azure-front-door"></a>Azure ön kapısını Kaydet

Azure ön kapısının hizmet sorumlusunu PowerShell aracılığıyla Azure Active Directory bir uygulama olarak kaydedin.

> [!NOTE]
> Bu eylem, genel yönetici izinleri gerektirir ve her kiracı için yalnızca **bir kez** gerçekleştirilmesi gerekir.

1. Gerekirse yerel makinenizdeki PowerShell’de [Azure PowerShell](/powershell/azure/install-az-ps)’i yükleyin.

2. PowerShell’de aşağıdaki komutu çalıştırın:

     `New-AzADServicePrincipal -ApplicationId "ad0e1c7e-6d38-4ba4-9efd-0bc77ba9f037"`              

#### <a name="grant-azure-front-door-access-to-your-key-vault"></a>Anahtar kasanıza Azure ön kapısına erişim izni verin
 
Azure Key Vault hesabınızdaki sertifikalara erişmek için Azure ön kapısına izin verin.

1. Anahtar kasası hesabınızda AYARLAR bölümünden **Erişim ilkeleri**’ni ve sonra **Yeni ekle**’yi seçip yeni bir ilke oluşturun.

2. **Sorumlu seç** bölümünde **ad0e1c7e-6d38-4ba4-9efd-0bc77ba9f037**'yi arayın ve **Microsoft.Azure.Frontdoor**’u seçin. **Seç**’e tıklayın.

3. **Gizli izinler**' de, sertifikayı almak Için ön kapıya izin vermek için **Al** ' ı seçin.

4. Sertifika **izinleri**' nde, sertifikayı almak Için ön kapıya izin vermek için **Al** ' ı seçin.

5. **Tamam**’ı seçin. 

    Azure ön kapısının artık bu Key Vault ve bu Key Vault depolanan sertifikalara erişimi olabilir.
 
#### <a name="select-the-certificate-for-azure-front-door-to-deploy"></a>Dağıtılacak Azure ön kapısının sertifikasını seçin
 
1. Portalda Front Door’unuza dönün. 

2. Özel etki alanları listesinde, HTTPS'yi etkinleştirmek istediğiniz özel etki alanını seçin.

    **Özel etki alanı** sayfası görünür.

3. Sertifika yönetimi türü bölümünde **Kendi sertifikamı kullan**’ı seçin. 

4. Azure ön kapısı, Key Vault hesabının aboneliğinin ön kapasitenizin ile aynı olmasını gerektirir. Bir anahtar kasası, sertifika (gizli gizi) ve sertifika sürümü seçin.

    Azure ön kapısı aşağıdaki bilgileri listeler: 
    - Abonelik kimliğiniz için anahtar kasası hesapları. 
    - Seçilen anahtar kasası altındaki sertifikalar (gizli diziler). 
    - Kullanılabilir sertifika sürümleri. 
 
5. Kendi sertifikanızı kullanıyorsanız etki alanı doğrulaması gerekmez. [Yayılma için bekleme](#wait-for-propagation) adımına geçin.

## <a name="validate-the-domain"></a>Etki alanını doğrulama

CNAME kaydı kullanılarak özel uç noktanızla eşlenen ve kullanılmakta olan bir özel etki alanınız zaten varsa ya da kendi sertifikanızı kullanıyorsanız şu adıma geçin:  
[Özel etki alanı, Front Door’unuzla eşlendi](#custom-domain-is-mapped-to-your-front-door-by-a-cname-record). Böyle bir etki alanınız yoksa, etki alanınıza yönelik CNAME kaydı girişinin artık mevcut olmaması veya afdverify alt etki alanını içeriyor olması durumunda [Özel etki alanı Front Door’unuzla eşlenmedi](#custom-domain-is-not-mapped-to-your-front-door) adımına geçin.

### <a name="custom-domain-is-mapped-to-your-front-door-by-a-cname-record"></a>Özel etki alanı bir CNAME kaydı kullanılarak Front Door’unuzla eşlendi

Front Door’unuzun ön uç konaklarına özel etki alanı eklediğinizde etki alanı kayıt yetkilinizin DNS tablosunda, Front Door’unuzun varsayılan .azurefd.net konak adıyla eşlenecek bir CNAME kaydı oluşturmuş oldunuz. Bu CNAME kaydı hala mevcutsa ve afdverify alt etki alanını içermiyorsa DigiCert Sertifika Yetkilisi (CA), özel etki alanınızın sahipliğini otomatik olarak doğrulamak için bunu kullanır. 

Kendi sertifikanızı kullanıyorsanız etki alanı doğrulaması gerekmez.

CNAME kaydınız, *Ad*’ın özel etki alanınız, *Değer*’in ise Front Door’unuzun varsayılan .azurefd.net konak adı olduğu aşağıdaki biçimde olmalıdır:

| Ad            | Tür  | Değer                 |
|-----------------|-------|-----------------------|
| <www.contoso.com> | CNAME | contoso.azurefd.net |

CNAME kayıtları hakkında daha fazla bilgi için bkz. [CNAME DNS kaydı oluşturma](https://docs.microsoft.com/azure/cdn/cdn-map-content-to-custom-domain).

CNAME kaydınız doğru biçimdeyse DigiCert, özel etki alanı adınızı otomatik olarak doğrular ve etki alanı adınız için ayrılmış bir sertifika oluşturur. DigitCert size doğrulama e-postası göndermez ve isteğinizi onaylamanız gerekmez. Sertifika bir yıl boyunca geçerlidir ve süresi dolmadan önce autorenewed olacaktır. [Yayılma için bekleme](#wait-for-propagation) adımına geçin. 

Otomatik doğrulama genellikle birkaç dakika sürer. Bir saat içinde etki alanınızı doğrulanmış olarak görmüyorsanız destek bileti açın.

>[!NOTE]
>DNS sağlayıcınız ile bir Sertifika Yetkilisi Yetkilendirme (CAA) kaydınız varsa bunun geçerli CA olarak DigiCert‘i içermesi gerekir. CAA kaydı; etki alanı sahiplerinin DNS sağlayıcıları ile hangi CA'ların, etki alanları için sertifika vermeye yetkili olduğunu belirtmesini sağlar. Bir CA, CAA kaydına sahip bir etki alanı için sertifika siparişi alırsa ve bu CA, sertifika vermeye yetkili olarak listelenmemişse bu CA’nın, söz konusu etki alanına veya alt etki alanına sertifika vermesi yasaklanır. CAA kayıtlarını yönetme ile ilgili bilgi için bkz. [CAA kayıtlarını yönetme](https://support.dnsimple.com/articles/manage-caa-record/). CAA kayıt aracı için bkz. [CAA Kayıt Yardımcısı](https://sslmate.com/caa/).

### <a name="custom-domain-is-not-mapped-to-your-front-door"></a>Özel etki alanı Front Door’unuzla eşlenmedi

Uç noktanız için CNAME kaydı girişi artık mevcut değilse veya afdverify alt etki alanını içeriyorsa bu adımın diğer yönergelerini uygulayın.

Özel etki alanınızda HTTPS'yi etkinleştirdikten sonra, DigiCert CA etki alanının [WHOIS](http://whois.domaintools.com/) kayıt yetkilisi bilgisine göre kayıt yetkilisiyle iletişim kurarak etki alanınızın sahipliğini doğrular. İletişim, WHOIS kaydında belirtilen e-posta adresi (varsayılan) veya telefon numarası aracılığıyla kurulur. HTTPS, özel etki alanınızda etkin hale gelmeden önce etki alanı doğrulamasını tamamlamanız gerekir. Etki alanını onaylamak için altı iş gününüz vardır. Altı iş günü içinde onaylanmamış istekler otomatik olarak iptal edilir. 

![WHOIS kaydı](./media/front-door-custom-domain-https/whois-record.png)

DigiCert ayrıca ek e-posta adreslerine de bir doğrulama e-postası gönderir. WHOIS kayıt yetkilisi bilgileri özelse doğrudan şu adreslerden birini kullanarak onaylayabildiğinizi doğrulayın:

admin@&lt;etki-alanı-adınız.com&gt;  
administrator@&lt;etki-alanı-adınız.com&gt;  
webmaster@&lt;etki-alanı-adınız.com&gt;  
hostmaster@&lt;etki-alanı-adınız.com&gt;  
postmaster@&lt;.com&gt;  

Birkaç dakika içinde sizden isteği onaylamanızı isteyen, aşağıdaki örneğe benzer bir e-posta alırsınız. Bir istenmeyen posta filtresi kullanıyorsanız, admin@digicert.com izin verilenler listesine ekleyin. E-postayı 24 saat içinde almazsanız Microsoft destek ekibine başvurun.

Onay bağlantısına tıkladığınızda, çevrimiçi bir onay formuna yönlendirilirsiniz. Formdaki yönergeleri uygulayın. İki doğrulama seçeneğiniz vardır:

- contoso.com gibi aynı kök etki alanı için aynı hesap üzerinden verilen gelecekteki tüm siparişleri onaylayabilirsiniz. Aynı kök etki alanı için ek özel etki alanları eklemeyi planlıyorsanız bu yaklaşım önerilir.

- Yalnızca bu istekte kullanılan söz konusu ana bilgisayar adını onaylayabilirsiniz. Sonraki istekler için ek onay gereklidir.

DigiCert onaydan sonra özel etki alanı adınız için sertifika oluşturma işlemlerini tamamlar. Sertifika bir yıl boyunca geçerlidir ve bu süre dolmadan önce autorenewed olacaktır.

## <a name="wait-for-propagation"></a>Yayılma için bekleme

Etki alanı doğrulandıktan sonra özel etki alanı HTTPS özelliğinin etkinleştirilmesi 6-8 saate kadar sürebilir. İşlem tamamlandığında, Azure portalındaki özel HTTPS durumu **Etkin** olarak ayarlanır ve özel etki alanı iletişim kutusundaki dört işlem adımı tamamlandı olarak işaretlenir. Özel etki alanınız artık HTTPS’yi kullanmak için hazırdır.

### <a name="operation-progress"></a>İşlem ilerleme durumu

Aşağıdaki tabloda, HTTPS’yi etkinleştirdiğinizde oluşan işlem ilerleme durumunu gösterir. HTTPS’yi etkinleştirmenizin ardından özel etki alanı iletişim kutusunda dört işlem adımı görünür. Her adım etkin hale gelir ve ilerledikçe adımın altında ek alt adım ayrıntıları görüntülenir. Bu alt adımların hiçbiri gerçekleşmez. Bir adım başarıyla tamamlandıktan sonra adımın yanında yeşil bir onay işareti görünür. 

| İşlem adımı | İşlem alt adımı ayrıntıları | 
| --- | --- |
| 1 İstek gönderiliyor | İstek gönderiliyor |
| | HTTPS isteğiniz gönderiliyor. |
| | HTTPS isteğiniz başarıyla gönderildi. |
| 2 Etki alanı doğrulaması | Front Door’unuzun varsayılan .azurefd.net ön uç konağıyla eşlenen CNAME ise etki alanı otomatik olarak doğrulanır. Eşleme yapılmadıysa etki alanınızın kayıt kaydında listelenen e-posta adresine (WHOIS kayıt şirketi) bir doğrulama isteği gönderilir. Etki alanını mümkün olan en kısa süre içinde doğrulayın. |
| | Etki alanı sahipliğiniz başarıyla doğrulandı. |
| | Etki alanı doğrulama isteğinin süresi doldu. (Müşteri büyük olasılıkla 6 gün içinde yanıt vermedi.) HTTPS, etki alanınızda etkinleştirilmeyecek. * |
| | Etki alanı sahipliğini doğrulama isteği, müşteri tarafından reddedildi. HTTPS, etki alanınızda etkinleştirilmeyecek. * |
| 3 Sertifika sağlanıyor | Sertifika yetkilisi şu anda etki alanınızdaki HTTPS'nin etkinleştirilmesi için gereken sertifikayı veriyor. |
| | Sertifika verildi ve şu anda Front Door’unuz için dağıtılıyor. Bu işlem 1 saat kadar sürebilir. |
| | Sertifika, Front Door’unuz için başarıyla dağıtıldı. |
| 4 Tamamlandı | HTTPS, etki alanınızda başarıyla etkinleştirildi. |

\* Hata oluşmadığı sürece bu ileti görüntülenmez. 

İstek gönderilmeden önce hata oluşursa, aşağıdaki hata iletisi görüntülenir:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

1. *Sertifika sağlayıcısı kimdir ve ne tür bir sertifika kullanılır?*

    Özel etki alanınız için Digicert tarafından sağlanan ayrılmış/tek bir sertifika kullanılır. 

2. *IP tabanlı veya SNI TLS/SSL kullanıyor musunuz?*

    Azure ön kapısı SNı TLS/SSL kullanır.

3. *DigiCert’ten etki alanı doğrulama e-postası almazsam ne olur?*

    Özel etki alanınız için doğrudan uç nokta ana bilgisayar adına işaret eden bir CNAME girişiniz varsa (ve afdverify alt etki alanı adını kullanmıyorsanız) etki alanı doğrulama e-postası almazsınız. Doğrulama otomatik olarak gerçekleşir. Ancak CNAME girişiniz yoksa ve e-postayı 24 saat içinde almazsanız Microsoft destek ekibine başvurun.

4. *Ayrılmış sertifika kullanmak, SAN sertifikasından daha mı güvenlidir?*
    
    SAN sertifikası, ayrılmış sertifika ile aynı şifreleme ve güvenlik standartlarını uygular. Tüm verilen TLS/SSL sertifikaları, gelişmiş sunucu güvenliği için SHA-256 kullanır.

5. *DNS sağlayıcım ile Sertifika Yetkilisi Yetkilendirme kaydı kullanmam gerekir mi?*

    Hayır, Sertifika Yetkilisi Yetkilendirme kaydı şu anda gerekli değildir. Ancak, varsa, geçerli CA olarak DigiCert’i içermelidir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda özel etki alanınızda HTTPS protokolünü etkinleştirdiniz. Artık HTTPS ile özel etki alanınızı kullanmak istiyorsanız, aşağıdaki adımları uygulayarak HTTPS’yi devre dışı bırakabilirsiniz:

### <a name="disable-the-https-feature"></a>HTTPS özelliğini devre dışı bırakma 

1. [Azure Portal](https://portal.azure.com) **Azure ön kapı** yapılandırmanıza gidin.

2. Ön uç konakları listesinde HTTPS’yi devre dışı bırakmak istediğiniz özel etki alanına tıklayın.

3. HTTPS’yi devre dışı bırakmak için **Devre Dışı**’na ve sonra da **Kaydet**’e tıklayın.

### <a name="wait-for-propagation"></a>Yayılma için bekleme

Özel etki alanı HTTPS özelliği devre dışı bırakıldıktan sonra bu işlemin geçerli olması 6-8 saate kadar sürebilir. İşlem tamamlandığında, Azure portalındaki özel HTTPS durumu **Devre Dışı** olarak ayarlanır ve özel etki alanı iletişim kutusundaki üç işlem adımı tamamlandı olarak işaretlenir. Özel etki alanınız artık HTTPS’yi kullanamaz.

#### <a name="operation-progress"></a>İşlem ilerleme durumu

Aşağıdaki tabloda, HTTPS’yi devre dışı bıraktığınızda oluşan işlem ilerleme durumunu gösterir. HTTPS’yi devre dışı bırakmanızın ardından özel etki alanı iletişim kutusunda üç işlem adımı görünür. Her adım etkin hale gelir ve adımın altında ek ayrıntılar görüntülenir. Bir adım başarıyla tamamlandıktan sonra adımın yanında yeşil bir onay işareti görünür. 

| İşlem ilerleme durumu | İşlem ayrıntıları | 
| --- | --- |
| 1 İstek gönderiliyor | İsteğiniz gönderiliyor |
| 2 Sertifika sağlaması kaldırılıyor | Sertifika siliniyor |
| 3 Tamamlandı | Sertifika silindi |

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Key Vault bir sertifikayı karşıya yükleyin.
* Bir etki alanını doğrulayın.
* Özel etki alanınız için HTTPS 'yi etkinleştirin.

Ön kapıya yönelik coğrafi filtreleme ilkesi ayarlamayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Coğrafi filtreleme ilkesi ayarlama](front-door-geo-filtering.md)
