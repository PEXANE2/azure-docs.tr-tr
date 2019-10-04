---
title: Öğretici-Azure CDN özel bir etki alanında HTTPS 'yi yapılandırma | Microsoft Docs
description: Bu öğreticide, Azure CDN uç nokta özel etki alanında HTTPS 'yi etkinleştirmeyi ve devre dışı bırakmayı öğreneceksiniz.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/1/2019
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 5506e4e4bb41725fd8791d3c6e47bb0b94584923
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71959431"
---
# <a name="tutorial-configure-https-on-an-azure-cdn-custom-domain"></a>Öğretici: Azure CDN özel bir etki alanında HTTPS 'yi yapılandırma

Bu öğreticide, Azure CDN uç noktasıyla ilişkili özel bir etki alanı için HTTPS protokolünün nasıl etkinleştirileceği gösterilmektedir. Özel etki alanında HTTPS protokolünü kullanarak (örneğin, https: \//www. contoso. com), hassas verilerinizin internet üzerinden gönderildiğinde TLS/SSL şifrelemesi aracılığıyla güvenli bir şekilde teslim edilmesini sağlayabilirsiniz. Web tarayıcınız HTTPS üzerinden bir Web sitesine bağlandığında, Web sitesinin güvenlik sertifikasını doğrular ve yasal bir sertifika yetkilisi tarafından verildiğini doğrular. Bu süreç güvenlik sağlar ve Web uygulamalarınızı saldırılara karşı korur.

Azure CDN, varsayılan olarak bir CDN uç noktası ana bilgisayar adı üzerinde HTTPS 'yi destekler. Örneğin, bir CDN uç noktası (örneğin, https: \//contoso. azureedge. net) oluşturursanız, HTTPS otomatik olarak etkinleştirilir.  

Özel HTTPS özelliğinin bazı anahtar öznitelikleri şunlardır:

- Ek maliyet yok: sertifika alma veya yenileme maliyetleri yoktur ve HTTPS trafiği için ek ücret alınmaz. Yalnızca CDN 'den GB çıkış için ödeme yaparsınız.

- Basit etkinleştirme: tek tıklamayla sağlama [Azure Portal](https://portal.azure.com)kullanılabilir. Özelliği etkinleştirmek için REST API veya diğer geliştirici araçlarını da kullanabilirsiniz.

- Sertifika yönetiminin tamamı kullanılabilir: tüm sertifika tedarik ve yönetimi sizin için gerçekleştirilir. Sertifikalar, süresi dolmadan önce otomatik olarak sağlanır ve yenilenir, bu da bir sertifikanın süresi dolduğunda hizmet kesintisi riskini ortadan kaldırır.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:
> [!div class="checklist"]
> - Özel etki alanında HTTPS protokolünü etkinleştirin.
> - CDN ile yönetilen bir sertifika kullanma 
> - Kendi sertifikanızı kullanın
> - Etki alanını doğrulama
> - Özel etki alanında HTTPS protokolünü devre dışı bırakın.

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)] 

Bu öğreticideki adımları tamamlayabilmeniz için önce bir CDN profili ve en az bir CDN uç noktası oluşturmanız gerekir. Daha fazla bilgi için bkz. [hızlı başlangıç: Azure CDN profili ve uç noktası oluşturma](cdn-create-new-endpoint.md).

Ayrıca, CDN uç noktanıza Azure CDN özel bir etki alanını ilişkilendirmeniz gerekir. Daha fazla bilgi için bkz [. Öğretici: Azure CDN uç noktanıza özel etki alanı ekleme](cdn-map-content-to-custom-domain.md) 

> [!IMPORTANT]
> CDN ile yönetilen sertifikalar, kök veya tepesinde etki alanları için kullanılamaz. Azure CDN özel etki alanınız bir kök veya tepesinde etki alanı ise kendi sertifikanızı getir özelliğini kullanmanız gerekir. 
>

---

## <a name="ssl-certificates"></a>SSL sertifikaları
HTTPS protokolünü, Azure CDN özel bir etki alanında güvenli bir şekilde içerik teslim etmek üzere etkinleştirmek için bir SSL sertifikası kullanmanız gerekir. Azure CDN tarafından yönetilen bir sertifika kullanmayı veya kendi sertifikanızı kullanmayı seçebilirsiniz.


# <a name="option-1-default-enable-https-with-a-cdn-managed-certificatetaboption-1-default-enable-https-with-a-cdn-managed-certificate"></a>[Seçenek 1 (varsayılan): CDN ile yönetilen bir sertifikayla HTTPS 'yi etkinleştirin](#tab/option-1-default-enable-https-with-a-cdn-managed-certificate)

CDN ile yönetilen bir sertifika kullandığınızda, HTTPS özelliği yalnızca birkaç tıklamayla etkinleştirilebilir. Azure CDN, tedarik ve yenileme gibi sertifika yönetimi görevlerini tamamen işler. Özelliği etkinleştirdikten sonra işlem hemen başlatılır. Özel etki alanı zaten CDN uç noktasıyla eşlenmişse, başka bir eylem gerekmez. Azure CDN adımları işleyecek ve isteğinizi otomatik olarak tamamlayacaktır. Ancak, özel etki alanınız başka bir yerde eşlenmişse, etki alanı sahipliğini doğrulamak için e-posta kullanmanız gerekir.

Özel bir etki alanında HTTPS 'yi etkinleştirmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com), **Microsoft 'un Azure CDN Standart** **Azure CDN**, Verizon profilinden Verizon veya **Azure CDN Premium** 'dan standart olan **Azure CDN** standart bir şekilde inceleyin.

2. CDN uç noktaları listesinde, özel etki alanınızı içeren uç noktayı seçin.

    ![Uç nokta listesi](./media/cdn-custom-ssl/cdn-select-custom-domain-endpoint.png)

    **Uç nokta** sayfası görüntülenir.

3. Özel etki alanları listesinde, HTTPS 'yi etkinleştirmek istediğiniz özel etki alanını seçin.

    ![Özel etki alanları listesi](./media/cdn-custom-ssl/cdn-custom-domain.png)

    **Özel etki alanı** sayfası görüntülenir.

4. Sertifika yönetimi türü altında **CDN yönetilen**öğesini seçin.

5. HTTPS 'yi etkinleştirmek için **Açık '** ı seçin.

    ![Özel etki alanı HTTPS durumu](./media/cdn-custom-ssl/cdn-select-cdn-managed-certificate.png)

6. [Etki alanını doğrulamaya](#validate-the-domain)devam edin.


# <a name="option-2-enable-https-with-your-own-certificatetaboption-2-enable-https-with-your-own-certificate"></a>[Seçenek 2: HTTPS 'yi kendi sertifikanız ile etkinleştirin](#tab/option-2-enable-https-with-your-own-certificate)

> [!IMPORTANT]
> Bu seçenek yalnızca **Microsoft 'tan Azure CDN** ve **Verizon profillerden Azure CDN** ile kullanılabilir. 
>
 
HTTPS özelliğini etkinleştirmek için kendi sertifikanızı kullanabilirsiniz. Bu işlem, Azure Key Vault ile bir tümleştirme aracılığıyla yapılır, bu da sertifikalarınızı güvenli bir şekilde depolamanıza olanak tanır. Azure CDN, sertifikanızı almak için bu güvenli mekanizmayı kullanır ve birkaç ek adım gerektirir. SSL sertifikanızı oluştururken, bunu izin verilen bir sertifika yetkilisi (CA) ile oluşturmanız gerekir. Aksi takdirde, izin verilmeyen bir CA kullanıyorsanız isteğiniz reddedilir. İzin verilen CA 'Ların bir listesi için bkz. [Azure CDN için özel https 'yi etkinleştirmek Için Izin verilen sertifika yetkilileri](cdn-troubleshoot-allowed-ca.md). **Verizon 'den Azure CDN**için geçerli CA 'lar kabul edilecektir. 

### <a name="prepare-your-azure-key-vault-account-and-certificate"></a>Azure Anahtar Kasası hesabınızı ve sertifikanızı hazırlayın
 
1. Azure Key Vault: özel HTTPS 'yi etkinleştirmek istediğiniz Azure CDN profili ve CDN uç noktalarıyla aynı abonelikte çalışan bir Azure Key Vault hesabınız olmalıdır. Bir Azure Key Vault hesabı oluşturun.
 
2. Azure Key Vault sertifikaları: zaten bir sertifikanız varsa, bunu doğrudan Azure Key Vault hesabınıza yükleyebilir veya Azure Key Vault tümleştirildiği iş ortağı CA 'lardan birinden doğrudan Azure Key Vault aracılığıyla yeni bir sertifika oluşturabilirsiniz. 

### <a name="register-azure-cdn"></a>Kayıt Azure CDN

PowerShell aracılığıyla Azure Active Directory Azure CDN bir uygulama olarak kaydedin.

1. Gerekirse, yerel makinenize [Azure PowerShell](/powershell/azure/install-az-ps) ' yi yüklemelisiniz.

2. PowerShell 'de şu komutu çalıştırın:

     `New-AzADServicePrincipal -ApplicationId "205478c0-bd83-4e1b-a9d6-db63a3e1e1c8"`

    ![PowerShell 'de Azure CDN kaydetme](./media/cdn-custom-ssl/cdn-register-powershell.png)
              

### <a name="grant-azure-cdn-access-to-your-key-vault"></a>Anahtar kasanıza Azure CDN erişim izni verin
 
Azure Key Vault hesabınızdaki sertifikalara (gizli dizi) erişmek için Azure CDN izin verin.

1. Anahtar Kasası hesabınızda, ayarlar altında, **erişim ilkeleri**' ni seçin, sonra yeni ilke oluşturmak Için **Yeni Ekle** ' yi seçin.

    ![Yeni erişim ilkesi oluştur](./media/cdn-custom-ssl/cdn-new-access-policy.png)

2. **Asıl seçin**bölümünde, **205478c0-bd83-4e1b-a9d6-db63a3e1e1c8**araması yapın ve **Microsoft. Azure. CDN**' yi seçin. **Seç**' e tıklayın.

    ![Erişim ilkesi ayarları](./media/cdn-custom-ssl/cdn-access-policy-settings.png)

3. **Sertifika izinleri**' ni seçin ve ardından, CDN 'nin sertifikaları almak ve listelemek üzere bu izinleri gerçekleştirmesini sağlamak için **Al** ve **Listele** onay kutularını seçin.

4. **Tamam ' ı**seçin. 

    Azure CDN artık bu anahtar kasasına ve bu Anahtar Kasası 'nda depolanan sertifikalara (gizli dizi) erişebilir.
 
### <a name="select-the-certificate-for-azure-cdn-to-deploy"></a>Dağıtılacak Azure CDN sertifikayı seçin
 
1. Azure CDN portalına dönün ve özel HTTPS 'yi etkinleştirmek istediğiniz profili ve CDN uç noktasını seçin. 

2. Özel etki alanları listesinde, HTTPS 'yi etkinleştirmek istediğiniz özel etki alanını seçin.

    **Özel etki alanı** sayfası görüntülenir.

3. Sertifika yönetimi türü altında **kendi sertifikamı kullan**' ı seçin. 

    ![Sertifikanızı yapılandırma](./media/cdn-custom-ssl/cdn-configure-your-certificate.png)

4. Bir Anahtar Kasası, sertifika (gizli) ve sertifika sürümü seçin.

    Azure CDN aşağıdaki bilgileri listeler: 
    - Abonelik KIMLIĞINIZ için Anahtar Kasası hesapları. 
    - Seçili anahtar kasasının altındaki Sertifikalar (gizlilikler). 
    - Kullanılabilir sertifika sürümleri. 
 
5. HTTPS 'yi etkinleştirmek için **Açık '** ı seçin.
  
6. Kendi sertifikanızı kullandığınızda etki alanı doğrulama gerekli değildir. [Yayılmayı beklemek](#wait-for-propagation)için devam edin.

---

## <a name="validate-the-domain"></a>Etki alanını doğrulama

Zaten bir CNAME kaydıyla özel uç noktanıza eşlenmiş bir özel etki alanınız varsa veya kendi sertifikanızı kullanıyorsanız, şu noktaya geçin  
[Özel etki alanı, CDN uç](#custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record)noktanızla eşleştirilir. Aksi takdirde, uç noktanız için CNAME kayıt girişi artık mevcut değilse veya cdnverify alt etki alanını içeriyorsa, [özel etki alanına devam etme, CDN uç](#custom-domain-is-not-mapped-to-your-cdn-endpoint)noktanızla eşlenmedi.

### <a name="custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record"></a>Özel etki alanı, CDN uç noktanıza bir CNAME kaydıyla eşlendi

Uç noktanıza özel bir etki alanı eklediğinizde, etki alanı Kaydedicinizin DNS tablosunda, CDN uç noktası ana bilgisayar adı ile eşlemek için bir CNAME kaydı oluşturdunuz. Bu CNAME kaydı hala varsa ve cdnverify alt etki alanını içermiyorsa, DigiCert CA bunu özel etki alanınızın sahipliğini otomatik olarak doğrulamak için kullanır. 

Kendi sertifikanızı kullanıyorsanız, etki alanı doğrulama gerekli değildir.

CNAME kaydınız aşağıdaki biçimde olmalıdır; burada *ad* özel etki alanı adınız ve *değer* CDN uç nokta ana bilgisayar adıdır:

| Name            | Tür  | Değer                 |
|-----------------|-------|-----------------------|
| < www. contoso. com > | CNAME | contoso.azureedge.net |

CNAME kayıtları hakkında daha fazla bilgi için bkz. [CNAME DNS kaydı oluşturma](https://docs.microsoft.com/azure/cdn/cdn-map-content-to-custom-domain).

CNAME kaydınız doğru biçimindeyse, DigiCert otomatik olarak özel etki alanı adınızı doğrular ve etki alanı adınız için adanmış bir sertifika oluşturur. DigitCert size bir doğrulama e-postası göndermez ve isteğinizi onaylamanız gerekmez. Sertifika bir yıl boyunca geçerlidir ve süresi dolmadan önce otomatik olarak yenilenir. [Yayılmayı beklemek](#wait-for-propagation)için devam edin. 

Otomatik doğrulama genellikle birkaç saat sürer. Etki alanınızı 24 saat içinde doğrulanıp görmüyorsanız bir destek bileti açın.

>[!NOTE]
>DNS sağlayıcınızla bir sertifika yetkilisi yetkilendirmesi (CAA) kaydınız varsa, bu, geçerli bir CA olarak DigiCert içermelidir. CAA kaydı, etki alanı sahiplerinin, etki alanı için sertifika verme yetkisine sahip DNS sağlayıcılarıyla belirtmesini sağlar. Bir CA, CAA kaydına sahip olan bir etki alanı için bir sertifika siparişi alırsa ve bu CA yetkili bir veren olarak listelenmiyorsa, sertifikayı bu etki alanına veya alt etki alanına verilmesine izin verilmez. CAA kayıtlarını yönetme hakkında bilgi için bkz. [CAA kayıtlarını yönetme](https://support.dnsimple.com/articles/manage-caa-record/). CAA kayıt aracı için bkz. [CAA kayıt Yardımcısı](https://sslmate.com/caa/).

### <a name="custom-domain-is-not-mapped-to-your-cdn-endpoint"></a>Özel etki alanı, CDN uç noktanızla eşlenmedi

>[!NOTE]
>**Akamai ' dan Azure CDN**kullanıyorsanız, otomatik etki alanı doğrulamasını etkinleştirmek IÇIN aşağıdaki CNAME ayarlanmalıdır. "_acme-Challenge. &lt;Özel etki alanı ana bilgisayar adı @ no__t-1-> CNAME-> &lt;Özel etki alanı hostname&gt;.ak-acme-challenge.azureedge.net"

CNAME kayıt girişi cdnverify alt etki alanını içeriyorsa, bu adımdaki yönergelerin geri kalanını izleyin.

DigiCert aşağıdaki e-posta adreslerine bir doğrulama e-postası gönderir. Aşağıdaki adreslerden birini doğrudan onaylayabildiğinizi doğrulayın:

admin @ @no__t -0your-Domain-Name. com @ no__t-1  
Yönetici @ @no__t -0your-Domain-Name. com @ no__t-1  
Web uzmanı @ @no__t -0your-Domain-Name. com @ no__t-1  
HOSTMASTER @ @no__t -0your-Domain-Name. com @ no__t-1  
postmaster @ @no__t -0your-Domain-Name. com @ no__t-1  

Birkaç dakika içinde, aşağıdaki örneğe benzer şekilde, isteği onaylamanızı isteyen bir e-posta almalısınız. Bir istenmeyen posta filtresi kullanıyorsanız, verification@digicert.com ' ı beyaz listesine ekleyin. 24 saat içinde bir e-posta almazsanız, Microsoft destek 'e başvurun.
    
![Etki alanı doğrulama e-postası](./media/cdn-custom-ssl/domain-validation-email.png)

Onay bağlantısına tıkladığınızda, aşağıdaki çevrimiçi onay formuna yönlendirilirsiniz: 
    
![Etki alanı doğrulama formu](./media/cdn-custom-ssl/domain-validation-form.png)

Formdaki yönergeleri izleyin; iki doğrulama seçeneğiniz vardır:

- Aynı kök etki alanı için aynı hesap aracılığıyla verilen tüm gelecek siparişleri onaylayabilirsiniz; Örneğin, contoso.com. Aynı kök etki alanı için ek özel etki alanları eklemeyi planlıyorsanız bu yaklaşım önerilir.

- Yalnızca bu istekte kullanılan belirli bir ana bilgisayar adını onaylayabilirsiniz. Sonraki istekler için ek onay gereklidir.

Onay sonrasında, DigiCert özel etki alanı adınız için sertifika oluşturmayı tamamlar. Sertifika bir yıl boyunca geçerlidir ve bu süre dolmadan önce otomatik olarak yenilenir.

## <a name="wait-for-propagation"></a>Yayılmayı bekle

Etki alanı adı doğrulandıktan sonra, özel etki alanı HTTPS özelliğinin etkinleştirilmesi 6-8 saate kadar sürebilir. İşlem tamamlandığında, Azure portal özel HTTPS durumu **etkin** olarak ayarlanır ve özel etki alanı iletişim kutusundaki dört işlem adımı tamamlanmış olarak işaretlenir. Özel etki alanınız artık HTTPS kullanmaya hazırdır.

![HTTPS iletişim kutusunu Etkinleştir](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>İşlem ilerleme durumu

Aşağıdaki tabloda, HTTPS 'yi etkinleştirdiğinizde gerçekleşen işlem ilerlemesi gösterilmektedir. HTTPS 'yi etkinleştirdikten sonra, özel etki alanı iletişim kutusunda dört işlem adımı görünür. Her bir adım etkin hale geldiğinde, adım ilerlerken ek alt adım ayrıntıları görüntülenir. Bu alt adımların hepsi gerçekleşmeyecektir. Bir adım başarıyla tamamlandıktan sonra bunun yanında yeşil bir onay işareti görünür. 

| İşlem adımı | İşlem alt adımı ayrıntıları | 
| --- | --- |
| 1 istek gönderiliyor | İstek gönderiliyor |
| | HTTPS isteğiniz gönderiliyor. |
| | HTTPS isteğiniz başarıyla gönderildi. |
| 2 etki alanı doğrulama | Etki alanı, CDN uç noktasıyla eşlenmişse otomatik olarak onaylanır. Aksi takdirde, bir doğrulama isteği, etki alanının kayıt kaydında listelenen e-postaya gönderilir (WHOSıS sahibi). Lütfen etki alanını mümkün olan en kısa sürede doğrulayın. |
| | Etki alanı sahipliğiniz başarıyla doğrulanmadı. |
| | Etki alanı sahiplik doğrulama isteği süresi dolmuştur (müşteri, büyük olasılıkla 6 gün içinde yanıt vermedi). HTTPS, etki alanınız üzerinde etkinleştirilmeyecektir. * |
| | Etki alanı sahipliği doğrulama isteği müşteri tarafından reddedildi. HTTPS, etki alanınız üzerinde etkinleştirilmeyecektir. * |
| 3 sertifika sağlama | Sertifika yetkilisi Şu anda etki alanı üzerinde HTTPS 'yi etkinleştirmek için gereken sertifikayı veriyor. |
| | Sertifika verildi ve şu anda CDN ağına dağıtılıyor. Bu işlem 6 saate kadar sürebilir. |
| | Sertifika, CDN ağına başarıyla dağıtıldı. |
| 4 Tamam | HTTPS, etki alanınızı başarıyla etkinleştirdi. |

\* bir hata oluşmadığı takdirde bu ileti görüntülenmez. 

İstek gönderilmeden önce bir hata oluşursa, aşağıdaki hata iletisi görüntülenir:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>



## <a name="clean-up-resources---disable-https"></a>Kaynakları Temizleme-HTTPS 'yi devre dışı bırakma

Yukarıdaki adımlarda, özel etki alanında HTTPS protokolünü etkinleştirdiniz. Artık HTTPS ile özel etki alanınızı kullanmak istemiyorsanız, bu adımları gerçekleştirerek HTTPS 'yi devre dışı bırakabilirsiniz:

### <a name="disable-the-https-feature"></a>HTTPS özelliğini devre dışı bırak 

1. [Azure Portal](https://portal.azure.com), **Microsoft 'tan Azure CDN Standart**Azure CDN, **Verizon profilinden Premium veya Azure CDN Premium** **'dan standart** ' a gidin.

2. Uç noktalar listesinde, özel etki alanınızı içeren uç noktaya tıklayın.

3. HTTPS 'yi devre dışı bırakmak istediğiniz özel etki alanına tıklayın.

    ![Özel etki alanları listesi](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. HTTPS 'yi devre dışı bırakmak için **Kapat** 'ı, ardından **Uygula**'yı tıklatın.

    ![Özel HTTPS iletişim kutusu](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="wait-for-propagation"></a>Yayılmayı bekle

Özel etki alanı HTTPS özelliği devre dışı bırakıldıktan sonra, bunun etkili olması 6-8 saat kadar sürebilir. İşlem tamamlandığında, Azure portal özel HTTPS durumu **devre dışı** olarak ayarlanır ve özel etki alanı iletişim kutusundaki üç işlem adımı tamamlanmış olarak işaretlenir. Özel etki alanınız artık HTTPS kullanamaz.

![HTTPS iletişim kutusunu devre dışı bırak](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

#### <a name="operation-progress"></a>İşlem ilerleme durumu

Aşağıdaki tabloda, HTTPS 'yi devre dışı bıraktığınızda gerçekleşen işlem ilerlemesi gösterilmektedir. HTTPS 'yi devre dışı bıraktıktan sonra, özel etki alanı iletişim kutusunda üç işlem adımı görünür. Her adım etkin hale geldiğinde, adım altında ek ayrıntılar gösterilir. Bir adım başarıyla tamamlandıktan sonra bunun yanında yeşil bir onay işareti görünür. 

| İşlem ilerleme durumu | İşlem ayrıntıları | 
| --- | --- |
| 1 istek gönderiliyor | İsteğiniz gönderiliyor |
| 2 sertifika sağlamayı kaldırma | Sertifika siliniyor |
| 3 Tamam | Sertifika silindi |

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

1. *Sertifika sağlayıcısı kim ve ne tür bir sertifika kullanıldı?*

    Hem Verizon hem de **Microsoft 'tan Azure CDN** **Azure CDN** için, özel etki alanınız için DigiCert tarafından sunulan adanmış/tek bir sertifika kullanılır. 

2. *IP tabanlı veya SNı TLS/SSL kullanıyor musunuz?*

    Hem **Verizon** Azure CDN hem de **Microsoft 'un** SNı TLS/SSL kullanan Azure CDN.

3. *DigiCert tarafından etki alanı doğrulama e-postasını almadım mı?*

    Özel etki alanınız için uç nokta ana bilgisayar adına doğrudan işaret eden bir CNAME girdiniz varsa (ve cdnverify alt etki alanı adını kullanmıyorsanız), bir etki alanı doğrulama e-postası almazsınız. Doğrulama otomatik olarak gerçekleşir. Aksi takdirde, CNAME girdiniz yoksa ve 24 saat içinde bir e-posta almadıysanız, Microsoft destek 'e başvurun.

4. *Bir SAN sertifikası, ayrılmış bir sertifikadan daha az güvenli mi?*
    
    SAN sertifikası, ayrılmış sertifikayla aynı şifreleme ve güvenlik standartlarına uyar. Tüm verilen SSL sertifikaları, gelişmiş sunucu güvenliği için SHA-256 kullanır.

5. *DNS sağlayıcıma sahip bir sertifika yetkilisi yetkilendirme kaydına ihtiyacım var mı?*

    Hayır, sertifika yetkilisi yetkilendirme kaydı Şu anda gerekli değildir. Ancak, varsa, geçerli bir CA olarak DigiCert içermelidir.

6. *20 Haziran 2018 ' de, varsayılan olarak SNı TLS/SSL ile ayrılmış bir sertifika kullanarak Verizon 'tan Azure CDN başladı. Konu diğer adları (SAN) sertifikası ve IP tabanlı TLS/SSL kullanarak var olan özel etki alanlarıma ne olur?*

    Microsoft, uygulamanıza yalnızca SNı istemci isteklerinin yapıldığını çözümlediği takdirde, mevcut etki alanlarınız yakında çıkacak aylarda tek bir sertifikaya geçirilir. Microsoft, uygulamanıza bazı SNı olmayan istemci istekleri algılarsa, etki alanlarınız IP tabanlı TLS/SSL ile SAN sertifikasında kalır. Herhangi bir durumda, bu isteklerin SNı veya SNı olmayan istemci istekleriniz olmadan hizmetinize veya desteğiniz için herhangi bir kesinti olmayacaktır.

7. *Sertifika yenilemesi kendi sertifikanızı getir ile nasıl çalışır?*

    Yeni bir sertifikanın PoP altyapısına dağıtılmasını sağlamak için yeni sertifikanızı Azure Anahtar Kasası 'na yüklemeniz, ardından Azure CDN SSL ayarlarınızda en yeni sertifika sürümünü seçip Kaydet ' e tıklayın. Azure CDN, yeni güncelleştirilmiş CERT propogate. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> - Özel etki alanında HTTPS protokolünü etkinleştirin.
> - CDN ile yönetilen bir sertifika kullanma 
> - Kendi sertifikanızı kullanın
> - Etki alanını doğrulayın.
> - Özel etki alanında HTTPS protokolünü devre dışı bırakın.

CDN uç noktanıza önbelleğe almayı yapılandırmayı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: Azure CDN önbelleğe alma kuralları ayarlama](cdn-caching-rules-tutorial.md)

