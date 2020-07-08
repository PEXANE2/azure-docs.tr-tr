---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 410570302eec418f1e4bcb75d6413936a96b5171
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77471585"
---
Koşullu erişim, uygulama başına ayrıntılı erişim denetimi sağlar. Koşullu erişimi kullanabilmeniz için, koşullu erişim kurallarına tabi olacak kullanıcılara Azure AD Premium 1 veya daha büyük bir lisanslama uygulanmış olmalıdır.

1. **Kurumsal uygulamalar-tüm uygulamalar** sayfasına gidin ve **Azure VPN**' ye tıklayın.

   - **Koşullu erişim**' e tıklayın.
   - Yeni **ilke** ' ye tıklayarak **Yeni** bölmeyi açın.
2. **Yeni** bölmede, **atamalar-> kullanıcılar ve gruplar**' a gidin. **Kullanıcılar ve gruplar->** **ekleme** sekmesi:

   - **Kullanıcıları ve grupları seç**' e tıklayın.
   - **Kullanıcıları ve grupları**denetleyin.
   - MFA 'dan etkilenecek bir grup veya kullanıcı kümesi seçmek için **Seç** ' e tıklayın.
   - **Bitti**’ye tıklayın.

   ![Atamalar](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-assignments.png)
3. **Yeni** bölmede, **erişim denetimleri-> izin** bölmesine gidin:

   - **Erişim ver**' e tıklayın.
   - **Multi-Factor Authentication gerektir**' e tıklayın.
   - **Seçili tüm denetimleri gerektir**' e tıklayın.
   - **Seç**' e tıklayın.
   
   ![Erişim verme-MFA](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-grant-mfa.png)
4. **Ilkeyi etkinleştir** bölümünde:

   - **Açık**seçeneğini belirleyin.
   - **Oluştur**'a tıklayın.

   ![Ilkeyi etkinleştir](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-enable-policy.png)