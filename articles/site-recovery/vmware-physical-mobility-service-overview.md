---
title: Azure Site Recovery ile VMware VM 'Leri ve fiziksel sunucuları olağanüstü durum kurtarmaya yönelik Mobility hizmeti hakkında | Microsoft Docs
description: Azure Site Recovery hizmetini kullanarak VMware VM 'Leri ve fiziksel sunucuları Azure 'a olağanüstü durum kurtarmaya yönelik Mobility hizmeti Aracısı hakkında bilgi edinin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: how-to
ms.date: 04/10/2020
ms.author: ramamill
ms.openlocfilehash: ec4d1cfbe0c76c8245c4beeaa7c044d76d917a7a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81259823"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>VMware VM 'Leri ve fiziksel sunucular için Mobility hizmeti hakkında

[Azure Site Recovery](site-recovery-overview.md)kullanarak VMware sanal MAKINELERI (VM) ve fiziksel sunucular için olağanüstü durum kurtarmayı ayarlarken, her şirket içi VMware VM 'sine ve fiziksel sunucuya Site Recovery Mobility hizmetini yüklersiniz. Mobility hizmeti makinede veri yazmaları yakalar ve bunları Site Recovery işlem sunucusuna iletir. Mobility hizmeti, aşağıdaki yöntemleri kullanarak dağıtabileceğiniz Mobility hizmet Aracısı yazılımı tarafından yüklenir:

- [Anında yükleme](#push-installation): koruma Azure Portal aracılığıyla etkinleştirildiğinde, Site Recovery Mobility hizmetini sunucuya yüklenir.
- El ile yükleme: Mobility hizmetini, [Kullanıcı arabirimi (UI)](#install-the-mobility-service-using-ui) veya [komut istemi](#install-the-mobility-service-using-command-prompt)aracılığıyla her makineye el ile yükleyebilirsiniz.
- [Otomatik dağıtım](vmware-azure-mobility-install-configuration-mgr.md): Configuration Manager gibi yazılım dağıtım araçlarıyla Mobility hizmeti yüklemesini otomatik hale getirebilirsiniz.

> [!NOTE]
> Mobility hizmeti, VMware VM 'Leri veya fiziksel makineler için kaynak makinelerde yaklaşık %6-%10 bellek kullanır.

## <a name="antivirus-on-replicated-machines"></a>Çoğaltılan makinelerde virüsten koruma

Çoğaltmak istediğiniz makineler virüsten koruma yazılımı çalıştırıyorsa, Mobility hizmetinin _C:\programdata\asr\agent_ yükleme klasörünü virüsten koruma işlemlerinden hariç tutun. Bu dışlama, çoğaltmanın beklendiği gibi çalışmasını sağlar.

## <a name="push-installation"></a>Gönderim yükleme

Anında yükleme, [çoğaltmayı etkinleştirmek](vmware-azure-enable-replication.md#enable-replication)için Azure Portal çalıştırılan işin integral bir parçasıdır. Korumak ve çoğaltmayı etkinleştirmek istediğiniz VM kümesini seçtikten sonra, yapılandırma sunucusu Mobility hizmet aracısını sunuculara gönderir, aracıyı kurar ve yapılandırma sunucusu ile aracının kaydını tamamlar.

### <a name="prerequisites"></a>Ön koşullar

- Tüm gönderme yükleme [önkoşullarının](vmware-azure-install-mobility-service.md) karşılandığından emin olun.
- Tüm sunucu yapılandırmalarının, [VMware VM 'leri ve fiziksel sunucuları Azure 'a olağanüstü durum kurtarma Için destek matrisindeki](vmware-physical-azure-support-matrix.md)ölçütlere uygun olduğundan emin olun.

Anında yükleme iş akışı aşağıdaki bölümlerde açıklanmıştır:

### <a name="mobility-service-agent-version-923-and-higher"></a>Mobility hizmeti Aracısı sürüm 9,23 ve üzeri

Sürüm 9,23 hakkında daha fazla bilgi için bkz. [güncelleştirme paketi 35 Azure Site Recovery](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery).

Mobility hizmetinin göndererek yüklenmesi sırasında aşağıdaki adımlar gerçekleştirilir:

1. Aracı kaynak makineye gönderilir. Aracı kaynak makineye kopyalamak birden çok ortam hatası nedeniyle başarısız olabilir. Anında yükleme hatalarıyla ilgili sorunları gidermek için [kılavuzumuzu](vmware-azure-troubleshoot-push-install.md) ziyaret edin.
1. Aracı sunucuya başarıyla kopyalandıktan sonra sunucuda bir önkoşul denetimi yapılır.
   - Tüm önkoşullar karşılanıyorsa, yükleme başlar.
   - Bir veya daha fazla [Önkoşul](vmware-physical-azure-support-matrix.md) karşılanmazsa yükleme başarısız olur.
1. Aracı yüklemesinin bir parçası olarak, Azure Site Recovery için Birim Gölge Kopyası Hizmeti (VSS) sağlayıcısı yüklenir. VSS sağlayıcısı, uygulamayla tutarlı kurtarma noktaları oluşturmak için kullanılır. VSS sağlayıcısı yüklemesi başarısız olursa, bu adım atlanır ve aracı yüklemesi devam eder.
1. Aracı yüklemesi başarılı olur ancak VSS sağlayıcısı yüklemesi başarısız olursa, iş durumu **Uyarı**olarak işaretlenir. Bu, kilitlenme ile tutarlı kurtarma noktası üretimini etkilemez.

    - Uygulamayla tutarlı kurtarma noktaları oluşturmak için, Site Recovery VSS sağlayıcısı 'nı el ile yüklemeyi tamamlamaya yönelik [kılavuzumuza](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) bakın.
    - Uygulamayla tutarlı kurtarma noktaları oluşturmak istemiyorsanız, uygulamayla tutarlı kurtarma noktalarını kapatmak için [çoğaltma ilkesini değiştirin](vmware-azure-set-up-replication.md#create-a-policy) .

### <a name="mobility-service-agent-version-922-and-below"></a>Mobility hizmeti Aracısı sürüm 9,22 ve altı

1. Aracı kaynak makineye gönderilir. Aracı kaynak makineye kopyalamak birden çok ortam hatası nedeniyle başarısız olabilir. Anında yükleme hatalarıyla ilgili sorunları gidermek için [kılavuzumuza](vmware-azure-troubleshoot-push-install.md) bakın.
1. Aracı sunucuya başarıyla kopyalandıktan sonra sunucuda bir önkoşul denetimi yapılır.
   - Tüm önkoşullar karşılanıyorsa, yükleme başlar.
   - Bir veya daha fazla [Önkoşul](vmware-physical-azure-support-matrix.md) karşılanmazsa yükleme başarısız olur.

1. Aracı yüklemesinin bir parçası olarak, Azure Site Recovery için Birim Gölge Kopyası Hizmeti (VSS) sağlayıcısı yüklenir. VSS sağlayıcısı, uygulamayla tutarlı kurtarma noktaları oluşturmak için kullanılır.
   - VSS sağlayıcısı yüklemesi başarısız olursa, aracı yüklemesi başarısız olur. Aracı yüklemesinde hata oluşmaması için, kilitlenme tutarlı kurtarma noktaları oluşturmak ve VSS sağlayıcısı 'nı el ile yüklemek için [sürüm 9,23](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery) veya üstünü kullanın.

## <a name="install-the-mobility-service-using-ui"></a>Kullanıcı arabirimini kullanarak Mobility hizmetini yükler

### <a name="prerequisites"></a>Ön koşullar

- Tüm sunucu yapılandırmalarının, [VMware VM 'leri ve fiziksel sunucuları Azure 'a olağanüstü durum kurtarma Için destek matrisindeki](vmware-physical-azure-support-matrix.md)ölçütlere uygun olduğundan emin olun.
- Sunucunun işletim sistemi için [yükleyiciyi bulun](#locate-installer-files) .

>[!IMPORTANT]
> Bir Azure bölgesinden diğerine bir Azure altyapı hizmeti (IaaS) VM 'si çoğaltdıysanız, UI yükleme yöntemini kullanmayın. [Komut istemi](#install-the-mobility-service-using-command-prompt) yüklemesini kullanın.

1. Yükleme dosyasını makineye kopyalayın ve çalıştırın.
1. **Yükleme seçeneği**içinde **Mobility hizmetini yükleme**' yi seçin.
1. Yükleme konumunu seçin ve yükleme ' **yi**seçin.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility1.png" alt-text="Mobility hizmeti yükleme seçeneği sayfası.":::

1. Yükleme **işlemindeki**yüklemeyi izleyin. Yükleme tamamlandıktan sonra, hizmeti yapılandırma sunucusuna kaydetmek için **yapılandırmaya devam et** ' i seçin.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility3.png" alt-text="Mobility hizmeti kayıt sayfası.":::

1. **Yapılandırma sunucusu ayrıntıları**' nda, yapılandırdığınız IP adresini ve parolayı belirtin.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility4.png" alt-text="Mobility hizmeti kayıt sayfası.":::

1. Kayıt işleminin sona ermesini sağlamak için **Kaydet** ' i seçin.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility5.png" alt-text="Mobility hizmeti kaydı son sayfası.":::

## <a name="install-the-mobility-service-using-command-prompt"></a>Komut istemi kullanarak Mobility hizmetini yükler

### <a name="prerequisites"></a>Ön koşullar

- Tüm sunucu yapılandırmalarının, [VMware VM 'leri ve fiziksel sunucuları Azure 'a olağanüstü durum kurtarma Için destek matrisindeki](vmware-physical-azure-support-matrix.md)ölçütlere uygun olduğundan emin olun.
- Sunucunun işletim sistemi için [yükleyiciyi bulun](#locate-installer-files) .

### <a name="windows-machine"></a>Windows makinesi

- Bir komut isteminden, korumak istediğiniz sunucuda yükleyiciyi _C:\Temp_gibi bir yerel klasöre kopyalamak için aşağıdaki komutları çalıştırın. Yükleyicinin dosya adını gerçek dosya adıyla değiştirin.

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

- Aracıyı yapılandırma sunucusuna kaydetmek için bu komutları çalıştırın.

  ```cmd
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="installation-settings"></a>Yükleme ayarları

Ayar | Ayrıntılar
--- | ---
Sözdizimi | `UnifiedAgent.exe /Role \<MS/MT> /InstallLocation \<Install Location> /Platform "VmWare" /Silent`
Kurulum günlükleri | `%ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log`
`/Role` | Zorunlu yükleme parametresi. Mobility hizmeti (MS) veya ana hedefin (MT) yüklenip yüklenmeyeceğini belirtir.
`/InstallLocation`| İsteğe bağlı parametre. Mobility hizmeti yükleme konumunu (herhangi bir klasör) belirtir.
`/Platform` | Zorunlu. Mobility hizmetinin yüklendiği platformu belirtir: <br/> VMware VM 'Leri/fiziksel sunucuları için **VMware** . <br/> Azure VM 'Leri için **Azure** .<br/><br/> Azure VM 'lerini fiziksel makine olarak değerlendiriyorsanız, **VMware**' yi belirtin.
`/Silent`| İsteğe bağlı. Yükleyicinin sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları

Ayar | Ayrıntılar
--- | ---
Sözdizimi | `UnifiedAgentConfigurator.exe  /CSEndPoint \<CSIP> /PassphraseFilePath \<PassphraseFilePath>`
Aracı yapılandırma günlükleri | `%ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log`
`/CSEndPoint` | Zorunlu parametre. `<CSIP>`yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
`/PassphraseFilePath` |  Zorunlu. Parolanın konumu. Geçerli bir UNC veya yerel dosya yolu kullanın.

### <a name="linux-machine"></a>Linux makinesi

1. Terminal oturumundan, yükleyiciyi korumak istediğiniz sunucuda _/tmp_ gibi bir yerel klasöre kopyalayın. Yükleyicinin dosya adını Linux dağılımının gerçek dosya adıyla değiştirin, ardından komutları çalıştırın.

   ```shell
   cd /tmp ;
   tar -xvf Microsoft-ASR_UA_version_LinuxVersion_GA_date_release.tar.gz
   ```

2. Aşağıdaki gibi yüklenir:

   ```shell
   sudo ./install -d <Install Location> -r MS -v VmWare -q
   ```

3. Yükleme tamamlandıktan sonra Mobility hizmetinin yapılandırma sunucusuna kayıtlı olması gerekir. Mobility hizmetini yapılandırma sunucusuna kaydetmek için aşağıdaki komutu çalıştırın.

   ```shell
   /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
   ```

#### <a name="installation-settings"></a>Yükleme ayarları

Ayar | Ayrıntılar
--- | ---
Sözdizimi | `./install -d \<Install Location> -r \<MS/MT> -v VmWare -q`
`-r` | Zorunlu yükleme parametresi. Mobility hizmeti (MS) veya ana hedefin (MT) yüklenip yüklenmeyeceğini belirtir.
`-d` | İsteğe bağlı parametre. Mobility hizmeti yükleme konumunu belirtir: `/usr/local/ASR`.
`-v` | Zorunlu. Mobility hizmetinin yüklü olduğu platformu belirtir. <br/> VMware VM 'Leri/fiziksel sunucuları için **VMware** . <br/> Azure VM 'Leri için **Azure** .
`-q` | İsteğe bağlı. Yükleyicinin sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları

Ayar | Ayrıntılar
--- | ---
Sözdizimi | `cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i \<CSIP> -P \<PassphraseFilePath>`
`-i` | Zorunlu parametre. `<CSIP>`yapılandırma sunucusunun IP adresini belirtir. Herhangi bir geçerli IP adresi kullanın.
`-P` |  Zorunlu. Parolasının kaydedildiği dosyanın tam dosya yolu. Geçerli bir klasör kullanın.

## <a name="azure-virtual-machine-agent"></a>Azure sanal makine Aracısı

- **Windows VM 'leri**: Mobility hizmetinin 9.7.0.0 sürümünden [Azure VM Aracısı](/azure/virtual-machines/extensions/features-windows#azure-vm-agent) , Mobility hizmeti yükleyicisi tarafından yüklenir. Bu, makinenin Azure 'a devredilmesini sağlar, Azure VM, herhangi bir VM uzantısını kullanmak için aracı yükleme önkoşullarını karşılar.
- **Linux VM 'leri**: [Walınuxagent](/azure/virtual-machines/extensions/update-linux-agent) yük DEVRETMEDEN sonra Azure VM 'ye el ile yüklenmelidir.

## <a name="locate-installer-files"></a>Yükleyici dosyalarını bul

Yapılandırma sunucusunda _%ProgramData%\asr\home\svsystems\pushınstallsvc\repository_klasörüne gidin. İşletim sistemine bağlı olarak hangi yükleyicide ihtiyacınız olduğunu denetleyin. Aşağıdaki tabloda, her VMware VM ve fiziksel sunucu işletim sistemi için yükleyici dosyaları özetlenmektedir. Başlamadan önce [desteklenen işletim sistemlerini](vmware-physical-azure-support-matrix.md#replicated-machines)gözden geçirebilirsiniz.

> [!NOTE]
> Dosya adları, aşağıdaki tabloda gösterilen sözdizimini gerçek değerler için yer tutucu olarak _Sürüm_ ve _Tarih_ ile birlikte kullanır. Gerçek dosya adları şu örneklere benzer şekilde görünür:
> - `Microsoft-ASR_UA_9.30.0.0_Windows_GA_22Oct2019_release.exe`
> - `Microsoft-ASR_UA_9.30.0.0_UBUNTU-16.04-64_GA_22Oct2019_release.tar.gz`

Yükleyici dosyası | İşletim sistemi (yalnızca 64 bit)
--- | ---
`Microsoft-ASR_UA_version_Windows_GA_date_release.exe` | Windows Server 2016 </br> Windows Server 2012 R2 </br> Windows Server 2012 </br> Windows Server 2008 R2 SP1
`Microsoft-ASR_UA_version_RHEL6-64_GA_date_release.tar.gz` | Red Hat Enterprise Linux (RHEL) 6 </br> CentOS 6
`Microsoft-ASR_UA_version_RHEL7-64_GA_date_release.tar.gz` | Red Hat Enterprise Linux (RHEL) 7 </br> CentOS 7
`Microsoft-ASR_UA_version_SLES12-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 12 SP1 </br> SP2 ve SP3 'Ü içerir.
`Microsoft-ASR_UA_version_SLES11-SP3-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 11 SP3
`Microsoft-ASR_UA_version_SLES11-SP4-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 11 SP4
`Microsoft-ASR_UA_version_OL6-64_GA_date_release.tar.gz` | Oracle Enterprise Linux 6,4 </br> Oracle Enterprise Linux 6,5
`Microsoft-ASR_UA_version_UBUNTU-14.04-64_GA_date_release.tar.gz` | Ubuntu Linux 14,04
`Microsoft-ASR_UA_version_UBUNTU-16.04-64_GA_date_release.tar.gz` | Ubuntu Linux 16,04 LTS sunucusu
`Microsoft-ASR_UA_version_DEBIAN7-64_GA_date_release.tar.gz` | Deyi 7
`Microsoft-ASR_UA_version_DEBIAN8-64_GA_date_release.tar.gz` | Debian 8

## <a name="next-steps"></a>Sonraki adımlar

[Mobility hizmeti için anında yükleme ayarlayın](vmware-azure-install-mobility-service.md).
