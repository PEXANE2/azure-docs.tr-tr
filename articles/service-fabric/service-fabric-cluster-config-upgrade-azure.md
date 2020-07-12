---
title: Azure Service Fabric kümesinin yapılandırmasını yükseltme
description: Kaynak Yöneticisi şablonu kullanarak Azure 'da Service Fabric kümesi çalıştıran yapılandırmayı nasıl yükselteceğinizi öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 010f2b5d6dee3a5985c421e468bcd28b18148e5a
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86247736"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Azure 'da bir kümenin yapılandırmasını yükseltme 

Bu makalede Service Fabric kümeniz için çeşitli doku ayarlarının nasıl özelleştirileceği açıklanır. Azure 'da barındırılan kümeler için [Azure Portal](https://portal.azure.com) veya Azure Resource Manager şablonu kullanarak ayarları özelleştirebilirsiniz.

> [!NOTE]
> Portalda tüm ayarlar kullanılamaz ve [bir Azure Resource Manager şablonu kullanarak özelleştirmek en iyi uygulamadır](./service-fabric-best-practices-infrastructure-as-code.md); Portal yalnızca Service Fabric Dev\Test senaryosu için geçerlidir.
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

- Kaynak Yöneticisi şablonunu dışarı aktarmak ve güncelleştirmek için [Azure Portal](../azure-resource-manager/templates/export-template-portal.md) kullanın.
- Kaynak Yöneticisi şablonunu dışarı aktarmak ve güncelleştirmek için [PowerShell](../azure-resource-manager/management/manage-resources-powershell.md) 'i kullanın.
- Kaynak Yöneticisi şablonunu dışarı aktarmak ve güncelleştirmek için [Azure CLI](../azure-resource-manager/management/manage-resources-cli.md) 'yi kullanın.
- Ayarı doğrudan değiştirmek için Azure PowerShell [set-azservicefabricsetting](/powershell/module/az.servicefabric/set-azservicefabricsetting) ve [Remove-azservicefabricsetting](/powershell/module/az.servicefabric/remove-azservicefabricsetting) komutlarını kullanın.
- Ayarı doğrudan değiştirmek için Azure CLı [az SF Cluster Setting](/cli/azure/sf/cluster/setting) komutlarını kullanın.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric kümesi ayarları](service-fabric-cluster-fabric-settings.md)hakkında bilgi edinin.
* [Kümenizin ölçeğini ve ölçeğini nasıl ölçeklendireceğinizi](service-fabric-cluster-scale-in-out.md)öğrenin.
