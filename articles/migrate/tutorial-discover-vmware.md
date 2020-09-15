---
title: Azure geçişi sunucu değerlendirmesi ile VMware VM 'lerini bulma
description: Azure geçişi sunucu değerlendirmesi aracı ile şirket içi VMware VM 'lerini bulmayı öğrenin
ms.topic: tutorial
ms.date: 09/14/2020
ms.custom: mvc
ms.openlocfilehash: 55a7f15233d32f5ee60a57ad34e8b0177ca29ff3
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90064573"
---
# <a name="tutorial-discover-vmware-vms-with-server-assessment"></a>Öğretici: Sunucu değerlendirmesi ile VMware VM 'lerini bulma

Azure 'a geçiş sürecinizin bir parçası olarak şirket içi envanterinizi ve iş yüklerinizi keşfedeceksiniz. 

Bu öğreticide, basit bir Azure geçişi gereci kullanarak Azure geçişi: Sunucu değerlendirmesi aracı ile şirket içi VMware sanal makinelerini (VM) nasıl keşfedebileceğiniz gösterilmektedir. Gereci bir VMware VM olarak dağıtırsınız; VM ve performans meta verilerini sürekli olarak bulur, VM 'lerde çalışan uygulamalar ve VM bağımlılıkları.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

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
**vCenter Server/ESXi Konağı** | 5,5, 6,0, 6,5 veya 6,7 sürümünü çalıştıran bir vCenter Server gerekir.<br/><br/> VM 'Ler sürüm 5,5 veya üzerini çalıştıran bir ESXi konağında barındırılmalıdır.<br/><br/> VCenter Server, Gereç verilerini toplayabilmesi için TCP bağlantı noktası 443 ' de gelen bağlantılara izin verin.<br/><br/> Gereç, bağlantı noktası 443 ' de varsayılan olarak vCenter 'a bağlanır. VCenter sunucusu farklı bir bağlantı noktasını dinliyorsa, bulmayı başlatmak için gerecden sunucuya bağlandığınızda bağlantı noktasını değiştirebilirsiniz.<br/><br/> VM 'Leri barındıran Exsı sunucusunda, uygulama bulma için 443 numaralı TCP bağlantı noktasında gelen erişime izin verildiğinden emin olun.
**Elektrikli** | vCenter Server Azure geçiş gereci için bir VM ayırmak üzere kaynaklara ihtiyaç duyuyor:<br/><br/> - Windows Server 2016<br/><br/> -32 GB RAM, sekiz vCPU ve yaklaşık 80 GB disk depolaması.<br/><br/> -Bir dış sanal anahtar ve VM 'nin doğrudan veya bir ara sunucu üzerinden internet erişimi.
**VM’ler** | Bu öğreticiyi kullanabilmeniz için Windows VM 'lerinin Windows Server 2016, 2012 R2, 2012 veya 2008 R2 çalıştırması gerekir.<br/><br/> Linux VM 'lerinin Red Hat Enterprise Linux 7/6/5, Ubuntu Linux 14.04/16.04, de, 7/8, Oracle Linux 6/7 veya CentOS 5/6/7 çalıştırıyor olması gerekir.<br/><br/> VM 'Ler, VMware araçları (10.2.0 'den sonraki bir sürüm) yüklü ve çalışır olmalıdır.<br/><br/> Windows VM 'lerde, Windows PowerShell 2,0 veya sonraki bir sürümü yüklenmelidir.


## <a name="prepare-an-azure-user-account"></a>Azure Kullanıcı hesabı hazırlama

Azure geçişi projesi oluşturmak ve Azure geçişi gerecini kaydettirmek için, şu bir hesaba sahip olmanız gerekir:
- Azure aboneliğinde katkıda bulunan veya sahip izinleri.
- Azure Active Directory uygulamaları kaydetme izinleri.

Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz. Abonelik sahibi değilseniz, izinleri aşağıdaki şekilde atamak için sahibiyle birlikte çalışın:

1. Azure portal, "abonelikler" araması yapın ve **Hizmetler**altında **abonelikler**' i seçin.

    ![Azure aboneliğini aramak için arama kutusu](./media/tutorial-discover-vmware/search-subscription.png)

2. **Abonelikler** sayfasında, Azure geçişi projesi oluşturmak istediğiniz aboneliği seçin. 
3. Abonelikte **erişim denetimi (IAM)**  >  **erişimi denetle**' yi seçin.
4. **Erişimi denetle**' de ilgili Kullanıcı hesabını arayın.
5. **Rol ataması Ekle**' de, **Ekle**' ye tıklayın.

    ![Erişimi denetlemek ve rol atamak için bir kullanıcı hesabı arayın](./media/tutorial-discover-vmware/azure-account-access.png)

6. **Rol ataması Ekle**' de, katkıda bulunan veya sahip rolünü seçin ve hesabı (örneğimizde azmigrateuser) seçin. Daha sonra **Kaydet**'e tıklayın.

    ![Hesaba rol atamak için rol ataması Ekle sayfasını açar](./media/tutorial-discover-vmware/assign-role.png)

7. Portalda, kullanıcılar için arama yapın ve **Hizmetler**altında **Kullanıcılar**' ı seçin.
8. **Kullanıcı ayarları**' nda, Azure AD kullanıcılarının uygulamaları kaydedebildiğini doğrulayın (varsayılan olarak **Evet** ' e ayarlanır).

    ![Kullanıcıların Active Directory uygulamalar kaydedebildiğini Kullanıcı ayarlarında doğrula](./media/tutorial-discover-vmware/register-apps.png)


## <a name="prepare-vmware"></a>VMware 'yi hazırlama

VCenter Server, gerecin vCenter Server erişmek için kullanabileceği bir hesap oluşturun ve gerekli bağlantı noktalarının açık olduğundan emin olun. Ayrıca, gerecin sanal makinelere erişmek için kullanabileceği bir hesaba ihtiyacınız vardır. 

### <a name="create-an-account-to-access-vcenter"></a>VCenter 'a erişmek için bir hesap oluşturun

VSphere Web Istemcisinde bir hesabı aşağıdaki şekilde ayarlayın:

1. VSphere Web Istemcisinde yönetici ayrıcalıklarına sahip bir hesap kullanarak > **Yönetim**' i seçin.
2. **Erişim**, **SSO kullanıcıları ve grupları**' nı seçin.
3. **Kullanıcılar**' da yeni bir kullanıcı ekleyin.
4. **Yeni Kullanıcı**' da hesap ayrıntılarını yazın. Daha sonra, **Tamam**'a tıklayın.
5. **Genel izinler**' de, Kullanıcı hesabını seçin ve hesaba **salt okunurdur** rolünü atayın. Daha sonra, **Tamam**'a tıklayın.
6. **Roller** ' de > **salt oku** rolünü seçin ve **ayrıcalıklar**bölümünde **Konuk işlemler**' i seçin. Bu ayrıcalıklar, VM 'lerde çalışan uygulamaları keşfetmesi ve VM bağımlılıklarını analiz etmek için gereklidir.
 
    ![Salt okuma rolünde Konuk işlemlere izin vermek için onay kutusu](./media/tutorial-discover-vmware/guest-operations.png)


### <a name="create-an-account-to-access-vms"></a>VM 'Lere erişmek için bir hesap oluşturma

Gereç, uygulamaları bulmaya ve VM bağımlılıklarını çözümlemeye yönelik VM 'Lere erişir. Gereç, sanal makinelere hiçbir aracı yüklemez.

1. Gerecin Windows VM 'lerinde uygulama ve bağımlılıkları keşfetmesi için kullanabileceği bir yerel yönetici hesabı oluşturun.
2. Linux makineler için,/bin/netstat ve/bin/ls dosyalarında şu izinlere sahip bir kullanıcı hesabı olan kök ayrıcalıklarına sahip bir kullanıcı hesabı veya alternatif olarak bir kullanıcı hesabı oluşturun: CAP_DAC_READ_SEARCH ve CAP_SYS_PTRACE.

> [!NOTE]
> Azure geçişi, tüm Windows sunucularında App-Discovery için bir kimlik bilgisi ve tüm Linux makinelerde App-Discovery için bir kimlik bilgisi destekler.


## <a name="set-up-a-project"></a>Proje ayarlama

Yeni bir Azure geçişi projesi ayarlayın.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel bakış**bölümünde **proje oluştur**' u seçin.
5. **Proje oluştur**' da Azure aboneliğinizi ve kaynak grubunuzu seçin. Yoksa, bir kaynak grubu oluşturun.
6. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

   ![Proje adı ve bölgesi için kutular](./media/tutorial-discover-vmware/new-project.png)

7. **Oluştur**’u seçin.
8. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin.

**Azure geçişi: Sunucu değerlendirmesi** Aracı, varsayılan olarak yeni projeye eklenir.

![Varsayılan olarak eklenen sunucu değerlendirmesi aracını gösteren sayfa](./media/tutorial-discover-vmware/added-tool.png)


## <a name="set-up-the-appliance"></a>Gereci ayarlama

Bu öğreticide bir VMware VM 'si üzerindeki gereç ayarlanır.
- Gereç şablonunu indirip vCenter Server gereç sanal makinesini oluşturmak için içeri aktarabilirsiniz.
- Gereci oluşturduktan sonra ilk kez ayarlayın ve Azure geçişi projesine kaydedin.

> [!NOTE]
> Bir nedenden dolayı, şablonu kullanarak gereci ayarlayamazsınız, bunu bir PowerShell betiği kullanarak ayarlayabilirsiniz. [Daha fazla bilgi edinin](deploy-appliance-script.md#set-up-the-appliance-for-vmware).


### <a name="download-the-ova-template"></a>OVA şablonunu indirin

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mı?**, **VMware vSphere Hiper Yöneticisi ile Evet '** i seçin.
3. OVA şablon dosyasını indirmek için **İndir** ' i seçin.

   ![OVA dosyasını indirmek için seçim](./media/tutorial-discover-vmware/download-ova.png)


### <a name="deploy-the-appliance-vm"></a>Gereç sanal makinesini dağıtma

İndirilen dosyayı içeri aktarın ve bir VM oluşturun:

1. Oluşturduğunuz vCenter hesabını kullanarak vSphere Istemci konsolunda oturum açın.
2. **Sanal makineler** sekmesi > **Eylemler** menüsünde **ovf**şablonu Dağıt ' ı seçerek ovf şablonu Dağıtma Sihirbazı ' nı açın.
3. **BIR OVF şablonu seçin**bölümünde **yerel dosya**' yı seçin ve indirme şablonuna gidin. 
   ![OVF şablonu dağıtmak için vSphere menü komutu](./media/tutorial-discover-vmware/deploy-ovf.png)
3. Sihirbaz Ayarları Sihirbazı 'nın geri kalanını belirtin, dağıtım konumunu, sanal makinenin çalıştırılacağı konak/kümeyi ve depolama/ağ ayarlarını da içerir.
4. **Tamamlamaya hazırlanma**bölümünde **son**' a tıklayın. Sistem şablonu içeri aktarır ve dağıtır. 
5. Dağıtımdan sonra, Gereç VM 'si **sanal makineler** sekmesinde görünür.
6. **> sanal**makineyi seçin.
7. VMware uzak konsolunda **Lisans koşullarını**>, lisans koşullarını gözden geçirip kabul edin.
8. **Ayarları Özelleştir**' de, Kullanıcı hesabı için bir parola ayarlayın.


### <a name="verify-appliance-access-to-azure"></a>Azure 'a gereç erişimini doğrulama

Gereç VM erişimini denetleyin.

1. VM 'nin Azure 'a bağlanıp bağlanamadan emin olun.
    - Ortak bulutta, Gereç makinesi bu [URL 'lere](migrate-appliance.md#public-cloud-urls)bağlanabilmelidir.
    - Kamu bulutunda, VM 'nin bu [kamu URL 'lerine](migrate-appliance.md#government-cloud-urls)bağlanabilmesi gerekir.
2. Bu bağlantı noktalarının gereç makinesinde açık olduğundan emin olun:

    - TCP bağlantı noktası 3389 üzerinde gelen bağlantılara, Gereç ile Uzak Masaüstü bağlantılarına izin vermek için izin verin.
    - 44368 numaralı bağlantı noktası üzerinde gelen bağlantılara izin ver: https://<gereç-IP-veya-adı>:44368 ' yi kullanarak gereç Web uygulamasına uzaktan erişin.
    - Bağlantı noktası 443 ' de (HTTPS) giden bağlantılara izin vermek için Azure geçişi 'ne bulma ve performans meta verileri gönderme.

### <a name="configure-the-appliance"></a>Gereci yapılandırma

Gereci ilk kez ayarlayın.

1. Gereç sanal makinesinde oturum açın. 
    - Gereç Web uygulaması bir tarayıcıda otomatik olarak açılır.
    - Alternatif olarak, uygulamayı gereç masaüstünden uygulama kısayoluyla açabilirsiniz.
2. Azure geçiş gereci Web uygulamasını > **önkoşulları ayarlayın**, lisans koşullarını gözden geçirip kabul edin ve üçüncü taraf bilgilerini okuyun.
3. Gereç, sanal makinenin internet erişimi olup olmadığını ve VM 'deki sürenin internet saatine eşit olduğunu denetler.
    - Ara sunucu kullanıyorsanız, **proxy 'Yi ayarla** ' ya tıklayın ve proxy adresini ve bağlantı noktasını (veya biçiminde) belirtin http://ProxyIPAddress http://ProxyFQDN) . 
    - Ara sunucu kimlik doğrulaması gerektiriyorsa kimlik bilgilerini belirtin. Yalnızca HTTP proxy’si desteklenir.
4. Gereç, en son Azure geçiş güncelleştirmelerini yüklüyor ve VMWare vSphere sanal disk geliştirme seti 'nin (VDDK) yüklü olup olmadığını denetler.
5. VDDK 6,7 ' ü yüklemek için, VMware 'den indirmek için **İndir** ' e tıklayın ve indirilen ZIP dosyası içeriğini gereç üzerinde belirtilen konuma ayıklayın. Ardından **Doğrula ve Install**öğesine tıklayın.

    ![Gereç dağıtımı için Internet erişimini ve ayarlarını doğrulamak üzere Önkoşullar sayfası](./media/tutorial-discover-vmware/prerequisites.png)
  
3. VDDK yüklendikten sonra ayarları gözden geçirin ve **devam**' a tıklayın.

### <a name="register-the-appliance"></a>Gereci Kaydet 

1. **Azure geçişi Ile kaydolun**bölümünde **oturum aç**' ı seçin. Görünmüyorsa, tarayıcıda açılır pencere engelleyicisini devre dışı bırakmış olduğunuzdan emin olun.

    ![Gereci kaydetmeye başlamak için oturum aç ' a tıklayın](./media/tutorial-discover-vmware/register.png)

1. **Oturum aç** sayfasında, Azure Kullanıcı adı ve parolanızla oturum açın. PIN ile oturum açma desteklenmez.

    ![Gereci kaydetmek için oturum aç düğmesi](./media/tutorial-discover-vmware/sign-in.png)
1. Başarıyla oturum açtıktan sonra uygulamaya geri dönün.
1. **Azure geçişi Ile kaydolun**bölümünde, Azure geçişi projesinin oluşturulduğu aboneliği seçin ve ardından projeyi seçin.
1. Gereç için bir ad belirtin. Ad 14 karakter veya daha kısa bir harf olmalıdır.
3. **Kaydet**’i seçin. Sonra **devam**' a tıklayın. Bir ileti başarıyla kaydı gösterir.

    ![Abonelik, proje ve gereç adını doldur ve gereci Kaydet](./media/tutorial-discover-vmware/success-register.png)

## <a name="start-continuous-discovery"></a>Sürekli bulmayı Başlat

Gereç, VM 'Leri bulmaya yönelik vCenter Server bağlanmalıdır.

### <a name="connect-to-vcenter-server"></a>vCenter Server Bağlan

Gereç, VM 'Leri bulmaya yönelik vCenter Server bağlanmalıdır.

1. **VCenter Server belirtin**bölümünde vCenter Server adı (FQDN) veya IP adresini belirtin.
2. Varsayılan bağlantı noktasını bırakın veya vCenter Server dinlediği özel bir bağlantı noktası belirtin.
3. **Kullanıcı adı** ve **parola**' da, gerecin vCenter Server VM 'leri bulması için kullanacağı vCenter Server hesabı kimlik bilgilerini belirtin.
3. Gerecin vCenter Server bağlanabildiğini sağlamak için **bağlantıyı doğrula**' yı seçin. 

    ![vCenter Server bağlantısını doğrulamak için vCenter Server ayrıntıları ve düğmesi](./media/tutorial-discover-vmware/vcenter.png)

1. **VM 'lerde uygulamaları ve bağımlılıkları bul**' da, **kimlik bilgileri ekle**' ye tıklayın.
1. Uygulama bulma ve bağımlılık analizi için kullanmakta olduğunuz Windows/Linux kimlik bilgisini belirtin.
1. Bulma işlemini **başlatmak Için Kaydet ve bulmayı Başlat**' a tıklayın.

    ![Uygulama bulma ve bağımlılık analizi için kimlik bilgileri ekleyin ve sürekli bulma işlemini başlatın. ](./media/tutorial-discover-vmware/start-discovery.png)


Bulma başladıktan sonra:

- Keşfedilen VM meta verilerinin portalda görünmesi 15 dakika içinde sürer.
- Uygulama bulma işlemi biraz zaman alır. Süre, bulunan VM sayısına bağlıdır. 500 VM 'Ler için, uygulama envanterinin Azure geçişi portalında görünmesi yaklaşık bir saat sürer.



## <a name="verify-discovered-vms-in-the-portal"></a>Portalda bulunan VM 'Leri doğrulama

Bulmadan sonra, VM 'Lerin Azure portal göründüğünü doğrulayabilirsiniz:

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucuların**sayısını gösteren simgeyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM 'lerine geçiş için [VMware VM 'Lerini değerlendirin](tutorial-assess-vmware.md) .
- Bulma sırasında gereç tarafından toplanan [verileri gözden geçirin](migrate-appliance.md#collected-data---vmware) .
