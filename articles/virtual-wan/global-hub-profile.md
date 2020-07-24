---
title: Azure sanal WAN genel veya hub tabanlı VPN profillerini indirin | Microsoft Docs
description: Sanal WAN otomatik ölçeklenebilir dalı-dal bağlantısı, kullanılabilir bölgeler ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: kumudD
ms.service: virtual-wan
ms.topic: how-to
ms.date: 4/20/2020
ms.author: alzam
ms.openlocfilehash: d0fc2f608617ca00fea8b9ed5c4b874c65940263
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87064790"
---
# <a name="download-a-global-or-hub-based-profile-for-user-vpn-clients"></a>Kullanıcı VPN istemcileri için genel veya hub tabanlı bir profil indirin

Azure sanal WAN, uzak kullanıcılar için iki bağlantı türü sunar: genel ve hub tabanlı. Bir profili öğrenmek ve indirmek için aşağıdaki bölümleri kullanın. 

> [!IMPORTANT]
> RADIUS kimlik doğrulaması yalnızca hub tabanlı profili destekler.

## <a name="global-profile"></a>Genel profil

Profil, tüm etkin kullanıcı VPN hub 'larını içeren bir yük dengeleyiciyi işaret eder. Kullanıcı, kullanıcının coğrafi konumuna en yakın hub 'a yönlendirilir. Bu tür bir bağlantı, kullanıcılar farklı konumlara sık seyahat ederken faydalıdır. **Genel** profili indirmek için:

1. Sanal WAN'a gidin.
2. **Kullanıcı VPN yapılandırması**' na tıklayın.
3. Profili indirmek istediğiniz yapılandırmayı vurgulayın.
4. **Sanal WAN Kullanıcı VPN profilini indir**' e tıklayın.

   ![Genel profil](./media/global-hub-profile/global1.png)

## <a name="hub-based-profile"></a>Hub tabanlı profil

Profil, tek bir hub 'a işaret eder. Kullanıcı yalnızca bu profili kullanarak belirli bir hub 'a bağlanabilir. **Hub tabanlı** profili indirmek için:

1. Sanal WAN'a gidin.
2. Genel Bakış sayfasında **Merkez** ' e tıklayın.

    ![Hub profili 1](./media/global-hub-profile/hub1.png)
3. **Kullanıcı VPN (siteye işaret)** seçeneğine tıklayın.
4. **Sanal hub Kullanıcı VPN profilini indir**' e tıklayın.

   ![Hub profili 2](./media/global-hub-profile/hub2.png)
5. **EAPTLS**'yi denetleyin.
6. **Profil oluştur ve indir**' e tıklayın.

   ![Hub profili 3](./media/global-hub-profile/download.png)

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
