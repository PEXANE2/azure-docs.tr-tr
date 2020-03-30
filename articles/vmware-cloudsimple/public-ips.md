---
title: CloudSimple'a göre Azure VMware Çözümü - Genel IP adreslerini ayırma
description: Özel Bulut ortamındaki sanal makineler için genel IP adreslerinin nasıl ayrılacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 565c9ad0fbd37b026a1ba555d83b9032d2efcba4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024305"
---
# <a name="allocate-public-ip-addresses-for-private-cloud-environment"></a>Özel Bulut ortamı için genel IP adreslerini ayırma

Özel Bulut ortamınızdaki sanal makineler için genel IP adreslerini ayırmak için Ağ sayfasındaki Genel IP'ler sekmesini açın.

1. [CloudSimple portalına erişin](access-cloudsimple-portal.md) ve yan menüde **Ağ'ı** seçin.
2. **Genel IP'leri**seçin.
3. **Yeni Genel IP'yi**tıklatın.

    ![Genel IP'ler sayfası](media/public-ips-page.png)

4. IP adresi girişini tanımlamak için bir ad girin.
5. Varsayılan konumu koruyun.
6. Gerekirse boşta kalan zaman anına değiştirmek için kaydırıcıyı kullanın.
7. Herkese açık bir IP adresi atamak istediğiniz yerel IP adresini girin.
8. İlişkili bir DNS adı girin.
9. **Gönder'i**tıklatın.

![Genel IP'leri ayırma](media/network-public-ip-allocate.png)

Genel IP adresini ayırma görevi başlar. **Görevler etkinliği** sayfasında görevin durumunu > kontrol edebilirsiniz. Ayırma tamamlandığında, yeni giriş Genel IP'ler sayfasında gösterilir.
