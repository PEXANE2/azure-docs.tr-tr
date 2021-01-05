---
title: Şirket içi yönetim konsolunu yönetme
description: Yedekleme ve geri yükleme, ana bilgisayar adını tanımlama ve algılayıcılar için bir proxy ayarlama gibi şirket içi yönetim konsolu seçenekleri hakkında bilgi edinin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/12/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: 34efef4a01b58cc26fd1567336184837a703ade2
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97842001"
---
# <a name="manage-the-on-premises-management-console"></a>Şirket içi yönetim konsolunu yönetme

Bu makalede yedekleme ve geri yükleme, komite cihaz etkinleştirme dosyasını indirme, sertifikaları güncelleştirme ve bir proxy 'nin algılayıcılar 'a kurulması gibi şirket içi yönetim konsolu seçenekleri ele alınmaktadır.

Şirket içi yönetim konsolunu Azure portal ekleyin.

## <a name="upload-an-activation-file"></a>Bir etkinleştirme dosyasını karşıya yükle

İlk kez oturum açtığınızda, şirket içi yönetim konsolu için bir etkinleştirme dosyası indirilir. Bu dosya ekleme işlemi sırasında tanımlanan toplam kaydedilmiş cihazları içerir. Liste, birden çok abonelikle ilişkili algılayıcılar içerir.

İlk etkinleştirmeden sonra, izlenen cihazların sayısı ekleme sırasında tanımlanan kaydedilmiş cihazların sayısını aşabilir. Bu olay, örneğin, yönetim konsoluna daha fazla algılayıcı bağladığınızda meydana gelebilir. İzlenen cihazların sayısıyla kaydedilmiş cihazların sayısı arasında bir tutarsızlık varsa, yönetim konsolunda bir uyarı görüntülenir. Bu olay oluşursa, yeni bir etkinleştirme dosyası yüklemeniz gerekir.

Bir etkinleştirme dosyasını karşıya yüklemek için:

1. IoT için Azure Defender **fiyatlandırma** sayfasına gidin.
1. **Yönetim Konsolu için etkinleştirme dosyasını indirin** sekmesini seçin. Etkinleştirme dosyası indirilir.

   :::image type="content" source="media/how-to-manage-sensors-from-the-on-premises-management-console/cloud_download_opm_activation_file.png" alt-text="Etkinleştirme dosyasını indirin.":::

1. Yönetim konsolundan **sistem ayarları** ' nı seçin.
1. **Etkinleştirme**'yi seçin.
1. **Dosya Seç** ' i seçin ve kaydettiğiniz dosyayı seçin.

## <a name="manage-certificates"></a>Sertifikaları yönetme

Şirket içi yönetim konsolunun yüklenmesinden sonra, yerel olarak imzalanan bir sertifika oluşturulur ve yönetim konsolunun Web uygulamasına erişim için kullanılır. Yönetici kullanıcılar yönetim konsolunda ilk kez oturum açtığında, bir SSL/TLS sertifikası sağlamaları istenir. İlk kez kurulum hakkında daha fazla bilgi için bkz. Şirket [içi yönetim konsolunuzu etkinleştirme ve ayarlama](how-to-activate-and-set-up-your-on-premises-management-console.md).

Aşağıdaki bölümlerde, sertifikaları güncelleştirme, sertifika CLı komutlarıyla çalışma ve desteklenen sertifikalar ve sertifika parametreleri hakkında bilgiler sağlanmaktadır.

### <a name="about-certificates"></a>Sertifikalar hakkında

IoT için Azure Defender, SSL ve TLS sertifikalarını şu amaçlarla kullanır:

- CA imzalı sertifikayı karşıya yükleyerek kuruluşunuz tarafından istenen belirli sertifika ve şifreleme gereksinimlerini karşılayın.

- Yönetim Konsolu ile bağlı sensörler arasında ve bir yönetim konsolu ile yüksek kullanılabilirliğe sahip bir yönetim konsolu arasında doğrulamaya izin verin. Doğrulama, bir sertifika iptal listesi ve sertifika sona erme tarihine göre değerlendirilir. *Doğrulama başarısız olursa, Yönetim Konsolu ile algılayıcı arasındaki iletişim durdurulur ve konsolda bir doğrulama hatası görüntülenir.* Bu seçenek, yüklemeden sonra varsayılan olarak etkinleştirilmiştir.

Üçüncü taraf iletme kuralları doğrulanmaz. SYSLOG, splunk veya ServiceNow 'a gönderilen uyarı bilgileri örnekleri verilmiştir. ve Active Directory iletişim.

### <a name="update-certificates"></a>Sertifikaları güncelleştirme

Şirket içi yönetim konsolunun yönetici kullanıcıları sertifikaları güncelleştirebilir.

Bir sertifikayı güncelleştirmek için:  

1. **Sistem ayarları**' nı seçin.
1. **SSL/TLS sertifikaları**' nı seçin.
1. Sertifikayı silin veya düzenleyin ve yeni bir tane ekleyin.
   
   - Bir sertifika adı ekleyin.
   - Bir CRT dosyası ve anahtar dosyası yükleyin ve bir parola girin.
   - Gerekirse bir ped dosyası yükleyin.

Doğrulama ayarını değiştirmek için:

1. **Sertifika doğrulamasını etkinleştir** seçeneğini açın veya devre dışı bırakın.
1. **Kaydet**’i seçin.

Seçenek etkinleştirilirse ve doğrulama başarısız olursa, Yönetim Konsolu ile algılayıcı arasındaki iletişim durdurulur ve konsolda bir doğrulama hatası görüntülenir.

### <a name="certificate-support"></a>Sertifika desteği

Aşağıdaki sertifikalar desteklenir:

- Özel ve Kurumsal anahtar altyapısı (özel PKI) 
- Ortak anahtar altyapısı (genel PKI) 
- Gereç üzerinde yerel olarak oluşturuldu (yerel olarak kendinden imzalı) 

  > [!IMPORTANT]
  > Otomatik olarak imzalanan sertifikalar kullanmanızı önermiyoruz. Bu bağlantı güvenli değil ve yalnızca test ortamları için kullanılmalıdır. Sertifikanın sahibi doğrulanamaz ve sisteminizin güvenliği korunabilir olamaz. Otomatik olarak imzalanan sertifikalar, üretim ağları için asla kullanılmamalıdır.  

Aşağıdaki parametreler desteklenir: 

**Sertifika CRT**

- Etki alanı adınızın birincil sertifika dosyası
- İmza algoritması = SHA256RSA
- İmza karma algoritması = SHA256
- Geçerlilik = geçerli bir geçmiş tarihi
- Geçerli = geçerli bir gelecek tarih
- Ortak anahtar = RSA 2048 bitleri (minimum) veya 4096 bit
- CRL dağıtım noktası =. CRL dosyasının URL 'SI
- Konu CN = URL, joker bir sertifika olabilir; Örneğin, www.contoso.com veya \* . contoso.com
- Subject (C) ons TRY = tanımlanmış, örneğin, US
- Konu (OU) kuruluş birimi = tanımlı; Örneğin, contoso Labs
- Konu (O) rganleştirme = tanımlı; Örneğin, contoso Inc

**Anahtar dosyası**

- CSR 'yi oluştururken oluşturulan anahtar dosya
- RSA 2048 bit (minimum) veya 4096 bit

**Sertifika zinciri**

- CA 'nız tarafından sağlanan ara sertifika dosyası (varsa).
- Sunucu sertifikasını veren CA sertifikası, dosyanın ilk olarak köke kadar olan diğer kullanıcılar tarafından gelmelidir. 
- Zincir, paket öznitelikleri içerebilir.

**Deyimi**

- Bir anahtar desteklenir.
- Sertifikayı içeri aktarırken ayarlayın.

Diğer parametrelere sahip sertifikalar işe yarasa da Microsoft bunları desteklemez.

#### <a name="encryption-key-artifacts"></a>Şifreleme anahtarı yapıtları

**. pek: sertifika kapsayıcı dosyası**

Ad, güvenli e-posta için bir geçmiş yöntemi olan gizlilik Gelişmiş posta (PED) ' den. Kapsayıcı biçimi x509 ASN. 1 anahtarlarının Base64 dönüştürmesidir.  

Bu dosya, RFC 1421 ' de 1424 ' de tanımlanır: yalnızca genel sertifikayı (Apache yüklemeleri, CA sertifika dosyaları ve diğer SSL sertifikaları gibi) içerebilen bir kapsayıcı biçimidir. Ya da ortak anahtar, özel anahtar ve kök sertifikaları dahil olmak üzere tüm sertifika zincirini içerebilir.  

PKCS10 biçimi ped 'ye çevrilebilmesi için bir CSR de kodlayabilir.

**. cert. cer. CRT: sertifika kapsayıcı dosyası**

Bu, farklı bir uzantıya sahip. pek (veya nadiren,. der) biçimli bir dosyadır. Windows Dosya Gezgini bunu bir sertifika olarak tanır. Dosya Gezgini. ped dosyasını tanımıyor.

**. Key: özel anahtar dosyası**

Anahtar dosya, ped dosyasıyla aynı biçimde, ancak farklı bir uzantıya sahiptir. 

#### <a name="cli-commands"></a>CLI komutları

`cyberx-xsense-certificate-import`Sertifikaları içeri aktarmak için CLI komutunu kullanın. Bu aracı kullanmak için, sertifika dosyalarını cihaza yüklemeniz gerekir (WinSCP veya wget gibi araçları kullanarak).

Komut aşağıdaki giriş bayraklarını destekler:

- `-h`: Komut satırı yardım söz dizimini gösterir.

- `--crt`: Bir sertifika dosyasının yolu (. CRT uzantısı).

- `--key`:  \* . Key dosyası. Anahtar uzunluğu en az 2.048 bit olmalıdır.

- `--chain`: Bir sertifika zinciri dosyasının yolu (isteğe bağlı).

- `--pass`: Sertifikayı şifrelemek için kullanılan parola (isteğe bağlı).

- `--passphrase-set`: Varsayılan = `False` , kullanılmıyor. `True`Önceki sertifikayla sağlanan önceki parolayı kullanmak için olarak ayarlayın (isteğe bağlı).

CLı komutunu kullanırken:

- Sertifika dosyalarının gereç üzerinde okunabilir olduğunu doğrulayın.

- Sertifikadaki etki alanı adının ve IP 'nin, BT departmanının planlandığından yapılandırma ile eşleştiğinden emin olun.

## <a name="define-backup-and-restore-settings"></a>Yedekleme ve geri yükleme ayarlarını tanımlayın

Şirket içi Yönetim Konsolu sistem yedeklemesi, her gün otomatik olarak gerçekleştirilir. Veriler farklı bir diske kaydedilir. Varsayılan konum `/var/cyberx/backups` . 

Bu dosyayı otomatik olarak iç ağa aktarabilirsiniz. 

> [!NOTE]
> Yedekleme ve geri yükleme yordamını yalnızca aynı sürümde gerçekleştirebilirsiniz. 

Şirket içi yönetim konsolu makinesini yedeklemek için: 

- Bir yönetim hesabında oturum açın ve girin `sudo cyberx-management-backup -full` .

En son yedekleme dosyasını geri yüklemek için:

- Bir yönetim hesabında oturum açın ve girin `$ sudo cyberx-management-system-restore` .

Yedeği bir dış SMB sunucusuna kaydetmek için:

1. Dış SMB sunucusunda paylaşılan bir klasör oluşturun.  

   SMB sunucusuna erişmek için gereken klasör yolunu, Kullanıcı adını ve parolayı alın. 

2. IoT için Defender 'da yedeklemeler için bir dizin oluşturun:

   - `sudo mkdir /<backup_folder_name_on_ server>` 

   - `sudo chmod 777 /<backup_folder_name_on_c_server>/` 

3. Fstab Düzenle:  

   - `sudo nano /etc/fstab` 

   - `add - //<server_IP>/<folder_path> /<backup_folder_name_on_server> cifs rw,credentials=/etc/samba/user,vers=3.0,uid=cyberx,gid=cyberx,file_mode=0777,dir_mode=0777 0 0` 

4. Paylaşılacak SMB sunucusu için kimlik bilgilerini düzenleyin veya oluşturun: 

   - `sudo nano /etc/samba/user` 

5. Ekle: 

   - `username=<user name>`

   - `password=<password>` 

6. Dizini bağlama: 

   - `sudo mount -a` 

7. Bir yedekleme dizinini IoT şirket içi yönetim konsolu için Defender 'da paylaşılan klasöre yapılandırın:  

   - `sudo nano /var/cyberx/properties/backup.properties` 

   - `set Backup.shared_location to <backup_folder_name_on_server>`

## <a name="edit-the-host-name"></a>Konak adını Düzenle

Yönetim konsolunun Kurumsal DNS sunucusunda yapılandırılan ana bilgisayar adını düzenlemek için:

1. Yönetim konsolunun sol bölmesinde **sistem ayarları**' nı seçin.  

2. Konsolunun ağ bölümünde **ağ**' ı seçin. 

3. Kuruluş DNS sunucusunda yapılandırılan ana bilgisayar adını girin. 

4. **Kaydet**’i seçin.

## <a name="define-vlan-names"></a>VLAN adlarını tanımlama

VLAN adları, algılayıcı ve yönetim konsolu arasında eşitlenmez. Bileşenlerde özdeş adlar tanımlamanız gerekir.

Ağ alanı ' nda, **VLAN** ' ı seçin ve bulunan VLAN kimliklerine ad ekleyin. Sonra **Kaydet**'i seçin.

## <a name="define-a-proxy-to-sensors"></a>Algılayıcılar için bir proxy tanımlayın

Kullanıcının doğrudan sensöre oturum açmasını önleyerek sistem güvenliğini geliştirin. Bunun yerine, kullanıcıların algılayıcıya şirket içi yönetim konsolundan tek bir güvenlik duvarı kuralıyla erişmesine izin vermek için proxy Tüneli kullanın. Bu geliştirme, algılayıcı ötesinde ağ ortamına yetkisiz erişim olasılığını daraltır.

Algılayıcıların doğrudan bağlantısının olmadığı ortamlarda ara sunucu kullanın.

:::image type="content" source="media/how-to-access-sensors-using-a-proxy/proxy-diagram.png" alt-text="Kullanıcısını.":::

Aşağıdaki yordam, şirket içi yönetim konsoluna bir algılayıcı bağlar ve bu konsolda tünelleme sağlar:

1. Yönetici kimlik bilgileriyle şirket içi yönetim konsolu gereci CLı ' de oturum açın.

2. Yazın `sudo cyberx-management-tunnel-enable` ve **ENTER**' u seçin.

4. Yazın `--port 10000` ve **ENTER**' u seçin.

## <a name="adjust-system-properties"></a>Sistem özelliklerini ayarla

Sistem Özellikleri yönetim konsolundaki çeşitli işlemleri ve ayarları denetler. Bunları düzenlemek veya değiştirmek, yönetim konsolunun işlemine zarar verebilir. Ayarlarınızı değiştirmeden önce [Microsoft desteği](https://support.microsoft.com) başvurun.

Sistem özelliklerine erişmek için: 

1. Şirket içi yönetim konsolunda veya sensörde oturum açın.

2. **Sistem ayarları**' nı seçin.

3. **Genel** bölümünden **Sistem Özellikleri** ' ni seçin.

## <a name="change-the-name-of-the-on-premises-management-console"></a>Şirket içi yönetim konsolunun adını değiştirme

Şirket içi yönetim konsolunun adını değiştirebilirsiniz. Yeni adlar konsol Web tarayıcısında, çeşitli konsol pencerelerinin ve sorun giderme günlüklerinde görüntülenir. Varsayılan ad **yönetim konsoludur**.

Adı değiştirmek için:

1. Sol bölmenin en altında, geçerli adı seçin.

   :::image type="content" source="media/how-to-change-the-name-of-your-azure-consoles/console-name.png" alt-text="Şirket içi yönetim konsolu sürümünün ekran görüntüsü.":::

2. **Yönetim Konsolu yapılandırmasını düzenle** iletişim kutusunda, yeni adı girin. Ad 25 karakterden uzun olamaz.

   :::image type="content" source="media/how-to-change-the-name-of-your-azure-consoles/edit-management-console-configuration.png" alt-text="IoT platform yapılandırması için Defender 'ı düzenlemenin ekran görüntüsü.":::

3. **Kaydet**’i seçin. Yeni ad uygulandı.

   :::image type="content" source="media/how-to-change-the-name-of-your-azure-consoles/name-changed.png" alt-text="Konsolun değiştirilen adını gösteren ekran görüntüsü.":::

## <a name="password-recovery"></a>Parola kurtarma

Şirket içi yönetim konsolunuz için parola kurtarma, cihazın eklendiği aboneliğe bağlıdır. Bir cihazın hangi abonelikte ekli olduğunu bilmiyorsanız parolayı kurtaramazsınız.

Parolanızı sıfırlamak için:

1. Şirket içi yönetim konsolunun oturum açma sayfasına gidin.
1. **Parola kurtarma**' yı seçin.
1. Benzersiz tanımlayıcıyı kopyalayın.
1. IoT **siteleri ve sensör** için Defender sayfasına gidin ve **parolamı kurtar** sekmesini seçin.
1. Benzersiz tanımlayıcıyı girip **kurtar**' ı seçin. Etkinleştirme dosyası indirilir.
1. **Parola kurtarma** sayfasına gidin ve etkinleştirme dosyasını karşıya yükleyin.
1. **İleri**’yi seçin.
 
   Artık Kullanıcı adınız ve sistem tarafından oluşturulan yeni bir parola vermiş olursunuz.

> [!NOTE]
> Algılayıcı, ilk olarak bağlandığı aboneliğe bağlanır. Parolayı yalnızca, eklendiği abonelik ile kurtarabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

[Yönetim konsolundan algılayıcıları yönetme](how-to-manage-sensors-from-the-on-premises-management-console.md)

[Her sensöri Yönet](how-to-manage-individual-sensors.md)
