---
title: Azure Site Recovery veri şifreleme özelliğinin kullanımdan kaldırılması | Microsoft Docs
description: Ayrıntılar Azure Site Recovery veri şifreleme özelliği
services: site-recovery
author: Sharmistha-Rai
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/15/2019
ms.author: sharrai
ms.openlocfilehash: 5860928d71c0e7431190908d5df5d7496e8ffb17
ms.sourcegitcommit: ac5cbef0706d9910a76e4c0841fdac3ef8ed2e82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89426325"
---
# <a name="deprecation-of-site-recovery-data-encryption-feature"></a>Site Recovery veri şifreleme özelliğinin kullanımdan kaldırılması

Bu belge, Hyper-V sanal makinelerini Azure 'a olağanüstü durum kurtarmayı yapılandırırken Site Recovery veri şifreleme özelliğini kullanıyorsanız uygulamanız gereken kullanımdan kaldırma ayrıntılarını ve düzeltme eylemini açıklar. 

## <a name="deprecation-information"></a>Kullanımdan kaldırma bilgileri


Site Recovery veri şifreleme özelliği, çoğaltılan verilerin güvenlik tehditlerine karşı korunmasını sağlamak üzere Hyper-V VM 'lerini koruyan müşteriler için kullanılabilir. Bu özellik **30 nisan 2022 ' den**kullanım dışı bırakılacak. [Depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md) (SSE) kullanan Rest özelliğinde daha gelişmiş [şifreleme](https://azure.microsoft.com/blog/azure-site-recovery-encryption-at-rest/) ile değiştiriliyor. SSE ile, veriler depolama alanı kalıcı hale gelmeden ve geri alınmadan önce şifrelenir ve Azure 'a yük devretmeden VM 'niz, geliştirilmiş kurtarma süresi hedefi (RTO) için şifrelenmiş depolama hesaplarından çalıştırılır.

Bu özelliği kullanan mevcut bir müşteriyseniz, kullanımdan kaldırma ayrıntıları ve düzeltme adımlarıyla iletişim aldığınızı lütfen unutmayın. 


## <a name="what-are-the-implications"></a>Etkileri nelerdir?

**30 nisan 2022**' den sonra, kullanımdan kaldırılan şifreleme özelliğini kullanmaya devam eden tüm VM 'lerin yük devretme yapmasına izin verilmez. 

## <a name="required-action"></a>Gerekli eylem
Başarılı yük devretme işlemlerine devam etmek için, çoğaltmalar aşağıda bahsedilen adımları izler:

Her VM için aşağıdaki adımları izleyin: 
1.  [Çoğaltmayı devre dışı bırakın](./site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).
2.  [Yeni bir çoğaltma Ilkesi oluşturun](./hyper-v-azure-tutorial.md#set-up-a-replication-policy).
3.  [Çoğaltmayı etkinleştirin](./hyper-v-vmm-azure-tutorial.md#enable-replication) ve SSE etkin bir depolama hesabı seçin.

SSE etkinken depolama hesaplarına ilk çoğaltmayı tamamladıktan sonra, VM 'niz Azure Site Recovery ile bekleyen şifrelemeyi kullanacaktır.


## <a name="next-steps"></a>Sonraki adımlar
Düzeltme adımlarını gerçekleştirmeyi planlayın ve bunları en erken yürütün. Bu kullanımdan kaldırma ile ilgili herhangi bir sorgunuz varsa, lütfen Microsoft Desteği ulaşın. Hyper-V ' d e Azure senaryosu hakkında daha fazla bilgi edinmek için [buraya](hyper-v-vmm-architecture.md)bakın.
