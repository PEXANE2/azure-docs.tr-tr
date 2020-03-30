---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 410570302eec418f1e4bcb75d6413936a96b5171
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471585"
---
Koşullu Erişim, uygulama başına ince taneli erişim denetimine olanak tanır. Koşullu Erişim'i kullanmak için, Koşullu Erişim kurallarına tabi olacak kullanıcılara Azure AD Premium 1 veya daha fazla lisans uygulamanız gerekir.

1. **Kurumsal uygulamalara** gidin - Tüm uygulamalar sayfası ve **Azure VPN'i**tıklatın.

   - **Koşullu Erişim'i**tıklatın.
   - **Yeni** bölmeyi açmak için **Yeni ilkeyi** tıklatın.
2. **Yeni** bölmede, Kullanıcılar **ve gruplar > Atamalar'a**gidin. Kullanıcılar **ve gruplar ->** **Ekle** sekmesi:

   - **Kullanıcıları ve grupları seçin'i**tıklatın.
   - **Kullanıcıları ve grupları**kontrol edin.
   - MFA'dan etkilenecek bir grup veya kullanıcı kümesi seçmek için **Seç'i** tıklatın.
   - **Bitti**’ye tıklayın.

   ![Atamalar](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-assignments.png)
3. **Yeni** bölmede, **Access denetimleri -> Hibe** bölmesine gidin:

   - **Erişim Eki'ni Ver'i**tıklatın.
   - **Çok faktörlü kimlik doğrulamayı gerektir'i**tıklatın.
   - **Seçili tüm denetimleri iste'yi**tıklatın.
   - **Seç'i**tıklatın.
   
   ![Erişim izni - MFA](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-grant-mfa.png)
4. Etkinleştir **ilkesi** bölümünde:

   - **Üzerinde'ni**seçin.
   - **Oluştur'u**tıklatın.

   ![İlkeyi Etkinleştir](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-enable-policy.png)