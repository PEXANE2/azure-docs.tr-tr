---
title: Azure kuyruklara giriş-Azure depolama
description: Azure kuyruklara giriş
author: mhopkins-msft
ms.author: mhopkins
ms.date: 06/07/2019
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: cbrooks
ms.openlocfilehash: 67e4874fcca93633140b7630ceadd273d1646f86
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721167"
---
# <a name="what-are-azure-queues"></a>Azure Kuyrukları nedir?

Azure kuyruk depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. HTTP veya HTTPS kullanarak kimliği doğrulanmış çağrılar aracılığıyla dünyanın her yerinden iletilere erişirsiniz. Kuyruk iletisi boyutu 64 KB 'ye kadar olabilir. Bir kuyruk, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir.

## <a name="common-uses"></a>Ortak kullanımlar

Kuyruk depolamanın yaygın kullanımları şunlardır:

* Zaman uyumsuz olarak işlemek için kapsamı oluşturma
* İletileri Azure web rolünden Azure çalışan rolüne geçirme

## <a name="queue-service-concepts"></a>Kuyruk hizmeti kavramlar

Kuyruk hizmetinde şu bileşenler bulunur:

![Kuyruk kavramları](./media/storage-queues-introduction/queue1.png)

* **URL biçimi:** Kuyruklar aşağıdaki URL biçimi kullanılarak adreslenebilir:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    Aşağıdaki URL diyagramdaki bir kuyruğun adresini belirtir:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Depolama hesabı:** Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında ayrıntılı bilgi için bkz. [Azure Storage Ölçeklenebilirlik ve Performans Hedefleri](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Sıradaki** Sıra bir ileti kümesi içerir. Sıra **adı tamamen** küçük harfle yazılmalıdır. Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **İleti:** Herhangi bir biçimde, 64 KB 'ye kadar bir ileti. Sürüm 2017-07-29 ' den önce, izin verilen maksimum yaşam süresi yedi gündür. Sürüm 2017-07-29 veya üzeri için, en fazla yaşam süresi herhangi bir pozitif sayı veya iletinin süresinin dolmadığını belirten-1 olabilir. Bu parametre atlanırsa, varsayılan yaşam süresi yedi gündür.

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [.NET kullanarak kuyruklar ile çalışmaya başlama](storage-dotnet-how-to-use-queues.md)
