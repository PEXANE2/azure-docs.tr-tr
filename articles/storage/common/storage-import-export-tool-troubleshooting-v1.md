---
title: Azure Içeri/dışarı aktarma aracı sorunlarını giderme | Microsoft Docs
description: Azure Içeri/dışarı aktarma Aracı kullanılırken görülen yaygın sorunlardan bazıları ve bunların nasıl işleneceği hakkında bilgi edinin.
author: twooley
services: storage
ms.service: storage
ms.topic: troubleshooting
ms.date: 01/15/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: bc9d338579385001d33669ed06ff71e590571502
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85514156"
---
# <a name="troubleshooting-the-azure-importexport-tool"></a>Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme
Microsoft Azure İçeri/Dışarı Aktarma araç, sorunlar halinde çalıştırılıyorsa hata iletileri döndürür. Bu konuda, kullanıcıların çalıştırabilinen bazı yaygın sorunları listelenmektedir.  

## <a name="a-copy-session-fails-what-i-should-do"></a>Bir kopyalama oturumu başarısız oluyor, ne yapmam gerekir?  
 Bir kopyalama oturumu başarısız olduğunda, iki seçenek vardır:  

 Hata yeniden denenebilir, örneğin, ağ paylaşımının kısa bir süre için çevrimdışı olması ve şimdi yeniden çevrimiçi olması halinde kopyalama oturumunu sürdürebilirsiniz. Hata yeniden denenmez ise, örneğin komut satırı parametrelerinde yanlış kaynak dosya dizini belirttiyseniz kopyalama oturumunu iptal etmeniz gerekir. Kopyalama oturumlarını sürdürme ve iptal etme hakkında daha fazla bilgi için bkz. [bir Içeri aktarma işi Için sabit sürücüleri hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md) .  

## <a name="i-cant-resume-or-abort-a-copy-session"></a>Bir kopyalama oturumunu sürdürülemiyor veya iptal edemiyorum.  
 Kopyalama oturumu bir sürücü için ilk kopyalama oturumundaysanız, hata iletisi şu şekilde olmalıdır: "ilk kopya oturumu devam ettirilemez veya iptal edilemez." Bu durumda, eski günlük dosyasını silip komutunu yeniden çalıştırabilirsiniz.  

 Bir kopya oturumu bir sürücü için ilk değilse, her zaman devam edebilir veya iptal edilebilir.  

## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a>Günlük dosyası kaybedildim, işi hala oluşturabilir miyim?  
 Bir sürücünün günlük dosyası, bu sürücüye veri kopyalama hakkında bilgilerin tamamını içerir ve sürücüye daha fazla dosya eklemek ve bir içeri aktarma işi oluşturmak için kullanılır. Günlük dosyası kaybolursa, sürücünün tüm kopyalama oturumlarını yeniden yapmanız gerekir.  

## <a name="next-steps"></a>Sonraki adımlar

* [Azure içeri/dışarı aktarma aracı 'nı ayarlama](../storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir içeri aktarma işini onarma](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)
