---
title: Azure Otomasyon Güncelleme Yönetimi ile dinamik grupları kullanma
description: Bu makalede, dinamik grupların Azure Otomasyon Güncelleştirme Yönetimi ile nasıl çalıştığı açıklanmaktadır.
services: automation
ms.subservice: update-management
ms.date: 11/20/2019
ms.topic: conceptual
ms.openlocfilehash: 29a72eb1fe7b8be18cd2160fc63160e408378585
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617432"
---
# <a name="use-dynamic-groups-with-update-management"></a>Güncelleştirme Yönetimi ile dinamik grupları kullanma

Update Management, güncelleştirme dağıtımları için dinamik bir Azure veya Azure olmayan VM grubunu hedeflemenize olanak tanır. Sorgularla tanımlanan bu gruplar, makine eklemek için dağıtımınızı düzenlemeniz gerekmeden dağıtım zamanında değerlendirilir.

## <a name="azure-machines"></a>Azure makineleri

Dinamik gruplar klasik VM'lerle çalışmaz. Sorgunuzu tanımlarken, dinamik bir grubu doldurmak için aşağıdaki öğeler birlikte kullanılabilir:

* Abonelik
* Kaynak grupları
* Konumlar
* Etiketler

![Grupları seçin](./media/automation-update-management/select-groups.png)

Dinamik bir grubun sonuçlarını önizlemek için **Önizleme'yi**tıklatın. Önizleme, grup üyeliğini geçerli zamanda gösterir. Örnekte, **BackendServer**grubu için etikete `Role` sahip makineleri arıyoruz. Daha fazla makine bu etiket ekledivarsa, bu gruba karşı gelecekteki dağıtımlara eklenir.

![önizleme grupları](./media/automation-update-management/preview-groups.png)

## <a name="non-azure-machines"></a>Azure olmayan makineler

Dinamik grubu oluşturmak için Azure olmayan makinelerde bilgisayar grupları olarak da adlandırılan kaydedilmiş aramalar kullanılır. Kaydedilmiş bir aramayı nasıl oluşturabilirsiniz öğrenmek için [bkz.](../azure-monitor/platform/computer-groups.md#creating-a-computer-group) Grubunuzun oluşturulmasından sonra, kaydedilen aramalar listesinden seçebilirsiniz. O anda kaydedilen aramadaki bilgisayarları önizlemek için **Önizleme'yi** tıklatın.

![Grupları seçin](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Dinamik bir grup oluşturduktan [sonra, Bir Güncelleştirme Dağıtımı oluşturabilirsiniz.](automation-tutorial-update-management.md)
