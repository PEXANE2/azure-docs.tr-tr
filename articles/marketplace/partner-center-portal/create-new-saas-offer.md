---
title: Microsoft Commercial Market için yeni bir SaaS teklifi oluşturun
description: Microsoft AppSource, Azure Marketi 'nde veya Microsoft Iş Ortağı Merkezi 'ndeki Microsoft ticari Market programını kullanarak bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listeleme veya satma için yeni bir hizmet olarak yazılım (SaaS) teklifi oluşturma.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 9ffa10726c1ca838f00b713e6640f609c122975a
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735105"
---
# <a name="create-a-new-saas-offer-in-the-commercial-marketplace"></a>Ticari Market 'te yeni bir SaaS teklifi oluşturun

Ticari Market 'te hizmet olarak yazılım (SaaS) teklifi oluşturmaya başlamak için, önce [bir Iş Ortağı Merkezi hesabı](./create-account.md) oluşturduğunuzdan ve **genel bakış** sekmesi seçili olarak [ticari Market panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers)açın.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market** > **genel bakış**' ı seçin.
3. Genel Bakış sayfasında, hizmet olarak **yeni teklif** > **yazılımı**' nı seçin.

   ![Sol gezinti menüsünü gösterir.](./media/new-offer-saas.png)

> [!NOTE]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca stoporonts 'de görüntülenir. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, burada **Test-teklif-1** girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
- Teklif diğer adı, **Oluştur**' u seçtikten sonra değiştirilemez.

<!---
![Offer overview on Partner Center](./media/commercial-marketplace-offer-overview.png)
-->

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-overview"></a>Teklifin genel bakış

**Yayımlama durumu** , bu teklifi yayımlamak için gereken adımların görsel bir gösterimini ve her adımın tamamlanması için ne kadar sürdüğünü gösterir. Tamamlanmamış yayımlama adımı simgeleri gri olacak.

**Teklif genel bakış** menüsü, bu teklif üzerinde işlem gerçekleştirmek için bağlantıların bir listesini içerir. Bu işlem listesi, teklifiniz için yaptığınız seçime göre değişecektir.  

- Teklif bir taslak silise taslak silme
- Teklif canlı değilse, satış teklifini durdur
- Teklif önizleme sürümündedir-canlı
- Yayımcı oturumunu kapatma Işlemini tamamlamadıysanız, yayımlamayı Iptal edin

## <a name="offer-setup"></a>Teklif kurulumu

Bu sayfa aşağıdaki bilgileri ister.

- **Microsoft üzerinden satımek istiyor musunuz?** (Evet/Hayır)
    - **Evet**, Microsoft 'tan satış yapmak ve Microsoft ana bilgisayar işlemlerini benim yerime almak istiyorum
    - **Hayır**, yalnızca Pazar yerleri aracılığıyla teklifimi listelemek ve işlemleri bağımsız olarak işlemek tercih ediyorum.

### <a name="sell-through-microsoft"></a>Microsoft ile satış

Microsoft ile satış yapmak daha iyi müşteri bulma ve alma olanağı sağlar, Microsoft 'un Market işlemlerini sizin adınıza barındırmasına olanak tanır ve Microsoft 'un küresel olarak kullanılabilir ticari özellikleri avantajlarından yararlanır.

#### <a name="saas-offer-requirements"></a>SaaS teklif gereksinimleri

Iş Ortağı Merkezi 'nde ticari Market ile hizmet olarak yazılım (SaaS) teklifleri listelemek için aşağıdaki kriterlerin karşılanması gerekir:

- Teklifinizin kimlik yönetimi ve kimlik doğrulaması için [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) kullanması gerekir.
- Teklifinizin Azure Marketi ile tümleştirilecek [SaaS karşılama API 'lerini](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) kullanması gerekir.
- Daha kapsamlı gereksinimler için [SaaS teklifi Yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide)' na bakın.

#### <a name="saas-pricing-and-billing-options"></a>SaaS fiyatlandırma ve faturalandırma seçenekleri

Yayımcının Azure aboneliğinde çalışan SaaS çözümleri sayesinde, müşteriler tarafından ödenen lisans ücretleri, yazılımın dağıtıldığı altyapının maliyetini içerir. Azure altyapı kullanımı, iş ortağı tarafından doğrudan yönetilir ve size faturalandırılır. Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez. Yayımcılar, Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmasına paketlemelidir. 

SaaS, aylık veya yıllık faturalandırmaya, Kullanıcı başına veya ölçülen faturalandırma hizmeti kullanılarak sunulan tüketim ücretlerine göre destek sunuyor. Microsoft 'un ticari Market 'i, yayımcıların fiyatları, Microsoft ürün müşterileri ve Microsoft 'un bir kurum ücretini stopajına göre gelir ödediği bir kurum modeli üzerinde çalışır.

Bu, kurumlar modelini göstermek için maliyetleri ve ödemeler için örnek bir dökümdir:

|**Lisans maliyetiniz**|**$100/ay**|
|:---|:---|
|Azure kullanım maliyeti (D1/1-çekirdek)|Müşteriye değil doğrudan yayımcıya faturalandırılır|
|Müşteri Microsoft tarafından faturalandırılır|aylık $100,00 (yayımcı, lisans ücretindeki herhangi bir tahakkuk eden veya geçiş altyapı maliyeti için hesap olmalıdır)|

|**Microsoft faturaları**|**$100/ay**|
|:---|:---|
|Microsoft, lisans maliyetinizi %80 oranında ödetir <br>**Microsoft, tam SaaS uygulamaları için lisans maliyetlerinizin %90 ' ını öder*|$80,00/ay <br>*$* 90,00/ay *|

- Bu örnekte, Microsoft, yazılım lisansınızın $100,00 ' i müşteriye faturalandırır ve Yayımcı için $80,00 ' i ödeder.
- **Azaltılmış Market hizmet ücreti** için uygun olan iş ortakları, 2019 2020 Haziran 'a kadar Mayıs 'a kadar olan SaaS teklifleriyle daha düşük bir işlem ücreti görür. Bu senaryoda, Microsoft, yazılım lisansınızın $100,00 ' i faturalandırır ve Yayımcı için $90,00 ' i ödeder.

> [!NOTE]
> **Daha az Market servis ücreti** – Microsoft 'un ticari Market 'te yayımladığı belirli SaaS teklifleri Için, Microsoft 'un Market hizmet ücretini %20 ' den (Microsoft Publisher anlaşmasında açıklandığı gibi) %10 ' a azaltacak. Teklifinizin uygun olmasını sağlamak için, tekliflerinizin en az birinin, Microsoft tarafından IP ortak satışı veya IP ortak satışı önceliği atanmış olarak belirlenmiş olması gerekir. Bu ay için daha düşük olan bu Market servis ücretini almak amacıyla her bir takvim ayının sonundan önce uygunluk en az beş (5) iş gününe ulaşılmalıdır.  Düşük Market hizmet ücreti, sanal makineler, yönetilen uygulamalar veya ticari Market 'ten sunulan diğer ürünlerle ilgili değildir. Daha az Market hizmet ücreti yalnızca Microsoft tarafından 1 Mayıs 2019 ile 30 Haziran 2020 arasında toplanan lisans ücretleri için uygun tekliflerdir. Bu süreden sonra Market hizmeti ücreti normal miktarına geri döner.

### <a name="list-through-microsoft"></a>Microsoft ile Listele

Market listesi oluşturarak işletmenizi Microsoft ile yükseltin. Microsoft 'un doğrudan yazılım lisans işlemlerine katılmayacağı, teklifinizi yalnızca bir şekilde listelemek için seçin. İlişkili bir işlem ücreti yoktur ve Yayımcı, müşteriden toplanan tüm yazılım lisanslama ücretlerine %100 oranında devam eder. Ancak yayımcı, yazılım lisans işleminin, bunlarla sınırlı olmamak üzere tüm yönlerini desteklemekten sorumludur: sipariş karşılama, ölçüm, faturalandırma, faturalama, ödeme ve koleksiyon.

<!-- - **How do you want potential customers to interact with this listing offer?** -->

#### <a name="get-it-now-free"></a>Şimdi alın (ücretsiz)

[Azure Active Directory (Azure AD) kullanarak tek tıklamayla kimlik doğrulama](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials)aracılığıyla deneme süresi sunabileceği geçerli bir adres ( *http* veya *https*ile başlayarak) sunarak, müşterilerinizin teklifinizi ücretsiz olarak listeleyin. Örneğin, `https://contoso.com/saas-app`.

#### <a name="free-trial-listing"></a>Ücretsiz deneme (listeleme)

[Azure Active Directory (Azure AD) kullanarak tek tıklamayla kimlik doğrulama](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials)aracılığıyla bir denemeye ulaşmak istedikleri, geçerli bir adres sağlayarak ( *http* veya *https*ile başlayarak), ücretsiz bir denemeye yönelik bir bağlantıya sahip müşterilere teklifinizi listeleyin. Örneğin, `https://contoso.com/trial/saas-app`. Ücretsiz denemelerdeki teklif listesi, hizmetiniz tarafından oluşturulur, yönetilir ve yapılandırılır ve Microsoft tarafından yönetilen abonelikler içermez.

> [!NOTE]
> Deneme bağlantınız aracılığıyla uygulamanızın alacağı belirteçler yalnızca, uygulamanızda hesap oluşturmayı otomatikleştirmek için Azure AD aracılığıyla Kullanıcı bilgilerini almak üzere kullanılabilir. Bu belirteci kullanarak kimlik doğrulaması için Microsoft hesapları (MSA) desteklenmez.

#### <a name="contact-me"></a>Benimle iletişim kurun

Müşteri Ilişkileri yönetimi (CRM) sisteminizi bağlayarak müşteri iletişim bilgilerini toplayın. Müşterinin, bilgilerini paylaşması için izin istenir. Bu müşteri ayrıntıları, teklifinizin bulunduğu teklif adı, KIMLIĞI ve Market kaynağıyla birlikte, yapılandırdığınız CRM sistemine gönderilir. CRM 'nizi yapılandırma hakkında daha fazla bilgi için bkz. [Connect lider yönetimi](#connect-lead-management).

#### <a name="example-marketplace-offer-listing"></a>Örnek market teklifi listesi

![Not ile örnek Market teklif listesi](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Sınama sürücüsünü etkinleştir

Test sürücüsü, "satın almadan önce dene" seçeneği sunarak olası müşterilere teklifinizi göstermek için harika bir yoldur. bu sayede, artan dönüştürmeye ve yüksek oranda nitelikli müşteri adaylarının oluşturulmasına neden olur. [Sınama sürücüleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Bir sınama sürücüsünü sabit bir süre etkinleştirmek için, **bir sınama sürücüsünü etkinleştir** onay kutusunu seçin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Daha fazla bilgi için bkz. [ticari Market 'te teklifinizin test](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive)etme.

### <a name="test-drive-resources"></a>Sınama sürücüsü kaynakları

- [En iyi pazarlama deneyimleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [En iyi teknik uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Genel bakış](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF; açılır pencere engelleyicinizin kapalı olduğundan emin olun)

### <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlama

[!INCLUDE [Connect lead management](./includes/connect-lead-management-a.md)]

#### <a name="additional-lead-management-resources"></a>Ek lider Yönetimi kaynakları
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri adayı yönetimine genel bakış bir sayfalayıcı](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi markette gruplandırmak için kullanılan kategorileri ve endüstrileri, teklifinizi destekleyen yasal sözleşmeleri ve uygulama sürümünüzü tanımlamanızı ister.

### <a name="category"></a>Kategori

Teklifinizi uygun Market arama alanlarında gruplandırmak için en az bir ve en fazla üç kategori seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini açıklayacağız.

### <a name="industries"></a>Sektörler

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

### <a name="app-version"></a>Uygulama sürümü

Bu alan isteğe bağlıdır ve teklifinizin sürüm numarasını belirlemek için AppSource Market 'te kullanılır.

### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari Market için standart sözleşme

Microsoft, standart bir sözleşme şablonu sağlar.

- **Microsoft ticari Market için standart sözleşme kullanılsın mı?**

Microsoft, müşterilerin tedarik sürecini basitleştirmek ve yazılım satıcılarının yasal karmaşıklığını azaltmak için Microsoft ticari Market 'e yönelik standart bir sözleşme sunarak Market 'teki işlemleri kolaylaştırmaya yardımcı olur. Ticari Market yayımcıları, özel hüküm ve koşulları ortadan kaldırmaktansa, müşterilerin yalnızca her bir kez ve kabul etmesi gereken standart sözleşme altında yazılımlarını sunmayı tercih edebilir. Standart Sözleşme adresinde https://go.microsoft.com/fwlink/?linkid=2041178bulunabilir.

"Ticari Market için standart sözleşmeyi kullan" onay kutusunu seçerek, kendi özel hüküm ve koşullarınızı sağlamak yerine standart sözleşmeyi kullanmayı seçebilirsiniz.

![Standart Sözleşme onay kutusunu kullanma](./media/use-standard-contract.png)

> [!NOTE]
> Microsoft ticari Market için standart sözleşmeyi kullanarak bir teklif yayımladığınızda, kendi özel hüküm ve koşullarınızı kullanamazsınız. Bu bir "veya" senaryosudur. Çözümünüzü standart sözleşme kapsamında **ya** da kendi hüküm ve koşullarınız için sunun. Standart sözleşmenin şartlarını değiştirmek isterseniz, bunu standart sözleşme değişiklikleri aracılığıyla yapabilirsiniz.

#### <a name="standard-contract-amendments"></a>Standart Sözleşme düzeltme açıklamaları

Standart anlaşma değişikliği, yayımcıların basitlik için standart sözleşme koşullarını seçmesini ve ürün ya da işletmelerinin koşullarını özelleştirmesini sağlar. Müşterilerin, Microsoft standart sözleşmesini gözden geçirmiş ve kabul etmiş olmaları durumunda yalnızca sözleşmenin değişiklik yapması gerekir.

Ticari Market yayımcıları için iki çeşit değişiklik bulunur:

- Evrensel değişiklik: Bu değişiklik, tüm müşteriler için standart sözleşmeye evrensel olarak uygulanır. Evrensel değişiklik, satın alma akışındaki teklifin her müşterisi için gösterilir. Müşteriler teklifinizi kullanabilmeniz için standart sözleşmenin ve değişikliğin koşullarını kabul etmelidir.
- Özel değişiklik: Bu değişiklik, yalnızca Azure kiracı kimlikleri aracılığıyla belirli müşterileri hedefleyen standart sözleşmeye yönelik özel değişiklik amaçlıdır. Yayımcılar hedeflemek istedikleri kiracıyı seçebilirler. Yalnızca kiracının müşterilerine, teklifin satın alma akışındaki özel değişiklik koşulları sunulur.  Müşterilerin teklifinizi kullanabilmesi için önce standart sözleşme ve değişiklik koşullarını kabul etmesi gerekir.

>[!NOTE]
> Bu iki tür değişiklik yığını birbirlerinin üzerine. Özel değişiklik ile hedeflenen müşteriler, satın alma sırasında standart sözleşmeye evrensel değişiklik de alır.

**Evrensel değişiklik Microsoft 'un ticari Market 'e yönelik standart sözleşme** – bu kutuya evrensel düzeltme koşullarını girin. Teklif başına tek bir evrensel düzeltme sağlayabilirsiniz. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Bu terimler, bulma ve satın alma akışında AppSource, Azure Marketi 'nde müşterilere ve/veya Azure portal görüntülenir.

**Microsoft 'un ticari Market 'e yönelik standart sözleşmeye yönelik özel düzeltme koşulları** – **özel değişiklik koşulları Ekle**' ye tıklayarak başlayın. Teklif başına en fazla 10 özel düzeltme terimi sağlayabilirsiniz.

- **Özel değişiklik koşulları** : özel değişiklik koşullarını özel düzeltme koşulları kutusuna girin. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Yalnızca bu özel şartlar için belirttiğiniz kiracı kimliklerinin müşterileri, Azure portal teklifin satın alma akışındaki özel değişiklik hükümleriyle birlikte sunulacaktır.  
- **Kiracı kimlikleri** (gerekli) – her bir özel değişiklik, en fazla 20 Kiracı kimliğini hedefleyebilir. Özel bir değişiklik eklerseniz, en az bir kiracı KIMLIĞI sağlamanız gerekir. Kiracı kimliği, Azure 'da müşterinizin kimliğini tanımlar. Müşterinizden bu KIMLIĞI sorabilir ve portal.azure.com > Azure Active Directory > özelliklerine giderek bulabilecekleri. Dizin KIMLIĞI değeri kiracı KIMLIĞIDIR (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). Ayrıca, [Microsoft Azure ve Office 365 KIRACı kimliği olan](https://www.whatismytenantid.com)etki alanı adı URL 'sini kullanarak KURULUŞUNUZUN Kiracı kimliğini de arayabilirsiniz.
- **Açıklama** (isteğe bağlı): Isteğe bağlı olarak, Kiracı kimliği için, değişiklik yaparken hedeflediğiniz müşteriyi belirlemenize yardımcı olan kolay bir açıklama sağlayın.

#### <a name="terms-and-conditions"></a>hüküm ve koşullar

Kendi özel hüküm ve koşullarınızı sağlamak istiyorsanız, bunları hüküm ve koşullar alanına girmeye karar verebilirsiniz. Bu alana en fazla 10.000 karakter girebilirsiniz. Hüküm ve koşullarınız daha uzun bir açıklama gerektiriyorsa, hüküm ve koşullarınızın bulunabileceği bu alana tek bir URL bağlantısı girin. Bu, müşterilere etkin bir bağlantı olarak görüntülenecektir.

Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

Bu sayfada teklifinizin kullanılabildiği diller (ve pazarlar) görüntülenir. Şu anda Ingilizce (Birleşik Devletler) kullanılabilir tek konumdur. Ayrıca, Bu sayfa dile özgü listenin durumunu ve eklendiği tarih/saati görüntüler. Her dil/Pazar için Market ayrıntılarını (teklif adı, açıklama, arama terimleri vb.) tanımlamanız gerekir.

> [!NOTE]
> Teklif açıklaması (teklif açıklaması, belgeler, ekran görüntüleri, kullanım koşulları ve Gizlilik ilkesi gibi) sunan teklif listesi, "Bu uygulama yalnızca [Ingilizce olmayan dil] ' de kullanılabilir." Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

### <a name="offer-listings"></a>Teklif listeleri

Teklif ve pazarlama varlıklarınızın açıklamaları dahil olmak üzere Market 'te görüntülenecek ayrıntıları sağlayın.

- **Ad** (gerekli) – burada tanımlanan ad, seçtiğiniz Market (ler) de teklif listelerinizin başlığı olarak görünür. Ad, önceki **yeni teklif** girişinizin temel alınarak önceden doldurulur. Ad trademarked olabilir. Bu, emojıs (ticari marka ve telif hakkı sembolleri olmadıkları müddetçe) içeremez ve 50 karakterle sınırlı olmalıdır.
- **Özet** (zorunlu) – Market listeleme arama sonuçlarında kullanılacak teklifinizin kısa bir açıklamasını sağlayın. Bu alana en fazla 100 karakter metin girilebilir.
- **Açıklama** (gerekli) – Market dökümüne genel bakış bölümünde gösterilecek teklifin açıklamasını sağlayın. Bir değer teklifi, önemli avantajlar, herhangi bir kategori veya sektör ilişkilendirmesi, uygulama içi satın alma fırsatları, gerekli ön kapanışlar ve daha fazla bilgi edinmek için bir bağlantı dahil etmeyi göz önünde bulundurun. Biçimlendirme de dahil olmak üzere bu alana en fazla 3.000 karakter girilebilir. Ek ipuçları için bkz. [harika bir uygulama açıklaması yazma](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description).
- **Anahtar sözcükleri ara** – müşterilerin Market 'te teklifinizi bulmak için kullanabileceği en fazla üç arama anahtar sözcüğü girin.
- Başlarken yönergeleri (gerekli) – uygulamanızı nasıl yapılandıracağınızı ve potansiyel müşteriler için kullanmaya **başlamanızı** açıklayın.  Bu hızlı başlangıç, daha ayrıntılı çevrimiçi belgelere bağlantılar içerebilir. Bu alana en fazla 3.000 karakter metin girilebilir.

#### <a name="description"></a>**Açıklama**

Bu alan gereklidir. **Açıklamaya**eklenecek öğeler:

* Tanımınızın ilk birkaç cümlede teklifinizin değer teklifini açık bir şekilde açıklamalıdır.  
* İlk birkaç tümcenin arama motoru sonuçlarında görüntülenebileceğini aklınızda bulundurun.  
* Ürününüzü satmaya yönelik özelliklere ve işlevlere güvenmeyin. Bunun yerine, teslim ettiğiniz değere odaklanın.  
* Sektöre özgü sözlük veya avantaj temelli bir ifade mümkün olduğunca yararlanın.

Değer teklifini temel bileşenler şunlardır:

* Ürünün açıklaması
* Üründen faydalanan Kullanıcı türü
* Müşteri ihtiyaçları veya ürün adresleriyle ilgili bir sorun

Teklif açıklamanızı daha ilgi çekici hale getirmek için, biçimlendirme uygulamak için zengin metin düzenleyicisini kullanın.

![Zengin metin düzenleyicisini kullanma](./media/rich-text-editor.png)

| <center>Metin biçimini değiştir | <center>Madde işaretleri veya numaralandırma ekleme | <center>Metin girintisi ekleme veya kaldırma |
| --- | --- | --- |
| <center>![Metin biçimini değiştirmek için zengin metin düzenleyicisini kullanma](./media/text-editor3.png) |  <center>![Liste eklemek için zengin metin düzenleyicisini kullanma](./media/text-editor4.png) |  <center>![Girintilemek için zengin metin düzenleyicisini kullanma](./media/text-editor5.png) |

#### <a name="links"></a>Bağlantılar

- **Gizlilik ilkesi** (gerekli) – kuruluşunuzun gizlilik ilkesine bağlayın. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.
- **CSP program pazarlama malzemeleri** (isteğe bağlı) – teklifinizi [bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) programına genişletmeyi seçerseniz, pazarlama malzemeleri için bir bağlantı sağlayın. CSP, CSP iş ortaklarının teklifinizi paketlemelerine, pazarlamaya ve satmasına olanak tanıyarak, teklifinizi daha geniş bir yelpazede nitelikli müşterilere genişletir. Bu satıcıların, teklifinizi pazarlama malzemelerinden sorumlu olması gerekir. Daha fazla bilgi için bkz. [Go-to-market Services](https://partner.microsoft.com/reach-customers/gtm).
- **Kullanışlı bağlantılar** (isteğe bağlı) – uygulamanız veya bir **başlık** ve **URL 'si**sağlanarak listelenen ilgili hizmetlerinize ilişkin isteğe bağlı ek çevrimiçi belgeler. **+ URL Ekle**' ye tıklayarak ek yararlı bağlantılar ekleyin.

#### <a name="contact-information"></a>İletişim bilgileri

- **Kişiler** – her müşteri kişisi için, bir çalışan **adı**, **telefon numarası**ve **e-posta** adresi sağlayın ( *bunlar genel olarak gösterilmez* ). **Destek kişi** grubu Için bir **Destek URL 'si** gereklidir (Bu, herkese açık *şekilde görüntülenecektir)* .

    - **Destek kişisi** (gerekli) – genel destek soruları için.
    - **Mühendislik ilgili kişisi** (gerekli) – Teknik sorular için.
    - CSP programıyla ilgili satıcı soruları Için **Channel Manager ile iletişim** (gerekli).

#### <a name="files-and-images"></a>Dosyalar ve görüntüler

- **Belgeler** (gerekli) – teklif başına en az bir (1) ve en fazla üç (3) belge sağlayan PDF biçiminde ilgili pazarlama belgelerini ekleyin.
- **Görüntüler** (isteğe bağlı): teklifinizin logo görüntülerinin Market genelinde GÖRÜNEBILECEĞI, PNG biçiminde aşağıdaki piksel boyutlarını gerektiren birden çok yer vardır:

    - **Küçük** (48 x 48, gereklidir)
    - **Orta** (90 x 90, gereklidir)
    - **Büyük** (216 x 216, gereklidir)
    - **Geniş** (255 x 115)
    - **Hero** (815 x 290)

- **Ekran görüntüleri** (gerekli) – teklifinizi gösteren ekran görüntüleri ekleyin. En fazla beş (5) ekran görüntüsü eklenebilir ve 1280 x 720 piksel boyutunda olmalıdır. Tüm görüntülerin içinde olması gerekir. PNG biçimi.
- **Videolar** (isteğe bağlı) – teklifinizi gösteren videoların bağlantılarını ekleyin. Müşteriler teklifinizle birlikte gösterilen YouTube ve/veya Vimeo videolarını içeren bağlantıları kullanabilirsiniz. Ayrıca, PNG biçiminde 1280 x 720 piksel boyutunda videonun küçük resmini de girmeniz gerekir. Her teklif için en fazla dört video görüntüleyebilirsiniz.

>[!NOTE]
>Dosya yükleme sorununuz varsa, yerel ağınızın Iş Ortağı Merkezi tarafından kullanılan https://upload.xboxlive.com hizmeti engellemediğinden emin olun.

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

- [Market teklif listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="preview-audience"></a>İzleyiciyi Önizle

Bu sayfa, teklifinizi daha geniş Market hedef kitlelerine yayımlamadan önce teklifinizi serbest bırakmak için sınırlı bir **Önizleme hedef kitlesi** tanımlamanızı sağlar.

> [!IMPORTANT]
> Teklifinizi önizlemede denetledikten sonra, teklifinizin Market ortak hedef kitlesi için canlı olarak yayınlanabilmesi için **canlı git** ' i seçin.

İsteğe bağlı bir açıklamayla birlikte her satır için tek bir AAD/MSA hesap e-postası ekleyin.

Canlı yayımlamadan önce teklifinizi doğrulamaya yardımcı olmak üzere mevcut Microsoft hesabı (MSA) veya Azure Active Directory hesapları için el ile 10 e-posta adresi veya bir CSV dosyası karşıya yüklüyorsanız 20 ' yi ekleyin. Bu hesapları ekleyerek, Market 'te yayınlanmadan önce teklifiniz için önizlemeye erişim izni verilecek bir hedef kitle tanımlamanız gerekir. Teklifiniz zaten canlı ise, teklifiniz için herhangi bir değişikliği veya güncelleştirmeyi test etmek üzere bir önizleme hedef kitlesi de tanımlayabilirsiniz.

> [!NOTE]
> Önizleme hedef kitlesi, özel bir hedef kitleye göre farklılık gösterir. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine izin verilir. Ayrıca, bir plan oluşturmayı ve yalnızca özel bir hedef kitle için kullanılabilir hale getirebilirsiniz. **Plan listeleme** sekmesinde, **Bu özel bir plan** onay kutusu ile özel bir hedef kitle tanımlayabilirsiniz. Daha sonra, Azure kiracı kimliklerini kullanarak 20.000 adede kadar müşterinin özel bir kitlesini tanımlayabilirsiniz.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="technical-configuration"></a>Teknik yapılandırma

Bu sayfa, teklifinizi bağlamak için kullanılan teknik ayrıntıları (URL yolu, Web kancası, kiracı KIMLIĞI ve uygulama KIMLIĞI) tanımlar. Bu bağlantı, teklifi edinmeyi tercih ettiklerinde, son müşteri için teklifinizi sağlamamızı sağlar. Toplanan alanların kullanımını açıklayan diyagramlar [SaaS karşılama API 'leri](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2)için belgelerde bulunabilir.

- **Giriş sayfası URL 'si** (gerekli) – müşterilerinizin teklifinizi Market 'ten aldıktan sonra HANGI site URL 'sini tanımlayın. Bu URL, bir müşteri sayfaya yönlendirildiğini belirteç alan uç nokta olur. Bu belirteç, karşılama API 'Lerinde çözümle kullanılarak sağlama ayrıntıları için takas edilebilir. Bu ayrıntılar ve topladığınız tüm diğerleri, kaydınızı tamamlayıp satın almasını etkinleştirmek için deneyiminizin yerleşik olduğu müşteri etkileşimli Web sayfasının bir parçası olarak kullanılabilir.

- **Bağlantı Web kancası** (gerekli) – Microsoft 'un müşteri adına size gönderilmesi gereken tüm zaman uyumsuz olaylar için (örneğin, SaaS aboneliği geçersiz oldu), bağlantı Web kancası sağlamanız gerekir. Zaten bir Web kancası sisteminiz yoksa, en basit yapılandırma, kendisine gönderilen olayları dinleyen bir HTTP uç noktası mantıksal uygulamasına sahip olmak ve bunları uygun şekilde işleymelidir (örneğin, https:\//prod-1westus.Logic.Azure.com:443/Work). Daha fazla bilgi için bkz. [Logic Apps 'TE HTTP uç noktaları Ile çağrı, tetikleyici veya iç içe iş akışları](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **Azure AD KIRACı kimliği** (gerekli) – Azure Portal içinde, iki hizmetimiz arasındaki bağlantının kimliği doğrulanmış bir iletişimin arkasında olduğunu doğrulayabilmemiz için [BIR Azure Active Directory (ad) uygulaması oluşturmanız](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) gerekir. [KIRACı kimliğini](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)bulmak için Azure Active Directory gidin ve **Özellikler**' i seçin, ardından listelenen **dizin kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD uygulama kimliği** (gerekli) – Ayrıca, [Uygulama Kimliğiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) ve bir kimlik doğrulama anahtarınız olması gerekir. Bu değerleri almak için Azure Active Directory gidin ve **uygulama kayıtları**' i seçin, ardından LISTELENEN **uygulama kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). Kimlik doğrulama anahtarını bulmak için **Ayarlar** ' a gidin ve **anahtarlar**' ı seçin. Bir açıklama ve süre sağlamanız gerekir ve ardından bir sayı değeri sağlanacaktır.

>[!Note]
>Azure uygulama KIMLIĞI, yayımcı KIMLIĞINIZLE ilişkilendirilir, bu nedenle tüm tekliflerinizi aynı uygulama KIMLIĞININ kullanıldığından emin olun.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="plan-overview"></a>Plana genel bakış

Bu sayfa, aynı teklif dahilinde çeşitli plan seçenekleri sağlamanıza olanak tanır. Bu planlar (bazen SKU 'Lar olarak adlandırılır) sürüm, para veya hizmet katmanları bakımından farklılık gösterebilir. Teklifinizin Market 'te satılabilir olması için en az bir plan ayarlamanız gerekir.

Oluşturulduktan sonra plan adlarınızı, kimlikleri, fiyatlandırma modellerini, kullanılabilirliği (genel veya özel), geçerli yayımlama durumunu ve kullanılabilir eylemleri görürsünüz.

**Plana genel bakış** bakımından sunulan **Eylemler** planınızın geçerli durumuna bağlı olarak farklılık gösterir ve şunlar olabilir:

- Plan durumu **taslak** ise taslağı Sil
- Plan durumu **canlı** ise, bir planı satmayı veya özel izleyiciyi Eşitlemeyi Durdur

**Yeni plan oluştur** (Microsoft üzerinden satış için seçim yapan kişiler için en az bir plan)

- **Plan kimliği:** Bu teklifteki her plan için benzersiz bir plan KIMLIĞI oluşturun. Bu KIMLIK, ürün URL 'sindeki ve Azure Resource Manager şablonlarındaki (varsa) müşterilere görünür olacaktır. Yalnızca küçük harf, alfasayısal karakter, kısa çizgi veya alt çizgi kullanın. Bu plan KIMLIĞI için en fazla 50 karakter kullanılabilir. Oluştur seçildikten sonra KIMLIK değiştirilemez.
- **Plan adı:** Tekliflerinizi hangi plana seçeceğinize karar verirken, müşteriler bu adı görür. Bu teklifteki her plan için benzersiz bir teklif adı oluşturun. Plan adı, aynı teklifin bir parçası olabilecek yazılım planlarını ayırt etmek için kullanılır (örneğin, teklif adı: Windows Server; planlar: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Planı listeleme

Bu sayfa, plan adı ve açıklama tanımlamanızı sağlar. <!-- displays the languages (and markets) where your plan is available, currently English (United States) is the only location available. Additionally, this page displays the status of the language-specific listing and the date/time that it was added. You will need to define the marketplace details (offer name, description, search terms, etc.) for each language / market.-->

<!--#### Plan listing details-->

<!--Selecting one of the plan languages will display the **plan listing** information, including **Name** and **Description.** -->

- **Ad** – Önizleme **Yeni plan** girdinizi temel alarak önceden doldurulmuştur ve teklifinizin Market 'te görüntülenen "yazılım planının" başlığı olarak görünür.
- **Açıklama** – bu açıklama, bu yazılım planının özel ve teklifinizdeki diğer yazılım planlarından herhangi bir farklılık yaptığını açıklayan bir fırsattır. En fazla 500 karakter içerebilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

#### <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Bu sayfa, bu planın kullanılabileceği pazarları, istenen ödeme modeli, Fiyat ve fatura terimini yapılandırmanıza olanak tanır. Ayrıca, planın herkes için mi yoksa yalnızca belirli müşterilere mi (özel bir hedef kitle) görünür mı olacağını belirtebilirsiniz.

#### <a name="markets-optional"></a>Pazarlar (isteğe bağlı)

Her plan en az bir pazarda kullanılabilir olmalıdır. **Pazarları Düzenle** ' yi seçin ve bu planı kullanılabilir yapmak istediğiniz herhangi bir pazar konumunun onay kutusunu seçin. Bu sayfa, Microsoft remits Sales ve sizin adınıza kullanım vergisini sunan "vergi havalesi" ülkelerinden oluşan bir arama kutusu ve seçeneği içerir.

Planınız için Birleşik Devletler dolar (USD) cinsinden zaten fiyatlar ayarladıysanız ve başka bir pazar konumu eklerseniz, yeni pazar fiyatı geçerli döviz kurlarına göre hesaplanır. Yayımlamadan önce her bir pazar için fiyatı gözden geçirin. Değişikliklerinizi kaydettikten sonra "fiyatları dışarı aktar (xlsx)" bağlantısını kullanarak fiyatlandırmayı görüntüleyin.

Devam etmeden önce **Kaydet** ' i seçin.

#### <a name="pricing"></a>Fiyatlandırma

##### <a name="pricing-model"></a>Fiyatlandırma modeli

**Düz oran** : tek bir aylık veya yıllık fiyat düz fiyat fiyatı ile teklifinizin erişimini etkinleştirin. Bu, bazen site tabanlı fiyatlandırma olarak adlandırılır. Bu fiyatlandırma modeliyle isteğe bağlı olarak, müşterileri standart olmayan birimlere göre ücretlendirmesi için Market ölçüm hizmeti API 'sini kullanan tarifeli planlar tanımlayabilirsiniz.  Tarifeli faturalandırma hakkında daha fazla bilgi için bkz. [Market ölçüm hizmeti kullanılarak ölçülen faturalandırma](./saas-metered-billing.md).

**Kullanıcı başına** – teklifle veya işgal eden kullanıcılara erişen kullanıcı sayısına bağlı olarak fiyat ile teklifinizin erişimini etkinleştirin. Bu Kullanıcı tabanlı model, fiyata göre izin verilen en düşük ve en yüksek Kullanıcı sayısını ayarlamanıza olanak sağlar. Bu şekilde, farklı fiyat noktaları, birden çok plan yapılandırılarak kullanıcı sayısına göre yapılandırılabilir.  Bu alanlar isteğe bağlıdır. Sol tarafta yoksa, Kullanıcı sayısı bir sınıra sahip değildir (en az 1, en fazla sistem tarafından desteklenebilir). Bu alanlar, planınızdaki bir güncelleştirmenin parçası olarak düzenlenebilir.

Yayımlandıktan sonra faturalandırma fiyatlandırma modeli seçimi değiştirilemez. Ayrıca, aynı teklif için tüm planlar aynı fiyatlandırma modelini paylaşmalıdır.

##### <a name="user-limits"></a>Kullanıcı sınırları

Uygulanabiliyorsa, en düşük ve en yüksek Kullanıcı sınırlarını seçin ve ayarlayın.

##### <a name="billing-term-and-price"></a>Fatura dönemi ve fiyat

Müşterilerin listelenen fiyatı ödemesi gereken **terimi** ve **fiyatı** seçin. En az bir aylık veya yıllık fiyat belirtilmelidir ya da müşteriler için her iki seçenek de kullanılabilir hale getirilebilir.

ABD Doları cinsinden ayarlanan fiyatlar (USD = Birleşik Devletler dolar), kaydedildiği sırada geçerli döviz kurları kullanılarak tüm seçili piyasaların yerel para birimine dönüştürülür. Fiyatlandırma elektronik tablosunu dışarı aktarıp her bir pazardaki fiyatı inceleyerek yayımlamadan önce bu fiyatları doğrulayın. Tek bir pazarda özel fiyatlar ayarlamak isterseniz, fiyatlandırma elektronik tablosunu değiştirin ve içeri aktarın. Bu fiyatlandırmayı doğrulamak ve bu ayarların sahibi olmak sizin sorumluluğunuzdadır.
*\*Fiyatlandırma verilerinin dışarı aktarılmasını sağlamak için öncelikle fiyatlandırma değişikliklerinizi kaydetmeniz gerekir.*

Bir plan yayımlandıktan sonra nelerin değiştirebilecekleri hakkında bazı kısıtlamalar olduğundan, yayımlamadan önce fiyatlarınızı dikkatlice gözden geçirin:

- Bir plan yayımlandığında fiyatlandırma modeli değiştirilemez.
- Bir plan için faturalandırma dönemi yayımlandıktan sonra, daha sonra kaldırılamaz.
- Planınızdaki bir pazar bedeli yayımlandığında, daha sonra değiştirilemez.

#### <a name="free-trial"></a>Ücretsiz Deneme

Ticari Market aracılığıyla sunulan SaaS teklifleri, Microsoft üzerinden satılırken bir aylık ücretsiz deneme olanağı sağlamanıza olanak tanır. Ölçümlü planlar hariç tüm faturalama modelleri ve koşulları için ücretsiz denemeler desteklenir. Bu seçenek, müşterilerin bir ay boyunca ücretsiz erişim için düşük bir engel olmasına olanak sağlar.  Teklifiniz dahilinde planlar için ücretsiz denemeyi etkinleştirmeyi seçerseniz, müşteri, ilk bir ay döneminin sonundan önce ücretli bir aboneliğe dönüştürülemez.  Bu süre boyunca, teklifinizi satın alan müşteriler ücretsiz deneme sürümü etkin olan ve aralarında dönüştürme desteklenen planların herhangi birini deneyebilir.  Ücretli aboneliğe dönüştürme işlemi, dönem sonunda otomatik olarak yapılır.

>[!NOTE]
>Müşteri, ücretsiz denemeler olmadan bir plana dönüştürmeyi seçerse, dönüştürme gerçekleşecektir, ancak ücretsiz deneme hemen kaybolacaktır. Ayrıca, bir müşteri bir plan için ödeme başladıktan sonra, ücretsiz denemeleri destekleyen bir SKU 'ya dönüştürüseler bile, aynı abonelikte artık ücretsiz deneme alamaz.

Teklifinizdeki her plan için ücretsiz bir deneme yapılandırabilirsiniz. Bir aylık denemeye izin vermek için onay kutusunu işaretleyin.

![Bir aylık ücretsiz deneme onay kutusu](./media/free-trial-enable.png)

>[!NOTE]
>Transactable teklifiniz ücretsiz bir deneme ile yayımlandıktan sonra bu plan için devre dışı bırakılamaz. Planı yeniden oluşturmak zorunda kalmamak için bu ayarın ilk yayımlama için doğru olduğundan emin olun.

Ücretsiz bir denemeye katılmış olan müşteri abonelikleri hakkında bilgi edinmek için, doğru veya yanlış olarak işaretlenecek yeni `isFreeTrial`API özelliğini kullanın. Daha fazla bilgi için [SaaS Get abonelik API 'sine](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2#get-subscription)bakın.

>[!NOTE]
>Market ölçüm hizmetinden yararlanan planlar için ücretsiz denemeler desteklenmez.

#### <a name="plan-visibility"></a>Plan görünürlüğü

Her planı herkese görünür olacak şekilde veya yalnızca seçtiğiniz belirli bir kitlede olacak şekilde yapılandırabilirsiniz. Azure AD kiracı kimliklerini kullanarak bu kısıtlanmış hedef kitleye üyelik atayabilirsiniz.

##### <a name="privacy"></a>Gizlilik

Planınızı özel ve yalnızca seçtiğiniz kısıtlanmış kitlele görünür hale getirmek için **özel bir plan olan bu bir plandır** . Özel bir plan olarak yayımlandıktan sonra izleyiciyi güncelleştirebilir veya planı herkes için kullanılabilir hale getirebilirsiniz. Bir plan herkese görünür olarak yayımlandıktan sonra herkese görünür kalması gerekir (bir özel plan olarak yapılandırılamaz).

##### <a name="restricted-audience-tenant-ids"></a>**Kısıtlanmış hedef kitle (kiracı kimlikleri)**

Bu özel plana erişimi olacak hedef kitleyi atayın. Atanan her kiracı KIMLIĞININ açıklamasını ekleme seçeneğiyle kiracı kimlikleri kullanılarak erişim atanır. Bir. csv elektronik tablo dosyası içeri aktarıldıysanız, en fazla 10 Kiracı kimliği eklenebilir veya 20.000 müşteri Kiracı kimliği olabilir.

Kiracı, bir GUID (genel benzersiz tanımlayıcı, kaynakları tanımlamak için kullanılan 128 bitlik bir tamsayı numarası) olarak temsil edilen KIMLIĞE sahip bir kuruluşun gösterimidir. Kuruluş veya uygulama geliştiricisi, Microsoft ile bir ilişki oluştururken bir kuruluşun veya uygulama geliştiricisinin Microsoft ile bir ilişki oluşturduğunda (örneğin, Azure için kaydolurken, Microsoft Intune veya Microsoft 365) adanmış bir Azure AD örneğidir. Her Azure AD kiracısı benzersizdir ve diğer Azure AD kiracılarından ayrıdır. Kiracıyı denetlemek için, uygulamanızı yönetirken kullanmak istediğiniz hesapla Azure portalında oturum açın. Bir kiracınız varsa, otomatik olarak bu kiracıda oturum açar ve kiracı adını doğrudan hesap adınızın altında görebilirsiniz. Adınızı, e-postanızı, dizininizi/kiracı kimliğinizi (GUID) ve etki alanınızı görmek için, Azure portalının sağ üst kısmında bulunan hesap adınızın üzerine gelin. Hesabınız birden çok kiracıyla ilişkiliyse, kiracılar arasında geçiş yapabileceğiniz bir menüyü açmak için hesap adınızı seçebilirsiniz. Her kiracının kendi kiracı kimliği vardır. Ayrıca, bir etki alanı adı URL 'sini kullanarak kuruluşunuzun kiracı KIMLIĞINI de arayabilirsiniz [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

SaaS, özel bir hedef kitle tanımlamak için kiracı kimlikleri kullanır, diğer teklif türleri de Azure abonelik kimliklerini (GUID 'Ler olarak da temsil edilir) kullanabilir.

> [!NOTE]
> Özel hedef kitle (veya kısıtlı kitleci), önizleme kitlemesinden farklıdır. **[Önizleme](#preview-audience)** sayfasında, bir önizleme hedef kitlesi tanımlayabilirsiniz. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan *önce* teklifinizin erişimine izin verilir. Özel hedef kitle ataması yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi tüm planları (özel veya değil) görüntüleyebilir, ancak yalnızca plan test edilirken ve doğrulandığında sınırlı önizleme döneminde bu süre boyunca yalnızca sınırlı önizleme dönemi boyunca görüntülenebilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Market teklifi içindeki planların örnek listesi

![Notlardaki örnek Market planı listesi](./media/marketplace-plan.svg)

## <a name="cloud-solution-provider-csp-reseller-audience"></a>Bulut çözümü sağlayıcısı (CSP) satıcı hedef kitlesi

, Teklifinizin CSP programında kullanılabilmesini sağlamak için, bulut çözümü sağlayıcılarının, müşterilerine paketlenmiş bir çözümün parçası olarak ürününüzü satmasına olanak sağlar. Daha fazla bilgi için bkz. [bulut çözümü sağlayıcıları](https://go.microsoft.com/fwlink/?linkid=2111109).

## <a name="publish"></a>Yayımlama

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, portalın sağ üst köşesinde bulunan **gözden geçir ve Yayımla** ' yı seçin.

### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Bu teklifi ilk kez yayınlıyorsanız, şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - **Başlatılmamış** – bölüm dokunulmamış ve tamamlanması gerekiyor.
    - **Tamamlanmamış** – bölümde düzeltilmesi gereken hatalar var veya daha fazla bilgi sağlanması gerekiyor. Bölümüne dönüp güncelleştirmeniz gerekir.
    - **Tamamlandı** – bölüm tamamlandı, tüm gerekli veriler sağlanmış ve hata yok. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- Uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Size, gözden geçirmeniz ve onaylamanız için teklifin bir önizleme sürümünün ne zaman kullanılabileceğini bilmenizi sağlayacak bir e-posta göndereceğiz. Teklifinizi ortak (veya özel bir teklif, özel hedef kitleye) yayımlamak için Iş Ortağı Merkezi 'ne dönmeniz ve teklif için **Go-Live** ' ı seçmeniz gerekir.

## <a name="next-step"></a>Sonraki adım

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
