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
ms.openlocfilehash: 0b5558501042dd7816202ea05b3a332b23400ff4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91345501"
---
# <a name="troubleshoot-capacity-pool-issues"></a>Kapasite havuzu sorunlarını giderme

Bu makalede, kapasite havuzlarını yönetirken karşılaşabileceğiniz sorunlara yönelik çözümler açıklanmaktadır. 

## <a name="issues-creating-a-capacity-pool"></a>Kapasite havuzu oluşturma sorunları

Kapasite havuzu sayısının sınırı aşmadığından emin olun. [Azure NetApp Files Için kaynak sınırlarına](azure-netapp-files-resource-limits.md)bakın.  Sayı sınırdan küçükse ve sorun yaşamaya devam ediyorsanız, bir destek bileti dosyası ve kapasite havuzu adını belirtin.

## <a name="issues-deleting-a-capacity-pool"></a>Bir kapasite havuzunu silme sorunları

Kapasite havuzunu silmeye çalıştığınız abonelikte tüm Azure NetApp Files birimlerini ve anlık görüntüleri kaldırdığınızdan emin olun.   

Tüm birimleri ve anlık görüntüleri zaten kaldırdıysanız ve yine de kapasite havuzunu silemiyorsanız, kaynaklara yapılan başvurular portalda gösterilmeden yine de mevcut olabilir. Bu durumda, bir destek bileti dosyası ve yukarıdaki önerilen adımları gerçekleştirdiğini belirtin. 

## <a name="volume-creation-or-modification-fails-with-requested-throughput-not-available-error"></a>Birim oluşturma veya değiştirme "Istenen üretilen iş miktarı yok" hatası ile başarısız oluyor

Bir birim için kullanılabilir verimlilik, kapasite havuzunun boyutu ve hizmet düzeyi tarafından belirlenir. Yeterli üretilen iş yoksa, havuz boyutunu artırmanız veya var olan birim aktarım hızını ayarlamanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar  

* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [El ile QoS kapasite havuzunu yönetme](manage-manual-qos-capacity-pool.md)
