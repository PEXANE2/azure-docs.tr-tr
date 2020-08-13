---
title: Sanal makinelere genel bakış
titleSuffix: Azure VMware Solution by CloudSimple
description: CloudSimple sanal makineleri ve bunların avantajları hakkında bilgi edinin. VMware sanal makinelerini Azure portal yönetebilirsiniz.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6f9be035978667287b8b88ec1bb64b3882b0f929
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88141985"
---
# <a name="cloudsimple-virtual-machines-overview"></a>CloudSimple sanal makinelerine genel bakış

CloudSimple, Azure portal VMware sanal makinelerini (VM) yönetmenizi sağlar.  VSphere kümenizdeki bir küme veya kaynak havuzu, aboneliğiniz ile eşleyerek Azure üzerinden yönetilir.

Azure 'dan CloudSimple VM oluşturmak için özel bulut vCenter 'unuzda bir VM şablonu bulunmalıdır.  Şablon, işletim sistemini ve uygulamaları özelleştirmek için kullanılır.  Kurumsal güvenlik ilkelerini karşılamak için şablon sanal makinesi sağlamlaştırılmış olabilir.  Sanal makine oluşturmak ve ardından bir self servis modeli kullanarak Azure portal kullanmak için şablonu kullanabilirsiniz.

## <a name="benefits"></a>Yararları

Azure portal CloudSimple sanal makineleri, kullanıcıların VMware sanal makinelerini oluşturmalarına ve yönetmesine yönelik bir self servis mekanizması sağlar.

* Özel bulut vCenter 'unuzda CloudSimple VM oluşturma
* VM özelliklerini yönetme
  * Disk Ekle/Kaldır
  * NIC 'Leri Ekle/Kaldır
* CloudSimple VM 'nizin güç işlemleri
  * Açma ve kapatma
  * VM Sıfırlama
* VM Silme

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'Da VMware VM](quickstart-create-vmware-virtual-machine.md) 'lerini kullanmayı öğrenin
* [Azure aboneliğinizi nasıl eşleyeceğinizi](azure-subscription-mapping.md) öğrenin
