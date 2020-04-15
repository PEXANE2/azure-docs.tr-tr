---
title: Azure Monitör'de Azure etkinlik günlüğünü toplama ve analiz
description: Azure Monitör Günlüklerinde Azure Etkinlik Günlüğü'nün toplanmasını ve tüm Azure aboneliklerinizde Azure etkinlik günlüğünü analiz etmek ve arama yapmak için izleme çözümünü kullanın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/14/2020
ms.openlocfilehash: 098aeaa06a26c57744402722aa3eacc51ea85fb7
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382864"
---
# <a name="collect-and-analyze-azure-activity-log-in-azure-monitor"></a>Azure Monitör'de Azure Etkinliği oturumlarını toplama ve analiz
[Azure Etkinliği günlüğü,](platform-logs-overview.md) Azure'da gerçekleşen abonelik düzeyi olaylarına ilişkin bilgi sağlayan bir platform [günlüğüdür.](platform-logs-overview.md) Azure portalındaki Etkinlik günlüğünü görüntüleyebilmenize ekolarak, Azure Monitor'un ek özelliklerini etkinleştirmek için günlük analitiği çalışma alanına gönderecek şekilde yapılandırmanız gerekir. Bu makalede, bu yapılandırmanın nasıl gerçekleştirilişini ve Etkinlik günlüğünün Azure depolama ve etkinlik merkezlerine nasıl gönderilen açıklar.

Günlük Analizi çalışma alanında Etkinlik Günlüğü'nün toplanması aşağıdaki avantajları sağlar:

- Log Analytics çalışma alanında depolanan Etkinlik günlüğü verileri için veri alımı veya veri saklama ücreti alınmaz.
- Etkinlik günlüğü verilerini Azure Monitor tarafından toplanan diğer izleme verileriyle ilişkilendirin.
- Karmaşık çözümleme gerçekleştirmek ve Etkinlik Günlüğü girişleri hakkında derin bilgiler edinmek için günlük sorgularını kullanın.
- Daha karmaşık uyarı mantığına izin veren Etkinlik girişleri içeren günlük uyarılarını kullanın.
- Etkinlik günlük girişlerini 90 günden uzun süre saklayın.
- Birden çok Azure aboneliğinden ve kiracıdan gelen günlük girişlerini birlikte analiz için tek bir konumda birleştirin.

> [!IMPORTANT]
> Kiracılar arasında günlükleri toplamak [Azure Deniz Feneri](/azure/lighthouse)gerektirir.

## <a name="collecting-activity-log"></a>Toplama Etkinlik günlüğü
Etkinlik günlüğü, [Azure portalında görüntülenmek](activity-log-view.md)için otomatik olarak toplanır. Günlük Analizi çalışma alanında toplamak veya Azure depolama veya etkinlik hub'ları göndermek için bir [tanılama ayarı](diagnostic-settings.md)oluşturun. Bu, tüm platform günlükleri için tutarlı hale kaynak günlükleri tarafından kullanılan aynı [yöntemdir.](platform-logs-overview.md)  

Etkinlik günlüğü için tanılama ayarı oluşturmak için Azure Monitor'daki **Etkinlik günlüğü** menüsünden **Tanılama ayarlarını** seçin. Bkz. Ayarı oluşturma yla ilgili ayrıntılar için [Azure'da platform günlüklerini ve ölçümlerini toplamak için tanılama ayarı](diagnostic-settings.md) oluştur' ayarı. Filtre uygulayabileceğiniz kategorilerin açıklaması için [Etkinlik günlüğündeki Kategorilere](activity-log-view.md#categories-in-the-activity-log) bakın. Eski ayarlarınız varsa, tanılama ayarı oluşturmadan önce bunları devre dışı bıraktığından emin olun. Her ikisinin de etkin olması, yinelenen verilere neden olabilir.

![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)


> [!NOTE]
> Şu anda, yalnızca Azure portalı ve Kaynak Yöneticisi şablonu kullanarak bir abonelik düzeyi tanılama ayarı oluşturabilirsiniz. 


## <a name="legacy-settings"></a>Eski ayarlar 
Tanılama ayarları, Etkinlik günlüğünü farklı hedeflere göndermek için tercih edilen yöntem olsa da, tanılama ayarı yla değiştirmeyi seçmezseniz eski yöntemler çalışmaya devam eder. Tanılama ayarları eski yöntemlere göre aşağıdaki avantajlara sahiptir ve yapılandırmanızı güncelleştirmeniz önerilir:

- Tüm platform günlüklerini toplamak için tutarlı bir yöntem.
- Birden çok abonelik ve kiracılar arasında Etkinlik günlüğü toplayın.
- Yalnızca belirli kategoriler için günlükleri toplamak için koleksiyonu filtreleyin.
- Tüm Etkinlik günlüğü kategorilerini toplayın. Bazı kategoriler eski yöntem kullanılarak toplanmaz.
- Günlük alımı için daha hızlı gecikme. Tanı ayarları yalnızca yaklaşık 1 dakika eklerken önceki yöntem yaklaşık 15 dakika gecikme vardır.



### <a name="log-profiles"></a>Günlük profilleri
Günlük profilleri, Etkinlik günlüğünü Azure depolama alanına veya etkinlik merkezlerine göndermek için kullanılan eski yöntemdir. Bir günlük profiliyle çalışmaya devam etmek veya tanılama ayarına geçiş ehazırlık olarak devre dışı kalmak için aşağıdaki yordamı kullanın.

1. Azure portalındaki **Azure Monitör** menüsünden **Etkinlik günlüğü'nü**seçin.
3. **Tanılama ayarları**'na tıklayın.

   ![Tanılama ayarları](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Eski deneyim için mor banner'ı tıklatın.

    ![Eski deneyim](media/diagnostic-settings-subscription/legacy-experience.png)

### <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı
Günlük Analizi çalışma alanına Etkinlik günlüğünü toplamak için eski yöntem, günlük çalışma alanı yapılandırmasındaki günlüğü bağlamaktır. 

1. Azure portalındaki **Günlük Analizi çalışma alanları** menüsünden Etkinlik Günlüğü'nü toplamak için çalışma alanını seçin.
1. Çalışma **alanı** menüsünün Çalışma Alanı Veri Kaynakları bölümünde Azure **Etkinliği günlüğünü**seçin.
1. Bağlanmak istediğiniz aboneliği tıklatın.

    ![Çalışma Alanları](media/activity-log-collect/workspaces.png)

1. Abonelikteki Etkinlik günlüğünü seçili çalışma alanına bağlamak için **Bağlan'ı** tıklatın. Abonelik zaten başka bir çalışma alanına bağlıysa, bağlantısını kesmek için önce **Bağlantıyı Kes'i** tıklatın.

    ![Çalışma Alanlarını Bağla](media/activity-log-collect/connect-workspace.png)


Ayarı devre dışı kaldırmak için aynı yordamı gerçekleştirin ve aboneliği çalışma alanından kaldırmak için **Bağlantıyı Kes'i** tıklatın.


## <a name="analyze-activity-log-in-log-analytics-workspace"></a>Log Analytics çalışma alanında Etkinlik oturumlarını analiz et
Bir Etkinlik Günlüğü'nün Günlük Analizi çalışma alanına bağlandığınızda, girişler çalışma alanına bir [günlük sorgusuyla](../log-query/log-query-overview.md)alabileceğiniz *AzureActivity* adlı bir tabloya yazılır. Bu tablonun yapısı [günlük girişikategorisine](activity-log-view.md#categories-in-the-activity-log)bağlı olarak değişir. Her kategorinin açıklaması için [Azure Etkinlik Günlüğü etkinlik şemasına](activity-log-schema.md) bakın.


### <a name="data-structure-changes"></a>Veri yapısı değişiklikleri
Tanılama ayarları, *Azure Etkinliği* tablosunun yapısında yapılan bazı değişikliklerle Etkinlik günlüğünü toplamak için kullanılan eski yöntemle aynı verileri toplar.

Aşağıdaki tablodaki sütunlar güncelleştirilmiş şemada küçümsülmüyor. *AzureActivity'de* hala bulunurlar, ancak verileri olmaz. Bu sütunların değiştirilmesi yeni değildir, ancak azalan sütunla aynı verileri içerir. Bunlar farklı bir biçimde olduğundan, bunları kullanan günlük sorgularını değiştirmeniz gerekebilir. 

| Amortismana uymulan sütun | Yedek sütun |
|:---|:---|
| Etkinlik Durumu    | ActivityStatusValue    |
| ActivitySubstatus | ActivitySubstatusValue |
| ThrottledRequests     | OperationNameValue     |
| ResourceProvider  | Kaynak Sağlayıcı Değeri  |

> [!IMPORTANT]
> Bazı durumlarda, bu sütunlarda değerleri tüm büyük harf olabilir. Bu sütunları içeren bir sorgunuz varsa, büyük/küçük harf duyarsız karşılaştırması yapmak için [=~ işleci](https://docs.microsoft.com/azure/kusto/query/datatypes-string-operators) kullanmalısınız.

Aşağıdaki sütun güncelleştirilmiş şemada *AzureEtkinliği'ne* eklendi:

- Authorization_d
- Claims_d
- Properties_d


## <a name="activity-logs-analytics-monitoring-solution"></a>Aktivite Günlükleri Analiz izleme çözümü
Azure Log Analytics izleme çözümü yakında amortismana tabi olacak ve Log Analytics çalışma alanındaki güncelleştirilmiş şema kullanılarak bir çalışma kitabı yla değiştirilecektir. Çözümü zaten etkinleştirdiyseniz de kullanabilirsiniz, ancak yalnızca eski ayarları kullanarak Etkinlik günlüğünü topluyorsanız kullanılabilir. 



### <a name="use-the-solution"></a>Çözümü kullanın
İzleme çözümlerine Azure portalındaki **Monitör** menüsünden erişilir. Çözüm kutucuklarıyla **Genel Bakış** sayfasını açmak için **Öngörüler** bölümünde **Daha Fazla'yı** seçin. **Azure Etkinlik Günlükleri döşemesi,** çalışma alanınızdaki **Azure Etkinliği** kayıtlarının sayısını görüntüler.

![Azure Etkinlik Günlükleri döşeme](media/collect-activity-logs/azure-activity-logs-tile.png)


Azure **Etkinlik Günlükleri** görünümünü açmak için Azure Etkinlik **Günlükleri** döşemesini tıklatın. Görünüm aşağıdaki tabloda görselleştirme bölümlerini içerir. Her parça, belirtilen zaman aralığı için bu parçaların ölçütleriyle eşleşen en fazla 10 öğeyi listeler. Bölümün altındaki **tümlerini gör'e** tıklayarak eşleşen tüm kayıtları döndüren bir günlük sorgusu çalıştırabilirsiniz.

![Azure Etkinlik Günlükleri panosu](media/collect-activity-logs/activity-log-dash.png)


### <a name="enable-the-solution-for-new-subscriptions"></a>Yeni abonelikler için çözümü etkinleştirme
Azure portalını kullanarak aboneliğinize Etkinlik Günlükleri Analizi çözümlerini artık ekleyemeyeceksiniz. Kaynak yöneticisi şablonuyla aşağıdaki yordamı kullanarak ekleyebilirsiniz. 

1. Aşağıdaki json'u *ActivityLogTemplate*.json adlı bir dosyaya kopyalayın.

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

- [Etkinlik Günlüğü](platform-logs-overview.md)hakkında daha fazla bilgi edinin.
- Azure Monitör [veri platformu](data-platform.md)hakkında daha fazla bilgi edinin.
- Etkinlik Günlüğünüzdeki ayrıntılı bilgileri görüntülemek için [günlük sorgularını](../log-query/log-query-overview.md) kullanın.
