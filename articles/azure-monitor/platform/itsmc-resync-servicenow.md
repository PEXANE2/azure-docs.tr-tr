---
title: ServiceNow eşitleme sorunları el ile nasıl düzeltilir?
description: ServiceNow bağlantısını sıfırla, böylece Microsoft Azure'daki uyarılar ServiceNow'u yeniden arayabilir
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 04/12/2020
ms.openlocfilehash: 9d32b66b7e3ab27012ee671ba8d70735dce73884
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81274282"
---
# <a name="how-to-manually-fix-servicenow-sync-problems"></a>ServiceNow eşitleme sorunları el ile nasıl düzeltilir?

Azure Monitor, üçüncü taraf BT Hizmet Yönetimi (ITSM) sağlayıcılarına bağlanabilir. ServiceNow bu sağlayıcılardan biridir. 

Güvenlik nedeniyle ServiceNow ile bağlantınız için kullanılan kimlik doğrulama belirtecinizi yenilemeniz gerekebilir.
Bağlantıyı yeniden etkinleştirmek ve belirteci yenilemek için aşağıdaki eşitleme işlemini kullanın: 


1. En üst arama başlığında çözümü arayın ve ilgili çözümleri seçin

    ![Yeni bağlantı](media/itsmc-resync-servicenow/solution-search-8bit.png)

1. Çözüm ekranında, abonelik filtresinde "Tümünü Seç"i seçin ve ardından "ServiceDesk" tarafından filtreuygulayın

    ![Yeni bağlantı](media/itsmc-resync-servicenow/solutions-list-8bit.png)

1. ITSM bağlantınızın çözümünü seçin.
1. Sol başlıktaki ITSM bağlantısını seçin.

    ![Yeni bağlantı](media/itsmc-resync-servicenow/itsm-connector-8bit.png)

1. Listeden her bağlayıcıyı seçin. 
    1. Yapılandırmak için Bağlayıcı adını tıklatın
    1. Artık kullanımda olmayan tüm bağlayıcıları silme

    1. Alanları iş ortağı türünüze göre [bu tanımlara](https://docs.microsoft.com/azure/azure-monitor/platform/itsmc-connections) göre güncelleştirme

    1. Eşitleme üzerine tıklayın

       ![Yeni bağlantı](media/itsmc-resync-servicenow/resync-8bit2.png)

    1. Kaydet'e tıklayın

        ![Yeni bağlantı](media/itsmc-resync-servicenow/save-8bit.png)

f.    İşlemin başarıyla bitip bitip bitmeden tamamlanıp bitip bitmeden görmek için bildirimleri gözden geçirin 

## <a name="next-steps"></a>Sonraki Adımlar

[BT Hizmet Yönetimi Bağlantıları](itsmc-connections.md) hakkında daha fazla bilgi edinin
