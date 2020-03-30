---
title: Azure Monitör Çalışma Kitapları ve Azure Kaynak Yöneticisi Şablonları
description: Azure Kaynak Yöneticisi Şablonları aracılığıyla dağıtılan önceden oluşturulmuş ve özel parametrelendirilmiş Azure Monitör Çalışma Kitapları ile karmaşık raporlamayı basitleştirin
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 2c2d70d1c945e700a3fa42609f8aa0e1607ba77c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658413"
---
# <a name="programmatically-manage-workbooks"></a>Çalışma kitaplarını programlı bir şekilde yönetme

Kaynak sahipleri, Kaynak Yöneticisi şablonları aracılığıyla çalışma kitaplarını programlı bir şekilde oluşturma ve yönetme seçeneğine sahiptir. 

Bu, aşağıdaki gibi senaryolarda yararlı olabilir:
* Kaynak dağıtımlarıyla birlikte org veya etki alanına özel analiz raporlarının dağıtılması. Örneğin, yeni uygulamalarınız veya sanal makineleriniz için org'a özgü performans ve hata çalışma kitapları dağıtabilirsiniz.
* Varolan kaynaklar için çalışma kitaplarını kullanarak standart raporlar veya panolar dağıtma.

Çalışma kitabı istenilen alt/kaynak grubunda ve Kaynak Yöneticisi şablonlarında belirtilen içerikle oluşturulur.

## <a name="azure-resource-manager-template-for-deploying-workbooks"></a>Çalışma kitaplarını dağıtmak için Azure Kaynak Yöneticisi şablonu
1. Programlı olarak dağıtmak istediğiniz bir çalışma kitabı açın.
2. İş kitabını _düzenleme_ araç çubuğu öğesini tıklatarak düzenleme moduna geçin.
3. Araç çubuğundaki düğmeyi _</>_ kullanarak Gelişmiş _Düzenleyici'yi_ açın.
4. Düzenleyicide Şablon _Türü'nden_ _Kaynak Yöneticisi şablonuna_geçin.
5. Oluşturmak için Kaynak Yöneticisi şablonu düzenleyicide gösterir. İçeriği kopyalayın ve olduğu gibi kullanın veya hedef kaynağı da dağıtan daha büyük bir şablonla birleştirin.

    ![Çalışma kitabı içinden Kaynak Yöneticisi şablonu nasıl alınır gösteren resim](./media/workbooks-automate/programmatic-template.png)

## <a name="sample-azure-resource-manager-template"></a>Örnek Azure Kaynak Yöneticisi şablonu
Bu şablon, 'Merhaba Dünya' görüntüleyen basit bir çalışma kitabının nasıl dağıtılangerektiğini gösterir.
```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workbookDisplayName":  {             
            "type":"string",
            "defaultValue": "My Workbook",
            "metadata": {
                "description": "The friendly name for the workbook that is used in the Gallery or Saved List. Needs to be unique in the scope of the resource group and source" 
            }
        },
        "workbookType":  {             
            "type":"string",
            "defaultValue": "tsg",
            "metadata": {
                "description": "The gallery that the workbook will been shown under. Supported values include workbook, `tsg`, Azure Monitor, etc." 
            }
        },
        "workbookSourceId":  {             
            "type":"string",
            "defaultValue": "<insert-your-resource-id-here>",
            "metadata": {
                "description": "The id of resource instance to which the workbook will be associated" 
            }
        },
        "workbookId": {
            "type":"string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "The unique guid for this workbook instance" 
            }
        }
    },    
    "resources": [
        {
            "name": "[parameters('workbookId')]",
            "type": "Microsoft.Insights/workbooks",
            "location": "[resourceGroup().location]",
            "kind": "shared",
            "apiVersion": "2018-06-17-preview",
            "dependsOn": [],
            "properties": {
                "displayName": "[parameters('workbookDisplayName')]",
                "serializedData": "{\"version\":\"Notebook/1.0\",\"items\":[{\"type\":1,\"content\":\"{\\\"json\\\":\\\"Hello World!\\\"}\",\"conditionalVisibility\":null}],\"isLocked\":false}",
                "version": "1.0",
                "sourceId": "[parameters('workbookSourceId')]",
                "category": "[parameters('workbookType')]"
            }
        }
    ],
    "outputs": {
        "workbookId": {
            "type": "string",
            "value": "[resourceId( 'Microsoft.Insights/workbooks', parameters('workbookId'))]"
        }
    }
}
```

### <a name="template-parameters"></a>Şablon parametreleri

| Parametre | Açıklama |
| :------------- |:-------------|
| `workbookDisplayName` | Galeri'de veya Kaydedilmiş Liste'de kullanılan çalışma kitabının dostu adı. Kaynak grubu ve kaynak kapsamında benzersiz olması gerekir |
| `workbookType` | Çalışma kitabının altında gösterilecek galeri. Desteklenen değerler çalışma kitabı, `tsg`Azure Monitör, vb. içerir. |
| `workbookSourceId` | Çalışma kitabının ilişkilendirileceği kaynak örneğinin kimliği. Yeni çalışma kitabı bu kaynak örneğiile ilgili olarak gösterilecek - _örneğin, Kaynak'ın Çalışma Kitabı_altındaki içerik tablosunda. Çalışma kitabınızın Azure Monitor'daki çalışma kitabı galerisinde gösterilmesini istiyorsanız, kaynak kimliği yerine _Azure Monitor_ dizesini kullanın. |
| `workbookId` | Bu çalışma kitabı örneği için benzersiz kılavuz. Yeni bir kılavuz oluşturmak için _[newGuid()]_ kullanın. |
| `kind` | Oluşturulan çalışma kitabının paylaşıldığını veya özel olup olmadığını belirtmek için kullanılır. Paylaşılan çalışma kitapları için _paylaşılan_ değeri ve özel kitaplar için _kullanıcıyı_ kullanın. |
| `location` | Çalışma kitabının oluşturulacağı Azure konumu. Kaynak grubuyla aynı konumda oluşturmak için _[resourceGroup().location]_ kullanın |
| `serializedData` | Çalışma kitabında kullanılacak içeriği veya yükü içerir. Değeri almak için çalışma kitaplarındaki Kaynak Yöneticisi şablonuna yardımcı olun |

### <a name="workbook-types"></a>Çalışma kitabı türleri
Çalışma kitabı türleri, yeni çalışma kitabı örneğinin altında hangi çalışma kitabı galerisi nin görüneceğini belirtir. Seçeneklere şunlar dahildir:

| Tür | Galeri konumu |
| :------------- |:-------------|
| `workbook` | Uygulama Öngörüleri Çalışma Kitapları galerisi, Azure Monitörü vb. dahil olmak üzere çoğu raporda kullanılan varsayılan değerdir.  |
| `tsg` | Application Insights'taki Sorun Giderme Kılavuzları galerisi |
| `usage` | Uygulama Öngörülerinde _Kullanım_ Altında _Daha Fazla_ Galeri |

### <a name="limitations"></a>Sınırlamalar
Teknik nedenlerden dolayı, bu mekanizma Uygulama Öngörüleri _Çalışma Kitapları_ galerisinde çalışma kitabı örnekleri oluşturmak için kullanılamaz. Bu sınırlamayı ele almaya çalışıyoruz. Bu arada, Application Insights ile ilgili çalışma kitaplarını dağıtmak `tsg`için Sorun Giderme Kılavuzu galerisini (çalışma kitabıTürü: ) kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Yeni [Azure Depolama Monitörü deneyimine](../insights/storage-insights-overview.md)güç sağlamak için çalışma kitaplarının nasıl kullanıldığını keşfedin.

