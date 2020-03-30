---
title: Azure Virtual WAN global veya hub tabanlı VPN profillerini indirin | Microsoft Dokümanlar
description: Sanal WAN otomatik ölçeklenebilir daldan şubeye bağlantı, kullanılabilir bölgeler ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 1/31/2020
ms.author: alzam
ms.openlocfilehash: 3b7e765dbd024d46939e8989993f0c882b2a8f4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76965235"
---
# <a name="download-a-global-or-hub-based-profile-for-user-vpn-clients"></a>Kullanıcı VPN istemcileri için genel veya hub tabanlı bir profil indirin

Azure Virtual WAN uzak kullanıcılar için iki tür bağlantı sunar: Genel ve Hub tabanlı. Bir profil hakkında bilgi edinmek ve indirmek için aşağıdaki bölümleri kullanın.

## <a name="global-profile"></a>Genel profil

Profil, tüm etkin Kullanıcı VPN hub'larını içeren bir yük dengeleyicisini işaret eder. Kullanıcı, kullanıcının coğrafi konumuna en yakın hub'a yönlendirilir. Bu tür bir bağlantı, kullanıcılar sık sık farklı konumlara seyahat ettiğinde yararlıdır. **Genel** profili indirmek için:

1. Sanal WAN'a gidin.
2. **Kullanıcı VPN yapılandırmasını**tıklatın.
3. Profili indirmek istediğiniz yapılandırmayı vurgulayın.
4. **Tıklayın Sanal WAN kullanıcı VPN profili indirin.**

   ![Genel profil](./media/global-hub-profile/global1.png)

## <a name="hub-based-profile"></a>Hub tabanlı profil

Profil tek bir hub'ı işaret. Kullanıcı yalnızca bu profili kullanarak belirli bir hub'a bağlanabilir. **Hub tabanlı** profili indirmek için:

1. Sanal WAN'a gidin.
2. Genel Bakış sayfasında **Hub'ı** tıklatın.

    ![Hub profili 1](./media/global-hub-profile/hub1.png)
3. **Kullanıcı VPN'ini tıklatın (Siteye işaret edin)**.
4. **Sanal Hub Kullanıcı VPN profilini indir'e**tıklayın.

   ![Hub profili 2](./media/global-hub-profile/hub2.png)
5. **EAPTLS'yi kontrol edin.**
6. **Profili Oluştur ve İndir'i**tıklatın.

   ![Hub profili 3](./media/global-hub-profile/download.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
