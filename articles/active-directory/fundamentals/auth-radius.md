---
title: Azure Active Directory ile RADIUS kimlik doğrulaması
description: Azure Active Directory ile RADIUS kimlik doğrulamasını elde etmeye yönelik mimari rehberlik.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 155b359c109de948ab9b9d6862ef7507ee76f619
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94576821"
---
# <a name="radius-authentication-with-azure-active-directory"></a>Azure Active Directory ile RADIUS kimlik doğrulaması

Arayan Kullanıcının Uzaktan Kimliğini Doğrulama Hizmeti (YARıÇAP), çevirmeli kullanıcıların merkezi kimlik doğrulaması ve yetkilendirmesini etkinleştirerek bir ağın güvenliğini sağlayan bir ağ protokolüdür. Birçok uygulama, kullanıcıların kimliğini doğrulamak için RADIUS protokolüne hala güvenir.

Microsoft Windows Server, bir RADIUS sunucusu olarak görev yapacak ve RADIUS kimlik doğrulamasını destekleyen ağ Ilkesi sunucusu (NPS) adlı bir role sahiptir.

Azure Active Directory (Azure AD), RADIUS tabanlı sistemlerle Multi-Factor Authentication 'ı sunar. Bir müşteri, daha önce bahsedilen RADIUS iş yüklerinden herhangi birine Azure Multi-Factor Authentication uygulamak isterse, Azure Multi-Factor Authentication NPS uzantısını Windows NPS sunucusuna yükleyebilir. 

Windows NPS sunucusu bir kullanıcının kimlik bilgilerini Active Directory doğrular ve sonra Multi-Factor Authentication isteği Azure 'a gönderir. Böylece Kullanıcı, mobil kimlik doğrulayıcısında bir zorluk alır. Başarılı olduktan sonra istemci uygulamanın hizmete bağlanmasına izin verilir. 

## <a name="use-when"></a>Şu durumlarda kullanın: 

Şunun gibi uygulamalara Multi-Factor Authentication eklemeniz gerekir
* bir sanal özel ağ (VPN)
* WiFi erişimi
* Uzak Masaüstü Ağ Geçidi (RDG)
* Sanal Masaüstü Altyapısı (VDı)
* Kullanıcılara kimlik doğrulaması yapmak için RADIUS protokolüne bağlı olan diğerleri. 

> [!NOTE]
> VPN iş yüklerine Azure Multi-Factor Authentication uygulamak için RADIUS ve Azure Multi-Factor Authentication NPS uzantısına güvenmek yerine, VPN 'nizi SAML 'ye yükseltmenizi ve Azure AD ile VPN 'nizi doğrudan Federasyonu öneririz. Bu, VPN 'nize koşullu erişim, Multi-Factor Authentication, cihaz uyumluluğu ve kimlik koruması gibi Azure AD korumasının tam kapsamını verir.

![mimari diyagram](./media/authentication-patterns/radius-auth.png)


## <a name="components-of-the-system"></a>Sistemin bileşenleri 

* **İstemci uygulaması (VPN istemcisi)** : RADIUS istemcisine kimlik doğrulama isteği gönderir.

* **RADIUS istemcisi** : istemci uygulamasından gelen istekleri dönüştürür ve NPS uzantısının yüklü olduğu RADIUS sunucusuna gönderir.

* **RADIUS sunucusu** : RADIUS isteği için birincil kimlik doğrulamasını gerçekleştirmek üzere Active Directory ile bağlanır. Başarılı olduğunda, isteği Azure Multi-Factor Authentication NPS uzantısına geçirir.

* **NPS uzantısı** : ikincil kimlik doğrulaması Için bir Azure Multi-Factor Authentication isteği tetikler. Başarılı olursa, NPS uzantısı, Azure 'un güvenlik belirteci hizmeti tarafından verilen Multi-Factor Authentication talebi içeren güvenlik belirteçleri ile RADIUS sunucusunu sağlayarak kimlik doğrulama isteğini tamamlar.

* **Azure Multi-Factor Authentication** : kullanıcının ayrıntılarını almak IÇIN Azure AD ile iletişim kurar ve Kullanıcı tarafından yapılandırılan bir doğrulama yöntemi kullanarak ikincil kimlik doğrulaması gerçekleştirir.

## <a name="implement-radius-with-azure-ad"></a>Azure AD ile RADIUS uygulama 

* [NPS kullanarak Azure Multi-Factor Authentication olanakları sağlama](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-nps-extension) 

* [Azure Multi-Factor Authentication NPS uzantısını yapılandırma](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-nps-extension-advanced) 

* [NPS uzantısını kullanarak Azure Multi-Factor Authentication ile VPN](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-nps-extension-vpn) 

  
‎ 

 
