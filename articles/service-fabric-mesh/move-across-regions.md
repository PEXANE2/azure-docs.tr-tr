---
title: Service Fabric kafes uygulamasını başka bir bölgeye taşıma
description: Geçerli şablonunuzun bir kopyasını yeni bir Azure bölgesine dağıtarak, Service Fabric kafes kaynaklarını taşıyabilirsiniz.
author: erikadoyle
ms.author: edoyle
ms.topic: how-to
ms.date: 01/14/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 376808a6d8f61d4dc03d17061323a473d48053a6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76908169"
---
# <a name="move-a-service-fabric-mesh-application-to-another-azure-region"></a>Service Fabric bir kafes uygulamasını başka bir Azure bölgesine taşıma

Bu makalede, Service Fabric kafes uygulamanızın ve kaynaklarının farklı bir Azure bölgesine nasıl taşınacağı açıklanır. Birkaç nedenden dolayı kaynaklarınızı başka bir bölgeye taşıyabilirsiniz. Örneğin kesintilere yanıt olarak, yalnızca belirli bölgelerde bulunan özellik veya Hizmetleri, iç ilke ve idare gereksinimlerini karşılayacak şekilde veya kapasite planlama gereksinimlerine yanıt olarak elde etmek için.

 [Service Fabric kafesi](../azure-resource-manager/management/region-move-support.md#microsoftservicefabricmesh) , kaynakları Azure bölgelerinde doğrudan taşıma özelliğini desteklemez. Ancak, geçerli Azure Resource Manager şablonunuzun bir kopyasını yeni hedef bölgeye dağıtarak ve sonra giriş trafiğini ve bağımlılıklarını yeni oluşturulan Service Fabric kafes uygulamasına yönlendirerek kaynakları dolaylı olarak taşıyabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* İstemciler ile Service Fabric kafes uygulamanız arasında trafiği yönlendirme için bir aracı olarak kullanılacak giriş denetleyicisi (örneğin [Application Gateway](https://docs.microsoft.com/azure/application-gateway/))
* Hedef Azure bölgesinde (`westus`, `eastus`veya `westeurope`) Service Fabric ağ (Önizleme) kullanılabilirliği

## <a name="prepare"></a>Hazırlama

1. En son dağıtımdan Azure Resource Manager şablonunu ve parametrelerini dışarı aktararak Service Fabric kafes uygulamanızın geçerli durumunun "anlık görüntüsünü" alın. Bunu yapmak için Azure portal kullanarak [dağıtımdan sonra şablonu dışarı aktarma](../azure-resource-manager/templates/export-template-portal.md#export-template-after-deployment) ' daki adımları izleyin. [Azure CLI](../azure-resource-manager/management/manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](../azure-resource-manager/management/manage-resource-groups-powershell.md#export-resource-groups-to-templates)veya [REST API](https://docs.microsoft.com/rest/api/resources/resourcegroups/exporttemplate)de kullanabilirsiniz.

2. Uygulanabiliyorsa, hedef bölgede yeniden dağıtım için [aynı kaynak grubundaki diğer kaynakları dışarı aktarın](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal#export-template-from-a-resource-group) .

3. Mevcut özellik değerlerinin hedef bölgede kullanmak istediklerinizle aynı olduğundan emin olmak için, bu şablonu gözden geçirin (ve gerekirse düzenleyin). Yeni `location` (Azure bölgesi) yeniden dağıtım sırasında sağlayabilmeniz için bir parametredir.

## <a name="move"></a>Taşı

1. Hedef bölgede yeni bir kaynak grubu oluşturun (veya mevcut bir tane kullanın).

2. Verdiğiniz şablonla, Azure portal kullanarak [özel şablondan kaynak dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-portal#deploy-resources-from-custom-template) ' daki adımları izleyin. [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-cli), [Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell)veya [REST API](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-rest)de kullanabilirsiniz.

3. [Azure depolama hesapları](../storage/common/storage-account-move.md)gibi ilgili kaynakları taşımaya yönelik yönergeler Için, [Azure kaynaklarını bölgeler arasında taşıma](../azure-resource-manager/management/move-region.md)konusunun altında yer alan hizmetlere yönelik kılavuza bakın.

## <a name="verify"></a>Doğrulama

1. Dağıtım tamamlandığında uygulamanızın işlevlerini doğrulamak için uygulama uç noktaları ' nı test edin.

2. Ayrıca, uygulama durumunu denetleyerek (az önce uygulama[göster](https://docs.microsoft.com/cli/azure/ext/mesh/mesh/app?view=azure-cli-latest#ext-mesh-az-mesh-app-show)) ve [Azure Service Fabric kafes CLI](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-quickstart-deploy-container#set-up-service-fabric-mesh-cli)kullanarak uygulama günlüklerini ve ([az kafes kodu-paket-günlüğü](https://docs.microsoft.com/cli/azure/ext/mesh/mesh/code-package-log?view=azure-cli-latest)) komutlarını inceleyerek uygulamanızın durumunu doğrulayabilirsiniz.

## <a name="commit"></a>İşleme

Hedef bölgede Service Fabric kafes uygulamanızın eşdeğer işlevlerini onayladıktan sonra, trafiği yeni uygulamaya yeniden yönlendirmek için giriş denetleyicinizi (örneğin, [Application Gateway](../application-gateway/redirect-overview.md)) yapılandırın.

## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını Temizleme

Service Fabric kafes uygulamasının taşınmasını tamamlamaya yönelik [kaynak uygulamayı ve/veya üst kaynak grubunu silin](../azure-resource-manager/management/delete-resource-group.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynaklarını bölgeler arasında taşıma](../azure-resource-manager/management/move-region.md)
* [Bölgeler arasında Azure kaynaklarını taşıma desteği](../azure-resource-manager/management/region-move-support.md)
* [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Kaynaklar için taşıma işlemi desteği](../azure-resource-manager/management/move-support-resources.md
)
