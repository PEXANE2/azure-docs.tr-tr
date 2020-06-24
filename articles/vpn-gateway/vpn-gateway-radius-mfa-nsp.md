---
title: MFA için VPN Gateway RADIUS kimlik doğrulamasıyla NPS 'YI tümleştirme
description: Multi-Factor Authentication için NPS sunucusu ile Azure Gateway RADIUS kimlik doğrulamasını tümleştirme açıklanır.
services: vpn-gateway
documentationcenter: na
author: ahmadnyasin
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/16/2019
ms.author: genli
ms.openlocfilehash: 8c439113907c2eb28c41aed3c21c1d27398d5207
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84987084"
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Multi-Factor Authentication için Azure VPN ağ geçidi RADIUS kimlik doğrulamasını NPS sunucusuyla tümleştirme 

Makalesinde, Noktadan siteye VPN bağlantıları için Multi-Factor Authentication (MFA) sunmak üzere Azure VPN Gateway RADIUS kimlik doğrulamasıyla ağ Ilkesi sunucusu 'nun (NPS) nasıl tümleştirileceği açıklanmaktadır. 

## <a name="prerequisite"></a>Önkoşul

MFA 'yı etkinleştirmek için kullanıcıların şirket içi veya bulut ortamından eşitlenmesi gereken Azure Active Directory (Azure AD) olması gerekir. Ayrıca, kullanıcının MFA için otomatik kayıt işlemini zaten tamamlamış olması gerekir.  Daha fazla bilgi için bkz. [iki adımlı doğrulama için hesabımı ayarlama](../active-directory/user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="detailed-steps"></a>Ayrıntılı adımlar

### <a name="step-1-create-a-virtual-network-gateway"></a>1. Adım: sanal ağ geçidi oluşturma

1. [Azure Portal](https://portal.azure.com)oturum açın.
2. Sanal ağ geçidini barındıracak sanal ağda **alt ağlar**' ı seçin ve ağ **geçidi alt** Ağı ' nı seçerek bir alt ağ oluşturun. 

    ![Ağ geçidi alt ağı ekleme hakkında resim](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Aşağıdaki ayarları belirterek bir sanal ağ geçidi oluşturun:

    - **Ağ geçidi türü**: **VPN**’i seçin.
    - **VPN türü**: **Rota tabanlı**' ı seçin.
    - **SKU**: gereksinimlerinize göre bir SKU türü seçin.
    - **Sanal ağ**: ağ geçidi alt ağını oluşturduğunuz sanal ağı seçin.

        ![Sanal ağ geçidi ayarları hakkında resim](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>2. adım Azure MFA için NPS 'YI yapılandırma

1. NPS sunucusunda, [Azure MFA IÇIN NPS uzantısını yükler](../active-directory/authentication/howto-mfa-nps-extension.md#install-the-nps-extension).
2. NPS konsolunu açın, **RADIUS istemcileri**' ne sağ tıklayın ve ardından **Yeni**' yi seçin. Aşağıdaki ayarları belirterek RADIUS istemcisini oluşturun:

    - **Kolay ad**: herhangi bir ad yazın.
    - **Adres (IP veya DNS)**: adım 1 ' de oluşturduğunuz ağ geçidi alt ağını yazın.
    - **Paylaşılan gizlilik**: herhangi bir gizli anahtarı yazın ve daha sonra kullanmak üzere hatırlayın.

      ![RADIUS istemci ayarları hakkında resim](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  **Gelişmiş** sekmesinde, satıcı adını **yarıçap standardı** olarak ayarlayın ve **ek seçenekler** onay kutusunun seçili olmadığından emin olun.

    ![RADIUS istemcisi Gelişmiş ayarları hakkında resim](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. **İlkeler**  >  **ağ ilkeleri**' ne gidin, **Microsoft Yönlendirme ve uzaktan erişim sunucusu ilkesi bağlantıları** ' na çift tıklayın, **erişim ver**' i seçin ve ardından **Tamam**' a tıklayın.

### <a name="step-3-configure-the-virtual-network-gateway"></a>3. adım sanal ağ geçidini yapılandırma

1. [Azure Portal](https://portal.azure.com)oturum açın.
2. Oluşturduğunuz sanal ağ geçidini açın. Ağ Geçidi türünün **VPN** olarak AYARLANDıĞıNDAN ve VPN türünün **Rota tabanlı**olduğundan emin olun.
3. **Site yapılandırması**  >  **Şimdi Yapılandır**' a tıklayın ve ardından aşağıdaki ayarları belirtin:

    - **Adres havuzu**: adım 1 ' de oluşturduğunuz ağ geçidi alt ağını yazın.
    - **Kimlik doğrulama türü**: **RADIUS kimlik doğrulamasını**seçin.
    - **Sunucu IP adresi**: NPS sunucusunun IP adresini yazın.

      ![Site ayarlarına işaret eden resim](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md)
- [Mevcut NPS altyapınızı Azure Multi-Factor Authentication ile tümleştirme](../active-directory/authentication/howto-mfa-nps-extension.md)
