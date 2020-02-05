---
title: Azure geçişi ile değerlendirme/geçiş için Hyper-V VM 'lerini hazırlama
description: Azure geçişi ile Hyper-V VM 'lerinin değerlendirmesi/geçirilmesi için hazırlanma hakkında bilgi edinin.
ms.topic: tutorial
ms.date: 01/01/2020
ms.custom: mvc
ms.openlocfilehash: 1315b52e4ee6f39c27d21e3307d228219bc953d7
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76984753"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Hyper-V VM 'lerinin Azure 'a değerlendirmesi ve geçirilmesi için hazırlanma

Bu makalede, [Azure geçişi](migrate-services-overview.md)ile şirket içi Hyper-V VM 'lerinin Azure 'a değerlendirmesi ve geçirilmesi için nasıl hazırlanılacağı açıklanır.

[Azure geçişi](migrate-overview.md) , Microsoft Azure için uygulamaları, altyapıyı ve iş yüklerini keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan araçların merkezini sağlar. Hub, Azure geçiş araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir.

Bu öğretici, Hyper-V sanal makinelerini Azure 'a nasıl değerlendirmenizi ve geçirekullanacağınızı gösteren bir serinin ilk ilkidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure 'u hazırlayın. Azure hesabınız ve kaynaklarınızın Azure geçişi ile çalışması için izinleri ayarlayın.
> * Sunucu değerlendirmesi için şirket içi Hyper-V konakları ve VM 'Leri hazırlayın. Bir yapılandırma betiği veya el ile hazırlayabilir.
> * Azure geçişi gerecinin dağıtımına hazırlanın. Gereç, şirket içi VM 'Leri keşfetmek ve değerlendirmek için kullanılır.
> * Sunucu geçişi için şirket içi Hyper-V konakları ve VM 'Leri hazırlayın.


> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde ayarlayabilmenizi sağlayabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için bkz. Hyper-V değerlendirmesi ve geçişi için nasıl yapılır.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

### <a name="azure-permissions"></a>Azure izinleri

Azure geçişi dağıtımı için izinleri ayarlamanız gerekir.

**Görev** | **İzinler**
--- | ---
**Azure geçişi projesi oluşturma** | Azure hesabınızın bir proje oluşturmak için izinleri olması gerekir.
**Azure geçişi gereci kaydetme** | Azure geçişi, Azure geçişi sunucu değerlendirmesi ile Hyper-v VM 'lerini bulup değerlendirmek için hafif bir Azure geçiş gereci kullanır. Bu gereç VM 'Leri bulur ve Azure geçişi 'ne VM meta verilerini ve performans verilerini gönderir.<br/><br/>Gereç kaydı sırasında, aşağıdaki kayıt sağlayıcıları gereç-Microsoft. OffAzure, Microsoft. Migrate ve Microsoft. Keykasasında seçilen abonelikle kaydedilir. Kaynak sağlayıcısı kaydı, aboneliğinizi kaynak sağlayıcısıyla çalışacak şekilde yapılandırır. Kaynak sağlayıcılarını kaydetmek için abonelikte bir katkıda bulunan veya sahip rolü gerekir.<br/><br/> Azure geçişi, ekleme 'nin bir parçası olarak bir Azure Active Directory (Azure AD) uygulaması oluşturur:<br/> AAD uygulaması, Gereç üzerinde çalışan aracılar arasında Azure üzerinde çalışan hizmetlerle birlikte iletişim için kullanılır (kimlik doğrulaması ve yetkilendirme). Bu uygulamanın herhangi bir kaynakta ARM çağrısı veya RBAC erişimi yapma ayrıcalıkları yoktur.



### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

Azure geçişi projesi oluşturma izniniz olup olmadığını denetleyin.

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.


### <a name="assign-permissions-to-register-the-appliance"></a>Gereci kaydetmek için izin atama

Aşağıdaki yöntemlerden birini kullanarak, Gereç kaydı sırasında Azure AD uygulaması oluşturmak için Azure geçişi için izinler atayabilirsiniz:

- Kiracı/Genel yönetici, Kiracıdaki kullanıcılara Azure AD uygulamaları oluşturmak ve kaydolmak için izin verebilir.
- Kiracı/Genel yönetici, uygulama geliştirici rolünü (izinleri olan) hesaba atayabilir.

> [!NOTE]
> - Uygulamanın, yukarıda açıklananlar dışında başka bir abonelik üzerinde başka erişim izinleri yoktur.
> - Yalnızca yeni bir gereç kaydettiğinizde bu izinlere ihtiyacınız vardır. Gereç kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="grant-account-permissions"></a>Hesap izinleri verme

Kiracı/Genel yönetici, izinleri aşağıdaki gibi verebilir:

1. Azure AD 'de, kiracı/genel yönetici **Azure Active Directory** > **Kullanıcılar** > **Kullanıcı ayarları**' na gitmelidir.
2. Yönetici **uygulama kayıtları** **Evet**olarak ayarlanmalıdır.

    ![Azure AD izinleri](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Bu, hassas olmayan bir varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Uygulama geliştirici rolü atama

Kiracı/Genel yönetici, uygulama geliştirici rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-hyper-v-for-assessment"></a>Hyper-V ' i değerlendirme için hazırlama

VM değerlendirmesi için Hyper-V ' y i el ile veya bir yapılandırma betiği kullanarak hazırlayabilirsiniz. Komut dosyasıyla veya [el ile](#prepare-hyper-v-manually)hazırlanması gerekenler aşağıda verilmiştir.

- [Doğrula](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) Hyper-V konak ayarları ve [gerekli bağlantı noktalarının](migrate-support-matrix-hyper-v.md#port-access) Hyper-v konaklarında açık olduğundan emin olun.
- Her konakta PowerShell uzaktan iletişimini ayarlayın, böylece Azure geçişi gereci konakta PowerShell komutlarını bir WinRM bağlantısı üzerinden çalıştırabilir.
- VM diskleri uzak SMB paylaşımlarında bulunuyorsa kimlik bilgilerini devretmek.
- Gerecin Hyper-V konaklarındaki VM 'Leri bulması için kullanacağı bir hesap ayarlayın.
- Bulma ve değerlendirme yapmak istediğiniz her VM 'de Hyper-V tümleştirme hizmetlerini ayarlayın.



## <a name="prepare-with-a-script"></a>Komut dosyasıyla hazırlama

Betik şunları yapar:

- Betiği desteklenen bir PowerShell sürümünde çalıştırıp çalıştırdığınızı denetler.
- Sizin (betiği çalıştıran kullanıcı) Hyper-V konağında yönetim ayrıcalıklarına sahip olduğunu doğrular.
- Azure geçişi hizmetinin Hyper-V konağından iletişim kurmak için kullandığı yerel bir kullanıcı hesabı (yönetici değil) oluşturmanıza olanak sağlar. Bu Kullanıcı hesabı konaktaki bu gruplara eklenir:
    - Uzaktan yönetim kullanıcıları
    - Hyper-V yöneticileri
    - Performans Izleyicisi kullanıcıları
- Konağın desteklenen bir Hyper-V sürümü ve Hyper-V rolü çalıştığını denetler.
- WinRM hizmetini etkinleştirilir ve konakta 5985 (HTTP) ve 5986 (HTTPS) bağlantı noktalarını (meta veri koleksiyonu için gereklidir) açar.
- Konakta PowerShell uzaktan iletişimini mümkün bir şekilde sunar.
- Hyper-V tümleştirme hizmetinin konak tarafından yönetilen tüm VM 'lerde etkin olduğunu denetler.
- Gerekirse, konakta CredSSP 'yi etkinleştirilir.

Betiği aşağıdaki gibi çalıştırın:

1. Hyper-V konağında PowerShell sürüm 4,0 veya daha yeni bir sürümün yüklü olduğundan emin olun.
2. Betiği [Microsoft Indirme merkezi](https://aka.ms/migrate/script/hyperv)' nden indirin. Betik, Microsoft tarafından şifreli olarak imzalanır.
3. MD5 veya SHA256 karma dosyalarını kullanarak betik bütünlüğünü doğrulayın. Diyez etiketi değerleri aşağıda verilmiştir. Komut dosyasının karmasını oluşturmak için şu komutu çalıştırın:
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Örnek kullanım:
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1
    SHA256
    ```

4.  Betik bütünlüğünü doğruladıktan sonra, bu PowerShell komutuyla her Hyper-V konağında betiği çalıştırın:
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

### <a name="hashtag-values"></a>Diyez etiketi değerleri

Karma değerleri şunlardır:

| **Yla** | **Değer** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |


## <a name="prepare-hyper-v-manually"></a>Hyper-V ' i el ile hazırlama

Betiği kullanmak yerine, Hyper-V ' ı el ile hazırlamak için bu bölümdeki yordamları izleyin.

### <a name="verify-powershell-version"></a>PowerShell sürümünü doğrula

Hyper-V konağında PowerShell sürüm 4,0 veya daha yeni bir sürümün yüklü olduğundan emin olun.



### <a name="set-up-an-account-for-vm-discovery"></a>VM bulma için bir hesap ayarlama

Azure geçişi 'nin şirket içi VM 'Leri bulması için izinleri olması gerekir.

- Hyper-V konakları/kümesi üzerinde yönetici izinlerine sahip bir etki alanı veya yerel kullanıcı hesabı ayarlayın.

    - Bulmaya dahil etmek istediğiniz tüm konaklar ve kümeler için tek bir hesaba ihtiyacınız vardır.
    - Hesap bir yerel veya etki alanı hesabı olabilir. Hyper-V konakları veya kümeleri üzerinde yönetici izinlerine sahip olması önerilir.
    - Alternatif olarak, yönetici izinleri atamak istemiyorsanız, aşağıdaki izinler gereklidir:
        - Uzaktan yönetim kullanıcıları
        - Hyper-V yöneticileri
        - Performans Izleyicisi kullanıcıları

### <a name="verify-hyper-v-host-settings"></a>Hyper-V konak ayarlarını doğrulama

1. Sunucu değerlendirmesi için [Hyper-V konağı gereksinimlerini](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) doğrulayın.
2. Hyper-V konaklarında [gerekli bağlantı noktalarının](migrate-support-matrix-hyper-v.md#port-access) açık olduğundan emin olun.

### <a name="enable-powershell-remoting-on-hosts"></a>Konaklarda PowerShell uzaktan iletişimini etkinleştir

Her konakta PowerShell uzaktan iletişimini aşağıdaki gibi ayarlayın:

1. Her konakta, yönetici olarak bir PowerShell konsolu açın.
2. Şu komutu çalıştırın:

    ```
    Enable-PSRemoting -force
    ```
### <a name="enable-integration-services-on-vms"></a>VM 'lerde tümleştirme hizmetlerini etkinleştirme

Azure geçişi 'nin VM 'deki işletim sistemi bilgilerini yakalaması için, her bir sanal makinede Tümleştirme Hizmetleri etkinleştirilmelidir.

Bulup değerlendirmek istediğiniz VM 'lerde, her VM 'de [Hyper-V tümleştirme hizmetlerini](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) etkinleştirin.


### <a name="enable-credssp-on-hosts"></a>Konaklarda CredSSP 'yi etkinleştirme

Konağın diskler içeren VM 'Ler varsa, bu adımı konakta doldurun.

- Bu komutu, tüm Hyper-V konaklarında uzaktan çalıştırabilirsiniz.
- Kümeye yeni konak düğümleri eklerseniz, bulma için otomatik olarak eklenir, ancak gerekirse yeni düğümlerde CredSSP 'yi el ile etkinleştirmeniz gerekir.

Aşağıdaki gibi etkinleştirin:

1. SMB paylaşımlarında diskler içeren Hyper-V VM 'lerini çalıştıran Hyper-V konaklarını belirler.
2. Tanımlanan her Hyper-V konağında aşağıdaki komutu çalıştırın:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

Gereci ayarlarken, [Bu uygulamayı gereç üzerinde etkinleştirerek](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds)CredSSP ayarlamayı tamamlayacağız. Bu, bu serideki bir sonraki öğreticide açıklanmıştır.


## <a name="prepare-for-appliance-deployment"></a>Gereç dağıtımına hazırlanma

Sonraki öğreticide Azure geçişi gerecini ve değerlendirmeyi ayarlamadan önce, Gereç dağıtımına hazırlanın.

1. Gereç gereksinimlerini [doğrulayın](migrate-appliance.md#appliance---hyper-v) .
2. Gerecin erişmesi gereken Azure URL 'Lerini [gözden geçirin](migrate-appliance.md#url-access) .
3. Bulma ve değerlendirme sırasında gereç tarafından toplanacak verileri gözden geçirin.
4. Gereç [için bağlantı noktası](migrate-appliance.md#collected-data---hyper-v) erişim gereksinimleri.


## <a name="prepare-for-hyper-v-migration"></a>Hyper-V geçişine hazırlanma

1. [İnceleme](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) Geçiş için Hyper-V konak gereksinimleri ve Hyper-V konaklarının ve kümelerinin VM geçişi için erişmesi gereken Azure URL 'Leri.
2. Azure 'a geçirmek istediğiniz Hyper-V VM 'lerinin gereksinimlerini [gözden geçirin](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms) .


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure hesabı izinlerini ayarlayın.
> * Değerlendirme ve geçiş için Hyper-V konakları ve VM 'Ler hazırlandı.
> * Azure geçişi gereci dağıtımı için hazırlandı.

Azure geçişi projesi oluşturmak, gereci dağıtmak ve Azure 'a geçiş için Hyper-V VM 'lerini bulup değerlendirmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Hyper-V VM 'lerini değerlendirme](./tutorial-assess-hyper-v.md)
