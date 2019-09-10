---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: 25928ef35da1ce4b3824303a5d46749c32aa701f
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "69626415"
---
| Resource | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Kaynak [grubu](../articles/azure-resource-manager/resource-group-overview.md#resource-groups)başına kaynaklar, kaynak türü başına |800 |Bazı kaynak türleri 800 sınırını aşabilir. [Kaynak grubu başına 800 örnek için sınırlı kaynaklara](../articles/azure-resource-manager/resources-without-rg-limit.md)bakın. |
| Dağıtım geçmişinde kaynak grubu başına dağıtımlar |800<sup>1</sup> |800 |
| Dağıtım başına kaynaklar |800 |800 |
| Benzersiz kapsam başına yönetim kilitleri |20 |20 |
| Kaynak veya kaynak grubu başına etiket sayısı |50 |50 |
| Etiket anahtarı uzunluğu |512 |512 |
| Etiket değeri uzunluğu |256 |256 |

<sup>1</sup> Kaynak grubu başına 800 dağıtım sınırına ulaşırsanız, artık gerekli olmayan geçmişten dağıtımları silin. Dağıtım geçmişinden bir girişin silinmesi dağıtılan kaynakları etkilemez. Geçmişten girdileri, [az önce grup dağıtımı Delete](/cli/azure/group/deployment) for Azure CLI veya PowerShell 'de [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) ile silebilirsiniz.  Sürekli tümleştirme ve sürekli teslim (CI/CD) senaryosunda dağıtımları silmeyi otomatikleştiren bir PowerShell betiği için, bkz [. Remove-Deployments. ps1](https://gist.github.com/bmoore-msft/ed33fb940dafb09380174b7fca57651f).

#### <a name="template-limits"></a>Şablon sınırları

| Value | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Parametreler |256 |256 |
| Değişkenler |256 |256 |
| Kaynaklar (kopya sayısı dahil) |800 |800 |
| outputs |64 |64 |
| Şablon ifadesi |24.576 karakter |24.576 karakter |
| Aktarılmış şablonlarda kaynaklar |200 |200 | 
| Şablon boyutu |4 MB |4 MB |
| Parametre dosyası boyutu |64 KB |64 KB |

İç içe geçmiş bir şablon kullanarak bazı şablon sınırlarını aşabilirsiniz. Daha fazla bilgi için bkz. [Azure kaynaklarını dağıtırken bağlı şablonları kullanma](../articles/azure-resource-manager/resource-group-linked-templates.md). Parametre, değişken veya çıkış sayısını azaltmak için, birkaç değeri bir nesne içinde birleştirebilirsiniz. Daha fazla bilgi için bkz. [nesneler parametreler olarak](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
