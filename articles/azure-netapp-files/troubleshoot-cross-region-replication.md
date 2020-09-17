---
title: Azure NetApp Files bölgeler arası çoğaltma sorunlarını giderme | Microsoft Docs
description: Azure NetApp Files için çapraz bölge çoğaltma sorunlarını gidermenize yardımcı olabilecek hata iletileri ve çözümleri açıklanmaktadır.
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
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: 3aaa5d2bc6fdbda0d1db212539c719aa65cae61b
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709171"
---
# <a name="troubleshoot-cross-region-replication"></a>Bölgeler arası çoğaltmanın sorunlarını giderme

Bu makalede, Azure NetApp Files için çapraz bölge çoğaltma sorunlarını gidermenize yardımcı olabilecek hata iletileri ve çözümleri açıklanmaktadır. 

## <a name="errors-creating-replication"></a>Çoğaltma oluşturma hataları  

|     Hata İletisi    |     Çözüm    |
|-|-|
|     `Volume {0} cannot   be used as source because it is already in replication`    |     Zaten veri çoğaltma ilişkisinde olan bir kaynak birimle çoğaltma oluşturamazsınız.    |
|     `Peered region   '{0}' is not accepted`    |     Eşlenmez bölgeler arasında bir çoğaltma oluşturmaya çalışıyorsunuz.    |
|     `RemoteVolumeResource   '{0}' of wrong type '{1}'`    |     Uzak kaynak KIMLIĞININ bir birim kaynak KIMLIĞI olduğunu doğrulayın.    |

## <a name="errors-authorizing-volume"></a>Birim yetkilendirme hatası  

|     Hata İletisi    |     Çözüm    |
|-|-|
|     `Missing value   for 'AuthorizeSourceReplication'`    |     `RemoteResourceID`Kullanıcı arabirimi veya API isteğinde eksik veya geçersiz (hata iletisini düzeltir).    |
|     `Missing value   for 'RemoteVolumeResourceId'`    |     `RemoteResourceID`Kullanıcı arabirimi veya API isteğinde eksik veya geçersiz.    |
|     `Data   Protection volume not found for RemoteVolumeResourceId: {remoteResourceId}`    |     `RemoteResourceID`Doğru olduğunu veya Kullanıcı için mevcut olduğunu doğrulayın.    |
|     `Remote volume   '{0}' is not configured for replication`    |     Hedef birim bir veri koruma birimi değil.    |
|     `Remote volume   '{0}' does not have source volume '{1}' as RemoteVolumeResourceId`    |     Veri koruma birimi, kendi uzak kaynak KIMLIĞINDE bu kaynak birime sahip değil (yanlış kaynak KIMLIĞI girildi).    |
|     `The   destination volume replication creation failed (message: {0})`    |     Bu hata bir sunucu hatasını gösterir. Desteğe başvurun.    |

## <a name="errors-deleting-replication"></a>Çoğaltma silme hataları

|     Hata İletisi    |     Çözüm    |
|-|-|
|     `Replication   cannot be deleted, mirror state needs to be in status: Broken before deleting`    |     Çoğaltmanın kesildiğini veya başlatılmamış olduğunu veya boşta olduğunu (başlatılamadı) doğrulayın.    |
|     `Cannot delete   source replication`    |     Kaynak taraftan çoğaltma silinmesine izin verilmiyor. Hedef taraftan çoğaltmayı sildiğinizden emin olun.    |

## <a name="errors-resyncing-volume"></a>Birim yeniden eşitleniyor hata

|     Hata İletisi    |     Çözüm    |
|-|-|
|     `Volume Replication is in invalid status: (Mirrored|Uninitialized) for operation: 'ResyncReplication'`     |     Birim çoğaltmasının "bozuk" durumunda olduğunu doğrulayın.    |

## <a name="errors-deleting-snapshot"></a>Anlık görüntü silme hataları 

|     Hata İletisi    |     Çözüm    |
|-|-|
|     `Snapshot   cannot be deleted, parent volume is a Data Protection volume with a   replication object`    |     Bu anlık görüntüyü silmek istiyorsanız birimin çoğaltmasını bozalcağınızı doğrulayın.    |
|     `Cannot delete   volume replication generated snapshot`    |     Çoğaltma temeli anlık görüntülerinin silinmesine izin verilmiyor.    |

## <a name="next-steps"></a>Sonraki adımlar  

* [Çapraz bölge çoğaltma](cross-region-replication-introduction.md)
* [Bölgeler arası çoğaltmayı kullanma gereksinimleri ve konuları](cross-region-replication-requirements-considerations.md)
* [Çoğaltma eşlemesi oluştur](cross-region-replication-create-peering.md)
* [Çoğaltma ilişkisinin sistem durumunu görüntüle](cross-region-replication-display-health-status.md)
* [Olağanüstü durum kurtarmayı yönetme](cross-region-replication-manage-disaster-recovery.md)
* [Bölgeler arası çoğaltmanın sorunlarını giderme](troubleshoot-cross-region-replication.md)
