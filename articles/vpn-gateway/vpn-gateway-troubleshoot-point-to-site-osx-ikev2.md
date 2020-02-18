---
title: 'Azure VPN Gateway: Noktadan siteye bağlantılar sorunlarını giderme: Mac OS X istemcileri'
description: P2S Mac OS X VPN istemcisi bağlantılarında sorun giderme adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 03/27/2018
ms.author: alzam
ms.openlocfilehash: f88053c93884e10e46a0f7d70106bda67b057562
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425739"
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Mac OS X VPN istemcilerinden noktadan siteye VPN bağlantı sorunlarını giderme

Bu makale, Mac OS X yerel VPN istemcisini ve IKEv2'yi kullanarak noktadan siteye bağlantı sorunları gidermenize yardımcı olur. Mac için Ikev2 VPN istemcisi oldukça basittir ve kadar özelleştirme için izin vermez. Denetlenmesi gereken yalnızca dört ayar vardır:

* Sunucu Adresi
* Uzak Kimliği
* Yerel bir kimliği
* Kimlik doğrulama ayarları
* İşletim sistemi sürümü (10.11 veya üzeri)


## <a name="VPNClient"></a>Sertifika tabanlı kimlik doğrulaması sorunlarını giderme
1. VPN istemci ayarlarını kontrol edin. Komut + SHIFT tuşlarına basarak **ağ ayarına** gidin ve ardından VPN istemci ayarlarını denetlemek IÇIN "VPN" yazın. Listeden araştırılması gereken VPN girişe tıklayın.

   ![Ikev2 sertifika tabanlı kimlik doğrulaması](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. **Sunucu adresinin** tam FQDN olduğunu ve cloudapp.net içerdiğini doğrulayın.
3. **Uzak kimlik** , sunucu adresiyle (ağ geçidi FQDN) aynı olmalıdır.
4. **Yerel kimlik** , Istemci sertifikasının **konusuyla** aynı olmalıdır.
5. Kimlik doğrulama ayarları sayfasını açmak için **kimlik doğrulama ayarları** ' na tıklayın.

   ![Kimlik doğrulama ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Açılan listeden **sertifikanın** seçildiğini doğrulayın.
7. **Seç** düğmesine tıklayın ve doğru sertifikanın seçildiğini doğrulayın. Değişiklikleri kaydetmek için **Tamam** ' ı tıklatın.

## <a name="ikev2"></a>Kullanıcı adı ve parola kimlik doğrulaması sorunlarını giderme

1. VPN istemci ayarlarını kontrol edin. Komut + SHIFT tuşlarına basarak **ağ ayarına** gidin ve ardından VPN istemci ayarlarını denetlemek IÇIN "VPN" yazın. Listeden araştırılması gereken VPN girişe tıklayın.

   ![Ikev2 kullanıcı adı parola](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. **Sunucu adresinin** tam FQDN olduğunu ve cloudapp.net içerdiğini doğrulayın.
3. **Uzak kimlik** , sunucu adresiyle (ağ geçidi FQDN) aynı olmalıdır.
4. **Yerel kimlik** boş olabilir.
5. **Kimlik doğrulama ayarı** düğmesine tıklayın ve açılan listeden "Kullanıcı adı" seçeneğinin seçildiğini doğrulayın.

   ![Kimlik doğrulama ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.png)
6. Doğru kimlik bilgilerinin girildiğinden emin olun.

## <a name="additional"></a>Ek adımlar

Önceki adımları ve her şeyi doğru şekilde yapılandırdıysanız, [Wireshark](https://www.wireshark.org/#download) indirin ve bir paket yakalama işlemi gerçekleştirin.

1. *ISAKMP* filtresi uygulayın ve **IKE_SA** paketlerine bakın. **Yük: güvenlik ilişkilendirmesi**altındaki sa teklif ayrıntılarına bakabilmeniz gerekir. 
2. İstemci ve sunucu bir dizi ortak sahip olduğunuzu doğrulayın.

   ![Paket](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg) 
  
3. Ağ izlemelerinde sunucu yanıtı yoksa, Azure portal web sitesindeki Azure ağ geçidi yapılandırması sayfasında Ikev2 protokolünü etkinleştirdiğinizi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Ek Yardım için bkz. [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
