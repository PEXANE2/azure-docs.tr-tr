---
title: Depolama hesabı kapsayıcısı kurtarma
description: Depolama hesabı kapsayıcısı kurtarma
services: storage
author: v-miegge
ms.topic: troubleshooting
ms.author: kartup
ms.date: 10/30/2019
ms.service: storage
manager: dcscontentpm
ms.openlocfilehash: 8b9b69fb32edab54fb2df558ea292df264e00cbb
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562284"
---
# <a name="storage-account-container-recovery"></a>Depolama hesabı kapsayıcısı kurtarma

Azure depolama, otomatik çoğaltmalar aracılığıyla veri dayanıklılığı sağlar. Ancak, bu, yanlışlıkla veya kötü amaçlı olarak, uygulama kodunun veya kullanıcıların verileri bozmasını engellemez. Uygulama veya Kullanıcı hatası durumunda verilerin aslına uygunluğunu korumak, verileri bir denetim günlüğü ile ikincil bir depolama konumuna kopyalama gibi daha gelişmiş teknikler gerektirir.

## <a name="checking-azure-storage-account-type"></a>Azure depolama hesabı türü denetleniyor

1. [Azure portalına](https://portal.azure.com/) gidin.

2. Depolama hesabınızı bulun.

3. **Genel bakış** bölümünde, **çoğaltmayı**kontrol edin.

   ![Görüntü](media/storage-account-container-recovery/1.png)

4. Çoğaltma türü **GRS/RA-GRS**ise, garanti olmadan hesap kapsayıcısı kurtarma mümkündür. Diğer tüm çoğaltma türleri için bu mümkün değildir.

5. Aşağıdaki bilgileri toplayın ve Microsoft Desteği bir destek isteği dosyasına koyun.

   * Depolama hesabı adı:
   * Kapsayıcı adı:
   * Silinme zamanı:

   Aşağıdaki tabloda, çoğaltma stratejisine bağlı olarak depolama hesabı kapsayıcısı kurtarma kapsamına genel bakış sunulmaktadır.

   |Içerik türü|LRS|ZRS|GRS|RA-GRS| 
   |---|---|---|---|---|
   |Depolama kapsayıcısı|Hayır|Hayır|Yes|Yes| 

   * Depolama hesabı kapsayıcısını ve garantisi olmadan geri yüklemeyi deneyebilirsiniz. 

## <a name="things-not-to-do-for-recover-to-be-successful"></a>Kurtarma işleminin başarılı olması için yapmanız gerekenler

* Lütfen kapsayıcıyı (aynı ada sahip) yeniden oluşturmayın.  
* Kapsayıcıyı zaten yeniden oluşturduysanız, kurtarma için bir destek isteği yapmadan önce kapsayıcıyı silmeniz gerekir.

## <a name="steps-to-prevent-this-in-the-future"></a>Gelecekte bunu engellemeye yönelik adımlar

1. Bundan sonra bunu önlemek için, en önerilen özelliği, [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)' dır.

2. [Anlık görüntü](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob) özelliğini de öneriyoruz.
 
## <a name="next-steps"></a>Sonraki adımlar

Bu özellik için iki örnek kod aşağıda verilmiştir:

  * [.NET 'te blob anlık görüntüsü oluşturma ve yönetme](https://docs.microsoft.com/azure/storage/storage-blob-snapshots)
  * [PowerShell ile blob anlık görüntülerini kullanma](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/)
  

