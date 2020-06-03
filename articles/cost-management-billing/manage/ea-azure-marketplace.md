---
title: Azure Market
description: EA müşterilerinin Azure Market’i nasıl kullandığını açıklar
author: bandersmsft
ms.reviewer: baolcsva
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 06/01/2020
ms.author: banders
ms.openlocfilehash: 4721d8dbe5973baf2ac82124ff516a89398227cb
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84298202"
---
# <a name="azure-marketplace"></a>Azure Market

Bu makalede, EA müşterilerinin ve iş ortaklarının market ücretlerini nasıl görüntüleyecekleri ve Azure Market satın alımlarını nasıl etkinleştirecekleri açıklanmaktadır.

## <a name="azure-marketplace-for-ea-customers"></a>EA müşterileri için Azure Market

Doğrudan müşteriler, Azure Market ücretlerini Azure Enterprise Portal'da görüntüleyebilir. Azure Market'ten yapılan satın alma ve tüketim işlemleri, parasal taahhüdün dışında faturalanır ve üç aylık veya aylık borçlar olarak yazılır.

Dolaylı müşteriler, Azure Market aboneliklerini Azure Enterprise Portal'ın **Abonelikleri Yönet** sayfasında bulabilir ancak fiyatlar gizli olacaktır. Müşteriler, Azure Market fiyatları için Lisanslama Çözümü Sağlayıcısı (LSP) ile iletişime geçmelidir.

Yeni aylık veya yıllık yinelenen Azure Market satın alma işlemleriyle ilgili ücretlerin tamamı, Azure Market öğelerinin satın alındığı dönemde faturalanacaktır. Bu öğeler bir sonraki dönemde ilk satın alma işlemiyle aynı günde otomatik olarak yenilenecektir.

Aylık yinelenen ücretler her takvim ayının ilk gününde yenilenmeye devam edecektir. Yıllık ücretler de satın alma tarihinin yıl dönümünde yenilenecektir.

Azure Market’teki bazı üçüncü taraf satıcı hizmetleri artık Kurumsal Anlaşma (EA) parasal taahhüt bakiyenizi kullanacaktır. Önceden bu hizmetler, EA parasal taahhüdü dışında faturalandırılıyor ve faturası ayrı kesiliyordu. Azure Market’teki bu hizmetler için EA parasal taahhüdü, müşterinin satın alma ve ödeme yönetiminin kolaylaştırılmasına yardımcı olur. Parasal taahhüdü kullanmaya başlayan hizmetlerin tam listesi için lütfen [Azure web sitesindeki 6 Mart 2018 güncelleştirmesini](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment/) inceleyin.

### <a name="partners"></a>İş Ortakları

LSP'ler, Azure Enterprise Portal'ın fiyat listesi sayfasından Azure Market fiyat listesini indirebilir. Sağ üstten **Market Fiyat Listesi**'ni seçin. Azure Market fiyat listesinde kullanılabilir durumdaki tüm hizmetler ve fiyatları yer alır.

Fiyat listesini indirmek için:

1. Azure Enterprise Portal'da **Raporlar** > **Fiyat Listesi**'ni seçin.
1. Sağ üst köşede, adınızın altında yer alan Azure Market fiyat listesi bağlantısını bulun.
1. Bağlantıya sağ tıklayıp **Hedefi Farklı Kaydet**'i seçin.
1. **Kaydet** penceresinde belgenin adını `AzureMarketplacePricelist.zip` olarak değiştirin. Bunu yaptığınızda dosya .xlsx yerine .zip dosyasına dönüşür.
1. İndirme işlemi tamamlandıktan sonra ülkeye özgü fiyat listelerinin bulunduğu bir zip dosyası elde edersiniz.
1. LSP'ler ülkeye özgü fiyatlandırma için ilgili ülke dosyasına başvurmalıdır. LSP'ler yeni veya kullanımdan kaldırılan SKU'ları **Bildirimler** sekmesinden takip edebilir.
1. Fiyatlar çok sık değiştirilmez. Fiyat artışları ve döviz kuru (FX) değişiklikleri LSP'lere 30 gün öncesinden e-posta ile bildirilir.
1. LSP'lere kayıt, ISV ve üç aylık dönem başına bir fatura gönderilir.

### <a name="enabling-azure-marketplace-purchases"></a>Azure Market'ten satın alma işlemlerini etkinleştirme

Kuruluş yöneticileri, kendi kayıtları altındaki tüm Azure abonelikleri için Azure Market'ten satın alma işlemlerini devre dışı bırakabilir veya etkinleştirebilir. Kuruluş yöneticilerinin satın alma işlemlerini devre dışı bırakması ve önceden Azure Market aboneliği satın almış olan Azure aboneliklerinin mevcut olması durumunda ilgili Azure Market abonelikleri iptal edilmez ve bu ayardan etkilenmez.

Müşteriler, doğrudan Azure aboneliklerini Azure Enterprise Portal kayıtlarıyla ilişkilendirerek Azure EA aboneliğine dönüştürebilir ancak bu işlem sonucunda alt abonelikleri otomatik olarak dönüştürülmez.

Azure Market'ten satın alma işlemlerini etkinleştirmek için:

1. Azure Enterprise Portal'da kuruluş yöneticisi olarak oturum açın.
1. **Yönet**'e gidin.
1. **Kayıt Ayrıntıları**'nın altında **Azure Market** satırının yanındaki kalem simgesini seçin.
1. **Etkin/Devre Dışı** durumunu veya Ücretsiz **Yalnızca KLG SKU'ları** seçeneğini belirleyin.
1. **Kaydet**’i seçin.

> [!NOTE]
> KLG (kendi lisansını getir) ve Yalnızca Ücretsiz seçeneği, Azure Market SKU satın alma ve edinme işlemlerini KLG ve Ücretsiz SKU'lar ile sınırlar.

### <a name="services-billed-hourly-for-azure-ea"></a>Azure EA için saatlik olarak faturalanan hizmetler

Aşağıdaki hizmetler, MOSP ile aylık ücret yerine Kurumsal Anlaşma kapsamında saatlik olarak faturalandırılır:

- Uygulama Teslim Ağı
- Web Uygulaması Güvenlik Duvarı

### <a name="azure-remoteapp"></a>Azure RemoteApp

Kurumsal Anlaşmanız varsa Azure RemoteApp için Kurumsal Anlaşma fiyat düzeyinize göre ödeme yaparsınız. Ek ücret alınmaz. Standart fiyata ilk 40 saat dahildir. Sınırsız fiyata ilk 80 saat dahildir. RemoteApp, 80 saati aşan kullanımları iletmez.

## <a name="azure-marketplace-faq"></a>Azure Market Hakkında SSS

Bu bölümde Azure parasal taahhüdünüzün Azure Market'teki bazı üçüncü taraf satıcı hizmetlerine nasıl uygulanabileceği anlatılmaktadır.

### <a name="what-changed-with-azure-marketplace-services-and-ea-monetary-commitment"></a>Azure Market hizmetleri ve EA parasal taahhüt ile neler değişti?

1 Mart 2018 itibarıyla bazı üçüncü taraf hizmetler EA parasal taahhüdünü (MC) kullanmaya başladı. Azure ayrılmış sanal makine örnekleri (RI) dışındaki hizmetler önceden EA parasal taahhüdünden ayrı bir şekilde faturalandırılıyordu.

Parasal taahhüt kullanım kapsamını Azure Market'te yayımlanmış olan ve sıklıkla satın alınan bazı üçüncü taraf hizmetleri kapsayacak şekilde genişlettik. Azure Market’teki bu hizmetler için EA parasal taahhüdü, satın alma ve ödeme yönetiminin kolaylaştırılmasına yardımcı olur.

### <a name="why-did-we-make-this-change"></a>Bu değişikliği neden yaptık?

Müşteriler, peşin parasal taahhüt ödemesini kullanabilecekleri farklı yollar arıyordu. Bu, müşteriler tarafından sıklıkla istenen bir değişiklikti ve Azure Market müşterilerinin büyük bir bölümünü etkiledi.

### <a name="how-do-you-benefit"></a>Nasıl faydalanabilirsiniz?

Daha basit bir faturalandırma deneyimine sahip olabilir ve EA parasal taahhüdünüzü daha iyi harcayabilirsiniz. Bu hizmetler peşin parasal taahhüt ödemenize dahil olduğundan EA parasal taahhüdünüz daha değerli hale gelir.

### <a name="what-azure-marketplace-services-use-ea-monetary-commitment-and-how-do-i-know"></a>EA parasal taahhüdünü kullanan Azure Market hizmetlerini nasıl tespit edebilirim?

Parasal taahhüt kullanan bir hizmet satın aldığınızda Azure Market'te bir sorumluluk reddi görüntülenir. Red Hat, SUSE, Autodesk ve Oracle tarafından yayımlanmış bazı hizmetler desteklenmektedir. Şu anda başkaları tarafından yayımlanan benzer ada sahip hizmetler parasal taahhüt kullanmamaktadır. Hizmetlerin tam listesini bu SSS sayfasının sonunda bulabilirsiniz.

### <a name="what-if-my-ea-monetary-commitment-runs-out"></a>EA parasal taahhüdüm biterse ne olacak?

Parasal taahhüdünüzü bitirip fazla kullanıma geçerseniz bu hizmetlerle ilgili ücretler, diğer tüketilen hizmetlerle birlikte bir sonraki fazla kullanım faturanıza yansıtılacaktır. 1 Mart 2018 tarihinden önce bu ücretler diğer Azure Market hizmetleriyle birlikte faturalandırılıyordu.

### <a name="why-dont-all-azure-marketplaces-consume-ea-monetary-commitment"></a>Neden tüm Azure Market hizmetleri EA parasal taahhüdünü kullanmıyor?

EA parasal taahhüdü ile ilgili en iyi müşteri deneyimi sunmak için düzenli güncelleştirmeler yapıyoruz. Bu değişiklik, çok sayıda müşteriyi ve Azure Market harcamalarının önemli bir miktarını etkileyecek. İleride farklı hizmetler de eklenebilir.

### <a name="how-does-this-impact-indirect-enrollment-and-partners"></a>Bu durum dolaylı kayıtları ve iş ortaklarını nasıl etkiliyor?

Dolaylı kayıt müşterileri veya iş ortakları bu durumdan etkilenmeyecektir. Bu hizmetler, diğer tüketim hizmetleriyle aynı iş ortağı kar payı özelliklerine sahiptir. Buradaki tek değişiklik, ücretlerin farklı bir faturada görünmesi ve ücretlerin ödemesinin müşterinin EA parasal taahhüdü ile yapılmasıdır.

### <a name="is-there-a-list-of-azure-marketplace-services-that-consume-ea-monetary-commitment"></a>EA parasal taahhüdünü kullanan Azure Market hizmetlerinin olduğu bir liste var mı?

Belirli Azure Market teklifleri, parasal taahhüt fonlarını kullanabilir. Bu programa katılan ürünlerin tam listesi için [parasal taahhüt kullanan üçüncü taraf hizmetler](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment) sayfasına bakın.


## <a name="next-steps"></a>Sonraki adımlar

- [Fiyatlandırma](ea-pricing-overview.md) hakkında daha fazla bilgi edinin.