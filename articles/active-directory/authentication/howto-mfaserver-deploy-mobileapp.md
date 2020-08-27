---
title: Azure MFA sunucusu mobil uygulama Web hizmeti-Azure Active Directory
description: MFA sunucusunu Microsoft Authenticator Uygulaması ile kullanıcılara anında iletme bildirimleri göndermek için yapılandırın.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84ed3ed26bc35feff016079117def256e8fe4ab5
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88919565"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile mobil uygulama kimlik doğrulamasını etkinleştirme

Microsoft Authenticator uygulaması ek bir bant dışı doğrulama seçeneği sunar. Azure Multi-Factor Authentication, oturum açma sırasında kullanıcıya otomatik telefon araması veya SMS koymak yerine kullanıcının akıllı telefonundaki veya tabletindeki Microsoft Authenticator uygulamasına bir bildirim gönderir. Kullanıcı, oturum açma işleminin tamamlanabilmesi için **Verify** (veya bir PIN girer ve "kimlik doğrulama" öğesine dokunduğunda).

Şebeke sinyal gücünün güvenilir olmadığı durumlarda iki aşamalı doğrulama için bir mobil uygulama kullanmak tercih edilir. Uygulamayı bir OATH belirteci oluşturucu olarak kullanıyorsanız ağ veya İnternet bağlantısı gerekmez.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle, Microsoft artık Yeni dağıtımlar için MFA sunucusu sağlamamaktadır. Oturum açma olayları sırasında çok faktörlü kimlik doğrulaması (MFA) gerektirmek isteyen yeni müşteriler bulut tabanlı Azure Multi-Factor Authentication kullanmalıdır.
>
> Bulut tabanlı MFA 'yı kullanmaya başlamak için bkz. [öğretici: Azure Multi-Factor Authentication Ile güvenli Kullanıcı oturum açma olayları](tutorial-enable-azure-mfa.md).
>
> MFA sunucusunu 1 Temmuz 2019 tarihinden önce etkinleştiren mevcut müşteriler, en son sürümü, gelecekteki güncelleştirmeleri indirebilir ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilir.

> [!IMPORTANT]
> Azure Multi-Factor Authentication Sunucusu v8.x veya üzeri yüklüyse aşağıdaki adımları gerçekleştirmeniz gerekmez. [Mobil uygulamayı yapılandırma](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) bölümündeki adımlar izlenerek mobil uygulama kimlik doğrulaması ayarlanabilir.

## <a name="requirements"></a>Gereksinimler

Microsoft Authenticator uygulamasını kullanmak için Azure Multi-Factor Authentication Sunucusu v8.x veya üzerini çalıştırıyor olmanız gerekir

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma

1. Multi-Factor Authentication Sunucusu konsolunda Kullanıcı Portalı simgesine tıklayın. Kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin veriliyorsa, Ayarlar sekmesindeki **Kullanıcıların yöntemi seçmesine izin ver** bölümünden **Mobil Uygulama**’yı işaretleyin. Bu özellik etkinleştirilmeden, Mobil Uygulama için etkinleştirme işlemini tamamlamak üzere son kullanıcıların Yardım Masanızla iletişim kurması gerekir.
2. **Kullanıcıların mobil uygulama etkinleştirmesine Izin ver** kutusunu işaretleyin.
3. **Kullanıcı kaydına Izin ver** kutusunu işaretleyin.
4. **Mobil uygulama** simgesine tıklayın.
5. **Hesap adı** alanına bu hesabın mobil uygulamasında görüntülenecek şirket veya kuruluş adını girin.
   ![MFA Sunucusu yapılandırması Mobil Uygulama ayarları](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication ve üçüncü taraf VPN’ler ile gelişmiş senaryolar](howto-mfaserver-nps-vpn.md).
