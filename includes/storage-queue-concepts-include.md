---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.custom: seo-python-october2019
ms.openlocfilehash: 92f638666d9ac832ee5e6a7d4dccf9a9e669f908
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72427981"
---
## <a name="what-is-queue-storage"></a>Kuyruk depolama nedir?

Azure Kuyruk depolama, HTTP veya HTTPS kullanan kimlik doğrulaması yapılmış çağrılar aracılığıyla dünyanın her yerinden erişilebilen çok sayıda iletinin depolanması için bir hizmettir. Tek bir kuyruk iletisinin boyutu 64 KB’ye kadar olabilir ve bir kuyrukta, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti bulunabilir. Kuyruk depolama genellikle zaman uyumsuz olarak işlenecek iş biriktirme listesi oluşturmak için kullanılır.

## <a name="queue-service-concepts"></a>Kuyruk hizmeti kavramlar

Azure Kuyruk hizmeti aşağıdaki bileşenleri içerir:

![Azure Kuyruk hizmeti bileşenleri](./media/storage-queue-concepts-include/azure-queue-service-components.png)

* **URL biçimi:** Kuyruklar şu URL biçimi kullanılarak adreslenebilir:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Aşağıdaki URL diyagramdaki bir kuyruğun adresini belirtir:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Depolama Hesabı:** Tüm Azure Storage erişimi bir depolama hesabıyla yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../articles/storage/common/storage-scalability-targets.md).
* **Kuyruk:** Kuyrukta bir dizi ileti vardır. Tüm iletiler bir kuyrukta olmalıdır. Kuyruk adının tamamen küçük harfli olması gerektiğini unutmayın. Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **İleti:** İleti, biçimi ne olursa olsun en çok 64 KB büyüklüktedir. Bir iletinin kuyrukta kalabileceği en uzun süre 7 gündür.

