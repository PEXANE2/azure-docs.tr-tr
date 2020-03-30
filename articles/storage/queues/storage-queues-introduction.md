---
title: Azure Kuyruklarına Giriş - Azure Depolama
description: Azure Kuyruklarına Giriş
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/18/2020
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: cbrooks
ms.openlocfilehash: 4a2bea77578282d68d86bc1a8cea765aa2cbd555
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80060870"
---
# <a name="what-are-azure-queues"></a>Azure kuyrukları nedir?

Azure Kuyruk Depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. Http veya HTTPS kullanarak kimlik doğrulaması yapılan aramalar aracılığıyla dünyanın her yerinden gelen iletilere erişebilirsiniz. Bir sıra iletisi 64 KB boyutuna kadar olabilir. Bir sıra, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle eşzamanlı olarak işlemek için bir çalışma biriktirme listesi oluşturmak için kullanılır.

## <a name="queue-service-concepts"></a>Sıra hizmeti kavramları

Kuyruk hizmetinde şu bileşenler bulunur:

![Depolama hesabı, kuyruklar ve iletiler arasındaki ilişkiyi gösteren diyagram](./media/storage-queues-introduction/queue1.png)

* **URL biçimi:** Kuyruklar şu URL biçimi kullanılarak adreslenebilir:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    Aşağıdaki URL diyagramdaki bir kuyruğun adresini belirtir:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Depolama hesabı:** Azure Depolama'ya tüm erişim bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında daha fazla bilgi [için, standart depolama hesapları için Ölçeklenebilirlik ve performans hedeflerine](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)bakın.

* **Kuyruk:** Kuyrukta bir dizi ileti vardır. Sıra adının tümü küçük **olmalıdır.** Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **İleti:** İleti, biçimi ne olursa olsun en çok 64 KB büyüklüktedir. Sürüm 2017-07-29'dan önce, en fazla yaşama süresi yedi gündür. Sürüm 2017-07-29 veya daha sonraki sürümiçin, en fazla canlı süre herhangi bir pozitif sayı veya -1 iletinin süresinin dolmadığını belirten olabilir. Bu parametre atlanırsa, varsayılan olarak canlı çalışma süresi yedi gündür.

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [.NET kullanarak Kuyruklar ile başlarken](storage-dotnet-how-to-use-queues.md)
