---
title: Depolama Hesabı Konteyner Kurtarma
description: Depolama Hesabı Konteyner Kurtarma
services: storage
author: v-miegge
ms.topic: troubleshooting
ms.author: kartup
ms.date: 10/30/2019
ms.service: storage
manager: dcscontentpm
ms.openlocfilehash: 8b9b69fb32edab54fb2df558ea292df264e00cbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562284"
---
# <a name="storage-account-container-recovery"></a>Depolama Hesabı kapsayıcısını kurtarma

Azure Depolama, otomatik yinelemeler aracılığıyla veri esnekliği sağlar. Ancak bu, uygulama kodunun veya kullanıcıların yanlışlıkla veya kötü amaçlı olarak verileri bozmasını engellemez. Uygulama veya kullanıcı hatası karşısında veri doğruluğunun korunması, verileri denetim günlüğüyle ikincil bir depolama konumuna kopyalamak gibi daha gelişmiş teknikler gerektirir.

## <a name="checking-azure-storage-account-type"></a>Azure Depolama hesap türünü denetleme

1. [Azure portalına](https://portal.azure.com/) gidin.

2. Depolama hesabınızı bulun.

3. Genel **Bakış** bölümünde Çoğaltma **olup**yok.

   ![Görüntü](media/storage-account-container-recovery/1.png)

4. Çoğaltma Türü **GRS/RA-GRS**ise, hesap konteynerkurtarma garantisi zedilen mümkündür. Diğer tüm çoğaltma türleri için bu mümkün değildir.

5. Aşağıdaki bilgileri toplayın ve Microsoft Destek ile bir destek isteği dosyalayın.

   * Depolama Hesap Adı:
   * Konteyner Adı:
   * Silme Zamanı:

   Aşağıdaki tablo, çoğaltma stratejisine bağlı olarak Depolama Hesabı Kapsayıcı Kurtarma kapsamına genel bakış sağlar.

   |İçerik Türü|LRS|ZRS|GRS|RA - GRS| 
   |---|---|---|---|---|
   |Depolama Konteyneri|Hayır|Hayır|Evet|Evet| 

   * Biz depolama hesabı konteyner geri deneyebilirsiniz ama herhangi bir garanti olmadan. 

## <a name="things-not-to-do-for-recover-to-be-successful"></a>Başarılı olmak için kurtarma için yapılamaz şeyler

* Lütfen kapsayıcıyı (aynı adla) yeniden oluşturmayın.  
* Kapsayıcıyı zaten yeniden oluşturduysanız, kurtarma için bir destek isteği nde bulunmadan önce kapsayıcıyı silmeniz gerekir.

## <a name="steps-to-prevent-this-in-the-future"></a>Gelecekte bunu önlemek için adımlar

1. Gelecekte bunu önlemek için, kullanılacak en çok önerilen özellik [Yumuşak Silme'dir.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)

2. Ayrıca Anlık [Görüntü](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob) özelliğini de öneririz.
 
## <a name="next-steps"></a>Sonraki adımlar

Özellik üzerinde iki örnek kod ve şunlardır:

  * [.NET'te bir blob anlık görüntüsü oluşturma ve yönetme](https://docs.microsoft.com/azure/storage/storage-blob-snapshots)
  * [PowerShell ile blob anlık görüntülerini kullanma](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)
  

