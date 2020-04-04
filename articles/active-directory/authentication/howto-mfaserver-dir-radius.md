---
title: RADIUS ve Azure MFA Server - Azure Active Directory
description: RADIUS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu dağıtılıyor.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 552226c35d4d129f73b96b689871708950b7ffb1
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652957"
---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>RADIUS kimlik doğrulamasını ve Azure Multi-Factor Authentication Sunucusuyla tümleştirme

RADIUS, kimlik doğrulama isteklerini kabul etmek ve bu istekleri işlemek için standart bir protokoldür. Azure Multi-Factor Authentication Sunucusu bir RADIUS sunucusu olarak görev yapabilir. İki aşamalı doğrulamayı eklemek için RADIUS istemciniz (VPN gereci) ile kimlik doğrulama hedefiniz arasına eklersiniz. Kimlik doğrulama hedefiniz, Active Directory, LDAP dizini ya da başka bir RADIUS sunucusu olabilir. Azure Multi-Factor Authentication’ın (MFA) çalışması için Azure MFA Sunucusu’nu hem istemci sunucuları hem de kimlik doğrulama hedefi ile iletişim kurabilecek şekilde yapılandırmalısınız. Azure MFA Sunucusu, RADIUS istemcisinden gelen istekleri kabul eder, kimlik doğrulama hedefine göre kimlik bilgilerini doğrular, Azure Multi-Factor Authentication ekler ve RADIUS istemcisine bir yanıt döndürür. Kimlik doğrulama isteği yalnızca hem birincil kimlik doğrulamasının hem de Azure Multi-Factor Authentication’ın başarılı olması durumunda başarılı olur.

> [!IMPORTANT]
> Bu makale yalnızca Azure MFA Server kullanıcıları içindir. Bulut tabanlı Azure MFA kullanıyorsanız, bunun yerine [Azure MFA için RADIUS kimlik doğrulaması ile nasıl tümleştirilebildiğinizi](howto-mfa-nps-extension.md)görün.
>
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

> [!NOTE]
> MFA sunucusu, RADIUS sunucusu olarak hareket ederken, yalnızca PAP (parola kimlik doğrulama protokolü) ve MSCHAPv2 (Microsoft Karşılıklı Kimlik Doğrulama Protokolü ) RADIUS protokollerini destekler.  MFA sunucusu bu protokolü destekleyen başka bir RADIUS sunucusu için RADIUS proxy işlevi görüyorsa, EAP (genişletilebilir kimlik doğrulama protokolü) gibi diğer protokoller kullanılabilir.
>
> Bu yapılandırmada, MFA Sunucusu alternatif protokolleri kullanarak başarılı bir RADIUS Sınama yanıtı başlatamadığından, tek yönlü SMS ve OATH belirteçleri çalışmaz.

![MFA Server'da Yarıçap Kimlik Doğrulaması](./media/howto-mfaserver-dir-radius/radius.png)

## <a name="add-a-radius-client"></a>RADIUS istemcisi ekleme

RADIUS kimlik doğrulamasını yapılandırmak için, bir Windows sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin. Bir Active Directory ortamınız varsa, sunucu ağ içindeki etki alanına eklenmelidir. Azure Multi-Factor Authentication Sunucusu’nu yapılandırmak için aşağıdaki yordamı uygulayın:

1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde RADIUS Kimlik Doğrulaması simgesine tıklayın.
2. RADIUS **kimlik doğrulama onay** kutusunu etkinleştir'i denetleyin.
3. Azure MFA RADIUS hizmetinin RADIUS isteklerini standart olmayan bağlantı noktalarında dinlemesi gerekiyorsa, İstemciler sekmesinden Kimlik Doğrulama ve Hesap Oluşturma bağlantı noktalarını değiştirin.
4. **Ekle**’ye tıklayın.
5. Azure Multi-Factor Authentication Sunucusu için kimlik doğrulaması yapacak gerecin/sunucunun IP adresini, bir uygulama adı (isteğe bağlı) ve paylaşılan bir gizli dizi girin.

   Uygulama adı raporlarda görünür ve SMS veya mobil uygulama kimlik doğrulama iletilerinde görüntülenebilir.

   Paylaşılan gizli dizinin Azure Multi-Factor Authentication Sunucusu’nda ve gereçte/sunucuda aynı olması gerekir.

6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ve multi-factor authentication’a tabi olacaksa, **Multi-Factor Authentication İste kullanıcı eşleme** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın.
7. Mobil doğrulama uygulamalarınızdan edindiğiniz OATH parolalarını yedekleme yöntemi olarak kullanmak istiyorsanız **Yedek OATH belirtecini etkinleştir** kutusunu işaretleyin.
8. **Tamam**'a tıklayın.

4 adımdan 8 adıma kadar yapılan işlemleri tekrarlayarak dilediğiniz kadar RADIUS istemcisi ekleyebilirsiniz.

## <a name="configure-your-radius-client"></a>RADIUS istemcinizi yapılandırma

1. **Hedef** sekmesine tıklayın.
   * Azure MFA Server, Etkin Dizin ortamında etki alanı birleştirilmiş sunucuya yüklüyse, **Windows etki alanını**seçin.
   * Kullanıcılara LDAP dizinine göre kimlik doğrulaması uygulanması gerekiyorsa **LDAP bağlaması**’nı seçin.
      Sunucu’nun dizininize bağlanabilmesi için Dizin Tümleştirme simgesine tıklayın ve Ayarlar sekmesinde LDAP yapılandırmasını düzenleyin. LDAP yapılandırma yönergeleri [LDAP Proxy yapılandırma kılavuzunda](howto-mfaserver-dir-ldap.md) bulunabilir.
   * Kullanıcıların başka bir RADIUS sunucusuna karşı kimlik doğrulaması yapılacaksa, **RADIUS sunucusu(lar)'ı**seçin.
1. Azure MFA Sunucusu’nun RADIUS istekleri için proxy olarak kullanacağı sunucuyu yapılandırmak için **Ekle**’ye tıklayın.
1. RADIUS Sunucusu Ekle iletişim kutusuna RADIUS sunucusunun IP adresi İle paylaşılan bir gizli dizi girin.

   Paylaşılan gizli dizinin Azure Multi-Factor Authentication Sunucusu’nda ve RADIUS sunucusunda aynı olması gerekir. RADIUS sunucusu tarafından farklı bağlantı noktaları kullanılıyorsa, Kimlik Doğrulama bağlantı noktasını ve Hesap bağlantı noktasını değiştirin.

1. **Tamam**'a tıklayın.
1. Azure MFA Sunucusu’ndan gönderilen erişim isteklerini işleyebilmesi için Azure MFA Sunucusu’nu başka bir RADIUS sunucusunda RADIUS istemcisi olarak ekleyin. Azure Multi-Factor Authentication Sunucusu’nda yapılandırılanla aynı paylaşılan gizli diziyi kullanın.

Başka RADIUS sunucuları eklemek için bu adımları yineleyin. **Yukarı Taşı** ve **Aşağı Taşı** düğmeleriyle Azure MFA Sunucusu’nun bunları çağıracağı sırayı yapılandırabilirsiniz.

Azure Multi-Factor Authentication Sunucusu’nu başarıyla yapılandırdınız. Sunucu artık yapılandırılan istemcilerden gelen RADIUS erişim istekleri için yapılandırılan bağlantı noktalarını dinler.

## <a name="radius-client-configuration"></a>RADIUS İstemcisi yapılandırması

RADIUS istemcisini yapılandırmak için yönergeleri kullanın:

* Cihazınızı/sunucunuzu RADIUS üzerinden, RADIUS sunucusu gibi davranan Azure Çok Faktörlü Authentication Server'ın IP adresine doğru doğrulayabilmek için yapılandırın.
* Daha önce yapılandırılanla aynı paylaşılan gizli diziyi kullanın.
* Kullanıcının kimlik bilgilerini doğrulamak, iki aşamalı doğrulama gerçekleştirmek, yanıtlarını almak ve sonra RADIUS erişim isteğine yanıt vermek için zaman olacak şekilde RADIUS zaman aşımını 30-60 saniyeye yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bulutta Azure Multi-Factor Authentication kullanıyorsanız [RADIUS kimlik doğrulaması ile tümleştirmeyi](howto-mfa-nps-extension.md) öğrenin. 
