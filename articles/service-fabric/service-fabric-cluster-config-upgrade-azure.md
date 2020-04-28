---
title: Azure Service Fabric kümesinin yapılandırmasını yükseltme
description: Kaynak Yöneticisi şablonu kullanarak Azure 'da Service Fabric kümesi çalıştıran yapılandırmayı nasıl yükselteceğinizi öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 476a2d910b916ea29132b108478d06f756454813
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75463296"
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
2. **\<Aboneliğinizi, abonelik>**  ->  **ResourceGroups** ->  -> **kaynak grubunuz>sağlayıcıları Microsoft. servicefabric kümelerini küme adınızla genişleterek aboneliğinize\< ****Microsoft.ServiceFabric** -> **clusters** ->  **providers**gidin **subscriptions**  ->  -> **\<>**
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
* [Kümenizin ölçeğini ve ölçeğini nasıl ölçeklendireceğinizi](service-fabric-cluster-scale-up-down.md)öğrenin.
