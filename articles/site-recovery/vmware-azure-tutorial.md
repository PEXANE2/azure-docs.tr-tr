---
title: Azure Site Kurtarma ile Azure'da VMware VM olağanüstü durum kurtarma yı ayarlama
description: Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarma ayarlamayı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 37fdd42adf66ebcb11b357ece6ea63384630d9f4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238870"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarmayı ayarlama

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak Azure'a olağanüstü durum kurtarma için şirket içi VMware VM'ler için çoğaltmanın nasıl etkinleştirilen açıklanmaktadır.

Bu, şirket içi VMware VM'ler için Azure'da olağanüstü durum kurtarma yı nasıl ayarlayabileceğinizi gösteren bir serinin üçüncü öğreticisidir. Önceki eğitimde, Azure'da olağanüstü durum kurtarma için [şirket içi VMware ortamını hazırladık.](vmware-azure-tutorial-prepare-on-premises.md)


Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kaynak çoğaltma ayarlarını ve şirket içi Site Kurtarma yapılandırma sunucusunu ayarlayın.
> * Çoğaltma hedef ayarlarını ayarlayın.
> * Bir çoğaltma ilkesi oluşturma.
> * VMware VM için çoğaltmayı etkinleştirin.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı talimatlar için, Site Kurtarma İçeriği Tablosunun Nasıl Yapılacağı bölümündeki makaleyi inceleyin.

## <a name="before-you-start"></a>Başlamadan önce

Önceki öğreticileri tamamlayın:
1. Azure'u şirket içi VMware olağanüstü durum kurtarma için [Azure'u azure'a ayarladığınızdan](tutorial-prepare-azure.md) emin olun.
2. Şirket içi VMware dağıtımınızı Azure'a olağanüstü durum kurtarma için hazırlamak için [aşağıdaki adımları](vmware-azure-tutorial-prepare-on-premises.md) izleyin.
3. Bu öğreticide, tek bir VM'yi nasıl kopyaladığınızı göstereceğiz. Birden çok VMware VM dağıtıyorsanız [Dağıtım Planlayıcısı Aracı'nı](https://aka.ms/asr-deployment-planner)kullanmanız gerekir. Bu araç hakkında [daha fazla bilgi edinin](site-recovery-deployment-planner.md).
4. Bu öğretici, farklı yapmak isteyebileceğin bir dizi seçenek kullanır:
    - Öğretici yapılandırma sunucusu VMware VM oluşturmak için bir OVA şablonu kullanır. Bunu bir nedenle yapamıyorsanız, yapılandırma sunucusunu el ile ayarlamak için [aşağıdaki yönergeleri](physical-manage-configuration-server.md) izleyin.
    - Bu eğitimde, Site Kurtarma mysql'i yapılandırma sunucusuna otomatik olarak indirir ve yükler. İsterseniz, bunun yerine el ile ayarlayabilirsiniz. [Daha fazla bilgi edinin](vmware-azure-deploy-configuration-server.md#configure-settings).




## <a name="select-a-protection-goal"></a>Koruma hedefi seçme

1. **Kurtarma Hizmetleri kasaları** bölümünde kasa adını seçin. Bu senaryo için**ContosoVMVault**’u kullanıyoruz.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Daha sonra **Altyapıyı Hazırlama**’yı seçin.
3. **Koruma hedefiNde** > **Makineleriniz nerede bulunur**, şirket **içi**seçin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.



## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamınızda, bu şirket içi Site Kurtarma bileşenlerini barındırmak için tek, son derece kullanılabilir, şirket içi makineye ihtiyacınız vardır:

- **Yapılandırma sunucusu**: Yapılandırma sunucusu şirket içi ve Azure arasındaki iletişimi koordine eder ve veri çoğaltmayı yönetir.
- **İşlem sunucusu**: İşlem sunucusu bir çoğaltma ağ geçidi gibi davranır. Çoğaltma verilerini alır; önbelleğe alma, sıkıştırma ve şifreleme ile optimize eder ve Azure'daki bir önbellek depolama hesabına gönderir. İşlem sunucusu ayrıca çoğaltmak istediğiniz VM'lere Mobilite Hizmeti aracısını yükler ve şirket içi VMware VM'lerin otomatik keşfini gerçekleştirir.
- **Ana hedef sunucu**: Ana hedef sunucu, Azure'dan geri dönüş sırasında çoğaltma verilerini işler.


Bu bileşenlerin tümü, *yapılandırma sunucusu*olarak bilinen tek şirket içi makinelerde birlikte yüklenir. Varsayılan olarak, VMware olağanüstü durum kurtarma için, yapılandırma sunucusunu yüksek kullanılabilir vmware VM olarak kurduk. Bunu yapmak için, hazırlanmış bir Açık Sanallaştırma Uygulaması (OVA) şablonunu karşıdan yükleyin ve VM oluşturmak için şablonu VMware'e aktarın. 

- Yapılandırma sunucusunun en son sürümü portalda mevcuttur. Ayrıca doğrudan Microsoft Download [Center'dan](https://aka.ms/asrconfigurationserver)indirebilirsiniz.
- VM ayarlamak için bir OVA şablonu kullanamıyorsanız, yapılandırma sunucusunu el ile ayarlamak için [aşağıdaki yönergeleri](physical-manage-configuration-server.md) izleyin.
- OVF şablonu ile sağlanan lisans 180 gün için geçerli bir değerlendirme lisansıdır. VM üzerinde çalışan pencereler, gerekli lisansla etkinleştirilmelidir. 


### <a name="download-the-vm-template"></a>VM şablonunu indirme

1. Kasada, Altyapı > **Kaynak** **Hazırla'ya**gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için OVF şablonunu indirin.



## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma


1. VMWare vSphere İstemcisi ile VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek **OVF Şablonu Dağıtma Sihirbazı**’nı başlatın. 

     ![OVF şablonu](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. **Kaynak seçin** bölümünde, indirilen OVF’nin konumunu girin.
4. **Değerlendirme ayrıntıları** bölümünde **İleri** seçeneğini belirleyin.
5. **Ad ve klasör seçin** ve **Yapılandırma seçin** bölümünde varsayılan ayarları kabul edin.
6. **Select storage** bölümünde, en iyi performans için **Select virtual disk format** bölümünden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
7. Sihirbaz sayfalarının geri kalan kısmında varsayılan ayarları kabul edin.
8. **Tamamlanmak için hazır** durumunda, VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

   > [!TIP]
   > Ek bir NIC eklemek istiyorsanız, Dağıtım > **Bittikten**sonra **Güç'ün açık olmasını.** Varsayılan olarak, şablon tek bir NIC içerir. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

Yapılandırma sunucusuna ek bir NIC eklemek istiyorsanız, sunucuyu kasaya kaydettirmeden önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Ardından **İleri'yi**seçin.
3. Bir bağdaştırıcı türü ve ağ seçin. 
4. VM açıldığında sanal NIC’ye bağlanmak için **Açıldığında bağlan**’ı seçin. **Sonraki** > **Bitiş'i**seçin. Sonra **Tamam**’ı seçin.


## <a name="register-the-configuration-server"></a>Yapılandırma sunucusunu kaydetme 

Yapılandırma sunucusu ayarlandıktan sonra kasaya kaydedersiniz.

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İlk oturum açma işleminizde Azure Site Recovery Yapılandırma Aracı birkaç saniye içinde başlatılır.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Ardından **İleri'yi**seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır. Bu kullanıcıya gerekli [rollerin](vmware-azure-deploy-configuration-server.md#azure-active-directory-permission-requirements) atandığından emin olun.
7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Makinede tekrar oturum açın. Birkaç saniye içinde, Yapılandırma Sunucusu Yönetim Sihirbazı otomatik olarak başlar.


### <a name="configure-settings-and-add-the-vmware-server"></a>Ayarları yapılandırma ve VMware sunucusunu ekleme

Yapılandırma sunucusunu ayarlamayı ve kaydetmeyi bitirin. İşleme başlamadan önce, yapılandırma sunucusunun başarılı bir şekilde ayarlanması için tüm [ön koşulların](vmware-azure-deploy-configuration-server.md#prerequisites) karşılandığından emin olun.


1. Yapılandırma sunucusu yönetimi sihirbazında **Kurulum bağlantısı'nı**seçin. Açılan geçitlerden önce yerleşik işlem sunucusunun kaynak makinelerde mobilite hizmetinin keşfi ve itme yüklemesi için kullandığı NIC'i seçin ve ardından Configuration Server'ın Azure ile bağlantı için kullandığı NIC'i seçin. Ardından **Kaydet'i**seçin. Bu ayar, yapılandırıldıktan sonra değiştiremezsiniz.
2. **Recovery Services kasasını seçin** bölümünde Azure aboneliğinizi, ilgili kaynak grubunu ve kasayı seçin.
3. **Üçüncü taraf yazılımı yükleyin** bölümünde lisans sözleşmesini kabul edin. MySQL Server’ı yüklemek için **İndir ve Yükle** seçeneğini belirleyin. MySQL'i yola koyduysanız, bu adım atlanabilir. [Daha fazla](vmware-azure-deploy-configuration-server.md#configure-settings) bilgi edinin
4. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
5. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
6. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kullanıcı kimlik bilgilerini girin. Girdiğiniz kullanıcı adı ve parola bilgilerinin doğru ve korunacak sanal makinenin Yöneticiler grubuna ait olduğuna emin olun. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. **Ekle** seçeneğini ve sonra **Devam** seçeneğini belirleyin.
7. **Sanal makine kimlik bilgilerini yapılandırma** bölümünde, çoğaltma etkinleştirildiğinde VM’lere Mobility Hizmetinin otomatik olarak yüklenmesi için kullanılacak kullanıcı adını ve parolayı girin.
    - Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir.
    - Linux’ta kök hesap için bilgileri sağlayın.
8. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.
9. Kayıt bittikten sonra Azure portalını açın ve yapılandırma sunucusunun ve VMware sunucusunun **Recovery Services Vault** > **Manage** > **Site Recovery Infrastructure** > **Configuration Servers'da**listeli olduğunu doğrulayın.


Yapılandırma sunucusu kaydedildikten sonra, Site Kurtarma belirtilen ayarları kullanarak VMware sunucularına bağlanır ve VM'leri keşfeder.

> [!NOTE]
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirmek için **Configuration Servers** > ***sunucu adını*** > **yenile Server'ı**seçin.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. Altyapı**Hedef** **Hazırla'yı** > seçin. Kullanmak istediğiniz Azure aboneliğini seçin. Bir Kaynak Yöneticisi modeli kullanıyoruz.
2. Site Kurtarma, bir veya daha fazla sanal ağınız olup olmadığını denetler. Bu öğretici serisindeki [ilk öğreticide](tutorial-prepare-azure.md) Azure bileşenlerini ayarladığınızda bunları edinmeniz gerekir.

   ![Hedef sekmesi](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Azure [portalını](https://portal.azure.com)açın. Kurtarma Hizmetleri **kasalarını**arayın ve seçin.
2. Kurtarma Hizmetleri kasasını (bu öğreticide **ContosoVMVault**) seçin.
3. Çoğaltma ilkesi oluşturmak için **Site Kurtarma altyapısı** > **Çoğaltma İlkeleri** > **+Çoğaltma İlkesi'ni**seçin.
4. **Çoğaltma ilkesi oluştur** seçeneğinde, ilke adını girin. **VMwareRepPolicy** ilkesini kullanıyoruz.
5. **RPO eşiği** bölümünde varsayılan 60 dakika seçeneğini kullanın. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
6. **Kurtarma noktası tutma** içinde, her bir kurtarma noktasının ne kadar tutulacağını belirtin. Bu öğretici için 72 saat değerini kullanıyoruz. Çoğaltılan VM’ler bir tutma penceresindeki herhangi bir noktaya kurtarılabilir.
7. **Uygulamayla tutarlı anlık görüntü sıklığı** içinde, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığını belirtin. Varsayılan 60 dakikayı kullanıyoruz. İlkeyi oluşturmak için **Tamam**’ı seçin.

   ![Çoğaltma ilkesi oluşturma](./media/vmware-azure-tutorial/replication-policy.png)

- İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir.
- Yeniden çalışma için varsayılan olarak, eşleşen bir ilke otomatik olarak oluşturulur. Örneğin, çoğaltma ilkesi **rep-policy** şeklindeyse yeniden çalışma ilkesi **rep-policy-failback** şeklindedir. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

VM'ler için çoğaltmayı aşağıdaki gibi etkinleştirin:

1. Uygulama**Kaynağını** **Çoğaltma'yı** > seçin.
2. **Kaynak** içinde, **Şirket içi**’ni seçin ve **Kaynak konumu** içinde yapılandırma sunucusunu seçin.
3. **Makine türü** bölümünde **Sanal Makineler**’i seçin.
4. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını veya konağı yöneten vCenter sunucusunu seçin.
5. İşlem sunucusunu seçin (varsayılan olarak yapılandırma sunucusu VM’sine yüklenir). Sonra **Tamam**’ı seçin. Her işlem sunucusunun sistem durumu, önerilen sınırlar ve diğer parametrelere göre belirtilir. Sağlıklı bir işlem sunucusu seçin. [Kritik](vmware-physical-azure-monitor-process-server.md#process-server-alerts) bir işlem sunucusu seçilemez. Hataları [giderebilir ve çözebilir](vmware-physical-azure-troubleshoot-process-server.md) **veya** [bir ölçeklendirme işlem sunucusu ayarlayabilirsiniz.](vmware-azure-set-up-process-server-scale.md)
6. **Hedef** bölümünde, yükü devredilen VM’leri oluşturmak istediğiniz aboneliği ve kaynak grubunu seçin. Kaynak Yöneticisi dağıtım modelini kullanacağız. 
7. Yük devretme işleminden sonra oluşturulan Azure VM’lerin bağlandığı Azure ağını ve alt ağını seçin.
8. Çoğaltmayı etkinleştirdiğiniz tüm VM’lere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
9. **Sanal Makinelerde** > **Sanal Makineleri seçin,** çoğaltmak istediğiniz her makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin. Belirli bir sanal makineyi görüntüleyemez/seçemezseniz, sorunu çözme hakkında [daha fazla bilgi edinin.](https://aka.ms/doc-plugin-VM-not-showing)
10. **Özellikleri** > **Yapılandırma özelliklerinde,** mobilite hizmetini makineye otomatik olarak yüklemek için işlem sunucusu tarafından kullanılacak hesabı seçin.
11. **Çoğaltma ayarlarında** > **çoğaltma ayarlarını yapılandırın,** doğru çoğaltma ilkesinin seçildiğini doğrulayın.
12. **Çoğaltmayı Etkinleştir'i**seçin. Bir VM için çoğaltma etkinleştirildiğinde Site Recovery, Mobility Hizmeti’ni yükler.
13. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Finalize Protection** işi çalıştığından ve kurtarma noktası oluşturma tamamlandıktan sonra, makine başarısız olmaya hazırdır.
14. Değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.
15. Eklediğiniz VM'leri izlemek için, Configuration Servers > Son Kişi**adresindeki**VM'ler için keşfedilen son zamanı kontrol **edin.** Zamanlanan bulma işlemini beklemeden VM’leri eklemek için yapılandırma sunucusunu vurgulayın (seçmeyin) ve **Yenile**’yi seçin.

## <a name="next-steps"></a>Sonraki adımlar
Çoğaltmayı etkinleştirdikten sonra, her şeyin beklendiği gibi çalıştığından emin olmak için bir matkap çalıştırın.
> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](site-recovery-test-failover-to-azure.md)
