---
title: Azure API Yönetimi SSS | Microsoft Dokümanlar
description: Azure API Yönetimi'nde sık sorulan soruların (SSS), desenlerin ve en iyi uygulamaların yanıtlarını öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: c32fdc67c74e100e0e31dad3afde128c05c356d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335981"
---
# <a name="azure-api-management-faqs"></a>Azure API Yönetimi SSS'leri
Azure API Yönetimi için sık sorulan soruların, desenlerin ve en iyi uygulamaların yanıtlarını alın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="contact-us"></a>Bizimle iletişim kurun
* [Microsoft Azure API Yönetimi ekibine nasıl soru sorabilirim?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
* [Bir özellik önizlemedeyken bu ne anlama gelir?](#what-does-it-mean-when-a-feature-is-in-preview)
* [API Management ağ geçidi ile arka uç hizmetleri arasındaki bağlantının güvenliğini nasıl sağlayabilirim?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
* [API Management hizmet örneğimi yeni bir örneğe nasıl kopyalayabilirim?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
* [API Yönetimi örneğimi programlı olarak yönetebilir miyim?](#can-i-manage-my-api-management-instance-programmatically)
* [Yöneticiler grubuna nasıl kullanıcı ekleyebilirim?](#how-do-i-add-a-user-to-the-administrators-group)
* [Eklemek istediğim ilke neden ilke düzenleyicisinde kullanılamıyor?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
* [Tek bir API'de birden çok ortam nasıl ayarlıyorum?](#how-do-i-set-up-multiple-environments-in-a-single-api)
* [API Yönetimi ile SOAP kullanabilir miyim?](#can-i-use-soap-with-api-management)
* [AD FS güvenliği olan bir OAuth 2.0 yetkilendirme sunucusu yapılandırabilir miyim?](#can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security)
* [API Yönetimi, birden çok coğrafi konuma dağıtımlarda hangi yönlendirme yöntemini kullanır?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
* [API Yönetimi hizmeti örneği oluşturmak için Azure Kaynak Yöneticisi şablonu kullanabilir miyim?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
* [Arka uç için kendi imzalı TLS/SSL sertifikası kullanabilir miyim?](#can-i-use-a-self-signed-tlsssl-certificate-for-a-back-end)
* [Git deposunu klonlamaya çalıştığımda neden kimlik doğrulama hatası alıyorum?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
* [API Yönetimi Azure ExpressRoute ile çalışıyor mu?](#does-api-management-work-with-azure-expressroute)
* [API Yönetimi bunlara dağıtıldığında neden Kaynak Yöneticisi stili VNET'lerde özel bir alt ağa ihtiyaç duyarız?](#why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them)
* [BIR VNET'e API Yönetimi dağıtılırken gereken minimum alt ağ boyutu nedir?](#what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet)
* [API Management hizmetini bir abonelikten diğerine taşıyabilir miyim?](#can-i-move-an-api-management-service-from-one-subscription-to-another)
* [API'mi alma yla ilgili kısıtlamalar veya bilinen sorunlar var mı?](#are-there-restrictions-on-or-known-issues-with-importing-my-api)

### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Microsoft Azure API Yönetimi ekibine nasıl soru sorabilirim?
Bu seçeneklerden birini kullanarak bizimle iletişime geçebilirsiniz:

* Sorularınızı [API Yönetimi MSDN forumumuzda](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt)yayınlayın.
* <mailto:apimgmt@microsoft.com> adresine e-posta gönderin.
* [Azure geri bildirim forumunda](https://feedback.azure.com/forums/248703-api-management)bize bir özellik isteği gönderin.

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Bir özellik önizlemedeyken bu ne anlama gelir?
Bir özellik önizlemedeyken, bu özelliğin sizin için nasıl çalıştığı hakkında etkin olarak geri bildirim istediğimiz anlamına gelir. Önizlemedeki bir özellik işlevsel olarak tamamlandı, ancak müşteri geri bildirimlerine yanıt olarak bir değişiklik yapmamız mümkündür. Üretim ortamınızda önizlemede olan bir özelliğe bağımlı olmadığınızı öneririz. Önizleme özellikleri hakkında herhangi bir geri bildiriminiz varsa, lütfen [Microsoft Azure API Yönetimi ekibine nasıl bir soru sorabilirim?'deki](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)iletişim seçeneklerinden biri aracılığıyla bize bildirin?

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>API Management ağ geçidi ile arka uç hizmetleri arasındaki bağlantının güvenliğini nasıl sağlayabilirim?
API Yönetimi ağ geçidi ile arka uç hizmetleriniz arasındaki bağlantıyı güvence altına almak için çeşitli seçenekleriniz var. Şunları yapabilirsiniz:

* HTTP temel kimlik doğrulamasını kullanın. Daha fazla bilgi için, [ilk API'nizi içe aktar'a bakın ve yayımlayın.](import-and-publish.md)
* [Azure API Yönetimi'nde istemci sertifikası kimlik doğrulamasını kullanarak arka uç hizmetlerinin nasıl güvenli hale getirilir'de](api-management-howto-mutual-certificates.md)açıklandığı gibi TLS ortak kimlik doğrulamasını kullanın.
* Arka uç hizmetinizde IP beyaz listesini kullanın. Tüketim katmanı hariç API Yönetimi'nin tüm katmanlarında, ip [dokümantasyonu makalesinde](api-management-howto-ip-addresses.md)açıklanan birkaç uyarıyla ağ geçidinin IP adresi sabit kalır.
* API Yönetimi örneğini bir Azure Sanal Ağına bağlayın.

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>API Management hizmet örneğimi yeni bir örneğe nasıl kopyalayabilirim?
Bir API Yönetimi örneğini yeni bir örneğe kopyalamak istiyorsanız birkaç seçeneğiniz var. Şunları yapabilirsiniz:

* API Yönetimi'nde yedekleme ve geri yükleme işlevini kullanın. Daha fazla bilgi için, [Azure API Yönetimi'nde hizmet yedekleme ve geri yükleme kullanarak olağanüstü durum kurtarma nasıl uygulanacağınız](api-management-howto-disaster-recovery-backup-restore.md)hakkında bilgi agörün.
* [API Management REST API'yi](/rest/api/apimanagement/)kullanarak kendi yedekleme ve geri yükleme özelliğinizi oluşturun. Varlıkları istediğiniz hizmet örneğinden kaydetmek ve geri yüklemek için REST API'sını kullanın.
* Git'i kullanarak hizmet yapılandırmasını karşıdan yükleyin ve ardından yeni bir örne yükleyin. Daha fazla bilgi [için, Git'i kullanarak API Yönetimi hizmet yapılandırmanızı nasıl kaydedip yapılandırılabilirsiniz.](api-management-configuration-repository-git.md)

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>API Yönetimi örneğimi programlı olarak yönetebilir miyim?
Evet, API Yönetimini şu kullanarak programlı olarak yönetebilirsiniz:

* [API Yönetimi REST API](/rest/api/apimanagement/).
* [Microsoft Azure ApiManagement Hizmet Yönetimi Kitaplığı SDK.](https://aka.ms/apimsdk)
* [Servis dağıtım](https://docs.microsoft.com/powershell/module/wds) ve [Servis yönetimi](https://docs.microsoft.com/powershell/azure/servicemanagement/overview) PowerShell cmdlets.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Yöneticiler grubuna nasıl kullanıcı ekleyebilirim?
Yöneticiler grubuna şu şekilde bir kullanıcı ekleyebilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Güncelleştirmek istediğiniz API Yönetimi örneğine sahip kaynak grubuna gidin.
3. API Yönetimi'nde, **Api Yönetim Hizmeti Katılımcısı** rolünü kullanıcıya atayın.

Artık yeni eklenen katılımcı Azure PowerShell [cmdlets'i](https://docs.microsoft.com/powershell/azure/overview)kullanabilir. Yönetici olarak şu şekilde oturum açabilirsiniz:

1. Oturum `Connect-AzAccount` etmek için cmdlet'i kullanın.
2. Bağlamı, hizmeti kullanan `Set-AzContext -SubscriptionID <subscriptionGUID>`aboneye ayarlayın.
3. Kullanarak tek bir oturum açma `Get-AzApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`URL'si alın.
4. Yönetici portalına erişmek için URL'yi kullanın.

### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Eklemek istediğim ilke neden ilke düzenleyicisinde kullanılamıyor?
Eklemek istediğiniz ilke, ilke düzenleyicisinde soluk veya gölgeli görünüyorsa, ilke için doğru kapsamda olduğunuzdan emin olun. Her ilke bildirimi, belirli kapsamlarda ve ilke bölümlerinde kullanmanız için tasarlanmıştır. Bir ilke nin ilke bölümlerini ve kapsamlarını gözden geçirmek [için, API Yönetimi ilkelerinde](/azure/api-management/api-management-policies)ilkenin Kullanım bölümüne bakın.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Tek bir API'de birden çok ortam nasıl ayarlıyorum?
Tek bir API'de bir test ortamı ve üretim ortamı gibi birden çok ortam ayarlamak için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

* Aynı kiracıda farklı API'ler barındırın.
* Aynı API'leri farklı kiracılarda barındırın.

### <a name="can-i-use-soap-with-api-management"></a>API Yönetimi ile SOAP kullanabilir miyim?
[SOAP geçiş](https://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) desteği artık mevcuttur. Yöneticiler SOAP hizmetinin WSDL'sini içe aktarabilir ve Azure API Yönetimi bir SOAP ön uç oluşturur. Geliştirici portalı dokümantasyonu, test konsolu, politikalar ve analizlerin tümü SOAP hizmetleri için kullanılabilir.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>AD FS güvenliği olan bir OAuth 2.0 yetkilendirme sunucusu yapılandırabilir miyim?
Active Directory Federation Services (AD FS) güvenliğine sahip bir OAuth 2.0 yetkilendirme sunucusunun nasıl yapılandırılabildiğini öğrenmek için, [API Yönetiminde ADFS'yi kullanma'ya](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/)bakın.

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>API Yönetimi, birden çok coğrafi konuma dağıtımlarda hangi yönlendirme yöntemini kullanır?
API Yönetimi, birden çok coğrafi konuma yapılan dağıtımlarda [performans trafiği yönlendirme yöntemini](../traffic-manager/traffic-manager-routing-methods.md#performance) kullanır. Gelen trafik en yakın API ağ geçidine yönlendirilir. Bir bölge çevrimdışı olursa, gelen trafik otomatik olarak bir sonraki en yakın ağ geçidine yönlendirilir. [Trafik Yöneticisi yönlendirme yöntemlerinde](../traffic-manager/traffic-manager-routing-methods.md)yönlendirme yöntemleri hakkında daha fazla bilgi edinin.

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>API Yönetimi hizmeti örneği oluşturmak için Azure Kaynak Yöneticisi şablonu kullanabilir miyim?
Evet. Azure [API Yönetim Hizmeti](https://aka.ms/apimtemplate) hızlı başlatma şablonlarına bakın.

### <a name="can-i-use-a-self-signed-tlsssl-certificate-for-a-back-end"></a>Arka uç için kendi imzalı TLS/SSL sertifikası kullanabilir miyim?
Evet. Bu PowerShell üzerinden veya doğrudan API göndererek yapılabilir. Bu, sertifika zinciri doğrulaması devre dışı kalır ve API Yönetimi'nden arka uç hizmetlerine iletişim kurarken kendi imzalı veya özel olarak imzalanmış sertifikalar kullanmanıza olanak sağlar.

#### <a name="powershell-method"></a>Powershell yöntemi ####
[`New-AzApiManagementBackend`](https://docs.microsoft.com/powershell/module/az.apimanagement/new-azapimanagementbackend) (Yeni arka uç için) [`Set-AzApiManagementBackend`](https://docs.microsoft.com/powershell/module/az.apimanagement/set-azapimanagementbackend) veya (mevcut arka uç için) PowerShell cmdlets kullanın ve parametreyi `-SkipCertificateChainValidation` `True`.

```powershell
$context = New-AzApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

#### <a name="direct-api-update-method"></a>Doğrudan API güncelleştirme yöntemi ####
1. API Yönetimi'ni kullanarak bir [Backend](/rest/api/apimanagement/) varlığı oluşturun.     
2. **SkipCertificateChainValidation** özelliğini **doğru**olarak ayarlayın.     
3. Artık kendi imzalı sertifikalara izin vermek istemiyorsanız, Arka Uç varlığını silin veya **skipCertificateChainValidation** özelliğini **false**olarak ayarlayın.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Bir Git deposunu kopyalamaya çalıştığımda neden kimlik doğrulaması hatası alıyorum?
Git Kimlik Bilgisi Yöneticisi kullanıyorsanız veya Visual Studio'yu kullanarak git deposunu klonlamaya çalışıyorsanız, Windows kimlik bilgileri iletişim kutusuyla ilgili bilinen bir sorunla karşınıza çıkabilir. İletişim kutusu parola uzunluğunu 127 karakterle sınırlar ve Microsoft tarafından oluşturulan parolayı truncates. Şifreyi kısaltmaya çalışıyoruz. Şimdilik, git deposunu klonlamak için lütfen Git Bash'i kullanın.

### <a name="does-api-management-work-with-azure-expressroute"></a>API Yönetimi Azure ExpressRoute ile çalışıyor mu?
Evet. API Yönetimi Azure ExpressRoute ile çalışır.

### <a name="why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them"></a>API Yönetimi bunlara dağıtıldığında neden Kaynak Yöneticisi stili VNET'lerde özel bir alt ağa ihtiyaç duyarız?
API Yönetimi için özel alt ağ gereksinimi, Klasik (PAAS V1 katmanı) dağıtım modeli üzerine inşa edilmiş olması gerçeğinden gelir. Kaynak Yöneticisi VNET'e (V2 katmanı) dağıtabilsek de, bunun sonuçları vardır. Azure'daki Klasik dağıtım modeli Kaynak Yöneticisi modeliyle sıkı bir şekilde birleştirilmez ve v2 katmanında bir kaynak oluşturursanız, V1 katmanı bu konuda bilgi vermez ve API Yönetimi zaten NIC'ye ayrılmış bir IP kullanmaya çalışmak gibi sorunlar olabilir ( V2 üzerine inşa edilmiştir).
Azure'daki Klasik ve Kaynak Yöneticisi modellerinin farkı hakkında daha fazla bilgi edinmek için [dağıtım modellerindeki farka](../azure-resource-manager/management/deployment-models.md)bakın.

### <a name="what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet"></a>BIR VNET'e API Yönetimi dağıtılırken gereken minimum alt ağ boyutu nedir?
API Yönetimi dağıtmak için gereken minimum alt ağ boyutu ,Azure'un desteklediği minimum alt ağ boyutu olan [/29'dur.](../virtual-network/virtual-networks-faq.md#configuration)

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>API Management hizmetini bir abonelikten diğerine taşıyabilir miyim?
Evet. Nasıl yapılacağını öğrenmek [için](../azure-resource-manager/management/move-resource-group-and-subscription.md)bkz.

### <a name="are-there-restrictions-on-or-known-issues-with-importing-my-api"></a>API'mi alma yla ilgili kısıtlamalar veya bilinen sorunlar var mı?
Açık API(Swagger), WSDL ve WADL biçimleri için bilinen sorunlar ve [kısıtlamalar.](api-management-api-import-restrictions.md)
