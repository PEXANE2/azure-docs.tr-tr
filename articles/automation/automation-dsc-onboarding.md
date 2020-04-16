---
title: Azure Otomasyon Durumu Yapılandırması ile yönetim için yerleşik makineler
description: Bu makalede, Azure Otomasyon Durumu Yapılandırması ile yönetim için makinelerin nasıl ayarlanır.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 04/15/2020
manager: carmonm
ms.openlocfilehash: 4fc9f701f4f832deb1fed34d7ac9f888521d3830
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406194"
---
# <a name="onboard-machines-for-management-by-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırması ile yönetim için yerleşik makineler

Azure Otomasyon Durumu Yapılandırması, bulutta veya şirket içi veri merkezlerinde İstenilen Durum Yapılandırması (DSC) düğümleri için bir yapılandırma yönetim hizmetidir. **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** seçerek Azure portalındaki hizmete erişebilirsiniz. 

Azure Otomasyon Durumu Yapılandırması ile, merkezi ve güvenli bir konumdan binlerce makinede hızlı ve kolay bir şekilde ölçeklenebilirlik elde edebilirsiniz. Makinelere kolayca binebilir, bunları bildirimsel yapılandırmalar atayabilir ve her makinenin belirttiğiniz duruma uygunluğunu gösteren raporları görüntüleyebilirsiniz.

Azure Otomasyon Durumu Yapılandırma hizmeti, Azure Otomasyon runbook'larının PowerShell komut dosyası için ne olduğunu DSC'ye sunar. Başka bir deyişle, Azure Automation'ın PowerShell komut dosyalarını yönetmenize yardımcı olduğu gibi, DSC yapılandırmalarını da yönetmenize yardımcı olur. Azure Otomasyon Durumu Yapılandırması'nı kullanmanın avantajları hakkında daha fazla bilgi edinmek için Azure [Otomasyon Durumu Yapılandırmasına genel bakış](automation-dsc-overview.md)bölümüne bakın.

Çeşitli makineleri yönetmek için Azure Otomasyon Durumu Yapılandırmasını kullanabilirsiniz:

- Azure sanal makineleri (VM)
- Azure sanal makineleri (klasik)
- Fiziksel/sanal Windows makineleri şirket içinde veya Azure dışındaki bir bulutta (Amazon Web Hizmetleri [AWS] Elastik İşlem Bulutu [EC2] örnekleri dahil)
- Fiziksel/sanal Linux makineleri şirket içinde, Azure'da veya Azure dışındaki bir bulutta

Buluttan makine yapılandırmasını yönetmeye hazır değilseniz, Azure Otomasyon Durumu Yapılandırmasını yalnızca rapor bitiş noktası olarak kullanabilirsiniz. Bu özellik, Yapılandırmaları DSC üzerinden ayarlamanıza veya itmenize ve Azure Otomasyonu'nda raporlama ayrıntılarını görüntülemenize olanak tanır.

> [!NOTE]
> Yüklü Azure VM İstenen Durum Yapılandırması uzantınız sürüm 2,70 veya daha yeniyse, Azure VM'leri Azure Otomasyon Durumu Yapılandırması ile yönetmek ek ücrete dahil değildir. Daha fazla bilgi için Otomasyon [fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.

Bu makalenin aşağıdaki bölümleri, daha önce listelenen makineleri Azure Otomasyon Durumu Yapılandırması'na nasıl ekebileceğinizi ana hatlarını açıklayabilirsiniz.

## <a name="onboard-azure-vms"></a>Yerleşik Azure VM'ler

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Çalışmacınız için Az modül yükleme yönergeleri için Azure [PowerShell modüllerini yükleyin'e](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda [Azure PowerShell modüllerini güncelleştirerek](automation-update-azure-modules.md)modüllerinizi en son sürüme güncelleştirebilirsiniz.

Azure uzantısı eşbiriyle uyumlu çalıştığından, bu makalenin Sorun [Giderme Azure sanal makine bölümünde](#troubleshoot-azure-virtual-machine-onboarding) ilerlemesini izlemek veya sorun giderme adımları sağlarız.

> [!NOTE]
> DSC'yi bir Linux düğümüne dağıtmak */tmp* klasörünü kullanır. Bu tür `nxautomation` modüller yüklenmeden önce doğrulama için geçici olarak indirilir. Modüllerin doğru şekilde yüklendiğinden emin olmak için, Linux için Log Analytics aracısının */tmp* klasöründe okuma/yazma izinlerine ihtiyacı vardır.<br><br>
> Linux için Log Analytics *aracısı omsagent* kullanıcısı olarak çalışır. *Omsagent* kullanıcısına yazma izni vermek için `setfacl -m u:omsagent:rwx /tmp`komutu çalıştırın.

### <a name="onboard-vms-by-using-the-azure-portal"></a>Azure portalını kullanarak yerleşik VM'ler

[Azure portalı](https://portal.azure.com/)aracılığıyla Azure VM'leri ile Azure Otomasyon Durumu Yapılandırması'na binmeiçin:

1. VM'lerde yer almak istediğiniz Azure Otomasyon uyrun. 

1. Durum **Yapılandırması** sayfasında **Düğümler** sekmesini seçin ve sonra **Ekle'yi**seçin.

1. Uçağa bir VM seçin.

1. Makinede PowerShell İstenilen Durum uzantısı yüklü değilse ve güç durumu çalışıyorsa **Bağlan'ı**seçin.

1. **Kayıt**altında, kullanım durumunuz için gereken [PowerShell DSC Local Configuration Manager (LCM) değerlerini](/powershell/scripting/dsc/managing-nodes/metaConfig) girin. İsteğe bağlı olarak, VM'ye atamak için bir düğüm yapılandırması girebilirsiniz.

![VM kayıt bölmesi](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="onboard-vms-by-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak yerleşik VM'ler

Azure Kaynak Yöneticisi şablonlarını kullanarak bir VM'i Azure Otomasyon Durumu Yapılandırmasına dağıtabilir ve yerleşik olarak oluşturabilirsiniz. Mevcut bir VM ile Azure Otomasyon Durumu Yapılandırması'nda bulunan örnek bir şablon [için](https://azure.microsoft.com/resources/templates/101-automation-configuration/)bkz. Sanal makine ölçeği kümesini yönetiyorsanız, [Azure Otomasyonu tarafından yönetilen Sanal makine ölçeği kümesi yapılandırmasındaörnek](https://azure.microsoft.com/resources/templates/201-vmss-automation-dsc/)şablona bakın.

### <a name="onboard-vms-by-using-powershell"></a>PowerShell kullanarak yerleşik VM'ler

PowerShell'deki [Register-AzAutomationDscNode](/powershell/module/az.automation/register-azautomationdscnode) cmdlet'i kullanarak Azure Otomasyon Durumu Yapılandırması'na giden VM'leri kullanabilirsiniz. 

> [!NOTE]
>`Register-AzAutomationDscNode` Cmdlet yalnızca Windows uzantısını tetiklediği için cmdlet şu anda yalnızca Windows çalıştıran makineler için uygulanmaktadır.

### <a name="register-vms-across-azure-subscriptions"></a>Azure aboneliklerinde VM'leri kaydetme

Diğer Azure aboneliklerinden VM'leri kaydetmenin en iyi yolu, DSC uzantısını bir Azure Kaynak Yöneticisi dağıtım şablonunda kullanmaktır. Örnekler, [Azure Kaynak Yöneticisi şablonları ile İstenilen Durum Yapılandırması uzantısında](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template)sağlanır.

Şablonda parametre olarak kullanılacak kayıt anahtarını ve kayıt URL'sini bulmak için, bu makaledeki kayıt bölümünü kullanarak Yerleşik Kart'a [güvenli bir şekilde](#onboard-securely-by-using-registration) bakın.

## <a name="onboard-physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure"></a>Şirket içinde veya Azure dışındaki bir bulutta yerleşik fiziksel/sanal Windows makineleri

Şirket içinde veya AWS EC2 örnekleri de dahil olmak üzere diğer bulut ortamlarında çalışan Windows sunucularında Azure Otomasyon Durumu Yapılandırması'na binebilirsiniz. Sunucuların [Azure'a giden erişimi](automation-dsc-overview.md#network-planning)olmalıdır.

1. [Windows Management Framework 5'in](https://aka.ms/wmf5latest) en son sürümünün Azure Otomasyon Durumu Yapılandırması'na dahil edilecek makinelere yüklendiğinden emin olun. Ayrıca, Windows Management Framework 5,,, onboarding işlemi için kullandığınız bilgisayara yüklenmiş olmalıdır.
1. Gerekli DSC metayapılandırmalarını içeren bir klasör oluşturmak [için, DSC metayapılandırmaları oluştur](#generate-dsc-metaconfigurations) bölümündeki yönergeleri izleyin. 
1. PowerShell DSC metayapılandırmalarını gemide kullanmak istediğiniz makinelere uzaktan uygulamak için aşağıdaki cmdlet'i kullanın: 

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. PowerShell DSC metayapılandırmalarını uzaktan uygulayamıyorsanız, **metaconfigurations** klasörünü bindiğiniz makinelere kopyalayın. Ardından, makinelerde [Set-DscLocalConfigurationManager'ı](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/set-dsclocalconfigurationmanager?view=powershell-5.1) yerel olarak aramak için kod ekleyin.

1. Azure portalında veya cmdlets kullanarak, yerleşik makinelerin Azure Otomasyon hesabınızda kayıtlı Durum Yapılandırma düğümleri olarak göründüğünü doğrulayın.

## <a name="onboard-physicalvirtual-linux-machines-on-premises-or-in-a-cloud-other-than-azure"></a>Şirket içinde veya Azure dışındaki bir bulutta yerleşik fiziksel/sanal Linux makineleri

Şirket içinde veya diğer bulut ortamlarında çalışan Linux sunucularında Azure Otomasyon Durumu Yapılandırması'na binebilirsiniz. Sunucuların [Azure'a giden erişimi](automation-dsc-overview.md#network-planning)olmalıdır.

1. [Linux için PowerShell İstenen Durum Yapılandırmasının](https://github.com/Microsoft/PowerShell-DSC-for-Linux) en son sürümünün Azure Otomasyon Durumu Yapılandırması'na dahil edilecek makinelere yüklendiğinden emin olun.
1. [PowerShell DSC LCM varsayılanları](/powershell/scripting/dsc/managing-nodes/metaConfig4) kullanım kılıfınızla eşleşirse ve makinelerin hem çekinip hem de Azure Otomasyon Durumu Yapılandırması'na rapor vermeleri için dahilolmak istiyorsanız, aşağıdakileri yapın:

   a. Azure Automation State Configuration'a dahil olmak `Register.py` üzere her Linux makinesinde, PowerShell DSC Local Configuration Manager varsayılanlarını kullanarak bunları kullanmak için bunları kullanın.

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   b. Otomasyon hesabınız için kayıt anahtarını ve kayıt URL'sini bulmak için, bu makalenin kayıt bölümünü [kullanarak Yerleşik'e güvenli bir şekilde](#onboard-securely-by-using-registration) bakın.  
   c. Adım 4'e atla.
   
1. PowerShell DSC LCM varsayılanları kullanım durumunuzla eşleşmiyorsa veya yalnızca Azure Otomasyon Durumu Yapılandırması'na rapor veren yerleşik makinelere sahip olmak istiyorsanız, aşağıdaki adımları izleyin. Aksi takdirde, doğrudan adım d'ye geçin.

    a. Gerekli DSC metayapılandırmalarını içeren bir klasör oluşturmak [için, DSC metayapılandırmaları oluştur](#generate-dsc-metaconfigurations) bölümündeki yönergeleri izleyin.

    b. [Windows Management Framework 5'in](https://aka.ms/wmf5latest) en son sürümünün biniş için kullanılan makineye yüklendiğinden emin olun.

    c. PowerShell DSC meta yapılandırmalarını gemide kullanmak istediğiniz makinelere uzaktan uygulamak için aşağıdaki kodu ekleyin:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

    d. PowerShell DSC metayapılandırmalarını uzaktan uygulayamıyorsanız, adım 3a'da açıklanan klasörden uzak makinelere karşılık gelen meta yapılandırmaları Linux makinelerine kopyalayın.

1. Azure Otomasyon Durumu Yapılandırması'na dahil edilecek her Linux makinesinde yerel olarak *Set-DscLocalConfigurationManager.py* çağırmak için kod ekleyin.

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

1. Azure portalını veya cmdlets'i kullanarak, yerleşik makinelerin Azure Otomasyon hesabınızda kayıtlı DSC düğümleri olarak artık ortaya çıktığından emin olun.

## <a name="generate-dsc-metaconfigurations"></a>DSC metakonfigürasyonları oluşturma

Azure Otomasyon Durumu Yapılandırması'na herhangi bir makinede yer almak için [Bir DSC metayapılandırması](/powershell/scripting/dsc/managing-nodes/metaConfig)oluşturabilirsiniz. Bu yapılandırma, DSC aracısına Azure Otomasyon Durumu Yapılandırmasından çekilmesini veya rapor etmesini söyler. PowerShell DSC yapılandırması veya Azure Automation PowerShell cmdlets kullanarak Azure Otomasyon Durum Yapılandırması için Bir DSC metayapılandırması oluşturabilirsiniz.

> [!NOTE]
> DSC meta yapılandırmaları, bir makineyi yönetim için bir Otomasyon hesabına takmak için gereken sırları içerir. Oluşturduğunuz DSC meta yapılandırmalarını düzgün bir şekilde koruduğunuzdan veya kullanımdan sonra sildiğinden emin olun.

Meta yapılandırmalar için proxy desteği, Windows PowerShell DSC motoru olan LCM tarafından denetlenir. LCM tüm hedef düğümlerde çalışır ve DSC metayapılandırma komut dosyasına dahil edilen yapılandırma kaynaklarını çağırmaktan sorumludur. 

Proxy URL tanımlarını ve proxy kimlik belgesini gerektiği gibi , ve `ConfigurationRepositoryWeb` `ResourceRepositoryWeb` `ReportServerWeb` bloklar ekleyerek bir meta yapılandırmaya proxy desteği ekleyebilirsiniz. Bkz. [Yerel Yapılandırma Yöneticisi'ni Yapılandır.](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig?view=powershell-7)

### <a name="generate-dsc-metaconfigurations-by-using-a-dsc-configuration"></a>DSC yapılandırması kullanarak DSC metakonfigürasyonları oluşturma

1. Yerel ortamınızdaki bir makinede yönetici olarak Visual Studio Code'u (veya en sevdiğiniz düzenleyiciyi) açın. Makine, [Windows Management Framework 5'in](https://aka.ms/wmf5latest) en son sürümünü yüklü olmalıdır.
1. Aşağıdaki komut dosyasını yerel olarak kopyalayın. Bu komut dosyası, meta yapılandırmalar oluşturmak için bir PowerShell DSC yapılandırması ve metayapılandırma oluşturma başlatmak için bir komut içerir.

    > [!NOTE]
    > Durum Yapılandırma Düğümü Yapılandırma adları Azure portalında büyük/küçük harf duyarlıdır. Servis talebi eşleşmezse, düğüm **Düğümler** sekmesinin altında gösterilmez.

   ```powershell
   # The DSC configuration that will generate metaconfigurations.
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

    # Create the metaconfigurations.
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: Edit the following as needed for your use case.
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

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked.
   # For more info about splatting, run: Get-Help -Name about_Splatting.
   DscMetaConfigs @Params
   ```

1. Otomasyon hesabınız için kayıt anahtarını ve URL'sini ve gemideki makinelerin adlarını doldurun. Diğer tüm parametreler isteğe bağlıdır. Otomasyon hesabınız için kayıt anahtarını ve kayıt URL'sini bulmak [için, kayıt](#onboard-securely-by-using-registration) bölümünü kullanarak Yerleşik'e güvenli bir şekilde bakın.

1. Makinelerin DSC durum bilgilerini Azure Otomasyon Durumu Yapılandırmasına bildirmesini, ancak yapılandırmayı veya PowerShell modüllerini çekmemelerini istiyorsanız, parametreyi `ReportOnly` *doğru*ayarlayın.

1. `ReportOnly` Ayaredilmezse, makineler DSC durum bilgilerini Azure Otomasyon Durumu Yapılandırması ve çekme yapılandırmasına veya PowerShell modüllerine bildirir. Parametreleri `ConfigurationRepositoryWeb`, `ResourceRepositoryWeb`ve `ReportServerWeb` bloklar halinde ayarlayın.

1. Betiği çalıştırın. Artık, makineler için (yönetici olarak) PowerShell DSC metayapılandırmalarını içeren *DscMetaConfigs*adlı bir çalışma dizin klasörüne sahip olmalısınız.

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="generate-dsc-metaconfigurations-by-using-azure-automation-cmdlets"></a>Azure Automation cmdlets kullanarak DSC metayapılandırmaları oluşturun

PowerShell DSC LCM varsayılanları kullanım durumunuzla eşleşirse ve makinedeki makinelerin hem azure otomasyon durumu yapılandırmasından çekilmesini hem de Azure Otomasyon Durumu Yapılandırmasına rapor etmesini istiyorsanız, Azure Otomasyon cmdletlerini kullanarak gerekli DSC metayapılandırmalarını daha basit bir şekilde oluşturabilirsiniz.

1. PowerShell konsolunu veya Visual Studio Kodunu yerel ortamınızdaki bir makinede yönetici olarak açın.
1. [Connect-AzAccount'ı](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount?view=azps-3.7.0)kullanarak Azure Kaynak Yöneticisi'ne bağlanın.
1. Düğümleri ayarladığınız Otomasyon hesabından, dahilolmak istediğiniz makineler için PowerShell DSC metayapılandırmalarını indirin.

   ```powershell
   # Define the parameters for Get-AzAutomationDscOnboardingMetaconfig using PowerShell Splatting.
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the metaconfiguration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked.
   # For more info about splatting, run: Get-Help -Name about_Splatting.
   Get-AzAutomationDscOnboardingMetaconfig @Params
   ```

1. Artık, (yönetici olarak) yerleşik olmak istediğiniz makineler için PowerShell DSC metayapılandırmalarını içeren bir *DscMetaConfigs* klasörüne sahip olmalısınız.

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="onboard-securely-by-using-registration"></a>Kayıt kullanarak güvenli bir şekilde yerleşik

Makineler, Windows Management Framework 5 DSC kayıt protokolü aracılığıyla bir Azure Otomasyon hesabına güvenli bir şekilde binilebilir. Bu protokol, Bir DSC düğümünde Azure Otomasyon Durumu Yapılandırması da dahil olmak üzere PowerShell DSC çekme veya rapor sunucusuna kimlik doğrulaması yapılmasına olanak tanır. Düğüm, kayıt URL'sindeki sunucuya kaydedilir ve bir kayıt anahtarı kullanarak kimlik doğrular. 

Kayıt sırasında, DSC düğümü ve DSC çekme/rapor sunucusu, düğümün sunucu dan kaydı sonrası kimlik doğrulaması için kullanması için benzersiz bir sertifika üzerinde anlaşma sağlar. Bu işlem, yerleşik düğümlerin birbirini taklit etmesini engeller (örneğin, düğüm tehlikeye girerse ve kötü amaçlı davranıyorsa). 

Kayıttan sonra, kayıt anahtarı kimlik doğrulaması için yeniden kullanılmaz ve düğümden silinir.

Durum Yapılandırması kayıt protokolü için gerekli olan bilgileri almak için Azure portalına gidin ve **Hesap Ayarları**altında **Keys'i**seçin. 

![Anahtarlar bölmesinde Azure otomasyon anahtarları ve URL](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- **Kayıt URL'si**: **URL** kutusundaki değer.
- **Kayıt anahtarı**: **Birincil erişim anahtarı** kutusundaki veya **İkincil erişim anahtar** kutusundaki değer. Her iki anahtarı da kullanabilirsiniz.

Daha fazla güvenlik için, Bir Otomasyon hesabının birincil ve ikincil erişim anahtarlarını **Anahtarlar** bölmesinde istediğiniz zaman yeniden oluşturabilirsiniz. Anahtar rejenerasyonu, gelecekteki düğüm kayıtlarının önceki anahtarları kullanmasını engeller.

## <a name="re-register-a-node"></a>Düğümü yeniden kaydetme

Bir makineyi Azure Otomasyon Durumu Yapılandırması'nda DSC düğümü olarak kaydettikten sonra, aşağıdaki nedenlerden herhangi biri için bu düğümü gelecekte yeniden kaydetmeniz gerekebilir:

- **Sertifika yenileme**: Windows Server 2019'dan önceki sürümlerinde, her düğüm otomatik olarak bir yıl sonra süresi dolan kimlik doğrulama için benzersiz bir sertifika üzerinde pazarlık yapar. Sertifikanın süresi yenilenmeden sona ererse, düğüm Azure Otomasyonu ile iletişim kuramıyor ve *Yanıt vermiyor*olarak işaretlenir. 

  Şu anda, PowerShell DSC kayıt protokolü sertifikaları sona ermelerine bağlıyken otomatik olarak yenileyemez ve bir yıl sonra düğümleri yeniden kaydetmeniz gerekir. Yeniden kaydolmadan önce, her düğümün Windows Management Framework 5 RTM çalıştırdığından emin olun. 

    Yeniden kayıt, sertifikanın son kullanma tarihinden itibaren 90 veya daha az gün içinde veya sertifikanın son kullanma tarihinden sonraki herhangi bir noktada yeni bir sertifikanın oluşturulmasına ve kullanılmasına neden olarak gerçekleştirilir. Bu sorunun çözümü Windows Server 2019 ve sonraki durumlarda eklenmiştir.

- **DSC LCM değerlerinde yapılan değişiklikler**: Düğümün ilk kaydı sırasında ayarlanan [PowerShell DSC LCM değerlerini](/powershell/scripting/dsc/managing-nodes/metaConfig4) değiştirmeniz gerekebilir (örneğin, `ConfigurationMode`). Şu anda, bu DSC aracı değerlerini yalnızca yeniden kayıt yoluyla değiştirebilirsiniz. Bir özel durum düğüme atanan Düğüm Yapılandırma değeridir. Bu değeri Azure Automation DSC'de doğrudan değiştirebilirsiniz.

Bir düğümü, bu belgede açıklanan onboarding yöntemlerinden herhangi birini kullanarak, başlangıçta kaydettiğiniz gibi yeniden kaydedebilirsiniz. Yeniden kaydetmeden önce Azure Otomasyon Durumu Yapılandırması'ndan bir düğümü niçin kaldırmanız gerekmez.

## <a name="troubleshoot-azure-virtual-machine-onboarding"></a>Azure sanal makineüzerinde sorun giderme

Azure Otomasyon Durumu Yapılandırması, yapılandırma yönetimi için Azure Windows VM'lere kolayca binmenizi sağlar. Başlık altında, Azure VM İstenen Durum Yapılandırması uzantısı, VM'yi Azure Otomasyon Durumu Yapılandırmasına kaydetmek için kullanılır. Azure VM İstenen Durum Yapılandırması uzantısı eşeşitolarak çalıştığından, ilerlemesini izleme ve yürütülmesinde sorun giderme önemli olabilir.

> [!NOTE]
> Azure VM İstenen Durum Yapılandırması uzantısını kullanan bir Azure Windows VM'den Azure Otomasyon Durumu Yapılandırmasına binme yönteminin Azure Otomasyonu'nun kayıtlı olarak göstermesi bir saat kadar sürebilir. Bu gecikme, VM'den Azure Otomasyon Durumu Yapılandırmasına dahil olması gereken Azure VM İstenen Durum Yapılandırması uzantısı tarafından VM'ye Windows Management Framework 5'in yüklenmesinden kaynaklanır.

Azure VM İstenen Durum Yapılandırması uzantısının sorun giderme veya durumunu görüntülemek için:

1. Azure portalında, gemiye binilmekte olan VM'ye gidin.
2. **Ayarlar** bölümünde **Uzantılar**’ı seçin. 
3. İşletim sisteminize bağlı olarak **DSC** veya **DSCForLinux'u**seçin. 
4. Uzantı hakkinda daha fazla bilgi için **ayrıntılı durumu güzden'** i seçin.

Sorun giderme hakkında daha fazla bilgi için [Azure Otomasyonu İstenen Durum Yapılandırması (DSC) ile sorun giderme sorunlarına](./troubleshoot/desired-state-configuration.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlayın'ı](automation-dsc-getting-started.md)görün.
- DSC yapılandırmalarını hedef düğümlerine atayabileceğiniz şekilde derleme hakkında bilgi edinmek için Azure [Otomasyon Durumu Yapılandırmasında Derleme yapılandırmalarına](automation-dsc-compile.md)bakın.
- PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneği için, [Azure Otomasyon Durumu Yapılandırması ve Chocolatey'i kullanarak sanal makinelere sürekli dağıtım](automation-dsc-cd-chocolatey.md)bölümüne bakın.
