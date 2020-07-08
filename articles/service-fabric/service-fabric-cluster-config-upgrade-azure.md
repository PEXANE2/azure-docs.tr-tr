---
title: Azure Service Fabric kümesinin yapılandırmasını yükseltme
description: Kaynak Yöneticisi şablonu kullanarak Azure 'da Service Fabric kümesi çalıştıran yapılandırmayı nasıl yükselteceğinizi öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: c2973428354f101b5b546128b08bf67587923a8e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82793083"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Azure 'da bir kümenin yapılandırmasını yükseltme 

Bu makalede Service Fabric kümeniz için çeşitli doku ayarlarının nasıl özelleştirileceği açıklanır. Azure 'da barındırılan kümeler için [Azure Portal](https://portal.azure.com) veya Azure Resource Manager şablonu kullanarak ayarları özelleştirebilirsiniz.

> [!NOTE]
> Portalda tüm ayarlar kullanılamaz ve [bir Azure Resource Manager şablonu kullanarak özelleştirmek en iyi uygulamadır](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code); Portal yalnızca Service Fabric Dev\Test senaryosu için geçerlidir.
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarını kullanarak küme ayarlarını özelleştirme
Azure kümeleri, JSON Kaynak Yöneticisi şablonu aracılığıyla yapılandırılabilir. Farklı ayarlar hakkında daha fazla bilgi edinmek için bkz. [kümeler Için yapılandırma ayarları](service-fabric-cluster-fabric-settings.md). Örnek olarak, aşağıdaki adımlarda Azure Kaynak Gezgini kullanarak *Tanılama* bölümüne *Maxdiskquocontainer MB adlı* yeni bir ayarın nasıl ekleneceği gösterilmektedir.

1. Şuraya gidin: https://resources.azure.com
2. **Abonelikler**  ->  **\<Your Subscription>**  ->  **ResourceGroups**  ->  **\<Your Resource Group>**  ->  **sağlayıcıları**  ->  **Microsoft. servicefabric**  ->  **kümeleri** ' ni genişleterek aboneliğinize gidin -> **\<Your Cluster Name>**
3. Sağ üst köşede **oku/yaz** ' ı seçin.
4. **Düzenle** ve `fabricSettings` JSON öğesini Güncelleştir ' i seçin ve yeni bir öğe ekleyin:

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

Ayrıca, Azure Resource Manager aşağıdaki yollarla küme ayarlarını özelleştirebilirsiniz:

- Kaynak Yöneticisi şablonunu dışarı aktarmak ve güncelleştirmek için [Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template) kullanın.
- Kaynak Yöneticisi şablonunu dışarı aktarmak ve güncelleştirmek için [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell) 'i kullanın.
- Kaynak Yöneticisi şablonunu dışarı aktarmak ve güncelleştirmek için [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli) 'yi kullanın.
- Ayarı doğrudan değiştirmek için Azure PowerShell [set-azservicefabricsetting](https://docs.microsoft.com/powershell/module/az.servicefabric/Set-azServiceFabricSetting) ve [Remove-azservicefabricsetting](https://docs.microsoft.com/powershell/module/az.servicefabric/Remove-azServiceFabricSetting) komutlarını kullanın.
- Ayarı doğrudan değiştirmek için Azure CLı [az SF Cluster Setting](https://docs.microsoft.com/cli/azure/sf/cluster/setting) komutlarını kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric kümesi ayarları](service-fabric-cluster-fabric-settings.md)hakkında bilgi edinin.
* [Kümenizin ölçeğini ve ölçeğini nasıl ölçeklendireceğinizi](service-fabric-cluster-scale-in-out.md)öğrenin.
