---
title: Azure geçişi ile değerlendirme/geçiş için Hyper-V VM 'lerini hazırlama
description: Azure geçişi ile Hyper-V VM 'lerinin değerlendirmesi/geçirilmesi için hazırlanma hakkında bilgi edinin.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: 5f669de6bd8d767ca7b947fca883187dad9fe29d
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "86109629"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Hyper-V VM 'lerinin Azure 'a değerlendirmesi ve geçirilmesi için hazırlanma

Bu makale, [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool)ve [Azure geçişi: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool)kullanarak şirket Içi Hyper-V VM 'lerinin Azure 'a değerlendirmesi ve geçişine hazırlanmanıza yardımcı olur.


Bu öğretici, Hyper-V sanal makinelerini Azure 'a nasıl değerlendirmenizi ve geçirekullanacağınızı gösteren bir serinin ilk ilkidir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure 'u Azure geçişi ile çalışacak şekilde hazırlayın.
> * Hyper-V VM 'lerini değerlendirmeye hazırlayın.
> * Hyper-V VM 'lerini geçirmeye hazırlanma 

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde ayarlayabilmenizi sağlayabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure"></a>Azure’u hazırlama

Tablo, Azure 'da gerçekleştirmeniz gereken görevleri özetler. Yönergeler tabloyu izler.

**Görev** | **Ayrıntılar** | **İzinler**
--- | --- | ---
**Azure geçişi projesi oluşturma** | Azure geçişi projesi, Azure geçiş araçları, Microsoft araçları ve üçüncü taraf teklifleriyle değerlendirmeleri ve geçişleri yönetmek ve yönetmek için merkezi bir konum sağlar. | Azure hesabınız, projenin bulunduğu kaynak grubunda katkıda bulunan veya sahip izinlerine ihtiyaç duyuyor.
**Gereci Kaydet** | Azure geçişi, Hyper-V VM 'lerini bulup değerlendirmek için hafif bir Azure geçiş gereci kullanır. [Daha fazla bilgi edinin](migrate-appliance-architecture.md#appliance-registration). | Gereci kaydettirmek için Azure hesabınızın Azure aboneliğinde katkıda bulunan veya sahip izinlerinin olması gerekir.
**Azure AD uygulaması oluşturma** | Gereci kaydederken Azure geçişi, Gereç ve Azure geçişi üzerinde çalışan aracılar arasındaki iletişim için kullanılan bir Azure Active Directory (Azure AD) uygulaması oluşturur. | Azure hesabınızın Azure AD uygulamaları oluşturmak için izinleri olması gerekir.
**VM oluşturma** | Kaynak grubunda ve sanal ağda VM oluşturma ve Azure yönetilen diskine yazma izinlerine sahip olmanız gerekir. | Azure hesabınızın, sanal makine katılımcısı rolüne ihtiyacı vardır.


### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

Azure geçişi projesi oluşturma izniniz olup olmadığını denetleyin.

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.


### <a name="assign-permissions-to-create-azure-ad-apps"></a>Azure AD uygulamaları oluşturmak için izin atama

Aşağıdaki yöntemlerden birini kullanarak, Gereç kaydı sırasında Azure AD uygulaması oluşturmak için Azure geçişi için izinler atayabilirsiniz:

- Kiracı/Genel yönetici, Kiracıdaki kullanıcılara Azure AD uygulamaları oluşturmak ve kaydolmak için izin verebilir.
- Kiracı/Genel yönetici, uygulama geliştirici rolünü (izinleri olan) hesaba atayabilir.

> [!NOTE]
> - Uygulamanın, yukarıda açıklananlar dışında başka bir abonelik üzerinde başka erişim izinleri yoktur.
> - Yalnızca yeni bir gereç kaydettiğinizde bu izinlere ihtiyacınız vardır. Gereç kurulduktan sonra izinleri kaldırabilirsiniz.


#### <a name="grant-account-permissions"></a>Hesap izinleri verme

Kiracı/Genel yönetici, izinleri aşağıdaki gibi verebilir:

1. Azure AD 'de, kiracı/genel yönetici **Azure Active Directory**  >  **Kullanıcı**  >  **Kullanıcı ayarları**' na gitmelidir.
2. Yönetici **uygulama kayıtları** **Evet**olarak ayarlanmalıdır.

    ![Azure AD izinleri](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Bu, hassas olmayan bir varsayılan ayardır. [Daha fazla bilgi edinin](../active-directory/develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Uygulama geliştirici rolü atama

Kiracı/Genel yönetici, uygulama geliştirici rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

### <a name="assign-azure-account-permissions"></a>Azure hesabı izinleri atama

Şu izinlere sahip olmanız için, sanal makine katılımcısı rolünü hesaba atayın:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Azure yönetilen diskine yazın. 


### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

[Bir Azure ağı kurun](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Şirket içi makineler Azure yönetilen disklere çoğaltılır. Geçiş için Azure 'a yük devretmek, Azure VM 'Leri bu yönetilen disklerden oluşturulur ve ayarladığınız Azure ağı 'na eklenir.


## <a name="prepare-for-assessment"></a>Değerlendirme için hazırlanma

VM değerlendirmesi için Hyper-V ' y i el ile veya bir yapılandırma betiği kullanarak hazırlayabilirsiniz. Bunlar, hazırlık adımlardır. Bir komut dosyasıyla hazırlandıysanız bunlar otomatik olarak yapılandırılır.

**Adım** | **Komut Dosyası** | **El ile**
--- | --- | ---
**Hyper-V konağı gereksinimlerini doğrulama** | Betik, konağın desteklenen bir Hyper-V sürümü ve Hyper-V rolü çalıştığını denetler.<br/><br/> WinRM hizmetini etkinleştirilir ve konakta 5985 (HTTP) ve 5986 (HTTPS) bağlantı noktalarını (meta veri koleksiyonu için gereklidir) açar. | Sunucu değerlendirmesi için [Hyper-V konağı gereksinimlerini](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) doğrulayın.<br/><br/> Hyper-V konaklarında [gerekli bağlantı noktalarının](migrate-support-matrix-hyper-v.md#port-access) açık olduğundan emin olun.
**PowerShell sürümünü doğrula** | Betiği desteklenen bir PowerShell sürümünde çalıştırıp çalıştırdığınızı denetler. | Hyper-V konağında PowerShell sürüm 4,0 veya üstünü kullandığınızı denetleyin.
**Hesap oluşturma** | Sizin (betiği çalıştıran kullanıcı) Hyper-V konağında yönetim ayrıcalıklarına sahip olduğunu doğrular.<br/><br/>  Azure geçişi hizmetinin Hyper-V konağından iletişim kurmak için kullandığı yerel bir kullanıcı hesabı (yönetici değil) oluşturmanıza olanak sağlar. Bu Kullanıcı hesabı konaktaki bu gruplara eklenir:<br/><br/> -Uzaktan yönetim kullanıcıları<br/><br/> -Hyper-V yöneticileri<br/><br/>-Performans Izleyicisi kullanıcıları | Hyper-V konakları/kümesi üzerinde yönetici izinlerine sahip bir etki alanı veya yerel kullanıcı hesabı ayarlayın.<br/><br/> -Bulmaya dahil etmek istediğiniz tüm konaklar ve kümeler için tek bir hesaba sahip olmanız gerekir.<br/><br/> -Hesap bir yerel veya etki alanı hesabı olabilir. Hyper-V konakları veya kümeleri üzerinde yönetici izinlerine sahip olması önerilir.<br/><br/> Alternatif olarak, yönetici izinleri atamak istemiyorsanız, aşağıdaki izinler gerekir: uzaktan yönetim kullanıcıları; Hyper-V yöneticileri; Performans Izleyicisi kullanıcıları.
**PowerShell uzaktan iletişimini etkinleştir** | Azure geçişi gerecinin bir WinRM bağlantısı üzerinden konakta PowerShell komutları çalıştırabilmeleri için konakta PowerShell uzaktan iletişimini mümkün bir şekilde sunar.| Ayarlamak için, her bir konakta, yönetici olarak bir PowerShell konsolu açın ve şu komutu çalıştırın:<br/><br/>``` Enable-PSRemoting -force ```
**Hyper-V tümleştirme hizmetlerini ayarlama** | Hyper-V tümleştirme hizmetlerinin konak tarafından yönetilen tüm VM 'lerde etkin olduğunu denetler. |  Her VM 'de [Hyper-V tümleştirme hizmetlerini etkinleştirin](/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) .<br/><br/> Windows Server 2003 çalıştırıyorsanız, [Bu yönergeleri izleyin](prepare-windows-server-2003-migration.md).
**VM diskleri uzak SMB paylaşımlarında bulunuyorsa kimlik bilgilerini devretmek** | Betik temsilcileri kimlik bilgileri. | Kimlik bilgilerini devretmek için [CredSSP 'Yi etkinleştirin](#enable-credssp-to-delegate-credentials) .

### <a name="run-the-script"></a>Betiği çalıştırın

Betiği aşağıdaki gibi çalıştırın:

1. Hyper-V konağında PowerShell sürüm 4,0 veya daha yeni bir sürümün yüklü olduğundan emin olun.
2. Betiği [Microsoft Indirme merkezi](https://aka.ms/migrate/script/hyperv)' nden indirin. Betik, Microsoft tarafından şifreli olarak imzalanır.
3. MD5 veya SHA256 karma dosyalarını kullanarak betik bütünlüğünü doğrulayın. Diyez etiketi değerleri aşağıda verilmiştir. Komut dosyasının karmasını oluşturmak için şu komutu çalıştırın:
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Örnek kullanım: 
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1 SHA256
    ```

4. Betik bütünlüğünü doğruladıktan sonra, bu PowerShell komutuyla her Hyper-V konağında betiği çalıştırın:
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

#### <a name="hashtag-values"></a>Diyez etiketi değerleri

Karma değerleri şunlardır:

| **Karma** | **Değer** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |



### <a name="enable-credssp-to-delegate-credentials"></a>Kimlik bilgilerini devretmek için CredSSP 'yi etkinleştirin

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
2. Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken Azure URL 'lerini gözden geçirin. URL tabanlı bir güvenlik duvarı veya proxy kullanıyorsanız, gereken URL 'lere erişim izni verdiğinden emin olun.
3. Bulma ve değerlendirme sırasında gereç tarafından toplanacak verileri gözden geçirin.
4. Gereç için bağlantı noktası erişim gereksinimlerini [gözden geçirin](migrate-appliance.md#collected-data---hyper-v) .


## <a name="prepare-for-hyper-v-migration"></a>Hyper-V geçişine hazırlanma

1. [İnceleme](migrate-support-matrix-hyper-v-migration.md#hyper-v-host-requirements) Geçiş için Hyper-V konak gereksinimleri ve Hyper-V konaklarının ve kümelerinin VM geçişi için erişmesi gereken Azure URL 'Leri.
2. Azure 'a geçirmek istediğiniz Hyper-V VM 'lerinin gereksinimlerini [gözden geçirin](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms) .
3. Azure 'a geçirmeden önce VM 'lerde gereken bazı değişiklikler vardır.
    - Geçişe başlamadan önce bu değişiklikleri yapmak önemlidir. Değişikliği yapmadan önce VM 'yi geçirirseniz, VM Azure 'da önyüklenemeyebilir.
    - Yapmanız gereken [Windows](prepare-for-migration.md#windows-machines) ve [Linux](prepare-for-migration.md#linux-machines) değişikliklerini gözden geçirin.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure hesabı izinlerini ayarlayın.
> * Değerlendirme ve geçiş için Hyper-V konakları ve VM 'Ler hazırlandı.
> * Azure geçişi gereci dağıtımı için hazırlandı.

Azure geçişi projesi oluşturmak, gereci dağıtmak ve Azure 'a geçiş için Hyper-V VM 'lerini bulup değerlendirmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Hyper-V VM 'lerini değerlendirme](./tutorial-assess-hyper-v.md)
