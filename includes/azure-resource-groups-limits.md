---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/24/2020
ms.author: tomfitz
ms.openlocfilehash: c883383d3c870689bb95f808f6f60c5185c165c3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80334949"
---
| Kaynak | Sınır |
| --- | --- |
| [Kaynak grubu](../articles/azure-resource-manager/management/overview.md#resource-groups) başına kaynaklar | Kaynaklar kaynak grubuyla sınırlı değildir. Bunun yerine, bir kaynak grubundaki kaynak türüyle sınırlıdır. Sonraki satıra bakın. |
| Kaynak grubu başına kaynaklar, kaynak türü başına |800-bazı kaynak türleri 800 sınırını aşabilir. [Kaynak grubu başına 800 örnek için sınırlı kaynaklara](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md)bakın. |
| Dağıtım geçmişinde kaynak grubu başına dağıtımlar |800<sup>1</sup> |
| Dağıtım başına kaynaklar |800 |
| Benzersiz kapsam başına yönetim kilitleri |20 |
| Kaynak veya kaynak grubu başına etiket sayısı |50 |
| Etiket anahtarı uzunluğu |512 |
| Etiket değeri uzunluğu |256 |

<sup>1</sup> Kaynak grubu başına 800 dağıtım sınırına ulaşırsanız, artık gerekli olmayan geçmişten dağıtımları silin. Dağıtım geçmişinden bir girişin silinmesi dağıtılan kaynakları etkilemez. Daha fazla bilgi için bkz. [dağıtım sayısı 800 ' i aştığında hata çözümleme](../articles/azure-resource-manager/templates/deployment-quota-exceeded.md).

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
