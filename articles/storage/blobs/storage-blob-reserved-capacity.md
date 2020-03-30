---
title: Ayrılmış kapasiteyle Blob depolama maliyetlerini optimize edin - Azure Depolama
description: Blok blob ve Azure Veri Gölü Depolama Gen2 kaynaklarında maliyetlerden tasarruf etmek için Azure Depolama rezerve edilmiş kapasite satın alma hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: e73686629de8481f6a37e5bfafc9b723206b4853
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351024"
---
# <a name="optimize-costs-for-blob-storage-with-reserved-capacity"></a>Ayrılmış kapasite ile Blob depolama maliyetlerini iyileştirme

Azure Depolama ayrılmış kapasite ile blob verileri için depolama maliyetlerinde tasarruf edebilirsiniz. Azure Depolama rezerve edilmiş kapasite, bir yıl veya üç yıl boyunca bir rezervasyon yaptığınızda standart depolama hesaplarındaki blok blob'lar ve Azure Veri Gölü Depolama Gen2 verileri için kapasitede indirim sunar. Rezervasyon, rezervasyon süresi için sabit bir depolama kapasitesi sağlar.

Azure Depolama ayrılmış kapasite, blok blob'ları ve Azure Veri Gölü Depolama Gen2 verileri için kapasite maliyetlerinizi önemli ölçüde azaltabilir. Elde edilen maliyet tasarrufu, rezervasyonunuzun süresine, rezerve etmeyi seçtiğiniz toplam kapasiteye ve depolama hesabınız için seçtiğiniz erişim katmanına ve artıklık türüne bağlıdır. Ayrılmış kapasite bir fatura indirimi sağlar ve Azure Depolama kaynaklarınızın durumunu etkilemez.

Azure Depolama ayrılmış kapasitesi fiyatları hakkında daha fazla bilgi için bkz. [Blok blobu fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/) ve [Azure Data Lake Storage 2. Nesil fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/data-lake/).

## <a name="reservation-terms-for-azure-storage"></a>Azure Depolama için rezervasyon koşulları

Aşağıdaki bölümlerde Azure Depolama rezervasyonunun koşulları açıklanmaktadır.

### <a name="reservation-capacity"></a>Rezervasyon kapasitesi

Bir yıllık veya üç yıllık bir dönem için ayda 100 TB ve 1 PB'lik birimlerde Ayrılmış Azure Depolama kapasitesini satın alabilirsiniz.

### <a name="reservation-scope"></a>Rezervasyon kapsamı

Azure Depolama rezerve edilen kapasite, tek bir abonelik veya birden çok abonelik (paylaşılan kapsam) için kullanılabilir. Tek bir aboneye uygulandığında, rezervasyon indirimi yalnızca seçili aboneye uygulanır. Birden çok aboneye dahil edildiğinde, rezervasyon indirimi müşterinin faturalandırma bağlamında bu abonelikler arasında paylaşılır.

Azure Depolama ayrılmış kapasitesatın aldığınızda, hem blok blob hem de Azure Veri Gölü Depolama Gen2 verileri için rezervasyonunuzu kullanabilirsiniz. Rezervasyon, satın alınan kapsamdaki kullanımınıza uygulanır ve abonelik içindeki belirli bir depolama hesabı, konteyner veya nesneyle sınırlandırılamaz. Rezervasyon birden çok abonelik arasında bölünemez.

Azure Depolama rezervasyonu yalnızca bir abonelik veya paylaşılan kaynak grubunda depolanan veri miktarını kapsar. Erken silme, işlemler, bant genişliği ve veri aktarım ücretleri rezervasyona dahil edilmez. Bir rezervasyon satın alır almaz, rezervasyon özelliklerine uyan kapasite ücretleri, kullandıkça öde fiyatları yerine indirimli fiyatlarüzerinden ücretlendirilir. Azure rezervasyonları hakkında daha fazla bilgi için azure [rezervasyonları nelerdir?](/azure/billing/billing-save-compute-costs-reservations)

### <a name="supported-account-types-tiers-and-redundancy-options"></a>Desteklenen hesap türleri, katmanlar ve artıklık seçenekleri

Azure Depolama ayrılmış kapasite, genel amaçlı v2 (GPv2) ve Blob depolama hesapları da dahil olmak üzere standart depolama hesaplarındaki kaynaklar için kullanılabilir.

Tüm erişim katmanları (sıcak, serin ve arşiv) rezervasyonlar için desteklenir. Erişim katmanları hakkında daha fazla bilgi için [Azure Blob depolama alanına bakın: sıcak, serin ve arşiv erişim katmanları.](storage-blob-storage-tiers.md)

Rezervasyonlar için her türlü fazlalık desteklenir. Artıklık seçenekleri hakkında daha fazla bilgi için [Azure Depolama artıklığı'na](../common/storage-redundancy.md)bakın.

> [!NOTE]
> Azure Depolama rezerve edilen kapasite, premium depolama hesapları, genel amaçlı v1 (GPv1) depolama hesapları, Azure Veri Gölü Depolama Gen1, sayfa blobs, Azure Sıra depolama, Azure Tablo depolama veya Azure Dosyaları için kullanılamaz.  

### <a name="security-requirements-for-purchase"></a>Satın alma için güvenlik gereksinimleri

Ayrılmış kapasite satın almak için:

- Kullandıkça öde oranlarıyla en az bir Kurumsal veya bireysel abonelik için **Sahip** rolünde olmalısınız.
- Kurumsal abonelikler için **Ayrılmış Örnekler Ekle'nin** EA portalında etkinleştirilmesi gerekir. Veya bu ayar devre dışı bırakılmışsa, abonelikte bir EA Yöneticisi olmalısınız.
- Bulut Çözüm Sağlayıcısı (CSP) programı için yalnızca yönetici aracılar veya satış temsilcileri Azure Blob Depolama ayrılmış kapasite satın alabilir.

## <a name="determine-required-capacity-before-purchase"></a>Satın almadan önce gerekli kapasiteyi belirleme

Bir Azure Depolama rezervasyonu satın aldığınızda, rezervasyon için bölge, erişim katmanı ve artıklık seçeneğini seçmeniz gerekir. Rezervasyonunuz yalnızca o bölgede depolanan veriler, erişim katmanı ve artıklık düzeyi için geçerlidir. Örneğin, bölge yedekli depolama (ZRS) kullanarak sıcak katman için ABD Batı'da veri için bir rezervasyon satın aldığınızı varsayalım. Aynı rezervasyonu ABD Doğu'sundaki veriler, arşiv katmanındaki veriler veya coğrafi yedekli depolama (GRS) verileri için kullanamazsınız. Ancak, ek ihtiyaçlarınız için başka bir rezervasyon satın alabilirsiniz.  

Rezervasyonlar 1 00 TB veya 1 PB bloklar için bugün mevcuttur, 1 PB blok lar için daha yüksek indirimler ile. Azure portalında bir rezervasyon satın aldığınızda, Microsoft, hangi rezervasyonu satın almanız gerektiğini belirlemenize yardımcı olmak için önceki kullanımınıza dayalı öneriler sunabilir.

## <a name="purchase-azure-storage-reserved-capacity"></a>Azure Depolama'yı satın alma için ayrılmış kapasite

[Azure portalı](https://portal.azure.com)üzerinden Azure Depolama rezerve edilen kapasiteyi satın alabilirsiniz. Peşin olarak veya aylık ödemelerle rezervasyon ödemesi yapın. Aylık ödemelerle satın alma hakkında daha fazla bilgi için, [ön veya aylık ödemelerle Azure Satın Alma rezervasyonları](/azure/billing/billing-monthly-payments-reservations)satın alın.

Senaryonuz için doğru olan rezervasyon koşullarını belirleme konusunda yardım için [bkz.](../../cost-management-billing/reservations/understand-storage-charges.md)

Ayrılmış kapasite satın almak için aşağıdaki adımları izleyin:

1. Azure portalındaki [Satınalma rezervasyonları](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/Browse_AddCommand) bölmesine gidin.  
1. Yeni bir rezervasyon satın almak için **Azure Blob Depolama'yı** seçin.  
1. Aşağıdaki tabloda açıklandığı gibi gerekli alanları doldurun:

    ![Ayrılmış kapasitenin nasıl satın alındığını gösteren ekran görüntüsü](media/storage-blob-reserved-capacity/select-reserved-capacity.png)

   |Alan  |Açıklama  |
   |---------|---------|
   |**Kapsam**   |  Rezervasyonla ilişkili faturaavantajını kaç aboneliğin kullanabileceğini gösterir. Ayrıca, rezervasyonun belirli aboneliklere nasıl uygulandığını da denetler. <br/><br/> **Paylaşılan'u**seçerseniz, rezervasyon indirimi faturalandırma bağlamınızdaki herhangi bir abonelikte Azure Depolama kapasitesine uygulanır. Faturalandırma bağlamı, Azure'a nasıl kaydolduğunuza bağlıdır. Kurumsal müşteriler için paylaşılan kapsam kayıttır ve kayıt içindeki tüm abonelikleri içerir. Kullandıkça öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan kullandıkça öde oranlarına sahip tüm bireysel abonelikleri içerir.  <br/><br/>  **Tek abonelik**seçerseniz, rezervasyon indirimi seçili abonelikteki Azure Depolama kapasitesine uygulanır. <br/><br/> **Tek kaynak grubu**seçerseniz, rezervasyon indirimi seçili abonelikteki Azure Depolama kapasitesine ve bu abonelik içindeki seçili kaynak grubuna uygulanır. <br/><br/> Rezervasyonu satın aldıktan sonra rezervasyon kapsamını değiştirebilirsiniz.  |
   |**Abonelik**  | Azure Depolama rezervasyonu için ödeme yapmak için kullanılan abonelik. Seçilen abonelikteki ödeme yöntemi, maliyetlerin tahsilinde kullanılır. Abonelik aşağıdaki türlerden biri olmalıdır: <br/><br/>  Kurumsal Sözleşme (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P): Kurumsal abonelik için ücretler kaydın parasal taahhüt bakiyesinden düşülür veya fazla lık olarak tahsil edilir. <br/><br/> Kullandıkça öde oranlarına sahip bireysel abonelik (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P): Kullandıkça öde oranlarına sahip bireysel abonelik için ücretler abonelikteki kredi kartına veya fatura ödeme yöntemine faturalandırılır.    |
   | **Bölge** | Rezervasyonun geçerli olduğu bölge. |
   | **Erişim katmanı** | Rezervasyonun geçerli olduğu erişim katmanı. Seçenekler *sıcak,* *serin*veya *Arşiv*içerir. Erişim katmanları hakkında daha fazla bilgi için [Azure Blob depolama alanına bakın: sıcak, serin ve arşiv erişim katmanları.](storage-blob-storage-tiers.md) |
   | **Yedeklilik** | Rezervasyon için fazlalık seçeneği. Seçenekler arasında *LRS,* *ZRS,* *GRS*ve *RA-GZRS*bulunmaktadır. Artıklık seçenekleri hakkında daha fazla bilgi için [Azure Depolama artıklığı'na](../common/storage-redundancy.md)bakın. |
   | **Faturalama sıklığı** | Rezervasyon için hesabın ne sıklıkta faturalandırıldırıdığını gösterir. Seçenekler *arasında Aylık* veya *Peşin.* |
   | **Boyut** | Rezervasyonun geçerli olduğu bölge. |
   |**Terim**  | Bir ya da üç yıl.   |

1. Rezervasyonunuz için parametreleri seçtikten sonra, Azure portalı maliyeti görüntüler. Portal ayrıca, istediğiniz kadar öde faturalandırmaüzerinden indirim yüzdesini de gösterir.

1. **Satınalma rezervasyonları** bölmesinde, rezervasyonun toplam maliyetini gözden geçirin. Rezervasyon için bir ad da verebilirsiniz.

    ![Rezervasyon satın alma işlemini gösteren ekran görüntüsü](media/storage-blob-reserved-capacity/purchase-reservations.png)

Bir rezervasyon satın aldıktan sonra, rezervasyon koşullarına uyan varolan Azure Depolama bloğu blob veya Azure Veri Gölü Depolama Gen2 kaynaklarına otomatik olarak uygulanır. Henüz herhangi bir Azure Depolama kaynağı oluşturmadıysanız, rezervasyon koşullarına uyan bir kaynak oluşturduğunuzda rezervasyon geçerli olacaktır. Her iki durumda da, rezervasyon süresi başarılı bir satın alma işleminden hemen sonra başlar.

## <a name="exchange-or-refund-a-reservation"></a>Bir rezervasyonu değiştirme veya iade

Belirli sınırlamalarla bir rezervasyonu değiştirebilir veya iade edebilirsiniz. Bu sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

Bir rezervasyonu değiştirmek veya iade etmek için Azure portalındaki rezervasyon ayrıntılarına gidin. **Değişim** veya **İade**seçeneğini belirleyin ve destek isteği göndermek için yönergeleri izleyin. İstek işlendiğinde, Microsoft isteğin tamamlandığını onaylamak için size bir e-posta gönderir.

Azure Rezervasyonları ilkeleri hakkında daha fazla bilgi için, [Azure Rezervasyonları için Self servis değişimleri ve para iadesi](../../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md)ne redelenir.

### <a name="exchange-a-reservation"></a>Rezervasyon değişimi

Rezervasyon değişimi, rezervasyonun kullanılmayan kısmına bağlı olarak eşit olarak geri ödeme almanızı sağlar. Daha sonra geri ödemeyi yeni bir Azure Depolama rezervasyonunun satın alma fiyatına uygulayabilirsiniz.

Yapabileceğiniz değişim sayısında bir sınır yoktur. Ayrıca, bir değişim ile ilgili bir ücret yoktur. Satın aldığınız yeni rezervasyon, orijinal rezervasyondaki eşit veya daha değerli olmalıdır. Azure Depolama rezervasyonu, başka bir Azure hizmeti için rezervasyon için değil, yalnızca başka bir Azure Depolama rezervasyonuyla değiştirilebilir.

### <a name="refund-a-reservation"></a>Rezervasyonu iade edin

Bir Azure Depolama rezervasyonunuzun iptalini istediğiniz zaman iptal edebilirsiniz. İptal ettiğinizde, rezervasyonun kalan süresine göre yüzde 12 erken fesih ücreti hariç eşit bir geri ödeme alırsınız. Yıllık maksimum geri ödeme 50.000 $ olduğunu.

Bir rezervasyonu iptal etmek rezervasyonu derhal sona erdirir ve kalan ayları Microsoft'a iade eder. Kalan eşit eşit lenmiş bakiye, ücret hariç, orijinal satın alma formunuza iade edilecektir.

## <a name="expiration-of-a-reservation"></a>Rezervasyonun sona ermesi

Bir rezervasyonun süresi dolduğunda, bu rezervasyon altında kullanmakta olduğunuz azure depolama kapasitesi kullandıkça öde fiyatına faturalandırılır. Rezervasyonlar otomatik olarak yenilenmez.

Rezervasyonun sona ermesinden 30 gün önce ve son kullanma tarihinden 30 gün önce bir e-posta bildirimi alacaksınız. Bir rezervasyonun sağladığı maliyet tasarruflarından yararlanmaya devam etmek için, son kullanma tarihinden sonra yenilemek.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız varsa, [bir destek isteği oluşturun.](https://go.microsoft.com/fwlink/?linkid=2083458)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Rezervasyonları nedir?](../../cost-management-billing/reservations/save-compute-costs-reservations.md)
- [Rezervasyon indiriminin Azure Depolama'ya nasıl uygulandığını anlama](../../cost-management-billing/reservations/understand-storage-charges.md)
