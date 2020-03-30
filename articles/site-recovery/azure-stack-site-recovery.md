---
title: Azure Site Kurtarma 'yı kullanarak Azure Yığını VM'lerini Azure'a çoğaltma | Microsoft Dokümanlar
description: Azure Site Kurtarma hizmetiyle Azure Yığın VM'leri için Azure'da olağanüstü durum kurtarma yı nasıl ayarlayamanızı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.topic: conceptual
ms.service: site-recovery
ms.date: 08/05/2019
ms.author: raynew
ms.openlocfilehash: 15cd729063545914f791de39a075af9084f72bef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426561"
---
# <a name="replicate-azure-stack-vms-to-azure"></a>Azure Stack VM'lerini Azure'a çoğaltma

Bu makalede, [Azure Site Kurtarma hizmetini](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)kullanarak olağanüstü durum kurtarma Azure Yığın VM'lerini Azure'a nasıl ayarlayabileceğinizi gösterilmektedir.

Site Kurtarma, iş sürekliliği nize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Hizmet, beklenen ve beklenmeyen kesintiler oluştuğunda VM iş yüklerinizin kullanılabilir kalmasını sağlar.

- Site Kurtarma, VM'lerin Azure depolama alanına çoğaltılmasını yönetir ve yönetir.
- Birincil sitenizde bir kesinti olduğunda, Azure'da başarısız olmak için Site Kurtarma'yı kullanırsınız.
- Azure VM'ler, depolanan VM verilerinden oluşturulur ve kullanıcılar bu Azure VM'lerinde çalışan iş yüklerine erişmeye devam edebilir.
- Her şey yeniden çalışır durumdayken, Azure VM'lerini birincil sitenize geri döndürebilir ve Azure depolama alanına yeniden çoğaltmaya başlayabilirsiniz.


Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * **Adım 1: Çoğaltma için Azure yığını VM'leri hazırlayın.** VM'lerin Site Kurtarma gereksinimlerine uygun olup olmadığını kontrol edin ve Site Kurtarma Mobilite hizmetinin yüklenmesine hazırlanın. Bu hizmet çoğaltmak istediğiniz her VM'de yüklenir.
> * **Adım 2: Kurtarma Hizmetleri kasası ayarlayın.** Site Kurtarma için bir kasa ayarlayın ve neyi çoğaltmak istediğinizi belirtin. Site Kurtarma bileşenleri ve eylemleri kasada yapılandırılır ve yönetilir.
> * **Adım 3: Kaynak çoğaltma ortamını ayarlayın.** Site Kurtarma yapılandırma sunucusu ayarlayın. Yapılandırma sunucusu, Site Kurtarma tarafından gereken tüm bileşenleri çalıştıran tek bir Azure Yığını VM'sidir. Yapılandırma sunucusunu kurduktan sonra kasaya kaydedersiniz.
> * **Adım 4: Hedef çoğaltma ortamını ayarlayın.** Azure hesabınızı ve kullanmak istediğiniz Azure depolama hesabını ve ağını seçin. Çoğaltma sırasında VM verileri Azure depolama alanına kopyalanır. Başarısız olduktan sonra, Azure VM'leri belirtilen ağa katılır.
> * **Adım 5: Çoğaltmayı etkinleştirin.** Çoğaltma ayarlarını yapılandırın ve VM'ler için çoğaltmayı etkinleştirin. Çoğaltma etkinleştirildiğinde Mobilite hizmeti bir VM'ye yüklenir. Site Kurtarma VM'nin ilk çoğaltmasını gerçekleştirir ve ardından devam eden çoğaltma başlar.
> * **Adım 6: Bir olağanüstü durum kurtarma matkabı çalıştırın**: Çoğaltma başladıktan ve çalıştırdıktan sonra, bir matkap çalıştırarak beklendiği gibi başarısız olacağını doğrularsınız. Matkabı başlatmak için, Site Kurtarma'da bir test başarısızlığını çalıştırın. Test inanın, üretim ortamınızı etkilemez.

Bu adımlar tamamlandığında, azure'a tam bir başarısızlık sağlayabilirsiniz.

## <a name="architecture"></a>Mimari

![Mimari](./media/azure-stack-site-recovery/architecture.png)

**Konum** | **Bileşen** |**Şey**
--- | --- | ---
**Yapılandırma Sunucusu** | Tek bir Azure Stack VM'de çalışır. | Her abonelikte bir yapılandırma sunucusu VM ayarlayın. Bu VM aşağıdaki Site Kurtarma bileşenlerini çalıştırMaktadır:<br/><br/> - Yapılandırma sunucusu: Şirket içi ve Azure arasındaki iletişimi koordine eder ve veri çoğaltmayı yönetir. - İşlem sunucusu: Çoğaltma ağ geçidi görevi görür. Çoğaltma verilerini alır, önbelleğe alma, sıkıştırma ve şifreleme ile en iyi duruma getirir; ve Azure depolama alanına gönderir.<br/><br/> Çoğaltmak istediğiniz VM'ler aşağıda belirtilen sınırları aşıyorsa, ayrı bir bağımsız işlem sunucusu ayarlayabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-process-server-scale).
**Mobilite hizmeti** | Çoğaltmak istediğiniz her VM'ye yüklenir. | Bu makaledeki adımlarda, çoğaltma etkinleştirildiğinde Mobilite hizmetinin otomatik olarak VM'ye yüklenmesi için bir hesap hazırlıyoruz. Hizmeti otomatik olarak yüklemek istemiyorsanız, kullanabileceğiniz başka yöntemler de vardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-install-mobility-service).
**Azure** | Azure'da Kurtarma Hizmetleri kasası, depolama hesabı ve sanal ağa ihtiyacınız vardır. |  Çoğaltılan veriler depolama hesabında depolanır. Azure VM'leri, başarısız olduğunda Azure ağına eklenir. 


Çoğaltma aşağıdaki gibi çalışır:

1. Kasada, çoğaltma kaynağını ve hedefini belirtir, yapılandırma sunucusunu ayarlar, bir çoğaltma ilkesi oluşturur ve çoğaltmayı etkinleştirin.
2. Mobilite hizmeti makineye yüklenir (itme yüklemesini kullandıysanız) ve makineler çoğaltma ilkesine uygun olarak çoğaltmaişlemine başlar.
3. Sunucu verilerinin ilk kopyası Azure depolamasına kopyalanır.
4. İlk çoğaltma işlemi bittikten sonra, Delta değişikliklerinin Azure'da çoğaltılması başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
5. Yapılandırma sunucusu çoğaltma yönetimini Azure ile (https 443 giden bağlantı noktası) yönetir.
6. İşlem sunucusu verileri kaynak makinelerden alır, optimize eder ve şifreler ve Azure depolama alanına gönderir (bağlantı noktası 443 giden).
7. Çoğaltılan makineler, çoğaltma yönetimi için yapılandırma sunucusuyla (https 443 gelen bağlantı noktası) iletişim kurar. Makineler çoğaltma verilerini işlem sunucusuna gönderir (https 9443 giriş noktası - değiştirilebilir).
8. Trafik İnternet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Alternatif olarak, Azure ExpressRoute genel eşliğini kullanabilirsiniz. Trafiğin siteden siteye bir VPN aracılığıyla şirket içi bir siteden Azure’a çoğaltılması desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Bu senaryoyu ayarlamak için gerekenler şunlardır.

**Gereksinim** | **Şey**
--- | ---
**Azure abonelik hesabı** | Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/pricing/free-trial/)oluşturun.
**Azure hesap izinleri** | Kullandığınız Azure hesabının aşağıdakilere izin alması gerekir:<br/><br/> - Kurtarma Hizmeti kasası oluşturun<br/><br/> - Senaryo için kullandığınız kaynak grubunda ve sanal ağda sanal bir makine oluşturun<br/><br/> - Belirttiğiniz depolama hesabına yazın<br/><br/> Şunlara dikkat edin:<br/><br/> -Bir hesap oluşturursanız, aboneliğinizin yöneticisisiniz ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> - Varolan bir aboneliği kullanıyorsanız ve yönetici değilseniz, size Sahip veya Katılımcı izinleri atamak için yöneticiyle birlikte çalışmanız gerekir.<br/><br/> - Daha ayrıntılı izinlere ihtiyacınız varsa, [bu makaleyi](https://docs.microsoft.com/azure/site-recovery/site-recovery-role-based-linked-access-control)inceleyin. 
**Azure Yığını VM** | Kiracı aboneliğinde, Site Kurtarma yapılandırma sunucusu olarak dağıtılacak bir Azure Stack VM'ye ihtiyacınız var. 


### <a name="prerequisites-for-the-configuration-server"></a>Yapılandırma sunucusu için ön koşullar

[!INCLUDE [site-recovery-config-server-reqs-physical](../../includes/site-recovery-config-server-reqs-physical.md)]


 
## <a name="step-1-prepare-azure-stack-vms"></a>Adım 1: Azure Yığını VM'lerini Hazırlama

### <a name="verify-the-operating-system"></a>İşletim sistemini doğrulama

VM'lerin tabloda özetlenen işletim sistemlerinden birini çalıştırdığından emin olun.


**İşletim sistemi** | **Şey**
--- | ---
**64 bit Windows** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 (SP1'den)
**CentOS** | 5.2 ila 5.11, 6.1 ila 6.9, 7.0 - 7.3
**Ubuntu** | 14.04 LTS sunucusu, 16.04 LTS sunucusu. [Desteklenen çekirdekleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#ubuntu-kernel-versions) gözden geçirin

### <a name="prepare-for-mobility-service-installation"></a>Mobilite servis kurulumu için hazırlanın

Çoğaltmak istediğiniz her VM'de Mobilite hizmeti yüklü olmalıdır. Çoğaltma etkinleştirildiğinde işlem sunucusunun hizmeti VM'ye otomatik olarak yükleyebilmesi için VM ayarlarını doğrulayın.

#### <a name="windows-machines"></a>Windows makineleri

- Çoğaltmayı etkinleştirmek istediğiniz VM ile işlem sunucusunu çalıştıran makine arasında ağ bağlantısına ihtiyacınız vardır (varsayılan olarak bu yapılandırma sunucusu VM'dir).
- Çoğaltmayı etkinleştirdiğiniz makinede yönetici hakları (etki alanı veya yerel) içeren bir hesaba ihtiyacınız vardır.
    - Site Kurtarma'yı ayarlarken bu hesabı belirtirsiniz. Daha sonra işlem sunucusu çoğaltma etkinleştirildiğinde Mobilite hizmetini yüklemek için bu hesabı kullanır.
    - Bu hesap yalnızca itme yüklemesi ve Mobilite hizmetini güncelleştirmek için Site Kurtarma tarafından kullanılacaktır.
    - Bir etki alanı hesabı kullanmıyorsanız, VM'deki Uzaktan Kullanıcı Erişimi denetimini devre dışı kaldırmanız gerekir:
        - Kayıt defterinde, HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System altında DWORD değeri **LocalAccountTokenFilterPolicy** oluşturun.
        - Değeri 1 olarak ayarlayın.
        - Komut isteminde bunu yapmak için aşağıdakileri yazın: **REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1**.
- VM'deki Windows Güvenlik Duvarı'nda çoğaltmak istediğiniz dosya ve yazıcı paylaşımına ve WMI'ye izin verin.
    - Bunu yapmak için Windows Güvenlik Duvarı konsolu açmak için **wf.msc** çalıştırın. Sağ tıklayın **Gelen Kurallar** > **Yeni Kural**. **Önceden Tanımlanmış'ı**seçin ve listeden Dosya ve **Yazıcı paylaşımını** seçin. Sihirbazı tamamlayın, **bitiş**> bağlantıya izin vermeyi seçin.
    - Etki alanı bilgisayarları için bunu yapmak için bir GPO kullanabilirsiniz.

    
#### <a name="linux-machines"></a>Linux makineleri

- Linux bilgisayar ile işlem sunucusu arasında ağ bağlantısı bulunduğundan emin olun.
- Çoğaltmayı etkinleştirdiğiniz makinede, kaynak Linux sunucusunda kök kullanıcı olan bir hesaba ihtiyacınız vardır:
    - Site Kurtarma'yı ayarlarken bu hesabı belirtirsiniz. Daha sonra işlem sunucusu çoğaltma etkinleştirildiğinde Mobilite hizmetini yüklemek için bu hesabı kullanır.
    - Bu hesap yalnızca itme yüklemesi ve Mobilite hizmetini güncelleştirmek için Site Kurtarma tarafından kullanılacaktır.
- Kaynak Linux sunucusundaki /etc/hosts dosyasında, yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler olduğunu denetleyin.
- Çoğaltmak istediğiniz bilgisayara en son openssh, openssh-server, openssl paketlerini yükleyin.
- Secure Shell’in (SSH) etkin olduğundan ve bağlantı noktası 22’de çalıştırıldığından emin olun.
- SFTP alt sistemi ve parola ile kimlik doğrulamasını sshd_config dosyasında etkinleştirin:
    1. Bunu yapmak için kök olarak oturum açın.
    2. **PasswordAuthentication**ile başlayan satırı /etc/ssh/sshd_config dosyasında bulun. Satırı açıklama durumundan çıkarın ve değerini **yes** (evet) olarak değiştirin.
    3. **Subsystem** ile başlayan satırı bulun ve açıklama durumundan çıkarın.

        ![Linux Mobility hizmeti](./media/azure-stack-site-recovery/linux-mobility.png)

    4. sshd hizmetini yeniden başlatın.


### <a name="note-the-vm-private-ip-address"></a>VM özel IP adresine dikkat edin

Çoğaltmak istediğiniz her makine için IP adresini bulun:

1. Azure Yığın Portalı'nda VM'yi tıklatın.
2. **Kaynak** menüsünde **Ağ Arabirimleri'ni**tıklatın.
3. Özel IP adresini not edin.

    ![Özel IP adresi](./media/azure-stack-site-recovery/private-ip.png)


## <a name="step-2-create-a-vault-and-select-a-replication-goal"></a>Adım 2: Bir tonoz oluşturun ve bir çoğaltma hedefi seçin

1. Azure portalında, **kaynak** > **Yönetim Araçları** > **Yedekleme ve Site Kurtarma**oluştur'u seçin.
2. **Ad** alanına kasayı tanımlamak için kolay bir ad girin. 
3. **Kaynak grubunda**bir kaynak grubu oluşturun veya seçin. **ContosoRG**kullanıyoruz.
4. **Konum'da,** Azure bölgesini girin. **Batı Avrupa** kullanacağız.
5. Panodan kasaya hızlı bir şekilde erişmek **için, Oluştur'a Pin'i** > **Create**seçin.

   ![Yeni kasa oluştur](./media/azure-stack-site-recovery/new-vault-settings.png)

   Yeni kasa **Pano** > **Tüm kaynaklarda**ve ana **Kurtarma Hizmetleri kasaları** sayfasında görünür.

### <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. **Kurtarma Hizmetleri kasalarında** bir kasa adı belirtmek >. **ContosoVMVault**kullanıyoruz.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Daha sonra **Altyapıyı Hazırlama**’yı seçin.
3. **Koruma hedefiNde** > **Makineleriniz nerede bulunur**, şirket **içi**seçin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırılmış mı,** **sanallaştırılan/Diğer değil'i**seçin. Sonra **Tamam**’ı seçin.

    ![Koruma hedefi](./media/azure-stack-site-recovery/protection-goal.png)

## <a name="step-3-set-up-the-source-environment"></a>Adım 3: Kaynak ortamını ayarlama

Yapılandırma sunucusu makinesini ayarlayın, kasaya kaydedin ve çoğaltmak istediğiniz makineleri keşfedin.

1. Altyapı**Kaynağını** **Hazırla'yı** > tıklatın.
2. **Kaynağı hazırla** bölümünde **+ Yapılandırma Sunucusu**’na tıklayın.

    ![Kaynağı ayarlama](./media/azure-stack-site-recovery/plus-config-srv.png)

3. **Add Server'da**Configuration **Server'ın** **Sunucu türünde**görünip görünmediğini denetleyin.
5. Site Kurtarma Birleşik Kurulum yükleme dosyasını indirin.
6. Kasa kayıt anahtarını indirin. Birleşik Kurulum'u çalıştırDığınızda kayıt anahtarına ihtiyacınız vardır. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/azure-stack-site-recovery/set-source2.png)


### <a name="run-azure-site-recovery-unified-setup"></a>Azure Site Kurtarma Birleşik Kurulum'u çalıştırın

Yapılandırma sunucusunu yüklemek ve kaydetmek için yapılandırma sunucusu için kullanmak istediğiniz VM'ye bir RDP bağlantısı yapın ve Birleşik Kurulum'u çalıştırın.

Başlamadan önce saatin VM'deki [bir zaman sunucusuyla eşitlendirildiä](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) inden emin olun. Zaman yerel saate beş dakikadan fazla kapalıysa yükleme başarısız olur.

Şimdi yapılandırma sunucusu yükleyin:

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Yapılandırma sunucusu komut satırından da yüklenebilir. [Daha fazla bilgi edinin](physical-manage-configuration-server.md#install-from-the-command-line).
> 
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirmek için **Configuration Servers** > ***sunucu adını*** > **yenile Server'ı**seçin.

## <a name="step-4-set-up-the-target-environment"></a>Adım 4: Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapı Hedef Hazırla'da,** > **Target**kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler. Bunları bulamazsa, sihirbazı tamamlamak için en az bir depolama hesabı ve sanal ağ oluşturmanız gerekir.


## <a name="step-5-enable-replication"></a>5. Adım: Çoğaltmayı etkinleştirme

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Altyapı**Çoğaltma Ayarlarını** **Hazırla'yı** > tıklatın.
2. **Çoğaltma ilkesi oluştur** bölümünde bir ilke adı belirtin.
3. **RPO eşiği** bölümünde kurtarma noktası hedefi (RPO) sınırını belirtin.
    - Çoğaltılan verilerin kurtarma noktaları zaman kümesine göre oluşturulur.
    - Bu ayar, sürekli olan çoğaltmayı etkilemez. Yalnızca, bir kurtarma noktası oluşturulmadan eşik sınırına ulaşılırsa bir uyarı sağlar.
4. **Kurtarma noktası bekletmede,** her kurtarma noktasının ne kadar süreyle tutulduğunu belirtin. Çoğaltılan VM'ler belirtilen zaman penceresindeki herhangi bir noktaya kurtarılabilir.
5. **Uygulama tutarlı anlık görüntü sıklığında,** uygulama tutarlı anlık görüntüoluşturma sıklığını belirtin.

    - Uygulama tutarlı anlık görüntüsü, VM içindeki uygulama verilerinin anlık görüntüsüdür.
    - Birim Gölge Kopyalama Hizmeti (VSS), anlık görüntü alındığında VM'deki uygulamaların tutarlı bir durumda olmasını sağlar.
6. İlkeyi oluşturmak için **Tamam**’ı seçin.


### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Bu adımı hemen atlayabilirsin. **Dağıtım Planlama** açılır listesinde, Evet'i **tıklatın, ben yaptım.**



### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

[Adım 1: Makineyi hazırlayın'daki](#step-1-prepare-azure-stack-vms)tüm görevleri tamamladığınızdan emin olun. Ardından çoğaltmayı aşağıdaki gibi etkinleştirin:

1. Uygulama**Kaynağını** **Çoğaltma'yı** > seçin.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. **Makine türünde,** **Fiziksel makineleri**seçin.
4. İşlem sunucusunu (yapılandırma sunucusu) seçin. Ardından **Tamam**'a tıklayın.
5. **Hedef'te,** başarısız olduktan sonra VM'leri oluşturmak istediğiniz abonelik ve kaynak grubunu seçin. Başarısız olan VM'ler için kullanmak istediğiniz dağıtım modelini seçin.
6. Çoğaltılan verileri depolamak istediğiniz Azure depolama hesabını seçin.
7. Yük devretme işleminden sonra oluşturulan Azure VM’lerin bağlandığı Azure ağını ve alt ağını seçin.
8. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandırı** seçin. Her makine için Azure ağını ayrı ayrı seçmek istiyorsanız **daha sonra Yapılandır'ı** seçin.
9. **Fiziksel Makinelerde** **+Fiziksel makineyi**tıklatın. Çoğaltmak istediğiniz her makinenin adını, IP adresini ve işletim sistemi türünü belirtin.

    - Makinenin dahili IP adresini kullanın.
    - Genel IP adresini belirtirseniz, çoğaltma beklendiği gibi çalışmayabilir.

10. **Özellikleri** > **Yapılandırma özelliklerinde,** işlem sunucusunun makineye Mobilite Hizmeti'ni otomatik olarak yüklemek için kullanacağı hesabı seçin.
11. **Çoğaltma ayarlarında** > **çoğaltma ayarlarını yapılandırın,** doğru çoğaltma ilkesinin seçilip seçilmediğini denetleyin.
12. **Çoğaltmayı Etkinleştir**’e tıklayın.
13.  >  **Ayarlar** > **İşleri****Sitesi Kurtarma İşlerinde** **KorumaYı Etkinleştir** işinin ilerlemesini izleyin. **Finalize Koruma** işi çalıştırdıktan sonra, makine başarısız olmaya hazırdır.

> [!NOTE]
> Bir VM için çoğaltma etkinleştirildiğinde Site Recovery, Mobility Hizmeti’ni yükler.
> 
> Değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.
> 
> Eklediğiniz VM'leri izlemek için, Configuration Servers > Son Kişi**adresindeki**VM'ler için keşfedilen son zamanı kontrol **edin.** Zamanlanan bulma işlemini beklemeden VM’leri eklemek için yapılandırma sunucusunu vurgulayın (seçmeyin) ve **Yenile**’yi seçin.


## <a name="step-6-run-a-disaster-recovery-drill"></a>Adım 6: Bir felaket kurtarma matkap çalıştırın

Her şeyin beklendiği gibi çalıştığından emin olmak için Azure'da bir test başarısızlığı çalıştırın. Bu hata üretim ortamınızı etkilemez.

### <a name="verify-machine-properties"></a>Makine özelliklerini doğrulama

Bir test başarısızlığını çalıştırmadan önce, makine özelliklerini doğrulayın ve [Azure gereksinimlerine](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements)uyduklarına emin olun. Özellikleri aşağıdaki gibi görüntüleyebilir ve değiştirebilirsiniz:

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.
2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
3. **Bilgi İşlem ve Ağ'da,** ayarları gerektiği gibi değiştirin.

    - Azure VM adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesini](../virtual-machines/windows/tutorial-availability-sets.md)ve yönetilen disk ayarlarını değiştirebilirsiniz.
    - Ağ ayarlarını görüntüleyebilir ve değiştirebilirsiniz. Bunlar arasında Azure VM'nin başarısız olduktan sonra birleştiği ağ/alt ağ ve VM'ye atanacak IP adresi yer alıyor.
1. **Disklerde,** VM'deki işletim sistemi ve veri diskleri hakkındaki bilgileri görüntüleyin.
   

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda şunlar olur:

1. Yük devretme için gerekli tüm koşulların karşılandığından emin olmak için bir önkoşul denetimi çalıştırılır.
2. Failover, verileri belirtilen kurtarma noktasını kullanarak işler:
    - **En son işlenmiş**: Makine, Site Kurtarma tarafından işlenen en son kurtarma noktasına kadar başarısız olur. Zaman damgası gösterilir. Bu seçenekte, verileri işlemek için zaman harcanmadığından düşük bir RTO (kurtarma süresi hedefi) sağlanır.
    - **En son uygulama tutarlılığı**: Makine en son uygulama tutarlı kurtarma noktasına kadar başarısız olur.
    - **Özel**: Başarısız olmak için kullanılan kurtarma noktasını seçin.

3. İşlenen veriler kullanılarak bir Azure VM oluşturulur.
4. Test başarısız mı, alıştırma sırasında oluşturulan Azure VM'lerini otomatik olarak temizleyebilir.

VM için aşağıdaki gibi bir test başarısızlığını çalıştırın:

1. **Yinelenen Ayarlar'da** > **Replicated Items**VM > **+Test Failover'ı**tıklatın.
2. Bu izlenme için, en son **işlenmiş** kurtarma noktasını kullanmayı seçeriz. 
3. **Test Failover'da**hedef Azure ağını seçin.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın.
5. Özelliklerini açmak için VM'ye tıklayarak ilerlemeyi izleyin. Veya, *vault adı* > **Ayarlar** > **İşler** >**Sitesi Kurtarma işlerinde**Test **Failover** iş tıklatın.
6. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM'nin uygun boyutta olup olmadığını, doğru ağa bağlı olup olmadığını ve çalıştığını denetleyin.
7. Şimdi Azure’da çoğaltılan sanal makineye bağlanabiliyor olmanız gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-test-failover-to-azure#prepare-to-connect-to-azure-vms-after-failover).
8. Yük devretme testi sırasında oluşturulan Azure sanal makinelerini silmek için, VM’de **Yük devretme testini temizle**’ye tıklayın. **Notlar,test**başarısızlıkla ilişkili tüm gözlemleri kaydedin.

## <a name="fail-over-and-fail-back"></a>Yük devretme ve ilk duruma döndürme

Çoğaltmayı ayarladıktan ve her şeyin çalıştığından emin olmak için bir matkap çalıştırdıktan sonra, gerektiğinde Makinelerazure'da başarısız olabilirsiniz.

Bir başarısızlık çalıştırmadan önce, başarısız olduktan sonra Azure'daki makineye bağlanmak istiyorsanız, başlamadan önce [bağlanmaya hazırolun.](https://docs.microsoft.com/azure/site-recovery/site-recovery-test-failover-to-azure#prepare-to-connect-to-azure-vms-after-failover)

Sonra aşağıdaki gibi bir failover çalıştırın:


1. **Ayarlar** > **Çoğaltılan Öğeler,** **Failover**> makineyi tıklatın.
2. Kullanmak istediğiniz kurtarma noktasını seçin.
3. **Test Failover'da**hedef Azure ağını seçin.
4. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Bu ayar ile, Site Kurtarma failover başlamadan önce kaynak makine kapatmaya çalışır. Ancak kapatma başarısız olsa bile hata devam eder. 
5. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
6. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. Başarısız olduktan sonra bağlanmaya hazırlandıysanız, VM'nin uygun boyutta, doğru ağa bağlı ve çalışan olduğundan kontrol edin.
7. VM'yi doğruladıktan sonra, başarısızı tamamlamak için **Commit'i** tıklatın. Bu, kullanılabilir tüm kurtarma noktalarını siler.

> [!WARNING]
> Devam eden yük devretme işlemini iptal etmeyin: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılamaz.


### <a name="fail-back-to-azure-stack"></a>Azure Yığınına geri dön

Birincil siteniz yeniden çalışmaya başladığında, Azure'dan Azure Yığını'na geri dönebilirsiniz. Bunu yapmak için Azure VM VHD'yi indirmeniz ve Azure Yığını'na yüklemeniz gerekir.

1. VHD'nin indirilebilmeleri için Azure VM'yi kapatın. 
2. VHD'yi indirmeye başlamak için [Azure Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/)yükleyin.
3. Azure Portalı'ndaki VM'ye gidin (VM adını kullanarak).
4. **Disklerde,** disk adını tıklatın ve ayarları toplamak.

    - Örnek olarak, testimizde kullanılan VHD https://502055westcentralus.blob.core.windows.net/wahv9b8d2ceb284fb59287/copied-3676553984.vhd URI: VHD'yi indirmek için kullanılan aşağıdaki giriş parametrelerini elde etmek için bölünebilir.
        - Depolama Hesabı: 502055westcentralus
        - Konteyner: wahv9b8d2ceb284fb59287
        - VHD Adı: kopyalanmış-3676553984.vhd

5. Şimdi, VHD'yi indirmek için Azure Depolama Gezgini'ni kullanın.
6. Bu adımlarla VHD'yi Azure [Yığını'na](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-disks#use-powershell-to-add-multiple-disks-to-a-vm)yükleyin.
7. Varolan VM veya yeni VM'de yüklenen VHD'leri takın.
8. Os Diskinin doğru olup olmadığını kontrol edin ve VM'yi başlatın.


Bu aşamada geri dönüş tamamlandı.


## <a name="conclusion"></a>Sonuç

Bu makalede Azure Stack VM'leri Azure'a çoğalttın. Çoğaltma nın devreye sayılması yla, Azure'un beklendiği gibi çalıştığından emin olmak için bir olağanüstü durum kurtarma tatbikatı yaptık. Makalede, Azure'da tam bir hata yapmak ve Azure Yığını'na geri dönmeme adımları da yer alıyor.

## <a name="next-steps"></a>Sonraki adımlar

Geri başarısız olduktan sonra, VM'yi yeniden koruyabilir ve bunu yapmak için yeniden Azure'da çoğaltmaya başlayabilir ve bu makaledeki adımları yineleyebilirsiniz.

