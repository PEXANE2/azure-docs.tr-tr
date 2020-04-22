---
title: Listeleme Türüne Göre Gereksinimler | Azure
description: Bu makalede, Azure Marketi'nde uygulamaların nasıl yayımlandırılabildiğini anlamaya çalışan uygunluk ölçütleri ve yayımlama gereksinimleri açıklanmaktadır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: dsindona
ms.openlocfilehash: c9936e9c406e262c06d9016b88f8999e46dcb917
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684351"
---
# <a name="requirements-by-listing-type"></a>Liste Türüne Göre Gereksinimler  
Teknik ve pazarlama içeriği gereksinimleri vitrine, teklif türüne ve giriş türüne göre değişir. Uyumluluğununuzu doğrulamak için aşağıdaki özellikleri gözden geçirin.  
1. Vitrin Gereksinimleri:  
    *   [AppSource](#storefront-requirements-appsource)  
    *   [Azure Market](#storefront-requirements-azure-marketplace)  
2. Listeleme Türü ve Teklif Türü Gereksinimleri:  
    *   Listeleme türleri ve teklif türleri hakkında daha fazla bilgi için, docs.microsoft.com/azure/marketplace/determine-your-listing-type'da bulunan Çözümünüzün Giriş Türünü Belirley sayfasını ziyaret [edin.](./determine-your-listing-type.md)  

## <a name="storefront-requirements-appsource"></a>Storefront Gereksinimleri: AppSource  
Aşağıdaki tabloda AppSource'da yayımlanma için ön koşul gereksinimleri açıklanmaktadır.  

| Gereksinim | Ayrıntılar | Gerekli ya da Önerilen |  
|:--- |:--- |:--- |  
| ***Azure Etkin Dizin (Azure AD)*** | Uygulamanız, Azure Active Directory federe tek oturum açma (Azure AD federe SSO) izni etkin leştirilmiş olmalıdır.<ul> <li>Azure AD federe SSO'yu etkinleştirme hakkında daha fazla bilgi için, [docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps'da](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)bulunan Azure Active Directory uygulama galerisi sayfasında olmayan uygulamalariçin Yapılandırma tek oturum açma sayfasını ziyaret edin.</li> </ul> | Gerekli |   
| ***Microsoft Bulut Hizmetleriyle Tümleştirme*** | Uygulamanız Microsoft Power BI, Cortana Intelligence veya Microsoft Azure hizmetleri gibi diğer Microsoft Cloud hizmetleriyle tümleştirmelidir.<ul> <li>Microsoft Cloud hizmetine örnek olarak Nesnelerin İnterneti'dir.</li> </ul> | Önerilen |  
| ***Hedef kitle*** | Uygulamanız iş yeri kullanıcıları ve işletme sahipleri için olmalıdır. | Gerekli | 
| ***İş için hizmet olarak yazılım (SaaS) uygulaması*** | Uygulamanız aşağıdaki gereksinimleri karşılamalıdır.<ul> <li>Bir line-of-business SaaS uygulaması</li> <li>İş-süreç odaklı</li> <li>İş müşterilerine yönelik</li> <li>Kullanıcıların kullanıcı adı ve parola gibi oturum açmaları için iş kimlik bilgilerini kullanmalarını sağlama</li> </ul> | Gerekli |  
| ***Ücretsiz deneme süresi ve deneme deneyimi*** | Bir müşterinin uygulamanızı sınırlı bir süre için ücretsiz olarak kullanabilmesi için uygulamanız aşağıdaki seçeneklerden birini içermelidir.<ul> <li>Müşteriler `try` AppSource içinde uygulamanızın deneme sürümünü başlatabilsin diye bir yöntem sağlayın</li> <li>AppSource'ta bir `request trial` seçenek sağlayın, böylece müşteriler uygulamanızın deneme sürümünü isteyebilir</li> </ul>Sağladığınız ücretsiz deneme sürümü, müşteriye ek bir ücret ödemeden uygulamanızı denemesi için önceden ayarlanmış bir süre sunmalıdır. | Gerekli |  
| ***Kolayca yapılandırılabilir, kullanıma hazır çözüm*** | Uygulamanız kolay ve hızlı yapılandırmak ve hiçbir özelleştirme gerektirmeden kurmak gerekir. | Gerekli |  
| ***Müşteri adayı yönetimi*** | Mağaza dan müşteri adayları almadan önce CRM'nizin müşteri adayı verilerini kabul etmesini etkinleştirin.<ul> <li>CRM'lere örnek olarak Marketo, Microsoft Dynamics veya Salesforce verilebilir:</li> </ul> | Gerekli |  
| ***Gizlilik politikası ve kullanım koşulları*** | Uygulamanız, herkese açık bir URL kullanarak gizlilik politikası sayfanıza bir bağlantı sağlamalıdır. Kullanım koşullarınız metin olarak yayımlama sırasında sağlanmalıdır. | Gerekli |  
| ***Destek*** | Uygulamanız, herkese açık bir URL kullanarak müşteri destek sayfanıza bir bağlantı sağlamalıdır. Uygulamanız deneme sürümüyse, deneme süresi boyunca hiçbir ek ücret ödemeden destek olmalısınız. | Gerekli |  

## <a name="storefront-requirements-azure-marketplace"></a>Storefront Gereksinimleri: Azure Marketi  
Aşağıda Azure Marketi'ndeki listeleme türleri için ön koşul gereksinimleri verilmiştir.  

| Gereksinim | Ayrıntılar | Listeleme türü |  
|:--- |:--- |:--- |  
| ***Katılım politikaları*** | Uygulamanız Azure Marketi katılım ilkelerini izlemeli.<ul> <li>Katılım ilkeleri hakkında daha fazla bilgi için azure.microsoft.com/support/legal/marketplace/participation-policies adresinde bulunan Azure Market Katılım İlkeleri sayfasını ziyaret [edin.](https://azure.microsoft.com/support/legal/marketplace/participation-policies)</li></ul> | list<br />Transact<br />trial |  
| ***Microsoft ile tümleştirme*** | Teklifiniz, bilgi işlem, ağ veya depolama gibi Microsoft Azure hizmet türlerini kullanmalı veya genişletmelidir. Teklifiniz, veritabanları, güvenlik veya ağ oluşturma gibi varolan bir Azure Marketi kategorisiyle hizalanmalıdır.<ul> <li>Market teklifleri hakkında daha fazla bilgi [için, azuremarketplace.microsoft.com/marketplace/apps](https://azuremarketplace.microsoft.com/marketplace/apps)adresinde bulunan Market Uygulamaları sayfasını ziyaret edin.</li> </ul> | list<br />Transact<br />trial |  
| ***Hedef kitle*** | Teklifiniz BT uzmanları, bulut geliştiricileri veya diğer teknik müşteri rolleri için olmalıdır. | list<br />Transact<br />trial |  
| ***Müşteri adayı yönetimi*** | Mağaza dan müşteri adayı almadan önce müşteri adayı verilerini kabul etmesi için CRM'nizi (Marketo, Microsoft Dynamics veya Salesforce) etkinleştirin. | list<br />Transact<br />trial |  
| ***Gizlilik politikası ve kullanım koşulları*** | Uygulamanız, herkese açık bir URL kullanarak gizlilik politikası sayfanıza bir bağlantı sağlamalıdır. Kullanım koşullarınız metin olarak yayımlama sırasında sağlanmalıdır. | list<br />Transact<br />trial |  
| ***Destek*** | Teklifiniz, herkese açık bir URL kullanarak müşteri destek sayfanıza bir bağlantı sağlamalıdır. Teklifiniz deneme sürümüyse, deneme süresi boyunca hiçbir ek ücret ödemeden destek olmalısınız. | Transact<br />trial |    

## <a name="non-transact-listings"></a>İşleme Olmayan İlanlar  
Bu bölümde, Transact giriş türünü kullanmayan tüm teklif türleri açıklanmaktadır. 

### <a name="list"></a>Liste  
Liste giriş türü, pazardaki vitrinlerde aşağıdaki Teklif türlerini içerir.  

| Teklif türü | Mağazası | Ayrıntılar |  
|:---        |:---        |:---     |  
| Danışmanlık Hizmetleri | AppSource | Gereksinimler: AppSource: List: Danışmanlık Hizmetleri |  
| Danışmanlık Hizmetleri | Azure Market | Gereksinimler: Azure Marketplace: Liste: Danışmanlık Hizmetleri |  
| Bana Ulaşın | AppSource | [](#) |  
| Bana Ulaşın | Azure Market | Gereksinimler: AppSource: List: Bana Ulaşın |  

#### <a name="requirements-appsource-list-consulting-service"></a>Gereksinimler: AppSource: Liste: Danışmanlık hizmeti  

| Gereksinimler | Ayrıntılar |  
|:--- |:--- |  
| Hizmet teklif özellikleri | Danışmanlık hizmetiniz aşağıdaki kriterleri karşılamalıdır.<ul> <li>Sabit kapsamlı, sabit süreli, sabit fiyatlı (veya ücretsiz) bir etkileşim sunun.</li> <li>Orient öncelikle ön satış için.</li> <li>Tek bir müşteriyle sınırlayın.</li> <li>Sahada davranış.</li> </ul> |  
| Danışmanlık Hizmetleri için ortak gereksinimleri | Hizmetiniz için ilgili alandaki kriterleri karşılarsınız.<table><tr><th>Çözüm Alanı</th><th>Ölçütler</th></tr><tr><td>Müşteri Etkileşimi için Dynamics 365</td><td>Silver veya Gold Cloud Müşteri İlişkileri Yönetimi uzmanlığına sahip olun.</td></tr><tr><td>Dynamics 365 Finans ve Operasyonlar için, Enterprise edition</td><td>25.000 TL veya daha uzun bir süredir bulut işlemlerinizden Silver veya Gold Enterprise Resource Planning uzmanlığıve geliri elde edin.</td></tr><tr><td>Dynamics 365 Finans ve Operasyonlar için, İş sürümü</td><td>Bir veya daha fazla müşteri için Bulut Hizmetleri Sağlayıcısı (CSP) veya Dijital Kayıt Ortağı (DPOR) olarak hizmet vermektedir.</td></tr><tr><td>Power BI</td><td>Çözüm Ortağı kriterlerini karşılayın.</td></tr><tr><td>PowerApps</td><td>Partner Vitrini çözümü ne varsa.</td></tr></table><ul> <li>Müşteri ilişkileri yönetimi hakkında daha fazla bilgi için [partner.microsoft.com/membership/cloud-customer-relationship-management-competency](https://partner.microsoft.com/membership/cloud-customer-relationship-management-competency)adresinde bulunan Bulut Müşteri İlişkileri Yönetimi sayfasını ziyaret edin.</li> <li>Kaynak planlama hakkında daha fazla bilgi [için, partner.microsoft.com/membership/enterprise-resource-planning-competency'da](https://partner.microsoft.com/membership/enterprise-resource-planning-competency)bulunan Kurumsal Kaynak Planlama sayfasını ziyaret edin.</li> <li>CSP hakkında daha fazla bilgi için [partner.microsoft.com/cloud-solution-provider'da](https://partner.microsoft.com/cloud-solution-provider)bulunan Bulut Hizmetleri Sağlayıcısı sayfasını ziyaret edin.</li> <li>DPOR hakkında daha fazla bilgi [için, partner.microsoft.com/membership/digital-partner-of-record'da](https://partner.microsoft.com/membership/digital-partner-of-record)bulunan Dijital Kayıt Ortağı ve Ortak Derneği sayfasını ziyaret edin.</li> <li>Çözüm ortağı kriterleri hakkında daha fazla bilgi için, solution [https://partner.microsoft.com/en-us/membership/partner-incentives](https://partner.microsoft.com/en-us/membership/partner-incentives)Partner Overview and Incentives dokümanında yer alan .</li> <li>Ortak vitrini hakkında daha fazla bilgi [için, powerapps.microsoft.com/partner-showcase'da](https://powerapps.microsoft.com/partner-showcase)bulunan Partner Vitrini sayfasını ziyaret edin.</li> </ul> |  

#### <a name="requirements-azure-marketplace-list-consulting-service"></a>Gereksinimler: Azure Marketi: Liste: Danışmanlık hizmeti  

| Gereksinimler | Ayrıntılar |  
|:--- |:--- |  
| Hizmet teklif özellikleri | Danışmanlık hizmetiniz aşağıdaki kriterleri karşılamalıdır.<ul> <li>Sabit kapsamlı, sabit süreli, sabit fiyatlı (veya ücretsiz) bir etkileşim sunun.</li> <li>Orient öncelikle ön satış için.</li> <li>Tek bir müşteriyle sınırlayın.</li> <li>Sahada davranış.</li> </ul> |  
| Danışmanlık Hizmetleri için ortak gereksinimleri | Hizmetiniz için ilgili alanda aşağıdaki yetkinliklerden birinde gümüş veya altın olmalıdır. <table><tr><th>Çözüm Alanı</th><th>Yetkinlik</th></tr><td>Bulut Platformu ve Altyapı</td><td>Bulut Platformu<br />Veri Merkezi</td><tr><td>Uygulama Geliştirme ve ISV</td><td>Uygulama Geliştirme<br />Uygulama Tümleştirme<br />DevOps</td></tr><tr><td>Veri Yönetimi ve Analitik</td><td>Veri Analizi<br />Veri Platformu</td></tr></table><ul> <li>Yetkinlikler hakkında daha fazla bilgi [için, partner.microsoft.com/membership/competencies'da](https://partner.microsoft.com/membership/competencies)bulunan Microsoft İş Ortağı Ağı sayfasını ziyaret edin.</li> <li>Giriş hakkında daha fazla bilgi [için, docs.microsoft.com/azure/marketplace/consulting-services](https://docs.microsoft.com/azure/marketplace/consulting-services)adresinde bulunan Azure Marketi Danışmanlık Hizmetleri sayfasını ziyaret edin.</li></ul> |  

<!-- #### Requirements: Azure Marketplace: List: Contact Me -->

---

### <a name="trial"></a>Deneme  

| Teklif türü | Mağazası | Ayrıntılar |  
|:---        |:---        |:---     |  
| Ücretsiz / SaaS deneme | AppSource | Listeleme Türü Gereksinimleri: Deneme |  
| Ücretsiz / SaaS deneme | Azure Market | Gereksinimler: Azure Marketi: Deneme: Ücretsiz deneme / SaaS deneme |  
| Etkileşimli tanıtım | AppSource | Listeleme Türü Gereksinimleri: Deneme |  
| Etkileşimli tanıtım | Azure Market | [Gereksinimler: Azure Marketi: Deneme: Etkileşimli demo](#requirements-azure-marketplace-trial-interactive-demo) |  
| Test sürüşü | AppSource | Listeleme Türü Gereksinimleri: Deneme |  
| Test sürüşü | Azure Market | [Gereksinimler: Azure Marketi: Deneme: Test sürüşü](#requirements-azure-marketplace-trial-test-drive) |  

#### <a name="requirements-azure-marketplace-trial"></a>Gereksinimler: Azure Marketi: Deneme  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Ücretsiz deneme süresi ve deneme deneyimi | Müşteriniz uygulamanızı sınırlı bir süre için ücretsiz olarak kullanabilir.<br /><br />Müşterinizin teklifiniz veya uygulamanız için herhangi bir lisans veya abonelik ücreti ödemesi gerekmez. Müşterinizin, Microsoft'un altında yatan birinci taraf ürün veya hizmeti için ödeme ödemesi gerekmez. Tüm deneme seçenekleri Azure aboneliğinize dağıtılır. Maliyet optimizasyonu ve yönetimi tek kontrol deneme var.<br /><br />Ücretsiz deneme sürümü, etkileşimli demo veya test sürüşü seçebilirsiniz. Ne seçerseniz seçin, ücretsiz deneme sürümünüz müşteriye uygulamayı ek ücret ödemeden denemesi için önceden ayarlanmış bir süre sunmalıdır.<ul> <li>Test sürüşü oluşturma işlemini başlatmak için, [amp-testdrive@microsoft.com](mailto:amp-testdrive@microsoft.com)'ye e-posta gönderin.</li> </ul>Not: Azure Marketi SaaS deneme deneyimleri, müşterilerin oturum açabilmek için iş kimlik bilgilerini kullanmasına izin vermelidir.<ul> <li>Daha fazla bilgi [için, docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences)adresinde bulunan AppSource deneme deneyimleri bölümünü ziyaret edin.</li> </ul> |  
| Kolayca yapılandırılabilir, kullanıma hazır çözüm | Uygulamanız kolay ve hızlı yapılandırmalı ve ayarlanmalıdır. |  
| Kullanılabilirlik / çalışma süresi | SaaS uygulamanızın veya platformunuzun çalışma süresi en az %99,9 olmalıdır. |  
| Azure Active Directory | Teklifiniz, Azure Active Directory (Azure AD) federe tek oturum açma (SSO) (Azure AD federe SSO) izni etkinleştirilmiş olmalıdır. |  

#### <a name="requirements-azure-marketplace-trial-free-trial--saas-trial"></a>Gereksinimler: Azure Marketi: Deneme: Ücretsiz deneme / SaaS deneme  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Müşterinin, ücretli kullanıma dönüştürmek için otomatik bir yöntemle satın almadan önce ürününüzü denemesini sağlar. Ayrıca müşteri için kavram kanıtları ve Microsoft satış ekipleriyle ortak etkileşim sağlar. | Çözümünüz sanal bir makine veya çözüm şablonudur.<br /><br />Çözümünüz bir SaaS teklifidir ve çok kiracılı bir SaaS ürünü sunarsınız.<br /><br />Bir müşteriyi hızlı bir şekilde çalışır hale getirmek için ilk çalıştırma deneyiminiz var.<br /><br />Tek bir kiracınız var, ancak müşterileri konuk kullanıcı olarak ekliyorsunuz. |  

#### <a name="requirements-azure-marketplace-trial-interactive-demo"></a>Gereksinimler: Azure Marketi: Deneme: Etkileşimli demo  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Müşterilerinizin kurulum karmaşıklığı olmadan çözümünüzü iş başında görmelerini sağlar. | Çözümünüz, deneme süresi içinde elde edilmesi zor olan karmaşık ayarlar gerektirir. |  

#### <a name="requirements-azure-marketplace-trial-test-drive"></a>Gereksinimler: Azure Marketi: Deneme: Test sürüşü  

| Avantaj | Gereksinim |  
|:--- |:--- |  
| Müşterinin ürününüzü satın almadan önce denemesini sağlar.<br /><br />Önceden yapılandırılmış ayarları kullanarak çözümünüzde rehberli bir deneyim sağlar.<br /><br />Test sürüşü kullanırken aşağıdaki ek avantajlar verebvardır.<ul> <li>Pazardaki kullanıcı aramalarının %27'si kullanıcılar tarafından yalnızca test sürücüleri ile teklifleri göstermek için rafine edilir.</li> <li>Test sürücüleri ile teklifler, olmayan tekliflere göre %38 daha fazla müşteri adayı oluşturur.</li> <li>Pazardaki yeni müşteri satın almalarının %36'sı test sürüşü yapan müşterilerden geliyor.</li> <li>Test sürücüleri, Microsoft alan satıcılarının birlikte satış çabaları için ürününüzü daha iyi anlamalarını sağlar.</li> </ul> | Çözümünüz tek bir kiracıya sahip sanal bir makine, çözüm şablonu veya SaaS uygulamasıdır veya sağlanması karmaşıktır. <br /><br />Deneme nizi ücretli bir teklife dönüştürmek için bir yönteminiz yok. |  

---

## <a name="transact-specific-listings"></a>İşleme Özel Listelemeler

### <a name="transact"></a>Transact  

| Teklif türü | Mağazası | Ayrıntılar |   
|:---        |:---        | :--- |  
| Azure uygulamaları: Yönetilen uygulama | Azure Market | Gereksinimler: Azure Marketi: İşleme: Azure uygulamaları: Yönetilen uygulama |  
| Azure uygulamaları: Çözüm şablonu | Azure Market | Gereksinimler: Azure Marketplace: Transact: Azure uygulamaları: Çözüm şablonu |  
| Kapsayıcılar | Azure Market | [Gereksinimler: Azure Marketplace: Transact: Konteyner](#requirements-azure-marketplace-transact-container) |  
| SaaS uygulaması  | Azure Market | [Gereksinimler: Azure Marketi: İşleme: SaaS uygulaması](#requirements-azure-marketplace-transact-saas-app) |  
| Sanal makine | Azure Market | [Gereksinimler: Azure Marketplace: Transact: Sanal makine](#requirements-azure-marketplace-transact-virtual-machine) |  

<!-- #### Requirements: Azure Marketplace: Transact: Azure apps: Managed app  

#### Requirements: Azure Marketplace: Transact: Azure apps: Solution template   -->

#### <a name="requirements-azure-marketplace-transact-container"></a>Gereksinimler: Azure Marketplace: Transact: Konteyner  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalama ve ölçüm | Ücretsiz veya BYOL faturalandırma modelini destekleyin. |  
| Docker tabanlı görüntü | Your container image must be based on the Docker image format and must be pulled from Azure Container Registries. |  

#### <a name="requirements-azure-marketplace-transact-saas-app"></a>Gereksinimler: Azure Marketi: İşleme: SaaS uygulaması  

| Gereksinim | Ayrıntılar |  
|:--- |:--- |  
| Faturalama ve ölçüm | Teklifiniz aylık sabit fiyata fiyatlandırılır. Kullanıma dayalı fiyatlandırma ve kullanıma dayalı *doğru-up* seçenekleri şu anda desteklenmez. |  
| İptal | Teklifiniz müşteri tarafından her zaman iptal edilebilir. |  
| İşlem Açılış Sayfası | Azure ortak markalı bir işlem açılış sayfası barındırın. Açılış sayfanız, müşterilerinizin SaaS hizmet hesabınızı oluşturmasına ve yönetmesine olanak tanır. |  
| SaaS Abonelik API | Bir kullanıcı hesabı ve hizmet planı oluşturmak, güncelleştirmek ve silmek için SaaS Aboneliği ile etkileşimedebilen bir hizmet sağlayın. Tüm kritik API değişiklikleri 24 saat içinde desteklenmelidir. Kritik olmayan API değişiklikleri düzenli aralıklarla güncelleştirilir. |  

#### <a name="requirements-azure-marketplace-transact-virtual-machine"></a>Gereksinimler: Azure Marketplace: Transact: Sanal makine  

| Gereksinim | Ayrıntılar |  
|:--- |:--- | 
| Faturalama ve ölçüm | VM'niz BYOL veya Pay-As-You-Go aylık faturalandırmayı desteklemelidir. |  
| Azure uyumlu sanal sabit disk (VHD) | VM'ler Windows veya Linux üzerine inşa edilmelidir.<ul> <li>Linux VHD oluşturma hakkında daha fazla bilgi için [Azure'da onaylanan Linux dağıtımlarına](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)bakın.</li> <li>Windows VHD oluşturma hakkında daha fazla bilgi için [bkz.](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md)</li> </ul> |  

## <a name="next-steps"></a>Sonraki adımlar
*   Azure [Marketi ve AppSource Publisher Guide](./marketplace-publishers-guide.md) sayfasını ziyaret edin.  

