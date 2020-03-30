---
title: CloudSimple tarafından Azure VMware Çözümü - CloudSimple ayrıcalıklarını artırın
description: CloudSimple izinlerinin Özel Bulut vCenter'da yönetim işlevlerini gerçekleştirmek için nasıl yükseltileni açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 36c6969ed89d0bb9222f52aa81de0d4128b9e533
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025342"
---
# <a name="escalate-cloudsimple-privileges-to-perform-administrative-functions-in-private-cloud-vcenter"></a>Private Cloud vCenter'da yönetim işlevleri gerçekleştirmek için CloudSimple ayrıcalıklarını artırın

CloudSimple ayrıcalıkları yaklaşımı, vCenter kullanıcılarına normal işlemleri gerçekleştirmek için ihtiyaç duydukları ayrıcalıkları sağlamak üzere tasarlanmıştır. Bazı durumlarda, bir kullanıcı belirli bir görevi gerçekleştirmek için ek ayrıcalıklar gerektirebilir.  Bir vCenter SSO kullanıcısının ayrıcalıklarını sınırlı bir süre için artırabilirsiniz.

Ayrıcalıkları tırmandırma nedenleri şunlardır:

* Kimlik kaynaklarının yapılandırması
* Kullanıcı yönetimi
* Dağıtılmış bağlantı noktası grubunun silinmesi
* vCenter çözümlerinin yüklenmesi (yedekleme uygulamaları gibi)
* Hizmet hesapları oluşturma

> [!WARNING]
> Artırılmış ayrıcalıklı durumda gerçekleştirilen eylemler sisteminizi olumsuz etkileyebilir ve sisteminizin kullanılamamasına neden olabilir. Yalnızca yükseltme döneminde gerekli eylemleri gerçekleştirin.

CloudSimple portalından, vCenter SSO'daki CloudOwner yerel kullanıcısıiçin [ayrıcalıkları artırın.](escalate-private-cloud-privileges.md)  Uzak kullanıcıayrıcalığını yalnızca vCenter'da ek kimlik sağlayıcısı yapılandırılırsa artırabilirsiniz.  Ayrıcalıkların artması, seçilen kullanıcının vSphere yerleşik Yöneticiler grubuna eklenmesini içerir.  Yalnızca bir kullanıcı nın artırılmış ayrıcalıkları olabilir.  Başka bir kullanıcının ayrıcalıklarını yükseltmeniz gerekiyorsa, önce geçerli kullanıcıların ayrıcalıklarını artırın.

Ek kimlik kaynaklarından gelen kullanıcılar CloudOwner grubunun üyeleri olarak eklenmelidir.

> [!CAUTION]
> Yeni kullanıcılar yalnızca *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group,* *Cloud-Global-Network-Admin-Group* veya *Cloud-Global-VM-Admin-Group'a*eklenmelidir.  *Yöneticiler* grubuna eklenen kullanıcılar otomatik olarak kaldırılır.  *Yöneticiler* grubuna yalnızca hizmet hesapları eklenmelidir ve hizmet hesapları vSphere web UI'da oturum açmak için kullanılmamalıdır.

Yükseltme döneminde CloudSimple, ortama yapılan yanlışlıkla yapılan değişiklikleri belirlemek için ilişkili uyarı bildirimleriyle birlikte otomatik izleme kullanır.
