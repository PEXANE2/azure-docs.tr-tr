---
title: Azure Otomasyonu ile dinamik grupları kullanın Güncelleştirme Yönetimi
description: Bu makalede, dinamik grupların Azure Otomasyonu Güncelleştirme Yönetimi ile nasıl çalıştığı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 11/20/2019
ms.topic: conceptual
ms.openlocfilehash: 29a72eb1fe7b8be18cd2160fc63160e408378585
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81617432"
---
# <a name="use-dynamic-groups-with-update-management"></a>Güncelleştirme Yönetimi ile dinamik grupları kullanma

Güncelleştirme Yönetimi, güncelleştirme dağıtımları için Azure veya Azure olmayan VM 'Lerin dinamik bir grubunu hedefetmenize olanak tanır. Sorgular tarafından tanımlanan bu gruplar, dağıtım zamanında değerlendirilir ve bu sayede, makineleri eklemek için dağıtımınızı düzenlemeniz gerekmez.

## <a name="azure-machines"></a>Azure makineleri

Dinamik Gruplar klasik VM 'Ler ile çalışmaz. Sorgunuzu tanımlarken, bir dinamik grubu doldurmak için aşağıdaki öğeler birlikte kullanılabilir:

* Abonelik
* Kaynak grupları
* Konumlar
* Etiketler

![Grupları seçin](./media/automation-update-management/select-groups.png)

Dinamik bir grubun sonuçlarını önizlemek için **Önizleme**' ye tıklayın. Önizleme, geçerli zamanda grup üyeliğini gösterir. Örnekte, `Role` **BackEndServer**grubu için etiketi olan makineler aranıyor. Bu etiket daha fazla makineye eklendiyse, bu Etiketler bu gruba karşı gelecekteki tüm dağıtımlara eklenir.

![Önizleme grupları](./media/automation-update-management/preview-groups.png)

## <a name="non-azure-machines"></a>Azure dışı makineler

Azure dışı makinelerde, bilgisayar grupları olarak da bilinen kayıtlı aramalar, dinamik grubu oluşturmak için kullanılır. Kayıtlı bir aramanın nasıl oluşturulduğunu öğrenmek için bkz. [bilgisayar grubu oluşturma](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Grubunuz oluşturulduktan sonra, kaydedilen aramalar listesinden seçebilirsiniz. Kaydedilen aramada bilgisayarları o anda önizlemek için **Önizleme** ' ye tıklayın.

![Grupları seçin](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Dinamik bir grup oluşturduktan sonra [bir güncelleştirme dağıtımı oluşturabilirsiniz](automation-tutorial-update-management.md).
