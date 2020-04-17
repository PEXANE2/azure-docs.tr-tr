---
title: Fiziksel sunucular için bir Azure Geçiş cihazı ayarlama
description: Fiziksel sunucu değerlendirmesi için bir Azure Geçir cihazını nasıl ayarlayatısınız öğrenin.
ms.service: azure-migrate
ms.topic: article
ms.date: 04/15/2020
ms.openlocfilehash: ddc70ee9430d3a767ce01191824c150a4dbd5e6f
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538282"
---
# <a name="set-up-an-appliance-for-physical-servers"></a>Fiziksel sunucular için bir cihaz ayarlama

Bu makalede, Azure Geçir: Sunucu Değerlendirmesi aracıyla fiziksel sunucuları değerlendiriyorsanız Azure Geçir cihazının nasıl ayarlanır.

Azure Geçir cihazı, Azure Geçir Sunucusu Değerlendirmesi tarafından aşağıdakileri yapmak için kullanılan hafif bir cihazdır:

- Şirket içi sunucuları keşfedin.
- Keşfedilen sunucular için meta verileri ve performans verilerini Azure Geçiş Sunucu Değerlendirmesi'ne gönderin.

Azure Geçiş cihazı hakkında [daha fazla bilgi edinin.](migrate-appliance.md)


## <a name="appliance-deployment-steps"></a>Cihaz dağıtım adımları

Cihazı kurmak için:
- Azure portalından Azure Geçir yükleyici komut dosyası içeren sıkıştırılmış bir dosya indirin.
- Sıkıştırılmış dosyadan içeriğini ayıklayın. PowerShell konsoluna yönetim ayrıcalıklarıyla başlatın.
- Cihaz web uygulamasını başlatmak için PowerShell komut dosyasını çalıştırın.
- Cihazı ilk kez yapılandırın ve Azure Geçiş projesine kaydedin.

## <a name="download-the-installer-script"></a>Yükleyici komut dosyasını indirin

Cihaz için sıkıştırılmış dosyayı indirin.

1. **Geçiş Hedefleri** > **Sunucularında** > **Azure Geçir: Sunucu Değerlendirmesi**, **Keşfet'i**tıklatın.
2. **Discover makinelerinde** >  **Not virtualized/Other****makineleriniz sanallaştırıldı mı?**
3. Sıkıştırılmış dosyayı indirmek için **İndir'i** tıklatın.

    ![VM'yi İndir](./media/tutorial-assess-physical/download-appliance.png)


### <a name="verify-security"></a>Güvenliği doğrula

Dağıtmadan önce sıkıştırılabilen dosyanın güvenli olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. Sıkıştırılanan dosya için karma oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Genel bulut için örnek kullanım:```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256 ```
    - Devlet bulutu için örnek kullanım:```  C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller-Server-USGov.zip MD5 ```
3.  Karma değerleri doğrulayın:
 
    - Genel bulut için (en son cihaz sürümü için):

        **Algoritma** | **Karma değeri**
          --- | ---
          MD5 | 1e92ede3e87c03bd148e56a708cdd33f
          SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e6de7b9f475b6542beef114b20bfdac3c

    - Azure yönetimi için (en son cihaz sürümü için):

        **Algoritma** | **Karma değeri**
          --- | ---
          MD5 | f81c155fc4a1409901caea948713913f


## <a name="run-the-azure-migrate-installer-script"></a>Azure Geçir yükleyici komut dosyasını çalıştırma
Yükleyici komut dosyası aşağıdakileri yapar:

- Fiziksel sunucu bulma ve değerlendirme için aracılar ve bir web uygulaması yükler.
- Windows Etkinleştirme Hizmeti, IIS ve PowerShell ISE dahil olmak üzere Windows rollerini yükleyin.
- IIS yeniden yazılabilir bir modül indirin ve yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure Geçiş için kalıcı ayar ayrıntılarıyla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Yol altında aşağıdaki dosyaları oluşturur:
    - **Config Files**: %Programdata%\Microsoft Azure\Config
    - **Günlük Dosyaları**: %Programdata%\Microsoft Azure\Günlükler

Komut dosyasını aşağıdaki gibi çalıştırın:

1. Sıkıştırılmış dosyayı sunucuda cihazı barındıracak bir klasöre ayıklayın.  Komut dosyasını varolan bir Azure Geçir cihazında bir makinede çalıştırmadığınızdan emin olun.
2. PowerShell'i yönetim (yüksek) ayrıcalığıyla yukarıdaki sunucuda başlatın.
3. PowerShell dizinini indirilen sıkıştırılmış dosyadan içeriğin ayıklandığı klasörle değiştirin.
4. **AzureMigrateInstaller.ps1** adlı komut dosyasını aşağıdaki komutu çalıştırarak çalıştırın:

    - Genel bulut için:``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 ```
    - Azure Kamu için:``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-Server-USGov>AzureMigrateInstaller.ps1 ```

    Komut dosyası, başarılı bir şekilde bittiğinde cihaz web uygulamasını başlatacaktır.

Herhangi bir sorunla karşılaşırsanız, sorun giderme için C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log adresindeki komut dosyası günlüklerine erişebilirsiniz.



### <a name="verify-appliance-access-to-azure"></a>Azure'a cihaz erişimini doğrulama

VM cihazının [genel](migrate-appliance.md#public-cloud-urls) ve [resmi](migrate-appliance.md#government-cloud-urls) bulutlar için Azure URL'lerine bağlanabileceğinden emin olun.

## <a name="configure-the-appliance"></a>Cihazı yapılandırın

Cihazı ilk kez ayarlayın.

1. VM'ye bağlanabilen herhangi bir makinede tarayıcı açın ve cihaz web uygulamasının URL'sini açın: **https:// cihaz adı veya IP*adresi*: 44368**.

   Alternatif olarak, uygulama kısayolu tıklayarak masaüstünden uygulamayı açabilirsiniz.
2. Web uygulamasında > **ön koşulları ayarlayın,** aşağıdakileri yapın:
    - **Lisans**: Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - **Bağlantı**: Uygulama, VM'nin internet erişimi ne olduğunu denetler. VM proxy kullanıyorsa:
        - **Proxy ayarlarını**tıklatın ve proxy adresini ve dinleme http://ProxyIPAddress http://ProxyFQDNbağlantı noktasını formda veya .
        - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
        - Yalnızca HTTP proxy’si desteklenir.
    - **Zaman eşitleme**: Zaman doğrulanır. VM keşfinin düzgün çalışması için cihazdaki süre internet süresiyle uyumlu olmalıdır.
    - **Yükleme güncelleştirmeleri**: Azure Geçir Sunucusu Değerlendirmesi, cihazın en son güncelleştirmeleri yüklü olduğunu denetler.

### <a name="register-the-appliance-with-azure-migrate"></a>Cihazı Azure Geçiş ile kaydedin

1. **Giriş Yap'ı**tıklatın. Görünmüyorsa, tarayıcıdaki açılır pencere engelleyicisini devre dışı bıraktığınızdan emin olun.
2. Yeni sekmede, Azure kimlik bilgilerinizi kullanarak oturum açın.
    - Kullanıcı adınız ve şifrenizle oturum açın.
    - PIN ile oturum açma desteklenmez.
3. Başarılı bir şekilde oturum imzaladıktan sonra web uygulamasına geri dön.
4. Azure Geçiş projesinin oluşturulduğu aboneliği seçin. Sonra projeyi seçin.
5. Cihaz için bir ad belirtin. Ad 14 karakter veya daha az alfanümerik olmalıdır.
6. **Kaydol'u**tıklatın.


## <a name="start-continuous-discovery"></a>Sürekli keşfi başlatma

Cihazdan fiziksel sunuculara bağlanın ve keşfi başlatın.

1. Cihazın sunucuları bulmak için kullanacağı hesap kimlik bilgilerini belirtmek için **Kimlik Bilgileri Ekle'yi** tıklatın.  
2. Kimlik bilgileri için uygun bir ad olan **İşletim Sistemi'ni**ve kullanıcı adını ve parolayı belirtin. Daha sonra **Ekle**'ye tıklayın.
Windows ve Linux sunucuları için her biri bir kimlik bilgileri kümesi ekleyebilirsiniz.
4. **Sunucu ekle'yi**tıklatın ve sunucu ayrıntılarını belirtin- FQDN/IP adresi ve sunucuya bağlanmak için kimlik bilgilerinin dostu adı (satır başına bir giriş).
3. **Doğrula**'ya tıklayın. Doğrulamadan sonra, bulunabilecek sunucuların listesi gösterilir.
    - Bir sunucu için doğrulama başarısız olursa, **Durum** sütunundaki simgenin üzerine geçerek hatayı gözden geçirin. Sorunları düzeltin ve yeniden doğrulayın.
    - Bir sunucukaldırmak için **Sil'>** seçin.
4. Doğrulamadan sonra **Kaydet'i** tıklatın ve bulma işlemini başlatmak için keşfi başlatın.

Bu keşif başlar. Keşfedilen Sanal M'lerin meta verilerinin Azure portalında görünmesi yaklaşık 15 dakika sürer.

## <a name="verify-servers-in-the-portal"></a>Portaldaki sunucuları doğrulama

Bulma işlemi bittikten sonra, sunucuların portalda görünip görünmediğini doğrulayabilirsiniz.

1. Azure Geçir panosunu açın.
2. **Azure Geçir - Sunucular** > Azure**Geçir: Sunucu Değerlendirme** **sayfasında, Keşfedilen sunucuların**sayısını görüntüleyen simgeyi tıklatın.


## <a name="next-steps"></a>Sonraki adımlar

Azure Geçir Sunucu Değerlendirmesi ile [fiziksel sunucuların değerlendirmesini](tutorial-assess-physical.md) deneyin.
