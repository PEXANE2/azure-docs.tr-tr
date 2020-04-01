---
title: Azure Geçişi ile değerlendirme/geçiş için Hyper-V VM'leri hazırlama
description: Azure Geçişi ile Hyper-V V VM'lerin değerlendirilmesi/geçişi için nasıl hazırlanacağınızı öğrenin.
ms.topic: tutorial
ms.date: 03/31/2020
ms.custom: mvc
ms.openlocfilehash: d14ae4282afb610d025d08419a69c6d10c2f1d08
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436211"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Hyper-V V M'lerin azure'a değerlendirilmesi ve geçişi için hazırlanın

Bu makalede, Azure Geçişi ile şirket içi Hyper-V V VM'lerin değerlendirilmesi için nasıl hazırlanacağı açıklanmaktadır:Sunucu Değerlendirmesi (geçir-hizmetler-genel bakış.md#azure-geçir-sunucu-değerlendirme aracı) ve Azure Geçişi ile Hyper-V V VM'lerin [geçişi:Sunucu Geçişi.](migrate-services-overview.md#azure-migrate-server-migration-tool)


Bu öğretici, Hyper-V V MM'leri nasıl değerlendirip Azure'a geçirtilebildiğinizi gösteren bir serinin ilk imasıdır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure'u hazırlayın. Azure hesabınız için izinler ve Azure Geçiş ile çalışmak için kaynaklar ayarlayın.
> * Sunucu değerlendirmesi için şirket içi Hyper-V ana bilgisayarları ve VM'leri hazırlayın. Bir yapılandırma komut dosyası kullanarak veya el ile hazırlayabilirsiniz.
> * Azure Geçiş cihazının dağıtımına hazırlanın. Cihaz, şirket içi VM'leri keşfetmek ve değerlendirmek için kullanılır.
> * Sunucu geçişi için şirket içi Hyper-V ana bilgisayarları ve VM'leri hazırlayın.


> [!NOTE]
> Öğreticiler, hızlı bir şekilde kavram kanıtı ayarlayabilmeniz için bir senaryo için en basit dağıtım yolunu gösterir. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Ayrıntılı talimatlar için, Hyper-V değerlendirmesi ve geçiş için Nasıl Yap'ı gözden geçirin.


Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

### <a name="azure-permissions"></a>Azure izinleri

Azure Geçiş dağıtımı için izin ayarlamanız gerekir.

**Görev** | **Şey** 
--- | --- 
**Azure Geçiş projesi oluşturma** | Azure hesabınız, proje oluşturmak için Contributer veya Owner izinlerine ihtiyaç duyar. | 
**Kaynak sağlayıcılar kaydedin** | Azure Geçir, Azure Geçir Sunucu Değerlendirmesi ile Hyper-V V'leri keşfetmek ve değerlendirmek için hafif bir Azure Geçir cihazı kullanır.<br/><br/> Cihaz kaydı sırasında, kaynak sağlayıcılar cihazda seçilen aboneye kaydedilir. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Kaynak sağlayıcılarını kaydetmek için abonelikte Bir Katılımcı veya Sahip rolüne ihtiyacınız var.
**Azure AD uygulaması oluşturma** | Azure Geçir, cihazı kaydederken, cihazüzerinde çalışan aracılar ile Azure'da çalışan ilgili hizmetleri arasında iletişim kurmak için kullanılan bir Azure Etkin Dizin (Azure AD) uygulaması oluşturur. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration).<br/><br/> Azure AD uygulamaları (Uygulama Geliştiricisi'nde kullanılabilir) rolü oluşturmak için izinlere ihtiyacınız vardır.



### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

Bir Azure Geçiş projesi oluşturmak için izinlere sahip olup olduğunuzu denetleyin.

1. Azure portalında aboneliği açın ve **Access denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi**Denetle'de, ilgili hesabı bulun ve izinleri görüntülemek için bu hesabı tıklatın.
3. **Katkıda Bulunan** veya **Sahip** İzni niz olmalıdır.
    - Ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi sizsiniz.
    - Abonelik sahibi değilseniz, rolü atamak için sahibiyle birlikte çalışın.


### <a name="assign-permissions-to-register-the-appliance"></a>Cihazı kaydettirmek için izin ler atama

Aşağıdaki yöntemlerden birini kullanarak, cihaz kaydı sırasında Azure AD uygulamasını oluşturmak için Azure Geçiş izni atayabilirsiniz:

- Bir kiracı/global yönetici, Azure AD uygulamaları oluşturmak ve kaydetmek için kiracıdaki kullanıcılara izin verebilir.
- Kiracı/genel yönetici, Uygulama Geliştiricisi rolünü (izinlere sahip) hesaba atayabilir.

> [!NOTE]
> - Uygulamanın abonelikte yukarıda açıklananlar dışında başka erişim izinleri yoktur.
> - Bu izinlere yalnızca yeni bir cihaz kaydettirdiğinizde ihtiyacınız vardır. Cihaz kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="grant-account-permissions"></a>Hesap izinlerini hibe

Kiracı/global yönetici aşağıdaki gibi izinverebilir:

1. Azure AD'de, kiracı/global yönetici **Azure Etkin Dizin** > **Kullanıcıları Kullanıcı** > **Ayarları'na**gitmelidir.
2. Yönetici, **Uygulama kayıtlarını** **Evet**olarak ayarlamalıdır.

    ![Azure AD izinleri](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Bu, hassas olmayan varsayılan bir ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Uygulama Geliştiricisi rolünü atama

Kiracı/genel yönetici, Uygulama Geliştiricisi rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-hyper-v-for-assessment"></a>Hyper-V'yi değerlendirmeye hazırlayın

Hyper-V'yi VM değerlendirmesi için el ile veya yapılandırma komut dosyası kullanarak hazırlayabilirsiniz. Hazırlama adımları aşağıdaki gibidir:
- [Doğrula](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) Hyper-V ana bilgisayar ayarları ve hyper-v ana bilgisayarlarda [gerekli bağlantı noktalarının](migrate-support-matrix-hyper-v.md#port-access) açık olduğundan emin olun.
- Azure Geçiş cihazının bir WinRM bağlantısı üzerinden ana bilgisayarda PowerShell komutlarını çalıştırabilmesi için her ana bilgisayarda PowerShell remoting'i ayarlayın.
- VM diskleri uzak Kobİ paylaşımlarında bulunuyorsa temsilci kimlik bilgileri.
- Cihazın Hyper-V ana bilgisayarlarında VM'leri keşfetmek için kullanacağı bir hesap ayarlayın.
- Keşfetmek ve değerlendirmek istediğiniz her VM'de Hyper-V Entegrasyon Hizmetleri'ni ayarlayın. Tümleştirme Hizmetlerini etkinleştirdiğinizde varsayılan ayarlar Azure Geçiş için yeterlidir.

    ![Entegrasyon Hizmetlerini Etkinleştir](./media/tutorial-prepare-hyper-v/integrated-services.png)


## <a name="prepare-with-a-script"></a>Komut dosyasıyla hazırlama

Komut dosyası aşağıdakileri yapar:

- Komut dosyasını desteklenen bir PowerShell sürümünde çalıştırdığınızdan kontrol eder.
- Hyper-V ana bilgisayarda sizin (komut dosyasını çalıştıran kullanıcı) yönetim ayrıcalıklarına sahip olduğunuzu doğrular.
- Azure Geçiş hizmetinin Hyper-V ana bilgisayarla iletişim kurmak için kullandığı yerel bir kullanıcı hesabı (yönetici değil) oluşturmanıza olanak tanır. Bu kullanıcı hesabı ana bilgisayarda bu gruplara eklenir:
    - Uzaktan Yönetim Kullanıcıları
    - Hyper-V Yöneticileri
    - Performans Monitörü Kullanıcıları
- Ana bilgisayar, Hyper-V'nin desteklenen bir sürümünü ve Hyper-V rolünü çalıştırıp çalıştırdığını denetler.
- WinRM hizmetini etkinleştirir ve ana bilgisayarda (meta veri toplama için gerekli) 5985 (HTTP) ve 5986 (HTTPS) bağlantı noktalarını açar.
- PowerShell'in ana bilgisayarda remoting ini sağlar.
- Hyper-V Tümleştirme Hizmetlerinin ana bilgisayar tarafından yönetilen tüm VM'lerde etkin olup olmadığını denetler.
- Gerekirse ana bilgisayarda CredSSP sağlar.

Komut dosyasını aşağıdaki gibi çalıştırın:

1. PowerShell sürüm 4.0 veya daha sonra Hyper-V ana bilgisayara yüklediğinizden emin olun.
2. Komut dosyasını [Microsoft Download Center'dan](https://aka.ms/migrate/script/hyperv)indirin. Komut dosyası, Microsoft tarafından şifreli olarak imzalanmıştır.
3. MD5 veya SHA256 karma dosyalarını kullanarak komut dosyası bütünlüğünü doğrulayın. Hashtag değerleri aşağıdadır. Komut dosyası için karma oluşturmak için bu komutu çalıştırın:
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Örnek kullanım: 
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1
    SHA256
    ```

4.    Komut dosyası bütünlüğünü doğruladıktan sonra, komut dosyasını her Hyper-V ana bilgisayarda bu PowerShell komutuyla çalıştırın:
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

### <a name="hashtag-values"></a>Hashtag değerleri

Karma değerler şunlardır:

| **Karma** | **Değer** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **ŞAHNIsI 256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |


## <a name="prepare-manually"></a>El ile hazırlama

Komut dosyasını kullanmak yerine Hyper-V'yi el ile hazırlamak için bu bölümdeki yordamları izleyin.

### <a name="verify-powershell-version"></a>PowerShell sürümünü doğrulayın

PowerShell sürüm 4.0 veya daha sonra Hyper-V ana bilgisayara yüklediğinizden emin olun.



### <a name="set-up-an-account-for-vm-discovery"></a>VM keşfi için bir hesap ayarlama

Azure Geçir'in şirket içi VM'leri keşfetmek için izinlere ihtiyacı vardır.

- Hyper-V ana bilgisayarlarında/kümesinde yönetici izinleri olan bir etki alanı veya yerel kullanıcı hesabı ayarlayın.

    - Keşfe eklemek istediğiniz tüm ana bilgisayarlar ve kümeler için tek bir hesaba ihtiyacınız vardır.
    - Hesap yerel veya etki alanı hesabı olabilir. Hyper-V ana bilgisayarlarında veya kümelerinde Yönetici izinlerine sahip olmasını öneririz.
    - Alternatif olarak, Yönetici izinleri atamak istemiyorsanız, aşağıdaki izinler gereklidir:
        - Uzaktan Yönetim Kullanıcıları
        - Hyper-V Yöneticileri
        - Performans Monitörü Kullanıcıları

### <a name="verify-hyper-v-host-settings"></a>Hyper-V ana bilgisayar ayarlarını doğrula

1. Sunucu değerlendirmesi için [Hyper-V ana bilgisayar gereksinimlerini](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) doğrulayın.
2. Hyper-V ana bilgisayarlarında [gerekli bağlantı noktalarının](migrate-support-matrix-hyper-v.md#port-access) açık olduğundan emin olun.

### <a name="enable-powershell-remoting-on-hosts"></a>Hosts'ta PowerShell remotingini etkinleştirme

PowerShell remoting'i her ana bilgisayarda aşağıdaki gibi ayarlayın:

1. Her ana bilgisayarda, yönetici olarak bir PowerShell konsolu açın.
2. Şu komutu çalıştırın:

    ```
    Enable-PSRemoting -force
    ```
### <a name="enable-integration-services-on-vms"></a>VM'lerde Entegrasyon Hizmetlerini Etkinleştir

Azure Geçir'in VM'deki işletim sistemi bilgilerini yakalayamaması için her VM'de Tümleştirme Hizmetleri etkinleştirilmelidir.

Keşfetmek ve değerlendirmek istediğiniz VM'lerde, her VM'de [Hyper-V Entegrasyon Hizmetlerini](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) etkinleştirin.


### <a name="enable-credssp-on-hosts"></a>Ana bilgisayarlarda CredSSP'yi etkinleştirme

Ana bilgisayarda SMB paylaşımlarında diskbulunan VM'ler varsa, ana bilgisayarda bu adımı tamamlayın.

- Bu komutu tüm Hyper-V ana bilgisayarlarında uzaktan çalıştırabilirsiniz.
- Bir kümeye yeni ana bilgisayar düğümleri eklerseniz, bunlar otomatik olarak keşif için eklenir, ancak gerekirse yeni düğümlerde CredSSP'yi el ile etkinleştirmeniz gerekir.

Aşağıdaki gibi etkinleştirin:

1. SMB paylaşımlarında diskleri olan Hyper-V V'leri çalıştıran Hyper-V ana bilgisayarlarını tanımlayın.
2. Tanımlanan her Hyper-V ana bilgisayariçin aşağıdaki komutu çalıştırın:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

Cihazı kurduğunuzda, credssp'yi [cihazüzerinde etkinleştirerek](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds)ayarlamayı bitirirsiniz. Bu, bu serinin bir sonraki öğretici açıklanmıştır.


## <a name="prepare-for-appliance-deployment"></a>Cihaz dağıtımına hazırlanın

Azure Geçir cihazını kurmadan ve bir sonraki öğreticide değerlendirmeye başlamadan önce, cihaz dağıtımına hazırlanın.

1. Cihaz gereksinimlerini [doğrulayın.](migrate-appliance.md#appliance---hyper-v)
2. Cihazın erişmesi gereken Azure URL'lerini [gözden geçirin.](migrate-appliance.md#url-access)
3. Cihazın keşif ve değerlendirme sırasında toplayacakları verileri gözden geçirin.
4. Cihaz için bağlantı noktası erişim gereksinimlerini [not](migrate-appliance.md#collected-data---hyper-v) edin.


## <a name="prepare-for-hyper-v-migration"></a>Hyper-V geçişine hazırlanın

1. [İnceleme](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) Geçiş için Hyper-V ana bilgisayar gereksinimleri ve Hyper-V ana bilgisayarlarının ve kümelerinin VM geçişi için erişmeye ihtiyaç duyduğu Azure URL'leri.
2. Azure'a geçirmek istediğiniz Hyper-V VM gereksinimlerini [gözden geçirin.](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure hesap izinlerini ayarlayın.
> * Değerlendirme ve geçiş için Hyper-V ana bilgisayarları ve VM'ler hazırlanmıştır.
> * Azure Geçir cihazının dağıtımı için hazırlanmıştır.

Azure'a geçiş için bir Azure Geçir projesi oluşturmak, cihazı dağıtmak ve Hyper-V VM'leri keşfetmek ve değerlendirmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Hyper-V VM'leri Değerlendirin](./tutorial-assess-hyper-v.md)
