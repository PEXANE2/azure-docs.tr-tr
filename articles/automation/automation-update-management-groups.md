---
title: Azure Otomasyonu ile dinamik grupları kullanın Güncelleştirme Yönetimi
description: Bu makalede, Azure Otomasyonu Güncelleştirme Yönetimi ile dinamik grupların nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 11/20/2019
ms.topic: conceptual
ms.openlocfilehash: c440862f1379983d4644fe99f33207456f747d23
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83830948"
---
# <a name="use-dynamic-groups-with-update-management"></a>Güncelleştirme Yönetimi ile dinamik grupları kullanma

Güncelleştirme Yönetimi, güncelleştirme dağıtımları için Azure veya Azure olmayan VM 'Lerin dinamik bir grubunu hedefetmenize olanak tanır. Dinamik bir grup kullanmak, makineleri güncelleştirmek için dağıtımınızı düzenleme zorunluluğunu ortadan önler.

> [!NOTE]
> Dinamik Gruplar klasik VM 'Ler ile çalışmaz.

Azure veya Azure dışı makineler için dinamik grupları, Azure portal **güncelleştirme yönetiminden** belirleyebilirsiniz. Bkz. [birden çok Azure sanal makinesi için güncelleştirmeleri yönetme](manage-update-multi.md).

Dinamik bir grup, Azure Otomasyonu 'nun dağıtım zamanında değerlendirdiği bir sorgu tarafından tanımlanır. Dinamik grup sorgusu çok sayıda makine alsa bile Azure Otomasyonu aynı anda yalnızca en fazla 1000 makineyi işleyebilir. Bkz. [Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#update-management). 

> [!NOTE]
> 1000 'den fazla makinenin güncelleştirilmesini bekleseniz, güncelleştirmeleri birden çok güncelleştirme zamanlaması arasında bölmeniz önerilir. 

## <a name="define-dynamic-groups-for-azure-machines"></a>Azure makineleri için dinamik grupları tanımlama

Azure makineleri için dinamik bir grup sorgusu tanımlarken, dinamik grubu doldurmak için aşağıdaki öğeleri kullanabilirsiniz:

* Abonelik
* Kaynak grupları
* Konumlar
* Etiketler

![Grupları seçin](./media/automation-update-management/select-groups.png)

Dinamik grup sorgunuzun sonuçlarını önizlemek için **Önizleme**' ye tıklayın. Önizleme, geçerli zamanda grup üyeliğini gösterir. Örnekte, `Role` **BackEndServer**grubu için etiketi olan makineler aranıyor. Bu etiket daha fazla makineye eklendiyse, bu Etiketler bu gruba karşı gelecekteki tüm dağıtımlara eklenir.

![Önizleme grupları](./media/automation-update-management/preview-groups.png)

## <a name="define-dynamic-groups-for-non-azure-machines"></a>Azure dışı makineler için dinamik grupları tanımlama

Azure dışı makineler için dinamik bir grup, bilgisayar grupları olarak da adlandırılan kaydedilmiş aramaları kullanır. Kayıtlı bir aramanın nasıl oluşturulduğunu öğrenmek için bkz. [bilgisayar grubu oluşturma](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Kayıtlı aramanız oluşturulduktan sonra, Azure portal **güncelleştirme yönetiminde** kayıtlı aramalar listesinden seçebilirsiniz. Kayıtlı aramada bilgisayarları önizlemek için **Önizleme** ' ye tıklayın.

![Grupları seçin](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirme Yönetimi çalışmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
