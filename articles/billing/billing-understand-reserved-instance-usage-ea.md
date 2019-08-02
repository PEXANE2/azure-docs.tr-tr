---
title: Kurumsal anlaşmalar için Azure ayırmaları kullanımını anlayın
description: Kurumsal kaydınız için Azure ayırmasının nasıl uygulandığını anlamak için kullanımınızı nasıl okuyacağınızı öğrenin.
author: bandersmsft
manager: yashar
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 507ad62a917120689bee3f1e293e23c9ab8b0f66
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598106"
---
# <a name="get-enterprise-agreement-reservation-costs-and-usage"></a>Kurumsal Anlaşma rezervasyon maliyetlerini ve kullanımını alın

Rezervasyon maliyetleri ve kullanım verileri, Azure portal ve REST API 'Lerinde Kurumsal Anlaşma müşteriler için kullanılabilir. Bu makale şunları yapmanıza yardımcı olur:

- Rezervasyon satın alma verilerini al
- Hangi aboneliğin, kaynak grubunun veya kaynağın ayırmayı kullanıldığını öğrenin
- Rezervasyon kullanımı için geri ödeme
- Rezervasyon tasarrufunu hesapla
- Kullanım için rezervasyon verileri al
- Rezervasyon maliyetlerini İtfası

Market ücretleri kullanım verilerinde birleştirilir. Tek bir veri kaynağından ilk parti kullanımı, Market kullanımı ve satın alma ücretleri görüntülenir.

## <a name="reservation-charges-in-azure-usage-data"></a>Azure kullanım verilerinde rezervasyon ücretleri

Veriler iki ayrı veri kümesine ayrılmıştır: _Gerçek maliyet_ ve _itfası maliyeti_. Bu iki veri kümesi nasıl farklılık gösterir:

**Gerçek maliyet** -aylık faturanızla mutabık kılınacak verileri sağlar. Bu verilerde rezervasyon satın alma maliyetleri ve rezervasyon uygulaması ayrıntıları vardır. Bu verilerle, hangi aboneliğin veya kaynak grubunun ya da kaynağın belirli bir günde rezervasyon indirimi aldığını bilirsiniz. Rezervasyon iskontosunu alan kullanım için uygulanan bir ücret sıfırdır.

**Itfası maliyeti** -bu veri kümesi gerçek maliyet veri kümesine benzer, ancak rezervasyon indirimi alan kullanım için uygulanan ücret, rezervasyonun eşit olarak dağıtılmasından (sıfır yerine). Bu, bir abonelik, kaynak grubu veya kaynak tarafından rezervasyon tüketiminin parasal değerini bilmenize yardımcı olur ve rezervasyon kullanımının dahili olarak geri ücretlendirmenize yardımcı olabilir. Veri kümesinde kullanılmayan ayırma saatleri de vardır. Veri kümesinde rezervasyon satın alma kayıtları yok. 

İki veri kümesi karşılaştırması:

| Data | Gerçek maliyet veri kümesi | İtfası maliyet veri kümesi |
| --- | --- | --- |
| Rezervasyon satın alımları | Bu görünümde kullanılabilir.<br><br>  Bu veri filtresini chargetype = &quot;Purchase&quot;üzerinde almak için. <br><br> Ücrette hangi ayırmayı olduğunu bildirmek için Rezervationıd veya Rezervationname bölümüne bakın.  | Bu görünüm için geçerli değildir. <br><br> Satın alma maliyetleri, itfası verilerinde sağlanmaz. |
| Efekt fiyatı | Değer, rezervasyon iskontosunu alan kullanım için sıfırdır. | Değer, rezervasyon indirimi olan kullanım için rezervasyonun saat başına eşit olarak dağıtılmış maliyetlidir. |
| Kullanılmayan ayırma (rezervasyonun bir gün içinde kullanıldığı saat sayısını ve atık değerinin parasal değerini sağlar) | Bu görünümde geçerli değildir. | Bu görünümde kullanılabilir.<br><br> Bu verileri almak için, chargetype = &quot;UnusedReservation&quot;üzerine filtre uygulayın.<br><br>  Hangi ayırmanın az kullanıldığını bildirmek için Rezervationıd veya Rezervationname bölümüne bakın. Bu, gün için rezervasyonun ne kadarının harcanmıştı.  |
| BirimFiyat (fiyat sayfanızdaki kaynağın fiyatı) | Kullanılabilir | Kullanılabilir |

Azure kullanım verilerinde bulunan diğer bilgiler değiştirilmiştir:

- Ürün ve ölçüm bilgileri-Azure, daha önce olduğu gibi, ilk tüketilen ölçümü Rezervationıd ve Rezervationname ile değiştirmez.
- Rezervkimliği ve Rezervadı-bunlar verilerdeki kendi alanlarıdır. Daha önce, yalnızca AdditionalInfo altında kullanılabilir.
- ProductOrderId-rezervasyon siparişi KIMLIĞI, kendi alanı olarak eklendi.
- ProductOrderName-satın alınan ayırmanın ürün adı.
- Dönem-12 ay veya 36 ay.
- RINormalizationRatio-AdditionalInfo altında kullanılabilir. Bu, rezervasyonun kullanım kaydına uygulandığı orandır. Ayırma için örnek boyutu esnekliği etkinleştirilirse, diğer boyutlara de uygulanabilir. Değer, kullanım kaydı için rezervasyonun uygulanma oranını gösterir.

## <a name="get-azure-consumption-and-reservation-usage-data-using-api"></a>API kullanarak Azure tüketim ve ayırma kullanım verilerini edinme

Verileri API kullanarak alabilir veya Azure portal adresinden indirebilirsiniz.

Yeni verileri almak için API sürüm &quot;2019-04-01-Önizleme&quot; ile [kullanım ayrıntıları API](/rest/api/consumption/usagedetails/list) 'sini çağırabilirsiniz. Terminoloji hakkında daha fazla bilgi için bkz. [kullanım koşulları](billing-understand-your-usage.md). Çağıranın [EA Portalı](https://ea.azure.com)kullanılarak kurumsal anlaşma Için bir kuruluş yöneticisi olması gerekir. Salt okunurdur kurumsal Yöneticiler verileri de alabilir.

Veriler, [Kurumsal müşteriler Için Raporlama API 'lerinde kullanılabilir değildir-kullanım ayrıntıları](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail).

API 'ye örnek bir çağrı aşağıda verilmiştir:

```
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{enrollmentId}/providers/Microsoft.Billing/billingPeriods/{billingPeriodId}/providers/Microsoft.Consumption/usagedetails?metric={metric}&amp;api-version=2019-04-01-preview&amp;$filter={filter}
```

{Kayıtkimliği} ve {billingPeriodId} hakkında daha fazla bilgi için bkz. [kullanım ayrıntıları – liste](https://docs.microsoft.com/rest/api/consumption/usagedetails/list) API 'si makalesi.

Ölçüm ve filtre hakkında aşağıdaki tablodaki bilgiler, yaygın ayırma sorunlarının çözülmesine yardımcı olabilir.

| **API verilerinin türü** | API çağrısı eylemi |
| --- | --- |
| **Tüm ücretler (kullanım ve satın alma)** | {Metric} öğesini ActualCost ile değiştirin |
| **Rezervasyon indirimi olan kullanım** | {Metric} öğesini ActualCost ile değiştirin<br><br>{Filter} öğesini ile Değiştir: Özellikler/Rezervationıd% 20ne% 20 |
| **Ayırma indirimi almadığınız kullanım** | {Metric} öğesini ActualCost ile değiştirin<br><br>{Filter} öğesini ile Değiştir: Özellikler/Rezervationıd% 20eq% 20 |
| **İtfası ücretleri (kullanım ve satın alma)** | {Metric} değerini Itfası Zedcost ile değiştirin |
| **Kullanılmayan ayırma raporu** | {Metric} değerini Itfası Zedcost ile değiştirin<br><br>{Filter} öğesini ile Değiştir: Özellikler/ChargeType% 20eq% 20 ' UnusedReservation ' |
| **Rezervasyon satın alımları** | {Metric} öğesini ActualCost ile değiştirin<br><br>{Filter} öğesini ile Değiştir: Özellikler/ChargeType% 20eq% 20 ' satın alma '  |
| **İadelerini** | {Metric} öğesini ActualCost ile değiştirin<br><br>{Filter} öğesini ile Değiştir: Özellikler/ChargeType% 20eq% 20 ' Iadesi ' |

## <a name="download-the-usage-csv-file-with-new-data"></a>Kullanım CSV dosyasını yeni verilerle indirin

EA yöneticisiyseniz, Azure portal yeni kullanım verilerini içeren CSV dosyasını indirebilirsiniz. Bu veriler [EA portalından](https://ea.azure.com)kullanılamaz.

Azure portal [maliyet yönetimi + faturalandırma](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/BillingAccounts)' e gidin.

1. Faturalandırma hesabını seçin.
2. **Kullanım + ücretler**' e tıklayın.
3. **İndir**'e tıklayın.  
![Azure portal CSV kullanım verileri dosyasının nereye Indirileceği gösteren örnek](./media/billing-understand-reserved-instance-usage-ea/portal-download-csv.png)
4. **Kullanım kullanımı + ücretler** bölümünde, **kullanım ayrıntıları sürüm 2** altında **Tüm ücretler ' i (kullanım ve satın alma)** seçin ve ardından İndir ' e tıklayın. **İtfası ücretleri (kullanım ve satın alma)** için yineleyin.

İndirilen CSV dosyaları gerçek maliyetler ve itfası maliyeti içerir.

## <a name="common-cost-and-usage-tasks"></a>Ortak maliyet ve kullanım görevleri

Aşağıdaki bölümler, çoğu kişinin rezervasyon maliyetini ve kullanım verilerini görüntülemek için kullandığı ortak görevlerdir.

### <a name="get-reservation-purchase-costs"></a>Rezervasyon satın alma maliyetlerini al

Rezervasyon satın alma maliyetleri gerçek maliyet verilerinde kullanılabilir. _Chargetype = Purchase_için filtre. Satınalmanın hangi rezervasyon sırasını öğrenmek için ProductOrderID adresine başvurun.

### <a name="get-underutilized-reservation-quantity-and-costs"></a>Az kullanılan rezervasyon miktarı ve maliyetlerini alın

_Chargetype_ _= UnusedReservation_için itfası maliyet verileri ve filtresi alın. Günlük kullanılmayan rezervasyon miktarını ve maliyeti alırsınız. Sırasıyla _Rezervationıd_ ve _ProductOrderId_ alanlarını kullanarak bir ayırma veya rezervasyon siparişi için verileri filtreleyebilirsiniz. Bir rezervasyon% 100 oranında kullanılıyorsa, kaydın 0 miktarı vardır.

### <a name="amortize-reservation-costs"></a>Rezervasyon maliyetlerini İtfası

Bir rezervasyona yönelik günlük itfası maliyetlerini almak için _ProductOrderID_ kullanarak bir rezervasyon siparişi Için itfası maliyeti ve filtre uygulayın.

### <a name="chargeback-for-a-reservation"></a>Rezervasyon için geri ödeme

Rezervasyon kullanımını, abonelik, kaynak grupları veya etiketlere göre diğer kuruluşlara geri ödeme yapabilirsiniz. İtfası maliyet verileri, aşağıdaki veri türlerinde bir ayırma kullanımının parasal değerini sağlar:

- Kaynaklar (örneğin, bir VM)
- Resource group
- Tags
- Subscription

### <a name="get-the-blended-rate-for-chargeback"></a>Geri ödeme için karıştırılan ücret alın

Karışım oranını öğrenmek için, itfası maliyeti verilerini alın ve toplam maliyeti toplayın. VM 'Ler için, AdditionalInfo JSON verilerinden MeterName veya ServiceType bilgilerini kullanabilirsiniz. Toplam maliyeti, karıştırılan oranı almak için kullanılan miktara bölün.

### <a name="audit-optimum-reservation-use-for-instance-size-flexibility"></a>Örnek boyutu esnekliği için en iyi ayırma kullanımını denetleme

AdditionalInfo 'dan, _Rinormalizationratio_ile birden fazla miktar. Sonuçlar, kullanım kaydına kaç saatlik rezervasyon kullanımının uygulandığını gösterir.

### <a name="determine-reservation-savings"></a>Rezervasyon tasarrufunu belirleme

Itfası maliyet verilerini alın ve ayrılmış bir örnek için verileri filtreleyin. Ni

1. Tahmini Kullandıkça Öde maliyetlerini alın. Rezervasyon iskontosunun kullanım için uygulanmadıysa, tahmin edilen Kullandıkça Öde maliyetlerini almak için _BirimFiyat_ değerini _Miktar_ değerleriyle çarpın.
2. Rezervasyon maliyetlerini alın. Ayrılmış örnek için ödediklerinizin parasal değerini almak üzere _Maliyet_ değerlerini toplayın. Bu, rezervasyonun kullanılan ve kullanılmayan maliyetlerini içerir.
3. Tahmini tasarruf elde etmek için tahmini Kullandıkça Öde maliyetlerinden rezervasyon maliyetlerini çıkarın.

## <a name="reservation-purchases-and-amortization-in-cost-analysis"></a>Maliyet analizinde rezervasyon satınalmaları ve İtfası

Rezervasyon maliyetleri, [Maliyet Analizi](https://aka.ms/costanalysis)'nde kullanılabilir. Varsayılan olarak, maliyet analizi, faturanızda maliyetlerin nasıl gösterileceğini gösteren **gerçek maliyeti**gösterir. Ayrılmış olan ve avantajın kullanıldığı kaynaklarla ilişkili rezervasyon satınalmaları görüntülemek için, **itfası maliyetine**geçin:

![Maliyet analizinde itfası maliyetinin nerede seçileceğini gösteren örnek](./media/billing-understand-reserved-instance-usage-ea/portal-cost-analysis-amortized-view.png)

Kullanım, satın alma ve para iadesi işlemlerinin bir listesini görmek için, ücretlendirme türüne göre gruplandırma; rezervasyon ve isteğe bağlı maliyetlerin dökümünü alır. Gerçek maliyetten satın alma işlemleri yaparken göreceğiniz rezervasyon maliyetlerini unutmayın, ancak bu işlem, en düşük maliyetli bir ücret alırken benle kullanılan tek tek kaynaklara göre ücretlendirilir. Ayrıca, itfası maliyetlendirerek yeni bir **UnusedReservation** ücret türü görürsünüz.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Rezervasyon iskontosunun nasıl uygulanacağını anlayın](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
- [Windows yazılım maliyetleri rezervasyonlar içermez](billing-reserved-instance-windows-software-costs.md)
