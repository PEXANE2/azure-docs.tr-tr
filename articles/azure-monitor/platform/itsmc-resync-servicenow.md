---
title: ServiceNow eşitleme sorunlarını el ile çözme
description: ServiceNow bağlantısını sıfırlayın Microsoft Azure içindeki uyarılar ServiceNow 'ı çağırabilir
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/17/2021
ms.openlocfilehash: aede7e3dec886d6a6213c64b386cacd725dd74f5
ms.sourcegitcommit: 61d2b2211f3cc18f1be203c1bc12068fc678b584
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2021
ms.locfileid: "98562803"
---
# <a name="how-to-manually-fix-sync-problems"></a>Eşitleme sorunlarını el ile çözme

Azure Izleyici, üçüncü taraf BT hizmet yönetimi (ıSM) sağlayıcılarına bağlanabilir. ServiceNow, bu sağlayıcılardan biridir.

Güvenlik nedenleriyle, ServiceNow ile bağlantınız için kullanılan kimlik doğrulama belirtecini yenilemeniz gerekebilir.
Bağlantıyı yeniden etkinleştirmek ve belirteci yenilemek için aşağıdaki eşitleme sürecini kullanın:

1. Üst arama başlığında çözümü arayın ve ilgili çözümleri seçin

    ![En üstteki arama başlığını ve ilgili çözümlerin nerede seçdiğinin gösterildiği ekran görüntüsü.](media/itsmc-resync-servicenow/solution-search-8bit.png)

1. Çözüm ekranında, abonelik filtresindeki "Tümünü Seç" i seçin ve ardından "ServiceDesk" olarak filtreleyin

    ![Tümünü Seç ' in nerede ve ServiceDesk tarafından filtreleneceğini gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/solutions-list-8bit.png)

1. ITSM bağlantınızın çözümünü seçin.
1. Sol başlıkta ıTSM bağlantısı ' nı seçin.

    ![ITSM bağlantılarının nerede seçdiğinin gösterildiği ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-connector-8bit.png)

1. Listedeki her bağlayıcıyı seçin. 
    1. Yapılandırmak için bağlayıcı adına tıklayın
    1. Artık kullanımda olmayan bağlayıcıları Sil

    1. İş ortağı türüne göre [bu tanımlara](./itsmc-connections.md) göre alanları güncelleştirin

    1. Eşitle 'ye tıklayın

       ![Eşitle düğmesini vurgulayan ekran görüntüsü.](media/itsmc-resync-servicenow/resync-8bit2.png)

    1. Kaydet 'e tıklayın

        ![Yeni bağlantı](media/itsmc-resync-servicenow/save-8bit.png)

f.    İşlemin başlayıp başlamadığına bakmak için bildirimleri gözden geçirin.
