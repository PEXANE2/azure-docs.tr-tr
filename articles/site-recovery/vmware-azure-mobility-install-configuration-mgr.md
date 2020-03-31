---
title: Azure Site Kurtarma'daki yüklemenin olağanüstü kurtarma sıyrık kurtarma için Mobilite Hizmetini otomatikleştirin
description: Azure Site Kurtarma ile VMware /fiziksel sunucu olağanüstü durum kurtarma için Mobilite Hizmeti otomatik olarak nasıl yüklenir?
author: Rajeswari-Mamilla
ms.topic: how-to
ms.date: 2/5/2020
ms.author: ramamill
ms.openlocfilehash: f24d321e882024d324435498adf11694037547f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77252236"
---
# <a name="automate-mobility-service-installation"></a>Mobilite Hizmeti kurulumunu otomatikleştirin

Bu makalede, [Azure Site Kurtarma'daki](site-recovery-overview.md)Mobilite Hizmeti aracısının yükleme ve güncelleştirmeleri nasıl otomatikleştirilen açıklanmaktadır.

Şirket içi VMware VM'lerinin ve fiziksel sunucuların olağanüstü kurtarma durumu için Site Kurtarma'yı Azure'a dağıtırken, çoğaltmak istediğiniz her makineye Mobilite Hizmeti aracısını yüklersiniz. Mobilite Hizmeti, makinedeki verileri yakalar ve çoğaltma için Site Kurtarma işlem sunucusuna iletir. Mobilite Hizmetini birkaç şekilde dağıtabilirsiniz:

- **Push yükleme**: Azure portalındaki bir makineiçin çoğaltmayı etkinleştirdiğinizde Site Kurtarma'nın Mobilite servis aracısını yüklemesine izin verin.
- **Manuel kurulum**: Mobilite hizmetini her makineye manuel olarak yükleyin. İtme ve manuel yükleme hakkında [daha fazla bilgi edinin.](vmware-physical-mobility-service-overview.md)
- **Otomatik dağıtım**: Yüklemeyi Microsoft Endpoint Configuration Manager gibi yazılım dağıtım araçları yla veya JetPatch gibi üçüncü taraf araçlarıyla otomatikleştirin.

Otomatik kurulum ve güncelleştirme, şu şekilde bir çözüm sağlar:

- Kuruluşunuz korumalı sunucularda itme yüklemesi için izin vermez.
- Şirket politikanız, parolaların düzenli aralıklarla değiştirilmesini gerektirir. Push yüklemesi için bir parola belirtmeniz gerekir.
- Güvenlik politikanız belirli makineler için güvenlik duvarı özel durumları eklemeye izin vermez.
- Barındırma hizmeti sağlayıcısı olarak hareket ediyorsanız ve Site Kurtarma ile anında yükleme için gereken müşteri makinesi kimlik bilgilerini sağlamak istemiyorsunuz.
- Aracı yüklemelerini aynı anda çok sayıda sunucuya ölçeklendirmeniz gerekir.
- Planlanan bakım pencereleri sırasında yüklemeleri ve yükseltmeleri zamanlamak istiyorsunuz.

## <a name="prerequisites"></a>Ön koşullar

Yüklemeyi otomatikleştirmek için aşağıdaki öğelere ihtiyacınız vardır:

- [Configuration Manager](/configmgr/) veya [JetPatch](https://jetpatch.com/microsoft-azure-site-recovery/)gibi dağıtılmış bir yazılım yükleme çözümü.
- VMware olağanüstü durum kurtarma veya [fiziksel sunucu](physical-azure-disaster-recovery.md) olağanüstü durum kurtarma için [Azure](tutorial-prepare-azure.md) ve [şirket içi](vmware-azure-tutorial-prepare-on-premises.md) dağıtım ön koşulları. Olağanüstü durum kurtarma için [destek gereksinimlerini](vmware-physical-azure-support-matrix.md) gözden geçirin.

## <a name="prepare-for-automated-deployment"></a>Otomatik dağıtıma hazırlanın

Aşağıdaki tabloda, Mobilite Hizmeti dağıtımını otomatikleştirmek için araçlar ve süreçler özetlenmiştir.

**Araç** | **Şey** | **Yönergeler**
--- | --- | ---
**Configuration Manager** | 1. Yukarıda listelenen [ön koşullara](#prerequisites) sahip olduğunuzu doğrulayın. <br/><br/> 2. Bir OVF şablonu kullanarak Site Kurtarma yapılandırma sunucusunu VMware VM olarak dağıtmak için bir OVA dosyası indirme dahil olmak üzere kaynak ortamı nı ayarlayarak olağanüstü durum kurtarma dağıtmayı.<br/><br/> 3. Yapılandırma sunucusunu Site Kurtarma hizmetine kaydettirin, hedef Azure ortamını ayarlar ve bir çoğaltma ilkesi yapılandırın.<br/><br/> 4. Otomatik Mobilite Hizmeti dağıtımı için, yapılandırma sunucusu geçiş cümlesi ve Mobilite Hizmeti yükleme dosyalarını içeren bir ağ paylaşımı oluşturursunuz.<br/><br/> 5. Yükleme veya güncelleştirmeleri içeren bir Configuration Manager paketi oluşturur ve Mobilite Hizmeti dağıtımına hazırlanırsınız.<br/><br/> 6. Daha sonra Mobilite Hizmeti yüklü olan makineler için Azure'a çoğaltmayı etkinleştirebilirsiniz. | [Configuration Manager ile otomatikleştirin](#automate-with-configuration-manager)
**JetPatch** | 1. Yukarıda listelenen [ön koşullara](#prerequisites) sahip olduğunuzu doğrulayın. <br/><br/> 2. Bir OVF şablonu kullanarak, Azure Site Kurtarma ortamınızda Azure Site Kurtarma için JetPatch Agent Manager'ı indirme ve dağıtma dahil olmak üzere kaynak ortamı kurarak olağanüstü durum kurtarmayı dağıtın.<br/><br/> 3. Yapılandırma sunucusunu Site Kurtarma'ya kaydettirin, hedef Azure ortamını ayarlar ve bir çoğaltma ilkesi yapılandırın.<br/><br/> 4. Otomatik dağıtım için JetPatch Agent Manager yapılandırmasını başlatma ve tamamlama.<br/><br/> 5. JetPatch'te, Mobilite Hizmeti aracısının dağıtımını ve yükseltmesini otomatikleştirmek için bir Site Kurtarma ilkesi oluşturabilirsiniz. <br/><br/> 6. Daha sonra Mobilite Hizmeti yüklü olan makineler için Azure'a çoğaltmayı etkinleştirebilirsiniz. | [JetPatch Agent Manager ile otomatikleştirin](https://jetpatch.com/microsoft-azure-site-recovery-deployment-guide/)<br/><br/> [JetPatch'te sorun giderme aracısı kurulumu](https://kc.jetpatch.com/hc/articles/360035981812)

## <a name="automate-with-configuration-manager"></a>Configuration Manager ile otomatikleştirin

### <a name="prepare-the-installation-files"></a>Yükleme dosyalarını hazırlama

1. Ön koşulları niçin yerine aldığınızdan emin olun.
1. Yapılandırma sunucusunu çalıştıran makine tarafından erişilebilen güvenli bir ağ dosya paylaşımı (SMB paylaşımı) oluşturun.
1. Configuration Manager'da, Mobilite Hizmetini yüklemek veya güncelleştirmek istediğiniz [sunucuları kategorilere](/sccm/core/clients/manage/collections/automatically-categorize-devices-into-collections) ayırın. Bir koleksiyon tüm Windows sunucularını, diğertüm Linux sunucularını içermelidir.
1. Ağ paylaşımında bir klasör oluşturun:

   - Windows makinelerine yükleme için _MobSvcWindows_adında bir klasör oluşturun.
   - Linux makinelerine kurulum için _MobSvcLinux_adında bir klasör oluşturun.

1. Yapılandırma sunucusu makinesinde oturum açın.
1. Yapılandırma sunucusu makinesinde, bir yönetim komut istemi açın.
1. Parola dosyasını oluşturmak için şu komutu çalıştırın:

    ```Console
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```

1. _MobSvc.passphrase_ dosyasını Windows klasörüne ve Linux klasörüne kopyalayın.
1. Yükleme dosyalarını içeren klasöre göz atmak için şu komutu çalıştırın:

    ```Console
    cd %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository
    ```

1. Bu yükleme dosyalarını ağ paylaşımına kopyalayın:

   - Windows için, _MobSvcWindows_ _Microsoft-ASR_UA_version_Windows_GA_date_Release.exe_ kopyalayın.
   - Linux için, _MobSvcLinux_aşağıdaki dosyaları kopyalamak:
     - _Microsoft-ASR_UARHEL6-64release.tar.gz_
     - _Microsoft-ASR_UARHEL7-64release.tar.gz_
     - _Microsoft-ASR_UASLES11-SP3-64release.tar.gz_
     - _Microsoft-ASR_UASLES11-SP4-64release.tar.gz_
     - _Microsoft-ASR_UAOL6-64release.tar.gz_
     - _Microsoft-ASR_UAUBUNTU-14.04-64release.tar.gz_

1. Aşağıdaki yordamlarda açıklandığı gibi, kodu Windows veya Linux klasörlerine kopyalayın. Biz varsayarak:

   - Yapılandırma sunucusunun IP `192.168.3.121`adresi.
   - Güvenli ağ dosya `\\ContosoSecureFS\MobilityServiceInstallers`paylaşımı .

### <a name="copy-code-to-the-windows-folder"></a>Kodu Windows klasörüne kopyalama

Aşağıdaki kodu kopyalayın:

- _Kodu MobSvcWindows_ klasörüne _install.bat_olarak kaydedin.
- Bu `[CSIP]` komut dosyasındaki yer tutucuları yapılandırma sunucunuzun IP adresinin gerçek değerleriyle değiştirin.
- Komut dosyası, Mobilite Hizmeti aracısının yeni yüklemelerini ve zaten yüklü olan aracılara güncelleştirmeleri destekler.

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up the folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract the installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction to complete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log
```

### <a name="copy-code-to-the-linux-folder"></a>Kodu Linux klasörüne kopyalama

Aşağıdaki kodu kopyalayın:

- _Kodu MobSvcLinux_ klasörüne _install_linux.sh_olarak kaydedin.
- Bu `[CSIP]` komut dosyasındaki yer tutucuları yapılandırma sunucunuzun IP adresinin gerçek değerleriyle değiştirin.
- Komut dosyası, Mobilite Hizmeti aracısının yeni yüklemelerini ve zaten yüklü olan aracılara güncelleştirmeleri destekler.

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed to configuration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed to Upgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed to Configure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="create-a-package"></a>Paket oluşturma

1. Configuration Manager konsolunda oturum açın ve **Yazılım Kitaplığı** > **Uygulama Yönetim** > **Paketleri'ne**gidin.
1. Sağ tıkla **Paketleri** > **Oluştur Paketi**.
1. Ad, açıklama, üretici, dil ve sürüm gibi paket ayrıntılarını sağlayın.
1. Select **Bu paket kaynak dosyaları içerir.**
1. **Gözat'ı**tıklatın ve ilgili yükleyiciyi _(MobSvcWindows_ veya _MobSvcLinux)_ içeren ağ paylaşımını ve klasörünü seçin. Ardından **İleri'yi**seçin.

   ![Paket Oluştur ve Program sihirbazının ekran görüntüsü](./media/vmware-azure-mobility-install-configuration-mgr/create_sccm_package.png)

1. Sayfa **oluşturmak istediğiniz program türünü seçin'** de **İleri'deki Standart Program'ı** > **Next**seçin.

   ![Paket Oluştur ve Program sihirbazının ekran görüntüsü](./media/vmware-azure-mobility-install-configuration-mgr/sccm-standard-program.png)

1. Bu standart program sayfası **yla ilgili bilgileri belirtin'de** aşağıdaki değerleri belirtin:

    **Parametre** | **Windows değeri** | **Linux değeri**
    --- | --- | ---
    **Adı** | Microsoft Azure Mobilite Hizmeti (Windows) yükleme | Microsoft Azure Mobilite Hizmeti'ni (Linux) yükleyin.
    **Komut satırı** | Install.bat | ./install_linux.ş
    **Program çalıştırabilirsiniz** | Bir kullanıcının oturum açıp açmaması | Bir kullanıcının oturum açıp açmaması
    **Diğer parametreler** | Varsayılan ayarını kullanma | Varsayılan ayarını kullanma

   ![Paket Oluştur ve Program sihirbazının ekran görüntüsü](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties.png)

1. **Bu standart programın gereksinimlerini belirtin,** aşağıdaki görevleri yapın:

   - Windows makineleri için, **Bu programın yalnızca belirtilen platformlarda çalıştırılabilen'i**seçin. Ardından, [desteklenen Windows işletim sistemlerini](vmware-physical-azure-support-matrix.md#replicated-machines) seçin ve **İleri'yi**seçin.
   - Linux makineleri için, **bu program herhangi bir platformda çalıştırabilirsiniz**seçin. Ardından **İleri'yi**seçin.

1. Sihirbazı tamamlayın.

### <a name="deploy-the-package"></a>Paketi dağıtma

1. Configuration Manager konsolunda pakete sağ tıklayın ve **İçeriği Dağıt'ı**seçin.

   ![Configuration Manager konsolunun ekran görüntüsü](./media/vmware-azure-mobility-install-configuration-mgr/sccm_distribute.png)

1. Paketlerin kopyalandığı dağıtım noktalarını seçin. [Daha fazla bilgi edinin](/sccm/core/servers/deploy/configure/install-and-configure-distribution-points).
1. Sihirbazı tamamlayın. Paket daha sonra belirtilen dağıtım noktalarına çoğaltmabaşlar.
1. Paket dağıtımı bittikten **sonra, Dağıt'>** paketi sağ tıklatın.

   ![Configuration Manager konsolunun ekran görüntüsü](./media/vmware-azure-mobility-install-configuration-mgr/sccm_deploy.png)

1. Daha önce oluşturduğunuz Windows veya Linux aygıt koleksiyonunu seçin.
1. İçerik **hedef** sayfasını belirtin' **de Dağıtım Noktaları'nı**seçin.
1. **Bu yazılımın nasıl dağıtılan sayfayı denetlemek için ayarlarını belirtin,** **Amaç'ı** **Gerekli**Olarak Ayarlayın.

   ![Yazılım Dağıt sihirbazının ekran görüntüsü](./media/vmware-azure-mobility-install-configuration-mgr/sccm-deploy-select-purpose.png)

1. **Bu dağıtım için zamanlama belirtin,** bir zamanlama ayarlayın. [Daha fazla bilgi edinin](/sccm/apps/deploy-use/deploy-applications#bkmk_deploy-sched).

   - Mobilite Hizmeti, belirttiğiniz zamanlamaya uygun olarak yüklenir.
   - Gereksiz yeniden başlatmaları önlemek için, aylık bakım pencereniz veya yazılım güncelleştirmeleri pencereniz sırasında paket yüklemesini zamanlayın.

1. Dağıtım **Noktaları** sayfasında, ayarları yapılandırın ve sihirbazı bitirin.
1. Configuration Manager konsolunda dağıtım ilerlemesini izleyin. Paket adınızı **İzleme** > **Dağıtımları'na** > _\<\>_ gidin.

### <a name="uninstall-the-mobility-service"></a>Mobilite Hizmetini Kaldırın

Mobilite Hizmetini kaldırmak için Configuration Manager paketleri oluşturabilirsiniz. Örneğin, aşağıdaki komut dosyası Mobilite Hizmetini yükler:

```DOS
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT
```

## <a name="next-steps"></a>Sonraki adımlar

VM'ler için [çoğaltmayı etkinleştirin.](vmware-azure-enable-replication.md)
