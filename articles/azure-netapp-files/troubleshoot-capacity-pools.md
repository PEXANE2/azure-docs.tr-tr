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
ms.date: 10/13/2020
ms.author: b-juche
ms.openlocfilehash: 54e6f4abd5ca6d15a4cc5a7bc9015abb005296a0
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92013653"
---
# <a name="troubleshoot-capacity-pool-issues"></a>Kapasite havuzu sorunlarını giderme

Bu makalede, havuz değiştirme işlemi de dahil olmak üzere kapasite havuzlarını yönetirken karşılaşabileceğiniz sorunlara yönelik çözümler açıklanmaktadır. 

## <a name="issues-managing-a-capacity-pool"></a>Kapasite havuzunu yönetme sorunları 

|     Hata koşulu    |     Çözüm    |
|-|-|
| Kapasite havuzu oluşturma sorunları |  Kapasite havuzu sayısının sınırı aşmadığından emin olun. [Azure NetApp Files Için kaynak sınırlarına](azure-netapp-files-resource-limits.md)bakın.  Sayı sınırdan küçükse ve sorun yaşamaya devam ediyorsanız, bir destek bileti dosyası ve kapasite havuzu adını belirtin. |
| Bir kapasite havuzunu silme sorunları  |  Kapasite havuzunu silmeye çalıştığınız abonelikte tüm Azure NetApp Files birimlerini ve anlık görüntüleri kaldırdığınızdan emin olun. <br> Tüm birimleri ve anlık görüntüleri zaten kaldırdıysanız ve yine de kapasite havuzunu silemiyorsanız, kaynak başvuruları portalda gösterilmeksizin yine de mevcut olabilir. Bu durumda, bir destek bileti dosyası ve yukarıdaki önerilen adımları gerçekleştirdiğini belirtin. |
| Birim oluşturma veya değiştirme hata vererek başarısız oluyor `Requested throughput not available` | Bir birim için kullanılabilir verimlilik, kapasite havuzunun boyutu ve hizmet düzeyi tarafından belirlenir. Yeterli üretilen iş yoksa, havuz boyutunu artırmanız veya var olan birim aktarım hızını ayarlamanız gerekir. | 

## <a name="issues-moving-a-capacity-pool"></a>Bir kapasite havuzunu taşıma sorunları 
|     Hata koşulu    |     Çözüm    |
|-|-|
| Bir birim için kapasite havuzunun değiştirilmesine izin verilmiyor. | Henüz bu özelliği kullanmak için yetkiniz olmayabilir. <br> Bir birimi başka bir kapasite havuzuna taşıma özelliği şu anda önizleme aşamasındadır. Bu özelliği ilk kez kullanıyorsanız, önce özelliği kaydetmeniz ve ayarlamanız gerekir `-FeatureName ANFTierChange` . [Bir birimin hizmet düzeyini dinamik olarak değiştirme](dynamic-change-volume-service-level.md)içindeki kayıt adımlarına bakın. |
| Kapasite havuzu boyutu toplam birim boyutu için çok küçük. |  Bu hata, hedef kapasite havuzunun, taşınmakta olan birim için kullanılabilir kapasiteye sahip olmayan bir sonucudur.  <br> Hedef havuzun boyutunu artırın veya daha büyük olan başka bir havuz seçin.  Bkz. [bir kapasite havuzunu veya birimi yeniden boyutlandırma](azure-netapp-files-resize-capacity-pools-or-volumes.md).   |
| Hedef kapasite havuzunun özgün KAPASİTE havuzundan farklı bir şifreleme türü olduğunda birim taşınamaz.  Örneğin, Çift şifrelemeden tek bir şifrelemeye geçiyorum veya tam tersi de geçerlidir.  | Kaynak kapasitesi havuzuyla aynı şifreleme türüne sahip bir hedef kapasite havuzu seçin.   |
|  Hedef havuzda zaten adlandırılmış bir birim bulunduğundan havuz değişikliği tamamlanamıyor `'{source pool name}'``'{target pool name}'` | Bu hata, aynı ada sahip bir birim hedef kapasite havuzunda zaten varolduğu için oluşur.  Aynı ada sahip bir birimi olmayan başka bir kapasite havuzu seçin.   | 

## <a name="next-steps"></a>Sonraki adımlar  

* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [El ile QoS kapasite havuzu yönetme](manage-manual-qos-capacity-pool.md)
* [Birimin hizmet düzeyini dinamik olarak değiştirme](dynamic-change-volume-service-level.md)
* [Kapasitesi havuzunu veya birimi yeniden boyutlandırma](azure-netapp-files-resize-capacity-pools-or-volumes.md)
