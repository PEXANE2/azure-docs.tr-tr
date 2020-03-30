---
title: Azure noktadan siteye bağlantı sorunlarını giderme
titleSuffix: Azure VPN Gateway
description: Sayfa-site bağlantı sorunlarını nasıl gidereceklerini öğrenin.
services: vpn-gateway
author: chadmath
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 03/26/2020
ms.author: genli
ms.openlocfilehash: 119f9c28b5413b8d2db5fa14ea839d1743f3d64a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297618"
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Sorun giderme: Azure noktadan siteye bağlantı sorunları

Bu makalede, karşılaşabileceğiniz ortak noktadan siteye bağlantı sorunları listeleneb.. Ayrıca, bu sorunların olası nedenleri ve çözümlerini de tartışır.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>VPN istemci hatası: Bir sertifika bulunamadı

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Bu Genişletilebilir Kimlik Doğrulama Protokolü ile kullanılabilecek bir sertifika bulunamadı. (Hata 798)**

### <a name="cause"></a>Nedeni

Bu sorun, istemci sertifikası Sertifikalar eksikse oluşur **- Geçerli Kullanıcı\Kişisel\Sertifikalar**.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için aşağıdaki adımları izleyin:

1. Sertifika Yöneticisini Aç: **Başlat'ı**tıklatın, **bilgisayar sertifikalarını yönet**yazın ve ardından arama sonucu bilgisayar **sertifikalarını yönet'i** tıklatın.

2. Aşağıdaki sertifikaların doğru konumda olduğundan emin olun:

    | Sertifika | Konum |
    | ------------- | ------------- |
    | AzureClient.pfx  | Geçerli Kullanıcı\Kişisel\Sertifikalar |
    | AzureRoot.cer    | Yerel Bilgisayar\Güvenilen Kök Sertifika Yetkilileri|

3. C:\Users\<UserName>\AppData\Roaming\Microsoft\Network\Connections\Cm\<GUID> adresine gidin, sertifikayı (*.cer dosyası) kullanıcıya ve bilgisayarın deposuna el ile yükleyin.

İstemci sertifikasının nasıl yüklenirhakkında daha fazla bilgi için, [sayfa-to-site bağlantıları için sertifika oluştur ve dışa aktarma](vpn-gateway-certificates-point-to-site.md)konusuna bakın.

> [!NOTE]
> İstemci sertifikasını içe aktarırken, **güçlü özel anahtar korumayı etkinleştir** seçeneğini seçmeyin.

## <a name="the-network-connection-between-your-computer-and-the-vpn-server-could-not-be-established-because-the-remote-server-is-not-responding"></a>Uzak sunucu yanıt vermiyor çünkü bilgisayarınız ve VPN sunucusu arasındaki ağ bağlantısı kurulamadı

### <a name="symptom"></a>Belirti

Windows'da IKEv2'yi kullanarak bir Azure sanal ağ ağ geçidine bağlanmayı denediğinizde aşağıdaki hata iletisini alırsınız:

**Uzak sunucu yanıt vermiyor çünkü bilgisayarınız ve VPN sunucusu arasındaki ağ bağlantısı kurulamadı**

### <a name="cause"></a>Nedeni
 
 Windows sürümü IKE parçalanma için destek yoksa sorun oluşur
 
### <a name="solution"></a>Çözüm

IKEv2, Windows 10 ve Server 2016’da desteklenir. Ancak IKEv2 kullanmak için güncelleştirmeleri yüklemeli ve yerel bir kayıt defteri anahtar değeri ayarlamalısınız. Windows 10’dan önceki işletim sistemleri desteklenmez ve yalnızca SSTP kullanabilir.

IKEv2 için Windows 10 ve Server 2016’yı hazırlamak için:

1. Güncelleştirmeyi yükleyin.

   | İşletim sistemi sürümü | Tarih | Sayı/Bağlantı |
   |---|---|---|---|
   | Windows Server 2016<br>Windows 10 Sürüm 1607 | 17 Ocak 2018 | [KB4057142](https://support.microsoft.com/help/4057142/windows-10-update-kb4057142) |
   | Windows 10 Sürüm 1703 | 17 Ocak 2018 | [KB4057144](https://support.microsoft.com/help/4057144/windows-10-update-kb4057144) |
   | Windows 10 Sürüm 1709 | 22 Mart 2018 | [KB4089848](https://www.catalog.update.microsoft.com/search.aspx?q=kb4089848) |
   |  |  |  |  |

2. Kayıt defteri anahtar değerini ayarlayın. Kayıt defterinde REG_DWORD anahtarı 1 olarak oluşturun veya ayarlayın. `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\ IKEv2\DisableCertReqPayload`

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a>VPN istemci hatası: Alınan ileti beklenmeyen veya kötü biçimlendirilmiş

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Alınan ileti beklenmeyen veya kötü biçimlendirilmişti. (Hata 0x80090326)**

### <a name="cause"></a>Nedeni

Aşağıdaki koşullardan biri doğruysa, bu sorun oluşur:

- Ağ Geçidi Alt Ağı'nda varsayılan rota içeren kullanıcı tanımlı yollar (UDR) yanlış ayarlanır.
- Kök sertifika ortak anahtarı Azure VPN ağ geçidine yüklenmez. 
- Anahtar bozuk veya süresi dolmuş.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için aşağıdaki adımları izleyin:

1. Ağ Geçidi Alt Ağı'ndaki UDR'yi kaldırın. UDR'nin tüm trafiği düzgün bir şekilde iledin.
2. İptal edilip edilmediğine bakmamak için Azure portalındaki kök sertifikanın durumunu denetleyin. İptal edilmezse, kök sertifikayı silmeyi ve yeniden yüklemeyi deneyin. Daha fazla bilgi için [bkz.](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts)

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>VPN istemci hatası: Bir sertifika zinciri işlendi ancak sonlandırıldı 

### <a name="symptom"></a>Belirti 

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Güven sağlayıcısı tarafından güvenilmeyen bir kök sertifikada işlenen ancak sonlandırılan bir sertifika zinciri.**

### <a name="solution"></a>Çözüm

1. Aşağıdaki sertifikaların doğru konumda olduğundan emin olun:

    | Sertifika | Konum |
    | ------------- | ------------- |
    | AzureClient.pfx  | Geçerli Kullanıcı\Kişisel\Sertifikalar |
    | Azureağ Geçidi-*GUID*.cloudapp.net  | Geçerli Kullanıcı\Güvenilir Kök Sertifika Yetkilileri|
    | AzureAğ Geçidi-*GUID*.cloudapp.net, AzureRoot.cer    | Yerel Bilgisayar\Güvenilen Kök Sertifika Yetkilileri|

2. Sertifikalar zaten konumdaysa, sertifikaları silmeyi ve yeniden yüklemeyi deneyin. **Azuregateway-*GUID*.cloudapp.net** sertifikası, Azure portalından indirdiğiniz VPN istemci yapılandırma paketinde yer alan bir sertifikadır. Dosyaları paketten ayıklamak için dosya arşivleyicilerini kullanabilirsiniz.

## <a name="file-download-error-target-uri-is-not-specified"></a>Dosya indirme hatası: Hedef URI belirtilmemiş

### <a name="symptom"></a>Belirti

Aşağıdaki hata iletisini alırsınız:

**Dosya indirme hatası. Hedef URI belirtilmemiştir.**

### <a name="cause"></a>Nedeni 

Bu sorun, yanlış bir ağ geçidi türü nedeniyle oluşur. 

### <a name="solution"></a>Çözüm

VPN ağ geçidi türü **VPN**olmalıdır ve VPN türü **RouteBased**olmalıdır.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>VPN istemci hatası: Azure VPN özel komut dosyası başarısız oldu 

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Özel komut dosyası (yönlendirme tablonuzu güncelleştirmek için) başarısız oldu. (Hata 8007026f)**

### <a name="cause"></a>Nedeni

Siteden noktaya VPN bağlantısını kısayol kullanarak açmaya çalışıyorsanız, bu sorun oluşabilir.

### <a name="solution"></a>Çözüm 

VPN paketini kısayoldan açmak yerine doğrudan açın.

## <a name="cannot-install-the-vpn-client"></a>VPN istemcisini yükleyemiyorum

### <a name="cause"></a>Nedeni 

Sanal ağınız için VPN ağ geçidine güvenmek için ek bir sertifika gerekir. Sertifika, Azure portalından oluşturulan VPN istemci yapılandırma paketine dahildir.

### <a name="solution"></a>Çözüm

VPN istemci yapılandırma paketini ayıklayın ve .cer dosyasını bulun. Sertifikayı yüklemek için aşağıdaki adımları izleyin:

1. Açık mmc.exe.
2. **Sertifikaları** snap-in ekleyin.
3. Yerel bilgisayar için **Bilgisayar** hesabını seçin.
4. **Güvenilen Kök Sertifika Syon Yetkilileri** düğümüne sağ tıklayın. **Tüm Görev** > **İçe Aktar'ı**tıklatın ve VPN istemci yapılandırma paketinden çıkardığınız .cer dosyasına göz atın.
5. Bilgisayarı yeniden başlatın. 
6. VPN istemcisini yüklemeyi deneyin.

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a>Azure portalı hatası: VPN ağ geçidini kaydedemeyen veriler geçersiz

### <a name="symptom"></a>Belirti

Değişiklikleri Azure portalındaki VPN ağ geçidine kaydetmeye çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Sanal ağ ağ &lt; *geçidi adını*&gt;kaydetmek için başarısız oldu. Sertifika &lt; *kimlik kimliği* &gt; için veriler geçersizdir.**

### <a name="cause"></a>Nedeni 

Yüklediğiniz kök sertifika ortak anahtarı boşluk gibi geçersiz bir karakter içeriyorsa, bu sorun oluşabilir.

### <a name="solution"></a>Çözüm

Sertifikadaki verilerin satır sonu (satır döndürmeleri) gibi geçersiz karakterler içermediğinden emin olun. Tüm değer uzun bir çizgi olmalıdır. Aşağıdaki metin sertifikanın bir örneğidir:

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a>Azure portalı hatası: VPN ağ geçidini kaydedemeyen kaynak adı geçersiz

### <a name="symptom"></a>Belirti

Değişiklikleri Azure portalındaki VPN ağ geçidine kaydetmeye çalıştığınızda aşağıdaki hata iletisini alırsınız: 

**Sanal ağ ağ &lt; *geçidi adını*&gt;kaydetmek için başarısız oldu. &gt; *Yüklemeye çalıştığınız* kaynak &lt;adı sertifikası adı geçersizdir.**

### <a name="cause"></a>Nedeni

Bu sorun, sertifikanın adı boşluk gibi geçersiz bir karakter içerdiğinden oluşur. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Azure portal hatası: VPN paketi dosya indirme hatası 503

### <a name="symptom"></a>Belirti

VPN istemci yapılandırma paketini indirmeye çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Dosyayı karşıdan yükleyemedi. Hata ayrıntıları: hata 503. Sunucu meşgul.**
 
### <a name="solution"></a>Çözüm

Bu hata geçici bir ağ sorunu neden olabilir. Birkaç dakika sonra VPN paketini tekrar indirmeyi deneyin.

## <a name="azure-vpn-gateway-upgrade-all-point-to-site-clients-are-unable-to-connect"></a>Azure VPN Ağ Geçidi yükseltmesi: All Point to Site istemcileri bağlanamıyor

### <a name="cause"></a>Nedeni

Sertifika ömrü boyunca yüzde 50'den fazlaysa, sertifika devredilir.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için, Point to Site paketini tüm istemcilere yeniden indirin ve yeniden dağıtın.

## <a name="too-many-vpn-clients-connected-at-once"></a>Aynı anda çok fazla VPN istemcisi bağlandı

İzin verilen maksimum bağlantı sayısına ulaşılır. Azure portalında toplam bağlı istemci sayısını görebilirsiniz.

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN istemcisi ağ dosyası paylaşımları erişemez

### <a name="symptom"></a>Belirti

VPN istemcisi Azure sanal ağına bağlanmıştır. Ancak, istemci ağ paylaşımları erişemez.

### <a name="cause"></a>Nedeni

SMB protokolü dosya paylaşımı erişimi için kullanılır. Bağlantı başlatıldığında, VPN istemcisi oturum kimlik bilgilerini ekler ve hata oluşur. Bağlantı kurulduktan sonra, istemci Kerberos kimlik doğrulaması için önbellek kimlik bilgilerini kullanmak zorunda kalır. Bu işlem, bir belirteç almak için Anahtar Dağıtım Merkezi (etki alanı denetleyicisi) sorguları başlatır. İstemci Internet'ten bağlandığından, etki alanı denetleyicisine ulaşamayabilir. Bu nedenle, istemci Kerberos'tan NTLM'ye kadar başarısız olamaz. 

İstemciden bir kimlik bilgisi için istendiği tek zaman, birleştiği etki alanı tarafından verilen geçerli bir sertifikaya (SAN=UPN ile) sahip olmasıdır. İstemci ayrıca etki alanı ağına fiziksel olarak bağlı olmalıdır. Bu durumda, istemci sertifikayı kullanmaya çalışır ve etki alanı denetleyicisine ulaşır. Ardından Anahtar Dağıtım Merkezi bir "KDC_ERR_C_PRINCIPAL_UNKNOWN" hatası döndürür. İstemci NTLM'ye başarısız olmak zorunda kalır. 

### <a name="solution"></a>Çözüm

Sorunu aşmak için, aşağıdaki kayıt defteri alt anahtarından etki alanı kimlik bilgilerinin önbelleğe alınışını devre dışı kılabilir: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a>VPN istemcisini yeniden yükledikten sonra Windows'da noktadan siteye VPN bağlantısını bulamıyor

### <a name="symptom"></a>Belirti

Noktadan siteye VPN bağlantısını kaldırın ve ardından VPN istemcisini yeniden yüklersiniz. Bu durumda, VPN bağlantısı başarıyla yapılandırılmamıştır. Windows'daki **Ağ bağlantıları** ayarlarında VPN bağlantısını göremezsiniz.

### <a name="solution"></a>Çözüm

Sorunu çözmek için eski VPN istemci yapılandırma dosyalarını **C:\Users\UserName\AppData\Roaming\Microsoft\Network\Connections\<VirtualNetworkId>** ve ardından VPN istemcisi yükleyicisini yeniden çalıştırın.

## <a name="point-to-site-vpn-client-cannot-resolve-the-fqdn-of-the-resources-in-the-local-domain"></a>Noktadan siteye VPN istemcisi yerel etki alanındaki kaynakların FQDN'sini çözemez

### <a name="symptom"></a>Belirti

İstemci, noktaya sayfa VPN bağlantısını kullanarak Azure'a bağlandığında, yerel etki alanınızdaki kaynakların FQDN'sini çözemez.

### <a name="cause"></a>Nedeni

Noktadan siteye VPN istemcisi, Azure sanal ağında yapılandırılan Azure DNS sunucularını kullanır. Azure DNS sunucuları istemcide yapılandırılan yerel DNS sunucularından önceliklidir, böylece tüm DNS sorguları Azure DNS sunucularına gönderilir. Azure DNS sunucularında yerel kaynakların kayıtları yoksa, sorgu başarısız olur.

### <a name="solution"></a>Çözüm

Sorunu çözmek için, Azure sanal ağında kullanılan Azure DNS sunucularının yerel kaynaklar için DNS kayıtlarını çözediğinden emin olun. Bunu yapmak için DNS Iletmecileri veya Koşullu iletmecileri kullanabilirsiniz. Daha fazla bilgi için [kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) bakın

## <a name="the-point-to-site-vpn-connection-is-established-but-you-still-cannot-connect-to-azure-resources"></a>Noktadan noktaya VPN bağlantısı kuruldu, ancak yine de Azure kaynaklarına bağlanamıyorsunuz 

### <a name="cause"></a>Nedeni

VPN istemcisi yolları Azure VPN ağ geçidinden alamazsa bu sorun oluşabilir.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için [Azure VPN ağ geçidini sıfırla.](vpn-gateway-resetgw-classic.md) Yeni yolların kullanıldığından emin olmak için, sanal ağ eşleme başarılı bir şekilde yapılandırıldıktan sonra Site'ye Nokta VPN istemcilerinin yeniden indirilmesi gerekir.

## <a name="error-the-revocation-function-was-unable-to-check-revocation-because-the-revocation-server-was-offlineerror-0x80092013"></a>Hata: "İptal işlevi iptal sunucusu çevrimdışı olduğu için iptali denetleyemedi. (Hata 0x80092013)"

### <a name="causes"></a>Nedenler
Bu hata iletisi istemci http://crl3.digicert.com/ssca-sha2-g1.crl erişemiyorsa oluşur ve http://crl4.digicert.com/ssca-sha2-g1.crl.  İptal denetimi bu iki siteye erişim gerektirir.  Bu sorun genellikle proxy sunucusu yapılandırılan istemcide olur. Bazı ortamlarda, istekler proxy sunucusundan geçmiyorsa, Edge Güvenlik Duvarı'nda reddedilir.

### <a name="solution"></a>Çözüm

Proxy sunucusu ayarlarını kontrol edin, http://crl3.digicert.com/ssca-sha2-g1.crl istemcinin erişebileceğinden emin olun ve. http://crl4.digicert.com/ssca-sha2-g1.crl

## <a name="vpn-client-error-the-connection-was-prevented-because-of-a-policy-configured-on-your-rasvpn-server-error-812"></a>VPN İstemci Hatası: Bağlantı, RAS/VPN sunucunuzda yapılandırılan bir ilke nedeniyle engellendi. (Hata 812)

### <a name="cause"></a>Nedeni

Vpn istemcisinin kimliğini doğrulamak için kullandığınız RADIUS sunucusunda yanlış ayarlar varsa veya Azure Ağ Geçidi Radius sunucusuna erişemezse bu hata oluşur.

### <a name="solution"></a>Çözüm

RADIUS sunucusunun doğru şekilde yapılandırıldığından emin olun. Daha fazla bilgi için [bkz.](../active-directory/authentication/howto-mfaserver-dir-radius.md)

## <a name="error-405-when-you-download-root-certificate-from-vpn-gateway"></a>VPN Ağ Geçidi'nden kök sertifika indirirken "Hata 405"

### <a name="cause"></a>Nedeni

Kök sertifikası yüklenmedi. Kök sertifika istemcinin **Güvenilen sertifikadeposuna** yüklenir.

## <a name="vpn-client-error-the-remote-connection-was-not-made-because-the-attempted-vpn-tunnels-failed-error-800"></a>VPN İstemci Hatası: Vpn denemesi tünelleri başarısız olduğu için uzak bağlantı yapılmadı. (Hata 800) 

### <a name="cause"></a>Nedeni

NIC sürücüsünün modası geçmiş.

### <a name="solution"></a>Çözüm

NIC sürücüsünü güncelleştirin:

1. **Başlat'ı**tıklatın, **Aygıt Yöneticisi**yazın ve sonuçlar listesinden seçin. Yönetici parolası veya onayı istenirse, parolayı yazın veya onay sağlayın.
2. Ağ **bağdaştırıcıları** kategorilerinde, güncelleştirmek istediğiniz NIC'yi bulun.  
3. Aygıt adını çift tıklatın, **Sürücüyü Güncelleştir'i**seçin, **güncelleştirilmiş sürücü yazılımı için otomatik olarak Ara'yı**seçin.
4.  Windows yeni bir sürücü bulmazsa, cihaz üreticisinin Web sitesinde bir sürücü arayıp yükleme yönergelerini izleyebilirsiniz.
5. Bilgisayarı yeniden başlatın ve bağlantıyı yeniden deneyin.

## <a name="vpn-client-error-dialing-vpn-connection-vpn-connection-name-status--vpn-platform-did-not-trigger-connection"></a>VPN İstemci Hatası: <VPN Connection Name>ARAMA VPN bağlantısı , Durum = VPN Platformu bağlantı tetiklemedi

Ayrıca RasClient'dan Olay Görüntüleyicisi'nde aşağıdaki <User> hatayı görebilirsiniz: <VPN Connection Name> "Kullanıcı başarısız olan bir bağlantıyı aradı. Hata üzerinde döndürülen hata kodu 1460'tır."

### <a name="cause"></a>Nedeni

Azure VPN İstemcisi, Windows için Uygulama Ayarları'nda etkinleştirilen "Arka Plan uygulamaları" Uygulama İzni'ne sahip değildir.

### <a name="solution"></a>Çözüm

1. Windows'da Ayarlar -> Gizlilik -> Arka Plan uygulamalarına gidin
2. "Uygulamaların arka planda çalışmasına izin ver"

## <a name="error-file-download-error-target-uri-is-not-specified"></a>Hata: 'Dosya indirme hatası Hedef URI belirtilmemiş'

### <a name="cause"></a>Nedeni

Bunun nedeni yanlış bir ağ geçidi türü yapılandırılır.

### <a name="solution"></a>Çözüm

Azure VPN ağ geçidi türü VPN olmalı ve VPN türü **RouteBased**olmalıdır.

## <a name="vpn-package-installer-doesnt-complete"></a>VPN paket yükleyicisi tamamlanmadı

### <a name="cause"></a>Nedeni

Bu sorun, önceki VPN istemci yüklemeleri neden olabilir. 

### <a name="solution"></a>Çözüm

**C:\Users\UserName\AppData\Roaming\Microsoft\Network\Connections\<VirtualNetworkId>'den** eski VPN istemci yapılandırma dosyalarını silin ve VPN istemci yükleyicisini yeniden çalıştırın. 

## <a name="the-vpn-client-hibernates-or-sleep-after-some-time"></a>VPN istemcisi bir süre sonra hazırda beklemede veya uyku

### <a name="solution"></a>Çözüm

VPN istemcisinin üzerinde çalıştığını bilgisayarda uyku ve hazırda bekleme ayarlarını denetleyin.
