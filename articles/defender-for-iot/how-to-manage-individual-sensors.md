---
title: Her sensöri Yönet
description: Etkinleştirme dosyalarını yönetme, yedeklemeleri gerçekleştirme ve tek başına algılayıcıyı güncelleştirme dahil olmak üzere tek tek algılayıcıları yönetmeyi öğrenin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/22/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 87bc3b172fdbd99130dbb36cceb5f3d16fc39dbd
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97841991"
---
# <a name="manage-individual-sensors"></a>Her sensöri Yönet

Bu makalede, tek tek sensörların nasıl yönetileceği açıklanır. Görevler etkinleştirme dosyalarını yönetmeyi, yedeklemeleri gerçekleştirmeyi ve tek başına algılayıcıyı güncellemeyi içerir.

Ayrıca, birden çok sensörün eşzamanlı olarak yönetilebileceği şirket içi yönetim konsolundan bazı algılayıcı yönetim görevlerini de yapabilirsiniz.

Algılayıcı ekleme ve kayıt için Azure portal kullanırsınız.

## <a name="manage-sensor-activation-files"></a>Algılayıcı etkinleştirme dosyalarını Yönet

Sensörinizi Azure portal IoT için Azure Defender ile eklendi. Her algılayıcı yerel olarak bağlı bir algılayıcı veya buluta bağlı bir algılayıcı olarak eklendi.

Dağıttığınız her sensöre benzersiz bir etkinleştirme dosyası yüklenir. Yeni bir dosyanın ne zaman ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Yeni etkinleştirme dosyalarını karşıya yükleme](#upload-new-activation-files). Dosyayı karşıya yükleyegöremiyorsanız, bkz. [etkinleştirme dosya yükleme sorunlarını giderme](#troubleshoot-activation-file-upload).

### <a name="about-activation-files-for-locally-connected-sensors"></a>Yerel olarak bağlı sensörler için etkinleştirme dosyaları hakkında

Yerel olarak bağlı sensörler bir Azure aboneliğiyle ilişkilendirilir. Yerel olarak bağlı sensörlerinizin etkinleştirme dosyası bir sona erme tarihi içerir. Bu tarihten bir ay önce, algılayıcı konsolunun en üstünde bir uyarı iletisi görünür. Uyarı, etkinleştirme dosyasını güncelleştirinceye kadar kalır.

:::image type="content" source="media/how-to-manage-individual-sensors/system-setting-screenshot.png" alt-text="Sistem ayarlarının ekran görüntüsü.":::

Etkinleştirme dosyasının süresi dolmuşsa bile IoT özellikleri için Defender ile çalışmaya devam edebilirsiniz. 

### <a name="about-activation-files-for-cloud-connected-sensors"></a>Buluta bağlı sensörler için etkinleştirme dosyaları hakkında

Bulut bağlantısı olan sensörler, IoT Hub için Defender ile ilişkilendirilir. Bu algılayıcılar, etkinleştirme dosyası için zaman dilimleriyle sınırlı değildir. Bulut bağlantılı algılayıcılar için etkinleştirme dosyası, IoT Hub için Defender ile bağlantı sağlamak üzere kullanılır.

### <a name="upload-new-activation-files"></a>Yeni etkinleştirme dosyalarını karşıya yükle

Şu durumlarda bir eklendi algılayıcısı için yeni bir etkinleştirme dosyası yüklemeniz gerekebilir:

- Yerel olarak bağlı bir sensörde etkinleştirme dosyası süresi dolar. 

- Farklı bir algılayıcı yönetim modunda çalışmak istiyorsunuz. 

- Bir bulut bağlantılı sensöre IoT Hub 'ı için yeni bir Defender atamak istiyorsunuz.

Yeni bir etkinleştirme dosyası eklemek için:

1. **Algılayıcı yönetimi** sayfasına gidin.

2. Yeni bir etkinleştirme dosyası yüklemek istediğiniz algılayıcıyı seçin.

3. Silin.

4. Yeni modda **ekleme** sayfasından veya IoT Hub 'ı için yeni bir Defender ile algılayıcısı yeniden ekleyin.

5. Etkinleştirme dosyasını **Indirme yükleme** sayfasından indirin.

6. Dosyayı kaydedin.

    :::image type="content" source="media/how-to-manage-individual-sensors/download-activation-file.png" alt-text="IoT Hub 'ı için Defender 'dan etkinleştirme dosyasını indirin.":::

7. IoT algılayıcısı konsolunda Defender 'da oturum açın.

8. Algılayıcı konsolunda **sistem ayarları** yeniden etkinleştirme ' yi seçin  >  .

    :::image type="content" source="media/how-to-manage-individual-sensors/reactivation.png" alt-text="Sistem ayarları ekranında seçimi yeniden etkinleştirme.":::

9. **Karşıya yükle** ' yi seçin ve kaydettiğiniz dosyayı seçin.

    :::image type="content" source="media/how-to-manage-individual-sensors/upload-the-file.png" alt-text="Kaydettiğiniz dosyayı karşıya yükleyin.":::

10. **Etkinleştir**' i seçin.

### <a name="troubleshoot-activation-file-upload"></a>Etkinleştirme dosya yükleme sorunlarını giderme

Etkinleştirme dosyası yüklenemediğinden bir hata iletisi alırsınız. Aşağıdaki olaylar oluşmuş olabilir:

- **Yerel olarak bağlı sensörler için**: etkinleştirme dosyası geçerli değil. Dosya geçerli değilse IoT portalı için Defender 'a gidin. **Algılayıcı yönetimi** sayfasında, geçersiz dosya ile algılayıcı ' ı seçin ve yeni bir etkinleştirme dosyası indirin.

- **Buluta bağlı sensörler için**: algılayıcı internet 'e bağlanamıyor. Algılayıcının ağ yapılandırmasını denetleyin. Sensörizin internet 'e erişmek için bir Web proxy üzerinden bağlanması gerekiyorsa, **algılayıcı ağ yapılandırması** ekranında ara sunucunuzun doğru şekilde yapılandırıldığını doğrulayın. \*. Azure-Devices.net:443 güvenlik duvarında ve/veya proxy 'de izin verildiğini doğrulayın. Joker karakterler desteklenmiyorsa veya daha fazla denetim istiyorsanız, IoT Hub için özel Defender 'ın FQDN 'SI güvenlik duvarınız ve/veya ara sunucunuzda açılmalıdır. Ayrıntılar için bkz. [başvuru IoT Hub uç noktaları](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints).  

- **Buluta bağlı sensörler için**: etkinleştirme dosyası geçerli, ancak IoT için Defender tarafından reddedildi. Bu sorunu gideremezseniz, IoT portalı için Defender 'ın **algılayıcı yönetimi** sayfasından başka bir etkinleştirme indirebilirsiniz. Bu işe yaramazsa Microsoft Desteği başvurun.

## <a name="manage-certificates"></a>Sertifikaları yönetme

Algılayıcı yüklemesinin ardından, yerel olarak imzalanan bir sertifika oluşturulur ve algılayıcı Web uygulamasına erişim için kullanılır. Sensörde ilk kez oturum açarken, Yönetici kullanıcılardan bir SSL/TLS sertifikası sağlaması istenir.  İlk kurulum hakkında daha fazla bilgi için bkz. [oturum açma ve algılayıcı etkinleştirme](how-to-activate-and-set-up-your-sensor.md).

Bu makalede, sertifikaları güncelleştirme, sertifika CLı komutlarıyla çalışma ve desteklenen sertifikalar ve sertifika parametreleri hakkında bilgi verilmektedir.

### <a name="about-certificates"></a>Sertifikalar hakkında

IoT için Azure Defender, SSL/TLS sertifikalarını şu amaçlarla kullanır:

1. CA imzalı sertifikayı karşıya yükleyerek kuruluşunuz tarafından istenen belirli sertifika ve şifreleme gereksinimlerini karşılayın.

1. Yönetim Konsolu ile bağlı sensörler arasında ve bir yönetim konsolu ve yüksek oranda kullanılabilir bir yönetim konsolu arasında doğrulamaya izin verin. Doğrulamalar bir sertifika Iptal listesi ve sertifikanın sona erme tarihi ile değerlendirilir. **Doğrulama başarısız olursa, Yönetim Konsolu ile algılayıcı arasındaki iletişim durdurulur ve konsolda bir doğrulama hatası sunulur. Bu seçenek, yüklemeden sonra varsayılan olarak etkinleştirilmiştir.**

 Üçüncü taraf Iletme kuralları, örneğin SYSLOG 'a gönderilen uyarı bilgileri, splunk veya ServiceNow; ya da Active Directory iletişim doğrulanmaz.

### <a name="update-certificates"></a>Sertifikaları güncelleştirme

Algılayıcı yönetici kullanıcıları sertifikaları güncelleştirebilir.

Bir sertifikayı güncelleştirmek için:  

1. **Sistem ayarları**' nı seçin.
1. **SSL/TLS sertifikaları** ' nı seçin.
1. Sertifikayı silin veya düzenleyin ve yeni bir tane ekleyin.
    - Bir sertifika adı ekleyin.
    - Bir CRT dosyası ve anahtar dosyası yükleyin ve bir parola girin.
    - Gerekirse bir ped dosyası yükleyin.

Doğrulama ayarını değiştirmek için:

1. **Sertifika doğrulamasını etkinleştir** değiştirmeyi etkinleştirin veya devre dışı bırakın.
1. **Kaydet**’i seçin.

Seçenek etkinleştirilirse ve doğrulama başarısız olursa, Yönetim Konsolu ile algılayıcı arasındaki iletişim durdurulur ve konsolda bir doğrulama hatası sunulur.

### <a name="certificate-support"></a>Sertifika desteği

Aşağıdaki sertifikalar desteklenir:

- Özel/Kurumsal anahtar altyapısı (özel PKI) 
- Ortak anahtar altyapısı (genel PKI) 
- Gereç üzerinde yerel olarak oluşturuldu (yerel olarak kendinden imzalı). **Otomatik olarak imzalanan sertifikaların kullanılması önerilmez.** Bu bağlantı *güvensiz* ve yalnızca test ortamları için kullanılmalıdır. Sertifikanın sahibi doğrulanamıyor ve sisteminizin güvenliği korunabilir olamaz. Otomatik olarak imzalanan sertifikalar, üretim ağları için asla kullanılmamalıdır.  

Aşağıdaki parametreler desteklenir. Sertifika CRT

- Etki alanı adınızın birincil sertifika dosyası
- İmza algoritması = SHA256RSA
- İmza karma algoritması = SHA256
- Geçerlilik = geçerli bir geçmiş tarihi
- Geçerli = geçerli bir gelecek tarih
- Ortak anahtar = RSA 2048bitleri (minimum) veya 4096bit
- CRL dağıtım noktası =. CRL dosyasının URL 'SI
- Konu CN = URL 'SI, example.contoso.com veya *. contoso.com* gibi bir joker karakter sertifikası olabilir*
- Subject (C) ons TRY = tanımlı, ör. US
- Konu (OU) kuruluş birimi = tanımlı, örn. contoso Labs
- Konu (O) rganleştirme = tanımlı, ör. contoso Inc.

Anahtar dosyası

- CSR oluşturduğunuzda oluşturulan anahtar dosya
- RSA 2048bitleri (minimum) veya 4096bit

Sertifika Zinciri

- CA 'nız tarafından sağlanan ara sertifika dosyası (varsa)
- Sunucu sertifikasını veren CA sertifikası, dosyanın ilk olarak köke kadar olan diğer kullanıcılar tarafından gelmelidir. 
- Paket öznitelikleri içerebilir.

Deyimi

- 1 anahtar destekleniyor
- Sertifikayı içeri aktarırken kurulum

Diğer parametrelere sahip sertifikalar çalışabilir, ancak Microsoft tarafından desteklenemez.

#### <a name="encryption-key-artifacts"></a>Şifreleme anahtarı yapıtları

**. pek – sertifika kapsayıcı dosyası**

Ad gizlilik Gelişmiş posta (PED), güvenli e-posta için geçmiş bir yöntem, ancak kullandığı kapsayıcı biçimi üzerinde yer alır ve x509 ASN. 1 anahtarlarının Base64 bir çevirisi olur.  

RFC 1421 ' de 1424 ' de tanımlanır: yalnızca ortak sertifikayı (örneğin Apache yüklemeleri ve CA sertifika dosyaları/etc/SSL/CERT) içerebilen bir kapsayıcı biçimi veya ortak anahtar, özel anahtar ve kök sertifikaları dahil olmak üzere tüm sertifika zincirlerini içerebilir.  

PKCS10 biçimi ped 'ye çevrilebilmesi için bir CSR de kodlayabilir.

**. cert. cer. CRT – sertifika kapsayıcı dosyası**

Farklı bir uzantıya sahip. pek (veya nadiren. der) biçimli dosya. Windows Gezgini tarafından sertifika olarak tanınır. . Ped dosyası Windows Gezgini tarafından tanınmıyor.

**. Key – özel anahtar dosyası**

ANAHTAR dosya, ped dosyası ile aynı "biçim", ancak farklı bir uzantıya sahip.
##### <a name="use-cli-commands-to-deploy-certificates"></a>Sertifikaları dağıtmak için CLı komutlarını kullanma

Sertifikaları içeri aktarmak için *Six-xsense-Certificate-Import* CLI komutunu kullanın. Bu aracı kullanmak için, sertifika dosyalarının cihaza yüklenmesi gerekir (WinSCP veya wget gibi araçlar kullanılarak).

Komut aşağıdaki giriş bayraklarını destekler:

-h komut satırı yardım sözdizimini göster

--sertifika dosyasının CRT yolu (CRT uzantısı)

--Key *. Key dosyası, anahtar uzunluğu en az 2048 bit olmalıdır

--sertifika zinciri dosyasına zincir yolu (isteğe bağlı)

--sertifikayı şifrelemek için kullanılan geçiş parolası (isteğe bağlı)

--Parola-Ayarla varsayılan = false, kullanılmıyor. Önceki sertifikayla sağlanan önceki parolayı kullanmak için TRUE olarak ayarlayın (isteğe bağlı)

CLı komutu kullanılırken:

- Gereç üzerinde sertifika dosyalarının okunabilir olduğunu doğrulayın.

- Sertifikadaki etki alanı adının ve IP 'nin BT departmanı tarafından planlanmış yapılandırmayla eşleştiğinden emin olun.


## <a name="connect-a-sensor-to-the-management-console"></a>Yönetim konsoluna bir algılayıcı bağlama

Bu bölümde, algılayıcı ve şirket içi yönetim konsolu arasında bağlantı sağlamak açıklanmaktadır. Bir AIR-gapped ağında çalışıyorsanız ve algılayıcıdan yönetim konsoluna varlık ve uyarı bilgilerini göndermek istiyorsanız bunu yapmanız gerekir. Bu bağlantı ayrıca yönetim konsolunun sistem ayarlarını sensöre itmesi ve algılayıcısı üzerinde diğer yönetim görevlerini gerçekleştirmesini sağlar.

Bağlanmak için:

1. Şirket içi yönetim konsolunda oturum açın.

2. **Sistem ayarları**' nı seçin.

3. **Algılayıcı kurulumu – bağlantı dizesi** bölümünde, otomatik olarak oluşturulan bağlantı dizesini kopyalayın.

   :::image type="content" source="media/how-to-manage-individual-sensors/connection-string-screen.png" alt-text="Bağlantı dizesini bu ekrandan kopyalayın."::: 

4. Algılayıcı konsolunda oturum açın.

5. Sol bölmede **sistem ayarları**' nı seçin.

6. **Yönetim Konsolu bağlantısı**' nı seçin.

    :::image type="content" source="media/how-to-manage-individual-sensors/management-console-connection-screen.png" alt-text="Yönetim Konsolu bağlantısı iletişim kutusunun ekran görüntüsü.":::

7. Bağlantı dizesini **bağlantı dizesi** kutusuna yapıştırın ve **Bağlan**' ı seçin.

8. Şirket içi yönetim konsolunda, **site yönetimi** penceresinde, algılayıcıyı bir bölgeye atayın.

## <a name="change-the-name-of-a-sensor"></a>Bir sensör adını değiştirme

Algılayıcı konsolunuzun adını değiştirebilirsiniz. Yeni ad konsol Web tarayıcısında, çeşitli konsol pencerelerinin ve sorun giderme günlüklerinde görüntülenir.

Algılayıcı adlarını değiştirme işlemi, yerel olarak bağlı sensörlere ve buluta bağlı sensörlere göre değişir. Varsayılan ad **algılayıcı**' dır.

### <a name="change-the-name-of-a-locally-connected-sensor"></a>Yerel olarak bağlı bir sensör adını değiştirme

Adı değiştirmek için:

1. Konsolun sol bölmesinin en altında, geçerli algılayıcı etiketini seçin.

   :::image type="content" source="media/how-to-change-the-name-of-your-azure-consoles/label-name.png" alt-text="Algılayıcı etiketini gösteren ekran görüntüsü.":::

1. **Algılayıcı adını Düzenle** iletişim kutusunda bir ad girin.

1. **Kaydet**’i seçin. Yeni ad uygulandı.

### <a name="change-the-name-of-a-cloud-connected-sensor"></a>Buluta bağlı algılayıcının adını değiştirme

Sensör buluta bağlı bir algılayıcı olarak kaydedilmişse, algılayıcı adı kayıt sırasında atanan ad tarafından tanımlanır. Ad, ilk kez oturum açarken karşıya yüklediğiniz etkinleştirme dosyasına dahil edilir. Algılayıcının adını değiştirmek için yeni bir etkinleştirme dosyası yüklemeniz gerekir.

Adı değiştirmek için:

1. IoT için Azure Defender portalında, **algılayıcı yönetimi** sayfasına gidin.

1. Algılayıcıyı **algılayıcı yönetimi** penceresinden silin.

1. Yeni adla kaydolun.

1. Yükleme ve yeni etkinleştirme dosyası.

1. Sensörde oturum açın ve yeni etkinleştirme dosyasını karşıya yükleyin.

## <a name="update-the-sensor-network-configuration"></a>Algılayıcı ağ yapılandırmasını güncelleştirme

Algılayıcı ağ yapılandırması, algılayıcı yüklemesi sırasında tanımlandı. Yapılandırma parametrelerini değiştirebilirsiniz. Ayrıca, bir ara sunucu yapılandırması da ayarlayabilirsiniz.

Yeni bir IP adresi oluşturursanız, yeniden oturum açmanız gerekebilir.

Yapılandırmayı değiştirmek için:

1. Yan menüde **sistem ayarları**' nı seçin.

2. **Sistem ayarları** penceresinde **ağ**' ı seçin.

    :::image type="content" source="media/how-to-manage-individual-sensors/edit-network-configuration-screen.png" alt-text="Ağ ayarlarınızı yapılandırın.":::

3. Parametreleri aşağıdaki gibi ayarlayın:

    | Parametre | Açıklama |
    |--|--|
    | IP Adresi | Algılayıcı IP adresi |
    | Alt ağ maskesi | Maske adresi |
    | Varsayılan ağ geçidi | Varsayılan ağ geçidi adresi |
    | DNS | DNS sunucusu adresi |
    | Konak adı | Algılayıcı konak adı |
    | Ara sunucu | Proxy Konağı ve bağlantı noktası adı |

4. **Kaydet**’i seçin.

## <a name="synchronize-time-zones-on-the-sensor"></a>Sensörde saat dilimlerini eşitler

Tüm kullanıcıların aynı zaman ve bölge görmesini sağlamak için sensör zamanını ve bölgesini yapılandırabilirsiniz.

:::image type="content" source="media/how-to-manage-individual-sensors/time-and-region.png" alt-text="Saat ve bölgeyi yapılandırın.":::

| Parametre | Açıklama |
|--|--|
| Saat dilimi | İçin saat dilimi tanımı:<br />-Uyarılar<br />-Eğilimler ve istatistik pencere öğeleri<br />-Veri araştırma raporları<br />   -Risk değerlendirmesi raporları<br />-Saldırı vektörleri |
| Tarih biçimi | Aşağıdaki biçim seçeneklerinden birini seçin:<br />-gg/aa/yyyy ss: DD: ss<br />-AA/gg/yyyy HH: mm: ss<br />-yyyy/AA/GG SS: DD: ss |
| Tarih ve saat | Geçerli tarih ve yerel saati seçtiğiniz biçimde görüntüler.<br />Örneğin, gerçek konumunuz Amerika ve New York ise, ancak saat dilimi Avrupa ve Berlin olarak ayarlandıysa, saat Istanbul yerel saatine göre görüntülenir. |

Algılayıcı süresini yapılandırmak için:

1. Yan menüde **sistem ayarları**' nı seçin.

2. **Sistem ayarları** penceresinde **saat & bölge**' yi seçin.

3. Parametreleri ayarlayın ve **Kaydet**' i seçin.

## <a name="set-up-backup-and-restore-files"></a>Yedekleme ve geri yükleme dosyalarını ayarlama

Sistem yedekleme, günlük 3:00 ' de otomatik olarak gerçekleştirilir. Veriler sensörde farklı bir diske kaydedilir. Varsayılan konum `/var/cyberx/backups` .

Bu dosyayı otomatik olarak iç ağa aktarabilirsiniz.

> [!NOTE]
> - Yedekleme ve geri yükleme yordamı yalnızca aynı sürümler arasında gerçekleştirilebilir.
> - Bazı mimarilerde yedekleme devre dışıdır. Bunu `/var/cyberx/properties/backup.properties` dosyada etkinleştirebilirsiniz.

Şirket içi yönetim konsolunu kullanarak bir algılayıcıyı denetlemenizin, bu yedeklemeleri toplamak ve bunları yönetim konsolunda veya bir dış yedekleme sunucusunda depolamak için sensör Yedekleme zamanlamasını kullanabilirsiniz.

**Yedeklenen:** Yapılandırma ve veriler.

**Yedeklenmeyen:** PCAP dosyaları ve günlükleri. PCAPs ve günlükleri el ile yedekleyebilir ve geri yükleyebilirsiniz.

Algılayıcı yedekleme dosyaları, aşağıdaki biçimde otomatik olarak adlandırılır: `<sensor name>-backup-version-<version>-<date>.tar` . `Sensor_1-backup-version-2.6.0.102-2019-06-24_09:24:55.tar` bunun bir örneğidir.

Yedeklemeyi yapılandırmak için:

- Bir yönetim hesabında oturum açın ve girin `$ sudo cyberx-xsense-system-backup` .

En son yedekleme dosyasını geri yüklemek için:

- Bir yönetim hesabında oturum açın ve girin `$ sudo cyberx-xsense-system-restore` .

Yedeği bir dış SMB sunucusuna kaydetmek için:

1. Dış SMB sunucusunda paylaşılan bir klasör oluşturun.

    SMB sunucusuna erişmek için gereken klasör yolunu, Kullanıcı adını ve parolayı alın.

2. Sensörde yedeklemeler için bir dizin oluşturun:

    - `sudo mkdir /<backup_folder_name_on_cyberx_server>`

    - `sudo chmod 777 /<backup_folder_name_on_cyberx_server>/`

3. Düzenle `fstab` : 

    - `sudo nano /etc/fstab`

    - `add - //<server_IP>/<folder_path> /<backup_folder_name_on_cyberx_server> cifsrw,credentials=/etc/samba/user,vers=X.X,uid=cyberx,gid=cyberx,file_mode=0777,dir_mode=0777 0 0`

4. SMB sunucusu için paylaşmak üzere kimlik bilgilerini düzenleyin ve oluşturun:

    `sudo nano /etc/samba/user` 

5. Ekle:

    - `username=&gt:user name&lt:`

    - `password=<password>`

6. Dizini bağlama:

    `sudo mount -a`

7. IoT algılayıcısı için Defender 'daki paylaşılan klasöre bir yedekleme dizini yapılandırın:  

    - `sudo nano /var/cyberx/properties/backup.properties`

    - `set backup_directory_path to <backup_folder_name_on_cyberx_server>`

### <a name="restore-sensors"></a>Algılayıcılar geri yükleme

Yedeklemeleri, algılayıcı konsolundan ve CLı kullanarak geri yükleyebilirsiniz.

**Konsolundan geri yüklemek için:**

- Algılayıcı **sistem ayarları** penceresinden **görüntüyü geri yükle** ' yi seçin.

:::image type="content" source="media/how-to-manage-individual-sensors/restore-image-screen.png" alt-text="Düğmeyi seçerek görüntünüzü geri yükleyin.":::

Konsol geri yükleme başarısızlıklarını görüntüler.

**CLı kullanarak geri yüklemek için:**

- Bir yönetim hesabında oturum açın ve girin `$ sudo cyberx-management-system-restore` .

## <a name="update-a-standalone-sensor-version"></a>Tek başına bir algılayıcı sürümünü güncelleştirme

Aşağıdaki yordamda, algılayıcı konsolunu kullanarak tek başına bir algılayıcıyı güncelleştirme işlemi açıklanmaktadır. Güncelleştirme işlemi yaklaşık 30 dakika sürer.

1. [Azure portalına](https://portal.azure.com/) gidin.

2. IoT için Defender 'a gidin.

3. **Güncelleştirmeler** sayfasına gidin.

   :::image type="content" source="media/how-to-manage-individual-sensors/updates-page.png" alt-text="IoT için Defender 'ın güncelleştirmeler sayfasının ekran görüntüsü.":::

4. **Sensör** bölümünden **İndir** ' i seçin ve dosyayı kaydedin.

5. Algılayıcı konsolunun kenar çubuğunda **sistem ayarları**' nı seçin.

6. **Sürüm yükseltme** bölmesinde, **Yükselt**' i seçin.

    :::image type="content" source="media/how-to-manage-individual-sensors/upgrade-pane-v2.png" alt-text="Yükseltme bölmesinin ekran görüntüsü.":::

7. IoT **güncelleştirmeleri** için Defender sayfasından indirdiğiniz dosyayı seçin.

8. Güncelleştirme işlemi başlar ve sistemin iki kez yeniden başlatıldığı zaman. İlk yeniden başlatmanın ardından (güncelleştirme işlemi tamamlanmadan önce) sistem, oturum açma penceresiyle açılır. Oturum açtıktan sonra, yükseltme sürümü kenar çubuğunun sol alt kısmında görünür.

    :::image type="content" source="media/how-to-manage-individual-sensors/defender-for-iot-version.png" alt-text="Oturum açtıktan sonra görüntülenen yükseltme sürümünün ekran görüntüsü.":::

## <a name="forward-sensor-failure-alerts"></a>İleri algılayıcı hatası uyarıları 

Hakkındaki ayrıntıları sağlamak için uyarıları üçüncü taraflara iletebilirsiniz:

- Bağlantısı kesilen algılayıcılar

- Uzak yedekleme sorunları

:::image type="content" source="media/how-to-work-with-system-notifications/image81.png" alt-text="Yönetim sistem durumu posta görünümünün ekran görüntüsü.] (medya/image80.png)! [Yönetim sistem durumu posta görünümünün ekran görüntüsü":::

Bu bilgiler, sistem bildirimleri için bir iletme kuralı oluşturduğunuzda gönderilir.

> [!NOTE]
> Yöneticiler, sistem bildirimleri gönderebilir.

Bildirim göndermek için:

1. Şirket içi yönetim konsolunda oturum açın.
1. Yan menüden **iletme** ' yi seçin.
1. Bir iletme kuralı oluşturun.
1. **Rapor sistemi bildirimleri**' ni seçin.

Kuralları iletme hakkında daha fazla bilgi için bkz. [iletme uyarı bilgileri](how-to-forward-alert-information-to-partners.md).

## <a name="adjust-system-properties"></a>Sistem özelliklerini ayarla

Sistem Özellikleri, sensördeki çeşitli işlemleri ve ayarları denetler. Bunları düzenlemek veya değiştirmek, algılayıcı konsolunun işlemine zarar verebilir.

Ayarlarınızı değiştirmeden önce [Microsoft desteği](https://support.microsoft.com/) başvurun.

Sistem özelliklerine erişmek için:

1. Şirket içi yönetim konsolunda veya sensörde oturum açın.

2. **Sistem ayarları**' nı seçin.

3. **Genel** bölümünden **Sistem Özellikleri** ' ni seçin.

## <a name="see-also"></a>Ayrıca bkz.

[Tehdit bilgileri araştırması ve paketleri](how-to-work-with-threat-intelligence-packages.md)

[Yönetim konsolundan algılayıcıları yönetme](how-to-manage-sensors-from-the-on-premises-management-console.md)
