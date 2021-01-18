---
title: Azure geçişi sunucu değerlendirmesi ile VMware VM 'lerini bulma
description: Azure geçişi sunucu değerlendirmesi aracı ile şirket içi VMware VM 'lerini bulmayı öğrenin
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: c697a8a944c74d12202c4e9e148713c4c8433026
ms.sourcegitcommit: 949c0a2b832d55491e03531f4ced15405a7e92e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2021
ms.locfileid: "98541368"
---
# <a name="tutorial-discover-vmware-vms-with-server-assessment"></a>Öğretici: Sunucu değerlendirmesi ile VMware VM 'lerini bulma

Azure 'a geçiş sürecinizin bir parçası olarak şirket içi envanterinizi ve iş yüklerinizi keşfedeceksiniz.

Bu öğreticide, basit bir Azure geçişi gereci kullanarak Azure geçişi: Sunucu değerlendirmesi aracı ile şirket içi VMware sanal makinelerini (VM) nasıl keşfedebileceğiniz gösterilmektedir. Sanal makineleri bir VMware sanal makinesi olarak dağıtırsınız; VM 'Ler ve performans meta verileri, VM 'lerde çalışan uygulamalar ve VM bağımlılıkları için sürekli olarak keşif yapabilirsiniz.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure hesabı ayarlayın.
> * VMware ortamını bulma için hazırlayın.
> * Bir Azure Geçişi projesi oluşturun.
> * Azure geçişi gereci ayarlayın.
> * Sürekli bulmayı başlatın.

> [!NOTE]
> Öğreticiler senaryo denemek için en hızlı yolu gösterir ve mümkün olan yerlerde varsayılan seçenekleri kullanır.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce, bu önkoşulların yerinde olup olmadığını kontrol edin.


**Gereksinim** | **Ayrıntılar**
--- | ---
**vCenter Server/ESXi Konağı** | 5,5, 6,0, 6,5 veya 6,7 sürümünü çalıştıran bir vCenter Server gerekir.<br/><br/> VM 'Ler sürüm 5,5 veya üzerini çalıştıran bir ESXi konağında barındırılmalıdır.<br/><br/> VCenter Server, Gereç yapılandırma ve performans meta verilerini toplayabilmesi için TCP bağlantı noktası 443 ' de gelen bağlantılara izin verin.<br/><br/> Gereç, bağlantı noktası 443 ' de varsayılan olarak vCenter 'a bağlanır. VCenter Server farklı bir bağlantı noktasında dinliyorsa, Gereç Yapılandırma Yöneticisi 'nde vCenter Server ayrıntılarını sağladığınızda bağlantı noktasını değiştirebilirsiniz.<br/><br/> VM 'Leri barındıran ESXi sunucusunda, VM 'Lerde ve VM bağımlılıklarında yüklü olan uygulamaları öğrenmek için TCP bağlantı noktası 443 ' de gelen erişime izin verildiğinden emin olun.
**Elektrikli** | vCenter Server Azure geçiş gereci için bir VM ayırmak üzere kaynaklara ihtiyaç duyuyor:<br/><br/> -32 GB RAM, 8 vCPU ve yaklaşık 80 GB disk depolaması.<br/><br/> -Bir dış sanal anahtar ve doğrudan ya da bir proxy aracılığıyla gereç VM 'sinde internet erişimi.
**VM’ler** | Tüm Windows ve Linux işletim sistemi sürümleri, yapılandırma ve performans meta verilerinin bulunması ve VM 'lerde yüklü olan uygulamaların keşfedilmesi için desteklenir. <br/><br/> Aracısız bağımlılık analizi için desteklenen işletim sistemi sürümleri için [buraya](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) bakın.<br/><br/> Yüklü uygulamaları ve VM bağımlılıklarını öğrenmek için, VMware araçları (10.2.0 ' den sonraki sürümler) yüklenmeli ve VM 'lerde çalışıyor olmalıdır ve Windows VM 'lerinde PowerShell sürüm 2,0 veya üzeri yüklü olmalıdır.


## <a name="prepare-an-azure-user-account"></a>Azure Kullanıcı hesabı hazırlama

Azure geçişi projesi oluşturmak ve Azure geçişi gerecini kaydettirmek için, şu bir hesaba sahip olmanız gerekir:
- Azure aboneliğinde katkıda bulunan veya sahip izinleri
- Azure Active Directory (AAD) uygulamalarını kaydetme izinleri
- Sahip veya katkıda bulunan, Azure aboneliğinde aracısız VMware geçişi sırasında kullanılan bir Key Vault oluşturmak için Kullanıcı erişimi Yöneticisi izinleri

Ücretsiz Azure hesabı oluşturduysanız aboneliğinizin sahibi siz olursunuz. Abonelik sahibi değilseniz, izinleri aşağıdaki şekilde atamak için sahibiyle birlikte çalışın:

1. Azure portal, "abonelikler" araması yapın ve **Hizmetler** altında **abonelikler**' i seçin.

    ![Azure aboneliğini aramak için arama kutusu](./media/tutorial-discover-vmware/search-subscription.png)

2. **Abonelikler** sayfasında, Azure geçişi projesi oluşturmak istediğiniz aboneliği seçin. 
3. Abonelikte **erişim denetimi (IAM)**  >  **erişimi denetle**' yi seçin.
4. **Erişimi denetle**' de ilgili Kullanıcı hesabını arayın.
5. **Rol ataması Ekle**' de, **Ekle**' ye tıklayın.

    ![Erişimi denetlemek ve rol atamak için bir kullanıcı hesabı arayın](./media/tutorial-discover-vmware/azure-account-access.png)

6. **Rol ataması Ekle**' de, katkıda bulunan veya sahip rolünü seçin ve hesabı (örneğimizde azmigrateuser) seçin. Daha sonra **Kaydet**'e tıklayın.

    ![Hesaba rol atamak için rol ataması Ekle sayfasını açar](./media/tutorial-discover-vmware/assign-role.png)

1. Gereci kaydettirmek için, Azure hesabınızın **AAD uygulamalarını kaydetme izinleri** olması gerekir.
1. Azure Portal ' de, **Azure Active Directory**  >  **kullanıcıları**  >  **Kullanıcı ayarları**' na gidin.
1. **Kullanıcı ayarları**' nda, Azure AD kullanıcılarının uygulamaları kaydedebildiğini doğrulayın (varsayılan olarak **Evet** ' e ayarlanır).

    ![Kullanıcıların Active Directory uygulamalar kaydedebildiğini Kullanıcı ayarlarında doğrula](./media/tutorial-discover-vmware/register-apps.png)

9. ' Uygulama kayıtları ' ayarlarının ' No ' olarak ayarlanması durumunda, gerekli izni atamak için kiracı/genel yönetici isteyin. Alternatif olarak, kiracı/genel yönetici, AAD uygulamasının kaydedilmesine izin vermek için **uygulama geliştirici** rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="prepare-vmware"></a>VMware 'yi hazırlama

VCenter Server, hesabınızın bir OVA dosyası kullanarak VM oluşturma izinlerine sahip olup olmadığını denetleyin. Bu, Azure geçişi gereci bir VMware sanal makinesi olarak bir OVA dosyası kullanarak dağıtırken gereklidir.

Sunucu değerlendirmesi, VMware VM 'lerinin bulunması ve değerlendirilmesi için vCenter Server salt okunurdur. Yüklü uygulamaları ve VM bağımlılıklarını da öğrenmek isterseniz, hesabın **Konuk işlemleri > sanal makineler** için etkinleştirilmiş ayrıcalıklara ihtiyacı vardır.

### <a name="create-an-account-to-access-vcenter"></a>VCenter 'a erişmek için bir hesap oluşturun

VSphere Web Istemcisinde bir hesabı aşağıdaki şekilde ayarlayın:

1. VSphere Web Istemcisinde yönetici ayrıcalıklarına sahip bir hesap kullanarak > **Yönetim**' i seçin.
2. **Erişim**, **SSO kullanıcıları ve grupları**' nı seçin.
3. **Kullanıcılar**' da yeni bir kullanıcı ekleyin.
4. **Yeni Kullanıcı**' da hesap ayrıntılarını yazın. Daha sonra, **Tamam**'a tıklayın.
5. **Genel izinler**' de, Kullanıcı hesabını seçin ve hesaba **salt okunurdur** rolünü atayın. Daha sonra, **Tamam**'a tıklayın.
6. Ayrıca yüklü uygulamaları ve VM bağımlılıklarını bulmayı istiyorsanız, **Roller** ' e gidin > **salt oku** rolünü seçin ve **ayrıcalıklar** bölümünde **Konuk işlemler**' i seçin. "Alt öğelere yay" onay kutusunu seçerek ayrıcalıkları vCenter Server altındaki tüm nesnelere yayabilirsiniz.
 
    ![Salt okuma rolünde Konuk işlemlere izin vermek için onay kutusu](./media/tutorial-discover-vmware/guest-operations.png)


### <a name="create-an-account-to-access-vms"></a>VM 'Lere erişmek için bir hesap oluşturma

Yüklü uygulamaları ve VM bağımlılıklarını öğrenmek için VM 'lerde gereken ayrıcalıklara sahip bir kullanıcı hesabına sahip olmanız gerekir. Kullanıcı hesabını gereç Yapılandırma Yöneticisi ' nde sağlayabilirsiniz. Gereç, sanal makinelere hiçbir aracı yüklemez.

1. Windows VM 'ler için VM 'lerde yönetim izinlerine sahip bir hesap (yerel veya etki alanı) oluşturun.
2. Linux VM 'Ler için kök ayrıcalıkları olan bir hesap oluşturun. Alternatif olarak,/bin/netstat ve/bin/ls dosyalarında şu izinlere sahip bir hesap oluşturabilirsiniz: CAP_DAC_READ_SEARCH ve CAP_SYS_PTRACE.

> [!NOTE]
> Şu anda Azure geçişi, Windows VM 'Ler için bir kullanıcı hesabını ve yüklü uygulamalar ve VM bağımlılıklarını bulmak için gereç üzerinde sağlandırabilen Linux VM 'Ler için bir kullanıcı hesabı destekler.


## <a name="set-up-a-project"></a>Proje ayarlama

Yeni bir Azure geçişi projesi ayarlayın.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel bakış** bölümünde **proje oluştur**' u seçin.
5. **Proje oluştur**' da Azure aboneliğinizi ve kaynak grubunuzu seçin. Yoksa, bir kaynak grubu oluşturun.
6. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

   ![Proje adı ve bölgesi için kutular](./media/tutorial-discover-vmware/new-project.png)

7. **Oluştur**’u seçin.
8. Azure geçişi projesinin dağıtılması için birkaç dakika bekleyin. **Azure geçişi: Sunucu değerlendirmesi** Aracı, varsayılan olarak yeni projeye eklenir.

![Varsayılan olarak eklenen sunucu değerlendirmesi aracını gösteren sayfa](./media/tutorial-discover-vmware/added-tool.png)

> [!NOTE]
> Zaten bir proje oluşturduysanız, daha fazla sanal makine bulmayı ve değerlendirmeyi yapmak için ek gereçlere kaydolmak üzere aynı projeyi kullanabilirsiniz.[daha fazla bilgi edinin](create-manage-projects.md#find-a-project)

## <a name="set-up-the-appliance"></a>Gereci ayarlama

Azure geçişi: Sunucu değerlendirmesi basit bir Azure geçişi gereci kullanır. Gereç, VM bulma işlemini gerçekleştirir ve Azure geçişi 'ne VM yapılandırma ve performans meta verilerini gönderir. Gereç, Azure geçişi projesinden indirilebilen bir OVA şablonu dağıtarak ayarlanabilir.

> [!NOTE]
> Bir nedenden dolayı, şablonu kullanarak gereci ayarlayamazsınız, var olan bir Windows Server 2016 sunucusunda bir PowerShell betiği kullanarak bu ayarı yapabilirsiniz. [Daha fazla bilgi edinin](deploy-appliance-script.md#set-up-the-appliance-for-vmware).


### <a name="deploy-with-ova"></a>OVA ile dağıtma

Bir OVA şablonunu kullanarak gereci ayarlamak için:
1. Portalda bir gereç adı sağlayın ve bir Azure geçişi proje anahtarı oluşturun
1. Bir OVA şablon dosyasını indirip vCenter Server içe aktarın. OVA 'nın güvenli olduğunu doğrulayın.
1. Gereci oluşturun ve Azure geçişi sunucu değerlendirmesi 'ne bağlanıp bağlanamadığından emin olun.
1. Gereci ilk kez yapılandırın ve Azure geçişi projesi anahtarını kullanarak Azure geçişi projesi ile kaydedin.

### <a name="1-generate-the-azure-migrate-project-key"></a>1. Azure geçişi proje anahtarını oluşturma

1. **Geçiş hedefleri** > **Sunucular** > **Azure Geçişi: Sunucu Değerlendirmesi** bölümünde **Bul**'u seçin.
2. Makinelerde **bulunan makinelerde**  >  **makineler sanallaştırılmış mı?**, **VMware vSphere hiper yöneticiyle Evet '** i seçin.
3. **1: Azure geçişi proje anahtarı oluştur**' da, Azure geçiş gereci VMware VM 'leri bulmak için ayarlayacaksınız. ad, 14 karakter veya daha kısa olmalıdır.
1. Gerekli Azure kaynaklarını oluşturmaya başlamak için **anahtar oluştur** ' a tıklayın. Lütfen kaynakları oluşturma sırasında makineleri keşfet sayfasını kapatmayın.
1. Azure kaynakları başarıyla oluşturulduktan sonra bir **Azure geçişi proje anahtarı** oluşturulur.
1. Yapılandırma sırasında gereç kaydını tamamlamamak için gerekli olacak şekilde anahtarı kopyalayın.

### <a name="2-download-the-ova-template"></a>2. OVA şablonunu indirin

**2: Azure geçişi yükleme gereci indirin**, öğesini seçin. OVA dosyası ve **İndir**' e tıklayın.

### <a name="verify-security"></a>Güvenliği doğrulama

Dağıtım yapmadan önce OVA dosyasının güvenli olup olmadığını denetleyin:

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA dosyasının karmasını oluşturmak için aşağıdaki komutu çalıştırın:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Örnek kullanım: ```C:\>CertUtil -HashFile C:\Users\Administrator\Desktop\MicrosoftAzureMigration.ova SHA256```

3. En son gereç sürümlerini ve karma değerlerini doğrulayın:

    - Azure genel bulutu için:
    
        **Algoritma** | **İndir** | **SHA256**
        --- | --- | ---
        VMware (11,9 GB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2140333) | e9c9a1fe4f3ebae81008328e8f3a7933d78ff835ecd871d1b17f367621ce3c74

    - Azure Kamu için:
    
        **Algoritma** | **İndir** | **SHA256**
        --- | --- | ---
        VMware (85,8 MB) | [En son sürüm](https://go.microsoft.com/fwlink/?linkid=2140337) | 2daaa2a59302bf911e8ef195f8add7d7c8352de77a9af0b860e2a627979085ca

### <a name="3-create-the-appliance-vm"></a>3. gereç VM 'sini oluşturma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun.

1. VSphere istemci konsolunda,   >  **ovf şablonu dosya dağıt**' a tıklayın.
2. OVF şablonu Dağıtma Sihirbazı > **kaynak** bölümünde ova dosyasının konumunu belirtin.
3. **Ad** ve **konum** bölümünde VM için bir kolay ad belirtin. VM 'nin barındırıldığı envanter nesnesini seçin.
5. **Konakta/kümede**, sanal makinenin çalıştırılacağı konağı veya kümeyi belirtin.
6. **Depolama** alanında VM için depolama hedefini belirtin.
7. **Disk Biçimi**’nde disk türünü ve boyutunu belirtin.
8. **Ağ eşlemesinde**, sanal makinenin bağlanacağı ağı belirtin. Ağ, Azure geçişi sunucu değerlendirmesi 'ne meta veri göndermek için internet bağlantısı gerektirir.
9. Ayarları gözden geçirip onayladıktan sonra **Son**’a tıklayın.


### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç sanal makinesinin, [kamu](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutları için Azure URL 'lerine bağlanabildiğinizden emin olun.


### <a name="4-configure-the-appliance"></a>4. gereci yapılandırma

Gereci ilk kez ayarlayın.

> [!NOTE]
> Gereci, indirilen OVA yerine bir [PowerShell betiği](deploy-appliance-script.md) kullanarak ayarlarsanız, bu yordamın ilk iki adımı ilgili değildir.

1. VSphere Istemci konsolunda VM 'ye sağ tıklayın ve ardından **Konsolu Aç**' ı seçin.
2. Gereç için dil, saat dilimi ve parola sağlayın.
3. VM 'ye bağlanabilecek herhangi bir makinede bir tarayıcı açın ve gereç Web uygulamasının URL 'sini açın: **https://*Gereç adı veya IP adresi*: 44368**.

   Alternatif olarak, uygulama kısayolunu seçerek uygulamayı gereç masaüstünden açabilirsiniz.
1. **Lisans koşullarını** kabul edin ve üçüncü taraf bilgilerini okuyun.
1. **Önkoşulları ayarlamak**> Web uygulamasında şunları yapın:
   - **Bağlantı**: uygulama, sanal makinenin internet erişimi olup olmadığını denetler. VM bir proxy kullanıyorsa:
     - Proxy adresini belirtmek için **proxy ayarla** ' ya tıklayın (formdaki http://ProxyIPAddress veya http://ProxyFQDN) dinleme bağlantı noktasında).
     - Proxy için kimlik doğrulaması gerekiyorsa kimlik bilgilerini gerekin.
     - Yalnızca HTTP proxy’si desteklenir.
     - Proxy ayrıntıları eklediyseniz veya proxy ve/veya kimlik doğrulamasını devre dışı bırakırsanız, bağlantıyı tetiklemek için **Kaydet** 'e tıklayarak bağlantı denetimini yeniden başlatın.
   - **Zaman eşitleme**: bulmanın düzgün çalışması için gereç üzerindeki zaman internet ile eşitlenmiş olmalıdır.
   - **Güncelleştirmeleri yükleme**: gereç en son güncelleştirmelerin yüklü olmasını sağlar. Denetim tamamlandıktan sonra gereç **hizmetlerini görüntüle** ' ye tıklayarak gereç üzerinde çalışan bileşenlerin durumunu ve sürümlerini görebilirsiniz.
   - **VDDK 'Yi yükleme**: gereç, VMware vSphere sanal disk geliştirme seti 'nın (VDDK) yüklü olduğunu denetler. Yüklü değilse, VMware 'den VDDK 6,7 ' i indirin ve indirilen ZIP içeriğini **yükleme yönergelerindeki** belirtilen konuma ayıklayın.

     Azure geçişi sunucu geçişi, Azure 'a geçiş sırasında makineleri çoğaltmak için VDDK 'yi kullanır. 
1. İsterseniz, gerecin tüm önkoşulları karşılayıp karşılamadığını denetlemek için gereç yapılandırması sırasında dilediğiniz zaman **Önkoşulları yeniden çalıştırabilirsiniz** .

### <a name="register-the-appliance-with-azure-migrate"></a>Gereci Azure geçişi ile kaydetme

1. Portaldan kopyalanmış **Azure geçişi proje anahtarını** yapıştırın. Anahtarınız yoksa, sunucu değerlendirmesi ' ne gidin **> var olan gereçlerini keşfet> yönetin**, anahtar oluşturma sırasında verdiğiniz gereç adını seçin ve ilgili anahtarı kopyalayın.
1. Azure ile kimlik doğrulaması yapmak için bir cihaz koduna ihtiyacınız olacaktır. **Oturum** açmak için tıkladığınızda, aşağıda gösterildiği gibi cihaz koduyla kalıcı olarak açılır.

    ![Cihaz kodunu kalıcı olarak gösterme](./media/tutorial-discover-vmware/device-code.png)

1. Cihaz kodunu kopyalamak ve yeni bir tarayıcı sekmesinde bir Azure oturum açma istemi açmak için **kodu kopyala & oturum** aç ' a tıklayın. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.
1. Yeni sekmede, cihaz kodunu yapıştırın ve Azure Kullanıcı adınızı ve parolanızı kullanarak oturum açın.
   
   PIN ile oturum açma desteklenmez.
3. Oturum açmadan oturum açma sekmesini yanlışlıkla kapatırsanız, oturum açma düğmesini yeniden etkinleştirmek için gereç Yapılandırma Yöneticisi 'nin tarayıcı sekmesini yenilemeniz gerekir.
1. Başarıyla oturum açtıktan sonra, Gereç Yapılandırma Yöneticisi ile önceki sekmeye geri dönün.
1. Günlüğe kaydetme için kullanılan Azure Kullanıcı hesabının, anahtar üretimi sırasında oluşturulan Azure kaynakları üzerinde doğru izinleri varsa, Gereç kaydı başlatılır.
1. Gereç başarıyla kaydedildikten sonra, **Ayrıntıları görüntüle**' ye tıklayarak kayıt ayrıntılarına bakabilirsiniz.



## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gerecin, VM 'lerin yapılandırma ve performans verilerini bulması için vCenter Server 'e bağlanması gerekir.

1. **1. Adım: vCenter Server kimlik** bilgilerini belirtin bölümünde kimlik bilgileri için kolay bir ad belirtmek üzere **kimlik bilgileri ekle** ' ye tıklayın, gerecin vCenter Server örneğindeki VM 'leri bulması için kullanacağı vCenter Server hesabı Için **Kullanıcı adı** ve **parola** ekleyin.
    - Önceki öğreticide gerekli izinlere sahip bir hesap ayarlamış olmanız gerekir.
    - Bulma işlemini belirli VMware nesneleri (vCenter Server veri merkezleri, kümeler, bir küme klasörü, konaklar, bir konaklar klasörü veya ayrı VM 'Ler) olarak atamak istiyorsanız, Azure geçişi tarafından kullanılan hesabı kısıtlamak için [Bu makaledeki](set-discovery-scope.md) yönergeleri gözden geçirin.
1. **2. Adım: vCenter Server ayrıntıları belirtin** bölümünde, açılan listeden kimlik bilgileri için kolay ad seçmek üzere **bulma kaynağı Ekle** ' ye tıklayın, vCenter Server ÖRNEĞININ **IP adresini/FQDN** 'sini belirtin. **Bağlantı noktasını** varsayılan (443) olarak bırakabilir veya vCenter Server dinlediği ve **Kaydet**' e tıklayarak özel bir bağlantı noktası belirtebilirsiniz.
1. Kaydet 'e tıklanınca, Gereç vCenter Server bağlantısını doğrulamayı dener ve tablodaki **doğrulama durumunu** vCenter Server IP ADRESINE/FQDN 'ye göre gösterir.
1. Bulmayı başlatmadan önce vCenter Server bağlantıyı **yeniden doğrulayabilirsiniz** .
1. **3. Adım: yüklü uygulamaları bulmaya ve aracısız bağımlılık eşlemesi gerçekleştirmeye YÖNELIK VM kimlik bilgilerini sağlayın**, kimlik bilgileri **Ekle**' ye tıklayın ve kimlik bilgilerinin sağlandığı işletim sistemini, kimlik bilgileri Için kolay adı ve **Kullanıcı** adını ve **parolayı** belirtin. Ardından **Kaydet**' e tıklayın.

    - [Uygulama bulma](how-to-discover-applications.md)veya [aracısız bağımlılık Analizi](how-to-create-group-machine-dependencies-agentless.md)için kullanılacak bir hesap oluşturduysanız, isteğe bağlı olarak burada kimlik bilgileri eklersiniz.
    - Bu özellikleri kullanmak istemiyorsanız, adımı atlamak için kaydırıcıya tıklayabilirsiniz. Amacınızı daha sonra dilediğiniz zaman tersine çevirebilirsiniz.
    - [Uygulama bulma](migrate-support-matrix-vmware.md#application-discovery-requirements)hesabında veya [aracısız bağımlılık Analizi](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless)için gereken izinleri gözden geçirin.

5. VM bulmayı **başlatmak için bulmayı Başlat**' a tıklayın. Bulma işlemi başarılı bir şekilde başlatıldıktan sonra, tablodaki vCenter Server IP adresine/FQDN 'ye karşı bulma durumunu kontrol edebilirsiniz.

Bulma işlemi aşağıdaki gibi çalışmaktadır:
- Keşfedilen VM meta verilerinin portalda görünmesi 15 dakika içinde sürer.
- Yüklenen uygulamaların, rollerin ve özelliklerin bulunması biraz zaman alır. Süre, bulunan VM sayısına bağlıdır. 500 VM 'Ler için, uygulama envanterinin Azure geçişi portalında görünmesi yaklaşık bir saat sürer.
- VM 'lerin bulunması tamamlandıktan sonra, portaldan istenen VM 'lerde aracısız bağımlılık analizini etkinleştirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- Azure VM 'lerine geçiş için [VMware VM 'Lerini değerlendirin](./tutorial-assess-vmware-azure-vm.md) .
- Bulma sırasında gereç tarafından toplanan [verileri gözden geçirin](migrate-appliance.md#collected-data---vmware) .