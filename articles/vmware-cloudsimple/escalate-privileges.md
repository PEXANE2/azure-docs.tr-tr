---
title: Azure VMware çözümleri (AVS)-AVS ayrıcalıklarına yükselt
description: AVS özel bulutu vCenter 'da yönetim işlevleri gerçekleştirmek için AVS izinlerinin nasıl ilerletiloluşturulacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 90dd61fc9856978bab0b68de19d48493a8e0c5fd
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025342"
---
# <a name="escalate-avs-privileges-to-perform-administrative-functions-in-avs-private-cloud-vcenter"></a>AVS özel bulutu vCenter 'da yönetim işlevleri gerçekleştirmek için AVS ayrıcalıklarına iletin

AVS ayrıcalıkları yaklaşımı, vCenter kullanıcılarına normal işlemler gerçekleştirmek için ihtiyaç duydukları ayrıcalıkları vermek üzere tasarlanmıştır. Bazı örneklerde, bir Kullanıcı belirli bir görevi gerçekleştirmek için ek ayrıcalıklar gerektirebilir. Bir vCenter SSO kullanıcısının ayrıcalıklarını sınırlı bir süre için ilerletebilirsiniz.

Yürüyen ayrıcalıkların nedenleri şunları içerebilir:

* Kimlik kaynakları yapılandırması
* Kullanıcı yönetimi
* Dağıtılmış bağlantı noktası grubu silme
* VCenter çözümlerini yükleme (yedekleme uygulamaları gibi)
* Hizmet hesapları oluşturma

> [!WARNING]
> İlerletilen ayrıcalıklı durumda gerçekleştirilen eylemler sisteminizi olumsuz etkileyebilir ve sisteminizin kullanılamaz hale gelmesine neden olabilir. Yalnızca yükseltme döneminde gerekli eylemleri gerçekleştirin.

AVS portalından, vCenter ' ın yerel kullanıcısına ait ayrıcalıkları vCenter SSO 'da [ilerletin](escalate-private-cloud-privileges.md) . Yalnızca vCenter üzerinde ek kimlik sağlayıcısı yapılandırılmışsa uzak kullanıcının ayrıcalığını ilerletebilirsiniz. Ayrıcalıkların artırılması, seçilen kullanıcıyı vSphere yerleşik Yöneticiler grubuna eklemeyi içerir. Yalnızca bir Kullanıcı ilerletilen ayrıcalıklara sahip olabilir. Başka bir kullanıcının ayrıcalıklarını yükseltmeniz gerekiyorsa, önce geçerli kullanıcıların ayrıcalıklarını de ilerletin.

Ek kimlik kaynaklarındaki kullanıcıların, CloudOwner grubunun üyesi olarak eklenmesi gerekir.

> [!CAUTION]
> Yeni kullanıcılar yalnızca *bulut sahibi grubu*, *bulut-genel-küme-yönetici-grubu*, *bulut-genel-depolama-yönetici-grubu*, bulut-genel- *Ağ-Yönetici-Grup* veya *bulut-genel-VM-yönetici grubu*için eklenmelidir.  *Yöneticiler* grubuna eklenen kullanıcılar otomatik olarak kaldırılacaktır.  Yalnızca hizmet hesaplarının *Yöneticiler* grubuna eklenmesi gerekir ve hizmet hesapları vSphere Web Kullanıcı arabiriminde oturum açmak için kullanılmamalıdır.
Yükseltme döneminde, AVS, ortamda yanlışlıkla yapılan değişiklikleri belirlemek için ilişkili uyarı bildirimleriyle otomatik izleme kullanır.
