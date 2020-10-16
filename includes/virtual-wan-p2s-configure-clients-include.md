---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9caf63fc90be7bae0461ddc24c94594a32199765
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91812783"
---
#### <a name="microsoft-windows"></a>Microsoft Windows

##### <a name="openvpn"></a>OpenVPN

1. Resmi web sitesinden OpenVPN istemcisini indirip yükleyin.
1. Ağ geçidinin VPN profilini indirin. Bu, Azure portal veya PowerShell 'de New-AzureRmVpnClientConfiguration Kullanıcı VPN yapılandırması sekmesinden yapılabilir.
1. Profilin sıkıştırmasını açın. OpenVPN klasöründeki vpnconfig.ovpn yapılandırma dosyasını not defterinde açın.
1. P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEK biçimli bir sertifikada,. cer dosyasını açabilir ve sertifika üstbilgileri arasında Base64 anahtarı üzerinden kopyalama yapabilirsiniz. Adımlar için bkz [. kodlanmış ortak anahtarı almak için sertifikayı dışarı aktarma.](../articles/virtual-wan/certificates-point-to-site.md)
1. Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Adımlar için bkz [. özel anahtar ayıklama.](../articles/virtual-wan/howto-openvpn-clients.md#windows).
1. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
1. vpnconfig.ovpn dosyasını C:\Program Files\OpenVPN\config klasörüne kopyalayın.
1. Sistem tepsisindeki OpenVPN simgesine sağ tıklayın ve **Bağlan**' ı seçin.

##### <a name="ikev2"></a>IKEv2

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için 'VpnClientSetupAmd64' yükleyici paketini seçin. 32 bit işlemci mimarisi için 'VpnClientSetupX86' yükleyici paketini seçin.
1. Yüklemek için pakete çift tıklayın. Bir SmartScreen açılır penceresi görürseniz **daha fazla bilgi**' yi ve **yine de Çalıştır**' ı seçin.
1. İstemci bilgisayarda **ağ ayarları** ' na gidin ve **VPN**' yi seçin. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.
1. Bağlanmayı denemeden önce, istemci bilgisayara bir istemci sertifikası yüklediğinizi doğrulayın. Yerel Azure sertifika kimlik doğrulaması türü kullanılırken kimlik doğrulaması için bir istemci sertifikası gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için bkz. [sertifika oluşturma](../articles/virtual-wan/certificates-point-to-site.md). İstemci sertifikasını nasıl yükleyeceğiniz hakkında bilgi için bkz. [istemci sertifikası yüklemesi](../articles/vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md).
