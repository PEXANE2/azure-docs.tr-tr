---
title: Azure Resource Manager şablonu kullanarak Redsıs için Azure önbelleği dağıtma
description: Redsıs kaynağı için bir Azure önbelleği dağıtmak üzere Azure Resource Manager şablonu kullanmayı öğrenin. Şablonlar, yaygın senaryolar için sağlanır.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.custom: subject-armqs
ms.date: 08/18/2020
ms.openlocfilehash: 3c2d13794f2fc3af4541032d1f94967681c0deee
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89078427"
---
# <a name="create-an-azure-cache-for-redis-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Reda için Azure önbelleği oluşturma

Reda için Azure önbelleği dağıtan bir Azure Resource Manager şablonu oluşturmayı öğrenin. Önbellek, tanılama verilerini korumak için mevcut bir depolama hesabıyla birlikte kullanılabilir. Ayrıca, hangi kaynakların dağıtıldığını ve dağıtım yürütüldüğünde belirtilen parametrelerin nasıl tanımlanacağını nasıl tanımlayacağınızı öğreneceksiniz. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz. Şu anda, Tanılama ayarları bir abonelik için aynı bölgedeki tüm önbellekler için paylaşılır. Bölgedeki bir önbelleğin güncelleştirilmesi bölgedeki diğer tüm önbellekleri etkiler.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Bir depolama hesabı**: oluşturmak için bkz. [Azure depolama hesabı oluşturma](/storage/common/storage-account-create?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=template). Depolama hesabı, tanılama verileri için kullanılır.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-redis-cache/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-redis-cache/azuredeploy.json":::

Aşağıdaki kaynaklar şablonda tanımlanmıştır:

* [Microsoft. Cache/Redsıs](/azure/templates/microsoft.cache/redis)
* [Microsoft. Insights/diagnosticsettings](/azure/templates/diagnosticsettings)

Yeni [Premium katmanının](cache-overview.md#service-tiers) Kaynak Yöneticisi şablonları da kullanılabilir.

* [Kümeleme ile Reda için Premium Azure önbelleği oluşturma](https://azure.microsoft.com/resources/templates/201-redis-premium-cluster-diagnostics/)
* [Veri kalıcılığı olan Redsıs için Premium Azure önbelleği oluşturma](https://azure.microsoft.com/resources/templates/201-redis-premium-persistence/)
* [Sanal bir ağa dağıtılan Premium Redis Cache oluşturma](https://azure.microsoft.com/resources/templates/201-redis-premium-vnet/)

En son şablonları denetlemek için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/) ve arama `Azure Cache for Redis` .

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure 'da oturum açmak ve şablonu açmak için aşağıdaki görüntüyü seçin.

    [![Azure’a dağıt](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)
1. Aşağıdaki değerleri seçin veya girin:

    * **Abonelik**: veri paylaşımının ve diğer kaynakların oluşturulması için kullanılan bir Azure aboneliğini seçin.
    * **Kaynak grubu**: yeni bir kaynak grubu oluşturmak Için **Yeni oluştur** ' u seçin veya var olan bir kaynak grubunu seçin.
    * **Konum**: kaynak grubu için bir konum seçin. Depolama hesabı ve Redo önbelleğinin aynı bölgede olması gerekir. Varsayılan olarak Redsıs Cache, kaynak grubuyla aynı konumu kullanır. Bu nedenle, depolama hesabıyla aynı konumu belirtin.
    * **Redis Cache adı**: redsıs önbelleği için bir ad girin.
    * **Mevcut tanılama depolama hesabı**: bir depolama HESABıNıN kaynak kimliğini girin. Sözdizimi **/Subscriptions/ &lt; SUBSCRIPTION ID>/resourceGroups/ &lt; kaynak grubu adı>/PROVIDERS/MICROSOFT.Storage/STORAGEACCOUNTS/ &lt; depolama hesabı adı>**' dır.

    Geri kalan ayarlar için varsayılan değeri kullanın.
1. **yukarıda belirtilen hüküm ve koşulları**ve **satın alma**seçimini kabul ediyorum ' u seçin.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Oluşturduğunuz redo önbelleğini açın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse kaynak grubundaki kaynakları silen kaynak grubunu silin.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Reda için Azure önbelleği dağıtan bir Azure Resource Manager şablonu oluşturmayı öğrenirsiniz. Redin için Azure Cache ile bir Azure Web uygulaması dağıtan Azure Resource Manager şablonu oluşturmayı öğrenmek için bkz. bir [Web uygulaması oluşturma ve bir şablon kullanarak Reda Için Azure önbelleği](./cache-web-app-arm-with-redis-cache-provision.md).