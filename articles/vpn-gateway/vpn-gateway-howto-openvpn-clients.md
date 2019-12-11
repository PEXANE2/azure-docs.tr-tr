---
title: Azure VPN Gateway için OpenVPN Istemcilerini yapılandırma | Microsoft Docs
description: Azure VPN Gateway için OpenVPN Istemcilerini yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: cherylmc
ms.openlocfilehash: 3366f3470e01e455acacf8748830f2b15c826f49
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74997182"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN Gateway için OpenVPN istemcileri yapılandırma

Bu makale, **OpenVPN® Protokolü** istemcilerini yapılandırmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

VPN ağ geçidiniz için OpenVPN yapılandırma adımlarını tamamladığınızdan emin olun. Ayrıntılar için bkz. [Azure Için OpenVPN 'ı yapılandırma VPN Gateway](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Windows istemcileri

1. OpenVPN istemcisini (sürüm 2,4 veya üzeri) resmi [OpenVPN Web sitesinden](https://openvpn.net/index.php/open-source/downloads.html)indirip yükleyin.
2. Ağ geçidinin VPN profilini indirin. Bu, Azure portal ' deki Noktadan siteye Yapılandırma sekmesinden veya PowerShell 'deki ' New-AzVpnClientConfiguration ' öğesinden yapılabilir.
3. Profilin sıkıştırmasını açın. Ardından, Not defteri 'Ni kullanarak OpenVPN klasöründen *vpnconfig. ovpn* yapılandırma dosyasını açın.
4. Oluşturduğunuz ve ağ geçidinde P2S yapılandırmanıza yüklediğiniz P2S istemci sertifikasını [dışarı aktarın](vpn-gateway-certificates-point-to-site.md#clientexport) .
5. Özel anahtarı ve Base64 parmak izini *. pfx*dosyasından ayıklayın. Bunu yapmanın birden çok yolu vardır. Makinenizde OpenSSL kullanmak tek bir yoldur. *ProfileInfo. txt* dosyası, CA ve istemci sertifikası için özel anahtarı ve parmak izini içerir. İstemci sertifikasının parmak izini kullandığınızdan emin olun.

   ```
   openssl pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
   ```
6. Not defteri 'nde *ProfileInfo. txt* dosyasını açın. İstemci (alt) sertifikasının parmak izini almak için, alt sertifika için "-----başlangıç SERTIFIKASı-----" ve "-----son SERTIFIKA-----" gibi metni seçin ve kopyalayın. Konu =/satırına bakarak alt sertifikayı belirleyebilirsiniz.
7. Adım 3 ' teki Not defteri 'nde açtığınız *vpnconfig. ovpn* dosyasına geçin. Aşağıda gösterilen bölümü bulun ve "CERT" ve "/CERT" arasındaki her şeyi değiştirin.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Not defteri 'nde *ProfileInfo. txt* dosyasını açın. Özel anahtarı almak için, "özel anahtar----------" ve "-----son özel anahtar-----
9. Not defteri 'nde vpnconfig. ovpn dosyasına dönün ve bu bölümü bulun. Özel anahtarı, ve "Key" ve "/Key" arasındaki her şeyi değiştirerek yapıştırın.

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```
10. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
11. vpnconfig.ovpn dosyasını C:\Program Files\OpenVPN\config klasörüne kopyalayın.
12. Sistem tepsisindeki OpenVPN simgesine sağ tıklayın ve Bağlan’a tıklayın.

## <a name="mac"></a>Mac istemcileri

1. [Tunnelblick](https://tunnelblick.net/downloads.html)gibi bir OpenVPN istemcisini indirip yükleyin. 
2. Ağ geçidinin VPN profilini indirin. Bu, Azure portal Noktadan siteye Yapılandırma sekmesinden veya PowerShell 'de ' New-AzVpnClientConfiguration ' kullanılarak yapılabilir.
3. Profilin sıkıştırmasını açın. Vpnconfig. ovpn yapılandırma dosyasını Not defteri 'nde OpenVPN klasöründen açın.
4. P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Kodlanmış ortak anahtarı almak için bir sertifikanın nasıl dışarı aktarılacağı hakkında bilgi için bkz. [ortak anahtarı dışarı aktarma](vpn-gateway-certificates-point-to-site.md#cer) .
5. Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Özel bir anahtarı ayıklama hakkında bilgi için bkz. [özel anahtarınızı dışarı aktarma](https://openvpn.net/community-resources/how-to/#pki) .
6. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
7. Profili oluşturmak için profil dosyasına çift tıklayın Tunnelblick içinde profil oluşturun.
8. Uygulamalar klasöründen Tunnelblick başlatın.
9. Sistem tepsisindeki Tunnelblick simgesine tıklayın ve Bağlan ' ı seçin.

> [!IMPORTANT]
>OpenVPN protokolünde yalnızca iOS 11,0 ve üzeri ve MacOS 10,13 ve üzeri desteklenir.
>

## <a name="linux"></a>Linux istemcileri

1. Yeni bir terminal oturumu açın. Aynı anda ' Ctrl + Alt + t ' tuşlarına basarak yeni bir oturum açabilirsiniz.
2. Gerekli bileşenleri yüklemek için aşağıdaki komutu girin:

   ```
   sudo apt-get install openvpn
   sudo apt-get -y install network-manager-openvpn
   sudo service network-manager restart
   ```
3. Ağ geçidinin VPN profilini indirin. Bu, Azure portal Noktadan siteye Yapılandırma sekmesinden yapılabilir.
4. Oluşturduğunuz ve ağ geçidinde P2S yapılandırmanıza yüklediğiniz P2S istemci sertifikasını [dışarı aktarın](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-certificates-point-to-site#clientexport) . 
5. Özel anahtarı ve Base64 parmak izini. pfx dosyasından ayıklayın. Bunu yapmanın birden çok yolu vardır. Bilgisayarınızda OpenSSL kullanmak tek bir yoldur.

    ```
    openssl.exe pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
    ```
   *ProfileInfo. txt* dosyası, CA için özel anahtarı ve parmak Izini ve istemci sertifikasını içerir. İstemci sertifikasının parmak izini kullandığınızdan emin olun.

6. *ProfileInfo. txt* dosyasını bir metin düzenleyicisinde açın. İstemci (alt) sertifikasının parmak izini almak için, alt sertifika için "-----başlangıç SERTIFIKASı-----" ve "-----son SERTIFIKA-----" gibi metni seçin ve kopyalayın. Konu =/satırına bakarak alt sertifikayı belirleyebilirsiniz.

7. *Vpnconfig. ovpn* dosyasını açın ve aşağıda gösterilen bölümü bulun. Ve "CERT" ve "/CERT" arasındaki her şeyi değiştirin.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. ProfileInfo. txt dosyasını bir metin düzenleyicisinde açın. Özel anahtarı almak için, "-----başlangıç özel anahtarı-----" ve "-----son özel anahtar-----" dahil olmak üzere metni seçin ve kopyalayın.

9. Vpnconfig. ovpn dosyasını bir metin düzenleyicisinde açın ve bu bölümü bulun. Özel anahtarı, ve "Key" ve "/Key" arasındaki her şeyi değiştirerek yapıştırın.

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```

10. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
11. Komut satırını kullanarak bağlanmak için aşağıdaki komutu yazın:
  
    ```
    sudo openvpn –-config <name and path of your VPN profile file>&
    ```
12. GUI 'yi kullanarak bağlanmak için sistem ayarları ' na gidin.
13. Yeni bir VPN bağlantısı eklemek için **+** ' ye tıklayın.
14. **VPN Ekle**altında **Dosyadan içeri aktar ' ı seçin...**
15. Profil dosyasına gidin ve çift tıklayın veya **Aç**' ı seçin.
16. **VPN Ekle** penceresinde **Ekle** ' ye tıklayın.
  
    ![Dosyadan içe aktar](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
17. **Ağ ayarları** sayfasında veya sistem tepsisindeki ağ SIMGESI altında VPN **'yi** açarak bağlanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

VPN istemcilerinin başka bir sanal ağdaki kaynaklara erişebilmesini istiyorsanız, VNET 'ten VNET 'e bağlantı kurmak için [VNET-VNET](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) makalesindeki yönergeleri izleyin. Ağ geçitleri ve bağlantılarda BGP 'yi etkinleştirdiğinizden emin olun, aksi takdirde trafik akmaz.

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
