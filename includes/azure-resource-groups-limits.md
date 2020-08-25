---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/24/2020
ms.author: tomfitz
ms.openlocfilehash: f0ab7c2efc499c43245680e56a7e5ca1b5261397
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88748806"
---
| Kaynak | Sınır |
| --- | --- |
| [Kaynak grubu](../articles/azure-resource-manager/management/overview.md#resource-groups) başına kaynaklar | Kaynaklar kaynak grubuyla sınırlı değildir. Bunun yerine, bir kaynak grubundaki kaynak türüyle sınırlıdır. Sonraki satıra bakın. |
| Kaynak grubu başına kaynaklar, kaynak türü başına |800-bazı kaynak türleri 800 sınırını aşabilir. [Kaynak grubu başına 800 örnek için sınırlı kaynaklara](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md)bakın. |
| Dağıtım geçmişinde kaynak grubu başına dağıtımlar |800<sup>1</sup> |
| Dağıtım başına kaynaklar |800 |
| Benzersiz [kapsam](../articles/azure-resource-manager/management/overview.md#understand-scope) başına yönetim kilitleri  |20 |
| Kaynak veya kaynak grubu başına etiket sayısı |50 |
| Etiket anahtarı uzunluğu |512 |
| Etiket değeri uzunluğu |256 |

<sup>1</sup> 2020 Haziran 'dan başlayarak, sınıra yaklaşmanıza kadar dağıtımlar otomatik olarak geçmişten silinir. Dağıtım geçmişinden bir girişin silinmesi dağıtılan kaynakları etkilemez. Daha fazla bilgi için bkz. [dağıtım geçmişinden otomatik silme işlemleri](../articles/azure-resource-manager/templates/deployment-history-deletions.md).

#### <a name="template-limits"></a>Şablon sınırları

| Değer | Sınır |
| --- | --- |
| Parametreler |256 |
| Değişkenler |256 |
| Kaynaklar (kopya sayısı dahil) |800 |
| Çıkışlar |64 |
| Şablon ifadesi |24.576 karakter |
| Aktarılmış şablonlarda kaynaklar |200 |
| Şablon boyutu |4 MB |
| Parametre dosyası boyutu |64 KB |

İç içe geçmiş bir şablon kullanarak bazı şablon sınırlarını aşabilirsiniz. Daha fazla bilgi için bkz. [Azure kaynaklarını dağıtırken bağlı şablonları kullanma](../articles/azure-resource-manager/templates/linked-templates.md). Parametre, değişken veya çıkış sayısını azaltmak için, birkaç değeri bir nesne içinde birleştirebilirsiniz. Daha fazla bilgi için bkz. [nesneler parametreler olarak](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
