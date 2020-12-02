---
title: ServiceNow eşitleme sorunlarını el ile çözme
description: ServiceNow bağlantısını sıfırlayın Microsoft Azure içindeki uyarılar ServiceNow 'ı çağırabilir
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: f7d69c4e112ded678c70a516202492d37ee8f60a
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96436870"
---
# <a name="troubleshooting-problems-in-itsm-connector"></a>ITSM Bağlayıcısı sorunlarını giderme

## <a name="how-to-manually-fix-servicenow-sync-problems"></a>ServiceNow eşitleme sorunlarını el ile çözme

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

f.    İşlemin başarılı ile bitmediğini görmek için bildirimleri gözden geçirin 

## <a name="next-steps"></a>Sonraki Adımlar

[BT hizmet yönetimi bağlantıları](itsmc-connections.md) hakkında daha fazla bilgi edinin
