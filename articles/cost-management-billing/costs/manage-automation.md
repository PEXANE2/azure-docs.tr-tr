---
title: Otomasyon ile Azure maliyetlerini yönetme
description: Bu makalede, otomasyon ile Azure maliyetlerini nasıl yönetebileceğiniz açıklanmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 04/15/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: adwise
ms.openlocfilehash: 0727f98b917944f3721c6c6758fde05c2efd8773
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449840"
---
# <a name="manage-costs-with-automation"></a>Otomasyon ile maliyetleri yönetme

Maliyet verilerini alıp yönetmek amacıyla özel bir çözüm kümesi oluşturmak için Maliyet Yönetimi otomasyonunu kullanabilirsiniz. Bu makalede, Maliyet Yönetimi otomasyonuna yönelik yaygın senaryolar ve içinde bulunduğunuz duruma göre kullanabileceğiniz seçenekler ele alınmaktadır. API’leri kullanarak uygulama geliştirmek istiyorsanız, bu süreci hızlandırmaya yardımcı olmak için sunulan yaygın API isteği örneklerinden faydalanabilirsiniz.

## <a name="automate-cost-data-retrieval-for-offline-analysis"></a>Çevrimdışı analiz için maliyet verilerini almayı otomatikleştirme

Azure maliyet verilerinizi diğer veri kümeleriyle birleştirmek için indirmeniz gerekebilir. Alternatif olarak, maliyet verilerinizi kendi sistemlerinize tümleştirmeniz gerekebilir. İlgili veri miktarına bağlı olarak farklı seçenekler mevcuttur. Her durumda API’leri ve araçları kullanmak için uygun kapsamda Maliyet Yönetimi izinlerine sahip olmanız gerekir. Daha fazla bilgi için bkz. [Verilere erişim atama](https://docs.microsoft.com/azure/cost-management-billing/costs/assign-access-acm-data).

## <a name="suggestions-for-handling-large-datasets"></a>Büyük veri kümelerini işlemeye yönelik öneriler

Kuruluşunuzun Azure’ın kullanıldığı çok sayıda kaynak ve abonelik varsa, kullanım ayrıntılarına ilişkin oldukça fazla veri elde edersiniz. Excel genellikle böyle büyük dosyaları yükleyemez. Bu durumda aşağıdaki seçenekleri öneririz:

**Power BI**

Power BI, büyük miktarda verileri alıp işlemek için kullanılır. Kurumsal Anlaşma müşterisiyseniz faturalama hesabınızın maliyetlerini analiz etmek için Power BI şablon uygulamasını kullanabilirsiniz. Raporda, müşteriler tarafından kullanılan önemli görünümler yer alır. Daha fazla bilgi için bkz. [Power BI şablon uygulamasıyla Azure maliyetlerini analiz etme](https://docs.microsoft.com/azure/cost-management-billing/costs/analyze-cost-data-azure-cost-management-power-bi-template-app).

**Power BI veri bağlayıcısı**

Verilerinizi günlük olarak analiz etmek istiyorsanız ayrıntılı analiz amacıyla verileri almak için [Power BI veri bağlayıcısını](https://docs.microsoft.com/power-bi/connect-data/desktop-connect-azure-cost-management) kullanmanızı öneririz. Daha fazla maliyet yansıdıkça bağlayıcı, oluşturduğunuz raporları güncel tutar.

**Maliyet Yönetimi dışarı aktarmaları**

Verileri günlük olarak analiz etmeniz gerekmeyebilir. Böyle durumlarda, veri dışarı aktarmalarını bir Azure Depolama hesabına yönlendirilecek biçimde zamanlamak için Maliyet Yönetimi’nin [Dışarı aktarmalar](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data) özelliğini kullanmanız faydalı olabilir. Daha sonra, verileri gerektiği şekilde Power BI’a yükleyebilir veya dosya yeterince küçükse Excel’de analiz edebilirsiniz. Dışarı aktarmaları Azure portalında bulabilirsiniz veya [Dışarı Aktarmalar API’sini](https://docs.microsoft.com/rest/api/cost-management/exports) kullanarak dışarı aktarmaları yapılandırabilirsiniz.

**Kullanım Ayrıntıları API’si**

Maliyet verileriniz küçük bir kümeyse [Kullanım Ayrıntıları API’sini](https://docs.microsoft.com/rest/api/consumption/usageDetails) kullanmanız faydalı olabilir. Maliyet verileriniz büyük bir kümeyse, bir dönem için talep edeceğiniz veri miktarını mümkün olan en düşük düzeyde tutmanız gerekir. Bunu yapmak için kısa bir zaman aralığı belirtin veya isteğinizde filtre kullanın. Örneğin, üç yıllık maliyet verilerine ihtiyaç duyduğunuz bir senaryoda, tek bir çağrı yerine farklı zaman aralıkları için birden çok çağrı yaptığınızda API kullanmanız daha faydalı olabilir. Buradan, daha ayrıntılı analiz etmek amacıyla verileri Excel’e yükleyebilirsiniz.

## <a name="automate-retrieval-with-usage-details-api"></a>Kullanım Ayrıntıları API’si ile alımı otomatikleştirme

[Kullanım Ayrıntıları API’si](https://docs.microsoft.com/rest/api/consumption/usageDetails) Azure faturanıza karşılık gelen ham, toplu olmayan maliyet verilerini almanın kolay bir yolunu sunar. Kuruluşunuz için programlı bir veri alma çözümüne ihtiyaç duyuyorsanız API’den faydalanabilirsiniz. Daha küçük maliyet verisi kümelerini analiz etmek istiyorsanız API’yi kullanmanız faydalı olabilir. Ancak daha büyük veri kümeleriniz varsa önceden açıklanan diğer çözümleri kullanmanız gerekir. Kullanım Ayrıntılarındaki veriler günlük olarak ölçüm başına sağlanır. Bu veriler, aylık faturanız hesaplanırken kullanılır. Bu API’lerin genel kullanılabilirlik (GA) sürümü, `2019-10-01` sürümüdür. Rezervasyon ve Azure Market satın alımlarına yönelik önizleme sürümüne erişmek için API’lerle `2019-04-01-preview` sürümünü kullanın.

### <a name="usage-details-api-suggestions"></a>Kullanım Ayrıntıları API’si önerileri

**İstek zamanlaması**

Kullanım Ayrıntıları API’sine bir günde _birden fazla istek yapmamanızı_ öneririz. Maliyet verilerinin ne sıklıkta yenilendiği ve yuvarlamanın nasıl ele alındığı hakkında daha fazla bilgi için bkz. [Maliyet yönetimi verilerini anlama](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data#rated-usage-data-refresh-schedule).

**Filtre uygulamadan en üst düzey kapsamları hedefleme**

İhtiyaç duyduğunuz tüm verileri mümkün olan en geniş kapsamda almak için API’yi kullanın. Filtreleme, gruplandırma veya toplu analiz yapmadan önce tüm gerekli verilerin alınmasını bekleyin. API, büyük miktarda toplu olmayan ham maliyet verilerini sunmak amacıyla özel olarak iyileştirilmiştir. Maliyet Yönetimi’nde mevcut olan kapsamlar hakkında daha fazla bilgi edinmek için bkz. [Kapsamları anlama ve bunlarla çalışma](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-work-scopes). Bir kapsam için gereken verileri indirdiğinizde, verileri filtrelerle ve PivotTable’larla daha ayrıntılı bir şekilde analiz etmek için Excel’i kullanın.

## <a name="example-usage-details-api-requests"></a>Örnek Kullanım Ayrıntıları API’si istekleri

Aşağıdaki örnek istekler, karşılaşabileceğiniz yaygın senaryoları ele almak amacıyla Microsoft müşterileri tarafından kullanılır.

### <a name="get-usage-details-for-a-scope-during-specific-date-range"></a>Belirli bir tarih aralığı süresince bir kapsama ait Kullanım Verilerini alma

İstek tarafından döndürülen veriler, faturalama sistemi tarafından kullanım verilerinin alındığı tarihe karşılık gelir. Bu, birden çok faturadaki maliyetleri içerebilir.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?$filter=properties%2FusageStart%20ge%20'2020-02-01'%20and%20properties%2FusageEnd%20le%20'2020-02-29'&$top=1000&api-version=2019-10-01

```

### <a name="get-amortized-cost-details"></a>Amorti edilmiş maliyet ayrıntılarını alma

Gerçek maliyetlerin, yansıtılan satın alma işlemlerini göstermesini istiyorsanız aşağıdaki istekte *ölçüm* değerini `ActualCost` ile değiştirin. Amorti edilmiş ve gerçek maliyetleri kullanmak için `2019-04-01-preview` sürümünü kullanmanız gerekir. Geçerli API sürümü, yeni tür/ölçüm özniteliği ve değişen özellik adları dışında `2019-10-01` sürümüyle aynı şekilde çalışır. Microsoft Müşteri Sözleşmeniz varsa aşağıdaki örnekte filtreleriniz `startDate` ve `endDate` olur.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

## <a name="retrieve-large-cost-datasets-recurringly-with-exports"></a>Dışarı aktarmalar ile büyük miktarda veri kümelerini düzenli olarak alma

Dışarı aktarmalar özelliği, maliyet verilerinin dökümünü düzenli olarak zamanlamaya yönelik bir çözümdür. Kullanım dosyaları, Kullanım Ayrıntıları API’sini kullanarak güvenilir bir şekilde çağrılıp indirilmek için fazla büyük olabilen kuruluşlar için toplu olmayan maliyet verilerini almak için bu yöntem önerilir. Veriler, tercih ettiğiniz bir Azure Depolama hesabına yerleştirilir. Buradan, verileri kendi sistemlerinize yükleyip ekiplerinizin ihtiyaç duyduğu şekilde analiz edebilirsiniz. Dışarı aktarmaları Azure portalında yapılandırmak için bkz. [Verileri dışarı aktarma](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data).

## <a name="data-latency-and-rate-limits"></a>Veri gecikme süresi ve hız sınırları

API’leri günde en fazla bir kez çağırmanızı öneririz. Maliyet Yönetimi verileri, Azure kaynak sağlayıcılarından yeni kullanım verileri alındıkça dört saatte bir yenilenir. Daha sık çağırmak, ek veri elde etmenizi sağlamaz. Bunun yerine, yalnızca yükün artmasına neden olur. Verilerin değişme sıklığı ve veri gecikme süresinin işlenme yöntemi hakkında daha fazla bilgi edinmek için bkz. [Maliyet Yönetimi verilerini anlama](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data).

### <a name="error-code-429---call-count-has-exceeded-rate-limits"></a>Hata kodu 429 - Çağrı sayısı, hız sınırını aştı

Tüm Maliyet Yönetimi abonelerine tutarlı bir deneyim sunmak amacıyla Maliyet Yönetimi API’leri hız sınırına sahiptir. Sınıra ulaştığınızda `429: Too many requests` HTTP durum kodunu alırsınız. API’lerimizin geçerli aktarım hızı aşağıdaki gibidir:

- Dakikada 30 çağrı - Kapsam, kullanıcı veya uygulama başına yapılır.
- Dakikada 200 çağrı - Kiracı, kullanıcı veya uygulama başına yapılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Power BI şablon uygulamasıyla Azure maliyetlerini analiz etme](https://docs.microsoft.com/azure/cost-management-billing/costs/analyze-cost-data-azure-cost-management-power-bi-template-app).
- Dışarı Aktarmalar ile [dışarı aktarılmış verileri oluşturup yönetin](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data).
- [Kullanım Ayrıntıları API’si](https://docs.microsoft.com/rest/api/consumption/usageDetails) hakkında daha fazla bilgi edinin.