---
title: Azure İçe Aktarma/Verme Aracı || Microsoft Dokümanlar
description: Azure İçe Aktarma/Verme Aracı'nı kullanırken görülen yaygın sorunlardan bazıları ve bunların nasıl işleyeceğiniz hakkında bilgi edinin.
author: twooley
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 4eeeb538bcd39eed40a92dd45e7ba7bed25558e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978411"
---
# <a name="troubleshooting-the-azure-importexport-tool"></a>Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme
Microsoft Azure İçe Alma/Dışa Aktarma Aracı, sorunlarla karşılanırsa hata iletileri döndürür. Bu konu, kullanıcıların karşılaşabileceği bazı sık karşılaşılan sorunları listeler.  

## <a name="a-copy-session-fails-what-i-should-do"></a>Bir kopyalama oturumu başarısız olursa, ne yapmalıyım?  
 Bir kopyalama oturumu başarısız olduğunda, iki seçenek vardır:  

 Hata yeniden denene ise, örneğin ağ paylaşımı kısa bir süre için çevrimdışıysa ve şimdi yeniden çevrimiçiyse, kopyalama oturumuna devam edebilirsiniz. Hata yeniden denene değilse, örneğin komut satırı parametrelerinde yanlış kaynak dosya dizinini belirttiyseniz, kopyalama oturumunu iptal etmeniz gerekir. Kopyalama oturumlarını devamlandırma ve iptal etme hakkında daha fazla bilgi için [Sabit Diskleri Alma İşi için Hazırlama'ya](../storage-import-export-tool-preparing-hard-drives-import-v1.md) bakın.  

## <a name="i-cant-resume-or-abort-a-copy-session"></a>Bir kopya oturumuna devam edemiyorum ya da iptal edemiyorum.  
 Kopyalama oturumu bir sürücünün ilk kopyalama oturumuysa, hata iletisi şöyle olmalıdır: "İlk kopyalama oturumu başlatılamaz veya iptal edilemez." Bu durumda, eski günlük dosyasını silebilir ve komutu yeniden çalıştırabilirsiniz.  

 Bir kopyalama oturumu bir sürücü için ilk değilse, her zaman devam ettirilebilir veya iptal edilebilir.  

## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a>Günlük dosyasını kaybettim, yine de işi oluşturabilir miyim?  
 Sürücünün günlük dosyası, bu sürücüye veri kopyalama nın tüm bilgilerini içerir ve sürücüye daha fazla dosya eklemek gerekir ve bir alma işi oluşturmak için kullanılır. Günlük dosyası kaybolursa, sürücünün tüm kopyalama oturumlarını yeniden yapmanız gerekir.  

## <a name="next-steps"></a>Sonraki adımlar

* [Azure alma/dışa aktarma aracını ayarlama](../storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir içeri aktarma işini onarma](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)
