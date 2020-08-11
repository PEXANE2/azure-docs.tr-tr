---
title: Amazon Web Services (AWS) EC2 VM 'lerini bulun, değerlendirin ve Azure 'a geçirin
description: Bu makalede Azure geçişi ile AWS VM 'lerinin Azure 'a nasıl geçirileceği açıklanır.
ms.topic: tutorial
ms.date: 06/16/2020
ms.custom: MVC
ms.openlocfilehash: 9aad6993af4a90acb41316da0056da84f2e95f70
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88066653"
---
# <a name="discover-assess-and-migrate-amazon-web-services-aws-vms-to-azure"></a>Amazon Web Services (AWS) sanal makinelerini bulma, değerlendirme ve Azure’a geçirme

Bu öğreticide, Azure geçişi 'ni kullanarak Amazon Web Services (AWS) sanal makinelerini (VM) Azure VM 'lerine nasıl keşfedebileceğiniz, değerlendirirsiniz ve geçireceğiniz gösterilmektedir: Sunucu değerlendirmesi ve sunucu geçiş araçları

> [!NOTE]
> AWS sanal makinelerinizi Azure 'a geçirdiğinizde, VM 'Ler fiziksel sunuculardır gibi değerlendirilir. AWS sanal makinelerinizi Azure 'a geçirmek için, fiziksel makinelerin geçirilmesi için sunucu geçiş akışını kullanacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Geçiş için önkoşulları doğrulayın.
> * Azure geçişi ile Azure kaynakları hazırlama: sunucu geçişi. Azure hesabınız ve kaynaklarınızın Azure geçişi ile çalışması için izinleri ayarlayın.
> * AWS EC2 örneklerini geçiş için hazırlayın.
> * Azure geçişi hub 'ına Azure geçişi: sunucu geçiş aracı 'nı ekleyin.
> * Çoğaltma gereci ayarlama ve yapılandırma sunucusunu dağıtma.
> * Taşımak istediğiniz AWS VM 'lerine Mobility hizmetini yükleyebilirsiniz.
> * Sanal makineler için çoğaltmayı etkinleştirme.
> * Çoğaltma durumunu izleyin ve izleyin. 
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın.
> * Azure 'a tam geçiş gerçekleştirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="discover-and-assess-aws-vms"></a>AWS VM 'lerini bulma ve değerlendirme  

Azure 'a geçirmeden önce, bir VM bulma ve geçiş değerlendirmesi gerçekleştirmenizi öneririz. Bu değerlendirme, AWS sanal makinelerinizin Azure 'a geçiş için doğru boyut ve potansiyel Azure çalışma maliyetlerini tahmin etmesine yardımcı olur.

Bir değerlendirmeyi aşağıdaki şekilde ayarlayın:

1. AWS VM 'lerinizi, Azure geçişi: Sunucu değerlendirmesi aracını kullanarak bir değerlendirme yapmak amacıyla fiziksel makine olarak düşünerek bir değerlendirme gerçekleştirilebilir. Azure 'u ayarlamak ve AWS VM 'lerinizi bir değerlendirme için hazırlamak üzere [öğreticiyi](./tutorial-prepare-physical.md) izleyin.
2. Ardından, AWS sanal makinelerinizi bulup değerlendirmek üzere bir Azure geçişi projesi ve gereci ayarlamak için bu [öğreticiyi](./tutorial-assess-physical.md) izleyin.

Bir değerlendirmeyi denemenizi öneririz, ancak değerlendirme gerçekleştirmek VM 'Leri geçirebilmek için zorunlu bir adım değildir.

## <a name="migrate-aws-vms"></a>AWS VM 'lerini geçirme   

## <a name="1-prerequisites-for-migration"></a>1. geçiş için Önkoşullar

- Geçirmek istediğiniz AWS VM 'lerinin desteklenen bir işletim sistemi sürümünü çalıştırdığından emin olun. AWS VM 'Leri, geçiş amacıyla fiziksel makineler gibi değerlendirilir. Fiziksel sunucu geçişi iş akışı için [desteklenen işletim sistemlerini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) gözden geçirin. Gerçek geçişe devam etmeden önce VM 'nin beklendiği gibi çalışıp çalışmadığını doğrulamak için bir test geçişi (yük devretme testi) gerçekleştirmenizi öneririz.
- AWS sanal makinelerinizin Azure 'a geçiş için [desteklenen yapılandırmalara](./migrate-support-matrix-physical-migration.md#physical-server-requirements) uyduğundan emin olun.
- Azure 'a çoğaltılan AWS VM 'lerinin [Azure VM gereksinimleriyle](./migrate-support-matrix-physical-migration.md#azure-vm-requirements) uyumlu olduğunu doğrulayın.
- Azure 'a geçirmeden önce VM 'lerde gereken bazı değişiklikler vardır.
    - Bazı işletim sistemleri için Azure geçişi bu değişiklikleri otomatik olarak yapar.
    - Geçişe başlamadan önce bu değişiklikleri yapmak önemlidir. Değişikliği yapmadan önce VM 'yi geçirirseniz, VM Azure 'da önyüklenemeyebilir.
Yapmanız gereken [Windows](prepare-for-migration.md#windows-machines) ve [Linux](prepare-for-migration.md#linux-machines) değişikliklerini gözden geçirin.

## <a name="2-prepare-azure-resources-for-migration"></a>2. geçiş için Azure kaynakları hazırlama

Azure geçişi: sunucu geçiş aracı ile geçiş için Azure 'u hazırlayın.

**Görev** | **Ayrıntılar**
--- | ---
**Azure geçişi projesi oluşturma** | Azure hesabınızın bir proje oluşturmak için katılımcısı veya sahip izinlerine ihtiyacı vardır.
**Azure hesabınız için izinleri doğrulama** | Azure hesabınızın bir VM oluşturmak ve Azure yönetilen diskine yazmak için izinleri olması gerekir.

### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.

### <a name="assign-azure-account-permissions"></a>Azure hesabı izinleri atama

Sanal makine katılımcısı rolünü Azure hesabına atayın. Bu izinler şunları sağlar:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Azure yönetilen diskine yazın. 

### <a name="create-an-azure-network"></a>Azure ağı oluşturma

Bir Azure sanal ağı (VNet) [ayarlayın](../virtual-network/manage-virtual-network.md#create-a-virtual-network) . Azure 'a çoğaltma yaptığınızda oluşturulan Azure VM 'Ler, geçişi ayarlarken belirttiğiniz Azure VNet 'e birleştirilir.

## <a name="3-prepare-aws-instances-for-migration"></a>3. geçiş için AWS örnekleri hazırlama

AWS 'yi Azure 'a geçişe hazırlamak için, geçiş için bir çoğaltma gereci hazırlamanız ve dağıtmanız gerekir.

### <a name="prepare-a-machine-for-the-replication-appliance"></a>Çoğaltma gereci için bir makine hazırlama

Azure geçişi: sunucu geçişi, makineleri Azure 'a çoğaltmak için bir çoğaltma gereci kullanır. Çoğaltma gereci aşağıdaki bileşenleri çalıştırır.

- **Yapılandırma sunucusu**: yapılandırma sunucusu AWS ortamı ile Azure arasındaki iletişimleri koordine eder ve veri çoğaltmasını yönetir.
- **İşlem sunucusu**: işlem sunucusu bir çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alır, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure 'da bir önbellek depolama hesabına gönderir.

Gereç dağıtımı için aşağıdaki gibi hazırlanın:

- Çoğaltma gerecini barındırmak için ayrı bir EC2 VM ayarlayın. Bu örnek, Windows Server 2012 R2 veya Windows Server 2016 çalıştırıyor olmalıdır. Gereç için donanım, yazılım ve ağ gereksinimlerini [gözden geçirin](./migrate-replication-appliance.md#appliance-requirements) .
- Gereç, çoğaltmak istediğiniz bir kaynak sanal makineye veya daha önce yüklemiş olduğunuz Azure geçişi bulma ve değerlendirme gerecine yüklenmelidir. Farklı bir VM üzerinde dağıtılmalıdır.
- Geçirilecek kaynak AWS VM 'lerinin, çoğaltma gerecine bir ağ hattı olması gerekir. Bunu etkinleştirmek için gerekli güvenlik grubu kurallarını yapılandırın. Çoğaltma gerecinin geçirilecek kaynak VM 'Ler ile aynı VPC 'ye dağıtılması önerilir. Çoğaltma gerecinin farklı bir VPC 'de olması gerekiyorsa, VPCs 'nin VPC eşlemesi aracılığıyla bağlanması gerekir.
- Kaynak AWS VM 'Leri, çoğaltma yönetimi ve çoğaltma veri aktarımı için gelen, HTTPS 443 (denetim kanalı düzenleme) ve TCP 9443 (veri aktarımı) bağlantı noktalarında çoğaltma gereci ile iletişim kurar. ' Deki çoğaltma gereci, Azure 'da HTTPS 443 giden bağlantı noktası üzerinden çoğaltma verilerini düzenler ve Azure 'a gönderir. Bu kuralları yapılandırmak için, güvenlik grubu gelen/giden kurallarını uygun bağlantı noktalarıyla ve kaynak IP bilgileriyle düzenleyin.

   ![AWS güvenlik grupları ](./media/tutorial-migrate-aws-virtual-machines/aws-security-groups.png)
     
 
   ![Güvenlik ayarlarını Düzenle ](./media/tutorial-migrate-aws-virtual-machines/edit-security-settings.png)

- Çoğaltma gereci MySQL kullanır. Gereçte MySQL yükleme [seçeneklerini](migrate-replication-appliance.md#mysql-installation) gözden geçirin.
- Çoğaltma gerecinin [ortak](migrate-replication-appliance.md#url-access) ve [kamu](migrate-replication-appliance.md#azure-government-url-access) bulutları 'Na erişmesi Için gereken Azure URL 'lerini gözden geçirin.

## <a name="4-add-the-server-migration-tool"></a>4. sunucu geçiş aracını ekleme

Bir Azure geçişi projesi ayarlayın ve ardından sunucu geçiş aracını buna ekleyin.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel Bakış** bölümünde **Sunucuları değerlendir ve geçir**’e tıklayın.
4. **Sunucuları bul, değerlendir ve geçir**altında, **sunucuları değerlendir ve geçir**' e tıklayın.

    ![Sunucuları bulma ve değerlendirme](./media/tutorial-migrate-physical-virtual-machines/assess-migrate.png)

5. **Sunucuları bul, değerlendir ve geçir** bölümünde **Araç ekle**’ye tıklayın.
6. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
7. **Proje Ayrıntıları** bölümünde proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtip **İleri**’ye tıklayın. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.
    - Proje Coğrafya yalnızca AWS makinelerinden toplanan meta verileri depolamak için kullanılır.
    - Bir geçiş çalıştırdığınızda herhangi bir hedef bölgeyi seçebilirsiniz.

    ![Azure geçişi projesi oluşturma](./media/tutorial-migrate-physical-virtual-machines/migrate-project.png)

8. **Değerlendirme seçme aracı**’nda **Değerlendirme aracı eklemeyi şimdilik atla** > **İleri**’yi seçin.
9. **Geçiş aracı seçin** bölümünde **Azure Geçişi: Sunucu Geçişi** > **İleri**.
10. **İnceleme + araç ekleme** bölümünde ayarları gözden geçirip **Araç ekle**’ye tıklayın
11. Araç eklendikten sonra, Azure geçişi proje > **sunucuları**  >  **geçiş araçları**' nda görüntülenir.

## <a name="5-set-up-the-replication-appliance"></a>5. çoğaltma gereç ayarı

Geçişin ilk adımı, çoğaltma gerecini ayarlamaya yönelik. AWS VM 'Leri geçişi için gereci ayarlamak üzere, Gereç için yükleyici dosyasını indirmeniz ve ardından bunu [hazırladığınız VM](#prepare-a-machine-for-the-replication-appliance)'de çalıştırmanız gerekir.

### <a name="download-the-replication-appliance-installer"></a>Çoğaltma gereç yükleyicisini indirin

1. Azure geçişi proje > **sunucularında** **Azure geçişi: sunucu geçişi**' nde **bul**' a tıklayın.

    ![VM'leri bulma](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

2. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mi?**, **sanallaştırılmamış/diğer**' e tıklayın.
3. **Hedef bölge**' de, makineleri geçirmek istediğiniz Azure bölgesini seçin.
4. **Geçiş için hedef bölgenin <bölge adı>olduğunu onaylayın **' i seçin.
5. **Kaynak oluştur**' a tıklayın. Bu, arka planda bir Azure Site Recovery Kasası oluşturur.
    - Azure geçişi sunucu geçişi ile geçiş zaten ayarladıysanız, kaynaklar daha önce ayarlandığı için hedef seçenek yapılandırılamaz.
    - Bu düğmeye tıkladıktan sonra bu proje için hedef bölgeyi değiştiremezsiniz.
    - Sanal makinelerinizi farklı bir bölgeye geçirmek için yeni/farklı bir Azure geçişi projesi oluşturmanız gerekir.

6. **Yeni bir çoğaltma gereci yüklemek**istiyor musunuz?, **çoğaltma gereci yüklensin**' i seçin.
7. **Çoğaltma gereci yazılımını indirip yükleyin**bölümünde gereç yükleyicisini ve kayıt anahtarını indirin. Gereci kaydettirmek için anahtar gerekir. Anahtar indirildikten beş gün sonra geçerlidir.

    ![Sağlayıcıyı indir](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

8. Gereç kurulum dosyasını ve anahtar dosyasını, çoğaltma gereci için oluşturduğunuz Windows Server 2016 veya Windows Server 2012 AWS VM 'ye kopyalayın.
9. Sonraki yordamda açıklandığı gibi, çoğaltma gereç kurulum dosyasını çalıştırın.  
    9.1. **Başlamadan Önce** bölümünde **Yapılandırma sunucusunu ve işlem sunucusunu yükleme**’yi seçin ve ardından **İleri**’yi seçin.   
    9,2 **üçüncü taraf yazılım lisansında**, **üçüncü taraf lisans sözleşmesini kabul ediyorum**' u seçin ve ardından **İleri**' yi seçin.   
    9,3 **kayıt**sırasında, git **' i seçin ve**ardından kasa kayıt anahtarı dosyasını yerleştirdiğiniz yere gidin. **İleri**’yi seçin.  
    9,4 **Internet ayarları**'nda, **proxy sunucusu olmadan Azure Site Recovery Bağlan**' ı seçin ve ardından **İleri**' yi seçin.  
    9,5 **Önkoşul denetimi** sayfası, birkaç öğe için Denetim çalıştırır. Tamamlandığında, **İleri**’yi seçin.  
    **MySQL yapılandırmasında**9,6 MySQL db için bir parola girin ve ardından **İleri**' yi seçin.  
    9,7 **ortam ayrıntılarında** **Hayır**' ı seçin. VM 'lerinizi korumanız gerekmez. Ardından **İleri**' yi seçin.  
    9,8 **Install location**bölümünde, Varsayılanı kabul etmek için **İleri** ' yi seçin.  
    9,9 **ağ seçiminde**, Varsayılanı kabul etmek için **İleri** ' yi seçin.  
    9,10 **Özet**bölümünde, **yüklensin**' i seçin.   
    9,11 **Yükleme ilerlemesi** , yükleme işlemi hakkındaki bilgileri gösterir. Tamamlandığında, **Bitir**’i seçin. Bir pencere sistemin yeniden başlatılması hakkında bir ileti görüntüler. **Tamam**’ı seçin.   
    9,12 sonra, bir pencerede yapılandırma sunucusu bağlantı parolası hakkında bir ileti görüntülenir. Parolayı panonuza kopyalayın ve kaynak VM 'lerdeki geçici bir metin dosyasına kaydedin. Bu parola, Mobility hizmeti yükleme işlemi sırasında daha sonra gerekecektir.
10. Yükleme tamamlandıktan sonra gereç Yapılandırma Sihirbazı otomatik olarak başlatılır (gerecin masaüstünde oluşturulan Cspsconfigtool kısayolunu kullanarak Sihirbazı el ile de başlatabilirsiniz). Mobility hizmetinin göndererek yüklenmesi için kullanılacak hesap ayrıntılarını eklemek için sihirbazın hesapları Yönet sekmesini kullanın. Bu öğreticide, çoğaltılacak kaynak VM 'Lere Mobility hizmetini el ile yükleyeceğiz, bu nedenle bu adımda bir kukla hesap oluşturun ve devam edin. "Konuk" adlı kukla hesabı, Kullanıcı adı olarak "Kullanıcı adı" ve hesabın parolası olarak "parola" olarak oluşturmak için aşağıdaki ayrıntıları sağlayabilirsiniz. Çoğaltmayı etkinleştir aşamasında bu kukla hesabı kullanacaksınız. 
11. Kurulum sonrasında gereç yeniden başlatıldıktan sonra, **makine bul**' da, **yapılandırma sunucusu**' nda yeni gereç ' ı seçin ve **kaydı Sonlandır**' a tıklayın. Kayıt işlemini sonuçlandırma, çoğaltma gerecini hazırlamak için birkaç son görevi gerçekleştirir.

    ![Kaydı Sonlandır](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

## <a name="6-install-the-mobility-service"></a>6. Mobility hizmetini yükler

Geçirilecek kaynak AWS VM 'lerine Mobility hizmeti aracısının yüklü olması gerekir. Aracı yükleyicileri, çoğaltma aracısında kullanılabilir. Doğru yükleyiciyi bulur ve geçirmek istediğiniz her makineye aracıyı yüklersiniz. Aşağıdaki gibi yapın:

1. Çoğaltma gereci 'nda oturum açın.
2. **%ProgramData%\asr\home\svsystems\pushınstallsvc\repository dizinine**gidin.
3. Kaynak AWS VM 'Leri işletim sistemi ve sürümü için yükleyiciyi bulun. [Desteklenen işletim sistemlerini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines)gözden geçirin.
4. Yükleyici dosyasını geçirmek istediğiniz kaynak AWS VM 'sine kopyalayın.
5. Çoğaltma gereci yüklerken oluşturulmuş olan kaydedilmiş parola metin dosyasına sahip olduğunuzdan emin olun.
    - Parolayı kaydetmeyi unuttuysanız, bu adımla çoğaltma gereci üzerinde parolayı görüntüleyebilirsiniz. Geçerli parolayı görüntülemek için komut satırından **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe-v** ' yi çalıştırın.
    - Şimdi bu parolayı panonuza kopyalayın ve kaynak VM 'lerde geçici bir metin dosyasına kaydedin.

### <a name="installation-guide-for-windows-aws-vms"></a>Windows AWS VM 'Leri için Yükleme Kılavuzu

1. Yükleyici dosyasının içeriğini AWS sanal makinesi üzerinde yerel bir klasöre (örneğin, C:\Temp) ayıklayın, aşağıdaki gibi:

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```  

2. Mobility hizmeti yükleyicisini çalıştırın:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```  

3. Aracıyı çoğaltma gereci ile kaydedin:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="installation-guide-for-linux-aws-vms"></a>Linux AWS VM 'Leri için Yükleme Kılavuzu

1. Yükleyici tarbol içeriğini AWS sanal makinesi üzerinde yerel bir klasöre (örneğin,/tmp/mobsvcınstaller) ayıklayın ve aşağıdaki gibi:
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```  

2. Yükleyici betiğini çalıştırın:
    ```
    sudo ./install -r MS -q
    ```  

3. Aracıyı çoğaltma gereci ile kaydedin:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="7-enable-replication-for-aws-vms"></a>7. AWS VM 'Leri için çoğaltmayı etkinleştirin

> [!NOTE]
> Portal aracılığıyla, çoğaltma için aynı anda en fazla 10 VM ekleyebilirsiniz. Aynı anda daha fazla VM çoğaltmak için, bunları 10 toplu iş olarak ekleyebilirsiniz.

1. Azure geçişi proje > **sunucularında** **Azure geçişi: sunucu geçişi**' nde **Çoğalt**' a tıklayın.

    ![Sanal makineleri çoğaltma](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. **Çoğaltma**' da, makinelerinizde > **kaynak ayarları**  >  **sanallaştırılır mi?**, **sanallaştırılmamış/diğer**seçeneğini belirleyin.
3. **Şirket içi gereç**bölümünde, ayarladığınız Azure geçiş gerecinin adını seçin.
4. **Işlem sunucusu**' nda, çoğaltma gerecinin adını seçin. 
5. **Konuk kimlik bilgileri**' nde, Mobility hizmetini el ile yüklemek için lütfen daha önce [çoğaltma yükleyicisi kurulumu](#download-the-replication-appliance-installer) sırasında oluşturulan kukla hesabı seçin (gönderme yüklemesi desteklenmez). Ileri ' ye tıklayın **: sanal makineler**.   
 
    ![Sanal makineleri çoğaltma](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)
6. **Sanal makinelerde** **geçiş ayarlarını bir değerlendirmede içeri aktar**bölümünde, varsayılan ayar Hayır olarak kalsın **, geçiş ayarlarını el ile belirteceğiz**.
7. Geçirmek istediğiniz her VM 'yi denetleyin. Ardından Ileri ' ye tıklayın **: hedef ayarlar**.

    ![VM 'Leri seçin](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)

8. **Hedef ayarları**’nda aboneliği ve geçiş yapacağınız hedef bölgeyi seçin. Daha sonra Azure VM’lerinin geçişten sonra bulunacağı kaynak grubunu belirtin.
9. **Sanal Ağ**’da Azure VM’lerinin geçişten sonra katılacağı Azure sanal ağını/alt ağını seçin.
10. **Azure Hibrit Avantajı**’nda:
    - Azure Hibrit Avantajı’nı uygulamak istemiyorsanız **Hayır**’ı seçin. Ardından **İleri**'ye tıklayın.
    - Etkin Yazılım Güvencesi veya Windows Server abonelikleri kapsamında olan Windows Server makineleriniz varsa ve avantajı geçirdiğiniz makinelere uygulamak istiyorsanız **Evet**’i seçin. Ardından **İleri**'ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

11. **İşlem** bölümünde VM adını, boyutunu, İşletim Sistemi disk türünü ve kullanılabilirlik kümesini gözden geçirin. VM’ler [Azure gereksinimleriyle](migrate-support-matrix-physical-migration.md#azure-vm-requirements)uyumlu olmalıdır.

    - **VM boyutu**: Azure geçişi sunucu geçişi, varsayılan olarak Azure aboneliğindeki en yakın eşleşmeye göre bir boyut seçer. Alternatif olarak **Azure VM boyutu **’nda el ile bir boyut seçin.
    - **Işletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim Sistemi diski, işletim sistemi önyükleyiciye ve yükleyiciye sahip disktir. 
    - **Kullanılabilirlik kümesi**: sanal makinenin geçişten sonra bir Azure kullanılabilirlik kümesinde olması gerekiyorsa, kümeyi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

    ![İşlem ayarları](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

12. **Diskler**' de, VM disklerinin Azure 'da çoğaltılıp çoğaltılmayacağını belirtin ve Azure 'da disk türünü (Standart SSD/HDD veya Premium yönetilen diskler) seçin. Ardından **İleri**'ye tıklayın.
    - Diskleri çoğaltmadan çıkarabilirsiniz.
    - Diskleri çıkarırsanız bu diskler geçişten sonra Azure VM’de bulunmaz. 

    ![Disk ayarları](./media/tutorial-migrate-physical-virtual-machines/disks.png)

13. **Çoğaltmayı gözden geçir ve başlat** bölümünde ayarları gözden geçirin ve sunuculara yönelik ilk çoğaltmayı başlatmak için **Çoğalt** üzerine tıklayın.

> [!NOTE]
> Çoğaltma ayarlarını, çoğaltma başlamadan önce dilediğiniz zaman güncelleştirebilirsiniz, **Manage**  >  **çoğaltılan makineleri**yönetin. Çoğaltma başladıktan sonra ayarlar değiştirilemez.

## <a name="8-track-and-monitor-replication-status"></a>8. çoğaltma durumunu izleme ve izleme

- **Çoğalt** ' a tıkladığınızda çoğaltma Başlat işi başlar.
- Çoğaltma Başlat işi başarıyla tamamlandığında, VM 'Ler ilk çoğaltmasını Azure 'a başlatır.
- İlk çoğaltma tamamlandıktan sonra Delta çoğaltma başlar. AWS VM disklerinde artımlı değişiklikler düzenli aralıklarla Azure 'daki çoğaltma disklerine çoğaltılır.

Portal bildirimlerinde iş durumunu izleyebilirsiniz.

Çoğaltma durumunu, **Azure geçişi: sunucu geçişi**' nde **sunucuları** çoğaltma ' ya tıklayarak izleyebilirsiniz.  

![Çoğaltmayı izleme](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="9-run-a-test-migration"></a>9. bir test geçişi çalıştırın

Delta çoğaltma başladığında, Azure 'a tam geçiş çalıştırmadan önce VM 'Ler için bir test geçişi çalıştırabilirsiniz. Test geçişi kesinlikle önerilir ve olası sorunları bulmaya ve gerçek geçişe devam etmeden önce bunları gidermeye yönelik bir fırsat sağlar. Bunu geçirmeden önce her VM için en az bir kez yapmanız önerilir.

- Bir test geçişi çalıştırmak, geçiş işleminin beklenen şekilde çalıştığını denetler ve bu işlem çalışır durumda kalır ve çoğaltmaya devam edilir.
- Test geçişi, çoğaltılan verileri kullanarak bir Azure VM oluşturarak geçişe benzetir (genellikle Azure aboneliğinizdeki bir üretim dışı VNet 'e geçiş yapar).
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce herhangi bir sorunu gidermek için çoğaltılan test Azure VM 'yi kullanabilirsiniz.

Test geçişini aşağıdaki şekilde yapın:

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: sunucu geçişi**' nde **geçirilen sunucuları test et**' e tıklayın.

     ![Geçirilen sunucuları test etme](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Test edilecek VM’yi sağ tıklayıp **Test geçişi** üzerine tıklayın.

    ![Test geçişi](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. **Test Geçişi** bölümünde, Azure VM’nin geçişten sonra bulunacağı Azure VNet’i seçin. Üretim dışı bir VNet kullanmanızı öneririz.
4. **Test Geçişi** işlemi başlar. Portal bildirimlerinde işi izleyin.
5. Geçiş bittikten sonra geçirilen Azure VM’yi, Azure portalında **Sanal Makineler** bölümünde görüntüleyin. Makine adında **Testi** şeklinde bir son ek vardır.
6. Test tamamlandıktan sonra **Makineleri çoğaltma** bölümünde Azure VM’yi sağ tıklayıp **Test geçişini temizle** üzerine tıklayın.

    ![Geçişi temizleme](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="10-migrate-aws-vms"></a>10. AWS VM 'lerini geçirin

Test geçişinin beklendiği gibi çalışıp çalışmadığını doğruladıktan sonra AWS sanal makinelerini geçirebilirsiniz.

1. Azure geçişi proje > **sunucuları**  >  **Azure geçişi: sunucu geçişi**' nde, **sunucuları çoğaltma**' ya tıklayın.

    ![Sunucuları çoğaltma](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. **Makineleri çoğaltma** bölümünde VM > **Geçir** üzerine sağ tıklayın.
3. **Migrate**  >  **Sanal makineleri Kapat ' a geçiş yapın ve veri kaybı olmadan planlı bir geçiş gerçekleştirin**, **Evet**  >  **Tamam**' ı seçin.
    - VM 'yi kapatmak istemiyorsanız **Hayır**' ı seçin.
4. VM için bir geçiş işlemi başlar. Portalın sağ üst köşesindeki bildirim zil simgesine tıklayarak veya sunucu geçiş aracının işler sayfasına giderek (araç kutucu> ğunda genel bakış ' a tıklayarak (sol menüden Işler ' i seçerek) iş durumunu görüntüleyebilirsiniz.
5. İşlem bittikten sonra Sanal Makineler sayfasında VM’yi görüntüleyebilir ve yönetebilirsiniz.

### <a name="complete-the-migration"></a>Geçişi tamamlamayı

1. Geçiş yapıldıktan sonra, **geçişi durdurmak**> VM 'ye sağ tıklayın. Bu, şunları yapar:
    - AWS sanal makinesi için çoğaltmayı durduruyor.
    - AWS sanal makinesini Azure geçişi: sunucu geçişi içindeki **çoğaltma sunucusu** sayısından kaldırır.
    - VM için çoğaltma durumu bilgilerini temizler.
2. Geçirilen makinelere [Linux](../virtual-machines/extensions/agent-linux.md) Aracısı 'nı yükler. Geçiş işlemi sırasında Azure VM Windows Aracısı önceden yüklenir.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure VM örneğine giden trafiği kesin.
6. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Azure Güvenlik Merkezi ile gelen trafik erişimini kilitleme ve sınırlayın [-tam zamanında yönetim](../security-center/security-center-just-in-time.md).
    - [Ağ Güvenlik Grupları](../virtual-network/security-overview.md) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](../security/fundamentals/azure-disk-encryption-vms-vmss.md)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
    - [Azure Maliyet Yönetimi](../cost-management-billing/cloudyn/overview.md)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure bulut benimseme çerçevesindeki [bulut geçiş yolculuğunu](/azure/architecture/cloud-adoption/getting-started/migrate) araştırın.

## <a name="troubleshooting--tips"></a>Sorun giderme/Ipuçları

**Soru:** Geçiş için bulunan sunucular listesinde AWS VM 'mi göremiyorum   
**Cevap:** Çoğaltma gerecinizin gereksinimleri karşılayıp karşılamadığını denetleyin. Geçirilecek kaynak VM 'de Mobility aracısının yüklü olduğundan ve yapılandırma sunucusu 'nun kaydedildiğinden emin olun. Çoğaltma gereci ve kaynak AWS VM 'Leri arasında bir ağ yolunu etkinleştirmek için ağ ayarını ve güvenlik duvarı kurallarını kontrol edin.  

**Soru:** Nasıl yaparım? VM 'nin başarıyla geçirilip geçirilmediğini öğrenin   
**Cevap:** Geçiş sonrası, sanal makineler sayfasından VM 'yi görüntüleyebilir ve yönetebilirsiniz. Doğrulamak için geçirilen VM 'ye bağlanın.  

**Soru:** Daha önce oluşturulmuş sunucu değerlendirmesi sonuçlarım 'dan geçiş için VM 'Leri içeri aktaramıyorum   
**Cevap:** Şu anda bu iş akışı için değerlendirme içeri aktarmayı desteklemiyoruz. Geçici bir çözüm olarak, değerlendirmeyi dışarı aktarabilir ve ardından çoğaltmayı etkinleştir adımı sırasında VM önerisini el ile seçebilirsiniz.
  
**Soru:** AWS VM 'lerimi bulmaya çalışırken "BIOS GUID 'SI getirilemedi" hatasını alıyorum   
**Cevap:** AWS VM 'Leri için desteklenen işletim sistemlerini gözden geçirin.  

**Soru:** Çoğaltma durumum ilerlemiyor    
**Cevap:** Çoğaltma gerecinizin gereksinimleri karşılayıp karşılamadığını denetleyin. Çoğaltma gereci TCP bağlantı noktası 9443 ve HTTPS 443 veri aktarımı için gerekli bağlantı noktalarını etkinleştirdiğinizden emin olun. Aynı projeye bağlı çoğaltma gerecinin eski bir sürümü olmadığından emin olun.   

**Soru:** Uzak Windows Yönetim hizmeti 'nden 504 HTTP durum kodu nedeniyle Azure geçişi 'ni kullanarak AWS örneklerini bulamadım    
**Cevap:** Azure geçiş gereci gereksinimlerini ve URL erişim gereksinimlerini gözden geçirdiğinizden emin olun. Hiçbir proxy ayarlarının gereç kaydını engellemediğinden emin olun.   
