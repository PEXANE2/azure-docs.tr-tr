---
title: Tanı ayarlarıyla Azure Etkinliği günlüğünü topla - Azure Monitor | Microsoft Dokümanlar
description: Azure Etkinlik günlüklerini Azure Monitör günlüklerine, Azure depolama alanına veya Azure Etkinlik Hub'larına iletmek için tanılama ayarlarını kullanın.
author: bwren
ms.subservice: logs
ms.topic: conceptual
ms.author: bwren
ms.date: 02/04/2020
ms.openlocfilehash: 6d4c724c7cfb4c1779f0fc6592a7e61e060755b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79096899"
---
# <a name="update-to-azure-activity-log-collection-and-export"></a>Azure Etkinliği günlük toplama ve dışa aktarma için güncelleştirme
[Azure Etkinliği günlüğü,](platform-logs-overview.md) Azure'da gerçekleşen abonelik düzeyi olaylarına ilişkin bilgi sağlayan bir platform [günlüğüdür.](platform-logs-overview.md) Etkinlik günlüğü girişlerini [bir olay hub'ına veya depolama hesabına veya](activity-log-export.md) Log [Analytics çalışma alanına](activity-log-collect.md) gönderme yöntemi [tanılama ayarlarını](diagnostic-settings.md)kullanmak üzere değiştirildi. Bu makalede, tanılama ayarlarına değiştirmeye hazırlık olarak eski ayarların nasıl temizlenir, yöntemler le arasındaki fark açıklanmaktadır.


## <a name="differences-between-methods"></a>Yöntemler arasındaki farklar

### <a name="advantages"></a>Yararları
Tanılama ayarlarını kullanmanın geçerli yöntemlere göre aşağıdaki avantajları vardır:

- Tüm platform günlüklerini toplamak için tutarlı bir yöntem.
- Birden çok abonelik ve kiracılar arasında Etkinlik günlüğü toplayın.
- Yalnızca belirli kategoriler için günlükleri toplamak için koleksiyonu filtreleyin.
- Tüm Etkinlik günlüğü kategorilerini toplayın. Bazı kategoriler eski yöntem kullanılarak toplanmaz.
- Günlük alımı için daha hızlı gecikme. Tanı ayarları yalnızca yaklaşık 1 dakika eklerken önceki yöntem yaklaşık 15 dakika gecikme vardır.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Bu özelliği etkinleştirmeden önce tanılama ayarlarını kullanarak Etkinlik günlüğü koleksiyonunun aşağıdaki ayrıntılarını göz önünde bulundurun.

- Etkinlik günlüğünü Azure depolama alanına toplamak için bekletme ayarı kaldırılmış, yani siz kaldırana kadar veriler süresiz olarak depolanacaktır.
- Şu anda, yalnızca Azure portalını kullanarak bir abonelik düzeyi tanılama ayarı oluşturabilirsiniz. PowerShell veya CLI gibi diğer yöntemleri kullanmak için bir Kaynak Yöneticisi şablonu oluşturabilirsiniz.


### <a name="differences-in-data"></a>Veri farklılıkları
Tanılama ayarları, Etkinlik günlüğünü toplamak için kullanılan önceki yöntemlerle aynı verileri aşağıdaki geçerli farklarla toplar:

Aşağıdaki sütunlar kaldırıldı. Bu sütunların değiştirilmesi farklı bir biçimde olduğundan, bunları kullanan günlük sorgularını değiştirmeniz gerekebilir. Şemada kaldırılan sütunları hala görebilirsiniz, ancak bunlar verilerle doldurulmaz.

| Kaldırılan sütun | Yedek sütun |
|:---|:---|
| Etkinlik Durumu    | ActivityStatusValue    |
| ActivitySubstatus | ActivitySubstatusValue |
| ThrottledRequests     | OperationNameValue     |
| ResourceProvider  | Kaynak Sağlayıcı Değeri  |

Aşağıdaki sütun eklendi:

- Authorization_d
- Claims_d
- Properties_d

> [!IMPORTANT]
> Bazı durumlarda, bu sütunlarda değerleri tüm büyük harf olabilir. Bu sütunları içeren bir sorgunuz varsa, büyük/küçük harf duyarsız karşılaştırması yapmak için [=~ işleci](https://docs.microsoft.com/azure/kusto/query/datatypes-string-operators) kullanmalısınız.

## <a name="work-with-legacy-settings"></a>Eski ayarlarla çalışma
Tanılama ayarı yla değiştirmeyi seçmezseniz, Etkinlik günlüğünü toplamak için eski ayarlar çalışmaya devam edecektir. Bir aboneliğin günlük profilini yönetmek için aşağıdaki yöntemi kullanın.

1. Azure portalındaki **Azure Monitör** menüsünden **Etkinlik günlüğü'nü**seçin.
3. **Tanılama ayarları**'na tıklayın.

   ![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Eski deneyim için mor banner'ı tıklatın.

    ![Eski deneyim](media/diagnostic-settings-subscription/legacy-experience.png)


Eski toplama yöntemlerini kullanma yla ilgili ayrıntılar için aşağıdaki makalelere bakın.

- [Azure Monitor'da Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve analiz](activity-log-collect.md)
- [Azure Etkin Dizin kiracılarında Azure Etkinliği günlüklerini Azure Monitörü'nde toplama](activity-log-collect-tenants.md)
- [Azure Etkinliği günlüğünü depolama veya Azure Etkinlik Hub'larına dışa aktarma](activity-log-export.md)

## <a name="disable-existing-settings"></a>Varolan ayarları devre dışı
Tanılama ayarlarını kullanmadan önce Etkinliğin varolan koleksiyonunu devre dışı bmelisiniz. Her ikisinin de etkin olması, yinelenen verilere neden olabilir.

### <a name="disable-collection-into-log-analytics-workspace"></a>Log Analytics çalışma alanına toplamayı devre dışı

1. Azure portalında **Günlük Analizi çalışma alanları** menüsünü açın ve Etkinlik Günlüğü'nü toplamak için çalışma alanını seçin.
2. Çalışma **alanı** menüsünün Çalışma Alanı Veri Kaynakları bölümünde Azure **Etkinliği günlüğünü**seçin.
3. Bağlantısını kesmek istediğiniz aboneliği tıklatın.
4. Seçiminizi onaylamanız istendiğinde **Bağlantıyı Kesme'yi** ve ardından **Evet'i** tıklatın.

### <a name="disable-log-profile"></a>Günlük profilini devre dışı

1. Eski ayarları açmak için [Eski Ayarlarla Çalışma'da](#work-with-legacy-settings) açıklanan yordamı kullanın.
2. Geçerli koleksiyonu depolama veya olay hub'larına devre dışı bırakma.



## <a name="activity-log-monitoring-solution"></a>Aktivite Günlüğü izleme çözümü
Azure Log Analytics izleme çözümü, Log Analytics çalışma alanınızdaki Etkinlik Günlüğü kayıtlarını analiz etmek için birden çok günlük sorgusu ve görünüm içerir. Bu çözüm, Log Analytics çalışma alanında toplanan günlük verilerini kullanır ve tanılama ayarlarını kullanarak Etkinlik günlüğünü toplarsanız herhangi bir değişiklik yapmadan çalışmaya devam eder. Bu çözümle ilgili ayrıntılar için [Activity Logs Analytics izleme çözümüne](activity-log-collect.md#activity-logs-analytics-monitoring-solution) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik Günlüğü hakkında daha fazla bilgi edinin](../../azure-resource-manager/management/view-activity-logs.md)
* [Tanılama ayarları hakkında daha fazla bilgi edinin](diagnostic-settings.md)
