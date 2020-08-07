---
title: 'Azure VPN Gateway: Noktadan siteye bağlantılar sorunlarını giderme: Mac OS X istemcileri'
description: 'Mac OS X Noktadan siteye bağlantı sorunlarını giderin. bu ayarları denetleyin: sunucu adresi, uzak KIMLIK, Yerel KIMLIK, kimlik doğrulama ayarları ve işletim sistemi sürümü.'
services: vpn-gateway
author: kumudD
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 03/27/2018
ms.author: alzam
ms.openlocfilehash: ae7ca9ad69bc5bea20fc959f86e41439bd01cd5c
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87920764"
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Mac OS X VPN istemcilerinden Noktadan siteye VPN bağlantıları sorunlarını giderme

Bu makale, yerel VPN istemcisi ve IKEv2 kullanarak Mac OS X Noktadan siteye bağlantı sorunlarını gidermenize yardımcı olur. Ikev2 için Mac 'teki VPN istemcisi çok temel ve çok özelleştirmeye izin vermez. Denetlenmesi gereken dört ayar vardır:

* Sunucu adresi
* Uzak KIMLIK
* Yerel KIMLIK
* Kimlik Doğrulama Ayarları
* İşletim sistemi sürümü (10,11 veya üzeri)


## <a name="troubleshoot-certificate-based-authentication"></a><a name="VPNClient"></a>Sertifika tabanlı kimlik doğrulaması sorunlarını giderme
1. VPN istemci ayarlarını kontrol edin. Komut + SHIFT tuşlarına basarak **ağ ayarına** gidin ve ardından VPN istemci ayarlarını denetlemek IÇIN "VPN" yazın. Listeden Araştırılması gereken VPN girdisine tıklayın.

   ![Ikev2 sertifika tabanlı kimlik doğrulaması](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. **Sunucu adresinin** tam FQDN olduğunu ve cloudapp.net içerdiğini doğrulayın.
3. **Uzak kimlik** , sunucu adresiyle (ağ geçidi FQDN) aynı olmalıdır.
4. **Yerel kimlik** , Istemci sertifikasının **konusuyla** aynı olmalıdır.
5. Kimlik doğrulama ayarları sayfasını açmak için **kimlik doğrulama ayarları** ' na tıklayın.

   ![Kimlik doğrulaması ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Açılan listeden **sertifikanın** seçildiğini doğrulayın.
7. **Seç** düğmesine tıklayın ve doğru sertifikanın seçildiğini doğrulayın. Değişiklikleri kaydetmek için **Tamam** ' ı tıklatın.

## <a name="troubleshoot-username-and-password-authentication"></a><a name="ikev2"></a>Kullanıcı adı ve parola kimlik doğrulaması sorunlarını giderme

1. VPN istemci ayarlarını kontrol edin. Komut + SHIFT tuşlarına basarak **ağ ayarına** gidin ve ardından VPN istemci ayarlarını denetlemek IÇIN "VPN" yazın. Listeden Araştırılması gereken VPN girdisine tıklayın.

   ![Ikev2 Kullanıcı adı parolası](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. **Sunucu adresinin** tam FQDN olduğunu ve cloudapp.net içerdiğini doğrulayın.
3. **Uzak kimlik** , sunucu adresiyle (ağ geçidi FQDN) aynı olmalıdır.
4. **Yerel kimlik** boş olabilir.
5. **Kimlik doğrulama ayarı** düğmesine tıklayın ve açılan listeden "Kullanıcı adı" seçeneğinin seçildiğini doğrulayın.

   ![Kimlik doğrulaması ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.png)
6. Doğru kimlik bilgilerinin girildiğinden emin olun.

## <a name="additional-steps"></a><a name="additional"></a>Ek adımlar

Önceki adımları ve her şeyi doğru şekilde yapılandırdıysanız, [Wireshark](https://www.wireshark.org/#download) indirin ve bir paket yakalama işlemi gerçekleştirin.

1. *ISAKMP* filtresi uygulayın ve **IKE_SA** paketlerine bakın. **Yük: güvenlik ilişkilendirmesi**altındaki sa teklif ayrıntılarına bakabilmeniz gerekir. 
2. İstemcisinin ve sunucunun ortak bir küme olduğunu doğrulayın.

   ![Paketle](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg) 
  
3. Ağ izlemelerinde sunucu yanıtı yoksa, Azure portal web sitesindeki Azure ağ geçidi yapılandırması sayfasında Ikev2 protokolünü etkinleştirdiğinizi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Ek Yardım için bkz. [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
