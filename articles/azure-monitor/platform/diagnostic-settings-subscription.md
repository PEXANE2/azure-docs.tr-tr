---
title: Tanılama ayarları (Önizleme) ile Azure etkinlik günlüğü toplama-Azure Izleyici | Microsoft Docs
description: Azure etkinlik günlüklerini Azure Izleyici günlüklerine, Azure depolama 'ya veya Azure Event Hubs iletmek için tanılama ayarlarını kullanın.
author: bwren
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
ms.author: bwren
ms.date: 10/31/2019
ms.openlocfilehash: 9f8783dc6d3c14b086364639b60273dbae626cee
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73587985"
---
# <a name="collect-azure-activity-log-with-diagnostic-settings-preview"></a>Tanılama ayarlarıyla Azure etkinlik günlüğü toplama (Önizleme)
[Azure etkinlik günlüğü](activity-logs-overview.md) , Azure 'da oluşan abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir [Platform günlüğliğidir](platform-logs-overview.md) . Bu aşamada, etkinlik günlüğü girdilerini bir [Olay Hub 'ına veya depolama hesabına](activity-log-export.md) göndermek için bir günlük profili oluşturdunuz ve bir [Log Analytics çalışma alanına](activity-log-collect.md)toplamak için bağlayıcı kullandınız.

Artık, [kaynak günlüklerini](resource-logs-overview.md)toplamak için kullanılan [tanılama ayarlarını](diagnostic-settings.md) kullanarak Azure etkinlik günlüğü toplamasını yapılandırabilirsiniz. Tanılama ayarlarının kullanılması geçerli yöntemlere göre aşağıdaki avantajları sağlar:

- Tüm platform günlüklerini toplamak için tutarlı yöntem.
- Birden çok abonelik ve kiracılar arasında etkinlik günlüğü toplayın.
- Koleksiyonu yalnızca belirli kategoriler için günlükleri toplayacak şekilde filtreleyin.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Bu özelliği etkinleştirmeden önce tanılama ayarlarını kullanarak etkinlik günlüğü koleksiyonunun aşağıdaki ayrıntılarını göz önünde bulundurun.

- Tanılama ayarlarını kullanarak etkinleştirilmeden önce var olan etkinlik koleksiyonunu devre dışı bırakmanız gerekir. Her ikisinin de etkinleştirilmesi yinelenen verilerin oluşmasına neden olabilir.
- Etkinlik günlüğünün Azure depolama 'ya toplanması için bekletme ayarı kaldırılmıştır ve bu, verilerin siz kaldırana kadar süresiz olarak depolanacağı anlamına gelir.
- Birden çok abonelikteki etkinlik günlüğünü tek bir Log Analytics çalışma alanına gönderebilirsiniz. Etkinlik günlüğünü en fazla beş Log Analytics çalışma alanına, tek bir abonelikten gönderebilirsiniz.

## <a name="configure-diagnostic-settings"></a>Tanılama ayarlarını yapılandırma
Azure etkinlik günlüğünü toplamak için Azure portal bir tanılama ayarı oluşturmak için aşağıdaki yordamı kullanın. Diğer yöntemleri kullanarak şu anda bir abonelik düzeyi tanılama ayarı oluşturamazsınız.

1. Etkinlik günlüğü için mevcut Log Analytics çalışma alanı koleksiyonunu devre dışı bırak:
   1. Azure portal **Log Analytics çalışma alanları** menüsünü açın ve etkinlik günlüğünü toplamak için çalışma alanını seçin.
   2. Çalışma alanının menüsündeki **çalışma alanı veri kaynakları** bölümünde **Azure etkinlik günlüğü**' nü seçin.
   3. Bağlantısını kesmek istediğiniz aboneliğe tıklayın.
   4. **Bağlantıyı kes** ' e tıklayın ve ardından seçiminizi onaylamanız istendiğinde **Evet** ' e tıklayın.
2. Azure portal **Azure izleyici** menüsünde **etkinlik günlüğü**' nü seçin.
3. **Tanılama ayarları**'na tıklayın.
   
   ![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)
   
4. Eski deneyimin mor başlığına tıklayın ve tüm geçerli koleksiyonları depolama veya Olay Hub 'larına devre dışı bırakın. 

    ![Eski deneyim](media/diagnostic-settings-subscription/legacy-experience.png)

5. Tanılama ayarı oluşturmak için [Azure Portal tanılama ayarlarını oluşturma](diagnostic-settings.md#create-diagnostic-settings-in-azure-portal) bölümündeki yordamları izleyin. Etkinlik günlüğünden olayları filtrelemek için kullanabileceğiniz kategorilerin bir açıklaması için [etkinlik günlüğündeki Kategoriler](activity-logs-overview.md#categories-in-the-activity-log) bölümüne bakın. 


## <a name="differences-in-data"></a>Verilerdeki farklılıklar
Tanılama ayarları, aşağıdaki geçerli farklılıklarla etkinlik günlüğünü toplamak için kullanılan önceki yöntemlerle aynı verileri toplar:

Aşağıdaki özellikler kaldırılmıştır:

- ActivityStatus
- Etkinlik alt durumu
- için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir
- ResourceProvider 

Aşağıdaki özellikler eklendi:

- Authorization_d
- Claims_d
- Properties_d

## <a name="activity-log-monitoring-solution"></a>Etkinlik günlüğü izleme çözümü
Azure Log Analytics izleme çözümü, Log Analytics çalışma alanınızdaki etkinlik günlüğü kayıtlarını çözümlemek için birden çok günlük sorgusu ve görünümü içerir. Bu çözüm bir Log Analytics çalışma alanında toplanan günlük verilerini kullanır ve tanılama ayarlarını kullanarak etkinlik günlüğünü topladıysanız hiçbir değişiklik yapılmadan çalışmaya devam edecektir. Bu çözümle ilgili ayrıntılar için [etkinlik günlükleri analitik izleme çözümüne](activity-log-collect.md#activity-logs-analytics-monitoring-solution) bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Etkinlik günlüğü hakkında daha fazla bilgi edinin](../../azure-resource-manager/resource-group-audit.md)
* [Tanılama ayarları hakkında daha fazla bilgi edinin](diagnostic-settings.md)
