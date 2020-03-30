---
title: Azure portalından karma makineleri Azure'a bağlayın
description: Bu makalede, Azure portalından sunucular için Azure Arc (önizleme) kullanarak aracıyı nasıl yükleyip Makineleri Azure'a bağlayın öğreneceksiniz.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.openlocfilehash: 40885e1de4ff4c16d2a50399c654d8596396ab53
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366379"
---
# <a name="connect-hybrid-machines-to-azure-from-the-azure-portal"></a>Azure portalından karma makineleri Azure'a bağlayın

Ortamınızdaki bir veya az sayıdaki Windows veya Linux makineleri için sunucular için Azure Arc'ı (önizleme) el ile bir dizi adım gerçekleştirerek etkinleştirebilirsiniz. Veya sağladığımız bir şablon komut dosyası çalıştırarak otomatik bir yöntem kullanabilirsiniz. Bu komut dosyası, her iki aracının karşıdan yüklenmesini ve indirilmesini otomatikleştirir.

Bu yöntem, aracıyı yüklemek ve yapılandırmak için makinede yönetici izinlerinin olmasını gerektirir. Linux'ta, kök hesabı nı kullanarak ve Windows'da Yerel Yöneticiler grubunun üyesi siniz.

Başlamadan [önce, ön koşulları](overview.md#prerequisites) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığını doğruladığını unutmayın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="generate-the-installation-script-from-the-azure-portal"></a>Azure portalından yükleme komut dosyası oluşturma

İndirme ve yüklemeyi otomatikleştirmek ve Azure Arc ile bağlantıyı kurmak için komut dosyası Azure portalından edinilebilir. İşlemi tamamlamak için aşağıdakileri yapın:

1. Tarayıcınızdan [Azure portalına](https://aka.ms/hybridmachineportal)gidin.

1. Makineler **- Azure Arc** sayfasında, sol üstteki **Ekle'yi**veya orta bölmenin altındaki **Azure Arc** seçeneğini seçin. 

1. **Yöntem** seç sayfasında, **etkileşimli komut dosyası** döşemesini kullanarak makine ekle'yi seçin ve ardından **komut dosyası oluştur'u**seçin.

1. Komut **dosyası oluştur** sayfasında, makinenin Azure içinde yönetilmesini istediğiniz abonelik ve kaynak grubunu seçin. Makine meta verilerinin depolanacağı bir Azure konumu seçin.

    >[!NOTE]
    >Sunucular için Azure Arc (önizleme) yalnızca aşağıdaki bölgeleri destekler:
    >- BatıUS2
    >- WestEurope
    >- Batı Asya
    >
    >Genel Bakış makalesinde [burada](overview.md#supported-regions) bir bölge seçerken göz önünde bulundurulması gereken diğer hususları gözden geçirin.

1. Komut **dosyası oluştur** sayfasında, **İşletim sistemi** açılır listesinde, komut dosyasının çalışacağı işletim sistemini seçin.

1. Makine internete bağlanmak için bir proxy sunucusu üzerinden iletişim kuruyorsa, **İleri: Proxy Server'ı**seçin. 
1. Proxy **sunucu** sekmesinde, proxy sunucusu IP adresini veya makinenin proxy sunucusuyla iletişim kurmak için kullanacağı ad ve bağlantı noktası numarasını belirtin. Değeri biçime `http://<proxyURL>:<proxyport>`girin. 
1. Gözden Geçir ' i seçin **+ oluşturun.**

1. Gözden **Geçir + oluştur** sekmesinde, özet bilgileri gözden geçirin ve ardından **İndir'i**seçin. Değişiklik yapmanız gerekiyorsa, **Önceki'yi**seçin.

## <a name="install-and-validate-the-agent-on-windows"></a>Aracıyı Windows'da yükleme ve doğrulama

### <a name="install-manually"></a>El ile yükleme

Windows Installer paketi *AzureConnectedMachineAgent.msi*çalıştırarak Bağlı Makine aracısını el ile yükleyebilirsiniz. 

> [!NOTE]
> * Aracıyı yüklemek veya kaldırmak için *Yönetici* izinlerine sahip olmalısınız.
> * İlk olarak Yükleyici paketini hedef sunucudaki bir klasöre veya paylaşılan bir ağ klasöründen indirmeniz ve kopyalamanız gerekir. Herhangi bir seçenek olmadan Installer paketini çalıştırın, aracıyı etkileşimli olarak yüklemek için izleyebileceğiniz bir kurulum sihirbazı başlatır.

Makinenin bir proxy sunucusu aracılığıyla hizmete iletişim kurması gerekiyorsa, aracıyı yükledikten sonra makalenin daha sonra açıklanan bir komutu çalıştırmanız gerekir. Bu proxy sunucu sistemi `https_proxy`ortamı değişkenayarlar.

Windows Installer paketleri için komut satırı seçeneklerini bilmiyorsanız, [Msiexec standart komut satırı seçeneklerini](https://docs.microsoft.com/windows/win32/msi/standard-installer-command-line-options) ve [Msiexec komut satırı seçeneklerini gözden geçirin.](https://docs.microsoft.com/windows/win32/msi/command-line-options)

Örneğin, yardım ve hızlı `/?` başvuru seçeneğini gözden geçirmek için yükleme programını parametreyle çalıştırın. 

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /?
```

Aracıyı sessizce yüklemek ve `C:\Support\Logs` klasörde bir kurulum günlüğü dosyası oluşturmak için aşağıdaki komutu çalıştırın.

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentsetup.log"
```

Bağlı Makine aracısının dosyaları varsayılan olarak *C:\Program Files\AzureConnectedMachineAgent'da*yüklenir. Aracı kurulum bittikten sonra başlamazsa, ayrıntılı hata bilgileri için günlükleri denetleyin. Günlük dizini *%Programfiles%\AzureConnectedMachineAgentAgent\logs'* dur.

### <a name="install-with-the-scripted-method"></a>Komut dosyası yöntemiyle yükleme

1. Sunucuya giriş yapın.

1. Yükseltilmiş bir PowerShell komut istemi açın.

1. Komut dosyasını kopyaladığınız klasöre veya paylaşıma değiştirin ve komut `./OnboardingScript.ps1` dosyasını çalıştırarak sunucuda çalıştırın.

### <a name="configure-the-agent-proxy-setting"></a>Aracı proxy ayarını yapılandırma

Proxy sunucu ortamı değişkenini ayarlamak için aşağıdaki komutu çalıştırın:

```powershell
# If a proxy server is needed, execute these commands with the proxy URL and port.
[Environment]::SetEnvironmentVariable("https_proxy", "http://{proxy-url}:{proxy-port}", "Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
Restart-Service -Name himds
```

>[!NOTE]
>Aracı, bu önizlemede proxy kimlik doğrulaması ayarlamayı desteklemez.
>

### <a name="configure-agent-communication"></a>Aracı iletişimini yapılandırma

Aracıyı yükledikten sonra, aşağıdaki komutu çalıştırarak aracıyı Azure Ark hizmetiyle iletişim kuracak şekilde yapılandırmanız gerekir:

`%ProgramFiles%\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="install-and-validate-the-agent-on-linux"></a>Linux'ta aracıyı yükleyin ve doğrulayın

Linux için Connected Machine aracısı dağıtım için tercih edilen paket formatında sağlanır (. RPM veya . DEB) Microsoft [paket deposunda](https://packages.microsoft.com/)barındırılan. [Kabuk komut `Install_linux_azcmagent.sh` dosyası paketi](https://aka.ms/azcmagent) aşağıdaki eylemleri gerçekleştirir:

- Aracı paketini packages.microsoft.com'dan indirmek için ana makineyi yapılandırır.
- Karma Kaynak Sağlayıcı paketini yükler.

İsteğe bağlı olarak, `--proxy "{proxy-url}:{proxy-port}"` parametreyi ekleyerek aracıyı proxy bilgilerinizle yapılandırabilirsiniz.

Komut dosyası, desteklenen ve desteklenmeyen dağılımları tanımlamak için de mantık içerir ve yüklemeyi gerçekleştirmek için gereken izinleri doğrular. 

Aşağıdaki örnek aracıyı indirir ve yükler:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh
```

Aracıyı proxy sunucunuz üzerinden `--proxy` iletişim kuracak şekilde yapılandırmak için parametre de dahil olmak üzere aracıyı indirmek ve yüklemek için aşağıdaki komutları çalıştırın:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

### <a name="configure-the-agent-communication"></a>Aracı iletişimini yapılandırma

Aracıyı yükledikten sonra, aşağıdaki komutu çalıştırarak Azure Arc hizmetiyle iletişim kuracak şekilde yapılandırın:

`/opt/azcmagent/bin/azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="verify-the-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yükledikten ve sunucular için Azure Arc'a bağlanacak şekilde yapılandırdıktan sonra (önizleme), sunucunun başarıyla bağlandığını doğrulamak için Azure portalına gidin. Makinelerinizi [Azure portalında](https://aka.ms/hybridmachineportal)görüntüleyin.

![Başarılı bir sunucu bağlantısı](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi'ni](../../governance/policy/overview.md)kullanarak makinenizi nasıl yöneteceğimizi öğrenin , VM [konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md)gibi şeyler için, makinenin beklenen Log Analytics çalışma alanına rapor ettiğini doğrulamak, [VM'lerle Azure Monitor](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md)ile izlemeyi etkinleştirmek ve çok daha fazlası.

- [Log Analytics aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Makinede çalışan işletim sistemlerini ve iş yüklerini proaktif olarak izlemek, Otomasyon runbook'larını veya Update Management gibi çözümleri kullanarak yönetmek veya [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics aracısı gereklidir.
