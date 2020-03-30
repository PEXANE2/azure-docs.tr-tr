---
title: 'Azure VPN Ağ Geçidi: Sorun Giderme Noktadan Siteye bağlantılar: Mac OS X istemcileri'
description: P2S Mac OS X VPN istemci bağlantılarını giderme adımları
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 03/27/2018
ms.author: alzam
ms.openlocfilehash: f88053c93884e10e46a0f7d70106bda67b057562
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77425739"
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Mac OS X VPN istemcilerinden Site'ye noktadan gidervpn bağlantıları

Bu makale, ana bilgisayar VPN istemcisini ve IKEv2'yi kullanarak Mac OS X'ten Site'ye Nokta bağlantısı sorunlarını gidermenize yardımcı olur. IKEv2 için Mac VPN istemcisi çok temel ve çok özelleştirme için izin vermez. Denetlenmeleri gereken yalnızca dört ayar vardır:

* Sunucu Adresi
* Uzak Kimlik
* Yerel Kimlik
* Kimlik Doğrulama Ayarları
* İşletim Sistemi Sürümü (10.11 veya üzeri)


## <a name="troubleshoot-certificate-based-authentication"></a><a name="VPNClient"></a>Sorun giderme sertifikası tabanlı kimlik doğrulama
1. VPN istemcisi ayarlarını kontrol edin. Komut + Shift tuşuna basarak **Ağ Ayarı'na** gidin ve ardından VPN istemci ayarlarını kontrol etmek için "VPN" yazın. Listeden, araştırılması gereken VPN girişini tıklatın.

   ![IKEv2 sertifika tabanlı kimlik doğrulama](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. **Sunucu Adresinin** fqdn'nin tamamı olduğunu ve cloudapp.net içerdiğini doğrulayın.
3. **Uzak Kimlik,** Sunucu Adresi (Ağ Geçidi FQDN) ile aynı olmalıdır.
4. **Yerel kimlik,** istemci sertifikasının **konusuyla** aynı olmalıdır.
5. Kimlik **Doğrulama Ayarları** sayfasını açmak için Kimlik Doğrulama Ayarları sayfasını tıklatın.

   ![Kimlik doğrulama ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Açılan alt **kattan Sertifika'nın** seçildiğini doğrulayın.
7. **Seç** düğmesini tıklatın ve doğru sertifikanın seçilip seçilmediğini doğrulayın. Değişiklikleri kaydetmek için **Tamam'ı** tıklatın.

## <a name="troubleshoot-username-and-password-authentication"></a><a name="ikev2"></a>Sorun giderme kullanıcı adı ve parola kimlik doğrulaması

1. VPN istemcisi ayarlarını kontrol edin. Komut + Shift tuşuna basarak **Ağ Ayarı'na** gidin ve ardından VPN istemci ayarlarını kontrol etmek için "VPN" yazın. Listeden, araştırılması gereken VPN girişini tıklatın.

   ![IKEv2 kullanıcı adı şifresi](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. **Sunucu Adresinin** fqdn'nin tamamı olduğunu ve cloudapp.net içerdiğini doğrulayın.
3. **Uzak Kimlik,** Sunucu Adresi (Ağ Geçidi FQDN) ile aynı olmalıdır.
4. **Yerel kimlik** boş olabilir.
5. Kimlik **Doğrulama Ayarı** düğmesini tıklatın ve açılır dosyadan "Kullanıcı Adı"nın seçildiğini doğrulayın.

   ![Kimlik doğrulama ayarları](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.png)
6. Doğru kimlik bilgilerinin girilmesini doğrulayın.

## <a name="additional-steps"></a><a name="additional"></a>Ek adımlar

Önceki adımları denerseniz ve her şey düzgün yapılandırılırsa, [Wireshark'ı](https://www.wireshark.org/#download) indirin ve paket yakalama gerçekleştirin.

1. *Isakmp* filtre ve **IKE_SA** paketleri bakmak. Yük altında SA teklif ayrıntıları bakmak **gerekir: Güvenlik Derneği.** 
2. İstemcinin ve sunucunun ortak bir kümeye sahip olduğunu doğrulayın.

   ![Paket](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg) 
  
3. Ağ izlemelerinde sunucu yanıtı yoksa, Azure portalı web sitesindeki Azure Ağ Geçidi Yapılandırma sayfasında IKEv2 protokolünü etkinleştirdiğinizi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Ek yardım için [Microsoft Desteği'ne](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)bakın.
