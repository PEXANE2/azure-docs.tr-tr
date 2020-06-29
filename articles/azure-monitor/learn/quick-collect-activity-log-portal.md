---
title: Azure portal kullanarak Log Analytics çalışma alanına Azure etkinlik günlüğü gönderme
description: Etkinlik günlüğünü Azure Izleyici günlüklerine göndermek için bir Log Analytics çalışma alanı ve bir tanılama ayarı oluşturmak için Azure portal kullanın.
ms.subservice: logs
ms.topic: quickstart
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: 6f13a465aa43f0c45fa99d2ea396a5ca92d75d66
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85446426"
---
# <a name="send-azure-activity-log-to-log-analytics-workspace-using-azure-portal"></a>Azure portal kullanarak Log Analytics çalışma alanına Azure etkinlik günlüğü gönderme
Etkinlik günlüğü, Azure 'da abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir platform Günlüğliğidir. Bu, bir kaynağın değiştirildiği veya bir sanal makinenin başlatıldığı zaman gibi bilgileri içerir. Etkinlik günlüğünü Azure portal görüntüleyebilir veya PowerShell ve CLı ile girdileri alabilirsiniz. Bu hızlı başlangıçta, [günlük sorgularını](../log-query/log-query-overview.md) kullanarak analiz oluşturabileceğiniz ve [günlük uyarıları](../platform/alerts-log-query.md) ve [çalışma kitapları](../platform/workbooks-overview.md)gibi diğer özellikleri etkinleştirerek, etkinlik günlüğünü Azure izleyici günlüklerine göndermek için Azure Portal bir Log Analytics çalışma alanı ve bir tanılama ayarı oluşturmak üzere nasıl kullanılacağı gösterilmektedir. 

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 



## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma
Azure portal, **Log Analytics çalışma alanları**' nı arayıp seçin. 

![Azure portal](media/quick-create-workspace/azure-portal-01.png)
  
**Ekle**' ye tıklayın ve ardından **kaynak grubu**, çalışma alanı **adı**ve **konum**için değerler sağlayın. Çalışma alanı adı tüm Azure abonelikleri genelinde benzersiz olmalıdır.

![Çalışma alanı oluşturma](media/quick-collect-activity-log/create-workspace.png)

Ayarları gözden geçirmek için **gözden geçir + oluştur** ' a tıklayın ve ardından çalışma alanını oluşturmak için **oluşturun** . Bu, yeterli miktarda veri toplamaya başlamadan önce bir değişiklik yapmayacak **şekilde Kullandıkça Öde** varsayılan fiyatlandırma katmanını seçer. Etkinlik günlüğünün toplanması için ücret alınmaz.


## <a name="create-diagnostic-setting"></a>Tanılama ayarı oluşturma
Azure portal için arama yapın ve ardından **izleyici**' yi seçin. 

![Azure portal](media/quick-collect-activity-log/azure-portal-monitor.png)

**Etkinlik günlüğü**' nü seçin. Geçerli abonelik için son olayları görmeniz gerekir. Aboneliğin tanılama ayarlarını görüntülemek için **Tanılama ayarları** ' na tıklayın.

![Etkinlik günlüğü](media/quick-collect-activity-log/activity-log.png)

Yeni bir ayar oluşturmak için **Tanılama ayarı Ekle** ' ye tıklayın. 

![Tanılama ayarı oluşturma](media/quick-collect-activity-log/create-diagnostic-setting.png)

*Çalışma alanına etkinlik günlüğü gönder*gibi bir ad yazın. Kategorilerin her birini seçin. Tek hedef olarak **Log Analytics gönder** ' i seçin ve ardından oluşturduğunuz çalışma alanını belirtin. Tanılama ayarını oluşturmak için **Kaydet** ' e tıklayın ve ardından sayfayı kapatın.

![Yeni tanılama ayarı](media/quick-collect-activity-log/new-diagnostic-setting.png)

## <a name="generate-log-data"></a>Günlük verileri oluştur
Log Analytics çalışma alanına yalnızca yeni etkinlik günlüğü girdileri gönderilir, bu nedenle aboneliğinizde bir sanal makineyi başlatma veya durdurma ya da başka bir kaynağı oluşturma veya değiştirme gibi günlüğe kaydedilecek bazı eylemler gerçekleştirin. Tanılama ayarının oluşturulması ve verilerin başlangıçta çalışma alanına yazılması için birkaç dakika beklemeniz gerekebilir. Bu gecikmeden sonra, etkinlik günlüğüne yazılan tüm olaylar çalışma alanına birkaç saniye içinde gönderilir.

## <a name="retrieve-data-with-a-log-query"></a>Günlük sorgusuyla veri alma

**Azure izleyici** menüsünde **Günlükler** ' i seçin. **Örnek sorgular** sayfasını kapatın. Kapsam, oluşturduğunuz çalışma alanına ayarlanmamışsa **kapsam Seç** ' e tıklayın ve bulun.

![Log Analytics kapsamı](media/quick-collect-activity-log/log-analytics-scope.png)

Sorgu penceresinde, yazın `AzureActivity` ve **Çalıştır**' a tıklayın. Bu, etkinlik günlüğünden gönderilen tüm kayıtları içeren *AzureActivity* tablosundaki tüm kayıtları döndüren basit bir sorgudur.

![Basit sorgu](media/quick-collect-activity-log/query-01.png)

Ayrıntılı özelliklerini görüntülemek için kayıtlardan birini genişletin.

![Özellikleri Genişlet](media/quick-collect-activity-log/expand-properties.png)

`AzureActivity | summarize count() by CategoryValue`Kategoriye göre özetlenen olay sayısını veren gibi daha karmaşık bir sorgu deneyin.

![Karmaşık sorgu](media/quick-collect-activity-log/query-02.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, etkinlik günlüğünü bir Log Analytics çalışma alanına gönderilmek üzere yapılandırdınız. Artık diğer verileri, Azure Izleyici 'deki [günlük sorgularını](../log-query/log-query-overview.md) kullanarak analiz oluşturabileceğiniz ve [günlük uyarıları](../platform/alerts-log-query.md) ve [çalışma kitapları](../platform/workbooks-overview.md)gibi özelliklerden yararlanarak, çalışma alanına toplanacak şekilde yapılandırabilirsiniz. Daha sonra, her kaynakta gerçekleştirilen işlemlere ilişkin Öngörüler sağlayan etkinlik günlüğündeki verileri karmaşıklama Azure kaynaklarınızdan [kaynak günlüklerini](../platform/resource-logs.md) toplamanız gerekir.


> [!div class="nextstepaction"]
> [Azure Izleyici ile kaynak günlüklerini toplayın ve çözümleyin](tutorial-resource-logs.md)
