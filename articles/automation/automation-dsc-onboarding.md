---
title: Azure Otomasyon Durumu Yapılandırması ile yönetim için onboarding makineleri
description: Azure Otomasyon Durumu Yapılandırması ile yönetim için makineler nasıl ayarlayın?
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 12/10/2019
manager: carmonm
ms.openlocfilehash: d4e008a0bd43f10b01d78a1c388f1ca6fee983ab
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457748"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırması ile yönetim için onboarding makineleri

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırması ile makineleri neden yönetin?

Azure Otomasyon Durumu Yapılandırması, herhangi bir bulutveya şirket içi veri merkezinde İstenilen Durum Yapılandırması (DSC) düğümleri için bir yapılandırma yönetim hizmetidir. **Yapılandırma Yönetimi**altında **Durum yapılandırması (DSC)** seçilerek Azure portalında erişilir. 

Bu hizmet, merkezi ve güvenli bir konumdan binlerce makinede hızlı ve kolay bir şekilde ölçeklenebilirlik sağlar. Makinelere kolayca binebilir, bunları bildirimsel yapılandırmalar atayabilir ve her makinenin belirttiğiniz duruma uygunluğunu gösteren raporları görüntüleyebilirsiniz.

Azure Otomasyon Durumu Yapılandırma hizmeti, Azure Otomasyon runbook'larının PowerShell komut dosyası için ne olduğunu DSC'ye sunar. Başka bir deyişle, Azure Automation'ın PowerShell komut dosyalarını yönetmenize yardımcı olduğu gibi, DSC yapılandırmalarını da yönetmenize yardımcı olur. Azure Otomasyon Durumu Yapılandırması'nı kullanmanın avantajları hakkında daha fazla bilgi edinmek için Azure [Otomasyon Durumu Yapılandırmasına genel bakış](automation-dsc-overview.md)bölümüne bakın.

Azure Otomasyon Durumu Yapılandırması, çeşitli makineleri yönetmek için kullanılabilir:

- Azure sanal makineleri
- Azure sanal makineleri (klasik)
- Fiziksel/sanal Windows makineleri şirket içinde veya Azure dışındaki bir bulutta (AWS EC2 örnekleri dahil)
- Fiziksel/sanal Linux makineleri şirket içinde, Azure'da veya Azure dışındaki bir bulutta

Buluttan makine yapılandırmasını yönetmeye hazır değilseniz, Azure Otomasyon Durumu Yapılandırmasını yalnızca rapor bitiş noktası olarak kullanabilirsiniz. Bu özellik, Yapılandırmaları DSC üzerinden ayarlamanızı (itmenizi) ve Azure Otomasyonu'nda raporlama ayrıntılarını görüntülemenize olanak tanır.

> [!NOTE]
> Yüklü Azure VM İstenen Durum Yapılandırması uzantısı sürümü 2,70'ten büyükse, Azure VM'leri Azure Otomasyon Durumu Yapılandırması ile yönetmek ek ücrete dahil değildir. Daha fazla bilgi için [**Otomasyon fiyatlandırma sayfasına**](https://azure.microsoft.com/pricing/details/automation/)bakın.

Bu makalenin aşağıdaki bölümleri, yukarıda listelenen makinelerde Azure Otomasyon Durumu Yapılandırması'na nasıl binebileceğinizi açıklayınız.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="onboarding-azure-vms"></a>Azure VM'lere Binme

Azure Otomasyon Durumu Yapılandırması, Azure portalı, Azure Kaynak Yöneticisi şablonları veya PowerShell'i kullanarak yapılandırma yönetimi için Azure VM'lerine kolayca binmenizi sağlar. Başlık altında ve bir yönetici vm içine uzaktan gerek kalmadan, Azure VM İstenen Durum Yapılandırma uzantısı Azure Otomasyon Durumu Yapılandırması ile VM kaydeder. Azure uzantısı eşbiriyle uyumlu çalıştığından, ilerlemesini izlemek veya sorun giderme adımları bu makalenin Sorun [Giderme Azure sanal makine onboarding](#troubleshooting-azure-virtual-machine-onboarding) bölümünde sağlanır.

> [!NOTE]
>DSC'yi bir Linux düğümüne dağıtmak **/tmp** klasörünü kullanır. Modüller, `nxautomation` uygun konumlarına yüklemeden önce doğrulama için geçici olarak indirilir. Modüllerin doğru yüklendiğinden emin olmak için, Linux için Log Analytics aracısının **/tmp** klasöründe okuma/yazma izinlerine ihtiyacı vardır.<br><br>
>Linux için Log Analytics aracısı `omsagent` kullanıcı olarak çalışır. `omsagent` Kullanıcıya yazma izni >vermek için `setfacl -m u:omsagent:rwx /tmp`komutu çalıştırın.

### <a name="onboard-a-vm-using-azure-portal"></a>Azure portalını kullanarak yerleşik bir VM

[Azure portalı](https://portal.azure.com/)aracılığıyla Azure Otomasyon Durumu Yapılandırmasına bir Azure VM'de uçağa binmek için:

1. VM'lerde bulunan Azure Otomasyon uyruna gidin. 

2. Durum Yapılandırması sayfasında **Düğümler** sekmesini seçin ve sonra **Ekle'yi**tıklatın.

3. Uçağa bir VM seçin.

4. Makinede istenen durum uzantısı yüklü değilse ve güç durumu çalışıyorsa **Bağlan'ı**tıklatın.

5. **Kayıt**altında, kullanım örneğiniz için gereken [PowerShell DSC Local Configuration Manager değerlerini](/powershell/scripting/dsc/managing-nodes/metaConfig) girin. İsteğe bağlı olarak, VM'ye atamak için bir düğüm yapılandırması girebilirsiniz.

![onboarding](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="onboard-a-vm-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak yerleşik bir VM

Azure Kaynak Yöneticisi şablonlarını kullanarak bir VM'i Azure Otomasyon Durumu Yapılandırmasına dağıtabilir ve yerleşik olarak oluşturabilirsiniz. Mevcut bir VM'den Azure Otomasyon Durumu Yapılandırmasına dahil olan örnek bir şablon için [İstenilen Durum Yapılandırması hizmeti tarafından yönetilen Sunucu'ya](https://azure.microsoft.com/resources/templates/101-automation-configuration/) bakın. Sanal makine ölçeği kümesini yönetiyorsanız, Azure [Otomasyonu tarafından yönetilen Sanal makine ölçeği kümesi yapılandırmasındaörnek](https://azure.microsoft.com/resources/templates/201-vmss-automation-dsc/)şablona bakın.

### <a name="onboard-machines-using-powershell"></a>PowerShell kullanan yerleşik makineler

PowerShell'deki [Register-AzAutomationDscNode](/powershell/module/az.automation/register-azautomationdscnode) cmdlet'i kullanarak Azure Otomasyon Durumu Yapılandırması'na giden VM'leri kullanabilirsiniz. 

> [!NOTE]
>Cmdlet `Register-AzAutomationDscNode` şu anda yalnızca Windows çalışan makineler için uygulanmaktadır, çünkü yalnızca Windows uzantısını tetikler.

### <a name="register-vms-across-azure-subscriptions"></a>Azure aboneliklerinde VM'leri kaydetme

Diğer Azure aboneliklerinden VM'leri kaydetmenin en iyi yolu, DSC uzantısını bir Azure Kaynak Yöneticisi dağıtım şablonunda kullanmaktır. Örnekler, [Azure Kaynak Yöneticisi şablonları ile İstenilen Durum Yapılandırması uzantısında](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template)sağlanır.

Şablonda parametre olarak kullanılacak kayıt anahtarını ve kayıt URL'sini bulmak için, bu makaledeki kayıt bölümünü [kullanarak Onboarding'e](#onboarding-securely-using-registration) bakın.

## <a name="onboarding-physicalvirtual-windows-machines"></a>Fiziksel/sanal Windows makinelerine biniş

Şirket içinde veya diğer bulut ortamlarında (AWS EC2 örnekleri dahil) çalışan Windows sunucularında Azure Otomasyon Durumu Yapılandırması'na binebilirsiniz. Sunucuların [Azure'a giden erişimi](automation-dsc-overview.md#network-planning)olmalıdır.

1. [WMF 5'in](https://aka.ms/wmf5latest) en son sürümünün, Azure Otomasyon Durumu Yapılandırması'na dahil olmak üzere makinelere yüklendiğinden emin olun. Buna ek olarak, WMF 5 onboarding işlemi için kullandığınız bilgisayara yüklü olmalıdır.
1. Gerekli DSC metayapılandırmalarını içeren bir klasör oluşturmak için [DSC metayapılandırmaları üreten](#generating-dsc-metaconfigurations) bölümündeki yönergeleri izleyin. 
1. PowerShell DSC metayapılandırmalarını gemide kullanmak istediğiniz makinelere uzaktan uygulamak için aşağıdaki cmdlet'i kullanın. 

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. PowerShell DSC metayapılandırmalarını uzaktan uygulayamıyorsanız, **metaconfigurations** klasörünü bindiğiniz makinelere kopyalayın. Ardından, makinelerde [Set-DscLocalConfigurationManager'ı](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/set-dsclocalconfigurationmanager?view=powershell-5.1) yerel olarak aramak için kod ekleyin.
1. Azure portalını veya cmdlets'i kullanarak, yerleşik makinelerde Azure Otomasyon hesabınızda kayıtlı Durum Yapılandırma düğümleri olarak görünün.

## <a name="onboarding-physicalvirtual-linux-machines"></a>Fiziksel/sanal Linux makinelerinde biniş

Şirket içinde veya diğer bulut ortamlarında çalışan Linux sunucularında Azure Otomasyon Durumu Yapılandırması'na binebilirsiniz. Sunucuların [Azure'a giden erişimi](automation-dsc-overview.md#network-planning)olmalıdır.

1. [Linux için PowerShell İstenen Durum Yapılandırmasının](https://github.com/Microsoft/PowerShell-DSC-for-Linux) en son sürümünün, Azure Otomasyon Durumu Yapılandırması'na dahil olmak üzere makinelere yüklendiğinden emin olun.
2. [PowerShell DSC Local Configuration Manager varsayılan olarak](/powershell/scripting/dsc/managing-nodes/metaConfig4) kullanım kılıfınızla eşleşirse ve makineüzerinde hem çekim yapıp hem de Azure Otomasyon Durumu Yapılandırması'na rapor vermek istiyorsanız:

   - Azure Automation State Configuration'a dahil olmak `Register.py` üzere her Linux makinesinde PowerShell DSC Local Configuration Manager varsayılanlarını kullanarak yerleşik olarak kullanın.

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Otomasyon hesabınız için kayıt anahtarını ve kayıt URL'sini bulmak için, bu makalenin kayıt bölümünü [kullanarak Onboarding'e](#onboarding-securely-using-registration) bakın.

3. PowerShell DSC Yerel Yapılandırma Yöneticisi (LCM) varsayılanları kullanım durumunuzla eşleşmiyorsa veya yalnızca Azure Otomasyon Durumu Yapılandırması'na rapor veren yerleşik makinelere uymak istiyorsanız, 4-7 adımlarını izleyin. Aksi takdirde, doğrudan adım 7'ye geçin.

4. Gerekli DSC metayapılandırmalarını içeren bir klasör oluşturmak için [Üreten DSC metayapılandırmaları](#generating-dsc-metaconfigurations) bölümündeki yönergeleri izleyin.

5. [WMF 5'in](https://aka.ms/wmf5latest) en son sürümünün onboarding için kullanılan makineye yüklendiğinden emin olun.

6. PowerShell DSC metayapılandırmalarını, yerleşik olarak kullanmak istediğiniz makinelere uzaktan uygulamak için aşağıdaki gibi kod ekleyin.

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

7. PowerShell DSC metayapılandırmalarını uzaktan uygulayamıyorsanız, 4.

8. Azure Otomasyon `Set-DscLocalConfigurationManager.py` Durumu Yapılandırması'na her Linux makinesinde yerel olarak aramak için kod ekleyin.

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

9. Azure portalını veya cmdlets'i kullanarak, yerleşik makinelerin Azure Otomasyon hesabınızda kayıtlı DSC düğümleri olarak artık ortaya çıktığından emin olun.

## <a name="generating-dsc-metaconfigurations"></a>DSC metakonfigürasyonları oluşturma

Azure Otomasyon Durumu Yapılandırması'na herhangi bir makinede yer almak için [Bir DSC metayapılandırması](/powershell/scripting/dsc/managing-nodes/metaConfig)oluşturabilirsiniz. Bu yapılandırma, DSC aracısına Azure Otomasyon Durumu Yapılandırmasından çekilmesini ve/veya rapor etmesini söyler. PowerShell DSC yapılandırması veya Azure Automation PowerShell cmdlets kullanarak Azure Otomasyon Durumu Yapılandırması için Bir DSC metayapılandırması oluşturabilirsiniz.

> [!NOTE]
> DSC metayapılandırmaları, yönetim için bir Otomasyon hesabına makinede kullanmak için gereken sırları içerir. Oluşturduğunuz DSC meta yapılandırmalarını düzgün bir şekilde koruduğunuzdan veya kullanımdan sonra sildiğimden emin olun.

Meta yapılandırmalar için proxy desteği, Windows PowerShell DSC motoru olan LCM tarafından denetlenir. LCM tüm hedef düğümleri üzerinde çalışır ve DSC metayapılandırma komut dosyasına dahil yapılandırma kaynaklarını çağırmaktan sorumludur. Proxy URL tanımlarını ve proxy kimlik belgesini gerektiği gibi , ve `ConfigurationRepositoryWeb` `ResourceRepositoryWeb` `ReportServerWeb` bloklar ekleyerek bir meta yapılandırmaya proxy desteği ekleyebilirsiniz. Bkz. [Yerel Yapılandırma Yöneticisini Yapılandırma.](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig?view=powershell-7)

### <a name="generate-dsc-metaconfigurations-using-a-dsc-configuration"></a>DSC yapılandırması kullanarak DSC meta konfigürasyonları oluşturma

1. VsCode'u (veya en sevdiğiniz düzenleyiciyi) yerel ortamınızdaki bir makinede yönetici olarak açın. Makine [wmf 5](https://aka.ms/wmf5latest) en son sürümü yüklü olmalıdır.
1. Aşağıdaki komut dosyasını yerel olarak kopyalayın. Bu komut dosyası, meta yapılandırmalar oluşturmak için bir PowerShell DSC yapılandırması ve metayapılandırma oluşturma başlatmak için bir komut içerir.

    > [!NOTE]
    > Durum Yapılandırma Düğümü Yapılandırma adları Azure portalında büyük/küçük harf duyarlıdır. Servis talebi eşleşmezse, düğüm **Düğümler** sekmesinin altında gösterilmez.

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl       = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. Otomasyon hesabınız için kayıt anahtarını ve URL'sini ve gemideki makinelerin adlarını doldurun. Diğer tüm parametreler isteğe bağlıdır. Otomasyon hesabınız için kayıt anahtarını ve kayıt URL'sini bulmak için, kayıt bölümünü [kullanarak Güvenli Bir Şekilde Onboarding](#onboarding-securely-using-registration) bölümüne bakın.

1. Makinelerin DSC durum bilgilerini Azure Otomasyon Durumu Yapılandırmasına bildirmesini, ancak yapılandırmayı veya PowerShell modüllerini çekmemelerini istiyorsanız, parametreyi `ReportOnly` doğru şekilde ayarlayın.

1. `ReportOnly` Ayaredilmezse, makineler DSC durum bilgilerini Azure Otomasyon Durumu Yapılandırması ve çekme yapılandırmasına veya PowerShell modüllerine bildirir. Parametreleri `ConfigurationRepositoryWeb`, `ResourceRepositoryWeb`ve `ReportServerWeb` bloklar halinde ayarlayın.

1. Betiği çalıştırın. Artık **dscMetaConfigs**adlı bir çalışma dizin klasörüne sahip olmalısınız, bu klasörde makineler için PowerShell DSC metayapılandırmaları içeren (yönetici olarak).

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="generate-dsc-metaconfigurations-using-azure-automation-cmdlets"></a>Azure Otomasyon cmdlets kullanarak DSC metayapılandırmaları oluşturun

PowerShell DSC LCM varsayılanları kullanım durumunuzla eşleşirse ve makinedeki makinelerin hem azure otomasyon durumu yapılandırmasından çekilmesini hem de Azure Otomasyon Durumu Yapılandırmasına rapor etmesini istiyorsanız, gerekli DSC metayapılandırmalarını Azure Otomasyon cmdlets'ini kullanarak daha basit bir şekilde oluşturabilirsiniz.

1. PowerShell konsolunu veya VSCode'u yerel ortamınızdaki bir makinede yönetici olarak açın.
2. [Connect-AzAccount'ı](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount?view=azps-3.7.0)kullanarak Azure Kaynak Yöneticisi'ne bağlanın.
3. Düğümleri ayarladığınız Otomasyon hesabından, yerleşik olarak kullanmak istediğiniz makineler için PowerShell DSC metayapılandırmalarını indirin.

   ```powershell
   # Define the parameters for Get-AzAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the metaconfiguration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzAutomationDscOnboardingMetaconfig @Params
   ```

1. Artık makineler için PowerShell DSC metayapılandırmalarını içeren bir **DscMetaConfigs** klasörüne sahip olmalısınız (yönetici olarak).

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="onboarding-securely-using-registration"></a>Kaydı kullanarak güvenli bir şekilde binin

Makineler, WMF 5 DSC kayıt protokolü aracılığıyla bir Azure Otomasyon hesabına güvenli bir şekilde binebilir. Bu protokol, Bir DSC düğümünde Azure Otomasyon Durumu Yapılandırması da dahil olmak üzere PowerShell DSC çekme veya rapor sunucusuna kimlik doğrulaması yapılmasına olanak tanır. Düğüm, kayıt URL'sindeki sunucuya kaydedilir ve bir kayıt anahtarı kullanarak kimlik doğrular. Kayıt sırasında, DSC düğümü ve DSC çekme/rapor sunucusu, düğümün sunucu dan kaydı sonrası kimlik doğrulaması için kullanması için benzersiz bir sertifika üzerinde anlaşma sağlar. Bu işlem, örneğin bir düğüm tehlikeye girerse ve kötü amaçlı davranıyorsa, yerleşik düğümlerin birbirini taklit etmesini önler. Kayıttan sonra, kayıt anahtarı yeniden kimlik doğrulama için kullanılmaz ve düğümden silinir.

Azure portalındaki **Hesap Ayarları** altındaki **Anahtarlar'dan** Durum Yapılandırması kayıt protokolü için gerekli bilgileri alabilirsiniz. 

![Azure otomasyon anahtarları ve URL](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- Kayıt URL'si, Keys sayfasındaki URL alanıdır.
- Kayıt anahtarı, Keys sayfasındaki **Birincil erişim anahtarı** alanının veya **İkincil erişim anahtarı** alanının değeridir. Her iki anahtar da kullanılabilir.

Daha fazla güvenlik için, Bir Otomasyon hesabının birincil ve ikincil erişim anahtarlarını Anahtarlar sayfasında istediğiniz zaman yeniden oluşturabilirsiniz. Anahtar rejenerasyonu, gelecekteki düğüm kayıtlarının önceki anahtarları kullanmasını engeller.

## <a name="re-registering-a-node"></a>Düğümü yeniden kaydetme

Bir makineyi Azure Otomasyon Durumu Yapılandırması'nda DSC düğümü olarak kaydettirdikten sonra, gelecekte bu düğümü yeniden kaydetmenizin birkaç nedeni olabilir.

- **Sertifika yenileme.** Windows Server 2019'dan önceki Windows Server sürümlerinde, her düğüm otomatik olarak bir yıl sonra süresi dolan kimlik doğrulama için benzersiz bir sertifika üzerinde görüşür. Sertifikanın süresi yenilenmeden sona ererse, düğüm Azure Otomasyonu ile iletişim kuramıyor ve işaretlenir. `Unresponsive` Şu anda, PowerShell DSC kayıt protokolü sertifikaları sona ermelerine bağlıyken otomatik olarak yenileyemez ve bir yıl sonra düğümleri yeniden kaydetmeniz gerekir. Yeniden kaydetmeden önce, her düğümün WMF 5 RTM çalıştırdığından emin olun. 

    Sertifikanın son kullanma tarihinden 90 gün veya daha kısa bir süre sonra gerçekleştirilen yeniden kayıt işlemi, sertifikanın son kullanma tarihinden sonraki herhangi bir noktada yeni bir sertifikanın oluşturulmasına ve kullanılmasına neden olabilir. Bu sorunun çözümü Windows Server 2019 ve sonraki durumlarda eklenmiştir.

- **DSC LCM değerlerinde değişiklikler.** Düğümilk kaydı sırasında belirlenen [PowerShell DSC LCM değerlerini](/powershell/scripting/dsc/managing-nodes/metaConfig4) değiştirmeniz gerekebilir, örneğin. `ConfigurationMode` Şu anda, bu DSC aracı değerlerini yalnızca yeniden kayıt yoluyla değiştirebilirsiniz. Bir özel durum düğüme atanan Düğüm Yapılandırma değeridir. Bunu doğrudan Azure Automation DSC'de değiştirebilirsiniz.

Bu belgede açıklanan onboarding yöntemlerinden herhangi birini kullanarak düğümü başlangıçta kaydettiğiniz gibi yeniden kaydedebilirsiniz. Yeniden kaydetmeden önce Azure Otomasyon Durumu Yapılandırması'ndan bir düğümü niçin kaldırmanız gerekmez.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Azure sanal makinede sorun giderme

Azure Otomasyon Durumu Yapılandırması, yapılandırma yönetimi için Azure Windows VM'lere kolayca binmenizi sağlar. Başlık altında, Azure VM İstenen Durum Yapılandırması uzantısı, VM'yi Azure Otomasyon Durumu Yapılandırmasına kaydetmek için kullanılır. Azure VM İstenen Durum Yapılandırması uzantısı eşsenkronize çalıştığından, ilerlemesini izlemek ve yürütülmesinde sorun giderme önemli olabilir.

> [!NOTE]
> Azure VM İstenen Durum Yapılandırması uzantısını kullanan bir Azure Windows VM'den Azure Otomasyon Durumu Yapılandırmasına binme yönteminin Azure Otomasyonu'nun kayıtlı olarak göstermesi bir saat kadar sürebilir. Bu gecikme, VM'den Azure Otomasyon Durumu Yapılandırmasına dahil olması gereken Azure VM İstenen Durum Yapılandırması uzantısı tarafından VM'ye WMF 5'in yüklenmesinden kaynaklanmaktadır.

Azure VM İstenen Durum Yapılandırması uzantısının sorun giderme veya durumunu görüntülemek için:

1. Azure portalında, gemide bulunan VM'ye gidin.
2. **Ayarlar'ın**altındaki **Uzantılar'ı** tıklatın. 
3. Şimdi işletim sisteminize bağlı olarak **DSC** veya **DSCForLinux'u**seçin. 
4. Daha fazla bilgi için **ayrıntılı durumu görüntüle'yi**tıklatabilirsiniz.

Sorun giderme hakkında daha fazla bilgi için [Azure Otomasyonu İstenen Durum Yapılandırması (DSC) ile sorun giderme sorunlarına](./troubleshoot/desired-state-configuration.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlarken](automation-dsc-getting-started.md)bkz.
- DSC yapılandırmalarını hedef düğümlerine atayabilmeniz için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli bir dağıtım ardışık yapısında Azure Otomasyon Durumu Yapılandırması'nı kullanma örneği için [bkz.](automation-dsc-cd-chocolatey.md)
