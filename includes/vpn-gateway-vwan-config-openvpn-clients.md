---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/17/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 921e22e637782ffd744af1a28e6bd43e7dd53c67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066057"
---
## <a name="windows-clients"></a><a name="windows"></a>Windows istemcileri

1. OpenVPN istemcisini (sürüm 2.4 veya üzeri) resmi [OpenVPN web sitesinden](https://openvpn.net/index.php/open-source/downloads.html)indirin ve kurun.
2. Ağ geçidinin VPN profilini indirin. Bu, Azure portalındaki Noktaya Noktaya yapılandırma sekmesinden veya PowerShell'deki 'New-AzVpnClientConfiguration' sekmesinden yapılabilir.
3. Profilin sıkıştırmasını açın. Ardından, Notepad'i kullanarak OpenVPN klasöründen *vpnconfig.ovpn* yapılandırma dosyasını açın.
4. Oluşturduğunuz ve yüklediğiniz noktadan siteye istemci sertifikasını ağ geçidindeki P2S yapılandırmanıza dışa aktarın. Aşağıdaki makale bağlantılarını kullanın:

   * [VPN Ağ Geçidi](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport) talimatları
   
   * [Sanal WAN](../articles/virtual-wan/certificates-point-to-site.md#clientexport) talimatları
5. Özel anahtarı ve base64 parmak izini *.pfx'ten*çıkar. Bunu yapmanın birden çok yolu vardır. OpenSSL'yi makinenizde kullanmak tek yönlüdür. *profileinfo.txt* dosyası, CA ve İstemci sertifikasıiçin özel anahtar ve parmak izi içerir. İstemci sertifikasının parmak izini kullandığınızdan emin olun.

   ```
   openssl pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
   ```
6. Notepad'de *profileinfo.txt'yi* açın. İstemci (alt) sertifikasının parmak izini almak için, çocuk sertifikası için "-----BEGIN CERTIFICATE-----" ve "-----END CERTIFICATE-----" metinlerini (dahil ve arasında) seçin ve kopyalayın. Alt sertifikayı özneye bakarak tanımlayabilirsiniz=/ satırı.
7. Adım 3'ten Notepad'de açtığınız *vpnconfig.ovpn* dosyasına geçin. Aşağıda gösterilen bölümü bulun ve "cert" ve "/cert" arasındaki her şeyi değiştirin.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Notepad'de *profileinfo.txt'yi* açın. Özel anahtarı almak için "-----BEGIN PRIVATE KEY-----" ve "-----END PRIVATE KEY-----" metinlerini seçin ve kopyalayın.
9. Notepad vpnconfig.ovpn dosyasına geri dön ve bu bölümü bulmak. "Anahtar" ve "/key" arasındaki her şeyi değiştirerek özel anahtarı yapıştırın.

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

## <a name="mac-clients"></a><a name="mac"></a>Mac istemcileri

1. TunnelBlick gibi bir OpenVPN istemcisini indirin ve [kurun.](https://tunnelblick.net/downloads.html) 
2. Ağ geçidinin VPN profilini indirin. Bu, Azure portalındaki noktaya noktaya yapılandırma sekmesinden veya PowerShell'de 'Yeni-AzVpnClientConfiguration' kullanılarak yapılabilir.
3. Profilin sıkıştırmasını açın. Bir metin düzenleyicisinde OpenVPN klasöründen vpnconfig.ovpn yapılandırma dosyasını açın.
4. P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Kodlanmış ortak anahtarı almak için sertifikanın nasıl dışa aktarılacak nasıl dışa aktarılacaklarına ilişkin bilgi için aşağıdaki makale bağlantılarını kullanın:

   * [VPN Ağ Geçidi](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#cer) talimatları 
   
   * [Sanal WAN](../articles/virtual-wan/certificates-point-to-site.md#cer) talimatları
5. Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Özel bir anahtarı nasıl ayıklayabilirsiniz hakkında bilgi almak için OpenVPN [sitesindeki özel anahtarınızı dışa](https://openvpn.net/community-resources/how-to/#pki) aktar'a bakın.
6. Başka bir alanı değiştirmeyin. VPN’e bağlanmak için istemci girişinde doldurulmuş yapılandırmayı kullanın.
7. Tunnelblick'teki profili oluşturmak için profil dosyasını çift tıklatın.
8. Uygulamalar klasöründen Tunnelblick'i başlatın.
9. Sistem tepsisindeki Tunnelblick simgesine tıklayın ve bağlan'ı seçin.

> [!IMPORTANT]
>OpenVPN protokolü ile yalnızca iOS 11.0 ve üzeri macos 10.13 ve üzeri desteklenir.
>
## <a name="ios-clients"></a><a name="iOS"></a>iOS istemcileri

1. OpenVPN istemcisini (sürüm 2.4 veya üzeri) App store'dan yükleyin.
2. Ağ geçidinin VPN profilini indirin. Bu, Azure portalındaki noktaya noktaya yapılandırma sekmesinden veya PowerShell'de 'Yeni-AzVpnClientConfiguration' kullanılarak yapılabilir.
3. Profilin sıkıştırmasını açın. Bir metin düzenleyicisinde OpenVPN klasöründen vpnconfig.ovpn yapılandırma dosyasını açın.
4. P2S istemci sertifikası bölümünü base64’teki P2S istemci sertifikası genel anahtarı ile doldurun. PEM biçimli bir sertifikada .cer dosyasını açıp base64 anahtarını sertifika üst bilgileri arasına kopyalamanız yeterlidir. Kodlanmış ortak anahtarı almak için sertifikanın nasıl dışa aktarılacak nasıl dışa aktarılacaklarına ilişkin bilgi için aşağıdaki makale bağlantılarını kullanın:

   * [VPN Ağ Geçidi](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#cer) talimatları 
   
   * [Sanal WAN](../articles/virtual-wan/certificates-point-to-site.md#cer) talimatları
5. Özel anahtar bölümünü, base64’teki P2S istemci sertifikası özel anahtarı ile doldurun. Özel bir anahtarı niçin ayıklayılacağı hakkında bilgi almak için [OpenVPN sitesindeki özel anahtarınızı dışa](https://openvpn.net/community-resources/how-to/#pki) aktar'a bakın.
6. Başka bir alanı değiştirmeyin.
7. Profil dosyasını (.ovpn) iPhone'unuzdaki posta uygulamasında yapılandırılan e-posta hesabınıza e-posta ile gönderin. 
8. iPhone'daki posta uygulamasındaki e-postayı açın ve ekli dosyaya dokunun

    ![E-postayı aç](./media/vpn-gateway-vwan-config-openvpn-clients/ios2.png)

9. **OpenVPN'e Kopyala** seçeneğini görmüyorsanız **Daha Fazla'ya** dokunun

    ![Daha fazla](./media/vpn-gateway-vwan-config-openvpn-clients/ios3.png)

10. **OpenVPN için Kopyala'ya** dokunun 

    ![OpenVPN'e Kopyala](./media/vpn-gateway-vwan-config-openvpn-clients/ios4.png)

11. **İçe Alma Profili** sayfasında **ADD'ye** dokunun

    ![Ekle](./media/vpn-gateway-vwan-config-openvpn-clients/ios5.png)

12. **İçe Aktarılan Profil** sayfasında **ADD'ye** dokunun

    ![EKLE'ye dokunun](./media/vpn-gateway-vwan-config-openvpn-clients/ios6.png)

13. OpenVPN uygulamasını başlatın ve bağlanmak için **profili profil** sayfasında kaydırın

    ![Bağlan](./media/vpn-gateway-vwan-config-openvpn-clients/ios8.png)


## <a name="linux-clients"></a><a name="linux"></a>Linux istemcileri

1. Yeni bir Terminal oturumu açın. Aynı anda 'Ctrl + Alt + t' tuşuna basarak yeni bir oturum açabilirsiniz.
2. Gerekli bileşenleri yüklemek için aşağıdaki komutu girin:

   ```
   sudo apt-get install openvpn
   sudo apt-get -y install network-manager-openvpn
   sudo service network-manager restart
   ```
3. Ağ geçidinin VPN profilini indirin. Bu, Azure portalındaki Noktaya Noktaya yapılandırma sekmesinden yapılabilir.
4. Oluşturduğunuz ve yüklediğiniz P2S istemci sertifikasını ağ geçidindeki P2S yapılandırmanıza dışa aktarın. Aşağıdaki makale bağlantılarını kullanın:

   * [VPN Ağ Geçidi](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport) talimatları 
   
   * [Sanal WAN](../articles/virtual-wan/certificates-point-to-site.md#clientexport) talimatları
5. Özel anahtarı ve base64 parmak izini .pfx'ten çıkar. Bunu yapmanın birden çok yolu vardır. Bilgisayarınızda OpenSSL kullanmak tek yönlüdür.

    ```
    openssl.exe pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
    ```
   *profileinfo.txt* dosyası, CA'nın özel anahtarını ve parmak izini ve İstemci sertifikasını içerir. İstemci sertifikasının parmak izini kullandığınızdan emin olun.

6. Bir metin düzenleyicisinde *profileinfo.txt'yi* açın. İstemci (alt) sertifikasının parmak izini almak için, çocuk sertifikası için "-----BEGIN CERTIFICATE-----" ve "-----END CERTIFICATE-----" arasındaki metni seçin ve kopyalayın. Alt sertifikayı özneye bakarak tanımlayabilirsiniz=/ satırı.

7. *Vpnconfig.ovpn* dosyasını açın ve aşağıda gösterilen bölümü bulun. "Cert" ve "/cert" arasındaki her şeyi değiştirin.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Bir metin düzenleyicisinde profileinfo.txt'yi açın. Özel anahtarı almak için "-----BEGIN PRIVATE KEY-----" ve "-----END PRIVATE KEY-----" arasındaki metni seçin ve kopyalayın.

9. Vpnconfig.ovpn dosyasını bir metin düzenleyicisinde açın ve bu bölümü bulun. "Anahtar" ve "/key" arasındaki her şeyi değiştirerek özel anahtarı yapıştırın.

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
12. GUI kullanarak bağlanmak için sistem ayarlarına gidin.
13. Yeni **+** bir VPN bağlantısı eklemek için tıklatın.
14. **VPN Ekle**altında, **dosyadan Içe Aktar'ı seçin...**
15. Profil dosyasına göz atın ve çift tıklayın veya **Aç'ı**seçin.
16. **VPN Ekle** penceresine **ekle'yi** tıklatın.
  
    ![Dosyadan içeri aktar](./media/vpn-gateway-vwan-config-openvpn-clients/import.png)
17. **Ağ Ayarları** sayfasındaki **VPN'i açarak** veya sistem tepsisindeki ağ simgesinin altında bağlanabilirsiniz.