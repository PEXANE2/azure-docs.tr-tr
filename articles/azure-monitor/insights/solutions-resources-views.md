---
title: Yönetim çözümlerinde görüşler | Microsoft Dokümanlar
description: 'Yönetim çözümleri genellikle verileri görselleştirmek için bir veya daha fazla görünüm içerir.  Bu makalede, Görünüm Tasarımcısı tarafından oluşturulan bir görünümün nasıl dışa aktarılabildiğini ve yönetim çözümüne nasıl dahil edilebildiğini açıklanmaktadır. '
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/16/2018
ms.openlocfilehash: a9a1c1718fb95a6ace3700af043134072d582473
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663054"
---
# <a name="views-in-management-solutions-preview"></a>Yönetim çözümlerinde görünümler (Önizleme)
> [!NOTE]
> Bu, şu anda önizlemede olan yönetim çözümleri oluşturmak için ön belgelerdir. Aşağıda açıklanan herhangi bir şema değişebilir.    


[Yönetim çözümleri](solutions.md) genellikle verileri görselleştirmek için bir veya daha fazla görünüm içerir.  Bu makalede, [Görünüm Tasarımcısı](../../azure-monitor/platform/view-designer.md) tarafından oluşturulan bir görünümün nasıl dışa aktarılabildiğini ve yönetim çözümüne nasıl dahil edilebildiğini açıklanmaktadır.  

> [!NOTE]
> Bu makaledeki örnekler, yönetim çözümleri için gerekli veya ortak olan ve Tasarım'da açıklanan parametreleri ve değişkenleri kullanır [ve Azure'da bir yönetim çözümü oluşturur](solutions-creating.md)
>
>

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, bir yönetim çözümü ve [çözüm](solutions-creating.md) dosyasının yapısı oluşturmak için nasıl zaten bildiğinizi varsayar.

## <a name="overview"></a>Genel Bakış
Bir yönetim çözümüne görünüm eklemek için, [çözüm dosyasında](solutions-creating.md)bunun için bir **kaynak** oluşturursunuz.  Görünümün ayrıntılı yapılandırmasını açıklayan JSON, tipik bir çözüm yazarının el ile oluşturabileceği bir şey olmasa da genellikle karmaşıktır.  En yaygın yöntem [Görünüm Tasarımcısı](../../azure-monitor/platform/view-designer.md)kullanarak görünümü oluşturmakiçin, dışa aktarma ve sonra çözüme ayrıntılı yapılandırma eklemektir.

Bir çözüme görünüm eklemek için temel adımlar aşağıdaki gibidir.  Her adım aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır.

1. Görünümü bir dosyaya dışa aktarın.
2. Çözümde görünüm kaynağı oluşturun.
3. Görünüm ayrıntılarını ekleyin.

## <a name="export-the-view-to-a-file"></a>Görünümü bir dosyaya dışa aktarma
Bir dosyaya görünüm aktarmak için [Log Analytics View Designer'daki](../../azure-monitor/platform/view-designer.md) yönergeleri izleyin.  Dışa aktarılan [dosya, çözüm dosyasıyla](solutions-solution-file.md)aynı öğelerle JSON formatında olacaktır.  

Görünüm dosyasının **kaynak** öğesi, Log Analytics çalışma alanını temsil eden **microsoft.operationalinsights/çalışma alanları** türüne sahip bir kaynağa sahip olacaktır.  Bu öğe, görünümü temsil eden ve ayrıntılı yapılandırmasını içeren bir **görünüm** türüne sahip bir alt öğeye sahip olacaktır.  Bu öğenin ayrıntılarını kopyalayacak ve ardından çözümünüzün içine kopyalayacaktır.

## <a name="create-the-view-resource-in-the-solution"></a>Çözümde görünüm kaynağı oluşturma
Çözüm dosyanızın **kaynaklar** öğesine aşağıdaki görünüm kaynağını ekleyin.  Bu, aşağıda sizin de eklemeniz gereken değişkenleri kullanır.  **Pano** ve **OverviewTile** özelliklerinin, dışa aktarılan görünüm dosyasından ilgili özelliklerle üzerine yazacağınız yer tutucular olduğunu unutmayın.

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

Çözüm dosyasının değişkenler öğesine aşağıdaki değişkenleri ekleyin ve çözümünüzün değerlerini değiştirin.

    "LogAnalyticsApiVersion": "<api-version>",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."

Dışa aktarılan görünüm dosyanızdan tüm görünüm kaynağını kopyalayabileceğinizi, ancak çözümünüzde çalışabilmek için aşağıdaki değişiklikleri yapmanız gerektiğini unutmayın.  

* Görünüm kaynağının **türü** **görünümlerden** **Microsoft.OperationalInsights/çalışma alanlarına**değiştirilmelidir.
* Görünüm kaynağının **ad** özelliğinin çalışma alanı adını içerecek şekilde değiştirilmesi gerekir.
* Çalışma alanı kaynağı çözümde tanımlanmadığından, çalışma alanına olan bağımlılığın kaldırılması gerekir.
* **DisplayName** özelliğinin görünüme eklenmesi gerekir.  **Id**, **Ad**ve **DisplayName** tüm eşleşmeli.
* Parametre adları gerekli parametre kümesiyle eşleşecek şekilde değiştirilmelidir.
* Çözeltide değişkenler tanımlanmalı ve uygun özelliklerde kullanılmalıdır.

### <a name="log-analytics-api-version"></a>Log Analytics API sürümü
Kaynak Yöneticisi şablonunda tanımlanan tüm Log Analytics kaynakları, kaynağın kullanması gereken API sürümünü tanımlayan bir özellik **apiVersion'a** sahiptir.  Bu sürüm, [eski ve yükseltilmiş sorgu dilini](../../azure-monitor/log-query/log-query-overview.md)kullanan sorguları olan görünümler için farklıdır.  

 Aşağıdaki tablo, eski ve yükseltilmiş çalışma alanlarındaki görünümler için Log Analytics API sürümlerini belirtir: 

| Çalışma alanı sürümü | API sürümü | Sorgu |
|:---|:---|:---|
| v1 (eski)   | 2015-11-01-önizleme | Eski biçim.<br> Örnek: Type=Event EventLevelName = Hata  |
| v2 (yükseltilmiş) | 2015-11-01-önizleme | Eski biçim.  Yükleme de yükseltilmiş biçime dönüştürülür.<br> Örnek: Type=Event EventLevelName = Hata<br>Dönüştürülen: EventLevelName == "Hata" olay &#124;  |
| v2 (yükseltilmiş) | 2017-03-03-önizleme | Yükseltme biçimi. <br>Örnek: EventLevelName == "Hata" olay &#124;  |


## <a name="add-the-view-details"></a>Görünüm ayrıntılarını ekleme
Dışa aktarılan görünüm dosyasındaki görünüm kaynağı, Gösterge **Tablosu** ve **Genel BakışTile** adlı **özellikler** öğesinde görünümün ayrıntılı yapılandırmasını içeren iki öğe içerir.  Bu iki öğeyi ve içeriğini çözüm dosyanızdaki görünüm kaynağının **özellikleri** öğesine kopyalayın.

## <a name="example"></a>Örnek
Örneğin, aşağıdaki örnek, görünümü olan basit bir çözüm dosyasını gösterir.  Elipsler (...) boşluk nedenleriyle **Pano** ve **Genel BakışTile** içerikleri için gösterilir.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a>Sonraki adımlar
* [Yönetim çözümleri](solutions-creating.md)oluşturmanın tüm ayrıntılarını öğrenin.
* [Yönetim çözümünüze Otomasyon runbook'larını](solutions-resources-automation.md)ekleyin.
