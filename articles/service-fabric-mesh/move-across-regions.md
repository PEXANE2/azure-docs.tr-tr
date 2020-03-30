---
title: Service Fabric Mesh uygulamasını başka bir bölgeye taşıma
description: Geçerli şablonunuzun bir kopyasını yeni bir Azure bölgesine dağıtarak Service Fabric Mesh kaynaklarını taşıyabilirsiniz.
author: erikadoyle
ms.author: edoyle
ms.topic: how-to
ms.date: 01/14/2020
ms.custom: subject-moving-resources
ms.openlocfilehash: 376808a6d8f61d4dc03d17061323a473d48053a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76908169"
---
# <a name="move-a-service-fabric-mesh-application-to-another-azure-region"></a>Service Fabric Mesh uygulamasını başka bir Azure bölgesine taşıma

Bu makalede, Hizmet Kumaş Kafesi uygulamanızın ve kaynaklarının nasıl farklı bir Azure bölgesine taşınır. Kaynaklarınızı birkaç nedenden dolayı başka bir bölgeye taşıyabilirsiniz. Örneğin, kesintilere yanıt olarak, yalnızca belirli bölgelerde bulunan özellikler veya hizmetler kazanmak, iç politika ve yönetim gereksinimlerini karşılamak veya kapasite planlama gereksinimlerine yanıt olarak.

 [Service Fabric Mesh,](../azure-resource-manager/management/region-move-support.md#microsoftservicefabricmesh) kaynakları Azure bölgeleri boyunca doğrudan taşıma özelliğini desteklemez. Ancak, geçerli Azure Kaynak Yöneticisi şablonunuzun bir kopyasını yeni hedef bölgeye dağıtarak ve ardından giriş trafiğini ve bağımlılıklarını yeni oluşturulan Hizmet Kumaş Kafesi uygulamasına yönlendirerek kaynakları dolaylı olarak taşıyabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* İstemciler ve Servis Kumaş Kafesi uygulamanız arasındaki trafiği yönlendirmede aracı olarak hizmet vermek üzere giriş denetleyicisi [(Uygulama Ağ Geçidi](https://docs.microsoft.com/azure/application-gateway/)gibi)
* Hedef Azure bölgesinde Hizmet Kumaş Kafesi`westus`(Önizleme) kullanılabilirliği ( , `eastus`, veya `westeurope`)

## <a name="prepare"></a>Hazırlama

1. Azure Kaynak Yöneticisi şablonunu ve parametrelerini en son dağıtımdan dışa aktararak Hizmet Kumaş Kafesi uygulamanızın geçerli durumunun bir "anlık görüntüsünü" alın. Bunu yapmak için, Azure portalını kullanarak [dağıtımdan sonra Dışa Aktarma şablonundaki](../azure-resource-manager/templates/export-template-portal.md#export-template-after-deployment) adımları izleyin. [Azure CLI, Azure](../azure-resource-manager/management/manage-resource-groups-cli.md#export-resource-groups-to-templates) [PowerShell](../azure-resource-manager/management/manage-resource-groups-powershell.md#export-resource-groups-to-templates)veya [REST API'yi](https://docs.microsoft.com/rest/api/resources/resourcegroups/exporttemplate)de kullanabilirsiniz.

2. Varsa, hedef bölgede yeniden dağıtım için [aynı kaynak grubundaki diğer kaynakları dışa](https://docs.microsoft.com/azure/azure-resource-manager/templates/export-template-portal#export-template-from-a-resource-group) aktarın.

3. Varolan özellik değerlerinin hedef bölgede kullanmak istediğiniz kişiler olduğundan emin olmak için dışa aktarılan şablonu gözden geçirin (ve gerekirse edin. Yeni `location` (Azure bölgesi), yeniden dağıtım sırasında sağlayacağınız bir parametredir.

## <a name="move"></a>Taşı

1. Hedef bölgede yeni bir kaynak grubu oluşturun (veya varolan bir kaynak grubu kullanın).

2. Dışa aktarılan şablonunla, Azure portalını kullanarak [özel şablondan kaynakları dağıt'taki](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-portal#deploy-resources-from-custom-template) adımları izleyin. [Azure CLI, Azure](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-cli) [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell)veya [REST API'yi](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-rest)de kullanabilirsiniz.

3. [Azure Depolama hesapları](../storage/common/storage-account-move.md)gibi ilgili kaynakların taşınması yla ilgili kılavuz için, bölgeler arasında Azure kaynaklarını [taşıma](../azure-resource-manager/management/move-region.md)konusu altında listelenen tek tek hizmetler için kılavuza bakın.

## <a name="verify"></a>Doğrulama

1. Dağıtım tamamlandığında, uygulamanızın işlevselliğini doğrulamak için uygulama bitiş noktasını test edin.

2. Ayrıca uygulama durumunu[(az mesh app show)](https://docs.microsoft.com/cli/azure/ext/mesh/mesh/app?view=azure-cli-latest#ext-mesh-az-mesh-app-show)kontrol ederek ve uygulama günlüklerini ve ([az örgü kod-paket günlüğü)](https://docs.microsoft.com/cli/azure/ext/mesh/mesh/code-package-log?view=azure-cli-latest)komutlarını [Azure Hizmet Kumaş Kafesi CLI'yi](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-quickstart-deploy-container#set-up-service-fabric-mesh-cli)kullanarak kontrol ederek de başvurunuzun durumunu doğrulayabilirsiniz.

## <a name="commit"></a>İşleme

Hedef bölgedeki Hizmet Kumaş Kafesi uygulamanızın eşdeğer işlevselliğini onayladıktan sonra, trafiği yeni uygulamaya yönlendirmek için giriş denetleyicinizi [(örneğin, Uygulama Ağ Geçidi)](../application-gateway/redirect-overview.md)yapılandırın.

## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını temizleme

Service Fabric Mesh uygulamasının hareketini tamamlamak [için kaynak uygulamayı ve/veya üst kaynak grubunu silin.](../azure-resource-manager/management/delete-resource-group.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynaklarını bölgeler arasında taşıma](../azure-resource-manager/management/move-region.md)
* [Azure kaynaklarını bölgeler arasında taşıma desteği](../azure-resource-manager/management/region-move-support.md)
* [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md)
* [Kaynaklar için taşıma işlemi desteği](../azure-resource-manager/management/move-support-resources.md
)
