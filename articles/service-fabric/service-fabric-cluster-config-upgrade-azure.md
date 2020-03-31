---
title: Azure Hizmet Kumaşı kümesinin yapılandırmasını yükseltme
description: Kaynak Yöneticisi şablonu kullanarak Azure'da Hizmet Dokusu kümesini çalıştıran yapılandırmayı nasıl yükselteceklerini öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 476a2d910b916ea29132b108478d06f756454813
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463296"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Azure'da bir kümenin yapılandırmasını yükseltme 

Bu makalede, Hizmet Kumaş kümeniz için çeşitli kumaş ayarlarının nasıl özelleştirilen açıklanmaktadır. Azure'da barındırılan kümeler için, ayarları [Azure portalı](https://portal.azure.com) üzerinden veya Azure Kaynak Yöneticisi şablonu kullanarak özelleştirebilirsiniz.

> [!NOTE]
> Tüm ayarlar portalda bulunmaz ve [azure kaynak yöneticisi şablonu kullanarak özelleştirmek en iyi yöntemdir;](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code) Portal Hizmet Kumaş Dev\Test senaryosu için sadece.
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarını kullanarak küme ayarlarını özelleştirme
Azure kümeleri JSON Kaynak Yöneticisi şablonu aracılığıyla yapılandırılabilir. Farklı ayarlar hakkında daha fazla bilgi edinmek [için kümeler için Yapılandırma ayarlarına](service-fabric-cluster-fabric-settings.md)bakın. Örnek olarak, aşağıdaki adımlar, Azure Kaynak Gezgini'ni kullanarak *Tanılama* bölümüne yeni bir ayar *MaxDiskQuotaInMB* nasıl ekleyeceğinizgösterilmektedir.

1. Şuraya gidin: https://resources.azure.com
2. **Abonelikleri** ->   ->  **providers** ->  ->  ->  -> **\<genişleterek** aboneliğinizi genişletin  -> >**kaynakGruplarıKaynak****\<Grubunuz>** sağlayıcıları**Microsoft.ServiceFabric**Küme Adınızı**kümeler**>**\< **
3. Sağ üst köşede **Oku/Yaz'ı seçin.**
4. JSON öğesini `fabricSettings` **güncelleştir'i** seçin ve güncelleştirin ve yeni bir öğe ekleyin:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Azure Kaynak Yöneticisi ile küme ayarlarını aşağıdaki yollardan birinde de özelleştirebilirsiniz:

- Kaynak Yöneticisi şablonuna dışa aktarmak ve güncelleştirmek için [Azure portalını](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template) kullanın.
- Kaynak Yöneticisi şablonunu dışa aktarmak ve güncelleştirmek için [PowerShell'i](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell) kullanın.
- Kaynak Yöneticisi şablonuna dışa aktarmak ve güncelleştirmek için [Azure CLI'yi](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli) kullanın.
- Ayarı doğrudan değiştirmek için Azure PowerShell [Set-AzServiceFabricSetting](https://docs.microsoft.com/powershell/module/az.servicefabric/Set-azServiceFabricSetting) ve [Remove-AzServiceFabricSetting](https://docs.microsoft.com/powershell/module/az.servicefabric/Remove-azServiceFabricSetting) komutlarını kullanın.
- Ayarı doğrudan değiştirmek için Azure CLI [az sf küme ayarı](https://docs.microsoft.com/cli/azure/sf/cluster/setting) komutlarını kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric küme ayarları](service-fabric-cluster-fabric-settings.md)hakkında bilgi edinin.
* [Kümenizi nasıl ölçeklendirecek ve dışarı çıkar' ı](service-fabric-cluster-scale-up-down.md)öğrenin.
