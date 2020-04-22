---
title: Bulut Çözüm Sağlayıcıları | Azure Marketi
description: Yayıncılar artık tekliflerini Microsoft Cloud Solution Provider (CSP) iş ortağı kanalı aracılığıyla satabilir.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/16/2020
ms.author: dsindona
ms.openlocfilehash: c4c8f21234cfbf040d57c0e1c8dbecbb698d7d50
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685919"
---
# <a name="cloud-solution-providers"></a>Bulut Çözüm Sağlayıcıları

Yazılım teklifleri, [Microsoft web storefronts](https://docs.microsoft.com/azure/marketplace/comparing-appsource-azure-marketplace)aracılığıyla tekliflerin genel kullanılabilirliğine ek olarak, Bulut Çözüm Sağlayıcısı (CSP) programındaki iş ortakları tarafından sunulan milyonlarca nitelikli Microsoft müşterisine de ulaşabilir.

Yayıncılar, csp programında kullanılabilirlik için teklifleri tercih bazında, yeni bir teklif veya mevcut bir teklif için yapılandırarak iş ortaklarının ürünlerinizi satmasını ve müşteriler için paket çözümler oluşturmasına olanak sağlar.

Yayıncılar, son müşterilere kesme düzeltme desteği sağlamakve CSP programındaki iş ortaklarının ve/veya müşterilerinin destek için sizinle iletişim kurmaları için bir mekanizma sağlamakla yükümlüdür. CSP programındaki iş ortaklarına kullanıcı belgeleri, eğitim ve hizmet durumu/kesinti bildirimleri (geçerli olduğu şekilde) sağlamak en iyi uygulamadır, böylece CSP programındaki iş ortakları müşterilerden gelen tier 1 destek isteklerini karşılayacak donanıma sahip olur.  

Aşağıdaki teklifler, CSP programındaki ortaklar tarafından satılmak üzere tercih edilebilir:

- Yazılım-as-a-Service (SaaS) işlem teklifleri
- Sanal Makineler (VM)
- Çözüm şablonları
- Yönetilen uygulamalar

> [!NOTE]
> Konteynerler ve Bring Your Own License (BYOL) VM SKUs varsayılan olarak CSP programında ortakları tarafından satılmak üzere tercih edilir.

## <a name="how-to-configure-an-offering"></a>Bir teklif nasıl yapılandırılabilen

CSP programı tercih ayarı İş Ortağı Merkezi'nde yapılandırılır veya Bulut İş Ortağı Portalı oluşturma deneyimi sunar. [Değişen yayımcı deneyimi hakkında daha fazla bilgi edinin.](https://www.microsoftpartnercommunity.com/t5/Azure-Marketplace-and-AppSource/Cloud-Marketplace-In-Partner-Center/m-p/9738#M293)

### <a name="partner-center-opt-in"></a>İş Ortağı Merkezi'ni tercih edin

İş Ortağı Merkezi'nde, CSP Bayi hedef kitle modülü altında tercih deneyimini bulacaksınız.

![CSP Bayi kitlesi](media/marketplace-publishers-guide/csp-reseller-audience.png)

CSP Bayi hedef kitle modülünde, aralarından seçim yapabileceğiniz üç seçenek vardır:

- Seçenek bir: CSP programındaki herhangi bir ortak
- Seçenek iki: Seçtiğim CSP programında belirli ortaklar
- Üçüncü seçenek: CSP programında ortak yok

#### <a name="option-one-any-partner-in-the-csp-program"></a>Seçenek bir: CSP programındaki herhangi bir ortak

![CSP programındaki herhangi bir ortak](media/marketplace-publishers-guide/csp-reseller-option-one.png)

 Bu seçeneği seçerek, CSP programındaki tüm iş ortakları teklifinizi müşterilerine yeniden satabilecektir.

#### <a name="option-two-specific-partners-in-the-csp-program-i-select"></a>Seçenek iki: Seçtiğim CSP programında belirli ortaklar

![Seçtiğim CSP programında belirli ortaklar](media/marketplace-publishers-guide/csp-reseller-option-two.png)

Bu seçeneği seçerek, CSP programındaki hangi ortakların teklifinizi yeniden satmaya uygun olduğuna yetki verebilirsiniz.

İş ortaklarını yetkilendirmek için **CSP İş Ortaklarını Seçin'i** tıklatın ve iş ortağı adına veya CSP Azure Etkin Dizin (AAD) kiracı kimliğine göre arama yapmanızı sağlayan bir menü görüntülenir.

![CSP menüsünü seçin](media/marketplace-publishers-guide/csp-pop-up-module.png)

**Ülke,** **Yetkinlik**veya **Beceri**gibi arama filtrelerini uygulayabilirsiniz.

![İş ortağı araması için ülke, yetkinlik ve beceri filtreleri](media/marketplace-publishers-guide/csp-add-resellers.png)

Ortaklar listesini seçtikten sonra **Ekle'yi**seçin.

![CSP programındaki yetkili ortakların örnek listesi](media/marketplace-publishers-guide/csp-add-resellers-details.png)

Seçtiğiniz iş ortaklarının listesini gösteren bir tablo CSP Bayi hedef kitle sayfasında görünür.

![CSP Bayi hedef kitle sayfasında iş ortaklarının listesi olan tablo](media/marketplace-publishers-guide/csp-option-two-add-reseller.png)

Değişikliklerinizi kaydetmek için **taslağı Kaydet'i** seçin.

Bu teklif yayınlanmamışsa, seçili iş ortaklarınızın kullanımına sunmak için teklifinizi yayınlamanız gerekir.

>[!NOTE]
>Belirli bir bölgedeki CSP programındaki bir ortağa yetki verilirse, teklifi o bölgeye ait olan herhangi bir müşteriye satabilirler. CSP tekliflerinin bölgeler altında nasıl sınıflandırış ları hakkında daha fazla bilgi için [bölgesel pazarlar ve para birimi](https://docs.microsoft.com/partner-center/regional-authorization-overview) için Bulut Çözüm Sağlayıcısı programına bakın.

Zaten yayınlanmış bir teklifin CSP listesini güncelliyorsanız, ek iş ortakları ekleyin ve **Eşitleme CSP hedef kitlesini**seçin.

Yetkili ortakların listesi zaten olan bir teklifiniz varsa ve aynı listeyi başka bir teklif için kullanmak istiyorsanız, **Dışa Aktarma/Dışa Aktarma'yı**kullanın. CSP listesi olan teklife gidin ve **Dışa Aktarma CSP'lerini**seçin. İşlev, başka bir teklife aktarılabilen bir .csv dosyası geliştirir.

#### <a name="option-three-no-partners-in-the-csp-program"></a>Üçüncü seçenek: CSP programında ortak yok

![CSP programında ortak yok](media/marketplace-publishers-guide/csp-reseller-option-three.png)

Bu seçeneği seçerek, teklifinizi CSP programından çıkararak seçmiyoruz. Bu seçimi istediğiniz zaman değiştirebilirsiniz.

### <a name="cloud-partner-portal-opt-in"></a>Bulut İş Ortağı Portalı tercih

Cloud İş Ortağı Portalı'nda, tercih market veya storefront sekmesinde ayarlanır. CSP programında belirli iş ortaklarını seçme olanağı yalnızca İş Ortağı Merkezi'nde kullanılabilir.

![CPP'de CSP tercih deneyimi](media/marketplace-publishers-guide/csp-opt-in.png)

## <a name="deauthorize-partners-in-the-csp-program"></a>CSP programındaki İş Ortaklarını Yetkilendirme

CSP programında bir iş ortağına yetki verdiyseniz ve bu iş ortağı ürünü müşterilerine zaten sattıysa, bu ortağın yetkisini devreden izin verilmez.

CSP programındaki bir iş ortağı ürününüzü müşterilerine satmadıysa ve teklifiniz yayınlandıktan sonra CSP'yi kaldırmak istiyorsanız aşağıdaki yönergeleri kullanın:

1. [Destek isteği sayfasına](https://aka.ms/marketplacepublishersupport)gidin. İlk birkaç açılır menü ler sizin için otomatik olarak doldurulur.

   > [!NOTE]
   > Önceden doldurulmuş açılır menü seçimlerini değiştirmeyin.

2. **Ürün sürümünü seçin**, Canlı teklif **yönetimini**seçin.
3. **Sorunu en iyi açıklayan kategori seçin,** teklifinizi ifade eden kategoriyi seçin.
4. **Sorunu en iyi açıklayan bir sorunu seçin** **için, varolan teklifi güncelleştir'i**seçin.
5. Sorununla ilgili daha fazla ayrıntı girmek için **Sorun ayrıntıları sayfasına** yönlendirilecek **İleri'yi** seçin.
6. Konu başlığı olarak **CSP'yi Deauthorize'u** kullanın ve gerekli bölümlerin geri kalanını doldurun.




## <a name="navigate-between-options"></a>Seçenekler arasında gezinme

Üç CSP bayi seçeneği arasında gezinmek için bu bölümü kullanın.

### <a name="navigate-from-option-one-any-partner-in-the-csp-program"></a>Seçenek 1'den gidin: CSP programındaki tüm iş ortağı

Teklifiniz şu anda **Seçenek 1: CSP programındaki herhangi bir iş ortağıysanız** ve diğer iki seçenekten birine gitmek istiyorsanız, bir istek oluşturmak için aşağıdaki yönergeleri kullanın:

1. [Destek isteği sayfasına](https://aka.ms/marketplacepublishersupport)gidin. İlk birkaç açılır menü ler sizin için otomatik olarak doldurulur.

   > [!NOTE]
   > Önceden doldurulmuş açılır menü seçimlerini değiştirmeyin.

2. **Ürün sürümünü seçin**, Canlı teklif **yönetimini**seçin.
3. **Sorunu en iyi açıklayan kategori seçin,** teklifinizi ifade eden kategoriyi seçin.
4. **Sorunu en iyi açıklayan bir sorunu seçin** **için, varolan teklifi güncelleştir'i**seçin.
5. Sorununla ilgili daha fazla ayrıntı girmek için **Sorun ayrıntıları sayfasına** yönlendirilecek **İleri'yi** seçin.
6. Konu başlığı olarak **CSP'yi Deauthorize'u** kullanın ve gerekli bölümlerin geri kalanını doldurun.

> [!NOTE]
> Seçenek 2'ye gitmeye çalışıyorsanız ve CSP programındaki bir iş ortağı teklifi müşterilerine zaten sattıysa, bu iş ortağı varsayılan olarak yetkili iş ortakları listenizde zaten yer alır.  

### <a name="navigate-from-option-two-specific-partners-in-the-csp-program-i-select"></a>Seçenek 2'den gidin: Seçtiğim CSP programında belirli ortaklar

Teklifiniz şu anda **Seçenek 2 ise: Seçtiğim CSP programında belirli ortaklar** ve Seçenek 1'e gitmek **istiyorsanız: CSP programındaki herhangi bir iş ortağı,** bir istek oluşturmak için aşağıdaki yönergeleri kullanın:

1. [Destek isteği sayfasına](https://aka.ms/marketplacepublishersupport)gidin. İlk birkaç açılır menü ler sizin için otomatik olarak doldurulur.

   > [!NOTE]
   > Önceden doldurulmuş açılır menü seçimlerini değiştirmeyin.

2. **Ürün sürümünü seçin**, Canlı teklif **yönetimini**seçin.
3. **Sorunu en iyi açıklayan kategori seçin,** teklifinizi ifade eden kategoriyi seçin.
4. **Sorunu en iyi açıklayan bir sorunu seçin** **için, varolan teklifi güncelleştir'i**seçin.
5. Sorununla ilgili daha fazla ayrıntı girmek için **Sorun ayrıntıları sayfasına** yönlendirilecek **İleri'yi** seçin.
6. Konu başlığı olarak **CSP'yi Deauthorize'u** kullanın ve gerekli bölümlerin geri kalanını doldurun.

 Teklifiniz şu anda **Seçenek 2: Seçtiğim CSP programındaki belirli iş ortakları** ysa ve Seçenek **3:CSP programında hiçbir iş ortağına**gitmek istiyorsanız, yalnızca daha önce yetki vermiş olduğunuz CSP programındaki iş ortakları son müşterilere teklifinizi yeniden satmamışsa bu seçenede gidebilirsiniz. Bir istek oluşturmak için lütfen aşağıdaki yönergeleri kullanın:

1. [Destek isteği sayfasına](https://aka.ms/marketplacepublishersupport)gidin. İlk birkaç açılır menü ler sizin için otomatik olarak doldurulur.

   > [!NOTE]
   > Önceden doldurulmuş açılır menü seçimlerini değiştirmeyin.

2. **Ürün sürümünü seçin**, Canlı teklif **yönetimini**seçin.
3. **Sorunu en iyi açıklayan kategori seçin,** teklifinizi ifade eden kategoriyi seçin.
4. **Sorunu en iyi açıklayan bir sorunu seçin** **için, varolan teklifi güncelleştir'i**seçin.
5. Sorununla ilgili daha fazla ayrıntı girmek için **Sorun ayrıntıları sayfasına** yönlendirilecek **İleri'yi** seçin.
6. Konu başlığı olarak **CSP'yi Deauthorize'u** kullanın ve gerekli bölümlerin geri kalanını doldurun.

### <a name="navigate-from-option-3-no-partners-in-the-csp-program"></a>Seçenek 3'ten gidin: CSP programında ortak yok

Teklifiniz şu anda **Seçenek 3 ise: CSP programında ortak**yoksa, istediğiniz zaman diğer iki seçenekten birine gidebilirsiniz.

## <a name="sharing-sales-and-support-materials-with-partners-in-the-csp-program"></a>CSP programında satış ve destek malzemelerinin iş ortaklarıyla paylaşılması

Bulut Çözüm Sağlayıcısı programındaki iş ortaklarına teklifinizi en etkili şekilde temsil etmek ve kuruluşunuzla etkileşimde bulunmak için, satıcıların kullanabileceği satış ve destek materyalleri göndermeniz gerekir. Bu kaynaklar pazar vitrinlerinde müşterilere maruz olmayacaktır.

### <a name="partner-center-csp-channel"></a>Ortak Merkezi CSP kanalı

İş Ortağı Merkezi'ndeki CSP kanalını seçtiyseniz, yayıncılarTeklif listeleme modülü altında ilgili pazarlama malzemelerini ve kanal iletişim bilgilerini CSP kanalına barındıran bir URL girmeli:

![Ortak Merkezi CSP teminat bilgileri](media/marketplace-publishers-guide/pc-csp-channel.png)

### <a name="cloud-partner-portal-csp-channel"></a>Bulut İş Ortağı Portalı CSP kanalı

Cloud Partner Portal'daki CSP kanalını seçtiyseniz, yayıncıların ilgili pazarlama malzemelerini barındıran bir URL ve CSP kanalına kanal iletişim bilgilerini içeren bir URL girmeleri gerekir:

![Bulut İş Ortağı Portalı CSP teminat bilgileri](media/marketplace-publishers-guide/cpp-csp-information.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure [Marketi ve AppSource Publisher Kılavuzu'nu ziyaret edin.](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide)

Pazar daki GTM hizmetleri hakkında daha fazla bilgi edinmek [için pazara git hizmetleri'ne](https://partner.microsoft.com/reach-customers/gtm)bakın.

Teklifinizi oluşturmak ve yapılandırmak için [İş Ortağı Merkezi](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) veya Bulut İş Ortağı [Portalı'nda](https://cloudpartner.azure.com/) oturum açın.
