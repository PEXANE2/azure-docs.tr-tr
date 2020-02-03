---
title: Tanılama ayarları (Önizleme) ile Azure etkinlik günlüğü toplama-Azure Izleyici | Microsoft Docs
description: Azure etkinlik günlüklerini Azure Izleyici günlüklerine, Azure depolama 'ya veya Azure Event Hubs iletmek için tanılama ayarlarını kullanın.
author: bwren
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
ms.author: bwren
ms.date: 01/21/2020
ms.openlocfilehash: dff4901f1488406ed1259d1411a6b05b949382cb
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76715851"
---
# <a name="update-to-azure-activity-log-collection-and-export"></a>Azure etkinlik günlüğü koleksiyonuna ve dışarı aktarmaya Güncelleştir
[Azure etkinlik günlüğü](platform-logs-overview.md) , Azure 'da oluşan abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir [Platform günlüğliğidir](platform-logs-overview.md) . Etkinlik günlüğü girdilerini [bir olay hub 'ına veya depolama hesabına](activity-log-export.md) veya bir [Log Analytics çalışma alanına](activity-log-collect.md) gönderme yöntemi, [tanılama ayarlarını](diagnostic-settings.md)kullanacak şekilde değiştirilmiştir. Bu makalede, Yöntemler ve tanılama ayarlarına geçiş hazırlığı sırasında eski ayarların nasıl temizleneceğini gösteren farklar açıklanmaktadır.


## <a name="differences-between-methods"></a>Yöntemler arasındaki farklılıklar

### <a name="advantages"></a>Yararları
Tanılama ayarlarının kullanılması geçerli yöntemlere göre aşağıdaki avantajları sağlar:

- Tüm platform günlüklerini toplamak için tutarlı yöntem.
- Birden çok abonelik ve kiracılar arasında etkinlik günlüğü toplayın.
- Koleksiyonu yalnızca belirli kategoriler için günlükleri toplayacak şekilde filtreleyin.
- Tüm etkinlik günlüğü kategorilerini toplayın. Bazı kategoriler eski yöntem kullanılarak toplanmaz.
- Günlük alımı için daha hızlı gecikme. Tanılama ayarları yalnızca yaklaşık 1 dakika eklediğinde önceki yöntemde yaklaşık 15 dakikalık gecikme vardır.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
Bu özelliği etkinleştirmeden önce tanılama ayarlarını kullanarak etkinlik günlüğü koleksiyonunun aşağıdaki ayrıntılarını göz önünde bulundurun.

- Etkinlik günlüğünün Azure depolama 'ya toplanması için bekletme ayarı kaldırılmıştır ve bu, verilerin siz kaldırana kadar süresiz olarak depolanacağı anlamına gelir.
- Şu anda, Azure portal kullanarak yalnızca bir abonelik düzeyi tanılama ayarı oluşturabilirsiniz. PowerShell veya CLı gibi diğer yöntemleri kullanmak için bir Kaynak Yöneticisi şablonu oluşturabilirsiniz.


### <a name="differences-in-data"></a>Verilerdeki farklılıklar
Tanılama ayarları, aşağıdaki geçerli farklılıklarla etkinlik günlüğünü toplamak için kullanılan önceki yöntemlerle aynı verileri toplar:

Aşağıdaki sütunlar kaldırılmıştır. Bu sütunların yerini değiştirmek farklı bir biçimde olduğundan, bunları kullanan günlük sorgularını değiştirmeniz gerekebilir. Şemada kaldırılan sütunları görmeye devam edebilirsiniz, ancak bunlar verilerle doldurulmaz.

| Sütun kaldırıldı | Değiştirme sütunu |
|:---|:---|
| ActivityStatus    | ActivityStatusValue    |
| Etkinlik alt durumu | ActivitySubstatusValue |
| için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir     | OperationNameValue     |
| ResourceProvider  | ResourceProviderValue  |

Aşağıdaki sütun eklendi:

- Authorization_d
- Claims_d
- Properties_d

## <a name="work-with-legacy-settings"></a>Eski ayarlarla çalışma
Etkinlik günlüğünü toplamaya yönelik eski ayarlar, bir tanılama ayarıyla değiştirmeyi tercih ederseniz çalışmaya devam edecektir. Bir aboneliğin günlük profilini yönetmek için aşağıdaki yöntemi kullanın.

1. Azure portal **Azure izleyici** menüsünde **etkinlik günlüğü**' nü seçin.
3. **Tanılama ayarları**'na tıklayın.

   ![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Eski deneyimin mor başlığına tıklayın.

    ![Eski deneyim](media/diagnostic-settings-subscription/legacy-experience.png)


Eski koleksiyon yöntemlerini kullanma hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın.

- [Azure Izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplayın ve çözümleyin](activity-log-collect.md)
- [Azure etkinlik günlüklerini Azure Active Directory kiracılar genelinde Azure Izleyici 'ye toplayın](activity-log-collect-tenants.md)
- [Azure etkinlik günlüğünü depolamaya veya Azure Event Hubs dışarı aktarma](activity-log-export.md)

## <a name="disable-existing-settings"></a>Var olan ayarları devre dışı bırak
Tanılama ayarlarını kullanarak etkinleştirilmeden önce var olan etkinlik koleksiyonunu devre dışı bırakmanız gerekir. Her ikisinin de etkinleştirilmesi yinelenen verilerin oluşmasına neden olabilir.

### <a name="disable-collection-into-log-analytics-workspace"></a>Koleksiyonu Log Analytics çalışma alanına devre dışı bırak

1. Azure portal **Log Analytics çalışma alanları** menüsünü açın ve etkinlik günlüğünü toplamak için çalışma alanını seçin.
2. Çalışma alanının menüsündeki **çalışma alanı veri kaynakları** bölümünde **Azure etkinlik günlüğü**' nü seçin.
3. Bağlantısını kesmek istediğiniz aboneliğe tıklayın.
4. **Bağlantıyı kes** ' e tıklayın ve ardından seçiminizi onaylamanız istendiğinde **Evet** ' e tıklayın.

### <a name="disable-log-profile"></a>Günlük profilini devre dışı bırak

1. Eski ayarları açmak için [eski ayarlarla çalışma](#work-with-legacy-settings) bölümünde açıklanan yordamı kullanın.
2. Depolama veya Olay Hub 'ları için geçerli koleksiyonu devre dışı bırakın.



## <a name="activity-log-monitoring-solution"></a>Etkinlik günlüğü izleme çözümü
Azure Log Analytics izleme çözümü, Log Analytics çalışma alanınızdaki etkinlik günlüğü kayıtlarını çözümlemek için birden çok günlük sorgusu ve görünümü içerir. Bu çözüm bir Log Analytics çalışma alanında toplanan günlük verilerini kullanır ve tanılama ayarlarını kullanarak etkinlik günlüğünü topladıysanız hiçbir değişiklik yapılmadan çalışmaya devam edecektir. Bu çözümle ilgili ayrıntılar için [etkinlik günlükleri analitik izleme çözümüne](activity-log-collect.md#activity-logs-analytics-monitoring-solution) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüğü hakkında daha fazla bilgi edinin](../../azure-resource-manager/management/view-activity-logs.md)
* [Tanılama ayarları hakkında daha fazla bilgi edinin](diagnostic-settings.md)
