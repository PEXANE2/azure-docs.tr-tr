---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.custom: seo-python-october2019
ms.openlocfilehash: 23b93f507ef6abe19a0202b28afa31d28490b2b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80151329"
---
## <a name="what-is-queue-storage"></a>Sıra depolama alanı nedir?

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir. Sıra depolama genellikle eşzamanlı olarak işlemek için bir çalışma biriktirme listesi oluşturmak için kullanılır.

## <a name="queue-service-concepts"></a>Sıra hizmeti kavramları

Azure Kuyruk hizmeti aşağıdaki bileşenleri içerir:

![Azure Sıra hizmet bileşenleri](./media/storage-queue-concepts-include/azure-queue-service-components.png)

* **URL biçimi:** Kuyruklar aşağıdaki URL biçimi kullanılarak giderilebilir: http://`<storage account>`.queue.core.windows.net/`<queue>`
  
    Aşağıdaki URL diyagramdaki bir kuyruğun adresini belirtir:  
  
    `http://myaccount.queue.core.windows.net/incoming-orders`

* **Depolama Hesabı:** Azure Depolama'ya tüm erişim bir depolama hesabı üzerinden yapılır. Depolama hesapları hakkında daha fazla bilgi için Bkz. [Depolama hesabı genel bakışı.](../articles/storage/common/storage-account-overview.md)
* **Kuyruk:** Kuyrukta bir dizi ileti vardır. Tüm iletiler bir kuyrukta olmalıdır. Kuyruk adının tamamen küçük harfli olması gerektiğini unutmayın. Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **İleti:** İleti, biçimi ne olursa olsun en çok 64 KB büyüklüktedir. Bir iletinin kuyrukta kalabileceği en uzun süre 7 gündür. Sürüm 2017-07-29 veya daha sonraki sürümiçin, en fazla canlı süre herhangi bir pozitif sayı veya -1 iletinin süresinin dolmadığını belirten olabilir. Bu parametre atlanırsa, varsayılan olarak canlı çalışma süresi yedi gündür.

