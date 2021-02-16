---
title: ITSM bağlayıcısını ve onunla ilişkili eylemi silme
description: Bu makalede ıTSM bağlayıcısının ve onunla ilişkili eylem gruplarının nasıl silineceği hakkında bir açıklama sağlanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/29/2020
ms.custom: references_regions
ms.openlocfilehash: 1fbaa48d104642984e604bc66a3aa914fbfed44c
ms.sourcegitcommit: 7ec45b7325e36debadb960bae4cf33164176bc24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530976"
---
# <a name="deletion-of-unused-itsm-connectors"></a>Kullanılmayan ıTSM bağlayıcıları silme

Kullanılmayan bağlayıcının silinme süreci 2 aşama içerir:

1. İlişkili eylemlerin silinmesi: ITSM bağlayıcısıyla ilişkili tüm eylemler silinmelidir. Bu işlem, aboneliğinizde hatalara neden olabilecek bağlayıcı olmayan eylemler olmaması halinde yapılmalıdır.

2. Kullanılmayan ıTSM bağlayıcısını silme.

## <a name="deletion-of-the-associated-actions"></a>İlişkili eylemlerin silinmesi

1. Eylem grubunu bulmak için izleyici seçiminin "Monitor"  ![ ekran görüntüsüne gitmeniz gerekir.](media/itsmc-connector-deletion/itsmc-monitor-selection.png)

2. Uyarı seçiminin "Uyarılar"  ![ ekran görüntüsünü seçin.](media/itsmc-connector-deletion/itsmc-alert-selection.png)
3. Eylemleri yönetme seçiminin "eylemleri Yönet"  ![ ekran görüntüsünü seçin.](media/itsmc-connector-deletion/itsmc-actions-selection.png)
4. ![Cherwell 'e bağlı ITSM bağlayıcılarının Cherwell ekran görüntüsüne bağlı tüm ITSM bağlayıcılarını seçin.](media/itsmc-connector-deletion/itsmc-actions-screen.png)
5. ![Eylem grubu silme işleminin eylem grubu ekran görüntüsünü silin.](media/itsmc-connector-deletion/itsmc-action-deletion.png)

## <a name="deletion-of-the-unused-itsm-connector"></a>Kullanılmayan ıTSM bağlayıcısını silme

1. Search 'ün en üstteki arama çubuğu ekran görüntüsünde "ServiceDesk" LA araması yapmanız ve  ![ "ServiceDesk" La ' yı seçmeniz gerekir.](media/itsmc-connector-deletion/itsmc-connector-selection.png)
2. "ITSM bağlantıları" nı seçin ve  ![ CHERWELL ITSM bağlayıcılarının Cherwell bağlayıcı ekran görüntüsünü seçin.](media/itsmc-connector-deletion/itsmc-cherwell-connector.png)
3. ![ITSM Bağlayıcısı silmenin "Sil" ekran görüntüsünü seçin.](media/itsmc-connector-deletion/itsmc-connector-deletion.png)

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)
