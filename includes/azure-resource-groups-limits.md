---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/24/2020
ms.author: tomfitz
ms.openlocfilehash: c883383d3c870689bb95f808f6f60c5185c165c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334949"
---
| Kaynak | Sınır |
| --- | --- |
| Kaynak [grubu](../articles/azure-resource-manager/management/overview.md#resource-groups) başına kaynaklar | Kaynaklar kaynak grubuyla sınırlı değildir. Bunun yerine, kaynak grubundaki kaynak türüyle sınırlıdırlar. Sonraki satıra bak. |
| Kaynak grubu başına kaynaklar, kaynak türü başına |800 - Bazı kaynak türleri 800 sınırını aşabilir. Bkz. [Kaynak grubu başına 800 örnekle sınırlı olmayan Kaynaklar.](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md) |
| Dağıtım geçmişinde kaynak grubu başına dağıtımlar |800<sup>1</sup> |
| Dağıtım başına kaynaklar |800 |
| Benzersiz kapsam başına yönetim kilitleri |20 |
| Kaynak veya kaynak grubu başına etiket sayısı |50 |
| Etiket tuşu uzunluğu |512 |
| Etiket değeri uzunluğu |256 |

<sup>1.1.2</sup> Kaynak grubu başına 800 dağıtım sınırına ulaşırsanız, artık gerekmeden gelen dağıtımları geçmişten silin. Bir girişi dağıtım geçmişinden silerken, dağıtılan kaynakları etkilemez. Daha fazla bilgi için dağıtım [sayısı 800'ü aştığında hatayı](../articles/azure-resource-manager/templates/deployment-quota-exceeded.md)çözümle'ye bakın.

#### <a name="template-limits"></a>Şablon sınırları

| Değer | Sınır |
| --- | --- |
| Parametreler |256 |
| Değişkenler |256 |
| Kaynaklar (kopya sayısı dahil) |800 |
| Çıkışlar |64 |
| Şablon ifadesi |24.576 karakter |
| Dışa aktarılan şablonlarda kaynaklar |200 |
| Şablon boyutu |4 MB |
| Parametre dosya boyutu |64 KB |

İç içe bir şablon kullanarak bazı şablon sınırlarını aşabilirsiniz. Daha fazla bilgi için bkz. [Azure kaynaklarını dağıtırken bağlantılı şablonları kullan.](../articles/azure-resource-manager/templates/linked-templates.md) Parametrelerin, değişkenlerin veya çıktıların sayısını azaltmak için, bir nesneye birkaç değer birleştirebilirsiniz. Daha fazla bilgi [için, parametreler olarak Nesneler'e](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md)bakın.
