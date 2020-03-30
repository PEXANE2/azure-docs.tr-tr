---
title: Azure Site Kurtarma ile VMware VM'lerin ve fiziksel sunucuların olağanüstü kurtarma için Mobilite Hizmeti hakkında | Microsoft Dokümanlar
description: Azure Site Kurtarma hizmetini kullanarak VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü kurtarma hizmeti için Mobilite Hizmeti aracısı hakkında bilgi edinin.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: c5acc9637fe5afe8f7dd32d23fbdbb80373b4f61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256841"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>VMware VM'ler ve fiziksel sunucular için Mobilite hizmeti hakkında

[Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak VMware VM'leri ve fiziksel sunucular için olağanüstü durum kurtarma ayarlarken, Site Kurtarma Mobilite hizmetini şirket içi VMware VM ve fiziksel sunucuya yüklersiniz.  Mobilite hizmeti, makinedeki verileri yakalar ve bunları Site Kurtarma işlem sunucusuna iletir. Mobilite Hizmetini aşağıdaki yöntemleri kullanarak dağıtabilirsiniz:

- [Push installation](#push-installation): Azure portalı üzerinden koruma etkinleştirildiğinde Site Kurtarma sunucuya mobilite aracısı yükler.
- El ile yükleyin: Mobilite hizmetini her makineye [UI](#install-mobility-agent-through-ui) veya [komut istemi](#install-mobility-agent-through-command-prompt)aracılığıyla el ile yükleyebilirsiniz.
- [Otomatik dağıtım](vmware-azure-mobility-install-configuration-mgr.md): Yüklemeyi Configuration Manager gibi yazılım dağıtım araçlarıyla otomatikleştirebilirsiniz.

> [!NOTE]
> Mobilite aracısı VMware VM'ler veya fiziksel makineler için kaynak makinelerde belleğin yaklaşık %6-10'unu kullanır.

## <a name="anti-virus-on-replicated-machines"></a>Çoğaltılan makinelerde anti-virüs

Çoğaltmak istediğiniz makinelerde etkin anti-virüs yazılımı çalışıyorsa, Mobilite hizmeti yükleme klasörünü virüsten koruma işlemlerinden *(C:\ProgramData\ASR\agent)* hariç tutturduğınızdan emin olun. Bu, çoğaltmanın beklendiği gibi çalışmasını sağlar.

## <a name="push-installation"></a>İtme yüklemesi

Push yükleme portalda tetiklenen "[Çoğaltma yı etkinleştir](vmware-azure-enable-replication.md#enable-replication)" işinin ayrılmaz bir parçasıdır. "Çoğaltmayı Etkinleştir"i korumak ve tetiklemek istediğiniz sanal makineler kümesini seçtikten sonra, yapılandırma sunucusu mobilite aracısını sunuculara iter, aracıyı yükler ve aracının yapılandırma sunucusuna tam kaydını yükler. Bu operasyonun başarıyla tamamlanması için,

- Tüm itme yükleme [ön koşullarının](vmware-azure-install-mobility-service.md) karşılandığından emin olun.
- Sunucuların tüm yapılandırmalarının [VMware'den Azure DR senaryosuna destek matrisi](vmware-physical-azure-support-matrix.md)altında olduğundan emin olun.

Push installation iş akışının ayrıntıları aşağıdaki bölümlerde açıklanmıştır.

### <a name="from-923-version-onwards"></a>[9.23 sürümünden](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery) itibaren

Mobilite aracısının itme kurulumu sırasında aşağıdaki adımlar gerçekleştirilir

1. Aracıyı kaynak makinesine iter. Aracının kaynak makineye kopyalanması, birden çok çevresel hata nedeniyle başarısız olabilir. Anında geçirme yükleme hatalarını gidermek için kılavuzumuzu ziyaret [edin.](vmware-azure-troubleshoot-push-install.md)
2. Aracı başarıyla sunucuya kopyalandıktan sonra sunucuda ön koşul denetimleri gerçekleştirilir. [Ön koşullardan](vmware-physical-azure-support-matrix.md) biri veya birkaçı karşılanmazsa yükleme başarısız olur. Tüm ön koşullar karşılanırsa, yükleme tetiklenir.
3. Azure Site Kurtarma VSS sağlayıcısı, Mobilite aracı yüklemesinin bir parçası olarak sunucuya yüklenir. Bu sağlayıcı, Uygulama tutarlı noktaları oluşturmak için kullanılır. VSS sağlayıcısının yüklenmesi başarısız olursa, bu adım atlanır ve aracı yüklemesi devam eder.
4. Aracı yükleme başarılı olursa ancak VSS sağlayıcı yükleme başarısız olursa, iş durumu "Uyarı" olarak işaretlenir. Bu, kilitlenme tutarlılık noktaları oluşturma etkilemez.

    a. Uygulama tutarlı noktaları oluşturmak için, Site Kurtarma VSS sağlayıcısının el ile yüklenmesini tamamlamak için [kılavuzumuza](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) bakın. </br>
    b.  Uygulama tutarlı noktalarının oluşturulmasını istemiyorsanız, uygulama tutarlı noktalarını kapatmak için [çoğaltma ilkesini değiştirin.](vmware-azure-set-up-replication.md#create-a-policy)

### <a name="before-922-versions"></a>9.22 sürümleri önce

1. Aracıyı kaynak makinesine iter. Aracının kaynak makineye kopyalanması, birden çok çevresel hata nedeniyle başarısız olabilir. Anında geçirme yükleme hatalarını gidermek için kılavuzumuzu ziyaret [edin.](vmware-azure-troubleshoot-push-install.md)
2. Aracı başarıyla sunucuya kopyalandıktan sonra sunucuda ön koşul denetimleri gerçekleştirilir. [Ön koşullardan](vmware-physical-azure-support-matrix.md) biri veya birkaçı karşılanmazsa yükleme başarısız olur. Tüm ön koşullar karşılanırsa, yükleme tetiklenir.
3. Azure Site Kurtarma VSS sağlayıcısı, Mobilite aracı yüklemesinin bir parçası olarak sunucuya yüklenir. Bu sağlayıcı, Uygulama tutarlı noktaları oluşturmak için kullanılır. VSS sağlayıcısının yüklenmesi başarısız olursa, aracı yükleme başarısız olur. Mobilite aracısı yüklemesinin başarısız olmasını önlemek için, kilitlenme tutarlı noktaları oluşturmak ve VSS sağlayıcısını el ile yüklemek için [9.23 veya](https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery) daha yüksek sürümü kullanın.

## <a name="install-mobility-agent-through-ui"></a>UI aracılığıyla mobilite aracısı yükleme

### <a name="prerequisite"></a>Önkoşul

- Sunucuların tüm yapılandırmalarının [VMware'den Azure DR senaryosuna destek matrisi](vmware-physical-azure-support-matrix.md)altında olduğundan emin olun.
- Sunucunun işletim sistemine göre [yükleyiciyi bulun.](#locate-installer-files)

>[!IMPORTANT]
> Azure IaaS VM'yi bir Azure bölgesinden diğerine çoğaltırsanız, bu yöntemi kullanmayın. Bunun yerine komut satırı tabanlı yükleme yöntemini kullanın.

1. Yükleme dosyasını makineye kopyalayın ve çalıştırın.
2. **Yükleme**Seçeneği'nde, **Mobilite Hizmetini Yükle'yi**seçin.
3. Yükleme konumunu seçin **>.**

    ![Mobilite Hizmeti yükleme seçeneği sayfası](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

4. **Yükleme İlerlemesinde**yüklemeyi izleyin. Yükleme tamamlandıktan sonra, hizmeti yapılandırma sunucusuna kaydetmek için **Yapılandırmaya Devam** Et'i seçin.

    ![Mobilite Hizmeti kayıt sayfası](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

5. **Configuration Server Ayrıntıları'nda,** yapılandırdığınız IP adresini ve parolayı belirtin.

    ![Mobilite Hizmeti kayıt sayfası](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

6. Kaydı bitirmek için **Kaydol'u** seçin.

    ![Mobilite Hizmeti kayıt son sayfası](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-mobility-agent-through-command-prompt"></a>Komut istemi ile mobilite aracısını yükleme

### <a name="prerequisite"></a>Önkoşul

- Sunucuların tüm yapılandırmalarının [VMware'den Azure DR senaryosuna destek matrisi](vmware-physical-azure-support-matrix.md)altında olduğundan emin olun.
- Sunucunun işletim sistemine göre [yükleyiciyi bulun.](#locate-installer-files)

### <a name="on-a-windows-machine"></a>Windows makinesinde

- Yükleyiciyi korumak istediğiniz sunucudaki yerel bir klasöre (örneğin, C:\Temp) kopyalayın.

    ```
    cd C:\Temp
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```

- Aşağıdaki gibi yükleyin:

    ```
    UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    ```

- Aracıyı yapılandırma sunucusuna kaydedin.

    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
    ```

#### <a name="installation-settings"></a>Yükleme ayarları
**Ayar** | **Şey**
--- | ---
Kullanım | UnifiedAgent.exe /Role \<MS/MT> \</InstallLocation Yükleme Konumu> /Platform "VmWare" /Silent
Kurulum günlükleri | %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log altında.
/Rol | Zorunlu kurulum parametresi. Mobilite hizmetinin (MS) veya ana hedefin (MT) yüklenip yüklenmeyeceğini belirtir.
/InstallLocation| İsteğe bağlı parametre. Mobilite hizmeti yükleme konumunu (herhangi bir klasör) belirtir.
/Platform | Zorunlu. Mobilite Hizmetinin yüklendiği platformu belirtir. **VMware VMware/fiziksel** sunucular için; Azure VM'ler için **Azure.**<br/><br/> Azure VM'leri fiziksel makine olarak ele alıyorsanız, **VMware'i**belirtin.
/Silent| İsteğe bağlı. Yükleyicinin sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları
**Ayar** | **Şey**
--- | ---
Kullanım | UnifiedAgentConfigurator.exe /CSEndPoint \<CSIP> /PassphraseFilePath \<PassphraseFilePath>
Aracı yapılandırma günlükleri | %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log altında.
/CSEndPoint | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Geçerli bir IP adresi kullanın.
/PassphraseFilePath |  Zorunlu. Parolanın konumu. Geçerli UNC veya yerel dosya yolunu kullanın.

### <a name="on-a-linux-machine"></a>Linux makinesinde

1. Yükleyiciyi korumak istediğiniz sunucudaki yerel bir klasöre (örneğin, /tmp) kopyalayın. Bir terminalde, aşağıdaki komutları çalıştırın:

    ```
    cd /tmp ;
    tar -xvf Microsoft-ASR_UA*release.tar.gz
    ```

2. Aşağıdaki gibi yükleyin:

    ```
    sudo ./install -d <Install Location> -r MS -v VmWare -q
    ```

3. Yükleme tamamlandıktan sonra, Mobilite Hizmeti yapılandırma sunucusuna kaydedilmelidir. Mobilite Hizmetini yapılandırma sunucusuna kaydetmek için aşağıdaki komutu çalıştırın:

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
    ```

#### <a name="installation-settings"></a>Yükleme ayarları
**Ayar** | **Şey**
--- | ---
Kullanım | ./install -d \<Yükleme Yeri \<> -r MS/MT> -v VmWare -q
-r | Zorunlu kurulum parametresi. Mobilite hizmetinin (MS) veya ana hedefin (MT) yüklenip yüklenmeyeceğini belirtir.
-d | İsteğe bağlı parametre. Mobilite hizmeti yükleme konumunu belirtir: /usr/local/ASR.
-v | Zorunlu. Mobilite Hizmetinin yüklendiği platformu belirtir. **VMware VMware/fiziksel** sunucular için; Azure VM'ler için **Azure.**
-q | İsteğe bağlı. Yükleyicinin sessiz modda çalıştırılıp çalıştırılmayacağını belirtir.

#### <a name="registration-settings"></a>Kayıt ayarları
**Ayar** | **Şey**
--- | ---
Kullanım | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i \<CSIP \<> -P PassphraseFilePath>
-i | Zorunlu parametre. Yapılandırma sunucusunun IP adresini belirtir. Geçerli bir IP adresi kullanın.
-P |  Zorunlu. Parolanın kaydedildiği dosyanın tam dosya yolu. Geçerli bir klasör kullanın.

## <a name="azure-virtual-machine-agent"></a>Azure Sanal Makine aracısı

- **Windows VM'ler**: Mobilite hizmetinin 9.7.0.0 sürümünden, [Azure VM aracısı](../virtual-machines/extensions/features-windows.md#azure-vm-agent) Mobilite hizmet yükleyicisi tarafından yüklenir. Bu, makine Azure'a geçemediğinde, Azure VM'nin herhangi bir Vm uzantısını kullanmak için aracı yükleme ön koşuluyla karşılamasını sağlar.
- **Linux VM'leri**: [WALinuxAgent,](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) başarısız olduktan sonra Azure VM'ye el ile yüklenmelidir.

## <a name="locate-installer-files"></a>Yükleyici dosyalarını bulma

Yapılandırma sunucusunda %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository klasörüne gidin. İşletim sistemine göre hangi yükleyiciye ihtiyacınız olduğunu kontrol edin. Aşağıdaki tablo, her VMware VM ve fiziksel sunucu işletim sistemi için yükleyici dosyalarını özetler. Başlamadan önce [desteklenen işletim sistemlerini](vmware-physical-azure-support-matrix.md#replicated-machines) gözden geçirebilirsiniz.

**Yükleyici dosyası** | **İşletim sistemi (yalnızca 64 bit)**
--- | ---
Microsoft-ASR\_\*UA\*Windows release.exe | Windows Server 2016; Windows Server 2012 R2; Windows Server 2012; Windows Server 2008 R2 SP1
Microsoft-ASR\_UA\*RHEL6-64\*release.tar.gz | Kırmızı Şapka Kurumsal Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Kırmızı Şapka Kurumsal Linux (RHEL) 7.* </br> CentOS 7.*
Microsoft-ASR\_UA\*SLES12-64\*sürüm.tar.gz | SUSE Linux Kurumsal Sunucu 12 SP1,SP2,SP3
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Kurumsal Sunucu 11 SP3
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Kurumsal Sunucu 11 SP4
Microsoft-ASR\_UA\*OL6-64\*sürüm.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft-ASR\_UA UBUNTU-14.04-64\*\*release.tar.gz | Ubuntu Linux 14.04
Microsoft-ASR\_UA UBUNTU-16.04-64\*\*release.tar.gz | Ubuntu Linux 16.04 LTS sunucusu
Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7
Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8

## <a name="next-steps"></a>Sonraki adımlar

[Mobilite hizmeti için push yüklemesini ayarlayın.](vmware-azure-install-mobility-service.md)
