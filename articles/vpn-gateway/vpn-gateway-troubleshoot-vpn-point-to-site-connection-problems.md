---
title: Azure Noktadan siteye bağlantı sorunlarını giderme | Microsoft Docs
description: Noktadan siteye bağlantı sorunlarını nasıl giderebileceğinizi öğrenin.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: dcscontentpm
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: genli
ms.openlocfilehash: 37c2cf5ffb5e6eaf8b8da6e7bc9259cfa101c796
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71058816"
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Sorunu Azure Noktadan siteye bağlantı sorunları

Bu makalede karşılaşabileceğiniz ortak Noktadan siteye bağlantı sorunları listelenmektedir. Ayrıca, bu sorunlara yönelik olası nedenler ve çözümler açıklanmaktadır.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>VPN istemci hatası: Sertifika bulunamadı

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Bu Genişletilebilir kimlik doğrulama protokolüyle kullanılabilecek bir sertifika bulunamadı. (Hata 798)**

### <a name="cause"></a>Nedeni

Bu sorun, istemci sertifikası sertifikalarda yoksa oluşur **-geçerli User\personal \ Certificates**.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için şu adımları izleyin:

1. Sertifika Yöneticisi 'Ni açın: **Başlat**' a tıklayın, **bilgisayar sertifikalarını Yönet**yazın ve ardından arama sonucunda **bilgisayar sertifikalarını Yönet** ' e tıklayın.

2. Aşağıdaki sertifikaların doğru konumda olduğundan emin olun:

    | Sertifika | Location |
    | ------------- | ------------- |
    | AzureClient.pfx  | Geçerli Kullanıcı \ kişisel \ sertifikalar |
    | Azuregateway-*GUID*. cloudapp.net  | Geçerli Kullanıcı \ güvenilen kök sertifika yetkilileri|
    | AzureGateway-*GUID*. cloudapp.net, AzureRoot. cer    | Yerel bilgisayar \ güvenilen kök sertifika yetkilileri|

3. C:\Users\<Kullanıcı adı > \AppData\Roaming\Microsoft\Network\Connections\Cm\<GUID > adresine gidin ve sertifikayı (*. cer dosyası) Kullanıcı ve bilgisayar deposuna el ile yükleyebilirsiniz.

İstemci sertifikasını nasıl yükleyeceğiniz hakkında daha fazla bilgi için bkz. [Noktadan siteye bağlantılar için sertifikaları oluşturma ve dışarı aktarma](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> İstemci sertifikasını içeri aktardığınızda, **güçlü özel anahtar korumasını etkinleştir** seçeneğini seçmeyin.

## <a name="the-network-connection-between-your-computer-and-the-vpn-server-could-not-be-established-because-the-remote-server-is-not-responding"></a>Uzak sunucu yanıt vermediği için bilgisayarınızla VPN sunucusu arasındaki ağ bağlantısı kurulamadı

### <a name="symptom"></a>Belirti

Windows üzerinde Ikev2 kullanarak bir Azure sanal ağ geçidine çalıştığınızda ve bu ağ geçidine bağlandığınızda aşağıdaki hata iletisini alırsınız:

**Uzak sunucu yanıt vermediği için bilgisayarınızla VPN sunucusu arasındaki ağ bağlantısı kurulamadı**

### <a name="cause"></a>Nedeni
 
 Bu sorun, Windows sürümünün ıKE parçalanması desteği yoksa oluşur
 
### <a name="solution"></a>Çözüm

IKEv2, Windows 10 ve Server 2016’da desteklenir. Ancak IKEv2 kullanmak için güncelleştirmeleri yüklemeli ve yerel bir kayıt defteri anahtar değeri ayarlamalısınız. Windows 10’dan önceki işletim sistemleri desteklenmez ve yalnızca SSTP kullanabilir.

IKEv2 için Windows 10 ve Server 2016’yı hazırlamak için:

1. Güncelleştirmeyi yükleyin.

   | İşletim sistemi sürümü | Date | Sayı/Bağlantı |
   |---|---|---|---|
   | Windows Server 2016<br>Windows 10 Sürüm 1607 | 17 Ocak 2018 | [KB4057142](https://support.microsoft.com/help/4057142/windows-10-update-kb4057142) |
   | Windows 10 Sürüm 1703 | 17 Ocak 2018 | [KB4057144](https://support.microsoft.com/help/4057144/windows-10-update-kb4057144) |
   | Windows 10 sürüm 1709 | 22 Mart 2018 | [KB4089848](https://www.catalog.update.microsoft.com/search.aspx?q=kb4089848) |
   |  |  |  |  |

2. Kayıt defteri anahtar değerini ayarlayın. Kayıt defterinde REG_DWORD `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\ IKEv2\DisableCertReqPayload` anahtarını 1 olarak oluşturun veya ayarlayın.

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a>VPN istemci hatası: Alınan ileti beklenmeyen veya hatalı biçimlendirildi

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Alınan ileti beklenmeyen veya hatalı biçimlendirildi. (Hata 0x80090326)**

### <a name="cause"></a>Nedeni

Aşağıdaki koşullardan biri doğru ise bu sorun oluşur:

- Ağ geçidi alt ağında Kullanıcı tanımlı yollar (UDR) Kullan varsayılan yolu yanlış ayarlanır.
- Kök sertifika ortak anahtarı Azure VPN ağ geçidine yüklenmedi. 
- Anahtar bozuk veya zaman aşımına uğradı.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için şu adımları izleyin:

1. Ağ geçidi alt ağında UDR 'yi kaldırın. UDR 'nin tüm trafiği doğru bir şekilde iletdiğinizden emin olun.
2. İptal edilip edilmediğini görmek için Azure portal kök sertifikanın durumunu denetleyin. İptal edilmediği takdirde, kök sertifikayı silip yeniden karşıya yüklemeyi deneyin. Daha fazla bilgi için bkz. [sertifika oluşturma](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>VPN istemci hatası: Bir sertifika zinciri işlendi ancak sonlandırıldı 

### <a name="symptom"></a>Belirti 

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Bir sertifika zinciri işlense de, güven sağlayıcısı tarafından güvenilmeyen bir kök sertifikada sonlandırıldı.**

### <a name="solution"></a>Çözüm

1. Aşağıdaki sertifikaların doğru konumda olduğundan emin olun:

    | Sertifika | Location |
    | ------------- | ------------- |
    | AzureClient.pfx  | Geçerli Kullanıcı \ kişisel \ sertifikalar |
    | Azuregateway-*GUID*. cloudapp.net  | Geçerli Kullanıcı \ güvenilen kök sertifika yetkilileri|
    | AzureGateway-*GUID*. cloudapp.net, AzureRoot. cer    | Yerel bilgisayar \ güvenilen kök sertifika yetkilileri|

2. Sertifikalar zaten konumda varsa, sertifikaları silip yeniden yüklemeyi deneyin. **Azuregateway-*GUID*. cloudapp.net** SERTIFIKASı, Azure Portal indirdiğiniz VPN istemcisi yapılandırma paketidir. Dosyaları paketten ayıklamak için, Archivers dosyasını kullanabilirsiniz.

## <a name="file-download-error-target-uri-is-not-specified"></a>Dosya indirme hatası: Hedef URI belirtilmedi

### <a name="symptom"></a>Belirti

Aşağıdaki hata iletisini alırsınız:

**Dosya indirme hatası. Hedef URI belirtilmedi.**

### <a name="cause"></a>Nedeni 

Bu sorun, yanlış bir ağ geçidi türü nedeniyle oluşur. 

### <a name="solution"></a>Çözüm

VPN ağ geçidi türü **VPN**OLMALıDıR ve VPN türü **routebased**olmalıdır.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>VPN istemci hatası: Azure VPN özel betiği başarısız oldu 

### <a name="symptom"></a>Belirti

VPN istemcisini kullanarak bir Azure sanal ağına bağlanmaya çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Özel betik (yönlendirme tablonuzu güncelleştirmek için) başarısız oldu. (Hata 8007026f)**

### <a name="cause"></a>Nedeni

Bu sorun, bir kısayolu kullanarak siteden noktaya VPN bağlantısını açmaya çalıştığınızda meydana gelebilir.

### <a name="solution"></a>Çözüm 

Doğrudan VPN paketini kısayol 'tan açmak yerine açın.

## <a name="cannot-install-the-vpn-client"></a>VPN istemcisi yüklenemiyor

### <a name="cause"></a>Nedeni 

Sanal ağınız için VPN ağ geçidine güvenmek üzere ek bir sertifika gerekir. Sertifika, Azure portal oluşturulan VPN istemcisi yapılandırma paketine dahildir.

### <a name="solution"></a>Çözüm

VPN istemcisi yapılandırma paketini ayıklayın ve. cer dosyasını bulun. Sertifikayı yüklemek için şu adımları izleyin:

1. MMC. exe ' yi açın.
2. **Sertifikalar** ek bileşenini ekleyin.
3. Yerel bilgisayar için **bilgisayar** hesabını seçin.
4. **Güvenilen kök sertifika yetkilileri** düğümüne sağ tıklayın. **Tümü-görev** > **içeri aktar**' a tıklayın ve VPN istemcisi yapılandırma paketinden ayıkladığınız. cer dosyasına gidin.
5. Bilgisayarı yeniden başlatın. 
6. VPN istemcisini yüklemeyi deneyin.

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a>Azure portal hatası: VPN ağ geçidi kaydedilemedi ve veriler geçersiz

### <a name="symptom"></a>Belirti

VPN ağ geçidi değişikliklerini Azure portal kaydetmeye çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Sanal ağ geçidi &lt; *ağ geçidi adı*&gt;kaydedilemedi. Sertifika &lt; *sertifikası* kimliği&gt; için veriler geçersiz.**

### <a name="cause"></a>Nedeni 

Karşıya yüklediğiniz kök sertifika ortak anahtarı, boşluk gibi geçersiz bir karakter içeriyorsa bu sorun oluşabilir.

### <a name="solution"></a>Çözüm

Sertifikadaki verilerin satır sonu (satır başı) gibi geçersiz karakterler içermediğinden emin olun. Değerin tamamı bir uzun çizgi olmalıdır. Aşağıdaki metin, sertifikanın bir örneğidir:

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

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a>Azure portal hatası: VPN ağ geçidi kaydedilemedi ve kaynak adı geçersiz

### <a name="symptom"></a>Belirti

VPN ağ geçidi değişikliklerini Azure portal kaydetmeye çalıştığınızda aşağıdaki hata iletisini alırsınız: 

**Sanal ağ geçidi &lt; *ağ geçidi adı*&gt;kaydedilemedi. Karşıya &lt;** *yüklemeye çalıştığınız kaynak adı sertifikası adı* geçersiz.&gt;

### <a name="cause"></a>Nedeni

Bu sorun, sertifikanın adı boşluk gibi geçersiz bir karakter içerdiği için oluşur. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Azure portal hatası: VPN paketi dosyası indirme hatası 503

### <a name="symptom"></a>Belirti

VPN istemcisi yapılandırma paketini indirmeye çalıştığınızda aşağıdaki hata iletisini alırsınız:

**Dosya indirilemedi. Hata ayrıntıları: hata 503. Sunucu meşgul.**
 
### <a name="solution"></a>Çözüm

Bu hataya geçici bir ağ sorunu neden olabilir. Birkaç dakika sonra VPN paketini yeniden indirmeyi deneyin.

## <a name="azure-vpn-gateway-upgrade-all-point-to-site-clients-are-unable-to-connect"></a>Azure VPN Gateway yükseltmesi: Site istemcilerine yönelik tüm noktaları bağlayamadı

### <a name="cause"></a>Nedeni

Sertifika, ömrü boyunca yüzde 50 ' den fazla ise, sertifika alınır.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için, tüm istemcilerde Noktadan siteye paketi yeniden indirip yeniden dağıtın.

## <a name="too-many-vpn-clients-connected-at-once"></a>Aynı anda çok fazla VPN istemcisi bağlı

İzin verilen en fazla bağlantı sayısına ulaşıldı. Azure portal bağlı istemcilerin toplam sayısını görebilirsiniz.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-to-the-route-table"></a>Noktadan siteye VPN, yol tablosuna 10.0.0.0/8 için bir yol yanlış ekler

### <a name="symptom"></a>Belirti

Noktadan siteye istemcisinde VPN bağlantısını çevirdiğinizde, VPN istemcisinin Azure sanal ağına doğru bir rota eklemesi gerekir. IP Yardımcısı hizmeti, VPN istemcilerinin alt ağı için bir yol eklemeli. 

VPN istemci aralığı, 10.0.12.0/24 gibi 10.0.0.0/8 ' in daha küçük bir alt ağına aittir. 10.0.12.0/24 için bir yol yerine, daha yüksek önceliğe sahip olan 10.0.0.0/8 için bir yol eklenir. 

Bu yanlış yol, 10.0.0.0/8 aralığı içindeki başka bir alt ağa ait olabilecek, belirli bir yolu tanımlanmış olmayan diğer şirket içi ağlarla bağlantıyı keser (10.50.0.0/24 gibi). 

### <a name="cause"></a>Nedeni

Bu davranış, Windows istemcileri için tasarımdır. İstemci PPP ıPCP protokolünü kullandığında, tünel arabiriminin IP adresini sunucudan (Bu durumda VPN Gateway) alır. Ancak, protokoldeki bir sınırlama nedeniyle, istemcinin alt ağ maskesi yoktur. Bunu almanın başka bir yolu olmadığından, istemci, tünel arabirimi IP adresinin sınıfına göre alt ağ maskesini tahmin etmeye çalışır. 

Bu nedenle, aşağıdaki statik eşlemeye göre bir yol eklenir: 

Adres A sınıfına aitse--> Uygula/8

Adres B sınıfına aitse--> Uygula/16

Adres C sınıfına aitse--> Uygula/24

### <a name="solution"></a>Çözüm

Diğer ağlara yönelik yolların, en uzun ön ek eşleşmesi veya daha düşük ölçüm (Bu nedenle daha yüksek öncelikli), siteye noktadan eklenmesi gerekir. 

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN istemcisi ağ dosya paylaşımlarına erişemiyor

### <a name="symptom"></a>Belirti

VPN istemcisi, Azure sanal ağına bağlandı. Ancak, istemci ağ paylaşımlarına erişemez.

### <a name="cause"></a>Nedeni

SMB protokolü, dosya paylaşma erişimi için kullanılır. Bağlantı başlatıldığında, VPN istemcisi oturum kimlik bilgilerini ekler ve hata oluşur. Bağlantı kurulduktan sonra, istemci Kerberos kimlik doğrulaması için önbellek kimlik bilgilerini kullanmaya zorlanır. Bu işlem, belirteci almak için Anahtar Dağıtım Merkezi sorguları (bir etki alanı denetleyicisi) başlatır. İstemci Internet 'ten bağlandığından, etki alanı denetleyicisine ulaşamayacak olabilir. Bu nedenle, istemci Kerberos 'tan NTLM 'ye yük devredemeyebilir. 

İstemcinin bir kimlik bilgisi için istendiği tek zaman, katıldığı etki alanı tarafından verilen geçerli bir sertifikaya (SAN = UPN ile) sahip olur. İstemci Ayrıca etki alanı ağına fiziksel olarak bağlı olmalıdır. Bu durumda, istemci sertifikayı kullanmaya ve etki alanı denetleyicisine ulaşmaya çalışır. Sonra Anahtar Dağıtım Merkezi bir "KDC_ERR_C_PRINCIPAL_UNKNOWN" hatası döndürür. İstemci, NTLM 'ye yük devredezorlanıyor. 

### <a name="solution"></a>Çözüm

Sorunu geçici olarak çözmek için, aşağıdaki kayıt defteri alt anahtarından etki alanı kimlik bilgilerinin önbelleğe alınmasını devre dışı bırakın: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a>VPN istemcisini yeniden yükledikten sonra Windows 'da Noktadan siteye VPN bağlantısı bulunamıyor

### <a name="symptom"></a>Belirti

Noktadan siteye VPN bağlantısını kaldırır ve ardından VPN istemcisini yeniden yüklersiniz. Bu durumda, VPN bağlantısı başarılı bir şekilde yapılandırılmamış. VPN bağlantısını Windows 'daki **ağ bağlantıları** ayarları 'nda görmezsiniz.

### <a name="solution"></a>Çözüm

Sorunu çözmek için, eski VPN istemci yapılandırma dosyalarını **C:\Users\UserName\AppData\Roaming\Microsoft\Network\Connections\<virtualnetworkıd >** öğesinden silin ve ardından VPN istemci yükleyicisini yeniden çalıştırın.

## <a name="point-to-site-vpn-client-cannot-resolve-the-fqdn-of-the-resources-in-the-local-domain"></a>Noktadan siteye VPN istemcisi, yerel etki alanındaki kaynakların FQDN 'sini çözümleyemiyor

### <a name="symptom"></a>Belirti

İstemci, Noktadan siteye VPN bağlantısı kullanarak Azure 'a bağlandığında, yerel etki alanındaki kaynakların FQDN 'sini çözemez.

### <a name="cause"></a>Nedeni

Noktadan siteye VPN istemcisi, Azure sanal ağında yapılandırılmış Azure DNS sunucuları kullanır. Azure DNS sunucular, istemcide yapılandırılan yerel DNS sunucularından önceliklidir, bu nedenle tüm DNS sorguları Azure DNS sunucularına gönderilir. Azure DNS sunucularında yerel kaynaklar için kayıtlar yoksa sorgu başarısız olur.

### <a name="solution"></a>Çözüm

Sorunu çözmek için, Azure sanal ağında kullanılan Azure DNS sunucularının, yerel kaynaklar için DNS kayıtlarını çözümleyebilecek olduğundan emin olun. Bunu yapmak için DNS Ileticileri veya koşullu ileticiler kullanabilirsiniz. Daha fazla bilgi için, bkz. [kendı DNS sunucunuzu kullanarak ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)

## <a name="the-point-to-site-vpn-connection-is-established-but-you-still-cannot-connect-to-azure-resources"></a>Noktadan siteye VPN bağlantısı oluşturulur, ancak yine de Azure kaynaklarına bağlanamazsınız 

### <a name="cause"></a>Nedeni

VPN istemcisi Azure VPN Gateway 'ten yolları alamazsanız bu sorun oluşabilir.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için [Azure VPN Gateway 'i sıfırlayın](vpn-gateway-resetgw-classic.md). Yeni yolların kullanıldığından emin olmak için, sanal ağ eşlemesi başarıyla yapılandırıldıktan sonra Noktadan siteye VPN istemcileri yeniden indirilmelidir.

## <a name="error-the-revocation-function-was-unable-to-check-revocation-because-the-revocation-server-was-offlineerror-0x80092013"></a>Hata: "İptal sunucusu çevrimdışı olduğu için iptal işlevi iptali denetedemedi. (Hata 0x80092013) "

### <a name="causes"></a>Nedenler
İstemci ve http://crl3.digicert.com/ssca-sha2-g1.crl http://crl4.digicert.com/ssca-sha2-g1.crl erişemediğinde bu hata iletisi oluşur.  İptal denetiminin bu iki siteye erişimi olması gerekir.  Bu sorun genellikle ara sunucu yapılandırılmış istemcide gerçekleşir. Bazı ortamlarda, istekler proxy sunucusundan geçmezse, sınır güvenlik duvarında reddedilir.

### <a name="solution"></a>Çözüm

Proxy sunucusu ayarlarını denetleyin, istemcisinin ve http://crl3.digicert.com/ssca-sha2-g1.crl http://crl4.digicert.com/ssca-sha2-g1.crl erişiminin olduğundan emin olun.

## <a name="vpn-client-error-the-connection-was-prevented-because-of-a-policy-configured-on-your-rasvpn-server-error-812"></a>VPN Istemci hatası: RAS/VPN sunucunuzda yapılandırılan bir ilke nedeniyle bağlantı engellendi. (Hata 812)

### <a name="cause"></a>Nedeni

Bu hata, VPN istemcisinde kimlik doğrulaması için kullandığınız RADIUS sunucusunda yanlış ayarlar varsa veya Azure Gateway, RADIUS sunucusuna ulaşamamışsa oluşur.

### <a name="solution"></a>Çözüm

RADIUS sunucusunun doğru yapılandırıldığından emin olun. Daha fazla bilgi için bkz. [RADIUS kimlik doğrulamasını Azure Multi-Factor Authentication sunucusu Ile tümleştirme](../active-directory/authentication/howto-mfaserver-dir-radius.md).

## <a name="error-405-when-you-download-root-certificate-from-vpn-gateway"></a>VPN Gateway 'ten kök sertifika yüklerken "hata 405"

### <a name="cause"></a>Nedeni

Kök sertifika yüklenmedi. Kök sertifika, istemcinin **Güvenilen Sertifikalar** deposuna yüklenir.

## <a name="vpn-client-error-the-remote-connection-was-not-made-because-the-attempted-vpn-tunnels-failed-error-800"></a>VPN Istemci hatası: Uzak bağlantı, denenen VPN tünelleri başarısız olduğu için yapılamadı. (Hata 800) 

### <a name="cause"></a>Nedeni

NIC sürücüsü güncel değil.

### <a name="solution"></a>Çözüm

NIC sürücüsünü güncelleştirin:

1. **Başlat**' a tıklayın, **Aygıt Yöneticisi**yazın ve sonuç listesinden seçin. Yönetici parolası veya onay istenirse parolayı yazın ya da onay sağlayın.
2. **Ağ bağdaştırıcıları** kategorilerinde, GÜNCELLEŞTIRMEK istediğiniz NIC 'i bulun.  
3. Cihaz adına çift tıklayın, **Sürücüyü Güncelleştir**' i seçin, **güncelleştirilmiş sürücü yazılımı Için otomatik olarak ara**' yı seçin.
4. Windows yeni bir sürücü bulamazsa cihaz üreticisinin Web sitesinde arama yapmayı deneyebilir ve talimatlarını takip edebilirsiniz.
5. Bilgisayarı yeniden başlatın ve bağlantıyı yeniden deneyin.

## <a name="error-file-download-error-target-uri-is-not-specified"></a>Hata: ' Dosya indirme hatası hedef URI 'SI belirtilmedi '

### <a name="cause"></a>Nedeni

Bunun nedeni yanlış bir ağ geçidi türü yapılandırılmıştır.

### <a name="solution"></a>Çözüm

Azure VPN ağ geçidi türü VPN olmalıdır ve VPN türü **Routebased**olmalıdır.

## <a name="vpn-package-installer-doesnt-complete"></a>VPN paketi yükleyicisi tamamlanmamış

### <a name="cause"></a>Nedeni

Bu sorun, önceki VPN istemci yüklemelerinden kaynaklanabilir. 

### <a name="solution"></a>Çözüm

**C:\Users\UserName\AppData\Roaming\Microsoft\Network\Connections\<virtualnetworkıd >** 'den eski VPN istemci yapılandırma dosyalarını silip VPN istemci yükleyicisini yeniden çalıştırın. 

## <a name="the-vpn-client-hibernates-or-sleep-after-some-time"></a>VPN istemcisi, bir süre sonra hazırda bekleme veya uyku moduna geçme

### <a name="solution"></a>Çözüm

VPN istemcisinin üzerinde çalıştığı bilgisayardaki uyku ve hazırda bekleme ayarlarını kontrol edin.
