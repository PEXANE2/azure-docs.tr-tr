---
title: Karma makineleri Azure portal Azure 'a bağlama
description: Bu makalede, Azure portal için Azure yayı 'yi (Önizleme) kullanarak aracıyı yüklemeyi ve makineleri Azure 'a bağlamayı öğreneceksiniz.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.openlocfilehash: ac0a795c98673eba30531f586ff634c62673cdd6
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82980958"
---
# <a name="connect-hybrid-machines-to-azure-from-the-azure-portal"></a>Karma makineleri Azure portal Azure 'a bağlama

El ile bir adım kümesi gerçekleştirerek, ortamınızdaki bir veya az sayıda Windows veya Linux makinesi için sunucu (Önizleme) için Azure yayı 'yi etkinleştirebilirsiniz. Alternatif olarak, sağladığımız bir şablon betiği çalıştırarak otomatikleştirilmiş bir yöntemi kullanabilirsiniz. Bu betik, her iki aracısının de indirilmesini ve yüklenmesini otomatikleştirir.

Bu yöntem, aracıyı yüklemek ve yapılandırmak için makinede yönetici izinlerine sahip olmanızı gerektirir. Linux 'ta, kök hesabı kullanarak ve Windows 'ta, yerel Yöneticiler grubunun üyesi olursunuz.

Başlamadan önce, [önkoşulları](overview.md#prerequisites) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="generate-the-installation-script-from-the-azure-portal"></a>Azure portal yükleme betiğini oluşturma

İndirme ve yüklemeyi otomatik hale getirmeye ve Azure Arc ile bağlantı kurmaya yönelik betik, Azure portal kullanılabilir. İşlemi gerçekleştirmek için aşağıdakileri yapın:

1. Tarayıcınızdan [Azure Portal](https://aka.ms/hybridmachineportal)gidin.

1. **Makineler-Azure yay** sayfasında sol üst kısımdaki **Ekle**' yi veya orta bölmenin altındaki **makine-Azure yayı oluştur** seçeneğini belirleyin. 

1. **Yöntem seçin** sayfasında, **etkileşimli betiği kullanarak makine Ekle** kutucuğunu seçin ve ardından **betik oluştur**' u seçin.

1. **Betik oluştur** sayfasında, makinenin Azure 'da yönetilmesini istediğiniz aboneliği ve kaynak grubunu seçin. Makine meta verilerinin depolanacağı Azure konumunu seçin.

    >[!NOTE]
    >Sunucular için Azure Arc (Önizleme) yalnızca aşağıdaki bölgeleri destekler:
    >- WestUS2
    >- WestEurope
    >- Westasıya
    >
    >Genel Bakış [makalesinde bölge seçerken](overview.md#supported-regions) dikkat edilecek ek konuları gözden geçirin.

1. **Betik oluştur** sayfasında, **işletim sistemi** açılan listesinde, betiğin üzerinde çalıştığı işletim sistemini seçin.

1. Makine Internet 'e bağlanmak için bir proxy sunucusu üzerinden iletişim kurduğundan, Ileri ' yi seçin **: ara sunucu**. 
1. **Proxy sunucusu** sekmesinde, proxy sunucusu IP adresini veya makinenin proxy sunucusuyla iletişim kurmak için kullanacağı adı ve bağlantı noktası numarasını belirtin. Değeri biçiminde `http://<proxyURL>:<proxyport>`girin. 
1. **Gözden geçir + oluştur**' u seçin.

1. **Gözden geçir + oluştur** sekmesinde Özet bilgilerini gözden geçirin ve ardından **İndir**' i seçin. Hala değişiklik yapmanız gerekiyorsa, **önceki**' yi seçin.

## <a name="install-and-validate-the-agent-on-windows"></a>Windows 'da aracıyı yükleyip doğrulama

### <a name="install-manually"></a>El ile yükleme

Bağlı makine aracısını *AzureConnectedMachineAgent. msi*Windows Installer paketini çalıştırarak el ile yükleyebilirsiniz. 

> [!NOTE]
> * Aracıyı yüklemek veya kaldırmak için *yönetici* izinlerinizin olması gerekir.
> * Önce yükleyici paketini indirmeniz ve hedef sunucudaki bir klasöre veya paylaşılan bir ağ klasöründen kopyalamanız gerekir. Yükleyici paketini herhangi bir seçenek olmadan çalıştırırsanız, aracı etkileşimli olarak yüklemek için izleyebileceğiniz bir Kurulum Sihirbazı başlatılır.

Makinenin bir ara sunucu üzerinden hizmete iletişim kurması gerekiyorsa, aracıyı yükledikten sonra, makalenin ilerleyen kısımlarında açıklanan bir komutu çalıştırmanız gerekir. Bu, proxy sunucu sistemi ortam değişkenini `https_proxy`ayarlar.

Windows Installer paketlerine yönelik komut satırı seçeneklerini tanımıyorsanız, [msiexec standart komut satırı seçeneklerini](https://docs.microsoft.com/windows/win32/msi/standard-installer-command-line-options) ve [msiexec komut satırı seçeneklerini](https://docs.microsoft.com/windows/win32/msi/command-line-options)gözden geçirin.

Örneğin, yardım ve hızlı başvuru seçeneğini gözden geçirmek `/?` için yükleme programını parametresiyle çalıştırın. 

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /?
```

Aracı sessizce yüklemek ve var olan `C:\Support\Logs` klasörde bir kurulum günlük dosyası oluşturmak için aşağıdaki komutu çalıştırın.

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentsetup.log"
```

Bağlı makine aracısının dosyaları varsayılan olarak *C:\Program Files\AzureConnectedMachineAgent*' de yüklenir. Kurulum tamamlandıktan sonra aracı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Günlük dizini *%ProgramFiles%\AzureConnectedMachineAgentAgent\logs*' dir.

### <a name="install-with-the-scripted-method"></a>Komut dosyalı yöntemle Install

1. Sunucusunda oturum açın.

1. Yükseltilmiş bir PowerShell komut istemi açın.

1. Betiği kopyaladığınız klasöre veya paylaşıma değiştirin ve `./OnboardingScript.ps1` betiği çalıştırarak sunucuda yürütün.

### <a name="configure-the-agent-proxy-setting"></a>Aracı proxy ayarını yapılandırma

Proxy sunucusu ortam değişkenini ayarlamak için aşağıdaki komutu çalıştırın:

```powershell
# If a proxy server is needed, execute these commands with the proxy URL and port.
[Environment]::SetEnvironmentVariable("https_proxy", "http://{proxy-url}:{proxy-port}", "Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
Restart-Service -Name himds
```

>[!NOTE]
>Aracı bu önizlemede proxy kimlik doğrulamasını ayarlamayı desteklemiyor.
>

### <a name="configure-agent-communication"></a>Aracı iletişimini yapılandırma

Aracıyı yükledikten sonra, aşağıdaki komutu çalıştırarak aracıyı Azure Arc hizmeti ile iletişim kuracak şekilde yapılandırmanız gerekir:

`"%ProgramFiles%\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="install-and-validate-the-agent-on-linux"></a>Linux 'ta aracıyı yükleyip doğrulama

Linux için bağlı makine Aracısı, dağıtım için tercih edilen paket biçiminde sağlanır (. RPM veya. DEB) Microsoft [paket deposunda](https://packages.microsoft.com/)barındırılır. [Kabuk betik paketi `Install_linux_azcmagent.sh` ](https://aka.ms/azcmagent) aşağıdaki eylemleri gerçekleştirir:

- Packages.microsoft.com adresinden Aracı paketini indirmek için konak makineyi yapılandırır.
- Karma kaynak sağlayıcısı paketini kurar.

İsteğe bağlı olarak, `--proxy "{proxy-url}:{proxy-port}"` parametresini dahil ederek aracıyı proxy bilgileriniz ile yapılandırabilirsiniz.

Betik Ayrıca desteklenen ve desteklenmeyen dağıtımları tanımlayan mantığı içerir ve yüklemeyi gerçekleştirmek için gereken izinleri doğrular. 

Aşağıdaki örnek aracıyı indirir ve yükler:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh
```

Aracıyı, proxy sunucunuz üzerinden iletişim kurmak üzere yapılandırma `--proxy` parametresi dahil olmak üzere indirmek ve yüklemek için aşağıdaki komutları çalıştırın:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

### <a name="configure-the-agent-communication"></a>Aracı iletişimini yapılandırma

Aracıyı yükledikten sonra, aşağıdaki komutu çalıştırarak Azure Arc hizmeti ile iletişim kuracak şekilde yapılandırın:

`azcmagent connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="verify-the-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yükledikten ve sunucular için Azure yaya (Önizleme) bağlanacak şekilde yapılandırdıktan sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. [Azure Portal](https://aka.ms/hybridmachineportal)makinelerinizi görüntüleyin.

![Başarılı bir sunucu bağlantısı](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Sonraki adımlar

- VM [Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md), makinenin beklenen Log Analytics çalışma alanına rapor olduğunu doğrulama, [VM 'lerle Azure izleyici](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md)ile izlemeyi etkinleştirme ve çok daha birçok şey için [Azure ilkesi](../../governance/policy/overview.md)'ni kullanarak makinenizi yönetmeyi öğrenin.

- [Log Analytics Aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Makinede çalışan işletim sistemi ve iş yüklerini önceden izlemek, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi çözümleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics Aracısı gerekir.
