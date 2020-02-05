---
title: Azure VMware çözümleri (AVS)-genel IP adreslerini ayır
description: AVS özel bulut ortamındaki sanal makineler için genel IP adreslerinin nasıl ayrılacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 87133f5efb9f096d3fdb0956aab1caac58b4bd94
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024305"
---
# <a name="allocate-public-ip-addresses-for-avs-private-cloud-environment"></a>AVS özel bulut ortamı için genel IP adresleri ayır

AVS özel bulut ortamınızdaki sanal makineler için genel IP adresleri ayırmak üzere ağ sayfasında genel IP 'Ler sekmesini açın.

1. [AVS portalına erişin](access-cloudsimple-portal.md) ve yan menüdeki **ağ** ' ı seçin.
2. **Genel IP 'leri**seçin.
3. **Yeni genel IP**' ye tıklayın.

    ![Genel IP 'Ler sayfası](media/public-ips-page.png)

4. IP adresi girişini tanımlamak için bir ad girin.
5. Varsayılan konumu koruyun.
6. Gerekirse, boşta kalma zaman aşımını değiştirmek için kaydırıcıyı kullanın.
7. Genel IP adresi atamak istediğiniz yerel IP adresini girin.
8. İlişkili bir DNS adı girin.
9. **Gönder**'e tıklayın.

![Genel IP 'Leri ayır](media/network-public-ip-allocate.png)

Genel IP adresini ayırma görevi başlar. Görevin durumunu **etkinlik > görevler** sayfasında kontrol edebilirsiniz. Ayırma tamamlandığında, yeni giriş genel IP 'Ler sayfasında gösterilir.
