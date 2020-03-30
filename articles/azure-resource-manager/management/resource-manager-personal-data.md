---
title: Kişisel veriler
description: Azure Kaynak Yöneticisi işlemleriyle ilişkili kişisel verileri nasıl yönetebilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: 22cfc1b6096980f3d10db404a1c4e02f2de355d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485266"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Azure Kaynak Yöneticisi ile ilişkili kişisel verileri yönetme

Hassas bilgilerin açığa çıkarılmasını önlemek için, dağıtımlarda, kaynak gruplarında veya etiketlerde vermiş olabileceğiniz kişisel bilgileri silin. Azure Kaynak Yöneticisi, dağıtımlarda, kaynak gruplarında veya etiketlerde sağlamış olabileceğiniz kişisel verileri yönetmenize izin veren işlemler sağlar.

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Dağıtım geçmişindeki kişisel verileri silme

Dağıtımlar için Kaynak Yöneticisi, dağıtım geçmişinde parametre değerlerini ve durum iletilerini korur. Bu değerler, dağıtımı geçmişten silene kadar devam edin. Bu değerlerde kişisel veri sağlamış olup olmadığını görmek için dağıtımları listele. Kişisel verileri bulursanız, dağıtımları geçmişten silin.

Geçmişteki **dağıtımları** listelemek için şunları kullanın:

* [Kaynak Grubuna Göre Listele](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [az grup dağıtım listesi](/cli/azure/group/deployment#az-group-deployment-list)

**Dağıtımları** geçmişten silmek için şunları kullanın:

* [Sil](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [az grup dağıtım silme](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Kaynak grubu adlarında kişisel verileri silme

Kaynak grubunun adı, siz kaynak grubunu silene kadar devam eder. Adlarda kişisel veri sağlamış olup olmadığını görmek için kaynak gruplarını listele. Kişisel verileri bulursanız, kaynakları yeni bir kaynak grubuna [taşıyın](move-resource-group-and-subscription.md) ve addaki kişisel verilerle kaynak grubunu silin.

Kaynak **gruplarını**listelemek için şunları kullanın:

* [Liste](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [az grup listesi](/cli/azure/group#az-group-list)

Kaynak **gruplarını**silmek için şunları kullanın:

* [Sil](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Etiketlerdeki kişisel verileri silme

Etiketler adları ve değerleri, etiketi silene veya değiştirene kadar devam edin. Etiketlerde kişisel veri sağlamış olup olmadığını görmek için etiketleri listele. Kişisel verileribulursanız, etiketleri silin.

**Etiketleri**listelemek için, kullanın:

* [Liste](/rest/api/resources/tags/list)
* [Al-Aztag](/powershell/module/az.resources/Get-AzTag)
* [az etiket listesi](/cli/azure/tag#az-tag-list)

**Etiketleri**silmek için şunları kullanın:

* [Sil](/rest/api/resources/tags/delete)
* [Kaldır-Aztag](/powershell/module/az.resources/Remove-AzTag)
* [az etiketi silme](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Sonraki adımlar
* Azure Kaynak Yöneticisi'ne genel bakış için [Kaynak Yöneticisi nedir'e bakın.](overview.md)