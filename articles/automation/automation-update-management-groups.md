---
title: Azure Güncelleştirme Yönetimi ile dinamik grupları kullanma
description: Bu makalede, dinamik grupların Azure Otomasyon Güncelleştirme Yönetimi ile nasıl çalıştığı açıklanmaktadır.
services: automation
ms.subservice: update-management
ms.date: 11/20/2019
ms.topic: conceptual
ms.openlocfilehash: 678b3f361e4456a2c482896f7d7dc20d530b917b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75420392"
---
# <a name="use-dynamic-groups-with-update-management"></a>Güncelleştirme Yönetimi ile dinamik grupları kullanma

Update Management, güncelleştirme dağıtımları için dinamik bir Azure veya Azure olmayan VM grubunu hedefleme olanağı sağlar. Bu gruplar dağıtım zamanında değerlendirilir, böylece makine eklemek için dağıtımınızı yapmanız gerekmez.

## <a name="azure-machines"></a>Azure makineleri

Bu gruplar bir sorgu tarafından tanımlanır, bir güncelleştirme dağıtım başladığında, bu grubun üyeleri değerlendirilir. Dinamik gruplar klasik VM'lerle çalışmaz. Sorgunuzu tanımlarken, dinamik grubu doldurmak için aşağıdaki öğeler birlikte kullanılabilir:

* Abonelik
* Kaynak grupları
* Konumlar
* Etiketler

![Grupları seçin](./media/automation-update-management/select-groups.png)

Dinamik bir grubun sonuçlarını önizlemek için **Önizleme** düğmesini tıklatın. Bu önizleme o zaman grup üyeliğini gösterir, bu örnekte, biz **etiketI Ile** makineleri arıyoruz **BackendServer**eşittir. Daha fazla makine bu etiketi eklediyse, bu grup karşı gelecekteki dağıtımlara eklenir.

![önizleme grupları](./media/automation-update-management/preview-groups.png)

## <a name="non-azure-machines"></a>Azure olmayan makineler

Azure olmayan makinelerde, bilgisayar grupları olarak da adlandırılan kaydedilmiş aramalar dinamik grubu oluşturmak için kullanılır. Kaydedilmiş bir aramayı nasıl oluşturabilirsiniz öğrenmek için [bkz.](../azure-monitor/platform/computer-groups.md#creating-a-computer-group) Grubunuzun oluşturulduktan sonra kaydedilen aramalar listesinden seçebilirsiniz. O anda kaydedilen aramadaki bilgisayarları önizlemek için **Önizleme'yi** tıklatın.

![Grupları seçin](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Dinamik bir grup oluşturduktan sonra, [Güncelleştirme Dağıtımı Oluşturabilirsiniz](automation-tutorial-update-management.md)
