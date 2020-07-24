---
title: ServiceNow eşitleme sorunlarını el ile çözme
description: ServiceNow bağlantısını sıfırlayın Microsoft Azure içindeki uyarılar ServiceNow 'ı çağırabilir
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: a3382f93990612b0ab34eb0848cbf3d6577c44ff
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87087943"
---
# <a name="how-to-manually-fix-servicenow-sync-problems"></a>ServiceNow eşitleme sorunlarını el ile çözme

Azure Izleyici, üçüncü taraf BT hizmet yönetimi (ıSM) sağlayıcılarına bağlanabilir. ServiceNow, bu sağlayıcılardan biridir.

Güvenlik nedenleriyle, ServiceNow ile bağlantınız için kullanılan kimlik doğrulama belirtecini yenilemeniz gerekebilir.
Bağlantıyı yeniden etkinleştirmek ve belirteci yenilemek için aşağıdaki eşitleme sürecini kullanın:


1. Üst arama başlığında çözümü arayın ve ilgili çözümleri seçin

    ![Yeni bağlantı](media/itsmc-resync-servicenow/solution-search-8bit.png)

1. Çözüm ekranında, abonelik filtresindeki "Tümünü Seç" i seçin ve ardından "ServiceDesk" olarak filtreleyin

    ![Yeni bağlantı](media/itsmc-resync-servicenow/solutions-list-8bit.png)

1. ITSM bağlantınızın çözümünü seçin.
1. Sol başlıkta ıTSM bağlantısı ' nı seçin.

    ![Yeni bağlantı](media/itsmc-resync-servicenow/itsm-connector-8bit.png)

1. Listedeki her bağlayıcıyı seçin. 
    1. Yapılandırmak için bağlayıcı adına tıklayın
    1. Artık kullanımda olmayan bağlayıcıları Sil

    1. İş ortağı türüne göre [bu tanımlara](./itsmc-connections.md) göre alanları güncelleştirin

    1. Eşitle 'ye tıklayın

       ![Yeni bağlantı](media/itsmc-resync-servicenow/resync-8bit2.png)

    1. Kaydet 'e tıklayın

        ![Yeni bağlantı](media/itsmc-resync-servicenow/save-8bit.png)

f.    İşlemin başarılı ile bitmediğini görmek için bildirimleri gözden geçirin 

## <a name="next-steps"></a>Sonraki Adımlar

[BT hizmet yönetimi bağlantıları](itsmc-connections.md) hakkında daha fazla bilgi edinin
