---
title: Azure NetApp Files için kapasite havuzu sorunlarını giderme | Microsoft Docs
description: Kapasite havuzlarını yönetirken oluşabilecek olası sorunları açıklar ve sorunlar için çözümler sağlar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/22/2020
ms.author: b-juche
ms.openlocfilehash: e7e3c2eb058d3549f2e1a10ffacf01fd354fbd47
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91651063"
---
# <a name="troubleshoot-capacity-pool-issues"></a>Kapasite havuzu sorunlarını giderme

Bu makalede, kapasite havuzlarını yönetirken karşılaşabileceğiniz sorunlara yönelik çözümler açıklanmaktadır. 

## <a name="error-conditions-and-resolutions"></a>Hata koşulları ve çözümleri 

|     Hata koşulu    |     Çözüm    |
|-|-|
| Kapasite havuzu oluşturma sorunları |  Kapasite havuzu sayısının sınırı aşmadığından emin olun. [Azure NetApp Files Için kaynak sınırlarına](azure-netapp-files-resource-limits.md)bakın.  Sayı sınırdan küçükse ve sorun yaşamaya devam ediyorsanız, bir destek bileti dosyası ve kapasite havuzu adını belirtin. |
| Bir kapasite havuzunu silme sorunları  |  Kapasite havuzunu silmeye çalıştığınız abonelikte tüm Azure NetApp Files birimlerini ve anlık görüntüleri kaldırdığınızdan emin olun. <br> Tüm birimleri ve anlık görüntüleri zaten kaldırdıysanız ve yine de kapasite havuzunu silemiyorsanız, kaynak başvuruları portalda gösterilmeksizin yine de mevcut olabilir. Bu durumda, bir destek bileti dosyası ve yukarıdaki önerilen adımları gerçekleştirdiğini belirtin. |
| Birim oluşturma veya değiştirme hata vererek başarısız oluyor `Requested throughput not available` | Bir birim için kullanılabilir verimlilik, kapasite havuzunun boyutu ve hizmet düzeyi tarafından belirlenir. Yeterli üretilen iş yoksa, havuz boyutunu artırmanız veya var olan birim aktarım hızını ayarlamanız gerekir. | 

## <a name="next-steps"></a>Sonraki adımlar  

* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [El ile QoS kapasite havuzu yönetme](manage-manual-qos-capacity-pool.md)
