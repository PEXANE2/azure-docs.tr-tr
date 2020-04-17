---
title: VMware VM'leri aracı tabanlı Azure Geçiş Sunucusu Geçişi ile geçirin
description: Azure Geçişi ile VMware VM'lerin aracı tabanlı geçişini nasıl çalıştırılayarıştın öğrenin.
ms.topic: tutorial
ms.date: 03/09/2020
ms.custom: MVC
ms.openlocfilehash: 6855c3e81aece0358146608b6cf179fb923c54c8
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535341"
---
# <a name="migrate-vmware-vms-to-azure-agent-based"></a>VMware VM'leri Azure'a geçir (aracı tabanlı)

Bu makalede, Azure Geçiş Sunucusu Geçişi aracıyla aracı tabanlı geçiş kullanarak şirket içi VMware VM'leri Azure'a nasıl geçirilebilirsiniz.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Kaynak ortamı nı ayarlayın ve aracı tabanlı geçiş için bir Azure Geçir çoğaltma cihazı dağıtın.
> * Geçiş için hedef ortamı ayarlayın.
> * Çoğaltma ilkesi ayarlayın.
> * Çoğaltmayı etkinleştirin.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın.
> * Azure'a tam bir geçiş çalıştırın.

> [!NOTE]
> Öğreticiler, hızlı bir şekilde kavram kanıtı ayarlayabilmeniz için bir senaryo için en basit dağıtım yolunu gösterir. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Ayrıntılı talimatlar için VMware değerlendirmesi ve geçişi için Nasıl Yap'ı gözden geçirin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

VM'leri Azure'a geçirmeden önce Azure Geçir Sunucu Değerlendirmesi ile VMware VM değerlendirmesini denemenizi öneririz. Bir değerlendirmeyi aşağıdaki gibi ayarlayın:

1. Azure ve [VMware'i](tutorial-prepare-vmware.md) değerlendirmeye hazırlamak için öğreticiyi izleyin.
2. Ardından, değerlendirme için bir Azure Geçiş cihazı ayarlamak ve VM'leri keşfetmek ve değerlendirmek için [bu öğreticiyi](tutorial-assess-vmware.md) izleyin.


Bir değerlendirme denemenizi öneririz, ancak VM'leri geçirmeden önce bir değerlendirme çalıştırmanız gerekmez.

## <a name="migration-methods"></a>Geçiş yöntemleri

Azure Geçir Sunucu Geçişi aracını kullanarak VMware VM'leri Azure'a geçirebilirsiniz. Bu araç VMware VM geçişi için birkaç seçenek sunar:

- Aracısız çoğaltma. Üzerlerine bir şey yüklemenize gerek kalmadan VM'leri geçirin.
- Aracı tabanlı geçiş. veya çoğaltma. Çoğaltma için VM'ye bir aracı (Mobilite hizmetleri aracısı) yükleyin.

Aracısız veya aracı tabanlı geçiş kullanmak isteyip istemediğinize karar vermek için şu makaleleri inceleyin:

- VMware geçiş seçenekleri [hakkında bilgi edinin.](server-migrate-overview.md)
- [Geçiş yöntemlerini karşılaştırın.](server-migrate-overview.md#compare-migration-methods)
- Aracısız geçişi denemek için [bu makaleyi izleyin.](tutorial-migrate-vmware.md)



## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. VMware geçiş mimarisini [gözden geçirin.](migrate-architecture.md)
2. Azure hesabınıza Sanal Makine Katılımcısı rolü atandığından emin olun, böylece aşağıdakilere izin verebilirsiniz:

    - Seçilen kaynak grubunda sanal makine oluşturma.
    - Seçilen sanal ağda sanal makine oluşturma.
    - Azure yönetilen bir diske yazın. 

3. [Azure ağı ayarlayın.](../virtual-network/manage-virtual-network.md#create-a-virtual-network) Şirket içi makineler Azure yönetilen disklere çoğaltılır. Geçiş için Azure'a geçemediğinizi, Azure VM'leri bu yönetilen disklerden oluşturulur ve geçiş ayarlarken belirttiğiniz bir Azure ağına katılır.


## <a name="prepare-azure"></a>Azure’u hazırlama

Azure Geçir Sunucu Değerlendirmesi ile zaten bir değerlendirme çalıştırdıysanız, bu adımları zaten tamamladıktan sonra bu bölümdeki yönergeleri atlayabilirsiniz. 

Bir değerlendirme çalıştırmadıysanız, Azure Geçiş Sunucusu Geçişi ile geçiş yapmadan önce Azure izinlerini ayarlamanız gerekir.

- **Proje oluşturma**: Azure hesabınızın bir Azure Geçiş projesi oluşturmak için izinlere ihtiyacı vardır. 
- **Azure Geçir çoğaltma cihazını kaydedin**: Çoğaltma cihazı Azure hesabınızda bir Azure Active Directory uygulaması oluşturur ve kaydeder. Bunun için temsilci izinleri.
- **Anahtar Kasası Oluşturma**: Azure Geçir Sunucusu Geçişi'ni kullanarak VMware VM'leri geçirmek için Azure Geçiş, aboneliğinizdeki çoğaltma depolama hesabının erişim anahtarlarını yönetmek için kaynak grubunda bir Anahtar Kasası oluşturur. Kasa oluşturmak için, Azure Geçiş projesinin bulunduğu kaynak grubunda rol atama izinlerine ihtiyacınız vardır. 


### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portalında aboneliği açın ve **Access denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi**Denetle'de, ilgili hesabı bulun ve izinleri görüntülemek için bu hesabı tıklatın.
3. **Katkıda Bulunan** veya **Sahip** İzni niz olmalıdır.
    - Ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi sizsiniz.
    - Abonelik sahibi değilseniz, rolü atamak için sahibiyle birlikte çalışın.

### <a name="assign-permissions-to-register-the-replication-appliance"></a>Çoğaltma cihazını kaydetmek için izin atama

Aracı tabanlı geçiş için, hesabınızda bir Azure AD uygulaması oluşturmak ve kaydetmek için Azure Geçiş Sunucusu Geçişi için temsilci izinleri. Aşağıdaki yöntemlerden birini kullanarak izinatlayabilirsiniz:

- Bir kiracı/global yönetici, Azure AD uygulamaları oluşturmak ve kaydetmek için kiracıdaki kullanıcılara izin verebilir.
- Kiracı/genel yönetici, Uygulama Geliştiricisi rolünü (izinlere sahip) hesaba atayabilir.

Bu kayda değer:

- Uygulamaların abonelikte yukarıda açıklananlar dışında başka erişim izinleri yoktur.
- Bu izinlere yalnızca yeni bir çoğaltma cihazı kaydettirdiğinizde ihtiyacınız vardır. Çoğaltma cihazı kurulduktan sonra izinleri kaldırabilirsiniz. 


#### <a name="grant-account-permissions"></a>Hesap izinlerini hibe

Kiracı/genel yönetici aşağıdaki gibi izinverebilir

1. Azure AD'de, kiracı/global yönetici **Azure Etkin Dizin** > **Kullanıcıları Kullanıcı** > **Ayarları'na**gitmelidir.
2. Yönetici, **Uygulama kayıtlarını** **Evet**olarak ayarlamalıdır.

    ![Azure AD izinleri](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Bu, hassas olmayan varsayılan bir ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Uygulama Geliştiricisi rolünü atama 

Kiracı/genel yönetici, Uygulama Geliştiricisi rolünü bir hesaba atayabilir. [Daha fazla bilgi edinin](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="assign-permissions-to-create-key-vault"></a>Key Vault oluşturmak için izinler atama

Azure Geçiş projesinin bulunduğu kaynak grubuna rol atama izinleri aşağıdaki gibi atama:

1. Azure portalındaki kaynak grubunda **Access denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi**Denetle'de, ilgili hesabı bulun ve izinleri görüntülemek için bu hesabı tıklatın. **Sahibi** (veya **Katılımcı** ve **Kullanıcı Erişim Yöneticisi)** izinlerine ihtiyacınız vardır.
3. Gerekli izinlere sahip değilseniz, bunları kaynak grubu sahibinden isteyin. 


## <a name="prepare-on-premises-vmware"></a>Şirket içi VMware’leri hazırlama

### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Azure Geçir Sunucu Geçişi'nin VMware sunucularına şu şekilde erişmesi gerekir:

- VM'leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Diskleri oluşturma ve kaldırma ve VM’leri çalıştırma gibi işlemleri gerçekleştirebilen bir hesabınızın olması gerekir.

Hesabı aşağıdaki gibi oluşturun:

1. Ayrılmış bir hesap kullanmak için, rolü vCenter düzeyinde oluşturun. Role **Azure_Site_Recovery** gibi bir ad verin.
2. Role aşağıdaki tabloda özetlenen izinleri atayın.
3. vCenter sunucusu veya vSphere ana bilgisayarında bir kullanıcı oluşturun. Rolü kullanıcıya atayın.

#### <a name="vmware-account-permissions"></a>VMware hesap izinleri

**Görev** | **Rol/İzinler** | **Şey**
--- | --- | ---
**VM bulma** | En az bir salt okunur kullanıcı<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak **için, Yayı alt nesneye** (vSphere ana bilgisayarlar, veri depoları, VM'ler ve ağlar) **Access'le erişim yok** rolünü atayın.
**Tam çoğaltma, yük devretme, yeniden çalışma** |  Gerekli izinlere sahip bir rol (Azure_Site_Recovery) oluşturup rolü VMware kullanıcısı veya grubuna atayın<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Azure_Site_Recovery<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak **için, Yayı alt nesneye** (vSphere ana bilgisayarlar, veri depoları, VMsa, nd ağları) **Yayı** ile Erişim Yok rolünü atayın.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz makinelerde Mobility hizmeti yüklü olmalıdır.

- Azure Geçir çoğaltma cihazı, bir makine için çoğaltmayı etkinleştirdiğinizde bu hizmetin itme yüklemesini yapabilir veya el ile veya yükleme araçlarını kullanarak yükleyebilirsiniz.
- Bu öğreticide, Mobility hizmetini göndererek yükleme yoluyla yükleyeceğiz.
- Anında yükleme için, Azure Geçiş Sunucusu Geçişi'nin VM'ye erişmek için kullanabileceği bir hesap hazırlamanız gerekir. Mobilite hizmetini el ile yüklemezseniz, bu hesap yalnızca itme yüklemesi için kullanılır.

Hesabı aşağıdaki gibi hazırlayın:

1. VM üzerinde yükleme izinleri ile bir etki alanı veya yerel hesap hazırlayın.
2. Windows VM'ler için, bir etki alanı hesabı kullanmıyorsanız, **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** altında, kayıt defterinde bir değere sahip Olan DWORD girişi **LocalAccountTokenFilterPolicy**ekleyerek yerel makinede Uzaktan Kullanıcı Erişimi denetimini devre dışı bırakın
3. Linux VM'leri için kaynak Linux sunucusunda bir kök hesap hazırlayın.


### <a name="check-vmware-requirements"></a>VMware gereksinimlerini denetleme

VMware sunucuları ve VM'lerin Azure'a geçiş gereksinimlerine uyduklarına emin olun. 


> [!NOTE]
> Azure Geçiş Sunucusu Geçişi ile aracı tabanlı geçiş, Azure Site Kurtarma hizmetinin özelliklerine dayanır. Bazı gereksinimler Site Kurtarma belgelerine bağlanabilir.

1. VMware sunucusu gereksinimlerini [doğrulayın](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers).
2. [Doğrula](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms) VM geçiş gereksinimlerini destekler.
3. VM ayarlarını doğrulayın. Azure'da çoğaltacağınız şirket içi [VM'ler Azure VM gereksinimlerine](migrate-support-matrix-vmware-migration.md#azure-vm-requirements)uygun olmalıdır.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure Geçir Sunucusu Geçişi aracını ekleme

VMware VM'leri değerlendirmek için öğreticiyi izlemediyseniz, bir Azure Geçir projesi ayarlayın ve ardından Azure Geçir Sunucusu Geçişi aracını ekleyin:

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.

    ![Azure Geçiş'i ayarlama](./media/tutorial-migrate-vmware-agent/azure-migrate-search.png)

3. **Genel Bakış** bölümünde **Sunucuları değerlendir ve geçir**’e tıklayın.
4. **Sunucuları Keşfedin, değerlendirin ve geçirin,** **Sunucuları Değerlendir'i tıklatın ve sunucuları geçirin.**

    ! [Sunucuları keşfedin ve değerlendirin] (./media/tutorial-migrate-vmware-agent/assess-migrate.png

1. **Sunucuları bul, değerlendir ve geçir** bölümünde **Araç ekle**’ye tıklayın.
2. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
3. **Proje Ayrıntıları** bölümünde proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtip **İleri**’ye tıklayın. Kamu ve [hükümet bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafyaları gözden [geçirin.](migrate-support-matrix.md#supported-geographies-public-cloud)

    ![Azure Geçiş projesi oluşturma](./media/tutorial-migrate-vmware-agent/migrate-project.png)


4. **Değerlendirme aracını seç'te,** **şimdi için bir değerlendirme aracı ekleyerek Atla'yı** > seçin**İleri**.
5. **Geçiş aracı seç'te**Azure **Geçir: Sunucu Geçiş İlerletmek** > **İleri'yi**seçin.
6. **İnceleme + araç ekleme** bölümünde ayarları gözden geçirip **Araç ekle**’ye tıklayın
7. Aracı ekledikten sonra, Azure Geçiş projesinde görünür > **Sunucular** > **Geçiş araçları.**

## <a name="set-up-the-replication-appliance"></a>Çoğaltma cihazını ayarlama

Geçişin ilk adımı çoğaltma cihazını kurmaktır. Çoğaltma cihazı, bu bileşenleri barındıran tek, son derece kullanılabilir, şirket içi VMware VM'dir:

- **Yapılandırma sunucusu**: Yapılandırma sunucusu şirket içi ve Azure arasındaki iletişimi koordine eder ve veri çoğaltmayı yönetir.
- **İşlem sunucusu**: İşlem sunucusu bir çoğaltma ağ geçidi gibi davranır. Çoğaltma verilerini alır; önbelleğe alma, sıkıştırma ve şifreleme ile optimize eder ve Azure'daki bir önbellek depolama hesabına gönderir. İşlem sunucusu ayrıca çoğaltmak istediğiniz VM'lere Mobilite Hizmeti aracısını yükler ve şirket içi VMware VM'lerin otomatik keşfini gerçekleştirir.


Çoğaltma cihazını birkaç şekilde ayarlayabilirsiniz.

- İndirilen Açık Sanallaştırma Uygulaması (OVA) şablonuyla ayarlayın. Şablonu VMware'e aktarın ve çoğaltma cihazı VM'yi oluşturursunuz. Bu, bu öğreticide kullanılan yöntemdir.
- Bir komut dosyası yla ayarlayın.

### <a name="download-the-replication-appliance-template"></a>Çoğaltma cihazı şablonu indirin

Şablonu aşağıdaki gibi indirin:

1. Azure Geçir projesinde Geçiş **Hedefleri**altındaki **Sunucular'ı** tıklatın.
2. **Azure Geçir - Sunucularda** > Azure**Geçir: Sunucu Geçişi**, **Keşfet'i**tıklatın.

    ![VM'leri bulma](./media/tutorial-migrate-vmware-agent/migrate-discover.png)

3. **Discover makinelerinde** >  **Yes, with VMWare vSphere hypervisor****makineleriniz sanallaştırılmış mı?**
4. Nasıl **geçiş yapmak istiyorsunuz?** **Using agent-based replication**
5. **Hedef bölgesinde,** makineleri geçirmek istediğiniz Azure bölgesini seçin.
6. **Geçiş için hedef bölgenin bölge adı olduğunu onayla'yı**seçin.
7. **Kaynak Oluştur'u**tıklatın. Bu, arka planda bir Azure Site Kurtarma kasası oluşturur.
    - Bu düğmeyi tıklattıktan sonra bu projenin hedef bölgesini değiştiremezsiniz.
    - Sonraki tüm göçler bu bölgeye dir.

    ![Kurtarma Hizmetleri kasası oluşturma](./media/tutorial-migrate-vmware-agent/create-resources.png)

8. **Yeni bir çoğaltma cihazı yüklemek istiyor musunuz?** **Install a replication appliance**
9. Çoğaltma cihazını indirmek için **İndir'i**tıklatın. Bu, cihazı çalıştıran yeni bir VMware VM oluşturmak için kullandığınız bir OVF şablonu indirir.
    ![Karşıdan yükleme OVA](./media/tutorial-migrate-vmware-agent/download-ova.png)
10. Kaynak grubunun adını ve Kurtarma Hizmetleri kasasını not edin. Cihaz dağıtımı sırasında bunlara ihtiyacınız vardır.


### <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

OVF şablonu indirdikten sonra, Windows Server 2016 çalıştıran bir VMware VM çoğaltma uygulamasını oluşturmak için vmware içine içe aktarın.

1. VMWare vSphere İstemcisi ile VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek **OVF Şablonu Dağıtma Sihirbazı**’nı başlatın. 
3. **Kaynak**Seç'te, indirilen OVF'nin konumunu girin.
4. **Gözden Geçir ayrıntılarında,** **İleri'yi**seçin.
5. **Ad ve klasör seç** ve yapılandırmayı seç,varsayılan ayarları kabul **edin.**
6. **Select depolama** > da**seçin sanal disk formatı**, en iyi performans için **Kalın Karşılık Eager Zeroed**seçin .
7. Sihirbaz sayfalarının geri kalan kısmında varsayılan ayarları kabul edin.
8. **Tamamlanmak üzere hazır**olarak, Varsayılan ayarlarla VM'yi ayarlamak için Dağıtım > dan sonra**Güç'ü bitir'i**seçin. **Power on after deployment**

   > [!TIP]
   > Ek bir NIC eklemek istiyorsanız, Dağıtım > **Bittikten**sonra **Güç'ün açık olmasını.** Varsayılan olarak, şablon tek bir NIC içerir. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

### <a name="kick-off-replication-appliance-setup"></a>Çoğaltma cihazı kurulumu na başlama

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme bittikten sonra yönetici parolasını kullanarak Yönetici olarak VM'de oturum açın.
4. İlk oturum açğınızda, çoğaltma cihazı kurulum aracı (Azure Site Kurtarma Yapılandırma Aracı) birkaç saniye içinde başlar.
5. Cihazı Azure Geçiş Sunucusu Geçişi'ne kaydetmek için kullanmak üzere bir ad girin. Ardından **İleri**’ye tıklayın.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin.
7. Aracı, cihazı tanımlamak için bir Azure AD uygulamasını kaydetmeyi bitirmesini bekleyin. Cihaz yeniden başlatılır.
1. Makinede tekrar oturum açın. Birkaç saniye içinde, Yapılandırma Sunucusu Yönetim Sihirbazı otomatik olarak başlar.

### <a name="register-the-replication-appliance"></a>Çoğaltma cihazını kaydedin

Çoğaltma cihazını kurmayı ve kaydetmeyi bitirin.

1. Configuration Server Management Sihirbazı'nda **Kurulum bağlantısı'nı**seçin.
2. Çoğaltma cihazının VM keşfi için kullandığı NIC'i (varsayılan olarak yalnızca bir NIC vardır) seçin ve kaynak makinelerde Mobilite hizmetinin itme yüklemesini yapın.
3. Çoğaltma cihazının Azure ile bağlantı için kullandığı NIC'yi seçin. Ardından **Kaydet'i**seçin. Bu ayar, yapılandırıldıktan sonra değiştiremezsiniz.
4. Cihaz bir proxy sunucusunun arkasında bulunuyorsa, proxy ayarlarını belirtmeniz gerekir.
    - Proxy adını , **http://ip-address**veya **http://FQDN**. olarak belirtin HTTPS proxy sunucuları desteklenmez.
5. Abonelik, kaynak grupları ve kasa ayrıntıları istendiğinde, cihaz şablonu indirdiğinizde belirttiğiniz ayrıntıları ekleyin.
6. **Üçüncü taraf yazılımı yükleyin** bölümünde lisans sözleşmesini kabul edin. MySQL Server’ı yüklemek için **İndir ve Yükle** seçeneğini belirleyin.
7. **VMware PowerCLI’yi Yükle** seçeneğini belirleyin. Bunu yapmadan önce tüm tarayıcı pencerelerinin kapalı olduğundan emin olun. Daha sonra **Devam** seçeneğini belirleyin.
8. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
9. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
10. VMware keşfi için [oluşturduğunuz](#prepare-an-account-for-automatic-discovery) hesabın kimlik bilgilerini girin. **Devam Ekle'yi** > **Continue**seçin.
11. **Sanal makine kimlik bilgilerini yapılandırıldığında,** VM'ler için çoğaltmayı etkinleştirdiğinizde Mobilithizmetinin itme yüklemesi için [oluşturduğunuz](#prepare-an-account-for-mobility-service-installation) kimlik bilgilerini girin.  
    - Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir.
    - Linux’ta kök hesap için bilgileri sağlayın.
12. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.


Çoğaltma cihazı kaydedildikten sonra, Azure Geçir Sunucusu Değerlendirmesi belirtilen ayarları kullanarak VMware sunucularına bağlanır ve VM'leri keşfeder. Keşfedilen**Discovered items**Öğeleri **Yönet'te** **Manage** > keşfedilen VM'leri Diğer sekmesinde görüntüleyebilirsiniz.


## <a name="replicate-vms"></a>Sanal makineleri çoğaltma

Şimdi, geçiş için VM'leri seçin.

> [!NOTE]
> En fazla 10 makineyi birlikte çoğaltabilirsiniz. Daha fazla çoğaltmanız gerekiyorsa, bunları aynı anda 10'luk gruplar halinde çoğaltın.

1. Azure Geçir projesinde **> Sunucular,** **Azure Geçir: Sunucu Geçişi**, **Çoğaltma'yı**tıklatın.

    ![Sanal makineleri çoğaltma](./media/tutorial-migrate-vmware-agent/select-replicate.png)

2. **Çoğalt**, > **Kaynak ayarları** > **Makineleriniz sanallaştırıldı mı?** bölümünde **Evet, VMware vSphere** ile seçeneğini belirleyin.
3. **Şirket içi cihazda,** ayarladığınız Azure Geçiş cihazının adını seçin.
4. **vCenter sunucusunda,** VM'leri yöneten vCenter sunucusunun veya VM'lerin barındırıldığı vSphere sunucusunun adını belirtin.
5. **Process Server'da**çoğaltma cihazının adını seçin.
6. **Konuk kimlik bilgilerinde,** Mobilite hizmetinin itme yüklemesi için kullanılacak VM yönetici hesabını belirtin. Sonra **İleri'yi tıklatın: Sanal makineler.**

    ![Sanal makineleri çoğaltma](./media/tutorial-migrate-vmware-agent/source-settings.png)

7. **Sanal Makineler'de**çoğaltmak istediğiniz makineleri seçin.

    - VM’ler için bir değerlendirme çalıştırırsanız değerlendirme sonuçlarından VM boyutlandırma ve disk türü (premium/standart) önerilerini uygulayabilirsiniz. Bunu yapmak için **Azure Geçişi değerlendirmesinden geçiş ayarları içe aktarılsın mı?** bölümünde **Evet**’i seçin.
    - Bir değerlendirme çalıştırmadıysanız veya değerlendirme ayarlarını kullanmak istemiyorsanız **Hayır**’ı seçin.
    - Değerlendirmeyi kullanmayı seçtiyseniz VM grubunu ve değerlendirme adını belirleyin.

8. Geçirmek istediğiniz her VM'yi denetleyin. Sonra **İleri'yi tıklatın: Hedef ayarları.**
9. **Hedef ayarları**’nda aboneliği ve geçiş yapacağınız hedef bölgeyi seçin. Daha sonra Azure VM’lerinin geçişten sonra bulunacağı kaynak grubunu belirtin.
10. **Sanal Ağ**’da Azure VM’lerinin geçişten sonra katılacağı Azure sanal ağını/alt ağını seçin.
11. **Azure Hibrit Avantajı**’nda:

    - Azure Hibrit Avantajı’nı uygulamak istemiyorsanız **Hayır**’ı seçin. Ardından **İleri**’ye tıklayın.
    - Etkin Yazılım Güvencesi veya Windows Server abonelikleri kapsamında olan Windows Server makineleriniz varsa ve avantajı geçirdiğiniz makinelere uygulamak istiyorsanız **Evet**’i seçin. Ardından **İleri**’ye tıklayın.

12. **İşlem** bölümünde VM adını, boyutunu, İşletim Sistemi disk türünü ve kullanılabilirlik kümesini gözden geçirin. VM’ler [Azure gereksinimleriyle](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms)uyumlu olmalıdır.

    - **VM boyutu**: Değerlendirme önerileri kullanıyorsanız, VM boyutu açılır bırakma önerilen boyutu içerir. Aksi takdirde Azure Geçişi, Azure aboneliğindeki en yakın eşleşmeye göre bir boyut seçer. Alternatif olarak **Azure VM boyutu **’nda el ile bir boyut seçin. 
    - **İşletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim Sistemi diski, işletim sistemi önyükleyiciye ve yükleyiciye sahip disktir. 
    - **Kullanılabilirlik kümesi**: VM geçişten sonra bir Azure kullanılabilirlik kümesinde olması gerekiyorsa, kümesi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

13. **Diskler'de,** VM disklerinin Azure'da çoğaltılıp çoğaltılmayacağını belirtin ve Azure'da disk türünü (standart SSD/HDD veya premium yönetilen diskler) seçin. Ardından **İleri**’ye tıklayın.
    - Diskleri çoğaltmadan çıkarabilirsiniz.
    - Diskleri çıkarırsanız bu diskler geçişten sonra Azure VM’de bulunmaz. 

14. **Çoğaltmayı gözden geçir ve başlat** bölümünde ayarları gözden geçirin ve sunuculara yönelik ilk çoğaltmayı başlatmak için **Çoğalt** üzerine tıklayın.

> [!NOTE]
> Çoğaltma başlamadan önce çoğaltma ayarlarını istediğiniz zaman güncelleştirebilirsiniz,**Çoğaltma makinelerini** **yönetin.** >  Çoğaltma başladıktan sonra ayarlar değiştirilemez.




## <a name="track-and-monitor"></a>İzleme ve izleme

- Bir Başlat Çoğaltma işi **çoğaltma'yı** tıklattığınızda iş başlar. 
- Çoğaltmayı Başlat işi başarıyla bittiğinde, makineler ilk çoğaltmalarını Azure'a başlar.
- İlk çoğaltma bittikten sonra, delta çoğaltma başlar. Şirket içi disklerde yapılan artımlı değişiklikler, Azure'daki yineleme disklerine düzenli olarak çoğaltılır.


Portal bildirimlerinde iş durumunu izleyebilirsiniz.

![İşi takip et](./media/tutorial-migrate-vmware-agent/jobs.png)

**Azure Geçişi: Sunucu Geçişi'nde** **çoğaltma sunucularını** tıklatarak çoğaltma durumunu izleyebilirsiniz.
![Çoğaltmayı izleme](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Delta çoğaltma başladığında, Azure'a tam bir geçiş çalıştırmadan önce VM'ler için bir test geçişi çalıştırabilirsiniz. Bunu her makine için en az bir kez, geçirmeden önce yapmanızı öneririz.

- Bir test geçişi, çalışmaya devam eden ve çoğaltmaya devam eden şirket içi makineleri etkilemeden, geçişin beklendiği gibi çalışacağını denetler. 
- Test geçişi, çoğaltılan verileri kullanarak (genellikle Azure aboneliğinizde üretim dışı bir VNet'e geçiş yaparak) bir Azure VM oluşturarak geçişi simüle eder.
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce tüm sorunları gidermek için çoğaltılan Azure VM testini kullanabilirsiniz.

Bir test geçişini aşağıdaki gibi yapın:


1. **Geçiş hedefleri** > **Sunucularında** > Azure**Geçir: Sunucu Geçişi**, **geçirilen sunucuları sına'yı**tıklatın.

     ![Geçirilen sunucuları test etme](./media/tutorial-migrate-vmware-agent/test-migrated-servers.png)

2. Test edilecek VM’yi sağ tıklayıp **Test geçişi** üzerine tıklayın.

    ![Test geçişi](./media/tutorial-migrate-vmware-agent/test-migrate.png)

3. **Test Geçişi** bölümünde, Azure VM’nin geçişten sonra bulunacağı Azure VNet’i seçin. Üretim dışı bir VNet kullanmanızı öneririz.
4. **Test Geçişi** işlemi başlar. Portal bildirimlerinde işi izleyin.
5. Geçiş bittikten sonra geçirilen Azure VM’yi, Azure portalında **Sanal Makineler** bölümünde görüntüleyin. Makine adında **Testi** şeklinde bir son ek vardır.
6. Test tamamlandıktan sonra **Makineleri çoğaltma** bölümünde Azure VM’yi sağ tıklayıp **Test geçişini temizle** üzerine tıklayın.

    ![Geçişi temizleme](./media/tutorial-migrate-vmware-agent/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçişinin beklendiği gibi çalıştığını doğruladıktan sonra, şirket içi makineleri geçirebilirsiniz.

1. Azure Geçir projesinde **> Sunucular** > **Azure Geçir: Sunucu Geçişi**, **sunucuları çoğaltma'yı**tıklatın.

    ![Sunucuları çoğaltma](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

2. **Makineleri çoğaltma** bölümünde VM > **Geçir** üzerine sağ tıklayın.
3. **Geçiret** > **Sanal makineleri kapatın ve veri kaybı olmadan planlı geçiş gerçekleştirin**, **Evet** > **Tamam'ı**seçin.
    - Varsayılan olarak Azure Geçir, en az veri kaybı nı sağlamak için şirket içi VM'yi kapatır. 
    - VM’yi kapatmak istemiyorsanız, **Hayır** seçeneğini belirleyin
4. VM için bir geçiş işlemi başlar. Azure bildirimlerinde işlemi izleyin.
5. İşlem bittikten sonra **Sanal Makineler** sayfasında VM’yi görüntüleyebilir ve yönetebilirsiniz.

## <a name="complete-the-migration"></a>Geçişi tamamlama

1. Geçiş yapıldıktan sonra VM > **Stop geçişine**sağ tıklayın. Bu aşağıdakileri yapar:
    - Şirket içi makinenin çoğaltmasını durdurur.
    - Makineyi Azure Geçiş: Sunucu Geçişi'nde **çoğaltma sunucuları** sayısından kaldırır.
    - VM için çoğaltma durumu bilgilerini temizler.
2. Azure VM [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) aracısını geçirilen makinelere yükleyin.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure VM örneğine gelen trafiği azledin.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Şirket içi
    - Uygulama trafiğini, geçirilen Azure sanal makinesi örneğinde çalıştırılan uygulamaya taşıyın.
    - Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
    - Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
    - Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin.
- Geçişten sonra Azure VM ayarlarını değiştirin:
    - [Azure VM aracısı](../virtual-machines/extensions/agent-windows.md), Azure Yapı Denetleyicisi ile sanal makine etkileşimini yönetir. Azure Backup, Site Recovery ve Azure Güvenliği gibi bazı Azure hizmetleri için gereklidir. Mobilite Hizmeti yükleyici, VMare VM'lerini aracı tabanlı geçişle geçirterken, Windows makinelerine Azure VM aracısı yükler. Linux VM'lerinde, aracıyı geçişten sonra yüklemenizi öneririz.
    - Geçişten sonra Azure VM'den Mobilite hizmetini el ile kaldırın.
    - Geçişten sonra VMware araçlarını el ile kaldırın.
- Azure'da:
    - Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
    - Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
- İş sürekliliği/felaket kurtarma
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Azure Güvenlik Merkezi ile gelen trafik erişimini kilitleme ve sınırlama [- Tam zamanında yönetim.](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)
    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
    - [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.




 ## <a name="next-steps"></a>Sonraki adımlar

Azure Bulut Benimseme Çerçevesi'ndeki [bulut geçişi yolculuğunu](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) incele.
