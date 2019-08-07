---
title: Ticari Market 'te yeni bir SaaS teklifi oluşturun
description: Azure Marketi, AppSource veya Microsoft Iş Ortağı Merkezi 'ndeki ticari Market portalını kullanan bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listeleme veya satma için yeni bir hizmet olarak yazılım (SaaS) teklifi oluşturma.
author: mattwojo
manager: evansma
ms.author: mattwoj
ms.service: marketplace
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 300dd634cc99e5203145619b475d813705f09553
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815682"
---
# <a name="create-a-new-saas-offer"></a>Yeni bir SaaS teklifi oluşturun

Hizmet olarak yazılım (SaaS) teklifi oluşturmaya başlamak için, ilk olarak [bir Iş Ortağı Merkezi hesabı](./create-account.md) oluşturduğunuzdan ve **genel bakış** sekmesi seçili olarak [ticari Market panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers)açın.

![Iş Ortağı Merkezi 'nde ticari Market panosu](./media/new-offer-overview.png)

\+ **Yeni teklifi seçin...** düğmesine tıklayın ve ardından **hizmet olarak yazılım** menü öğesini seçin. 

Diğer teklif türlerinden birini seçerseniz, eski [bulut iş ortağı portalı](https://cloudpartner.azure.com/)yönlendirilirsiniz.  Yalnızca SaaS teklifleri Iş Ortağı Merkezi 'nde ticari Market portalında Şu anda kullanılabilir. 

![Iş Ortağı Merkezi 'nde teklif penceresi oluştur](./media/new-offer-click.png)

**Yeni teklif** iletişim kutusu görüntülenir. 

![Yeni teklif iletişim kutusu](./media/new-offer-popup.png)


## <a name="offer-id-and-alias"></a>Teklif KIMLIĞI ve diğer ad

- **TEKLIF kimliği**: Hesabınızdaki her teklif için benzersiz tanımlayıcı. Bu KIMLIK, Market teklifi ve Azure Resource Manager şablonları (varsa) için URL adresindeki müşterilere görünür olacaktır. Teklif KIMLIĞI küçük harf, alfasayısal (kısa çizgiler ve alt çizgiler dahil ancak boşluk olmadan) olmalıdır. Bu 50 karakterle sınırlıdır ve *Oluştur*' u seçtikten sonra değiştirilemez.  
Örnek: test-teklif-1
<br>URL 'ye neden olur:`https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`

- **Teklif diğer adı**: Iş Ortağı Merkezi portalı içindeki teklifine başvurmak için kullanılan ad. Bu ad Market 'te kullanılmayacak ve *teklif adından* ve müşterilere gösterilecek diğer değerlerden farklı. Bu değer, *Oluştur*' u seçtikten sonra değiştirilemez.

<br>Örnek: Test teklifi 1&#8482;

**Oluştur**’u seçin.  Bu teklif için bir **teklif genel bakış** sayfası oluşturulur.  

<!---
![Offer overview on Partner Center](./media/commercial-marketplace-offer-overview.png)
-->

## <a name="offer-overview"></a>Teklifin genel bakış

**Teklif genel bakış** sayfası şunları içerir: 

- **Yayımlama durumu** , bu teklifi yayımlamak için gereken adımların görsel bir gösterimini ve her adımın tamamlanması için ne kadar sürdüğünü gösterir. Tamamlanmamış yayımlama adımı simgeleri gri olacak. 

- **Teklif genel bakış** menüsü, bu teklif üzerinde işlem gerçekleştirmek için bağlantıların bir listesini içerir. Bu işlem listesi, teklifiniz için yaptığınız seçime göre değişecektir.  
    - Teklif bir taslak ise, taslağı Sil 
    - Teklif canlı ise, satış teklifini durdur 
    - Teklif önizleme sürümündedir – canlı ol 
    - Yayımcı oturumu kapatma Işlemini tamamlamadıysanız, yayımlamayı Iptal edin

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif kurulumu** sekmesi aşağıdaki bilgileri ister. Bu alanları tamamladıktan sonra **Kaydet** ' i seçin.

- **Microsoft üzerinden satımek istiyor musunuz?** (Evet/Hayır)
    - **Evet**, Microsoft 'un sizin adınıza Market işlemlerini barındırmakla Microsoft aracılığıyla teklifinizi satmak istiyorsunuz; veya 
    - **Hayır**, sizin teklifinizi, Microsoft 'tan bağımsız olarak tüm parasal işlemleri işleyerek yalnızca marketler aracılığıyla listelemek tercih edersiniz.    

### <a name="sell-through-microsoft"></a>Microsoft ile satış

Microsoft ile satış yapmak daha iyi müşteri bulma ve alma olanağı sağlar, Microsoft 'un Market işlemlerini sizin adınıza barındırmasına olanak tanır ve Microsoft 'un küresel olarak kullanılabilir ticari özellikleri avantajlarından yararlanır.

#### <a name="saas-offer-requirements"></a>SaaS teklif gereksinimleri

Iş Ortağı Merkezi 'nde ticari Market ile hizmet olarak yazılım (SaaS) teklifleri listelemek için aşağıdaki kriterlerin karşılanması gerekir:

- Teklifinizin kimlik yönetimi ve kimlik doğrulaması için [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) kullanması gerekir.
- Teklifinizin Azure Marketi ile tümleştirilecek [SaaS karşılama API 'lerini](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) kullanması gerekir.
- Daha kapsamlı gereksinimler için [SaaS teklifi Yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide)' na bakın.

#### <a name="saas-on-azure-billing-infrastructure-costs"></a>Azure Faturalandırma altyapısı maliyetlerinde SaaS
SaaS teklifi Azure 'da barındırılıyorsa, yayımcı olarak Azure altyapı kullanım ücretleri ve yazılım lisanslama ücretleri için tek bir maliyet öğesi olarak hesaba sahip olmanız gerekir. Bu maliyet, müşteriye yönelik sabit bir aylık ücret olarak gösterilir. Azure altyapı kullanımı, iş ortağı tarafından doğrudan yönetilir ve size faturalandırılır. Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez. Yayımcılar genellikle Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmasına paketlendirmeyi tercih edebilir. 

Yazılım Lisanslama ücretleri aylık, yinelenen bir site tabanlı abonelik sabit fiyat ücreti olarak sunulur ve tarifeli veya tüketim temelli değildir.

|**Lisans maliyetiniz**|**$100/ay**|
|:---|:---|
|Azure kullanım maliyeti (D1/1-çekirdek)|Müşteriye değil doğrudan yayımcıya faturalandırılır|
|Müşteri Microsoft tarafından faturalandırılır|aylık $100,00 (yayımcı, lisans ücretindeki herhangi bir tahakkuk eden veya geçiş altyapı maliyeti için hesap olmalıdır)|

- Bu senaryoda, Microsoft, yazılım lisansınızın $100,00 ' i faturalandırır ve Yayımcı için $80,00 ' i ödeder.
- **Azaltılmış Market hizmet ücreti** için uygun olan iş ortakları, 2019 2020 Haziran 'a kadar Mayıs 'a kadar olan SaaS teklifleriyle daha düşük bir işlem ücreti görür. Bu senaryoda, Microsoft, yazılım lisansınızın $100,00 ' i faturalandırır ve Yayımcı için $90,00 ' i ödeder.

> [!NOTE]
> **Daha az Market hizmeti ücreti**: Ticari marketi 'nde yayımladığınız belirli SaaS teklifleri için Microsoft, Market hizmet ücretini% 20 ' den (Microsoft Publisher anlaşmasındaki açıklandığı gibi)% 10 oranında azaltacak. Teklifinizin uygun olmasını sağlamak için, tekliflerinizin en az birinin, Microsoft tarafından IP ortak satışı veya IP ortak satışı önceliği atanmış olarak belirlenmiş olması gerekir.  Bu ay için daha düşük olan bu Market servis ücretini almak amacıyla her bir takvim ayının sonundan önce uygunluk en az beş (5) iş gününe ulaşılmalıdır.  Düşük Market hizmet ücreti, sanal makineler, yönetilen uygulamalar veya ticari Market 'ten sunulan diğer ürünlerle ilgili değildir.  Daha az Market hizmet ücreti yalnızca Microsoft tarafından 1 Mayıs 2019 ile 30 Haziran 2020 arasında toplanan lisans ücretleri için uygun tekliflerdir.  Bu süreden sonra Market hizmeti ücreti normal miktarına geri döner. 

|**Microsoft faturaları**|**$100/ay**|
|:---|:---|
|Microsoft, lisans maliyetinizi% 80 oranında ödetir <br>**Microsoft, tam SaaS uygulamaları için lisans maliyetlerinizin% 90 ' ını öder*|$80,00/ay <br>*$* 90,00/ay *|


#### <a name="csp-program-opt-in"></a>CSP program kabul etme
[Bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) programı, yazılım tekliflerini minimum pazarlama ve satış yatırımlarına sahip milyonlarca Microsoft müşterilerine ulaşmaya olanak sağlar.

- **Lardan Teklifinizin CSP programında** kullanılabilmesini sağlama (onay kutusu)

, Teklifinizin CSP programında kullanılabilmesini sağlamak için, bulut çözümü sağlayıcılarının, müşterilerine paketlenmiş bir çözümün parçası olarak ürününüzü satmasına olanak sağlar. 

### <a name="list-through-microsoft"></a>Microsoft ile Listele

Market listesi oluşturarak işletmenizi Microsoft ile yükseltin. Microsoft 'un doğrudan yazılım lisans işlemlerine katılmayacağı, teklifinizi yalnızca bir şekilde listelemek için seçin. İlişkili bir işlem ücreti yoktur ve Yayımcı, müşteriden toplanan tüm yazılım lisanslama ücretlerine% 100 oranında devam eder. Ancak yayımcı, yazılım lisans işleminin, bunlarla sınırlı olmamak üzere tüm yönlerini desteklemekten sorumludur: sipariş karşılama, ölçüm, faturalandırma, faturalama, ödeme ve koleksiyon. 

- **Potansiyel müşterilerin bu liste teklifiyle nasıl etkileşime geçmesini istiyorsunuz?**

##### <a name="get-it-now-free"></a>Şimdi alın (ücretsiz)
Uygulamanıza erişebilecekleri geçerli bir URL ( *http* veya *https*ile başlayarak) sağlayarak müşterilerinizin teklifinizi ücretsiz olarak listeleyin.  Örneğin, `https://contoso.com/saas-app`

##### <a name="free-trial-listing"></a>Ücretsiz deneme (listeleme)
[Azure Active Directory (Azure AD) kullanarak tek tıklamayla kimlik doğrulama](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials)aracılığıyla denemeyi alabileceğiniz, GEÇERLI bir URL ( *http* veya *https*ile başlayarak) sağlayarak, ücretsiz bir deneme bağlantısı olan müşterilere teklifinizi listeleyin.  Örneğin: `https://contoso.com/trial/saas-app` Ücretsiz denemelerdeki teklif listesi, hizmetiniz tarafından oluşturulur, yönetilir ve yapılandırılır ve Microsoft tarafından yönetilen abonelikler içermez.

> [!NOTE]
> Deneme bağlantınız aracılığıyla uygulamanızın alacağı belirteçler yalnızca, uygulamanızda hesap oluşturmayı otomatikleştirmek için Azure AD aracılığıyla Kullanıcı bilgilerini almak üzere kullanılabilir. Bu belirteci kullanarak kimlik doğrulaması için Microsoft hesapları (MSA) desteklenmez.

##### <a name="contact-me"></a>Benimle iletişime geçin
Müşteri Ilişkileri yönetimi (CRM) sisteminizi bağlayarak müşteri iletişim bilgilerini toplayın. Müşterinin, bilgilerini paylaşması için izin istenir. Bu müşteri ayrıntıları, teklifinizin bulunduğu teklif adı, KIMLIĞI ve Market kaynağıyla birlikte, yapılandırdığınız CRM sistemine gönderilir. CRM 'nizi yapılandırma hakkında daha fazla bilgi için bkz. [Connect lider yönetimi](#connect-lead-management). 

## <a name="example-marketplace-offer-listing"></a>Örnek Market teklif listesi

![Not ile örnek Market teklif listesi](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Sınama sürücüsünü etkinleştir

Test sürücüsü, bu kullanıcılara teklifinizi "satın almadan önce dene" seçeneği sunarak, daha fazla dönüştürmeye ve yüksek oranda nitelikli müşteri adaylarının oluşturulmasına neden olacak şekilde sergilemenin harika bir yoludur. [Sınama sürücüleri hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

- **Sınama sürücüsünü etkinleştir** kutusunun 

Sınama sürücüsünü etkinleştirerek, müşterilerin teklifinizi sabit bir süre için denemesini sağlayacak bir tanıtım ortamı yapılandırmanız istenir. 

### <a name="type-of-test-drive"></a>Test sürücüsünün türü

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : Çözümünüzü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonu. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır.
- **[Iş Merkezi Için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** : Microsoft, bir Iş Merkezi kurumsal kaynak planlama sistemi (Finans, işlemler, tedarik zinciri, CRM vb.) için test sürücüsü hizmetini (sağlama ve dağıtım dahil) barındırır ve korur.  
- **[Müşteri katılımı Için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** : Microsoft, bir müşteri katılım sistemi (Sales, Service, Project Service, alan hizmeti vb.) için test sürücüsü hizmetini (sağlama ve dağıtım dahil) barındırır ve korur.  
- **[İşlemler Için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** : Microsoft, finans ve Işlemler kurumsal kaynak planlama sistemi (Finans, işlemler, üretim, tedarik zinciri vb.) için test sürücüsü hizmetini (sağlama ve dağıtım dahil) barındırır ve korur. 
- **[Mantıksal uygulama](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : Tüm karmaşık çözüm mimarilerini çevreleyen bir dağıtım şablonu. Özel ürünlerin bu tür test sürücüsü kullanması gerekir.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** : Özel olarak oluşturulmuş bir panoya eklenmiş bir bağlantı. Etkileşimli bir Power BI görseli göstermek isteyen ürünlerin bu tür test sürücüsünü kullanması gerekir. Buraya yüklemeniz gereken, katıştırılmış Power BI URL 'sidir.

#### <a name="additional-test-drive-resources"></a>Ek test sürücüsü kaynakları
- [Test sürücüsü teknik En Iyi uygulamaları](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Test sürüşü Pazarlama En Iyi yöntemleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test sürücüsüne genel bakış bir sayfalayıcı](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlama

Teklifinizi markette listeleyerek ve Müşteri Ilişkileri yönetimi (CRM) sisteminizin dökümünü yaparak müşterilere doğrudan bağlanın ve müşteri iletişim bilgilerini bir müşterinin ilgilenmesini ifade ettikten veya dağıtabilmeniz için ürünüyle.

- **Bir müşteri adayı hedefi seçin** (açılan menü): Müşteri adaylarını göndermemizi istediğiniz CRM sistemine bağlantı ayrıntılarını sağlayın. 

İş Ortağı Merkezi, müşteri adayı yönetimi için aşağıdaki CRM sistemlerini destekler. Kurulum yönergeleri için bağlantıyı seçin.

- Azure blob: iletişim e-postası, kapsayıcı adı ve depolama hesabı bağlantı dizesi belirtin. 
- [Azure tablosu](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table) : iletişim e-postası ve depolama hesabı bağlantı dizesi belirtin. 
- [DYNAMICS CRM Online](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) – iletişim e-POSTASı, URL ve kimlik doğrulama modu (Office 365 veya Azure Active Directory) sağlayın.
- [Https uç noktası](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-https) – iletişim e-POSTASı ve HTTPS uç nokta URL 'si sağlayın 
- [Marketo](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-marketo) : iletişim e-postası, form kimliği, daha fazla hesap kimliği ve sunucu kimliği sağlar.
- [Salesforce](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-salesforce) -iletişim e-postası ve kuruluş kimliği sağlayın. 

#### <a name="additional-lead-management-resources"></a>Ek lider Yönetimi kaynakları
- [Müşteri adayı yönetimi SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Ortak lider yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri adayı yönetimine genel bakış bir sayfalayıcı](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf)

Sonraki bölüme geçmeden önce **kaydetmeyi** unutmayın!

## <a name="properties"></a>Özellikler
**Özellikler** sekmesi, teklifinizi markette gruplandırmak için kullanılan kategorileri ve endüstrileri, teklifinizi destekleyen yasal sözleşmeleri ve uygulama sürümünüzü tanımlamanızı ister. 

Bu alanları tamamladıktan sonra **Kaydet** ' i seçin. 

### <a name="category"></a>Category
Teklifinizi uygun Market arama alanlarında gruplandırmak için en az bir (1) ve en fazla üç (3) kategori seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini lütfen unutmayın. 

### <a name="industry"></a>Sektör
Teklifinizi uygun Market arama alanlarıyla gruplandırmak için kullanılan en fazla iki (2) sektör seçin. Teklifiniz bir sektöre özgü değilse, bir tane seçmeyin. Teklifinizin, teklif açıklamasında seçili sektörlerin nasıl destekdiğine lütfen ulaşın. 

### <a name="app-version"></a>Uygulama sürümü
Bu, teklifinizin sürüm numarasını belirlemek için AppSource marketi 'nde kullanılan isteğe bağlı bir alandır. 

### <a name="standard-contract"></a>Standart Sözleşme

- **Standart Sözleşme mi kullanıyorsunuz?**

Müşteriler için satın alma sürecini basitleştirmek ve yazılım satıcılarının yasal karmaşıklığını azaltmak için, Microsoft, Market 'teki bir işlemin kolaylaştırılmasına yardımcı olmak üzere standart bir sözleşme şablonu sunmaktadır. 

Azure Market yayımcıları, özel hüküm ve koşulları ortadan kaldırmaktansa, müşterilerine yalnızca bir kez ve kabul etmesi gereken standart sözleşme kapsamında yazılım sunmayı tercih edebilir. 

Standart Sözleşme şurada bulunabilir: https://go.microsoft.com/fwlink/?linkid=2041178.

#### <a name="terms-of-use"></a>Kullanım koşulları

Lisans koşullarınız standart sözleşmeden farklıysa, size ait yasal kullanım koşullarınızı buraya girmeye karar verebilirsiniz. Ayrıca, bu alana en fazla 10.000 karakter girebilirsiniz. Kullanım koşullarınız daha uzun bir açıklama gerektiriyorsa, bu alana ek lisans koşullarınızın bulunabileceği tek bir URL bağlantısı girin. Bu, müşterilere etkin bir bağlantı olarak görüntülenecektir.

Müşterilerinizin uygulamanızı deneyebilmeleri için önce bu koşulları kabul etmesi gerekir. 

Sonraki bölüme geçmeden önce **kaydetmeyi** unutmayın!

## <a name="offer-listing"></a>Teklif listesi

Teklif listesi sekmesinde teklifinizin kullanılabildiği diller (ve pazarlar) görüntülenir. Şu anda Ingilizce (Birleşik Devletler) kullanılabilir tek konumdur. Ayrıca, Bu sayfa dile özgü listenin durumunu ve eklendiği tarih/saati görüntüler. Her dil/Pazar için Market ayrıntılarını (teklif adı, açıklama, arama terimleri vb.) tanımlamanız gerekir.

> [!NOTE]
> Teklif açıklaması (teklif açıklaması, belgeler, ekran görüntüleri, kullanım koşulları ve Gizlilik ilkesi gibi) sunan teklif listesi, "Bu uygulama yalnızca [Ingilizce olmayan dil] ' de kullanılabilir." Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

### <a name="offer-listings"></a>Teklif listeleri

Teklif ve pazarlama varlıklarınızın açıklamaları dahil olmak üzere Market 'te görüntülenecek ayrıntıları sağlayın.

- **Ad** (gerekli): Burada tanımlanan ad, seçtiğiniz Market (ler) de teklif listelerinizin başlığı olarak görünür. Ad, önceki **yeni teklif** girişinizin temel alınarak önceden doldurulur.  Bu, trademarked olabilir.  Bu, bir boşluk, emojıs (ticari marka ve telif hakkı sembolleri olmadıkları müddetçe) içermemelidir ve 50 karakterle sınırlı olmalıdır.
- **Özet** (gerekli): Market listeleme arama sonuçlarında kullanılacak teklifinizin kısa bir açıklamasını sağlayın. Bu alana en fazla 100 karakter metin girilebilir.
- **Açıklama** (gerekli): Market dökümüne genel bakış bölümünde görüntülenecek teklifiniz için bir açıklama sağlayın. Bir değer teklifi, önemli avantajlar, herhangi bir kategori veya sektör ilişkilendirmesi, uygulama içi satın alma fırsatları, gerekli ön kapanışlar ve daha fazla bilgi edinmek için bir bağlantı dahil etmeyi göz önünde bulundurun.
Bu alana en fazla 3.000 karakter metin girilebilir. Ek ipuçları için [harika uygulama açıklaması yazma](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description)makalesine bakın.
- **Arama anahtar sözcükleri**: Müşterilerin teklifinizi Market 'te bulmak için kullanabileceği en fazla üç arama anahtar sözcüğü girin.
- Başlarken **yönergeleri** (gerekli): Uygulamanızı nasıl yapılandıracağınızı ve potansiyel müşteriler için kullanmaya nasıl başlayabileceğini açıklayın.  Bu hızlı başlangıç, daha ayrıntılı çevrimiçi belgelere bağlantılar içerebilir. Bu alana en fazla 3.000 karakter metin girilebilir. 

#### <a name="description"></a>**Açıklama**

Bu gerekli bir alandır. Açıklamaya eklenecek öğeler: 

* Tanımınızın ilk birkaç cümlede teklifinizin değer teklifini açık bir şekilde açıklamalıdır.  
* İlk birkaç tümcenin arama motoru sonuçlarında görüntülenebileceğini aklınızda bulundurun.  
* Ürününüzü satmaya yönelik özelliklere ve işlevlere güvenmeyin. Bunun yerine, teslim ettiğiniz değere odaklanın.  
* Sektöre özgü sözlük veya avantaj temelli bir ifade mümkün olduğunca yararlanın. 

Değer önerinizdeki çekirdek bileşenleri aşağıdaki bilgileri içermelidir: 

* Ürünün açıklaması. 
* Üründen faydalanan Kullanıcı türü. 
* Müşterinin ürün adresi olması veya bir sorun olması. 

Teklif açıklamanızı daha ilgi çekici hale getirmek için, açıklamayı biçimlendirmek için HTML etiketlerini kullanabilirsiniz. 

1. Paragraf oluşturmak istiyorsanız, metnin begine ekleyin `<p>` ve sonuna ekleyin. `</p>`

    **Örnek**: 

    `<p>`İlk paragrafım. `</p>` <br>
    `<p>`Bu, ikinci paragrafım. `</p>` <br>

    Yukarıdaki şöyle görünür:

    <p> İlk paragrafım. </p>
    <p> Bu, ikinci paragrafım. </p>

1. **Madde işaretli bir öğe listesi**eklemek istiyorsanız, metninizi `<li>` aşağıdaki etiketlere yerleştirin. Ve`<ul>` `<li>` etiketlerininiçinde`</li>` daha fazla madde işaretli öğe (ve Etiketler arasındaki öğeler) kopyalayabilir ve yapıştırabilirsiniz. `</ul>` ' İ eklediğinizden emin `<ul></ul>`olun. 

    **Örnek**:

    ```
    <ul> 
        <li>add text here</li> 
        <li> add text here </li> 
        <li> add text here </li> 
    </ul> 
    ```

    Yukarıdaki şöyle görünür:
    <ul> 
        <li>Buraya metin ekleyin</li> 
        <li> Buraya metin ekleyin </li> 
        <li> Buraya metin ekleyin </li> 
    </ul> 

1. **Kalın** içerikli içeriğe göre, `<b>` kalın yapmak istediğiniz metnin başlangıcına ekleyin ve kalın yapmak istediğiniz metnin sonuna `</b>` ekleyin. 

    **Örnek**: `<b>`ÜCRETSIZ DENEME`</b>`
    
    Yukarıdaki, ÜCRETSIZ deneme kelimesinin storefront içinde teklifin açıklamasına göre kalın olmasına neden olur. 

    **ÜCRETSIZ DENEME**

1. İçeriğiniz arasına **satır sonları** eklemek için, yeni bir `<br>` satırda başlamasını istediğiniz içerikten önce ekleyin. Bir boşluk bırakmak ve içeriğin yeni bir satırda başlamasını sağlamak istiyorsanız, içerikten önce ekleyin `<br><br>` . 

    **Örnek**:

    Bu bir metin çizgisi. `<br>`Bu, yeni bir satırda başlatılacak bir metin çizgisi. `<br><br>`Bu, aşağıdaki iki satırı başlatacak bir satırdır. 

    Yukarıdaki şöyle görünür:

    Bu bir metin çizgisi. <br> Bu, yeni bir satırda başlatılacak bir metin çizgisi. <br><br> Bu, aşağıdaki iki satırı başlatacak bir satırdır. 

1. **Metnin boyutunu artırmak**istiyorsanız, önce metnin ne kadar büyük olmasını istediğinizi seçin. Aşağıdaki örnekleri kullanın. Metnin boyutunu seçtikten sonra, metnin başlangıcına ve sonuna karşılık gelen `<H*></H*>` etiketleri ekleyin. 

    **Örnek**:

    `<h1>`Bu başlık 1 ' dir`</h1>` <br>
    `<h2>`Bu başlık 2 ' dir`</h2>` <br>
    `<h3>`Bu, başlık 3 ' ün`</h3>` <br>
    `<h4>`Bu başlık 4 ' ün`</h4>` <br>
    `<h5>`Bu başlık 5 ' tir`</h5>` <br>
    `<h6>`Bu başlık 6 ' dır`</h6>` 

    Yukarıdaki şöyle görünür:

    <h1>Bu başlık 1 ' dir</h1> 
    <h2>Bu başlık 2 ' dir</h2> 
    <h3>Bu, başlık 3 ' ün</h3> 
    <h4>Bu başlık 4 ' ün</h4> 
    <h5>Bu başlık 5 ' tir</h5> 
    <h6>Bu başlık 6 ' dır</h6> 

#### <a name="links"></a>Bağlantılar

- **Gizlilik ilkesi** (gerekli): Kuruluşunuzun gizlilik ilkesine bağlayın. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.
- **CSP program pazarlama malzemeleri** (isteğe bağlı): Teklifinizi [bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) programına genişletmeyi seçerseniz, pazarlama malzemeleri için bir bağlantı sağlamanız gerekir. CSP, CSP iş ortaklarının teklifinizi paketlemelerine, pazarlamaya ve satmasına olanak tanıyarak, teklifinizi daha geniş bir yelpazede nitelikli müşterilere genişletir. Bu satıcıların, teklifinizi pazarlama malzemelerinden sorumlu olması gerekir. Daha fazla bilgi için bkz. [Go-to-market Services](https://partner.microsoft.com/reach-customers/gtm).
- **Faydalı bağlantılar** (isteğe bağlı): Uygulamanız veya ilgili hizmetlerinize ilişkin isteğe bağlı ek çevrimiçi belgeler, bir **başlık** ve **URL**sunarak listelenmiştir. **+ URL Ekle**' ye tıklayarak ek yararlı bağlantılar ekleyin.

#### <a name="contact-information"></a>İletişim bilgileri

- **Kişiler**: Her müşteri kişisi için, bir çalışan **adı** , **telefon numarası**ve **e-posta** adresi sağlayın.  (Bunlar herkese açık olarak gösterilmez). **Destek kişi** grubu Için bir **Destek URL 'si** de gereklidir.  (Bu bilgiler genel *olarak* görüntülenecektir).

**Destek kişisi** (gerekli): Genel destek sorularına yöneliktir.

**Mühendislik iletişim** (gerekli): Teknik sorular için.

**Kanal yöneticisi kişisi** (gerekli): CSP programıyla ilgili satıcı soruları için.

#### <a name="files-and-images"></a>Dosyalar ve görüntüler

- **Belgeler** (gerekli): Teklif başına en az bir (1) ve en fazla üç (3) belge sağlayan, teklifiniz için ilgili pazarlama belgelerini PDF biçiminde ekleyin.
- **Görüntüler** (isteğe bağlı): Teklifinizin logo görüntülerinin Market genelinde görünebileceği birden çok yer vardır ve aşağıdaki boyutları gerektirir--küçük: 48 x 48 piksel _(gerekli),_ Orta: 90 x 90 piksel, büyük: 216 x 216 piksel _(gerekli),_ geniş: 255 x 115 piksel ve Hero: 815 x 290 piksel. Tüm görüntülerin içinde olması gerekir. PNG biçimi.
- **Ekran görüntüleri** (gerekli): Teklifinizi gösteren ekran görüntüleri ekleyin. En fazla beş (5) ekran görüntüsü eklenebilir ve 1280 x 720 piksel boyutunda olmalıdır. Tüm görüntülerin içinde olması gerekir. PNG biçimi.
- **Videolar** (isteğe bağlı): Teklifinizi gösteren videoların bağlantılarını ekleyin. Müşteriler teklifinizle birlikte gösterilen YouTube ve/veya Vimeo videolarını içeren bağlantıları kullanabilirsiniz. Ayrıca, PNG biçiminde 1280 x 720 piksel boyutunda videonun küçük resmini de girmeniz gerekir. Her teklif için en fazla dört video görüntüleyebilirsiniz.

Sonraki bölüme geçmeden önce **kaydetmeyi** unutmayın!

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

- [Market teklif listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)


## <a name="preview"></a>Önizleme

**Önizleme** sekmesi, teklifinizi daha geniş Market hedef kitlelerine yayımlamadan önce teklifinizi serbest bırakmak için sınırlı bir **Önizleme hedef kitlesi** tanımlamanızı sağlar.

> [!IMPORTANT]
> Teklifinizi önizlemede denetledikten sonra Market ortak hedef kitlesi için canlı **çalış** ' ı seçmeniz gerekir.

- **Bir önizleme Izleyiciyi tanımlayın: İsteğe bağlı bir açıklamayla birlikte her satır için tek bir AAD/MSA hesap e-postası ekleyin.**

Canlı yayımlamadan önce teklifinizi doğrulamaya yardımcı olmak üzere mevcut Microsoft hesabı (MSA) veya Azure Active Directory hesapları için el ile on (10) e-posta adresi veya bir CSV dosyası karşıya yüklüyorsanız yirmi (20). Bu hesapları ekleyerek, Market 'te yayınlanmadan önce teklifiniz için önizlemeye erişim izni verilecek bir hedef kitle tanımlamanız gerekir. Teklifiniz zaten canlı ise, teklifiniz için herhangi bir değişikliği veya güncelleştirmeyi test etmek üzere bir önizleme hedef kitlesi de tanımlayabilirsiniz.

> [!NOTE]
> Önizleme hedef kitlesi, özel bir hedef kitleye göre farklılık gösterir. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine izin verilir. Ayrıca, bir plan oluşturmayı ve yalnızca özel bir hedef kitle için kullanılabilir hale getirebilirsiniz. **Plan listeleme** sekmesinde, **Bu özel bir plan** onay kutusu ile özel bir hedef kitle tanımlayabilirsiniz. Daha sonra, Azure kiracı kimliklerini kullanarak 20.000 adede kadar müşterinin özel bir kitlesini tanımlayabilirsiniz.

## <a name="technical-configuration"></a>Teknik yapılandırma

**Teknik yapılandırma** sekmesi, teklifinizi bağlamak için kullanılan teknik ayrıntıları (URL yolu, Web kancası, Kiracı kimliği ve uygulama kimliği) tanımlar. Bu bağlantı, teklifi edinmeyi tercih ettiklerinde, son müşteri için teklifinizi sağlamamızı sağlar. Toplanan alanların kullanımını açıklayan diyagramlar [SaaS karşılama API 'leri](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2)için belgelerde bulunabilir.

- **Giriş sayfası URL 'si** (gerekli): Müşterilerin teklifinizi Market 'ten aldıktan sonra hangi site URL 'sini tanımlayın. Bu URL, bir müşteri sayfaya yönlendirildiğini belirteç alan uç nokta olur. Bu belirteç, karşılama API 'Lerinde çözümle kullanılarak sağlama ayrıntıları için takas edilebilir. Bu ayrıntılar ve topladığınız tüm diğerleri, kaydınızı tamamlayıp satın almasını etkinleştirmek için deneyiminizin yerleşik olduğu müşteri etkileşimli Web sayfasının bir parçası olarak kullanılabilir.

- **Bağlantı Web kancası** (gerekli): Microsoft 'un müşteri adına gönderilmesi gereken tüm zaman uyumsuz olaylar için (örneğin: SaaS aboneliği geçersiz oldu), bağlantı Web kancası sağlamanız gerekiyor. Zaten bir Web kancası sisteminiz yoksa, en basit yapılandırma, kendisine gönderilen olayları dinleyen bir HTTP uç noktası mantıksal uygulamasına sahip olmak ve bunları uygun şekilde (ör. https:\//prod-1westus.Logic.Azure.com:443/Work) işleymeniz gerekir. Daha fazla bilgi için bkz. [Logic Apps 'TE HTTP uç noktaları Ile çağrı, tetikleyici veya iç içe iş akışları](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **Azure AD KIRACı kimliği** (gerekli): Azure portal içinde, iki hizmetimiz arasındaki bağlantının kimliği doğrulanmış bir iletişimin arkasında olduğunu doğrulayabilmemiz için [bir Azure Active Directory (ad) uygulaması oluşturmanız](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) gerekir. [KIRACı kimliğini](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)bulmak için Azure Active Directory gidin ve **Özellikler**' i seçin, ardından listelenen **dizin kimliği** numarasını (ör. 50c464d3-4930-494c-963c-1e951d15360e) arayın.

- **Azure AD uygulama kimliği** (gerekli): Ayrıca, [uygulama kimliğinize](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) ve bir kimlik doğrulama anahtarına ihtiyacınız vardır. Bu değerleri almak için Azure Active Directory gidin ve **uygulama kayıtları**' i seçin, ardından LISTELENEN **uygulama kimliği** numarasını (ör. 50c464d3-4930-494c-963c-1e951d15360e) arayın. Kimlik doğrulama anahtarını bulmak için **Ayarlar** ' a gidin ve **anahtarlar**' ı seçin. Bir açıklama ve süre sağlamanız gerekir ve ardından bir sayı değeri sağlanacaktır.

 Azure uygulama KIMLIĞI 'nin yayımcı KIMLIĞINIZLE ilişkilendirildiğini unutmayın, bu nedenle tüm tekliflerinizi aynı uygulama KIMLIĞININ kullanıldığından emin olun.

## <a name="plan-overview"></a>Plana genel bakış

**Plana genel bakış** sekmesi, aynı teklif dahilinde çeşitli plan seçenekleri sağlamanıza olanak sağlar. Bu planlar (bazen SKU 'Lar olarak adlandırılır) sürüm, para veya hizmet katmanları bakımından farklılık gösterebilir. Teklifinizin Market 'te satılabilir olması için en az bir plan ayarlamanız gerekir.

Oluşturulduktan sonra plan adlarınızı, kimlikleri, fiyatlandırma modellerini, kullanılabilirliği (genel veya özel), geçerli yayımlama durumunu ve kullanılabilir eylemleri görürsünüz.

**Plana genel bakış** bakımından sunulan **Eylemler** planınızın geçerli durumuna bağlı olarak farklılık gösterir ve şunlar olabilir:

- Plan durumu **taslak** ise, taslağı Sil
- Plan durumu **canlı** ise, planı satmayı veya özel izleyiciyi Eşitlemeyi Durdur

**Yeni plan oluştur** (Microsoft üzerinden satış için seçim yapan kişiler için en az bir plan

- **Plan KIMLIĞI:** Bu teklifteki her plan için benzersiz bir plan KIMLIĞI oluşturun. Bu KIMLIK, ürün URL 'sindeki ve Azure Resource Manager şablonlarındaki (varsa) müşterilere görünür olacaktır. Yalnızca küçük harf, alfasayısal karakter, çizgi veya alt çizgi kullanın. Bu plan KIMLIĞI için en fazla 50 karakter kullanılabilir. Oluştur seçildikten sonra KIMLIğIN değiştirilemeyeceğini unutmayın.
- **Plan adı:** Tekliflerinizi hangi plana seçeceğinize karar verirken, müşteriler bu adı görür. Bu teklifteki her plan için benzersiz bir teklif adı oluşturun. Plan adı, aynı teklifin bir parçası olabilecek yazılım planlarını ayırt etmek için kullanılır (ör. Teklif adı: Windows Server; Planlama Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Planı listeleme

**Plan listeleme** sekmesinde, planınızın kullanılabildiği diller (ve pazarlar) görüntülenir. Şu anda ingilizce (Birleşik Devletler) kullanılabilir tek konumdur. Ayrıca, Bu sayfa dile özgü listenin durumunu ve eklendiği tarih/saati görüntüler. Her dil/Pazar için Market ayrıntılarını (teklif adı, açıklama, arama terimleri vb.) tanımlamanız gerekir.

#### <a name="plan-listing-details"></a>Plan listeleme ayrıntıları

Plan dillerinden birini seçmek, **ad** ve açıklama dahil olmak üzere **Plan listesi** bilgilerini görüntüler **.**

- **Ad**: Önizleme **Yeni plan** girdinizi temel alarak önceden doldurulduktan sonra, Market 'te görüntülenen teklifinizin "yazılım planının" başlığı olarak görünür.
- **Açıklama:** Bu açıklama, bu yazılım planının ne olduğunu ve teklifinizdeki diğer yazılım planlarından herhangi bir farklılık yaptığını açıklayan bir fırsattır. En fazla 500 karakter içerebilir.

Bu alanları tamamladıktan sonra **Kaydet** ' i seçin.

#### <a name="plan-pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik planlaması

**Fiyatlandırma ve kullanılabilirlik** sekmesi, bu planın kullanılabileceği pazarları, istenen ödeme modeli, Fiyat ve fatura terimini yapılandırmanıza olanak sağlar. Ayrıca, planın herkes için mi yoksa yalnızca belirli müşterilere mi (özel bir hedef kitle) görünür mı olacağını belirtebilirsiniz.

##### <a name="enabling-free-trials"></a>Ücretsiz denemeleri etkinleştirme

Ticari Market aracılığıyla sunulan SaaS teklifleri, Microsoft üzerinden satılırken bir aylık ücretsiz deneme olanağı sağlamanıza olanak tanır. Ölçümlü planlar hariç tüm faturalama modelleri ve koşulları için ücretsiz denemeler desteklenir. Bu seçenek, müşterilerin bir ay boyunca ücretsiz erişim için düşük bir engel olmasına olanak sağlar.  Teklifiniz dahilinde planlar için ücretsiz denemeyi etkinleştirmeyi seçerseniz, müşteri, ilk bir ay döneminin sonundan önce ücretli bir aboneliğe dönüştürülemez.  Bu süre boyunca, teklifinizi satın alan müşteriler ücretsiz deneme sürümü etkin olan ve aralarında dönüştürme desteklenen planların herhangi birini deneyebilir.  Ücretli aboneliğe dönüştürme işlemi, dönem sonunda otomatik olarak yapılır.

>[!Note]
>Müşteri, ücretsiz denemeler olmadan bir plana dönüştürmeyi seçerse, dönüştürme gerçekleşecektir, ancak ücretsiz deneme hemen kaybolacaktır.  Ayrıca, bir müşteri bir plan için ödeme başladıktan sonra, ücretsiz denemeleri destekleyen bir SKU 'ya dönüştürüseler bile, aynı abonelikte artık ücretsiz deneme alamaz.

Ücretsiz deneme yapılandırma özelliği, teklifinizin her bir planı için kullanılabilir. Her teklifin fiyatlandırmasına ve kullanılabilirliğine gitmeniz yeterlidir ve bir aylık denemeye izin vermek için kutuyu işaretleyin.

![Bir aylık ücretsiz deneme onay kutusu](./media/free-trial-enable.png)

Ücretsiz bir denemeye katılmış olan müşteri abonelikleri hakkında bilgi edinmek için, doğru veya yanlış olarak işaretlenecek yeni `isFreeTrial`API özelliğini kullanın. Daha fazla bilgi için [SaaS Get abonelik API 'sine](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2#get-subscription) bakın.

>[!Note]
>Market ölçüm hizmetinden yararlanan planlar için ücretsiz denemeler desteklenmez.

#### <a name="markets"></a>Pazar

- **Pazarları Düzenle** seçim

Her plan en az bir pazarda kullanılabilir olmalıdır. Bu planı kullanılabilir yapmak istediğiniz tüm pazar konumları için onay kutusunu seçin. Microsoft remits Sales ve kullanım vergisinin sizin adınıza yardımına dahil olduğu "vergi havalesi" ülkelerinden oluşan bir arama kutusu ve düğmesi. 


Planınız için Birleşik Devletler dolar (USD) cinsinden zaten fiyatlar ayarladıysanız ve başka bir pazar konumu eklerseniz, yeni pazar fiyatı geçerli döviz kurlarına göre hesaplanır. Yayımlamadan önce her bir pazar için ücreti her zaman gözden geçirmelisiniz. Fiyatlandırma, değişiklikleriniz kaydedildikten sonra "fiyatları dışarı aktar (xlsx)" bağlantısı kullanılarak incelenebilir.

#### <a name="pricing"></a>Fiyatlandırma

- **Fiyatlandırma modeli**: Düz hız veya koltuk tabanlı

**Düz hız:** Tek bir aylık veya yıllık fiyat sabit fiyat fiyatı ile teklifinizin erişimini etkinleştirin. Bu, bazen site tabanlı fiyatlandırma olarak adlandırılır. Bu fiyatlandırma modeliyle isteğe bağlı olarak, müşterileri standart olmayan birimlere göre ücretlendirmesi için Market ölçüm hizmeti API 'sini kullanan tarifeli planlar tanımlayabilirsiniz.  Tarifeli faturalandırma hakkında daha fazla bilgi için bkz. [Market ölçüm hizmeti kullanılarak ölçülen faturalandırma](./saas-metered-billing.md).

**Kullanıcı başına:** Teklifle veya şirket dışı bilgisayarlara erişen kullanıcı sayısına göre fiyat ile teklifinizin erişimini etkinleştirin. Bu Kullanıcı tabanlı model, fiyata göre izin verilen en düşük ve en yüksek Kullanıcı sayısını ayarlamanıza olanak sağlar. Bu şekilde, farklı fiyat noktaları, birden çok plan yapılandırılarak kullanıcı sayısına göre yapılandırılabilir.  Bu alanlar isteğe bağlıdır. Sol tarafta yoksa, Kullanıcı sayısı bir sınıra sahip değildir (en az 1, en fazla sistem tarafından desteklenebilir). Bu alanlar, planınızdaki bir güncelleştirmenin parçası olarak düzenlenebilir.

Yayımlandıktan sonra faturalandırma fiyatlandırma modeli seçimi değiştirilemez. Ayrıca, aynı teklif için tüm planlar aynı fiyatlandırma modelini paylaşmalıdır.

- **Fatura dönemi**: Aylık veya yıllık

Müşterilerin listelenen fiyatı ödemesi gereken sıklığı seçin. En az bir aylık veya yıllık fiyat belirtilmelidir ya da müşteriler için her iki seçenek de kullanılabilir hale getirilebilir.

- **Fiyat**: Ayda ABD Doları veya yıl başına ABD Doları

Yerel para birimi cinsinden ayarlanan fiyatlar (USD = Birleşik Devletler dolar), kurulum sırasında kullanılabilir olan geçerli döviz kurlarını kullanarak tüm seçili piyasaların yerel para birimine dönüştürülür. Fiyatlandırma elektronik tablosunu dışarı aktarıp her bir pazardaki fiyatı inceleyerek yayımlamadan önce bu fiyatları doğrulayın. Tek bir pazarda özel fiyatlar ayarlamak isterseniz, fiyatlandırma elektronik tablosunu değiştirin ve içeri aktarın. Bu fiyatlandırmayı doğrulamak ve bu ayarların sahibi olmak sizin sorumluluğunuzdadır.
**Fiyatlandırma verilerinin dışarı aktarılmasını sağlamak için öncelikle fiyatlandırma değişikliklerinizi kaydetmeniz gerekir.*

Bir plan yayımlandıktan sonra nelerin değiştirebilecekleri hakkında bazı kısıtlamalar olduğundan, yayımlamadan önce fiyatlarınızı dikkatlice gözden geçirin:

- Bir plan yayımlandığında fiyatlandırma modeli değiştirilemez.
- Bir plan için faturalandırma dönemi yayımlandıktan sonra, daha sonra kaldırılamaz.
- Planınızdaki bir pazar bedeli yayımlandığında, daha sonra değiştirilemez.

### <a name="plan-audience"></a>Hedef kitle planlayın

Her planı herkese görünür olarak veya yalnızca seçtiğiniz belirli bir kitlele yapılandırma seçeneğiniz vardır. Azure AD kiracı kimliklerini kullanarak bu kısıtlanmış hedef kitleye üyelik atayabilirsiniz.

#### <a name="privacy"></a>Gizlilik

- **Bu özel bir plandır** (İsteğe bağlı onay kutusu)

Planınızı özel ve yalnızca seçtiğiniz kısıtlanmış kitlele görünür hale getirmek için bu kutuyu işaretleyin. Özel bir plan olarak yayımlandıktan sonra izleyiciyi güncelleştirebilir veya planı herkes için kullanılabilir hale getirebilirsiniz. Bir plan herkese görünür olarak yayımlandıktan sonra herkese görünür kalması gerekir. (Plan bir özel plan olarak yeniden yapılandırılamaz).

- **Kısıtlanmış hedef kitle (kiracı kimlikleri)**

Bu özel plana erişimi olacak hedef kitleyi atayın. Atanan her kiracı KIMLIĞININ açıklamasını ekleme seçeneğiyle kiracı kimlikleri kullanılarak erişim atanır. Bir. csv elektronik tablo dosyası içeri aktarıldıysanız, en fazla 10 Kiracı kimliği eklenebilir veya 20.000 müşteri Kiracı kimliği olabilir.

Kiracı, bir GUID (genel benzersiz tanımlayıcı, kaynakları tanımlamak için kullanılan 128 bitlik bir tamsayı numarası) olarak temsil edilen KIMLIĞE sahip bir kuruluşun gösterimidir. Kuruluş veya uygulama geliştirici Microsoft'la bir ilişki oluşturduğunda, örneğin Azure'a, Microsoft Intune'a veya Microsoft 365'e kaydolduğunda kuruluşun veya uygulama geliştiricinin aldığı özel bir Azure AD örneğidir. Her Azure AD kiracısı benzersizdir ve diğer Azure AD kiracılarından ayrıdır. Kiracıyı denetlemek için, uygulamanızı yönetmek üzere kullanmak istediğiniz hesapla Azure portal oturum açın. Bir kiracınız varsa, otomatik olarak bu kiracıda oturum açar ve kiracı adını doğrudan hesap adınızın altında görebilirsiniz. Adınızı, e-postanızı, dizininizi/kiracı kimliğinizi (GUID) ve etki alanınızı görmek için, Azure portalının sağ üst kısmında bulunan hesap adınızın üzerine gelin. Hesabınız birden çok kiracıyla ilişkiliyse, kiracılar arasında geçiş yapabileceğiniz bir menüyü açmak için hesap adınızı seçebilirsiniz. Her kiracının kendi kiracı kimliği vardır. Ayrıca, şu adreste bir etki alanı adı URL 'SI kullanarak kuruluşunuzun kiracı KIMLIĞINI de arayabilirsiniz: [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

SaaS, özel bir hedef kitle tanımlamak için kiracı kimlikleri kullanır, diğer teklif türleri de Azure abonelik kimliklerini (GUID 'Ler olarak da temsil edilir) kullanabilir.

> [!NOTE]
> Özel hedef kitle (veya kısıtlı kitleci), önizleme kitlemesinden farklıdır. **[Önizleme](#preview)** sekmesinde bir önizleme hedef kitlesi tanımlayabilirsiniz. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan *önce* teklifinizin erişimine izin verilir. Özel hedef kitle ataması yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi tüm planları (özel veya değil) görüntüleyebilir, ancak yalnızca plan test edilirken ve doğrulandığında sınırlı önizleme döneminde bu süre boyunca yalnızca sınırlı önizleme dönemi boyunca görüntülenebilir.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Market teklifi içindeki planların örnek listesi

![Notlardaki örnek Market planı listesi](./media/marketplace-plan.svg)

## <a name="test-drive"></a>Test sürüşü

**Test sürücüsü** sekmesi, müşterilerin satın alma işlemine geçmeden önce teklifinizi denemesini sağlayacak bir tanıtım (veya "test sürücüsü") ayarlamanıza olanak sağlar. [Test sürücüsü nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)makalesinde daha fazla bilgi edinin. Teklifiniz için artık bir test sürücüsü sağlamak istemiyorsanız **[teklif kurulumu](#offer-setup)** sayfasına dönün ve **Test sürücüsünü etkinleştir**seçeneğinin işaretini kaldırın.

### <a name="technical-configuration"></a>Teknik yapılandırma
Aşağıdaki test sürücüsü türleri, her biri kendi teknik yapılandırma gereksinimlerine sahip kullanılabilir.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Mantıksal uygulama](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (Teknik yapılandırma gerekli değildir)

#### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Azure Resource Manager test sürücüsü için teknik yapılandırma

Çözümünüzü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonu. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır. [Azure Resource Manager test sürücüsü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)ayarlama hakkında daha fazla bilgi edinin.

- **Bölgeler** (gerekli): Şu anda, test sürücünüzün kullanılabilir hale getirilbileceği 26 Azure destekli bölge vardır. Genellikle, en iyi performans için en yakın bölgeyi seçebilmeniz için, en fazla sayıda müşteriyi tahmin ettiğiniz bölgelerde test sürücünüzü kullanılabilir hale getirmek isteyeceksiniz. Aboneliğinizin seçtiğiniz her bölgede gereken tüm kaynakları dağıtmasına izin verildiğinden emin olmanız gerekir.

- **Örnekler**: Teklifinizin kullanılabildiği bölge sayısıyla çarpılacak olan türü (sık veya soğuk) ve kullanılabilir örnek sayısını seçin.

**Sık**erişimli: Bu tür bir örnek dağıtılır ve seçilen bölgeye göre erişim bekleniyor. Müşteriler, bir dağıtımı beklemek yerine bir test sürücüsünün *sık* erişimli örneklerine anında erişebilir. Artırabilen maliyeti daha büyük bir çalışma süresi tabi şekilde, bu örneklerin her zaman Azure aboneliğinize göre çalıştığını ' dir. En az bir *sıcak* örnek olması önemle önerilir, çünkü çoğu müşteri tam dağıtımları beklemek Istemelidir ve *etkin* örnek yoksa, müşteri kullanımındaki bir bırakmaya neden olur.

**Soğuk**: Bu örnek türü, bölge başına büyük olasılıkla dağıtılabilecek örneklerin toplam sayısını temsil eder. Soğuk örnekler, bir müşteri test sürücüsünü istediğinde tüm test sürücüsü Kaynak Yöneticisi şablonun dağıtılmasını gerektirir, böylece *soğuk* örnekler, *etkin* örneklere göre yüklenmeye çok daha yavaştır. Zorunluluğunu getirir, yalnızca test sürücüsünün süresi için ödeme yapmanız gereken bir deyişle, Azure aboneliğinizde her zaman bir *sıcak* örnekle birlikte çalışmıyor.

- **Test sürücüsü Azure Resource Manager şablonu**: Azure Resource Manager şablonunuzu içeren. zip ' i yükleyin.  Hızlı başlangıç makalesinde Azure Resource Manager şablonu oluşturma hakkında daha fazla bilgi edinin [Azure Portal kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

- **Sınama sürücüsü süresi** (gerekli): Test sürücüsünün etkin kalacağı sürenin uzunluğunu saat cinsinden girin. Bu süre sona erdikten sonra Test Sürüşü otomatik olarak sona erer. Bu süre yalnızca birkaç saat kadar bir sonuç alabilir (ör. "2" saat, "1,5" geçerli değildir).

#### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Dynamics 365 test sürücüsü için teknik yapılandırma

Microsoft, bu test sürücüsü türünü kullanarak hizmet sağlamayı ve dağıtımını barındırarak ve tutarak bir test sürücüsü ayarlamanın karmaşıklığını ortadan kaldırabilir. Bu barındırılan test sürücüsü türünün yapılandırması, test sürücüsünün bir Iş Merkezi, müşteri katılımı veya Işlem kitlesini hedeflediğinden bağımsız olarak aynıdır.

- **Maksimum eşzamanlı test sürücüleri** (gerekli): Test sürücünüzü tek seferde kullanılabilecek maksimum müşteri sayısını ayarlayın. Her eşzamanlı kullanıcı, test sürücüsü etkinken bir Dynamics 365 lisansını kullanır, bu nedenle maksimum sınır kümesini destekleyecek yeterli lisansa sahip olduğunuzdan emin olmanız gerekir. Önerilen 3-5 değeri.

- **Sınama sürücüsü süresi** (gerekli): Saat sayısını tanımlayarak, test sürücüsünün etkin kalacağı sürenin uzunluğunu girin. Bu süre dolduktan sonra, oturum sona erdir ve artık lisanslarınızdan birini tüketmez. Teklifinizin karmaşıklığına bağlı olarak 2-24 saat arasında bir değer öneririz. Bu süre yalnızca birkaç saat kadar bir sonuç alabilir (ör. "2" saat, "1,5" geçerli değildir).  Kullanıcı zaman aşımına uğrar ve test sürücüsüne yeniden erişmek istediğinizde yeni bir oturum isteyebilir.

- **Örnek URL 'si** (gerekli): Müşterinin test sürücüsüne başlayacağı URL. Genellikle uygulamanızı örnek verilerle çalıştıran Dynamics 365 örneğinizin URL 'SI (ör https://testdrive.crm.dynamics.com).

- **Örnek Web API URL 'si** (gerekli): Microsoft 365 hesabınızda oturum açarak ve **Ayarlar** \&gt; ' ye giderek Dynamics 365 örneğiniz için Web API URL 'sini alın. **Özelleştirme** \&gt; **Geliştirici kaynakları** \&gt; **Örnek Web API 'si (hizmet kök URL 'si)** , burada bulunan URL 'yi kopyalayın ( https://testdrive.crm.dynamics.com/api/data/v9.0) ör.

- **Rol adı** (gerekli): Özel Dynamics 365 test sürücünüzde tanımladığınız güvenlik rolü adını sağlayın. Bu, kullanıcıya test sürücüleri sırasında atanır (örn. Test-Drive-Role).

#### <a name="technical-configuration-for-logic-app-test-drive"></a>Mantıksal uygulama sınama sürücüsü için teknik yapılandırma

Herhangi bir özel ürün, çeşitli karmaşık çözüm mimarilerini kapsayan bu tür test sürücüsü Dağıtım şablonunu kullanmalıdır. Mantıksal uygulama test sürücüleri ayarlama hakkında daha fazla bilgi için bkz. GitHub üzerinde [işlemler](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) ve [müşteri katılımı](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) .

- **Bölge** (gerekli, tek seçimli açılan liste): Şu anda, test sürücünüzün kullanılabilir hale getirilbileceği 26 Azure destekli bölge vardır. Mantıksal uygulamanızın kaynakları seçtiğiniz bölgede dağıtılır. Mantıksal uygulamanızda belirli bir bölgede depolanan özel kaynaklar varsa bölgenin burada seçildiğinden emin olun. Bunu yapmanın en iyi yolu, mantıksal uygulamanızı portalda Azure aboneliğinizde yerel olarak dağıtmaktır ve bu seçimi yapmadan önce doğru çalıştığını doğrular.

- **Maksimum eşzamanlı test sürücüleri** (gerekli): Test sürücünüzü tek seferde kullanılabilecek maksimum müşteri sayısını ayarlayın. Bu test sürücüleri zaten dağıtılır ve müşterilerin bir dağıtımı beklemeden bunlara anında erişmelerini sağlar.

- **Sınama sürücüsü süresi** (gerekli): Test sürücüsünün etkin kalacağı sürenin uzunluğunu saat cinsinden girin. Bu süre dolduktan sonra test sürücüsü otomatik olarak sona erer.

- **Azure Kaynak grubu adı** (gerekli): Mantıksal uygulama test sürücünüzün kaydedildiği [Azure Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) adını girin.

- **Azure mantıksal uygulama adı** (gerekli): Kullanıcıya test sürücüsünü atayan mantıksal uygulamanın adını girin. Bu mantıksal uygulama, yukarıdaki Azure kaynakları grubuna kaydedilmelidir.

- **Mantıksal uygulama adının sağlamasını kaldırma** (gerekli): Müşteri bittikten sonra test sürücüsünü sağlayan mantıksal uygulamanın adını girin. Bu mantıksal uygulama, yukarıdaki Azure kaynakları grubuna kaydedilmelidir.

#### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Power BI test sürücüleri için teknik yapılandırma gerekli değildir

Etkileşimli bir Power BI görseli göstermek isteyen ürünler, özel olarak oluşturulmuş bir panoyu test sürücüleri olarak paylaşmak için, daha fazla teknik yapılandırma gerekmeden gömülü bir bağlantı kullanabilir. [Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview) şablonu uygulamalarını ayarlama hakkında daha fazla bilgi edinin.

### <a name="deployment-subscription-details"></a>Dağıtım aboneliği ayrıntıları

Test sürücüsünü sizin adınıza dağıtmak için lütfen benzersiz ve benzersiz bir Azure aboneliği oluşturun ve sağlayın. (Power BI test sürücüleri için gerekli değildir).

- **Azure ABONELIK kimliği** (Azure Resource Manager ve Logic Apps için gereklidir): Kaynak kullanımı raporlama ve faturalama için Azure hesap hizmetlerinizi erişim izni vermek üzere abonelik KIMLIĞINI girin. Henüz bir tane yoksa, test sürücüleri için kullanmak üzere [ayrı bir Azure aboneliği oluşturmayı](https://docs.microsoft.com/azure/billing/billing-create-subscription) düşünmeniz önerilir. [Azure Portal](https://portal.azure.com/) oturum açarak ve sol taraftaki menüdeki **abonelikler** SEKMESINE giderek Azure abonelik kimliğinizi bulabilirsiniz. Sekmeyi seçtiğinizde, abonelik KIMLIĞINIZ görüntülenir (örneğin, "a83645ac-1234-5AB6-345-1h234g764ghty").

- **Azure AD KIRACı kimliği** (gerekli): Azure Active Directory (AD) [KIRACı kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol taraftaki menüden Active Directory sekmesini seçin, **Özellikler** ' i seçin, sonra listelenen **dizin kimliği** numarasını (ör. 50c464d3-4930-494c-963c-1e951d15360e) arayın. Ayrıca, şu adreste bulunan etki alanı adı URL 'nizi kullanarak kuruluşunuzun kiracı KIMLIĞINI de arayabilirsiniz: [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

- **Azure AD kiracı adı** (dinamik 365 için gereklidir): Azure Active Directory (AD) adınızı girin. Bu adı bulmak için sağ üst köşedeki [Azure Portal](https://portal.azure.com/)oturum açın, kiracı adınız hesap adınızın altında listelenecektir.

- **Azure AD uygulama kimliği** (gerekli): Azure Active Directory (AD) [uygulama kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol menüden Active Directory sekmesini seçin, **uygulama kayıtları**' yı seçin, ardından listelenen **uygulama kimliği** numarasını (ör. 50c464d3-4930-494c-963c-1e951d15360e) arayın.

- **Azure AD uygulama istemci parolası** (gerekli): Azure AD uygulama [istemci gizli](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)anahtarını girin. Bu değeri bulmak için [Azure Portal](https://portal.azure.com/)oturum açın. Sol menüdeki **Azure Active Directory** sekmesini seçin, **uygulama kayıtları**' i seçin ve ardından test sürücüsü uygulamanızı seçin. Sonra **Sertifikalar ve gizlilikler**' ı seçin, **yeni istemci parolası**' nı seçin, bir açıklama girin, **süresi dolmasın**' ı seçin ve ardından **Ekle**' yi Değeri kopyalamadığınızdan emin olun. (Bunu yapmadan önce sayfadan uzaklaşmayın, aksi takdirde değere erişemezsiniz.)

Sonraki bölüme geçmeden önce **kaydetmeyi** unutmayın!

### <a name="test-drive-listings-optional"></a>Sürücü listelerini test etme (isteğe bağlı)

Test sürücüsü sekmesinde bulunan **Test sürücü listeleri** seçeneği , test sürücünüzün kullanılabildiği dilleri (ve pazarlar) görüntüler; şu anda İngilizce (Birleşik Devletler) kullanılabilir tek konumdur. Ayrıca, Bu sayfa dile özgü listenin durumunu ve eklendiği tarih/saati görüntüler. Her dil/Pazar için test sürücüsü ayrıntılarını (Açıklama, Kullanıcı el ile, videolar vb.) tanımlamanız gerekir.

- **Açıklama** (gerekli): Kullanıcı teklifinizi elde edip etmeyeceğinizi belirlemesine yardımcı olmak için test sürücünüzü, neyi gösterdiklerinizi, kullanıcının deneme amaçlarını, araştırılacak özellikleri ve ilgili bilgileri tanıtın. Bu alana en fazla 3.000 karakter metin girilebilir. 

- **Erişim bilgileri** (Azure Resource Manager ve mantıksal test sürücüleri için gereklidir): Bu test sürücüsüne erişmek ve bunları kullanmak için müşterinin neleri bilmesi gerektiğini açıklayın. Teklifinizi kullanmaya yönelik bir senaryoya ve müşterinin test sürüşü genelinde özelliklere erişmek için bilmeleri gereken tam olarak ne olduğunu öğrenin. Bu alana en fazla 10.000 karakter metin girilebilir.

- **Kullanıcı el ile** (gerekli): Test sürücüsü deneyiminize ilişkin ayrıntılı bir anlatım. Kullanıcı el kitabı, müşterinin test sürücüsüyle karşılaşmasını istediğiniz şekilde tam olarak kapsamalıdır ve sahip olabileceği sorulara başvuru olarak görev yapar. Dosya, karşıya yüklendikten sonra PDF biçiminde ve adlandırılmalıdır (en fazla 255 karakter) olmalıdır.

- **Larınız Video** ekleme (isteğe bağlı): Müşteriler, bir bağlantı ve küçük resim görüntüsü (533 x 324 piksel) ile birlikte bulunabilir ve bu sayede bir müşterinin, test sürücüsünü daha iyi anlamasına yardımcı olmak için bilgileri bir adım adım görüntüleyebilmesini sağlayacak şekilde sağladığı avantajları vurgulayan senaryolar sunun ve anlayın.
  - **Ad** istenir
  - **URL (yalnızca YouTube veya Vimeo)** istenir
  - **Küçük resim (533 x 324px)** : Görüntü dosyası PNG biçiminde olmalıdır.

Bu alanları tamamladıktan sonra **Kaydet** ' i seçin.

## <a name="publish"></a>Yayımlama

#### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Teklifin tüm gerekli bölümlerini tamamladıktan sonra portalın sağ üst köşesinde **Yayımla** ' yı seçin. **İnceleme ve yayımlama** sayfasına yeniden yönlendirilirsiniz. 

Bu teklifi ilk kez yayınlıyorsanız, şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - *Başlatılmamış* – bölüm dokunulmamış ve tamamlanması gereken anlamına gelir.
    - *Tamamlanmamış* – bölümde düzeltilmesi gereken hatalar olduğu veya daha fazla bilgi sağlanması gerekir. Lütfen bölüm (ler) e geri dönün ve güncelleştirin.
    - *Tamamlandı* – bölümün tamamlandığı, tüm gerekli verilerin sağlandığı ve hata olmadığı anlamına gelir. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- Uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Size, gözden geçirmeniz ve onaylamanız için teklifin bir önizleme sürümünün ne zaman kullanılabileceğini bilmenizi sağlayacak bir e-posta göndereceğiz. Teklifinizi ortak (veya özel bir teklif, özel hedef kitleye) yayımlamak için Iş Ortağı Merkezi 'ne dönmeniz ve teklif için **Go-Live** ' ı seçmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market 'te mevcut bir teklifi güncelleştirme](./update-existing-offer.md)
