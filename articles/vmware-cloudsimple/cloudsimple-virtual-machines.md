---
title: Azure VMware çözümleri (AVS)-sanal makinelere genel bakış
description: AVS sanal makineleri ve bunların avantajları hakkında bilgi edinin.
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0e6cc6d51e37cea98cc553a236295311d21f595a
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024917"
---
# <a name="avs-virtual-machines-overview"></a>AVS sanal makinelerine genel bakış

AVS, Azure portal VMware sanal makinelerini (VM) yönetmenizi sağlar. VSphere kümenizdeki bir küme veya kaynak havuzu, aboneliğiniz ile eşleyerek Azure üzerinden yönetilir.

Azure 'dan bir AVS sanal makinesi oluşturmak için, AVS özel bulut vCenter 'unuzda bir VM şablonu bulunmalıdır. Şablon, işletim sistemini ve uygulamaları özelleştirmek için kullanılır. Kurumsal güvenlik ilkelerini karşılamak için şablon sanal makinesi sağlamlaştırılmış olabilir. Sanal makine oluşturmak ve ardından bir self servis modeli kullanarak Azure portal kullanmak için şablonu kullanabilirsiniz.

## <a name="benefits"></a>Avantajlar

Azure portal AVS sanal makineleri, kullanıcıların VMware sanal makinelerini oluşturmalarına ve yönetmesine yönelik bir self servis mekanizması sağlar.

* AVS özel bulut vCenter 'unuzda bir AVS sanal makinesi oluşturma
* VM özelliklerini yönetme
  * Disk Ekle/Kaldır
  * NIC 'Leri Ekle/Kaldır
* AVS sanal makinenizin güç işlemleri
  * Açma ve kapatma
  * VM 'yi sıfırlama
* VM 'yi silme

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'Da VMware VM](quickstart-create-vmware-virtual-machine.md) 'lerini kullanmayı öğrenin
* [Azure aboneliğinizi nasıl eşleyeceğinizi](azure-subscription-mapping.md) öğrenin
