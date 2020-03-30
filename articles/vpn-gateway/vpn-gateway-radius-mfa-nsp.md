---
title: MFA için VPN Ağ Geçidi RADIUS kimlik doğrulaması ile NPS'yi tümleştirin
description: Çok Faktörlü Kimlik Doğrulama için Azure ağ geçidi RADIUS kimlik doğrulamasını NPS sunucusuyla tümleştirmeyi açıklar.
services: vpn-gateway
documentationcenter: na
author: ahmadnyasin
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/16/2019
ms.author: genli
ms.openlocfilehash: 941b6ac86941824351f83592998e8735e3eb8ee5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75780377"
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Azure VPN ağ geçidi RADIUS kimlik doğrulamasını Çoklu Faktör Kimlik Doğrulama için NPS sunucusuyla tümleştirin 

Makalede, network policy server 'ın (NPS) azure VPN ağ geçidi YARıçapı kimlik doğrulaması ile nasıl entegre edilen çoklu faktörlü kimlik doğrulaması (MFA) noktaya vpn bağlantıları için nasıl entegre edilebiz. 

## <a name="prerequisite"></a>Önkoşul

MFA'yı etkinleştirmek için, kullanıcıların şirket içi veya bulut ortamından eşitlenmiş olması gereken Azure Etkin Dizini'nde (Azure AD) olması gerekir. Ayrıca, kullanıcı mfa için otomatik kayıt işlemini tamamlamış olmalıdır.  Daha fazla bilgi için [bkz.](../active-directory/user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="detailed-steps"></a>Ayrıntılı adımlar

### <a name="step-1-create-a-virtual-network-gateway"></a>Adım 1: Sanal ağ ağ geçidi oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sanal ağ ağ geçidini barındıracak sanal **ağda, Alt Ağlar'ı**seçin ve ardından bir alt ağ oluşturmak için Ağ Geçidi **alt ünü** seçin. 

    ![Ağ geçidi alt ağı nasıl eklendirilir hakkında resim](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Aşağıdaki ayarları belirterek sanal ağ ağ ağ geçidi oluşturun:

    - **Ağ geçidi türü**: **VPN**’i seçin.
    - **VPN türü**: **Rota tabanlı**seçin.
    - **SKU**: İhtiyaçlarınıza göre bir SKU türü seçin.
    - **Sanal ağ**: Ağ geçidi alt netini oluşturduğunuz sanal ağı seçin.

        ![Sanal ağ ağ geçidi ayarları yla ilgili görüntü](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>Adım 2 Azure MFA için NPS yapılandırma

1. NPS sunucusunda Azure [MFA için NPS uzantısını yükleyin.](../active-directory/authentication/howto-mfa-nps-extension.md#install-the-nps-extension)
2. NPS konsolu açın, **RADIUS İstemleri'ni**sağ tıklatın ve ardından **Yeni'yi**seçin. Aşağıdaki ayarları belirterek RADIUS istemcisini oluşturun:

    - **Friendly Name**: Herhangi bir ad yazın.
    - **Adres (IP veya DNS)**: Adım 1'de oluşturduğunuz ağ geçidi alt netini yazın.
    - **Paylaşılan gizli**: herhangi bir gizli anahtar yazın ve daha sonra kullanmak için hatırlıyorum.

      ![RADIUS istemci ayarları ile ilgili resim](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  **Gelişmiş** sekmesinde, satıcı adını **RADIUS Standardı'na** ayarlayın ve **Ek Seçenekler** onay kutusunun seçilmediğinden emin olun.

    ![RADIUS istemcisi Gelişmiş ayarları hakkında görüntü](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. **İlkeler** > **Ağ İlkeleri'ne**gidin, **Microsoft Yönlendirme ve Uzaktan Erişim sunucu ilkesine bağlanan bağlantıları** çift tıklatın, Erişim **Eki'ni**seçin ve **ardından Tamam'ı**tıklatın.

### <a name="step-3-configure-the-virtual-network-gateway"></a>Adım 3 Sanal ağ ağ geçidini yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Oluşturduğunuz sanal ağ ağ ağ geçidini açın. Ağ geçidi türünün **VPN** olarak ayarlandıklarına ve VPN türünün **rota tabanlı**olduğundan emin olun.
3. **Site yapılandırmasını** > şimdi**yapılandırmak**için Nokta'yı tıklatın ve ardından aşağıdaki ayarları belirtin:

    - **Adres havuzu**: Adım 1'de oluşturduğunuz ağ geçidi alt netini yazın.
    - **Kimlik doğrulama türü**: **RADIUS kimlik doğrulamasını**seçin.
    - **Sunucu IP adresi**: NPS sunucusunun IP adresini yazın.

      ![Site ayarlarına nokta ile ilgili resim](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Çok Faktörlü Kimlik Doğrulama](../active-directory/authentication/multi-factor-authentication.md)
- [Mevcut NPS altyapınızı Azure Multi-Factor Authentication ile tümleştirme](../active-directory/authentication/howto-mfa-nps-extension.md)
