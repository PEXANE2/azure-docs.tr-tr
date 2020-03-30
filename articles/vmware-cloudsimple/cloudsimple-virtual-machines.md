---
title: Sanal makinelere genel bakış
titleSuffix: Azure VMware Solution by CloudSimple
description: CloudSimple sanal makineleri ve avantajları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 436a5ab6fb286ce206f981487b6a29d774ecd1a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024917"
---
# <a name="cloudsimple-virtual-machines-overview"></a>CloudSimple sanal makinelere genel bakış

CloudSimple, Azure portalından VMware sanal makinelerini (VM'ler) yönetmenize olanak tanır.  vSphere kümenizden bir küme veya kaynak havuzu, aboneliğinize eşleyerek Azure üzerinden yönetilir.

Azure'dan bir CloudSimple VM oluşturmak için Özel Bulut vCenter'ınızda bir VM şablonu bulunması gerekir.  Şablon işletim sistemini ve uygulamaları özelleştirmek için kullanılır.  VM şablonu, kurumsal güvenlik ilkelerini karşılayacak şekilde sertleştirilmiş olabilir.  Şablonu, VM'ler oluşturmak ve bunları azure portalından self servis bir model kullanarak kullanmak için kullanabilirsiniz.

## <a name="benefits"></a>Avantajlar

Azure portalındaki CloudSimple sanal makineleri, kullanıcıların VMware sanal makineleri oluşturması ve yönetmesi için bir self servis mekanizması sağlar.

* Özel Bulut vCenter'ınızda CloudSimple VM oluşturma
* VM özelliklerini yönetme
  * Disk ekleme/kaldırma
  * NIC ekleme/kaldırma
* CloudSimple VM'nizin güç işlemleri
  * Güç ve kapalı
  * VM Sıfırlama
* VM Silme

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware VM'leri](quickstart-create-vmware-virtual-machine.md) Nasıl Tüketiriz öğrenin
* Azure aboneliğinizi nasıl [eşlenizi öğrenin](azure-subscription-mapping.md)
