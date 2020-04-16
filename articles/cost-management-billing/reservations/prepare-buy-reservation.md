---
title: Azure rezervasyonu satın alma
description: Azure rezervasyonu satın almanıza yardımcı olan önemli noktalar hakkında bilgi edinin.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: banders
ms.openlocfilehash: 4ef0420eab8064343b54f2c0bf15a692b8bc038c
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382805"
---
# <a name="buy-a-reservation"></a>Ayırma satın alma

Azure Rezervasyonları birçok Azure kaynağı için bir yıllık veya üç yıllık planları kabul ederek tasarruf etmenize yardımcı olur. Rezervasyon satın alma taahhüdünde bulunmadan önce, satın almanıza hazırlamak için aşağıdaki bölümleri gözden geçirdiğinizden emin olun.

## <a name="who-can-buy-a-reservation"></a>Kimler rezervasyon satın alabilir?

Bir plan satın almak için bir Kurumsal (MS-AZR-0017P veya MS-AZR-0148P) ya da Kullandıkça Öde aboneliğinde (MS-AZR-0003P veya MS-AZR-0023P) veya Microsoft Müşteri Sözleşmesi aboneliğinde abonelik sahibi rolünüz olmalıdır. Bulut çözümü sağlayıcıları, Azure Ayrılmış Sanal Makine Örnekleri satın almak için Azure portalını veya  [İş Ortağı Merkezi](/partner-center/azure-reservations) ’ni kullanabilir.

Kurumsal Anlaşma (EA) müşterileri, EA Portalındaki **Ayrılmış Örnekler Ekle** seçeneğini devre dışı bırakarak satın alma işlemlerini EA yöneticileriyle sınırlayabilir. EA yöneticilerinin bir rezervasyon satın almak için en az bir EA aboneliğinin abonelik sahibi olması gerekir. Bu seçenek, merkezi bir ekibin farklı maliyet merkezleri için rezervasyonlar satın almasını isteyen kuruluşlar için yararlıdır. Satın alma işleminden sonra merkezi ekipler, rezervasyonlara maliyet merkezi sahiplerini ekleyebilirler. Bundan sonra sahipler, rezervasyonu aboneliklerinin kapsamına alabilirler. Merkezi ekibin, rezervasyonun satın alındığı yerde abonelik sahibi erişimine sahip olması gerekmez.

Rezervasyon indirimi yalnızca Kurumsal, Bulut Çözümü Sağlayıcısı (CSP) ve Microsoft Müşteri Sözleşmesi ile satın alınan abonelikler ve kullandıkça öde tarifelerine sahip bireysel planlar ile ilişkili kaynaklar için geçerlidir.

## <a name="scope-reservations"></a>Rezervasyonları kapsama alma

Bir rezervasyonu bir abonelik veya kaynak gruplarının kapsamına alabilirsiniz. Bir rezervasyon kapsamının ayarlanması, ayırma tasarruflarının nereye uygulanacağını seçer. Rezervasyonu bir kaynak grubunun kapsamına aldığınızda rezervasyon indirimleri aboneliğin tamamı için değil yalnızca kaynak grubu için geçerli olur.

### <a name="reservation-scoping-options"></a>Rezervasyon kapsamı belirleme seçenekleri

Gereksinimlerinize bağlı olarak bir rezervasyonu kapsama almak için üç seçeneğiniz vardır:

- **Tek kaynak grubu kapsamı**—Yalnızca seçilen kaynak grubunda eşleşen kaynaklara rezervasyon indirimini uygular.
- **Tek abonelik kapsamı**—Yalnızca seçilen abonelikte eşleşen kaynaklara rezervasyon indirimini uygular.
- **Paylaşılan kapsam**—Faturalama bağlamında bulunan uygun aboneliklerdeki eşleşen kaynaklara rezervasyon indirimini uygular. Kurumsal Anlaşma müşterileri için faturalama bağlamı kayıttır. Microsoft Müşteri Sözleşmesi müşterileri için faturalama kapsamı, faturalama profilidir. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.

Kullanımınıza rezervasyon indirimleri uygulanırken, Azure rezervasyonu aşağıdaki sırayla işler:

1. Kaynak grubu kapsamındaki rezervasyonlar
2. Tek kapsam rezervasyonları
3. Paylaşılan kapsam rezervasyonları

Rezervasyon satın aldıktan sonra her zaman kapsamı güncelleştirebilirsiniz. Bunu yapmak için rezervasyona gidin, **Yapılandırma**'ya tıklayın ve rezervasyon kapsamını yeniden belirleyin. Rezervasyon kapsamını yeniden belirlemek ticari bir işlem değildir. Rezervasyon süreniz değişmez. Kapsamı güncelleştirme hakkında daha fazla bilgi için bkz. [Rezervasyon satın aldıktan sonra kapsamı güncelleştirme](manage-reserved-vm-instance.md#change-the-reservation-scope).

![Bir rezervasyon kapsamı değişikliğini gösteren örnek](./media/prepare-buy-reservation/rescope-reservation-resource-group.png)

## <a name="discounted-subscription-and-offer-types"></a>İndirimli abonelik ve teklif türleri

Rezervasyon indirimleri aşağıdaki uygun aboneliklere ve teklif türlerine uygulanır.

- Kurumsal anlaşma (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P)
- Microsoft Müşteri Sözleşmesi abonelikleri.
- Kullandıkça öde ücretlerinin uygulandığı bireysel planlar (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P)
- CSP abonelikleri

Başka teklif türlerinin kullanıldığı bir abonelikte çalıştırılan kaynaklar rezervasyon indirimi almaz.

## <a name="purchase-reservations"></a>Rezervasyon satın alma

Azure portalından, API’lerden, PowerShell’den ve CLI’dan rezervasyon satın alabilirsiniz. Rezervasyon satın almaya hazır olduğunuzda aşağıdaki makalelerden size uygun olanları okuyun:

- [App Service](prepay-app-service-isolated-stamp.md)
- [Redis için Azure Önbelleği](../../azure-cache-for-redis/cache-reserved-pricing.md)
- [Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md)
- [Databricks](prepay-databricks-reserved-capacity.md)
- [Veri Gezgini](/azure/data-explorer/pricing-reserved-capacity)
- [Disk Depolama](../../virtual-machines/linux/disks-reserved-capacity.md)
- [Ayrılmış Konak](../../virtual-machines/prepay-dedicated-hosts-reserved-instances.md)
- [Yazılım planları](../../virtual-machines/linux/prepay-suse-software-charges.md)
- [Depolama](../../storage/blobs/storage-blob-reserved-capacity.md)
- [SQL Veritabanı](../../sql-database/sql-database-reserved-capacity.md)
- [SQL Veri Ambarı](prepay-sql-data-warehouse-charges.md)
- [Sanal makineler](../../virtual-machines/windows/prepay-reserved-vm-instances.md)

## <a name="buy-reservations-with-monthly-payments"></a>Aylık ödemelerle rezervasyonlar satın alma

Aylık ödemelerle rezervasyonlar için ödeme yapabilirsiniz. Tüm tutarı ödediğiniz peşin satın almadan farklı olarak aylık ödeme seçeneği, rezervasyonun toplam maliyetini dönemin her ayına eşit olarak böler. Peşin ve aylık rezervasyonların toplam maliyeti aynıdır ve aylık ödemeyi seçtiğinizde ekstra ücret ödemezsiniz.

Rezervasyonunuz Microsoft müşteri sözleşmesi (MCA) kullanılarak satın alındıysa, aylık ödeme tutarınız yerel para biriminiz için güncel ayın döviz kuruna bağlı olarak değişiklik gösterebilir.

Şunlar için aylık ödemeler kullanılamaz: Databricks, SUSE Linux rezervasyonları, Red Hat Planları ve Azure Red Hat OpenShift Compute.

### <a name="view-payments-made"></a>Yapılan ödemeleri görüntüleme

API’ler, kullanım verileri ve maliyet analizi kullanılarak yapılan ödemeleri görüntüleyebilirsiniz. Aylık olarak ödenen rezervasyonlar için sıklık değeri, kullanım verileri ve Rezervasyon Ücretleri API’sinde **yinelenen** olarak gösterilir. Peşin ödenen rezervasyonlar için değer, **tek seferlik** olarak gösterilir.

Maliyet analizi, varsayılan görünümde aylık satın almaları gösterir. Satın alınan tüm öğeleri görmek istiyorsanız **Masraf türü** için **satın alma** filtresini ve **Sıklık** için **yinelenen** filtresini uygulayın. Yalnızca rezervasyonları görüntülemek istiyorsanız, **Rezervasyon** için bir filtre uygulayın.

![Maliyet analizinde rezervasyon satın alma maliyetlerini gösteren örnek](./media/prepare-buy-reservation/cost-analysis.png)

### <a name="exchange-and-refunds"></a>Değiştirme ve para iadeleri

Diğer rezervasyonlar gibi, aylık faturalama ile satın alınan rezervasyonları değiştirebilir veya para iadesi alabilirsiniz. 

Aylık olarak ödenen bir rezervasyonu değiştirdiğinizde, yeni satın almanın toplam yaşam süresi maliyeti, iade edilen rezervasyon için iptal edilen kalan ödemelerden fazla olmalıdır. Değişimler için başka bir limit veya ücret yoktur. Aylık olarak faturalandırılan yeni bir rezervasyon satın almak için peşin ödenen bir rezervasyonu değiştirebilirsiniz. Ancak yeni rezervasyonun yaşam süresi değeri, iade edilen rezervasyonun eşit olarak dağıtılan değerinden büyük olmalıdır.

Aylık ödeme yapılan bir rezervasyonu iptal ederseniz, iptal edilen gelecek ödemeleri 50.000 ABD doları para iadesi sınırına kadar tahakkuk ettirilir.

Değişim ve para iadeleri hakkında daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](exchange-and-refund-azure-reservations.md).

## <a name="reservation-notifications"></a>Rezervasyon bildirimleri

Azure aboneliğiniz için nasıl ödeme yaptığınıza bağlı olarak, kuruluşunuzdaki aşağıdaki kullanıcılara rezervasyon bildirimleri e-posta ile gönderilir. Bildirimleri aşağıdakiler dahil çeşitli olaylar için gönderilir: 

- Satın Al
- Yaklaşan rezervasyon süre sonu
- Süre sonu
- Yenileme
- İptal
- Kapsam değişikliği

EA abonelikleri olan müşteriler için:

- Bildirimler yalnızca EA bildirim kişilerine gönderilir.
- RBAC (IAM) izni kullanılarak rezervasyona eklenen kullanıcılar herhangi bir e-posta bildirimi almaz.

Bireysel abonelikleri olan müşteriler için:

- Satın alan kişi bir satın alma bildirimi alır.
- Satın alma sırasında, abonelik faturalama hesabı sahibi bir satın alma bildirimi alır.
- Hesap sahibi diğer tüm bildirimleri alır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için Ayırmaları Yönetme](manage-reserved-vm-instance.md)
