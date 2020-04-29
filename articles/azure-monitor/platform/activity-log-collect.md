---
title: Azure Izleyici 'de Azure etkinlik günlüğünü toplayın ve çözümleyin
description: Azure etkinlik günlüğünü Azure Izleyici günlüklerine toplayın ve izleme çözümünü kullanarak Azure etkinlik günlüğünü analiz edin ve tüm Azure aboneliklerinizde arayın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/14/2020
ms.openlocfilehash: 098aeaa06a26c57744402722aa3eacc51ea85fb7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81382864"
---
# <a name="collect-and-analyze-azure-activity-log-in-azure-monitor"></a>Azure Izleyici 'de Azure etkinlik günlüğünü toplayın ve çözümleyin
[Azure etkinlik günlüğü](platform-logs-overview.md) , Azure 'da oluşan abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir [Platform günlüğliğidir](platform-logs-overview.md) . Azure portal etkinlik günlüğünü görüntüleyebilmeniz mümkün olsa da, Azure Izleyici 'nin ek özelliklerini etkinleştirmek üzere bir Log Analytics çalışma alanına gönderecek şekilde yapılandırmanız gerekir. Bu makalede, bu yapılandırmanın nasıl gerçekleştirileceği ve Etkinlik günlüğünün Azure depolama ve Olay Hub 'larına gönderilmesi açıklanmaktadır.

Etkinlik günlüğünün bir Log Analytics çalışma alanında toplanması aşağıdaki avantajları sağlar:

- Log Analytics çalışma alanında depolanan etkinlik günlüğü verileri için veri alımı veya veri saklama ücreti alınmaz.
- Etkinlik günlüğü verilerinin Azure Izleyici tarafından toplanan diğer izleme verileriyle ilişkilendirilmesi.
- Karmaşık analizler gerçekleştirmek ve etkinlik günlüğü girişleriyle ilgili ayrıntılı Öngörüler elde etmek için günlük sorgularını kullanın.
- Daha karmaşık uyarı mantığına izin veren etkinlik girişleriyle birlikte günlük uyarılarını kullanın.
- Etkinlik günlüğü girdilerini 90 günden daha uzun bir süre için depolayın.
- Birden çok Azure aboneliği ve kiracısından günlük girişlerini analiz için tek bir konuma birleştirin.

> [!IMPORTANT]
> Kiracılar arasında günlüklerin toplanması için [Azure açık Thouse](/azure/lighthouse)gerekir.

## <a name="collecting-activity-log"></a>Etkinlik günlüğü toplanıyor
Etkinlik günlüğü [Azure Portal görüntülenmek](activity-log-view.md)üzere otomatik olarak toplanır. Bunu bir Log Analytics çalışma alanında toplamak veya Azure Storage ya da Olay Hub 'ları göndermek için bir [Tanılama ayarı](diagnostic-settings.md)oluşturun. Bu, tüm [Platform günlükleri](platform-logs-overview.md)için tutarlı hale getiren kaynak günlükleri tarafından kullanılan yöntem ile aynıdır.  

Etkinlik günlüğü için bir tanılama ayarı oluşturmak üzere Azure Izleyici 'de **etkinlik günlüğü** menüsünden **Tanılama ayarları** ' nı seçin. Ayarı oluşturma hakkında ayrıntılı bilgi için bkz. [Azure 'da platform günlüklerini ve ölçümlerini toplamak için tanılama ayarı oluşturma](diagnostic-settings.md) . Filtreleyebileceğiniz kategorilerin bir açıklaması için [etkinlik günlüğündeki Kategoriler](activity-log-view.md#categories-in-the-activity-log) bölümüne bakın. Eski ayarlara sahipseniz, bir tanılama ayarı oluşturmadan önce bu ayarları devre dışı bıraktığınızdan emin olun. Her ikisinin de etkinleştirilmesi yinelenen verilerin oluşmasına neden olabilir.

![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)


> [!NOTE]
> Şu anda yalnızca Azure portal ve Kaynak Yöneticisi şablonunu kullanarak bir abonelik düzeyi tanılama ayarı oluşturabilirsiniz. 


## <a name="legacy-settings"></a>Eski ayarlar 
Tanılama ayarları, etkinlik günlüğünü farklı hedeflere göndermek için tercih edilen yöntem olmakla birlikte, bir tanılama ayarıyla değiştirmeyi tercih ederseniz eski yöntemler çalışmaya devam edecektir. Tanılama ayarları eski yöntemlere göre aşağıdaki avantajları sunar ve yapılandırmanızı güncelleştirmeniz önerilir:

- Tüm platform günlüklerini toplamak için tutarlı yöntem.
- Birden çok abonelik ve kiracılar arasında etkinlik günlüğü toplayın.
- Koleksiyonu yalnızca belirli kategoriler için günlükleri toplayacak şekilde filtreleyin.
- Tüm etkinlik günlüğü kategorilerini toplayın. Bazı kategoriler eski yöntem kullanılarak toplanmaz.
- Günlük alımı için daha hızlı gecikme. Tanılama ayarları yalnızca yaklaşık 1 dakika eklediğinde önceki yöntemde yaklaşık 15 dakikalık gecikme vardır.



### <a name="log-profiles"></a>Günlük profilleri
Günlük profilleri, Azure depolama veya Olay Hub 'larına etkinlik günlüğü göndermek için eski yöntemdir. Bir günlük profiliyle çalışmaya devam etmek veya bir tanılama ayarına geçiş hazırlığı sırasında devre dışı bırakmak için aşağıdaki yordamı kullanın.

1. Azure portal **Azure izleyici** menüsünde **etkinlik günlüğü**' nü seçin.
3. **Tanılama ayarları**'na tıklayın.

   ![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Eski deneyimin mor başlığına tıklayın.

    ![Eski deneyim](media/diagnostic-settings-subscription/legacy-experience.png)

### <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı
Etkinlik günlüğünün bir Log Analytics çalışma alanında toplanması için eski yöntem, günlüğü çalışma alanı yapılandırmasında bağluyor. 

1. Azure portal **Log Analytics çalışma alanları** menüsünde, etkinlik günlüğünü toplamak için çalışma alanını seçin.
1. Çalışma alanının menüsündeki **çalışma alanı veri kaynakları** bölümünde **Azure etkinlik günlüğü**' nü seçin.
1. Bağlanmak istediğiniz aboneliğe tıklayın.

    ![Çalışma Alanları](media/activity-log-collect/workspaces.png)

1. Abonelikteki etkinlik günlüğünü seçili çalışma alanına bağlamak için **Bağlan** ' a tıklayın. Abonelik zaten başka bir çalışma alanına bağlıysa bağlantıyı kesmek için önce **bağlantıyı kes** ' e tıklayın.

    ![Çalışma alanlarını bağlama](media/activity-log-collect/connect-workspace.png)


Ayarı devre dışı bırakmak için aynı yordamı gerçekleştirin ve aboneliği çalışma alanından kaldırmak için **bağlantıyı kes** ' e tıklayın.


## <a name="analyze-activity-log-in-log-analytics-workspace"></a>Log Analytics çalışma alanında etkinlik günlüğünü çözümle
Bir etkinlik günlüğünü Log Analytics çalışma alanına bağladığınızda, girdiler çalışma alanına, bir [günlük sorgusuyla](../log-query/log-query-overview.md)alabileceğiniz *AzureActivity* adlı bir tabloya yazılır. Bu tablonun yapısı, [günlük girişinin kategorisine](activity-log-view.md#categories-in-the-activity-log)bağlı olarak değişir. Her kategorinin açıklaması için bkz. [Azure etkinlik günlüğü olay şeması](activity-log-schema.md) .


### <a name="data-structure-changes"></a>Veri yapısı değişiklikleri
Tanılama ayarları, *AzureActivity* tablosunun yapısındaki bazı değişikliklerle etkinlik günlüğünü toplamak için kullanılan eski yöntemle aynı verileri toplar.

Aşağıdaki tablodaki sütunlar güncelleştirilmiş şemada kullanımdan kaldırılmıştır. Hala *AzureActivity* içinde bulunur, ancak hiçbir veri içermez. Bu sütunların yerini değiştirme yeni değil, ancak kullanım dışı sütunuyla aynı verileri içerir. Farklı bir biçimde olduklarından, bunları kullanan günlük sorgularını değiştirmeniz gerekebilir. 

| Kullanım dışı sütun | Değiştirme sütunu |
|:---|:---|
| ActivityStatus    | ActivityStatusValue    |
| Etkinlik alt durumu | ActivitySubstatusValue |
| ThrottledRequests     | OperationNameValue     |
| ResourceProvider  | ResourceProviderValue  |

> [!IMPORTANT]
> Bazı durumlarda, bu sütunlardaki değerler tüm büyük harfle olabilir. Bu sütunları içeren bir sorgunuz varsa, büyük/küçük harfe duyarsız bir karşılaştırma yapmak için [= ~ işlecini](https://docs.microsoft.com/azure/kusto/query/datatypes-string-operators) kullanmanız gerekir.

Güncelleştirilmiş şemada *AzureActivity* 'e aşağıdaki sütun eklenmiştir:

- Authorization_d
- Claims_d
- Properties_d


## <a name="activity-logs-analytics-monitoring-solution"></a>Etkinlik günlükleri analitik izleme çözümü
Azure Log Analytics izleme çözümü yakında kullanımdan kalkmış ve Log Analytics çalışma alanındaki güncelleştirilmiş şemayı kullanarak bir çalışma kitabı tarafından değiştirilmeyecektir. Hala etkinse çözümü kullanabilirsiniz, ancak yalnızca eski ayarları kullanarak etkinlik günlüğü topluyorsanız kullanılabilir. 



### <a name="use-the-solution"></a>Çözümü kullanma
**İzleme çözümlerine Azure Portal izleme menüsünden erişilir** . **Genel bakış** sayfasını çözüm kutucukları ile açmak için **Öngörüler** bölümünde **daha fazla** ' yı seçin. **Azure etkinlik günlükleri** kutucuğu, çalışma alanınızdaki **AzureActivity** kayıt sayısının sayısını görüntüler.

![Azure etkinlik günlükleri kutucuğu](media/collect-activity-logs/azure-activity-logs-tile.png)


Azure **etkinlik günlükleri görünümünü açmak** için Azure etkinlik **günlükleri** kutucuğuna tıklayın. Görünüm, aşağıdaki tablodaki görselleştirme parçalarını içerir. Her bölümde, belirtilen zaman aralığı için parçaların ölçütlerine uyan 10 ' a kadar öğe listelenir. Bölümün en altında bulunan **Tümünü görüntüle ' ye** tıklayarak eşleşen tüm kayıtları döndüren bir günlük sorgusu çalıştırabilirsiniz.

![Azure etkinlik günlükleri panosu](media/collect-activity-logs/activity-log-dash.png)


### <a name="enable-the-solution-for-new-subscriptions"></a>Yeni abonelikler için çözümü etkinleştirin
Yakında etkinlik günlüğü Analizi çözümünü Azure portal kullanarak aboneliğinize ekleyemeyeceksiniz. Bir Resource Manager şablonuyla aşağıdaki yordamı kullanarak ekleyebilirsiniz. 

1. Aşağıdaki JSON *öğesini Activitylogtemplate*. JSON adlı bir dosyaya kopyalayın.

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "defaultValue": "my-workspace",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "east us",
              "west us",
              "australia central",
              "west europe"
            ],
            "defaultValue": "australia central",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        }
      },
        "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "features": {
                    "searchVersion": 2
                }
            }
        },
        {
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "2015-11-01-preview",
            "name": "[concat('AzureActivity(', parameters('workspaceName'),')')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]"
            ],
            "plan": {
                "name": "[concat('AzureActivity(', parameters('workspaceName'),')')]",
                "promotionCode": "",
                "product": "OMSGallery/AzureActivity",
                "publisher": "Microsoft"
            },
            "properties": {
                "workspaceResourceId": "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]",
                "containedResources": [
                    "[concat(resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName')), '/views/AzureActivity(',parameters('workspaceName'))]"
                ]
            }
        },
        {
          "type": "Microsoft.OperationalInsights/workspaces/datasources",
          "kind": "AzureActivityLog",
          "name": "[concat(parameters('workspaceName'), '/', subscription().subscriptionId)]",
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "dependsOn": [
              "[parameters('WorkspaceName')]"
          ],
          "properties": {
              "linkedResourceId": "[concat(subscription().Id, '/providers/microsoft.insights/eventTypes/management')]"
          }
        }
      ]
    }    
    ```

2. Aşağıdaki PowerShell komutlarını kullanarak şablonu dağıtın:

    ```PowerShell
    Connect-AzAccount
    Select-AzSubscription <SubscriptionName>
    New-AzResourceGroupDeployment -Name activitysolution -ResourceGroupName <ResourceGroup> -TemplateFile <Path to template file>
    ```


## <a name="next-steps"></a>Sonraki adımlar

- [Etkinlik günlüğü](platform-logs-overview.md)hakkında daha fazla bilgi edinin.
- [Azure izleyici veri platformu](data-platform.md)hakkında daha fazla bilgi edinin.
- Etkinlik Günlüğliğinizden ayrıntılı bilgileri görüntülemek için [günlük sorgularını](../log-query/log-query-overview.md) kullanın.
