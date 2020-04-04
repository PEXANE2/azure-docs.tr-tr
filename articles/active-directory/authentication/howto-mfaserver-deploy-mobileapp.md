---
title: Azure MFA Server Mobil Uygulama Web Hizmeti - Azure Active Directory
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
ms.openlocfilehash: 785c4ccf591ce6a7a50a3d144fb1740121c73ce6
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653185"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile mobil uygulama kimlik doğrulamasını etkinleştirme

Microsoft Authenticator uygulaması ek bir bant dışı doğrulama seçeneği sunar. Azure Çok Faktörlü Kimlik Doğrulama, oturum açma sırasında kullanıcıya otomatik bir telefon görüşmesi veya SMS yerleştirmek yerine, kullanıcının akıllı telefonu veya tabletinde Microsoft Authenticator uygulamasına bir bildirim gerçekleştirir. Kullanıcı oturum açma işlemini tamamlamak için uygulamadaki **Doğrula'ya** (veya PIN'e girer ve "Authenticate" dokunun) dokunur.

Şebeke sinyal gücünün güvenilir olmadığı durumlarda iki aşamalı doğrulama için bir mobil uygulama kullanmak tercih edilir. Uygulamayı bir OATH belirteci oluşturucu olarak kullanıyorsanız ağ veya İnternet bağlantısı gerekmez.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

> [!IMPORTANT]
> Azure Multi-Factor Authentication Sunucusu v8.x veya üzeri yüklüyse aşağıdaki adımları gerçekleştirmeniz gerekmez. [Mobil uygulamayı yapılandırma](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) bölümündeki adımlar izlenerek mobil uygulama kimlik doğrulaması ayarlanabilir.

## <a name="requirements"></a>Gereksinimler

Microsoft Authenticator uygulamasını kullanmak için Azure Multi-Factor Authentication Sunucusu v8.x veya üzerini çalıştırıyor olmanız gerekir

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma

1. Multi-Factor Authentication Sunucusu konsolunda Kullanıcı Portalı simgesine tıklayın. Kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin veriliyorsa, Ayarlar sekmesindeki **Kullanıcıların yöntemi seçmesine izin ver** bölümünden **Mobil Uygulama**’yı işaretleyin. Bu özellik etkinleştirilmeden, Mobil Uygulama için etkinleştirme işlemini tamamlamak üzere son kullanıcıların Yardım Masanızla iletişim kurması gerekir.
2. Mobil Uygulama kutusunu **etkinleştirmek için kullanabilmeleri için izin** ver'i kontrol edin.
3. Kullanıcı **Kaydına İzin Ver** kutusunu işaretleyin.
4. Mobil **Uygulama** simgesini tıklatın.
5. **Hesap adı** alanına bu hesabın mobil uygulamasında görüntülenecek şirket veya kuruluş adını girin.
   ![MFA Sunucusu yapılandırması Mobil Uygulama ayarları](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication ve üçüncü taraf VPN’ler ile gelişmiş senaryolar](howto-mfaserver-nps-vpn.md).
