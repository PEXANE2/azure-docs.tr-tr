---
title: Microsoft ticari pazarı için yeni bir SaaS teklifi oluşturun
description: Microsoft AppSource, Azure Marketplace veya Microsoft İş Ortağı Merkezi'ndeki Microsoft ticari pazar programı kullanarak Bulut Çözüm Sağlayıcısı (CSP) programı aracılığıyla listeleme veya satış için hizmet olarak yeni bir Yazılım (SaaS) teklifi nasıl oluşturulur?
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/13/2020
ms.openlocfilehash: a39e1e19f65722b4b5ae809ca943da719a3c6e22
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869769"
---
# <a name="create-a-new-saas-offer"></a>Yeni bir SaaS teklifi oluşturma

Hizmet Olarak Yazılım (SaaS) teklifleri oluşturmaya başlamak için, önce [bir İş Ortağı Merkezi hesabı oluşturduğunuzdan](./create-account.md) ve seçilen Genel **Bakış** sekmesiyle Ticari [Pazar panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers)açtığınızı sağlayın.

![İş Ortağı Merkezi'nde Ticari Pazar panosu](./media/new-offer-overview.png)

>[!Note]
> Bir teklif yayımlandıktan sonra, Ortak Merkezi'nde yapılan teklife yönelik yapılan güncellemeler yalnızca yeniden yayımlandıktan sonra sistemde ve mağaza önlerinde güncellenir. Değişiklik yaptıktan sonra teklifi yayınlanmak üzere gönderdiğinizden emin olun.

+ **Yeni teklifi seçin...** düğmesini seçin ve ardından Hizmet menüsü öğesi **olarak Yazılımı** seçin.

Başka bir teklif türü seçerseniz, eski Bulut [İş Ortağı Portalına](https://cloudpartner.azure.com/)yönlendirilebilirsiniz. Şu anda Partner Center'daki Commercial Marketplace portalında şu anda yalnızca SaaS ve Dynamics 365 teklifleri mevcuttur.

![İş Ortağı Merkezi'nde teklif penceresi oluşturma](./media/new-offer-click.png)

**Yeni teklif** iletişim kutusu görüntülenir.

![Yeni teklif iletişim kutusu](./media/new-offer-popup.png)

## <a name="offer-id-and-alias"></a>Teklif kimliği ve takma ad

- **Teklif Kimliği**: Hesabınızdaki her teklif için benzersiz tanımlayıcı. Bu kimlik, pazar teklifi ve Azure Kaynak Yöneticisi şablonlarının (varsa) URL adresindeki müşteriler tarafından görülebilir. Teklif kimliği küçük, alfasayısal olmalıdır (tireler ve alt çizerler dahil, ancak beyaz boşluk yoktur). **Teklif Kimliği** 50 karakterle sınırlıdır ve *Oluştur'u*seçtikten sonra değiştirilemez.  
Örnek: test-teklif-1
<br>URL ile sonuçlanan:`https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`

- **Teklif diğer adı**: Ortak Merkezi portalındaki teklife atıfta bulunmak için kullanılan ad. Bu ad pazarda kullanılmaz ve *Teklif Adı'ndan* ve müşterilere gösterilecek diğer değerlerden farklıdır. *Oluştur'u*seçtikten sonra bu değer değiştirilemez.

<br>Örnek: Test Teklifi 1&#8482;

**Oluştur**’u seçin.  Bu teklif için **Teklife genel bakış** sayfası oluşturulur.  

<!---
![Offer overview on Partner Center](./media/commercial-marketplace-offer-overview.png)
-->

## <a name="offer-overview"></a>Teklife genel bakış

**Teklife genel bakış** sayfası şunları içerir:

- **Yayımlama durumu,** bu teklifi yayımlamak için gereken adımların ve her adımın tamamlanmasının ne kadar süreceğinin görsel bir temsilini görüntüler. Tamamlanmamış yayımlama adım simgeleri soluk olacak.

- **Teklife genel bakış** menüsü, bu teklifteki işlemleri gerçekleştirmek için bağlantıların bir listesini içerir. Bu işlem listesi, teklifiniz için yaptığınız seçime bağlı olarak değişecektir.  
    - Teklif taslaksa - Taslağı sil
    - Teklif canlı ise - Satış teklifini durdurun
    - Teklif önizlemedeyse - Canlı Yayına Geçin
    - Yayımcı oturumlarını tamamlamadıysanız - Yayımla'yı iptal et

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif kurulumu** sekmesi aşağıdaki bilgileri ister. Bu alanları tamamladıktan sonra **Kaydet'i** seçin.

- **Microsoft üzerinden satmak ister misiniz?** (Evet/Hayır)
    - **Evet,** microsoft aracılığıyla teklifsatmak istiyorum, Microsoft sizin adınıza pazar işlemleri barındırma ile; Veya 
    - **Hayır,** teklifinizi pazar yerleri aracılığıyla listeleyerek, herhangi bir parasal işlemi Microsoft'tan bağımsız olarak işleme almayı tercih edersiniz.

### <a name="sell-through-microsoft"></a>Microsoft üzerinden satış

Microsoft üzerinden satış yapmak daha iyi müşteri bulma ve satın alma sağlar, Microsoft'un sizin adınıza pazar yeri işlemlerini barındırmasına olanak tanır ve Microsoft'un genel olarak kullanılabilen ticaret özelliklerinden yararlanır.

#### <a name="saas-offer-requirements"></a>SaaS teklif gereksinimleri

İş Ortağı Merkezi'ndeki Commercial Marketplace ile Yazılımları Hizmet Olarak Listelemek (SaaS) teklifleriiçin aşağıdaki kriterlerin karşılanması gerekir:

- Teklifiniz, kimlik yönetimi ve kimlik doğrulama için [Azure Active Directory 'i (Azure AD)](https://azure.microsoft.com/services/active-directory/) kullanmalıdır.
- Teklifiniz, Azure Marketi ile tümleştirmek için [SaaS Karşılama API'lerini](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) kullanmalıdır.
- Daha kapsamlı gereksinimler için [SaaS Teklif Yayın Kılavuzu'na](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide)bakın.

#### <a name="saas-pricing-and-billing-options"></a>SaaS fiyatlandırma ve faturaseçenekleri

Yayıncının Azure aboneliğinde çalışan SaaS çözümleriyle, müşteriler tarafından ödenen lisans ücretleri, yazılımın dağıtıldığı altyapının maliyetini içerir. Azure altyapı kullanımı doğrudan iş ortağı nız tarafından yönetilir ve faturalandırılır. Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez. Yayıncılar, Azure altyapı kullanım ücretlerini yazılım lisans fiyatlandırmalarına dahil etmelidir. 

SaaS, tarifeli faturalandırma hizmetini kullanarak kullanıcı başına sabit bir ücrete veya tüketim ücretlerine dayalı aylık veya yıllık faturalandırma desteği sunar. Microsoft'un ticari pazarı, yayıncıların fiyatları belirlediği, Microsoft faturalarını müşterilerini ve Microsoft'un bir ajans ücreti tutarken yayımcıya gelir ödediği bir ajans modeli üzerinde çalışır.

Aşağıdaki tablo, acente modelini göstermek için maliyetlerin ve ödemelerin örnek bir dökümünü gösterir.

|**Lisans maliyetiniz**|**Ayda 100 $**|
|:---|:---|
|Azure kullanım maliyeti (D1/1-Core)|Müşteriye değil, doğrudan yayımcıya faturalandırılır|
|Müşteri Microsoft tarafından faturalandırılır|$100.00 /ay (Publisher, lisans ücretinde tahakkuk eden veya geçiş yapılan altyapı maliyetlerini hesaba katmalıdır)|

|**Microsoft faturaları**|**Ayda 100 $**|
|:---|:---|
|Microsoft, lisans maliyetinizin %80'ini size öder <br>**Nitelikli SaaS uygulamaları için Microsoft lisans maliyetinizin %90'ını öder*|$80.00 /ay <br>*$* Ayda 90,00*|

- Bu örnekte, Microsoft yazılım lisansınız için müşteriye 100,00 TL fatura eder ve yayımcıya 80,00 TL öder.
- **İndirimli Pazar Yeri Hizmet Ücreti'ne** hak kazanan iş ortakları, Mayıs 2019'dan Haziran 2020'ye kadar SaaS tekliflerinde indirimli işlem ücreti görebilecektir. Bu senaryoda, Microsoft yazılım lisansınız için 100,00 TL fatura eder ve yayımcıya 90,00 TL öder.

> [!NOTE]
> **İndirimli Pazar Yeri Hizmet Ücreti**: Ticari Pazarımızda yayınladığınız belirli SaaS teklifleri için Microsoft, Pazar Yeri Hizmet Ücretini %20'den (Microsoft Publisher Anlaşması'nda açıklandığı gibi) %10'a düşürecektir. Teklifinizin geçerli olabilmesi için, tekliflerinizden en az birinin Microsoft tarafından IP ortak satış hazır veya IP ortak satışı öncelikli olarak belirlenmiş olması gerekir.  Bu indirimli Pazar Yeri Hizmet Ücretini alabilmek için her takvim ayının bitiminden en az beş (5) iş günü önce uygunluk karşılanmalıdır.  İndirimli Pazar Yeri Hizmet Ücreti, Ticari Pazar Yerimiz aracılığıyla sunulan VM'ler, Yönetilen Uygulamalar veya diğer ürünler için geçerli değildir.  İndirimli Pazar Yeri Hizmet Ücreti yalnızca 1 Mayıs 2019 ile 30 Haziran 2020 tarihleri arasında Microsoft tarafından tahsil edilen lisans ücretleri için nitelikli teklifleriçin geçerli olacaktır.  Bu süre sonunda, Market Servis Ücreti normal tutarına geri dönecektir.

### <a name="list-through-microsoft"></a>Microsoft üzerinden listele

Bir pazar yeri girişi oluşturarak Microsoft ile işletmenizi tanıtın. Teklifinizi yalnızca listelemek ve Microsoft aracılığıyla işlem yapmamayı seçmek, Microsoft'un doğrudan yazılım lisansı işlemlerine katılmadığı anlamına gelir. İlişkili bir işlem ücreti yoktur ve yayıncı müşteriden toplanan yazılım lisanslama ücretlerinin %100'ünü saklar. Ancak, yayıncı yazılım lisans işleminin tüm yönlerini desteklemekten sorumludur, ancak bunlarla sınırlı değildir: sipariş yerine getirilmesi, ölçümleme, faturalama, faturalama, ödeme ve tahsilat.

- **Potansiyel müşterilerin bu giriş teklifiyle nasıl etkileşimde bulunmasını istiyorsunuz?**

#### <a name="get-it-now-free"></a>Şimdi alın (ücretsiz)

[Azure Active Directory (Azure AD) kullanarak tek tıklamayla kimlik doğrulama](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials)yoluyla deneme alabilecekleri geçerli bir URL *(http* veya *https*ile başlayan) sağlayarak müşterilere teklifinizi ücretsiz olarak listeleyebilirsiniz.  Örneğin, `https://contoso.com/saas-app`

#### <a name="free-trial-listing"></a>Ücretsiz deneme (listeleme)

[Azure Active Directory (Azure AD) kullanarak tek tıklamayla kimlik doğrulama](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials)yoluyla deneme alabilecekleri geçerli bir URL *(http* veya *https*ile başlayan) sağlayarak müşterilere teklifinizi ücretsiz deneme sürümüyle listele.  Örneğin: `https://contoso.com/trial/saas-app`. Teklif listesi ücretsiz denemeler oluşturulur, yönetilir ve hizmetiniz tarafından yapılandırılır ve Microsoft tarafından yönetilen abonelikleri yoktur.

> [!NOTE]
> Uygulamanızın deneme bağlantınız aracılığıyla alacağı belirteçler, yalnızca uygulamanızdaki hesap oluşturmayı otomatikleştirmek için Azure AD aracılığıyla kullanıcı bilgilerini elde etmek için kullanılabilir. Microsoft Hesapları (MSA), bu belirteç kullanılarak kimlik doğrulaması için desteklenmez.

#### <a name="contact-me"></a>Bana ulaşın

Müşteri İlişkileri Yönetimi (CRM) sisteminizi bağlayarak müşteri iletişim bilgilerini toplayın. Müşteriden bilgilerini paylaşmak için izin istenecektir. Bu müşteri bilgileri, teklif inizi buldukları teklif adı, kimlik ve pazar yeri kaynağıyla birlikte, yapılandırdığınız CRM sistemine gönderilir. CRM'nizi yapılandırma hakkında daha fazla bilgi için [müşteri adayı yönetimini bağla'ya](#connect-lead-management)bakın.

## <a name="example-marketplace-offer-listing"></a>Örnek market teklifi listesi

![Notlar ile örnek pazar teklifi listesi](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Test sürüşü etkinleştirme

Test sürüşü, potansiyel müşterilere teklifinizi 'satın almadan önce deneme' seçeneği sunarak, dönüşümün artması ve yüksek nitelikli müşteri adaylarının üretilmesiyle sonuçlanan harika bir yoldur. Daha fazla bilgi için bkz: [Müşterilerinizin teklifinizi test etmesine izin](./test-drive.md)verin.

- **Test sürücüsü (onay** kutusu) etkinleştirme

Test sürüşü etkinleştirerek, müşterilerin teklifinizi belirli bir süre boyunca denemeleri için bir gösteri ortamı yapılandırmanız istenir. 

### <a name="test-drive-resources"></a>Test sürüşü kaynakları

- [Test Drive pazarlama en iyi uygulamaları](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)- [Test Drive teknik en iyi uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Test Sürüşüne genel bakış (PDF indir)](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlayın

[!INCLUDE [Connect lead management](./includes/connect-lead-management-a.md)]

#### <a name="additional-lead-management-resources"></a>Ek müşteri adayı yönetim kaynakları
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri Adayı Yönetimine Genel Bakış Bir Çağrı Cihazı](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf)

Bir sonraki bölüme geçmeden önce **Kaydetmeyi** unutmayın.

## <a name="properties"></a>Özellikler

**Özellikler** sekmesi, teklifinizi pazarlarda gruplandırmak için kullanılan kategorileri ve sektörleri, teklifinizi destekleyen yasal sözleşmeleri ve uygulama sürümünü tanımlamanızı ister.

Bu alanları tamamladıktan sonra **Kaydet'i** seçin.

### <a name="category"></a>Kategori

Teklifinizi uygun pazar yeri arama alanlarına gruplandırmak için kullanılan en az bir (1) ve en fazla üç (3) kategori seçin. Teklifaçıklamanızda teklifinizin bu kategorileri nasıl desteklediğini öğrenin.

### <a name="industry"></a>Endüstri

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

### <a name="app-version"></a>Uygulama sürümü

Bu alan isteğe bağlıdır ve teklifinizin sürüm numarasını belirlemek için AppSource pazarda kullanılır.

### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari pazar için Standart Sözleşme

Microsoft bir Standart Sözleşme şablonu sağlar.

- **Microsoft ticari pazarı için Standart Sözleşme'yi mi kullanıyormusunuz?**

Müşteriler için satın alma işlemini kolaylaştırmak ve yazılım satıcıları için yasal karmaşıklığı azaltmak için Microsoft, pazardaki işlemleri kolaylaştırmak için Microsoft ticari pazar yeri için standart bir sözleşme sunar. Ticari pazar yeri yayıncıları, özel hüküm ve koşulları oluşturmayerine, müşterilerin yalnızca bir kez inceleyip kabul etmeleri gereken Standart Sözleşme kapsamında yazılımlarını sunmayı seçebilirler. Standart Sözleşme burada bulunabilir: https://go.microsoft.com/fwlink/?linkid=2041178.

"Ticari pazar için Standart Sözleşmeyi Kullan" onay kutusunu seçerek kendi özel hüküm ve koşullarınızı sağlamak yerine Standart Sözleşme'yi kullanmayı seçebilirsiniz.

![Standart Sözleşme onay kutusunu kullanma](./media/use-standard-contract.png)

> [!NOTE]
> Microsoft ticari pazarı için Standart sözleşmeyi kullanarak bir teklif yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Bu bir "ya da" senaryosu. Çözümünüzü Standart Sözleşme kapsamında **veya** kendi hüküm ve koşullarınız altında sunarsınız. Standart Sözleşme'nin koşullarını değiştirmek isterseniz, bunu Standart Sözleşme Değişiklikleri yoluyla yapabilirsiniz.

#### <a name="standard-contract-amendments"></a>Standart Sözleşme Değişiklikleri

Standart Sözleşme Değişiklikleri, yayıncıların basitlik için Standart Sözleşme koşullarını seçmelerine ve ürün veya işletmelerinin koşullarını özelleştirmelerine izin vermez. Müşterilerin yalnızca Microsoft Standart Sözleşmesi'ni gözden geçirip kabul etmişlerse sözleşmedeki değişiklikleri gözden geçirmeleri gerekir.

Ticari pazar yeri yayıncıları için iki tür değişiklik vardır:

- Evrensel Değişiklikler: Bu değişiklikler tüm müşteriler için Standart Sözleşme'ye evrensel olarak uygulanır. Evrensel değişiklikler, teklifin her müşterisine satın alma akışında gösterilir. Müşteriler, teklifinizi kullanabilmeleri için Standart Sözleşme'nin koşullarını ve değişikliği kabul etmelidir.
- Özel Değişiklikler: Bu değişiklikler, Standart Sözleşme'de belirli müşterileri yalnızca Azure kiracı tbm'leri aracılığıyla hedefleyen özel değişikliklerdir. Yayıncılar hedeflemek istedikleri kiracıyı seçebilirler. Yalnızca kiracının müşterilerine teklifin satın alma akışında özel değişiklik koşulları sunulacaktır.  Müşteriler, teklifinizi kullanabilmeleri için Standart Sözleşme'nin ve değişiklik koşullarını kabul etmelidir.

>[!NOTE]
> Bu iki değişiklik türü üst üste yığ. Özel değişikliklerle hedeflenen müşteriler, satın alma sırasında Standart Sözleşme'de evrensel değişiklik de elde edeceklerdir.

**Microsoft'un ticari pazarı için Standart Sözleşme'nin evrensel değişiklik koşulları:** Bu kutuya evrensel değişiklik koşullarını girin. Teklif başına tek bir evrensel değişiklik sağlayabilirsiniz. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Bu koşullar, bulma ve satın alma akışı sırasında AppSource, Azure Marketplace ve/veya Azure portalında müşterilere görüntülenir.

**Microsoft'un ticari pazarı için Standart Sözleşme'ye özel değişiklik koşulları**: **Özel değişiklik terimleri ekle'yi**seçerek başlayın. Teklif başına en fazla 10 özel değişiklik şartı sağlayabilirsiniz.

- **Özel değişiklik koşulları**: Özel değişiklik terimlerinizi özel değişiklik terimleri kutusuna girin. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Yalnızca bu özel koşullar için belirttiğiniz kiracı lı müşterilere, teklifin Azure portalındaki satın alma akışındaki özel değişiklik koşulları sunulur.  
- **Kiracı atamaları** (gerekli): Her özel değişiklik en fazla 20 kiracı lı ekine hedeflenebilir. Özel bir değişiklik eklerseniz, en az bir kiracı kimliği sağlamanız gerekir. Kiracı kimliği Azure'da müşterinizi tanımlar. Müşterinizden bu kimliği isteyebilir ve azure Active Directory > Properties > portal.azure.com'e giderek bu kimliği bulabilirler. Dizin kimliği değeri kiracı kimliğidir (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). [Microsoft Azure ve Office 365 kiracı kimliğim nedir?](https://www.whatismytenantid.com).
- **Açıklama** (isteğe bağlı): İsteğe bağlı olarak, değişiklikle hedeflediğiniz müşteriyi belirlemenize yardımcı olan kiracı kimliği için dostça bir açıklama sağlayın.

#### <a name="terms-and-conditions"></a>hüküm ve koşullar

Kendi özel hüküm ve koşullarınızı sağlamak istiyorsanız, bunları şartlar ve koşullar alanına girmeyi seçebilirsiniz. Bu alana en fazla 10.000 karakter metin girebilirsiniz. Hüküm ve koşullarınız daha uzun bir açıklama gerektiriyorsa, şartlar ve koşullarınızın bulunabileceği bu alana tek bir URL bağlantısı girin. Bu aktif bir bağlantı olarak müşterilere görüntülenir.

Müşterilerin teklifinizi denemeden önce bu koşulları kabul etmeleri gerekmektedir.

Bir sonraki bölüme geçmeden önce **Kaydetmeyi** unutmayın.

## <a name="offer-listing"></a>Teklif listesi

Teklif listesi sekmesi, teklifinizin mevcut olduğu dilleri (ve pazarları) görüntüler, şu anda İngilizce (ABD) mevcut tek yerdir. Ayrıca, bu sayfa dile özgü girişin durumunu ve eklenme tarihi/saati görüntüler. Her dil / pazar için pazar ayrıntıları (teklif adı, açıklama, arama terimleri, vb) tanımlamak gerekir.

> [!NOTE]
> Teklif listesi içeriği (teklif açıklaması, belgeler, ekran görüntüleri, kullanım koşulları ve gizlilik politikası gibi) teklif açıklaması "Bu uygulama yalnızca [İngilizce olmayan dilde] olarak sunulduğu sürece İngilizce olması gerekmez." Teklif listesi içeriğinde kullanılandan başka bir dilde içerik sunmak için yararlı bir *bağlantı URL'si* sağlamak da kabul edilebilir.

### <a name="offer-listings"></a>Teklif listeleri

Teklifinizin açıklamaları ve pazarlama varlıklarının açıklamaları da dahil olmak üzere pazarda görüntülenecek ayrıntıları sağlayın.

- **Ad** (gerekli): Burada tanımlanan ad, seçtiğiniz pazarda teklif girişinizin başlığı olarak görünür. Ad, önceki **Yeni teklif** girişinize bağlı olarak önceden doldurulur. Ad ticari marka olabilir. Emojiler içeremez (ticari marka ve telif hakkı sembolleri olmadıkça) ve 50 karakterle sınırlı olmalıdır.
- **Özet** (gerekli): Teklifinizin pazar yeri giriş(ler) arama sonuçlarında kullanılmak üzere kısa bir açıklamasını sağlayın. Bu alana en fazla 100 metin karakteri girilebilir.
- **Açıklama** (gerekli): Teklifinizin pazar listesi(ler) genel bakışında görüntülenecek bir açıklamasını sağlayın. Bir değer teklifi, temel avantajlar, herhangi bir kategori veya endüstri dernekleri, uygulama içi satın alma fırsatları, gerekli açıklamaları ve daha fazla bilgi edinmek için bir bağlantı dahil etmeyi düşünün.
Bu alana işaretleme dahil olmak üzere en fazla 3.000 metin karakteri girilebilir. Ek ipuçları için, makaleye bakın [büyük bir uygulama açıklaması yazın.](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description)
- **Arama anahtar kelimeleri**: Müşterilerin teklifinizi pazarda (lar) bulmak için kullanabileceği en fazla üç arama anahtar kelimesini girin.
- **Başlangıç yönergeleri** (gerekli): Uygulamanızı potansiyel müşteriler için nasıl yapılandıracağınızı ve kullanmaya başlayacağınızı açıklayın.  Bu hızlı başlatma, daha ayrıntılı çevrimiçi belgelere bağlantılar içerebilir. Bu alana en fazla 3.000 metin karakteri girilebilir.

#### <a name="description"></a>**Açıklama**

Bu alan gereklidir. **Açıklama'ya**ekinde bulunan öğeler :

* Teklifinizin değer teklifini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın.  
* İlk birkaç cümlenin arama motoru sonuçlarında görüntülenebileceğini unutmayın.  
* Ürününüzü satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, sağladığınız değere odaklanın.  
* Sektöre özgü kelime dağarcığı veya fayda tabanlı ifadeler mümkün olduğunca kullanın.

Değer önermenizin temel bileşenleri aşağıdaki bilgileri içermelidir:

* Ürünün açıklaması.
* Üründen yararlanan kullanıcı türü.
* Müşteri ihtiyaçları veya ağrı bu ürün adresleri.

Teklifaçıklamanızı **Description** daha ilgi çekici hale getirmek için, açıklamanızı biçimlendirmek için zengin metin düzenleyicisini kullanın.

![Zengin metin düzenleyicisini kullanma](./media/text-editor2.png)

Zengin metin düzenleyicisini kullanmak için aşağıdaki yönergeleri kullanın:

- İçeriğinizin biçimini değiştirmek için, biçimlendirmek istediğiniz metni vurgulayın ve aşağıda gösterildiği gibi bir metin stili seçin:

     ![Metin biçimini değiştirmek için zengin metin düzenleyicisini kullanma](./media/text-editor3.png)

- Metne madde işaretli veya numaralanmış bir liste eklemek için aşağıdaki seçenekleri kullanın:

     ![Liste eklemek için zengin metin düzenleyicisini kullanma](./media/text-editor4.png)

- Metne girintiyi eklemek veya kaldırmak için aşağıdaki seçenekleri kullanın:

     ![Girinti için zengin metin düzenleyicisini kullanma](./media/text-editor5.png)

#### <a name="links"></a>Bağlantılar

- **Gizlilik Politikası** (gerekli): Kuruluşunuzun gizlilik politikasına bağlantı kurun. Uygulamanızın gizlilik yasalarına ve yönetmeliklerine uygun olmasını sağlamaktan ve geçerli bir gizlilik politikası sağlamaktan siz sorumlusunuz
- **CSP Programı Pazarlama Malzemeleri** (isteğe bağlı): Teklifinizi Bulut Çözüm [Sağlayıcısı (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) programına genişletmeyi seçerseniz pazarlama malzemelerine bağlantı sağlayın. CSP, CSP iş ortaklarının teklifinizi paketlemesini, pazarlamasını ve yeniden satmasını sağlayarak teklifinizi daha geniş bir nitelikli müşteri yelpazesine genişletir. Bu satıcılar teklifinizi pazarlamak için materyallere erişmeye ihtiyaç decektir. Daha fazla bilgi için, [Markete Git Hizmetleri'ne](https://partner.microsoft.com/reach-customers/gtm)bakın.
- **Yararlı Bağlantılar** (isteğe bağlı): **Uygulamanız** veya ilgili hizmetler le ilgili isteğe bağlı tamamlayıcı çevrimiçi belgeler bir Başlık ve **URL**sağlayarak listelenir. + URL **ekle'ye**tıklayarak ek yararlı bağlantılar ekleyin.

#### <a name="contact-information"></a>İletişim bilgileri

- **İlgili Kişiler**: Her müşteri iletişim itibamı için çalışan **adı,** **telefon numarası**ve **e-posta** adresi girin.  (Bunlar genel olarak *görüntülenmez).* **Destek İlgili Kişi** grubu için de bir Destek **URL'si** gereklidir.  (Bu *bilgiler* genel olarak görüntülenir).

**Destek kişisi** (gerekli): Genel destek soruları için.

**Mühendislik iletişimi** (gerekli): Teknik sorular için.

**Kanal Yöneticisi iletişim** (gerekli): CSP programı ile ilgili bayi soruları için.

#### <a name="files-and-images"></a>Dosyalar ve Görseller

- **Belgeler** (gerekli): Teklifinizle ilgili pazarlama belgelerini PDF biçiminde ekleyerek teklif başına en az bir (1) ve en fazla üç (3) belge ekleyin.
- **Resimler** (isteğe bağlı): Teklifinizin logo görüntülerinin pazar yerinde görünebileceği birden fazla yer vardır( ler), aşağıdaki boyutları gerektirir -- Küçük: 48 x 48 piksel _(gerekli),_ Orta: 90 x 90 piksel _(gerekli)_, Büyük: 216 x 216 piksel _(gerekli),_ Geniş: 255 x 115 piksel ve Kahraman: 815 x 290 piksel. Tüm görüntüler içinde olmalıdır. PNG biçimi.
- **Ekran görüntüleri** (gerekli): Teklifinizi gösteren ekran görüntüleri ekleyin. En fazla beş (5) ekran görüntüsü eklenebilir ve 1280 x 720 piksel boyutunda boyutlandırılmalıdır. Tüm görüntüler içinde olmalıdır. PNG biçimi.
- **Videolar** (isteğe bağlı): Teklifinizi gösteren videolara bağlantılar ekleyin. Müşterilere sunduğunuz teklifle birlikte gösterilen YouTube ve/veya Vimeo videolarına bağlantılar kullanabilirsiniz. Ayrıca png formatında 1280 x 720 piksel boyutunda videonun küçük resmini girmeniz gerekir. Teklif başına en fazla dört video görüntüleyebilirsiniz.

Bir sonraki bölüme geçmeden önce **Kaydetmeyi** unutmayın.

>[!Note]
>Dosya yükleme sorunu yaşıyorsanız, yerel ağınızın İş Ortağı https://upload.xboxlive.com Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="additional-marketplace-listing-resources"></a>Ek pazar yeri listeleme kaynakları

- [Pazar teklifi listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Önizleme

**Önizleme** sekmesi, teklifinizi daha geniş pazar kitleye canlı olarak yayınlamadan önce teklifinizi yayınlamak için sınırlı bir **Önizleme Hedef Kitlesi** tanımlamanıza olanak tanır.

> [!IMPORTANT]
> Teklifinizi Önizleme'de kontrol ettikten sonra, teklifinizin pazardaki herkese açık kitleye canlı olarak yayınlanabilmesi için **Canlı Yayına** Git'i seçin.

- **Önizleme Hedef Kitlesi Tanımlayın: İsteğe bağlı bir açıklamayla birlikte satır başına tek bir AAD/MSA hesabı e-postası ekleyin.**

Canlı yayımlamadan önce teklifinizi doğrulamaya yardımcı olmak için mevcut Microsoft Hesabı (MSA) veya Azure Active Directory hesapları için el ile 10 e-posta adresi veya csv dosyası yüklerken 20'ye kadar e-posta adresi ekleyin. Bu hesapları ekleyerek, teklifinizi pazar yerinde (ler) yayınlanmadan önce önizleme erişimine izin verilecek bir hedef kitle tanımlıyorsunuz. Teklifiniz zaten yayındaysa, teklifinizdeki değişiklikleri veya güncellemeleri test etmek için bir önizleme hedef kitlesi tanımlayabilirsiniz.

> [!NOTE]
> Önizleme kitlesi özel bir hedef kitleden farklıdır. Önizleme kitlesi, pazarlarda canlı olarak yayınlanmadan _önce_ teklifinize erişebilir. Ayrıca, bir plan oluşturmayı ve planı yalnızca özel bir hedef kitle nin kullanımına sunmayı da seçebilirsiniz. Plan **listeleme** sekmesinde, **Bu özel plan** onay kutusu ile özel bir hedef kitle tanımlayabilirsiniz. Ardından, Azure Kiracı tanımlarını kullanarak 20.000'e kadar müşteriden oluşan özel bir hedef kitle tanımlayabilirsiniz.

## <a name="technical-configuration"></a>Teknik yapılandırma

**Teknik yapılandırma** sekmesi, teklifinize bağlanmak için kullanılan teknik ayrıntıları (URL yolu, webhook, kiracı kimliği ve uygulama kimliği) tanımlar. Bu bağlantı, teklifinizi satın almayı seçerlerse son müşteriye sunmamızı sağlar. Toplanan alanların kullanımını açıklayan diyagramlar [SaaS yerine getirme API'leri](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2)için belgelerde mevcuttur.

- **Açılış sayfası** URL'si (gerekli): Teklifinizi pazardan aldıktan sonra müşterilerin ineceği site URL'sini tanımlayın. Bu URL, bir müşteri sayfaya yönlendirildiğinde bir belirteç alan bitiş noktası olacaktır. Bu belirteç, yerine getirme API'lerinde çözüm kullanarak ayrıntıları niçin sağlanabilir. Bu ayrıntılar ve topladığınız diğer bilgiler, kaydı tamamlamak ve satın alma işlemlerini etkinleştirmek için deneyiminizde yerleşik olan müşteri etkileşimli bir web sayfasının parçası olarak kullanılabilir.

- **Bağlantı webhook** (gerekli): Microsoft'un müşteri adına size göndermesi gereken tüm eşzamanlı olaylar için (örneğin: SaaS Aboneliği geçersiz kılmıştır), bir bağlantı webhook sağlamanızı istiyoruz. Zaten yerinde bir webhook sistemi yoksa, en basit yapılandırma herhangi bir olay için dinlemek ve daha sonra uygun (örneğin, https:\//prod-1westus.logic.azure.com:443/work) bunları ele bir HTTP Endpoint Logic App sahip olmaktır. Daha fazla bilgi için, [mantık uygulamalarında HTTP uç noktalarıyla Arama, tetikleme veya yuva iş akışlarına](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint)bakın.

- **Azure AD kiracı kimliği** (gerekli): Azure portalında, iki hizmetimiz arasındaki bağlantıyı doğrulayabilmemiz için [bir Azure Etkin Dizin (AD) uygulaması oluşturmanızı](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) zorunlu kılıyoruz. [Kiracı kimliğini](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)bulmak için Azure Etkin Dizininize gidin ve **Özellikler'i**seçin, ardından listelenen **Dizin Kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD uygulama kimliği** (gerekli): [Uygulama kimliğinize](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) ve kimlik doğrulama anahtarına da ihtiyacınız vardır. Bu değerleri almak için Azure Etkin Dizini'ne gidin ve **Uygulama kayıtlarını**seçin, ardından listelenen **Uygulama Kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). Kimlik doğrulama anahtarını bulmak için **Ayarlar'a** gidin ve **Tuşlar'ı**seçin. Bir açıklama ve süre sağlamanız gerekir ve daha sonra bir sayı değeri sağlanacaktır.

>[!Note]
>Azure uygulama kimliği yayıncı kimliğinizle ilişkilidir, bu nedenle tüm tekliflerinizde aynı uygulama kimliğinin kullanıldığından emin olun.

## <a name="plan-overview"></a>Plana genel bakış

**Plan'a genel bakış** sekmesi, aynı teklif içinde çeşitli plan seçenekleri sağlamanıza olanak tanır. Bu planlar (bazen SUS olarak da adlandırılır) sürüm, para kazanma veya hizmet katmanları açısından farklılık yaratabilir. Teklifinizi pazarda satmak için en az bir plan belirlemeniz gerekir.

Oluşturulduktan sonra, plan adlarınızı, bilgilerinizi, fiyatlandırma modellerinizi, kullanılabilirliği (Genel veya Özel), geçerli yayımlama durumunu ve kullanılabilir eylemleri görürsünüz.

Plan'a **genel bakışta** bulunan **eylemler,** planınızın geçerli durumuna bağlı olarak değişir ve şunları içerebilir:

- Plan durumu **Taslak** ise - Taslağı sil
- Plan durumu **Canlı** ise - Satış planını durdurun veya özel hedef kitleyi senkronize edin

**Yeni plan oluşturun** (Microsoft üzerinden satış yapmayı seçenler için en az bir plan)

- **Plan Kimliği:** Bu teklifteki her plan için benzersiz bir plan kimliği oluşturun. Bu kimlik, ürün URL'sindeki ve Azure Kaynak Yöneticisi şablonlarında (varsa) müşteriler tarafından görülebilir. Yalnızca küçük, alfasayısal karakterleri, tireleri veya alt çizgilerini kullanın. Bu plan kimliği için en fazla 50 karaktere izin verilir. Kimlik, oluştur'u seçtikten sonra değiştirilemez.
- **Plan adı:** Müşteriler, teklifiniz içinde hangi planı seçeceğinize karar verirken bu adı görür. Bu teklifteki her plan için benzersiz bir teklif adı oluşturun. Plan adı, aynı teklifin bir parçası olabilecek yazılım planlarını ayırt etmek için kullanılır (örneğin, Teklif adı: Windows Server; planlar: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Plan listesi

**Plan listeleme** sekmesi, planınızın mevcut olduğu dilleri (ve pazarları) görüntüler, şu anda İngilizce (ABD) kullanılabilir tek yerdir. Ayrıca, bu sayfa dile özgü girişin durumunu ve eklenme tarihi/saati görüntüler. Her dil / pazar için pazar ayrıntıları (teklif adı, açıklama, arama terimleri, vb) tanımlamak gerekir.

#### <a name="plan-listing-details"></a>Liste ayrıntılarını planla

Plan dillerinden birini seçmek, **Ad** ve Açıklama da dahil olmak üzere **plan listeleme** bilgilerini **görüntüler.**

- **Adı**: Önizlemenize göre önceden doldurulmuş **Yeni plan** girişi ve teklifinizin pazarda görüntülenen "Yazılım planı"nın başlığı olarak görünecektir.
- **Açıklama:** Bu açıklama, bu yazılım planını benzersiz kılan şeyleri ve teklifiniz dahilindeki diğer yazılım planlarından herhangi bir farkı açıklamak için bir fırsattır. 500 karaktere kadar olabilir.

Bu alanları tamamladıktan sonra **Kaydet'i** seçin.

#### <a name="plan-pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik planı

**Fiyatlandırma ve kullanılabilirlik** sekmesi, bu planın mevcut olacağı piyasaları, istenen para kazanma modelini, fiyatı ve faturalandırma terimini yapılandırmanızı sağlar. Ayrıca, planı herkes tarafından mı yoksa yalnızca belirli müşteriler (özel bir hedef kitle) tarafından mı görülebileceğini belirtebilirsiniz.

##### <a name="enabling-free-trials"></a>Ücretsiz Denemeleri Etkinleştirme

Ticari pazar aracılığıyla SaaS teklifleri, Microsoft üzerinden satış yaparken bir aylık ücretsiz deneme sürümü sağlamanıza olanak tanır. Tarifeli planlar dışındaki tüm faturamodelleri ve terimleri için ücretsiz denemeler desteklenir. Bu seçenek, müşterilerin bir aylık ücretsiz erişim için düşük bir engele sahip olmasını sağlar.  Teklifiniz dahilindeki planlar için ücretsiz deneme sürümünü etkinleştirmeyi seçerseniz, müşteri ilk bir aylık sürenin bitiminden önce ücretli aboneliğe dönüştüremez.  Bu süre zarfında, teklifinizi satın alan müşteriler, ücretsiz deneme olanağı sağlayan desteklenen planlardan herhangi birini deneyebilir ve aralarında dönüşüm sağlayabilir.  Ücretli aboneye dönüştürme dönem sonunda otomatik olarak yapılır.

>[!Note]
>Müşteri ücretsiz deneme sürümü olmadan bir plana dönüştürmeyi seçerse, dönüştürme gerçekleşir, ancak ücretsiz deneme hemen kaybolur.  Ayrıca, bir müşteri bir plan için ödeme yapmaya başladığında, ücretsiz deneme denemelerini destekleyen bir SKU'ya geçseler bile, artık aynı abonelikte ücretsiz deneme hakkı elde edemez.

Teklifinizdeki her plan için ücretsiz deneme sürümünü yapılandırma olanağı mevcuttur. Her teklif için Fiyatlandırma ve Kullanılabilirlik'e gidin ve bir aylık deneme sürümüne izin vermek için kutuyu işaretleyin.

![Bir ay ücretsiz deneme onay kutusu](./media/free-trial-enable.png)

>[!Note]
>İşlenebilir teklifiniz ücretsiz bir deneme sürümüyle yayımlandıktan sonra, bu plan için devre dışı tutulamaz. Planı yeniden oluşturmak zorunda kalmamak için ilk yayımlama için bu ayarın doğru olduğundan emin olun.

Şu anda ücretsiz deneme sürümüne katılan müşteri abonelikleri hakkında `isFreeTrial`bilgi edinmek için, doğru veya yanlış olarak işaretlenecek yeni API özelliğini kullanın. Daha fazla bilgi için [SaaS Get Abonelik API'sine](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2#get-subscription)bakın.

>[!Note]
>Ücretsiz denemeler, pazar yeri ölçüm hizmetinden yararlanan planlar için desteklenmez.

#### <a name="markets"></a>Piyasa

- **Piyasaları edin** (isteğe bağlı)

Her plan en az bir pazarda mevcut olmalıdır. Bu planı kullanıma açmak istediğiniz herhangi bir pazar konumu için onay kutusunu seçin. Microsoft'un satışları sizin adınıza havale ettiği ve vergi yi kullandığı "Vergi Havalesi" ülkeleri seçmek için bir arama kutusu ve düğmesi yardımcı olur.

Planınız için fiyatları Abd Doları (USD) olarak belirlediyseniz ve başka bir piyasa konumu eklediyseniz, yeni piyasa fiyatı geçerli döviz kurlarına göre hesaplanacaktır. Yayımlamadan önce her piyasanın fiyatını gözden geçirin. Fiyatlandırma, değişikliklerinizi kaydettikten sonra "İhracat fiyatları (xlsx)" bağlantısı kullanılarak gözden geçirilebilir.

#### <a name="pricing"></a>Fiyatlandırma

- **Fiyatlandırma modeli**: Sabit fiyat veya Koltuk bazlı

**Sabit oran:** Tek bir aylık veya yıllık fiyat sabit fiyat ile teklifinize erişimi etkinleştirin. Bu bazen site tabanlı fiyatlandırma olarak adlandırılır. Bu fiyatlandırma modeliyle, müşterileri standart olmayan birimlere göre ücretlendirmek için pazar ölçüm hizmeti API'sini kullanan tarifeli planları isteğe bağlı olarak tanımlayabilirsiniz.  Tarifeli faturalandırma hakkında daha fazla bilgi için, [pazar ölçüm hizmetini kullanarak ölçülü faturalandırmaya](./saas-metered-billing.md)bakın.

**Kullanıcı başına:** Teklife erişen veya koltukları işgal eden kullanıcı sayısına bağlı olarak fiyatla teklifinize erişimi etkinleştirin. Bu kullanıcı tabanlı model, fiyata bağlı olarak izin verilen minimum ve maksimum kullanıcı sayısını ayarlamanızı sağlar. Bu şekilde, birden çok plan yapılandırılarak kullanıcı sayısına göre farklı fiyat noktaları yapılandırılabilir.  Bu alanlar isteğe bağlıdır. Seçili bırakılmazsa, kullanıcı sayısı nın bir sınırı olmadığı (sistemin desteklebildiği kadar min ve en fazla 1 min) olarak yorumlanır. Bu alanlar, planınızın güncelleştirmesinin bir parçası olarak düzenlenebilir.

Yayımlandıktan sonra, fatura fiyatlandırma modeli seçimi değiştirilemez. Ayrıca, aynı teklifiçin tüm planlar aynı fiyatlandırma modelini paylaşmalıdır.

- **Faturadönemi**: Aylık veya Yıllık

Müşterilerin listelenen fiyatı ödemesi gereken sıklığını seçin. En az bir Aylık veya Yıllık fiyat sağlanmalıdır veya her iki seçenek de müşterilerin kullanımına sunulabilir.

- **Fiyat**: Aylık USD veya yıllık USD

USD cinsinden belirlenen fiyatlar (USD = Abd Doları) kaydedildiğinde geçerli döviz kurları kullanılarak seçilen tüm piyasaların yerel para birimine dönüştürülür. Fiyatlandırma elektronik tablosunu dışa aktararak ve her pazardaki fiyatı gözden geçirerek yayımlamadan önce bu fiyatları doğrulayın. Özel fiyatları tek bir pazarda ayarlamak istiyorsanız, fiyatlandırma elektronik tablosunu değiştirin ve içe aktarın. Bu fiyatlandırmayı doğrulamaktan siz sorumlusunuz ve bu ayarların sahibisiniz.
*\*Fiyatlandırma verilerinin dışa aktarılmasını etkinleştirmek için öncelikle fiyatlandırma değişikliklerinizi kaydetmeniz gerekir.*

Bir plan yayımlandıktan sonra nelerin değişebileceğine ilişkin bazı kısıtlamalar olduğundan, yayımlanmadan önce fiyatlarınızı dikkatle inceleyin:

- Bir plan yayımlandıktan sonra, fiyatlandırma modeli değiştirilemez.
- Bir plan için faturalandırma terimi yayımlandıktan sonra kaldırılamaz.
- Planınızdaki bir piyasa nın fiyatı yayımlandıktan sonra değiştirilemez.

### <a name="plan-audience"></a>Hedef Kitleyi Planla

Her planı herkes tarafından görülebilen veya yalnızca seçtiğiniz belirli bir hedef kitleye göre yapılandırma seçeneğiniz var. Azure AD kiracı adlarını kullanarak bu kısıtlı hedef kitleye üyelik atayabilirsiniz.

#### <a name="privacy"></a>Gizlilik

- **Bu özel bir plan (İsteğe** bağlı onay kutusu)

Planınızı yalnızca seçtiğiniz kısıtlı hedef kitle tarafından özel ve görünür hale getirmek için bu kutuyu işaretleyin. Özel bir plan olarak yayımlandıktan sonra, hedef kitleyi güncelleyebilir veya planı herkesin kullanımına sunmayı seçebilirsiniz. Bir plan herkes tarafından görülebilen olarak yayımlandıktan sonra, herkes tarafından görülebilir olarak kalmalıdır. (Plan yeniden özel plan olarak yapılandırılamaz).

- **Kısıtlı Hedef Kitle (Kiracı T'leri)**

Bu özel plana erişimi olan hedef kitleyi atayın. Erişim, atanan her kiracı kimliğinin açıklamasını ekleme seçeneğiyle birlikte kiracı kimlikleri kullanılarak atanır. Bir .csv elektronik tablo dosyası içe aktarıyorsanız en fazla 10 kiracı id'si veya 20.000 müşteri kiracı sıyrık sı eklenebilir.

Kiracı, guid (Genel Olarak Benzersiz Tanımlayıcı, kaynakları tanımlamak için kullanılan 128 bitlik bir tamsayı numarası) olarak temsil edilen bir kimliği olan bir kuruluşun temsilidir. Kuruluş veya uygulama geliştiricisi Microsoft ile bir ilişki oluşturduğunda (örneğin, Azure, Microsoft Intune veya Microsoft 365'e kaydolurken) bir kuruluşun veya uygulama geliştiricisinin aldığı özel bir Azure AD örneğidir. Her Azure AD kiracısı benzersizdir ve diğer Azure AD kiracılarından ayrıdır. Kiracıyı denetlemek için, uygulamanızı yönetirken kullanmak istediğiniz hesapla Azure portalında oturum açın. Bir kiracınız varsa, otomatik olarak bu kiracıda oturum açar ve kiracı adını doğrudan hesap adınızın altında görebilirsiniz. Adınızı, e-postanızı, dizininizi/kiracı kimliğinizi (GUID) ve etki alanınızı görmek için, Azure portalının sağ üst kısmında bulunan hesap adınızın üzerine gelin. Hesabınız birden çok kiracıyla ilişkiliyse, kiracılar arasında geçiş yapabileceğiniz bir menüyü açmak için hesap adınızı seçebilirsiniz. Her kiracının kendi kiracı kimliği vardır. Ayrıca, bir alan adı URL'si kullanarak kuruluşunuzun [https://www.whatismytenantid.com](https://www.whatismytenantid.com)kiracı kimliğine de bakabilirsiniz: .

SaaS özel bir hedef kitleyi tanımlamak için kiracı tanımlarını sunarken, diğer teklif türleri Azure Abonelik i'lerini (guid olarak da temsil edilen) kullanabilir.

> [!NOTE]
> Özel hedef kitle (veya kısıtlı hedef kitle) önizleme hedef kitlesinden farklıdır. **[Önizleme](#preview)** sekmesinde, bir önizleme hedef kitlesi tanımlayabilirsiniz. Teklifin pazarda canlı olarak yayınlanmasından *önce* bir önizleme kitlesinin teklifinize erişmesine izin verilir. Özel hedef kitle ataması yalnızca belirli bir plan için geçerli olsa da, önizleme hedef kitlesi tüm planları (özel veya özel) görüntüleyebilir, ancak plan test edilip doğrulanırken yalnızca sınırlı önizleme süresi boyunca görüntülenebilir.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Pazar teklifi nde yer alan planların örnek listesi

![Notlarla örnek pazar planı girişi](./media/marketplace-plan.svg)

## <a name="cloud-solution-provider-csp-reseller-audience"></a>Bulut Çözüm Sağlayıcısı (CSP) Bayi Kitlesi

Teklifinizi CSP programında kullanıma sunmayı seçmek, Bulut Çözüm Sağlayıcılarının ürününüzü müşterilerine birlikte sunulan bir çözümün parçası olarak satmasına olanak tanır. Daha fazla bilgi için [Bulut Çözüm Sağlayıcıları'na](https://go.microsoft.com/fwlink/?linkid=2111109)bakın.

## <a name="publish"></a>Yayımlama

Teklifin gerekli tüm bölümlerini tamamladıktan sonra, portalın sağ üst köşesinde **yayımla'yı** seçin. **İnceleme ve yayımlama** sayfasına yönlendirileceksiniz.

### <a name="submit-offer-to-preview"></a>Teklifönizlemeye gönderme

Bu teklifi ilk kez yayımlıyorsanız, şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - *Başlatılmamamıştır* - bölüme dokunulmadığı ve tamamlanması gerektiği anlamına gelir.
    - *Eksik* - bölümün düzeltilmesi gereken veya daha fazla bilgi sağlanması nı gerektiren hataları olduğu anlamına gelir. Bölüme geri dönüp güncellemeniz gerekir.
    - *Tam* - bölüm tamamlandı, gerekli tüm veriler sağlandı ve hiçbir hata yok demektir. Teklifi sunamadan önce teklifin tüm bölümlerinin tam bir durumda olması gerekir.
- Uygulamanızın anlaşılmasına yardımcı olan ek notlara ek olarak uygulamanızın doğru test edildiğinden emin olmak için sertifika ekibine test talimatları sağlayın.
- **Gönder'i**seçerek yayımlama için teklif gönderin. Teklifin önizleme sürümünün gözden geçirip onaylamanız için kullanılabilir olduğunda size bildirmek için size bir e-posta göndeririz. Teklifinizi halka (veya özel bir teklifte özel bir hedef kitleye) yayınlamak için Ortak Merkezi'ne geri dönmeli ve teklif için **Go-live'ı** seçmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
