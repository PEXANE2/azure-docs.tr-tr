---
title: Azure Geçiş ile makineleri fiziksel sunucu olarak Azure'a geçirin.
description: Bu makalede, Azure Geçiş ile fiziksel makinelerin Azure'a nasıl geçirilen açıklanmıştır.
ms.topic: tutorial
ms.date: 02/03/2020
ms.custom: MVC
ms.openlocfilehash: 51ce45b091fe2d8845963953c2c50cd7be618f58
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80297994"
---
# <a name="migrate-machines-as-physical-servers-to-azure"></a>Makineleri fiziksel sunucu olarak Azure'a geçirin

Bu makalede, Azure Geçiş:Sunucu Geçişi aracını kullanarak makineleri fiziksel sunucu olarak Azure'a nasıl geçirebilirsiniz gösterilmektedir. Makineleri fiziksel sunucular olarak ele alarak geçirme, bir dizi senaryoda yararlıdır:

- Şirket içi fiziksel sunucuları geçirin.
- Xen, KVM gibi platformlar tarafından sanallaştırılmış VM'leri geçirin.
- Hyper-V veya VMware VM'leri geçirin, bazı nedenlerden dolayı [Hyper-V](tutorial-migrate-hyper-v.md)veya [VMware](server-migrate-overview.md) geçişi için standart geçiş işlemini kullanamıyorsanız.
- Özel bulutlarda çalışan VM'leri geçirin.
- Amazon Web Hizmetleri (AWS) veya Google Bulut Platformu (GCP) gibi genel bulutlarda çalışan VM'leri geçirin.


[Azure Geçir,](migrate-services-overview.md) şirket içi uygulamalarınızın ve iş yüklerinizin keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar ve VM örneklerini Azure'a bulutlar. Hub, değerlendirme ve geçiş için Azure Geçiş araçlarının yanı sıra üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.


Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Azure Geçir Sunucusu Geçişi aracıyla Azure'u geçişe hazırlayın.
> * Geçiş yapmak istediğiniz makinelerin gereksinimlerini denetleyin ve makineleri keşfetmek ve Azure'a geçirmek için kullanılan Azure Geçir çoğaltma cihazı için bir makine hazırlayın.
> * Azure Geçir sunucusu geçişi aracını Azure Geçir hub'ına ekleyin.
> * Çoğaltma cihazını ayarlayın.
> * Mobilite hizmetini geçirmek istediğiniz makinelere yükleyin.
> * Çoğaltmayı etkinleştirin.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın.
> * Azure'a tam bir geçiş çalıştırın.

> [!NOTE]
> Öğreticiler, hızlı bir şekilde kavram kanıtı ayarlayabilmeniz için bir senaryo için en basit dağıtım yolunu gösterir. Öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Ayrıntılı yönergeler için Azure Geçiş Için Nasıl Nasıl Yap'ı inceleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. Geçiş mimarisini [gözden geçirin.](migrate-architecture.md)
2. Azure hesabınıza Sanal Makine Katılımcısı rolü atandığından emin olun, böylece aşağıdakilere izin verebilirsiniz:

    - Seçilen kaynak grubunda sanal makine oluşturma.
    - Seçilen sanal ağda sanal makine oluşturma.
    - Azure yönetilen bir diske yazın. 

3. [Azure ağı ayarlayın.](../virtual-network/manage-virtual-network.md#create-a-virtual-network) Azure'da çoğaltma yaptığınızda, Azure VM'leri oluşturulur ve bir Azure ağına katıldığınızda geçiş ayarladiğinizde belirtirsiniz.


## <a name="prepare-azure"></a>Azure’u hazırlama

Azure Geçiş Sunucusu Geçişi ile geçiş yapmadan önce Azure izinlerini ayarlayın.

- **Proje oluşturma**: Azure hesabınızın bir Azure Geçiş projesi oluşturmak için izinlere ihtiyacı vardır. 

### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portalında aboneliği açın ve **Access denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi**Denetle'de, ilgili hesabı bulun ve izinleri görüntülemek için bu hesabı tıklatın.
3. **Katkıda Bulunan** veya **Sahip** İzni niz olmalıdır.
    - Ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi sizsiniz.
    - Abonelik sahibi değilseniz, rolü atamak için sahibiyle birlikte çalışın.

## <a name="prepare-for-migration"></a>Geçiş için hazırlanma

### <a name="check-machine-requirements-for-migration"></a>Geçiş için makine gereksinimlerini denetleme

Makinelerin Azure'a geçiş gereksinimlerine uyduğunu unutmayın. 

> [!NOTE]
> Azure Geçiş Sunucusu Geçişi ile aracı tabanlı geçiş, Azure Site Kurtarma hizmetinin aracı tabanlı olağanüstü durum kurtarma özelliğiyle aynı çoğaltma mimarisine sahiptir ve kullanılan bileşenlerden bazıları aynı kod tabanını paylaşır. Bazı gereksinimler Site Kurtarma belgelerine bağlanabilir.

1. Fiziksel sunucu gereksinimlerini [doğrulayın.](migrate-support-matrix-physical-migration.md#physical-server-requirements)
2. VM ayarlarını doğrulayın. Azure'da çoğaltacağınız şirket içi makineler [Azure VM gereksinimlerine](migrate-support-matrix-physical-migration.md#azure-vm-requirements)uygun olmalıdır.


### <a name="prepare-a-machine-for-the-replication-appliance"></a>Çoğaltma cihazı için bir makine hazırlayın

Azure Geçir Sunucusu Geçişi, makineleri Azure'a çoğaltmak için bir çoğaltma cihazı kullanır. Çoğaltma cihazı aşağıdaki bileşenleri çalıştırın.

- **Yapılandırma sunucusu**: Yapılandırma sunucusu şirket içi ve Azure arasındaki iletişimi koordine eder ve veri çoğaltmayı yönetir.
- **İşlem sunucusu**: İşlem sunucusu bir çoğaltma ağ geçidi gibi davranır. Çoğaltma verilerini alır; önbelleğe alma, sıkıştırma ve şifreleme ile optimize eder ve Azure'daki bir önbellek depolama hesabına gönderir. 

Başlamadan önce, çoğaltma cihazını barındıracak bir Windows Server 2016 makinesi hazırlamanız gerekir. Makine [bu gereksinimlere](migrate-replication-appliance.md)uymalıdır. Cihaz korumak istediğiniz bir kaynak makineye monte edilmemelidir.


## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure Geçir Sunucusu Geçişi aracını ekleme

Bir Azure Geçir projesi ayarlayın ve ardından azure geçir sunucusu geçişi aracını ekleyin.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel Bakış** bölümünde **Sunucuları değerlendir ve geçir**’e tıklayın.
4. **Sunucuları Keşfedin, değerlendirin ve geçirin,** **Sunucuları Değerlendir'i tıklatın ve sunucuları geçirin.**

    ![Sunucuları keşfedin ve değerlendirin](./media/tutorial-migrate-physical-virtual-machines/assess-migrate.png)

5. **Sunucuları bul, değerlendir ve geçir** bölümünde **Araç ekle**’ye tıklayın.
6. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
7. **Proje Ayrıntıları'nda**proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtin ve **İleri'yi** tıklatın

    ![Azure Geçiş projesi oluşturma](./media/tutorial-migrate-physical-virtual-machines/migrate-project.png)

    Bu coğrafyalardan herhangi birinde bir Azure Geçiş projesi oluşturabilirsiniz.

    **Coğrafya** | **Bölge**
    --- | ---
    Asya | Güneydoğu Asya
    Avrupa | Kuzey Avrupa veya Batı Avrupa
    Amerika Birleşik Devletleri | Doğu ABD veya Batı Orta ABD

    Proje için belirtilen coğrafya yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. Gerçek geçiş için herhangi bir hedef bölge seçebilirsiniz.
8. **Değerlendirme aracını seç'te,** **şimdi için bir değerlendirme aracı ekleyerek Atla'yı** > seçin**İleri**.
9. **Geçiş aracı seç'te**Azure **Geçir: Sunucu Geçiş İlerletmek** > **İleri'yi**seçin.
10. **İnceleme + araç ekleme** bölümünde ayarları gözden geçirip **Araç ekle**’ye tıklayın
11. Aracı ekledikten sonra, Azure Geçiş projesinde görünür > **Sunucular** > **Geçiş araçları.**

## <a name="set-up-the-replication-appliance"></a>Çoğaltma cihazını ayarlama

Geçişin ilk adımı çoğaltma cihazını kurmaktır. Cihazın yükleyici dosyasını indirip [hazırladığınız makinede çalıştırın.](#prepare-a-machine-for-the-replication-appliance) Cihazı yükledikten sonra Azure Geçiş Sunucusu Geçişi'ne kaydettirin.


### <a name="download-the-replication-appliance-installer"></a>Çoğaltma cihazı yükleyicisini indirin

1. Azure > **Sunucular**projesinde , **Azure Geçir: Sunucu Geçişinde,** **Keşfet'i**tıklatın.

    ![VM'leri bulma](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

3. **Discover makinelerinde** >  **Not virtualized/Other****makineleriniz sanallaştırıldı mı?**
4. **Hedef bölgesinde,** makineleri geçirmek istediğiniz Azure bölgesini seçin.
5. **Geçiş için hedef bölgenin bölge adı olduğunu onayla'yı**seçin.
6. **Kaynak Oluştur'u**tıklatın. Bu, arka planda bir Azure Site Kurtarma kasası oluşturur.
    - Azure Geçiş Sunucusu Geçişi ile geçiş zaten ayarladıysanız, kaynaklar daha önce ayarlandığı için hedef seçenek yapılandırılamaz.
    - Bu düğmeyi tıklattıktan sonra bu projenin hedef bölgesini değiştiremezsiniz.
    - Sonraki tüm göçler bu bölgeye dir.

7. **Yeni bir çoğaltma cihazı yüklemek istiyor musunuz?** **Install a replication appliance**
9. **Çoğaltma cihazı yazılımını indirin ve kurun,** cihaz yükleyicisini ve kayıt anahtarını indirin. Cihazı kaydetmek için tuşa ihtiyacınız vardır. Anahtar indirildikten sonra beş gün boyunca geçerlidir.

    ![İndirme sağlayıcısı](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

10. Cihaz kurulum dosyasını ve anahtar dosyasını cihaz için oluşturduğunuz Windows Server 2016 makinesine kopyalayın.
11. Sonraki yordamda açıklandığı gibi çoğaltma cihazı kurulum dosyasını çalıştırın. Kurulum tamamlandıktan sonra, Appliance yapılandırma sihirbazı otomatik olarak başlatılır (Ayrıca cihazın masaüstünde oluşturulan cspsconfigtool kısayolu kullanarak sihirbazı el ile başlatabilirsiniz). Mobilit hizmetinin itme yüklemesi için kullanılacak hesap ayrıntılarını eklemek için sihirbazın Hesapları Yönet sekmesini kullanın. Bu eğitimde, çoğaltılacak makinelere Mobilite Hizmeti'ni el ile yükleyeceğiz, bu nedenle bu adımda sahte bir hesap oluşturun ve devam edin.

12. Cihaz kurulumdan sonra yeniden başlatıldıktan sonra **Discover makinelerinde,** **Select Configuration Server'da**yeni cihazı seçin ve **Finalize kaydını**tıklatın. Kayıt kesinleştirmek çoğaltma cihazı hazırlamak için son görevleri bir çift gerçekleştirir.

    ![Kaydı sonuçlandırın](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

Keşfedilen makinelerin Azure Geçiş Sunucusu Geçişi'nde görünmesi kaydın kesinleşmesinden sonra biraz zaman alabilir. VM'ler keşfedildikçe, **Keşfedilen sunucular** yükselir.

![Keşfedilen sunucular](./media/tutorial-migrate-physical-virtual-machines/discovered-servers.png)


## <a name="install-the-mobility-service"></a>Mobility hizmetini yükleme

Geçiş yapmak istediğiniz makinelerde Mobilite servis aracısını yüklemeniz gerekir. Aracı yükleyiciler çoğaltma cihazında mevcuttur. Doğru yükleyiciyi bulun ve geçiş yapmak istediğiniz her makineye aracıyı yükleyin. Bunu şu şekilde yapabilirsiniz:

1. Çoğaltma cihazında oturum açın.
2. **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**gidin.
3. Makine işletim sistemi ve sürümü için yükleyiciyi bulun. Desteklenen işletim sistemlerini gözden [geçirin.](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) 
4. Yükleyici dosyasını geçirmek istediğiniz makineye kopyalayın.
5. Cihazı dağıtırken oluşturulan parolaya sahip olduğunuzdan emin olun.
    - Dosyayı makinede geçici bir metin dosyasında saklayın.
    - Çoğaltma cihazındaki parolayı alabilirsiniz. Komut satırından, geçerli parolayı görüntülemek için **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe -v'yi** çalıştırın.
    - Şifreyi yenile. Bu bağlantı kopacak ve çoğaltma cihazı yeniden kayıt gerekir.


### <a name="install-on-windows"></a>Windows’ta yükleme

1. Yükleyici dosyasının içeriğini makinedeki yerel bir klasöre (örneğin C:\Temp) ayıklayın:

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```
2. Mobilite Hizmeti Yükleyicisini Çalıştırın:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```
3. Aracıyı çoğaltma cihazıyla kaydedin:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="install-on-linux"></a>Linux'ta yükleme

1. Montajcı tarball'ın içeriğini makinedeki yerel bir klasöre (örneğin /tmp/MobSvcInstaller) ayıklayın:
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```
2. Yükleyici komut dosyasını çalıştırın:
    ```
    sudo ./install -r MS -q
    ```
3. Aracıyı çoğaltma cihazıyla kaydedin:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="replicate-machines"></a>Makineleri çoğaltma

Şimdi, geçiş için makineleri seçin. 

> [!NOTE]
> En fazla 10 makineyi birlikte çoğaltabilirsiniz. Daha fazla çoğaltmanız gerekiyorsa, bunları aynı anda 10'luk gruplar halinde çoğaltın.

1. Azure Geçir projesinde **> Sunucular,** **Azure Geçir: Sunucu Geçişi**, **Çoğaltma'yı**tıklatın.

    ![Sanal makineleri çoğaltma](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. **Çoğaltma**, > **Kaynak ayarları** > **Makineleriniz sanallaştırılmış mı?** **Not virtualized/Other**
3. **Şirket içi cihazda,** ayarladığınız Azure Geçiş cihazının adını seçin.
4. **Process Server'da**çoğaltma cihazının adını seçin.
6. **Konuk kimlik bilgilerinde,** Mobilite hizmetini el ile yüklemek için kullanılacak sahte bir hesap belirtirsiniz (itme yüklemefiziksel olarak desteklenmez). Sonra **İleri'yi tıklatın: Sanal makineler.**

    ![Sanal makineleri çoğaltma](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)

7. **Sanal Makinelerde**, **bir değerlendirmeden geçiş ayarlarında içe aktarma ayarlarında?** **No, I'll specify the migration settings manually**
8. Geçirmek istediğiniz her VM'yi denetleyin. Sonra **İleri'yi tıklatın: Hedef ayarları.**

    ![VM'leri seçin](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)


9. **Hedef ayarları**’nda aboneliği ve geçiş yapacağınız hedef bölgeyi seçin. Daha sonra Azure VM’lerinin geçişten sonra bulunacağı kaynak grubunu belirtin.
10. **Sanal Ağ**’da Azure VM’lerinin geçişten sonra katılacağı Azure sanal ağını/alt ağını seçin.
11. **Azure Hibrit Avantajı**’nda:

    - Azure Hibrit Avantajı’nı uygulamak istemiyorsanız **Hayır**’ı seçin. Ardından **İleri**’ye tıklayın.
    - Etkin Yazılım Güvencesi veya Windows Server abonelikleri kapsamında olan Windows Server makineleriniz varsa ve avantajı geçirdiğiniz makinelere uygulamak istiyorsanız **Evet**’i seçin. Ardından **İleri**’ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

12. **İşlem** bölümünde VM adını, boyutunu, İşletim Sistemi disk türünü ve kullanılabilirlik kümesini gözden geçirin. VM’ler [Azure gereksinimleriyle](migrate-support-matrix-physical-migration.md#azure-vm-requirements)uyumlu olmalıdır.

    - **VM boyutu**: Varsayılan olarak, Azure Geçir Sunucusu Geçişi, Azure aboneliğindeki en yakın eşleşmeyi temel alarak bir boyut seçer. Alternatif olarak **Azure VM boyutu **’nda el ile bir boyut seçin. 
    - **İşletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim Sistemi diski, işletim sistemi önyükleyiciye ve yükleyiciye sahip disktir. 
    - **Kullanılabilirlik kümesi**: VM geçişten sonra bir Azure kullanılabilirlik kümesinde olması gerekiyorsa, kümesi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

    ![Bilgi işlem ayarları](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

13. **Diskler'de,** VM disklerinin Azure'da çoğaltılıp çoğaltılmayacağını belirtin ve Azure'da disk türünü (standart SSD/HDD veya premium yönetilen diskler) seçin. Ardından **İleri**’ye tıklayın.
    - Diskleri çoğaltmadan çıkarabilirsiniz.
    - Diskleri çıkarırsanız bu diskler geçişten sonra Azure VM’de bulunmaz. 

    ![Disk ayarları](./media/tutorial-migrate-physical-virtual-machines/disks.png)


14. **Çoğaltmayı gözden geçir ve başlat** bölümünde ayarları gözden geçirin ve sunuculara yönelik ilk çoğaltmayı başlatmak için **Çoğalt** üzerine tıklayın.

> [!NOTE]
> Çoğaltma başlamadan önce çoğaltma ayarlarını istediğiniz zaman güncelleştirebilirsiniz,**Çoğaltma makinelerini** **yönetin.** >  Çoğaltma başladıktan sonra ayarlar değiştirilemez.



## <a name="track-and-monitor"></a>İzleme ve izleme

- Bir Başlat Çoğaltma işi **çoğaltma'yı** tıklattığınızda iş başlar. 
- Çoğaltmayı Başlat işi başarıyla bittiğinde, makineler ilk çoğaltmalarını Azure'a başlar.
- İlk çoğaltma bittikten sonra, delta çoğaltma başlar. Şirket içi disklerde yapılan artımlı değişiklikler, Azure'daki yineleme disklerine düzenli olarak çoğaltılır.


Portal bildirimlerinde iş durumunu izleyebilirsiniz.

**Azure Geçişi: Sunucu Geçişi'nde** **çoğaltma sunucularını** tıklatarak çoğaltma durumunu izleyebilirsiniz.
![Çoğaltmayı izleme](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Delta çoğaltma başladığında, Azure'a tam bir geçiş çalıştırmadan önce VM'ler için bir test geçişi çalıştırabilirsiniz. Bunu her makine için en az bir kez, geçirmeden önce yapmanızı öneririz.

- Bir test geçişi, çalışmaya devam eden ve çoğaltmaya devam eden şirket içi makineleri etkilemeden, geçişin beklendiği gibi çalışacağını denetler. 
- Test geçişi, çoğaltılan verileri kullanarak (genellikle Azure aboneliğinizde üretim dışı bir VNet'e geçiş yaparak) bir Azure VM oluşturarak geçişi simüle eder.
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce tüm sorunları gidermek için çoğaltılan Azure VM testini kullanabilirsiniz.

Bir test geçişini aşağıdaki gibi yapın:


1. **Geçiş hedefleri** > **Sunucularında** > Azure**Geçir: Sunucu Geçişi**, **geçirilen sunucuları sına'yı**tıklatın.

     ![Geçirilen sunucuları test etme](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Test edilecek VM’yi sağ tıklayıp **Test geçişi** üzerine tıklayın.

    ![Test geçişi](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. **Test Geçişi** bölümünde, Azure VM’nin geçişten sonra bulunacağı Azure VNet’i seçin. Üretim dışı bir VNet kullanmanızı öneririz.
4. **Test Geçişi** işlemi başlar. Portal bildirimlerinde işi izleyin.
5. Geçiş bittikten sonra geçirilen Azure VM’yi, Azure portalında **Sanal Makineler** bölümünde görüntüleyin. Makine adında **Testi** şeklinde bir son ek vardır.
6. Test tamamlandıktan sonra **Makineleri çoğaltma** bölümünde Azure VM’yi sağ tıklayıp **Test geçişini temizle** üzerine tıklayın.

    ![Geçişi temizleme](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçişinin beklendiği gibi çalıştığını doğruladıktan sonra, şirket içi makineleri geçirebilirsiniz.

1. Azure Geçir projesinde **> Sunucular** > **Azure Geçir: Sunucu Geçişi**, **sunucuları çoğaltma'yı**tıklatın.

    ![Sunucuları çoğaltma](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. **Makineleri çoğaltma** bölümünde VM > **Geçir** üzerine sağ tıklayın.
3. **Geçiret** > **Sanal makineleri kapatın ve veri kaybı olmadan planlı geçiş gerçekleştirin**, **Evet** > **Tamam'ı**seçin.
    - VM’yi kapatmak istemiyorsanız, **Hayır** seçeneğini belirleyin

    Not: Fiziksel Sunucu Geçişi için öneri, yürütme penceresinin bir parçası olarak uygulamayı aşağı çekmek (uygulamaların herhangi bir bağlantıyı kabul etmesini izin vermeyin) ve geçişi başlatmaktır (Sunucunun çalışmaya devam etmesi gerekir, bu nedenle kalan değişiklikler geçiş tamamlanmadan önce eşitlenebilir)

4. VM için bir geçiş işlemi başlar. Azure bildirimlerinde işlemi izleyin.
5. İşlem bittikten sonra **Sanal Makineler** sayfasında VM’yi görüntüleyebilir ve yönetebilirsiniz.

## <a name="complete-the-migration"></a>Geçişi tamamlama

1. Geçiş yapıldıktan sonra VM > **Stop geçişine**sağ tıklayın. Bu aşağıdakileri yapar:
    - Şirket içi makinenin çoğaltmasını durdurur.
    - Makineyi Azure Geçiş: Sunucu Geçişi'nde **çoğaltma sunucuları** sayısından kaldırır.
    - Makine için çoğaltma durumu bilgilerini temizler.
2. Azure VM [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) aracısını geçirilen makinelere yükleyin.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure VM örneğine gelen trafiği azledin.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
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
