---
title: CloudSimple ile VMware çözümü-Azure sanal makinelerine genel bakış
description: CloudSimple sanal makineleri ve bunların avantajları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0f4967bbd12107bf6a04cb80537d4425c75c5f46
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812498"
---
# <a name="cloudsimple-virtual-machines-overview"></a>CloudSimple sanal makinelerine genel bakış

CloudSimple, Azure portal VMware VM 'lerini yönetmenizi sağlar.  VSphere kümenizdeki bir küme veya kaynak havuzu, aboneliğiniz ile eşleyerek Azure üzerinden yönetilir.  CloudSimple sanal makinesi, Azure portal VMware VM 'lerinin self servis yönetimini getirir.  

Azure 'dan CloudSimple VM oluşturmak için özel bulut vCenter 'unuzda bir VM şablonu bulunmalıdır.  Şablon, işletim sistemini ve uygulamaları özelleştirmek için kullanılır.  Kurumsal güvenlik ilkelerini karşılamak için şablon sanal makinesi sağlamlaştırılmış olabilir.  Bir self servis modeli kullanarak Azure portal VM 'Ler oluşturmak ve bunları kullanmak için şablonu kullanabilirsiniz.

## <a name="benefits"></a>Avantajlar

Azure portal CloudSimple sanal makineleri, kullanıcıların VMware sanal makinelerini oluşturmalarına ve yönetmesine yönelik bir self servis mekanizması sağlar.

* Özel bulut vCenter 'unuzda CloudSimple VM oluşturma
* VM özelliklerini yönetme
  * Disk Ekle/Kaldır
  * NIC 'Leri Ekle/Kaldır
* CloudSimple VM 'nizin güç işlemleri
  * Açma ve kapatma
  * VM'yi Sıfırla
* VM 'yi silme

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'Da VMware VM](quickstart-create-vmware-virtual-machine.md) 'lerini kullanmayı öğrenin
* [Azure aboneliğinizi](https://docs.azure.cloudsimple.com/azure-subscription-mapping/) nasıl eşleyeceğinizi öğrenin