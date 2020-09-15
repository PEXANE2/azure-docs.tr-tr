---
title: Azure geçişi sunucu değerlendirmesi ile fiziksel sunucuları bulma
description: Azure geçişi sunucu değerlendirmesi ile şirket içi fiziksel sunucuları bulmayı öğrenin.
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: d886d69e3ef40092af3d0d1e3d489d0d87fa343c
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90064593"
---
# <a name="tutorial-discover-physical-servers-with-server-assessment"></a>Öğretici: Sunucu değerlendirmesi ile fiziksel sunucuları bulma

Azure 'a geçiş yolculuğunun bir parçası olarak, sunucularınızı değerlendirme ve geçiş için keşfedeceksiniz.

Bu öğreticide, basit bir Azure geçişi gereci kullanarak Azure geçişi: Sunucu değerlendirmesi aracı ile şirket içi fiziksel sunucuları nasıl keşfedebileceğiniz gösterilmektedir. Makine ve performans meta verilerini sürekli olarak saptamak için gereci fiziksel sunucu olarak dağıtırsınız.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure hesabı ayarlayın.
> * Fiziksel sunucuları bulma için hazırlayın.
> * Bir Azure Geçişi projesi oluşturun.
> * Azure geçişi gereci ayarlayın.
> * Sürekli bulmayı başlatın.

> [!NOTE]
> Öğreticiler, bir senaryoyu denemek için en hızlı yolu gösterir ve varsayılan seçenekleri kullanır.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce, bu önkoşulların yerinde olup olmadığını kontrol edin.

**Gereksinim** | **Ayrıntılar**
--- | ---
**Elektrikli** | Azure geçişi gerecinin çalıştırılacağı bir makineye ihtiyacınız vardır. Makine şunları içermelidir:<br/><br/> -Windows Server 2016 yüklendi. Gereci Windows Server 2019 ile bir makinede çalıştırmak desteklenmez.<br/><br/> -16 GB RAM, 8 vCPU, 80 GB disk depolaması ve harici bir sanal anahtar.<br/><br/> -Doğrudan veya bir ara sunucu üzerinden internet erişimi olan statik veya dinamik bir IP adresi.
**Windows sunucuları** | BT 'nin yapılandırma ve performans meta verilerini çekebilmesi için WinRM bağlantı noktası 5985 (HTTP) üzerinde gelen bağlantılara izin verin.
**Linux sunucuları** | 22 (TCP) numaralı bağlantı noktasında gelen bağlantılara izin verin.

## <a name="prepare-an-azure-user-account"></a>Azure Kullanıcı hesabı hazırlama

Azure geçişi projesi oluşturmak ve Azure geçişi gerecini kaydettirmek için, şu bir hesaba sahip olmanız gerekir:
- Azure aboneliğinde katkıda bulunan veya sahip izinleri.
- Azure Active Directory uygulamaları kaydetme izinleri.

Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz. Abonelik sahibi değilseniz, izinleri aşağıdaki şekilde atamak için sahibiyle birlikte çalışın:

1. Azure portal, "abonelikler" araması yapın ve **Hizmetler**altında **abonelikler**' i seçin.

    ![Azure aboneliğini aramak için arama kutusu](./media/tutorial-discover-physical/search-subscription.png)

2. **Abonelikler** sayfasında, Azure geçişi projesi oluşturmak istediğiniz aboneliği seçin. 
3. Abonelikte **erişim denetimi (IAM)**  >  **erişimi denetle**' yi seçin.
4. **Erişimi denetle**' de ilgili Kullanıcı hesabını arayın.
5. **Rol ataması Ekle**' de, **Ekle**' ye tıklayın.

    ![Erişimi denetlemek ve rol atamak için bir kullanıcı hesabı arayın](./media/tutorial-discover-physical/azure-account-access.png)

6. **Rol ataması Ekle**' de, katkıda bulunan veya sahip rolünü seçin ve hesabı (örneğimizde azmigrateuser) seçin. Daha sonra **Kaydet**'e tıklayın.

    ![Hesaba rol atamak için rol ataması Ekle sayfasını açar](./media/tutorial-discover-physical/assign-role.png)

7. Portalda, kullanıcılar için arama yapın ve **Hizmetler**altında **Kullanıcılar**' ı seçin.
8. **Kullanıcı ayarları**' nda, Azure AD kullanıcılarının uygulamaları kaydedebildiğini doğrulayın (varsayılan olarak **Evet** ' e ayarlanır).

    ![Kullanıcıların Active Directory uygulamalar kaydedebildiğini Kullanıcı ayarlarında doğrula](./media/tutorial-discover-physical/register-apps.png)



## <a name="prepare-physical-servers"></a>Fiziksel sunucuları hazırlama

Gerecin fiziksel sunuculara erişmek için kullanabileceği bir hesap ayarlayın.

- Windows sunucuları için, bulmaya dahil etmek istediğiniz tüm Windows sunucularında yerel bir kullanıcı hesabı ayarlayın. Kullanıcı hesabını aşağıdaki gruplara ekleyin:-uzaktan yönetim kullanıcıları-performans Izleyicisi kullanıcılar-performans günlüğü kullanıcıları.
- Linux sunucuları için, bulmak istediğiniz Linux sunucularında bir kök hesabın olması gerekir. Alternatif olarak, erişimi aşağıdaki şekilde ayarlayın:
    - setcap CAP_DAC_READ_SEARCH + EIP/usr/sbin/Fdisk
    - setcap CAP_DAC_READ_SEARCH + EIP/sbin/Fdisk (/usr/sbin/Fdisk yoksa)<br/> -setcap "cap_dac_override, cap_dac_read_search, cap_fowner, cap_fsetid, cap_setuid, cap_setpcap, cap_net_bind_service, cap_net_admin, cap_sys_chroot, cap_sys_admin, cap_sys_resource, cap_audit_control, cap_setfcap = + EIP"/sbin/LVM
    - setcap CAP_DAC_READ_SEARCH + EIP/usr/sbin/dmidecode chmod a + r/sys/Class/DMI/ID/product_uuid


## <a name="set-up-a-project"></a>Proje ayarlama

Yeni bir Azure geçişi projesi ayarlayın.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel bakış**bölümünde **proje oluştur**' u seçin.
5. **Proje oluştur**' da Azure aboneliğinizi ve kaynak grubunuzu seçin. Yoksa, bir kaynak grubu oluşturun.
6. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

   ![Proje adı ve bölgesi için kutular](./media/tutorial-discover-physical/new-project.png)

7. **Oluştur**’u seçin.
8. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin.

**Azure geçişi: Sunucu değerlendirmesi** Aracı, varsayılan olarak yeni projeye eklenir.

![Varsayılan olarak eklenen sunucu değerlendirmesi aracını gösteren sayfa](./media/tutorial-discover-physical/added-tool.png)


## <a name="set-up-the-appliance"></a>Gereci ayarlama

Gereci fiziksel bir makineye ayarlamak için [uygun bir makineye](#prerequisites)bir yükleyici betiği indirip çalıştırın. Gereci oluşturduktan sonra ilk kez ayarlayın ve Azure geçişi projesine kaydedin.

### <a name="download-the-installer-script"></a>Yükleyici betiğini indir

Gereç için daraltılmış dosyayı indirin.

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **keşfet**' e tıklayın.
2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mi?**, **sanallaştırılmamış/diğer**' e tıklayın.
3. Sıkıştırılmış dosyayı indirmek için **İndir** ' e tıklayın.


### <a name="install-the-appliance"></a>Gereci yükler

Yükleyici betiği şunları yapar:

- Fiziksel sunucu keşfi ve değerlendirmesi için aracıları ve bir Web uygulamasını kurar.
- Windows etkinleştirme hizmeti, IIS ve PowerShell ıSE dahil Windows rollerini yükler.
- Bir IIS yeniden yazılabilir modülünü indirip yükler. [Daha fazla bilgi edinin](https://www.microsoft.com/download/details.aspx?id=7435).
- Azure geçişi için kalıcı ayar ayrıntılarıyla bir kayıt defteri anahtarını (HKLM) güncelleştirir.
- Yolun altında aşağıdaki dosyaları oluşturur:
    - **Yapılandırma dosyaları**:%ProgramData%\Microsoft Azure\Config
    - **Günlük dosyaları**:%ProgramData%\Microsoft Azure\Logs

Betiği aşağıdaki gibi çalıştırın:

1. Sıkıştırılmış dosyayı, Gereç barındıracak sunucuda bir klasöre ayıklayın.  Betiği mevcut bir Azure geçişi gereci üzerinde bir makinede çalıştırmayın emin olun.
2. Yönetim (yükseltilmiş) ayrıcalığıyla yukarıdaki sunucuda PowerShell 'i başlatın.
3. PowerShell dizinini, indirilen sıkıştırılmış dosyadan içeriğin ayıklandığı klasör olarak değiştirin.
4. Aşağıdaki komutu çalıştırarak **AzureMigrateInstaller.ps1** adlı betiği çalıştırın:

    - Genel bulut için: ``` PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 ```
    - Azure Kamu için: ``` PS C:\Users\Administrators\Desktop\AzureMigrateInstaller-Server-USGov>AzureMigrateInstaller.ps1 ```

    Betik, başarıyla tamamlandığında gereç Web uygulamasını başlatacaktır.

Herhangi bir sorun yaşıyorsanız, sorun giderme için C:\ProgramData\Microsoft Azure\Logs\ AzureMigrateScenarioInstaller_<em>timestamp</em>. log dosyasına komut dosyası günlüklerine erişebilirsiniz.


### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç erişimini aşağıdaki gibi denetleyin:

1. Gereç makinesinin, [kamu ve](migrate-appliance.md#public-cloud-urls) [kamu](migrate-appliance.md#government-cloud-urls) bulutları için Azure URL 'lerine bağlanabildiğinizden emin olun.
2. Bu bağlantı noktalarının gereç makinesinde açık olduğundan emin olun:

    - TCP bağlantı noktası 3389 üzerinde gelen bağlantılara, Gereç ile Uzak Masaüstü bağlantılarına izin vermek için izin verin.
    - 44368 numaralı bağlantı noktası üzerinde gelen bağlantılara izin ver: https://<gereç-IP-veya-adı>:44368 ' yi kullanarak gereç Web uygulamasına uzaktan erişin.
    - Bağlantı noktası 443 ' de (HTTPS) giden bağlantılara izin vermek için Azure geçişi 'ne bulma ve performans meta verileri gönderme.


### <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

1. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayolunu seçerek uygulamayı gereç masaüstünden açabilirsiniz.
2. Azure geçiş gereci Web uygulamasını > **önkoşulları ayarlayın**, lisans koşullarını gözden geçirip kabul edin ve üçüncü taraf bilgilerini okuyun.
3. Gereç, sanal makinenin internet erişimi olup olmadığını ve VM 'deki sürenin internet saatine eşit olduğunu denetler.
    - Ara sunucu kullanıyorsanız, **proxy 'Yi ayarla** ' ya tıklayın ve proxy adresini ve bağlantı noktasını (veya biçiminde) belirtin http://ProxyIPAddress http://ProxyFQDN) . 
    - Ara sunucu kimlik doğrulaması gerektiriyorsa kimlik bilgilerini belirtin. Yalnızca HTTP proxy’si desteklenir.
4. Gereç, en son Azure geçişi güncelleştirmelerini yüklüyor
5. Ayarları gözden geçirin ve **devam**' a tıklayın.


### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. **Azure geçişi Ile kaydolun**bölümünde **oturum aç**' ı seçin. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.

    ![Gereci kaydetmeye başlamak için oturum aç ' a tıklayın](./media/tutorial-discover-physical/register.png)

2. **Oturum aç** sayfasında, Azure Kullanıcı adı ve parolanızla oturum açın. PIN ile oturum açma desteklenmez.
3. Başarıyla oturum açtıktan sonra uygulamaya geri dönün.
4. **Azure geçişi Ile kaydolun**bölümünde, Azure geçişi projesinin oluşturulduğu aboneliği seçin ve ardından projeyi seçin.
5. Gereç için bir ad belirtin. Ad 14 karakter veya daha kısa bir harf olmalıdır.
6. **Kaydet**’i seçin. Sonra **devam**' a tıklayın. Bir ileti başarıyla kaydı gösterir.

    ![Abonelik, proje ve gereç adını doldur ve gereci Kaydet](./media/tutorial-discover-physical/success-register.png)

## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gereç, bulma için fiziksel sunucuya bağlanır.

1. Gerecin sunucuları keşfetme için kullanacağı hesap kimlik bilgilerini belirtmek için **kimlik bilgileri ekle** ' ye tıklayın.  
2. Kullanıcı adı ve parolayla oturum açın. Anahtar ile oturum açma desteklenmiyor. Ayrıca kullanıcının bir kök oturum açması veya yerel yönetici grubunun bir parçası olması gerekir.
3. **Işletim sistemini**, kimlik bilgileri için kolay bir adı ve Kullanıcı adını ve parolayı belirtin. Daha sonra **Ekle**'ye tıklayın.
Windows ve Linux sunucuları için birden çok kimlik bilgisi ekleyebilirsiniz.
4. Sunucuya bağlanmak için sunucu **Ekle**' ye tıklayın ve sunucu ayrıntılarını BELIRTIN-FQDN/IP adresi ve kimlik bilgilerinin kolay adı (satır başına bir giriş).
5. **Doğrula**'ya tıklayın. Doğrulamadan sonra, keşfedilebilir sunucu listesi gösterilir.
    - Bir sunucu için doğrulama başarısız olursa, **durum** sütunundaki simgenin üzerine gelerek hatayı gözden geçirin. Sorunları giderin ve yeniden doğrulayın.
    - Bir sunucuyu kaldırmak için > **Sil**' i seçin.
6. Doğrulamadan sonra, bulma işlemini başlatmak için **Kaydet ve bulmayı Başlat** ' a tıklayın.

Bu, bulmayı başlatır. Meta verilerin Azure portal görünmesi genellikle sunucu başına iki dakikadan kısa sürer.


## <a name="verify-discovered-vms-in-the-portal"></a>Portalda bulunan VM 'Leri doğrulama

Bulmadan sonra, VM 'Lerin Azure portal göründüğünü doğrulayabilirsiniz:

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucuların**sayısını gösteren simgeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM 'lerine geçiş için [fiziksel sunucuları değerlendirin](tutorial-assess-physical.md) .
- Bulma sırasında gereç tarafından toplanan [verileri gözden geçirin](migrate-appliance.md#collected-data---physical) .


