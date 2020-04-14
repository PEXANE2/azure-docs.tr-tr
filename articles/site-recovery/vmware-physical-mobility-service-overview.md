---
title: Azure Site Kurtarma ile VMware VM'lerin ve fiziksel sunucuların olağanüstü kurtarma için Mobilite hizmeti hakkında | Microsoft Dokümanlar
description: Azure Site Kurtarma hizmetini kullanarak VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü kurtarma hizmeti için Mobilite hizmet aracısı hakkında bilgi edinin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: how-to
ms.date: 04/10/2020
ms.author: ramamill
ms.openlocfilehash: ec4d1cfbe0c76c8245c4beeaa7c044d76d917a7a
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259823"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>VMware VM'ler ve fiziksel sunucular için Mobilite hizmeti hakkında

[Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak VMware sanal makineleri (VM) ve fiziksel sunucular için olağanüstü durum kurtarma ayarladığınızda, Site Kurtarma Mobilite hizmetini şirket içi VM VM ve fiziksel sunucuya yüklersiniz. Mobilite hizmeti, makinedeki verileri yakalar ve bunları Site Kurtarma işlem sunucusuna iletir. Mobilite hizmeti, aşağıdaki yöntemleri kullanarak dağıtabileceğiniz Mobility servis aracısı yazılımı tarafından yüklenir:

- [Push yükleme](#push-installation): Azure portalı üzerinden koruma etkinleştirildiğinde, Site Kurtarma Mobilite hizmetini sunucuya yükler.
- Manuel yükleme: Mobilite hizmetini [kullanıcı arabirimi (UI)](#install-the-mobility-service-using-ui) veya [komut istemi](#install-the-mobility-service-using-command-prompt)aracılığıyla her makineye el ile yükleyebilirsiniz.
- [Otomatik dağıtım](vmware-azure-mobility-install-configuration-mgr.md): Configuration Manager gibi yazılım dağıtım araçlarıyla Mobilite hizmet yüklemesini otomatikleştirebilirsiniz.

> [!NOTE]
> Mobilite hizmeti, VMware VM'ler veya fiziksel makineler için kaynak makinelerde belleğin yaklaşık %6-10'unu kullanır.

## <a name="antivirus-on-replicated-machines"></a>Çoğaltılan makinelerde virüsten koruma

Çoğaltmak istediğiniz makineler virüsten koruma yazılımı çalıştırıyorsa, Mobility hizmetinin yükleme klasörü _C:\ProgramData\ASR\agent'ı_ virüsten koruma işlemlerinden hariç tutar. Bu dışlama, çoğaltmanın beklendiği gibi çalışmasını sağlar.

## <a name="push-installation"></a>İtme yüklemesi

Push yükleme, [çoğaltmayı etkinleştirmek](vmware-azure-enable-replication.md#enable-replication)için Azure portalından çalıştırılabilen işin ayrılmaz bir parçasıdır. Korumak ve çoğaltmayı etkinleştirmek istediğiniz VM kümesini seçtikten sonra, yapılandırma sunucusu Mobilite hizmet aracısını sunuculara iter, aracıyı yükler ve aracının yapılandırma sunucusuna kaydını tamamlar.

### <a name="prerequisites"></a>Ön koşullar

- Tüm itme yükleme [ön koşullarının](vmware-azure-install-mobility-service.md) karşılandığından emin olun.
- [VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü kurtarma durumu için](vmware-physical-azure-support-matrix.md)destek matrisindeki ölçütleri tüm sunucu yapılandırmalarının karşıladığını sağlayın.

Push installation iş akışı aşağıdaki bölümlerde açıklanmıştır:

### <a name="mobility-service-agent-version-923-and-higher"></a>Mobilite servis aracısı sürüm 9.23 ve üzeri

Sürüm 9.23 hakkında daha fazla bilgi [için Azure Site Kurtarma için Güncelleştirme Rollup 35'e](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery)bakın.

Mobilit hizmetinin itme yüklemesi sırasında aşağıdaki adımlar gerçekleştirilir:

1. Ajan kaynak makineye itilir. Aracının kaynak makineye kopyalanması, birden çok çevresel hata nedeniyle başarısız olabilir. Anında geçirme yükleme hatalarını gidermek için kılavuzumuzu ziyaret [edin.](vmware-azure-troubleshoot-push-install.md)
1. Aracı sunucuya başarıyla kopyalandıktan sonra sunucuda ön koşul denetimi gerçekleştirilir.
   - Tüm ön koşullar karşılanırsa, yükleme başlar.
   - [Ön koşullardan](vmware-physical-azure-support-matrix.md) biri veya daha fazlası karşılanmazsa yükleme başarısız olur.
1. Aracı yüklemenin bir parçası olarak, Azure Site Kurtarma için Toplu Gölge Kopyalama Hizmeti (VSS) sağlayıcısı yüklenir. VSS sağlayıcısı, uygulama tutarlı kurtarma noktaları oluşturmak için kullanılır. VSS sağlayıcısının yüklenmesi başarısız olursa, bu adım atlanır ve aracı yüklemesi devam eder.
1. Aracı yükleme başarılı olursa ancak VSS sağlayıcı yükleme başarısız olursa, iş durumu **Uyarı**olarak işaretlenir. Bu, çarpışma tutarlı kurtarma noktası oluşturma etkilemez.

    - Uygulama tutarlı kurtarma noktaları oluşturmak için, Site Kurtarma VSS sağlayıcısının el ile yüklemesini tamamlamak için [kılavuzumuza](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) bakın.
    - Uygulama tutarlı kurtarma noktaları oluşturmak istemiyorsanız, uygulama tutarlı kurtarma noktalarını kapatmak için [çoğaltma ilkesini değiştirin.](vmware-azure-set-up-replication.md#create-a-policy)

### <a name="mobility-service-agent-version-922-and-below"></a>Mobilite servis aracısı sürüm 9.22 ve altı

1. Ajan kaynak makineye itilir. Aracının kaynak makineye kopyalanması, birden çok çevresel hata nedeniyle başarısız olabilir. Anında geçiryükleme hatalarını gidermek için [kılavuzumuza](vmware-azure-troubleshoot-push-install.md) bakın.
1. Aracı sunucuya başarıyla kopyalandıktan sonra sunucuda ön koşul denetimi gerçekleştirilir.
   - Tüm ön koşullar karşılanırsa, yükleme başlar.
   - [Ön koşullardan](vmware-physical-azure-support-matrix.md) biri veya daha fazlası karşılanmazsa yükleme başarısız olur.

1. Aracı yüklemenin bir parçası olarak, Azure Site Kurtarma için Toplu Gölge Kopyalama Hizmeti (VSS) sağlayıcısı yüklenir. VSS sağlayıcısı, uygulama tutarlı kurtarma noktaları oluşturmak için kullanılır.
   - VSS sağlayıcı yüklemesi başarısız olursa, aracı yüklemesi başarısız olur. Aracı yüklemenin başarısız olmasını önlemek için, çökme tutarlı kurtarma noktaları oluşturmak ve VSS sağlayıcısının el ile yüklemesini yapmak için [sürüm 9.23](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery) veya daha yüksek kullanın.

## <a name="install-the-mobility-service-using-ui"></a>UI kullanarak Mobilite hizmetini yükleme

### <a name="prerequisites"></a>Ön koşullar

- [VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü kurtarma durumu için](vmware-physical-azure-support-matrix.md)destek matrisindeki ölçütleri tüm sunucu yapılandırmalarının karşıladığını sağlayın.
- Sunucunun işletim sistemi için [yükleyiciyi bulun.](#locate-installer-files)

>[!IMPORTANT]
> Bir Azure Altyapısını Hizmet (IaaS) VM olarak bir Azure bölgesinden diğerine kopyalayan kişi, UI yükleme yöntemini kullanmayın. Komut [istemi](#install-the-mobility-service-using-command-prompt) yüklemesini kullanın.

1. Yükleme dosyasını makineye kopyalayın ve çalıştırın.
1. **Yükleme**Seçeneği'nde, **Mobilite Hizmetini Yükle'yi**seçin.
1. Yükleme konumunu seçin ve **Yükle'yi**seçin.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility1.png" alt-text="Mobilite hizmeti yükleme seçeneği sayfası.":::

1. **Yükleme İlerlemesinde**yüklemeyi izleyin. Yükleme tamamlandıktan sonra, hizmeti yapılandırma sunucusuna kaydetmek için **Yapılandırmaya Devam** Et'i seçin.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility3.png" alt-text="Mobilite hizmeti kayıt sayfası.":::

1. **Configuration Server Ayrıntıları'nda,** yapılandırdığınız IP adresini ve parolayı belirtin.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility4.png" alt-text="Mobilite hizmeti kayıt sayfası.":::

1. Kaydı bitirmek için **Kaydol'u** seçin.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility5.png" alt-text="Mobilite hizmeti kayıt son sayfası.":::

## <a name="install-the-mobility-service-using-command-prompt"></a>Komut istemini kullanarak Mobilite hizmetini yükleme

### <a name="prerequisites"></a>Ön koşullar

- [VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü kurtarma durumu için](vmware-physical-azure-support-matrix.md)destek matrisindeki ölçütleri tüm sunucu yapılandırmalarının karşıladığını sağlayın.
- Sunucunun işletim sistemi için [yükleyiciyi bulun.](#locate-installer-files)

### <a name="windows-machine"></a>Windows makinesi

- Komut isteminden, yüklemekçiyi korumak istediğiniz sunucuda _C:\Temp_gibi yerel bir klasöre kopyalamak için aşağıdaki komutları çalıştırın. Yükleyicinin dosya adını gerçek dosya adıyla değiştirin.

  ```cmd
  cd C:\Temp
  ren Microsoft-ASR_UA_version_Windows_GA_date_release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted
  ```

- Aracıyı yüklemek için bu komutu çalıştırın.

  ```cmd
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```

- Aracıyı yapılandırma sunucusuna kaydettirmek için bu komutları çalıştırın.

  ```cmd
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="installation-settings"></a>Yükleme ayarları

Ayar | Ayrıntılar
--- | ---
Sözdizimi | `UnifiedAgent.exe /Role \<MS/MT> /InstallLocation \<Install Location> /Platform "VmWare" /Silent`
Kurulum günlükleri | `%ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log`
`/Role` | Zorunlu kurulum parametresi. Mobilite hizmetinin (MS) veya ana hedefin (MT) yüklenip yüklenmeyeceğini belirtir.
`/InstallLocation`| İsteğe bağlı parametre. Mobilite hizmeti yükleme konumunu (herhangi bir klasör) belirtir.
`/Platform` | Zorunlu. Mobilite hizmetinin yüklendiği platformu belirtir: <br/> **VMware VMware/fiziksel** sunucular için. <br/> Azure VM'ler için **Azure.**<br/><br/> Azure VM'leri fiziksel makine olarak ele alıyorsanız, **VMware'i**belirtin.
`/Silent`| İsteğe bağlı. Yükleyicinin sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları

Ayar | Ayrıntılar
--- | ---
Sözdizimi | `UnifiedAgentConfigurator.exe  /CSEndPoint \<CSIP> /PassphraseFilePath \<PassphraseFilePath>`
Aracı yapılandırma günlükleri | `%ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log`
`/CSEndPoint` | Zorunlu parametre. `<CSIP>`yapılandırma sunucusunun IP adresini belirtir. Geçerli bir IP adresi kullanın.
`/PassphraseFilePath` |  Zorunlu. Parolanın konumu. Geçerli UNC veya yerel dosya yolunu kullanın.

### <a name="linux-machine"></a>Linux makinesi

1. Bir terminal oturumundan, yükleyiciyi korumak istediğiniz sunucudaki _/tmp_ gibi yerel bir klasöre kopyalayın. Yükleyicinin dosya adını Linux dağıtımınızın gerçek dosya adıile değiştirin ve komutları çalıştırın.

   ```shell
   cd /tmp ;
   tar -xvf Microsoft-ASR_UA_version_LinuxVersion_GA_date_release.tar.gz
   ```

2. Aşağıdaki gibi yükleyin:

   ```shell
   sudo ./install -d <Install Location> -r MS -v VmWare -q
   ```

3. Yükleme tamamlandıktan sonra, Mobilite hizmetiyapılandırma sunucusuna kaydedilmelidir. Mobilite hizmetini yapılandırma sunucusuna kaydetmek için aşağıdaki komutu çalıştırın.

   ```shell
   /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
   ```

#### <a name="installation-settings"></a>Yükleme ayarları

Ayar | Ayrıntılar
--- | ---
Sözdizimi | `./install -d \<Install Location> -r \<MS/MT> -v VmWare -q`
`-r` | Zorunlu kurulum parametresi. Mobilite hizmetinin (MS) veya ana hedefin (MT) yüklenip yüklenmeyeceğini belirtir.
`-d` | İsteğe bağlı parametre. Mobilite hizmeti yükleme konumunu `/usr/local/ASR`belirtir: .
`-v` | Zorunlu. Mobilite hizmetinin yüklendiği platformu belirtir. <br/> **VMware VMware/fiziksel** sunucular için. <br/> Azure VM'ler için **Azure.**
`-q` | İsteğe bağlı. Yükleyicinin sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları

Ayar | Ayrıntılar
--- | ---
Sözdizimi | `cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i \<CSIP> -P \<PassphraseFilePath>`
`-i` | Zorunlu parametre. `<CSIP>`yapılandırma sunucusunun IP adresini belirtir. Geçerli bir IP adresi kullanın.
`-P` |  Zorunlu. Parolanın kaydedildiği dosyanın tam dosya yolu. Geçerli bir klasör kullanın.

## <a name="azure-virtual-machine-agent"></a>Azure Sanal Makine aracısı

- **Windows VM'ler**: Mobilite hizmetinin 9.7.0.0 sürümünden, [Azure VM aracısı](/azure/virtual-machines/extensions/features-windows#azure-vm-agent) Mobilite hizmet yükleyicisi tarafından yüklenir. Bu, makine Azure'a geçemediğinde, Azure VM'nin herhangi bir VM uzantısını kullanmak için aracı yükleme ön koşuluyla karşılamasını sağlar.
- **Linux VM'leri**: [WALinuxAgent,](/azure/virtual-machines/extensions/update-linux-agent) başarısız olduktan sonra Azure VM'ye el ile yüklenmelidir.

## <a name="locate-installer-files"></a>Yükleyici dosyalarını bulma

Yapılandırma sunucusunda _%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository_klasörüne gidin. İşletim sistemine bağlı olarak hangi yükleyiciye ihtiyacınız olduğunu kontrol edin. Aşağıdaki tablo, her VMware VM ve fiziksel sunucu işletim sistemi için yükleyici dosyalarını özetler. Başlamadan önce desteklenen işletim [sistemlerini](vmware-physical-azure-support-matrix.md#replicated-machines)gözden geçirebilirsiniz.

> [!NOTE]
> Dosya adları, sürüm _ve_ _tarih_ ile birlikte aşağıdaki tabloda gösterilen sözdizimini gerçek değerler için yer tutucu olarak kullanır. Gerçek dosya adları aşağıdaki örneklere benzer:
> - `Microsoft-ASR_UA_9.30.0.0_Windows_GA_22Oct2019_release.exe`
> - `Microsoft-ASR_UA_9.30.0.0_UBUNTU-16.04-64_GA_22Oct2019_release.tar.gz`

Yükleyici dosyası | İşletim sistemi (yalnızca 64 bit)
--- | ---
`Microsoft-ASR_UA_version_Windows_GA_date_release.exe` | Windows Server 2016 </br> Windows Server 2012 R2 </br> Windows Server 2012 </br> Windows Server 2008 R2 SP1
`Microsoft-ASR_UA_version_RHEL6-64_GA_date_release.tar.gz` | Kırmızı Şapka Kurumsal Linux (RHEL) 6 </br> CentOS 6
`Microsoft-ASR_UA_version_RHEL7-64_GA_date_release.tar.gz` | Kırmızı Şapka Kurumsal Linux (RHEL) 7 </br> CentOS 7
`Microsoft-ASR_UA_version_SLES12-64_GA_date_release.tar.gz` | SUSE Linux Kurumsal Sunucu 12 SP1 </br> SP2 ve SP3 içerir.
`Microsoft-ASR_UA_version_SLES11-SP3-64_GA_date_release.tar.gz` | SUSE Linux Kurumsal Sunucu 11 SP3
`Microsoft-ASR_UA_version_SLES11-SP4-64_GA_date_release.tar.gz` | SUSE Linux Kurumsal Sunucu 11 SP4
`Microsoft-ASR_UA_version_OL6-64_GA_date_release.tar.gz` | Oracle Kurumsal Linux 6.4 </br> Oracle Kurumsal Linux 6.5
`Microsoft-ASR_UA_version_UBUNTU-14.04-64_GA_date_release.tar.gz` | Ubuntu Linux 14.04
`Microsoft-ASR_UA_version_UBUNTU-16.04-64_GA_date_release.tar.gz` | Ubuntu Linux 16.04 LTS sunucusu
`Microsoft-ASR_UA_version_DEBIAN7-64_GA_date_release.tar.gz` | Debian 7
`Microsoft-ASR_UA_version_DEBIAN8-64_GA_date_release.tar.gz` | Debian 8

## <a name="next-steps"></a>Sonraki adımlar

[Mobilite hizmeti için push yüklemesini ayarlayın.](vmware-azure-install-mobility-service.md)
