---
title: Azure Otomasyonu durum yapılandırmasına göre yönetim için makine ekleme
description: Azure Otomasyonu durum yapılandırması ile yönetim için makineleri ayarlama
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 12/10/2019
manager: carmonm
ms.openlocfilehash: d4e008a0bd43f10b01d78a1c388f1ca6fee983ab
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81457748"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırmasına göre yönetim için makine ekleme

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması olan makineleri neden yönetmeliyim?

Azure Otomasyonu durum yapılandırması, herhangi bir bulutta veya şirket içi veri merkezinde Istenen durum yapılandırması (DSC) düğümleri için bir yapılandırma yönetim hizmetidir. **Yapılandırma yönetimi**altında **Durum Yapılandırması (DSC)** seçilerek Azure Portal erişilir. 

Bu hizmet, merkezi bir güvenli konumdan binlerce makine genelinde hızla ve kolayca ölçeklenebilirlik sunar. Makineleri kolayca ekleyebilir, bunlara bildirime dayalı yapılandırmalara atayabilir ve her makinenin, belirttiğiniz istenen durumla uyumluluğunu gösteren raporları görüntüleyebilirsiniz.

Azure Otomasyonu durum yapılandırma hizmeti, Azure Otomasyonu runbook 'larının PowerShell komut dosyasına ne kadar olduğunu DSC 'ye göre yapılır. Diğer bir deyişle, Azure Otomasyonu 'nun PowerShell betiklerini yönetmenize yardımcı olduğu şekilde, DSC yapılandırmalarının yönetilmesine de yardımcı olur. Azure Otomasyonu durum yapılandırması kullanmanın avantajları hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu durum yapılandırmasına genel bakış](automation-dsc-overview.md).

Azure Otomasyonu durum yapılandırması, çeşitli makineleri yönetmek için kullanılabilir:

- Azure sanal makineleri
- Azure sanal makineleri (klasik)
- Şirket içinde veya Azure dışındaki bir bulutta bulunan fiziksel/sanal Windows makineleri (AWS EC2 örnekleri dahil)
- Şirket içinde, Azure 'da veya Azure dışındaki bir bulutta bulunan fiziksel/sanal Linux makineleri

Makine yapılandırmasını buluttan yönetmeye hazırsanız, Azure Otomasyonu durum yapılandırması 'nı yalnızca rapor uç noktası olarak kullanabilirsiniz. Bu özellik, konfigürasyonları DSC aracılığıyla ayarlamanıza ve Azure Otomasyonu 'nda raporlama ayrıntılarını görüntülemenize olanak sağlar.

> [!NOTE]
> Azure Otomasyonu durum yapılandırması ile Azure VM 'Leri yönetmek, yüklü Azure VM Istenen durum yapılandırma uzantısı sürümü 2,70 ' den büyükse ek ücret alınmaz. Daha fazla bilgi için bkz. [**Automation fiyatlandırma sayfası**](https://azure.microsoft.com/pricing/details/automation/).

Bu makalenin aşağıdaki bölümleri, yukarıda listelenen makineleri Azure Otomasyonu durum yapılandırmasına nasıl kullanabileceğinizi özetler.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="onboarding-azure-vms"></a>Azure VM 'lerini ekleme

Azure Otomasyonu durum yapılandırması, Azure portal, Azure Resource Manager şablonlarını veya PowerShell 'i kullanarak yapılandırma yönetimi için Azure sanal makinelerini kolayca eklemenizi sağlar. Yönetim altında ve bir yöneticinin bir VM 'ye uzak olarak gerek kalmadan, Azure VM Istenen durum yapılandırma uzantısı VM 'yi Azure Otomasyonu durum yapılandırması ile kaydeder. Azure uzantısı zaman uyumsuz olarak çalıştığından, bu makalenin ilerleme durumunu izlemek veya sorunu giderme adımları Bu makalenin [Azure sanal makinesini ekleme sorunlarını giderme](#troubleshooting-azure-virtual-machine-onboarding) bölümünde verilmiştir.

> [!NOTE]
>DSC 'yi bir Linux düğümüne dağıtmak **/tmp** klasörünü kullanır. `nxautomation` Gibi modüller, uygun konumlarına yüklemeden önce, doğrulama için geçici olarak indirilir. Modüllerin doğru bir şekilde yüklendiğinden emin olmak için, Linux Log Analytics aracısına **/tmp** klasöründe okuma/yazma izinleri gerekir.<br><br>
>Linux için Log Analytics Aracısı `omsagent` Kullanıcı olarak çalışır. `omsagent` Kullanıcıya >yazma izni vermek için komutunu `setfacl -m u:omsagent:rwx /tmp`çalıştırın.

### <a name="onboard-a-vm-using-azure-portal"></a>Azure portal kullanarak VM ekleme

[Azure Portal](https://portal.azure.com/)aracılığıyla Azure Otomasyonu durum yapılandırmasına BIR Azure VM eklemek için:

1. VM 'Leri eklemek için Azure Otomasyonu hesabına gidin. 

2. Durum Yapılandırması sayfasında **düğümler** sekmesini seçin ve ardından **Ekle**' ye tıklayın.

3. Eklenecek VM 'yi seçin.

4. Makinede PowerShell istenen durum uzantısı yüklü değilse ve güç durumu çalışıyorsa, **Bağlan**' a tıklayın.

5. **Kayıt**' ın altında, kullanım durumu Için gereken [PowerShell DSC yerel Configuration Manager değerlerini](/powershell/scripting/dsc/managing-nodes/metaConfig) girin. İsteğe bağlı olarak, VM 'ye atanacak bir düğüm yapılandırması girebilirsiniz.

![ekleme](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="onboard-a-vm-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak VM ekleme

Azure Resource Manager şablonları kullanarak Azure Otomasyonu durum yapılandırmasına bir VM dağıtabilir ve ekleyebilirsiniz. Var olan bir VM 'yi Azure Otomasyonu durum yapılandırması 'nda izleyen örnek bir şablon için bkz. [Istenen durum yapılandırma hizmeti tarafından yönetilen sunucu](https://azure.microsoft.com/resources/templates/101-automation-configuration/) . Bir sanal makine ölçek kümesini yönetiyorsanız, [sanal makine ölçek kümesi yapılandırmasında Azure Otomasyonu tarafından yönetilen](https://azure.microsoft.com/resources/templates/201-vmss-automation-dsc/)örnek şablona bakın.

### <a name="onboard-machines-using-powershell"></a>PowerShell kullanarak makine ekleme

Azure Otomasyonu durum yapılandırmasına VM 'Leri eklemek için PowerShell 'de [register-AzAutomationDscNode](/powershell/module/az.automation/register-azautomationdscnode) cmdlet 'ini kullanabilirsiniz. 

> [!NOTE]
>`Register-AzAutomationDscNode` Cmdlet yalnızca Windows uzantısını tetiklediği için, yalnızca Windows çalıştıran makineler için uygulanır.

### <a name="register-vms-across-azure-subscriptions"></a>VM 'Leri Azure abonelikleri arasında kaydetme

Diğer Azure aboneliklerinden VM 'Lerin kaydedileceği en iyi yol, Azure Resource Manager dağıtım şablonunda DSC uzantısını kullanmaktır. Örnek olarak, [Istenen durum yapılandırma uzantısında Azure Resource Manager şablonlarıyla birlikte](https://docs.microsoft.com/azure/virtual-machines/extensions/dsc-template)verilmiştir.

Şablonda parametreler olarak kullanılacak kayıt anahtarını ve kayıt URL 'sini bulmak için, bu makaledeki [kaydı kullanarak güvenli](#onboarding-securely-using-registration) bir şekilde ekleme bölümüne bakın.

## <a name="onboarding-physicalvirtual-windows-machines"></a>Fiziksel/sanal Windows makinelerini ekleme

Şirket içinde veya diğer bulut ortamlarında (AWS EC2 örnekleri dahil) çalışan Windows sunucularını Azure Otomasyonu durum yapılandırması ' na ekleyebilirsiniz. Sunucuların [Azure 'a giden erişimi](automation-dsc-overview.md#network-planning)olmalıdır.

1. Azure Otomasyonu durum yapılandırması 'na eklemek için makinelerde [WMF 5](https://aka.ms/wmf5latest) ' in en son sürümünün yüklü olduğundan emin olun. Ayrıca, ekleme işlemi için kullandığınız bilgisayarda WMF 5 ' ın yüklü olması gerekir.
1. Gerekli DSC meta yapılandırmalarının bulunduğu bir klasör oluşturmak için [DSC Metaconfigurations oluşturma](#generating-dsc-metaconfigurations) bölümündeki yönergeleri izleyin. 
1. PowerShell DSC metaconfigurations 'ı eklemek istediğiniz makinelere uzaktan uygulamak için aşağıdaki cmdlet 'i kullanın. 

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. PowerShell DSC metayapılandırmalarının uzaktan uygulanmamanız durumunda, **metaconfigurations** klasörünü, eklediğiniz makinelere kopyalayın. Ardından, makinelerde yerel olarak [set-DscLocalConfigurationManager](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/set-dsclocalconfigurationmanager?view=powershell-5.1) çağırmak için kod ekleyin.
1. Azure portal veya cmdlet 'lerini kullanarak, eklenecek makinelerin Azure Otomasyonu hesabınızda kayıtlı durum yapılandırma düğümleri olarak göründüğünü doğrulayın.

## <a name="onboarding-physicalvirtual-linux-machines"></a>Fiziksel/sanal Linux makinelerini ekleme

Şirket içinde veya diğer bulut ortamlarında çalışan Linux sunucularını Azure Otomasyonu durum yapılandırması 'na ekleyebilirsiniz. Sunucuların [Azure 'a giden erişimi](automation-dsc-overview.md#network-planning)olmalıdır.

1. Azure Otomasyonu durum yapılandırması 'na eklemek için makinelerde [Linux Için Istenen durum yapılandırması](https://github.com/Microsoft/PowerShell-DSC-for-Linux) 'nın en son sürümünün yüklü olduğundan emin olun.
2. [POWERSHELL DSC yerel Configuration Manager varsayılan](/powershell/scripting/dsc/managing-nodes/metaConfig4) olarak kullanım durumumızdan eşleşiyorsa ve makineleri hem Azure Automation durum yapılandırmasına hem de raporlamalarıyla birlikte eklemek istiyorsanız:

   - Azure Otomasyonu durum yapılandırmasına eklenecek her bir Linux makinesinde, PowerShell DSC yerel `Register.py` Configuration Manager varsayılanlarını kullanarak eklemek için kullanın.

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Otomasyon hesabınızın kayıt anahtarını ve kayıt URL 'sini bulmak için, bu makalenin [kaydı kullanarak güvenli bir şekilde ekleme](#onboarding-securely-using-registration) bölümüne bakın.

3. PowerShell DSC yerel Configuration Manager (LCM) Varsayılanları kullanım durumumuzla eşleşmiyorsa veya yalnızca Azure Otomasyonu durum yapılandırmasına rapor veren makineler eklemek istiyorsanız, 4-7 adımlarını izleyin. Aksi takdirde, doğrudan 7. adıma geçin.

4. Gerekli DSC meta yapılandırmalarının bulunduğu bir klasör oluşturmak için [DSC Metaconfigurations oluşturma](#generating-dsc-metaconfigurations) bölümündeki yönergeleri izleyin.

5. Ekleme için kullanılan makinede [WMF 5](https://aka.ms/wmf5latest) ' in en son sürümünün yüklü olduğundan emin olun.

6. PowerShell DSC metaconfigurations 'ı eklemek istediğiniz makinelere uzaktan uygulamak için aşağıdaki şekilde kod ekleyin.

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

7. PowerShell DSC metaconfigurations 'ı uzaktan uygulayadıysanız, uzak makinelere karşılık gelen metaconfigurations 'ı 4. adım bölümünde açıklanan klasörden Linux makinelere kopyalayın.

8. Azure Otomasyonu durum yapılandırması `Set-DscLocalConfigurationManager.py` 'na eklemek için her bir Linux makinesinde yerel olarak çağrılacak kodu ekleyin.

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

9. Azure portal veya cmdlet 'lerini kullanarak, kullanıma hazır olan makinelerin Azure Automation hesabınızda kayıtlı olan DSC düğümleri olarak görüntülendiğinden emin olun.

## <a name="generating-dsc-metaconfigurations"></a>DSC metaconfigurations oluşturma

Herhangi bir makineyi Azure Otomasyonu durum yapılandırmasına eklemek için bir [DSC metayapılandırması](/powershell/scripting/dsc/managing-nodes/metaConfig)oluşturabilirsiniz. Bu yapılandırma DSC aracısının Azure Otomasyonu durum yapılandırması üzerinden ve/veya rapor vermesini söyler. PowerShell DSC yapılandırması ya da Azure Otomasyonu PowerShell cmdlet 'lerini kullanarak Azure Otomasyonu durum yapılandırması için DSC metaconfiguration oluşturabilirsiniz.

> [!NOTE]
> DSC metaconfigurations, yönetim için bir Otomasyon hesabına makine eklemek için gereken gizli dizileri içerir. Oluşturduğunuz DSC metayapılandırmalarının doğru şekilde korunmasını sağlayın veya kullandıktan sonra silin.

Metaconfigurations için proxy desteği, Windows PowerShell DSC altyapısı olan LCM tarafından denetlenir. LCM, tüm hedef düğümlerde çalışır ve bir DSC metaconfiguration betiğine dahil olan yapılandırma kaynaklarını çağırmadan sorumludur. Proxy URL tanımlarını ve proxy kimlik bilgisini `ConfigurationRepositoryWeb`, `ResourceRepositoryWeb`, ve `ReportServerWeb` bloklarında gerektiği şekilde ekleyerek bir metaconfiguration 'da proxy desteği ekleyebilirsiniz. Bkz. [yerel Configuration Manager yapılandırma](https://docs.microsoft.com/powershell/scripting/dsc/managing-nodes/metaconfig?view=powershell-7).

### <a name="generate-dsc-metaconfigurations-using-a-dsc-configuration"></a>DSC yapılandırması kullanarak DSC metayapılandırmaları oluşturma

1. VSCode (veya en sevdiğiniz düzenleyiciyi) yerel ortamınızdaki bir makinede yönetici olarak açın. Makinede [WMF 5](https://aka.ms/wmf5latest) ' in en son sürümü yüklü olmalıdır.
1. Aşağıdaki betiği yerel olarak kopyalayın. Bu betik meta yapılandırmalar oluşturmak için bir PowerShell DSC yapılandırması ve metaconfiguration oluşturmayı başlatma komutu içerir.

    > [!NOTE]
    > Durum yapılandırma düğümü yapılandırma adları Azure portal büyük/küçük harfe duyarlıdır. Büyük/küçük harf uyuşayarlanırsa düğüm **düğümler** sekmesinin altında gösterilmez.

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

1. Otomasyon hesabınızın kayıt anahtarını ve URL 'sini, eklenecek makinelerin adlarını da girin. Diğer tüm parametreler isteğe bağlıdır. Otomasyon hesabınızın kayıt anahtarını ve kayıt URL 'sini bulmak için, [kayıt kullanarak güvenle ekleme](#onboarding-securely-using-registration) bölümüne bakın.

1. Makinelerin DSC durum bilgilerini Azure Automation durum yapılandırmasına bildirmesini, ancak istek temelli yapılandırma veya PowerShell modülleri olmadığını istiyorsanız, `ReportOnly` parametresini true olarak ayarlayın.

1. `ReportOnly` Ayarlanmamışsa, makineler DSC durum bilgilerini Azure Otomasyonu durum yapılandırması ve çekme yapılandırması ya da PowerShell modüllerine bildirir. Parametreleri,, ve `ConfigurationRepositoryWeb` `ResourceRepositoryWeb` `ReportServerWeb` bloklarına göre ayarlayın.

1. Betiği çalıştırın. Artık, eklenecek makineler için PowerShell DSC metayapılandırmalarını içeren **Dscmetaconfigs**adlı bir çalışma dizini klasörünüz olmalıdır (yönetici olarak).

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="generate-dsc-metaconfigurations-using-azure-automation-cmdlets"></a>Azure Otomasyonu cmdlet 'lerini kullanarak DSC metaconfigurations oluşturma

PowerShell DSC LCM Varsayılanları kullanım durumumuzun eşleşiyorsa ve Azure Otomasyonu durum yapılandırmasına hem çekme hem de raporlama yapmak için makineler eklemek istiyorsanız, gerekli DSC metayapılandırmalarını Azure Otomasyonu cmdlet 'lerini kullanarak daha fazla oluşturabilirsiniz.

1. PowerShell konsolunu veya VSCode 'u yerel ortamınızdaki bir makinede yönetici olarak açın.
2. [Connect-AzAccount](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount?view=azps-3.7.0)kullanarak Azure Resource Manager bağlanın.
3. Düğümleri ayarladığınız Otomasyon hesabından eklemek istediğiniz makineler için PowerShell DSC metaconfigurations ' i indirin.

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

1. Artık, eklenecek makineler için PowerShell DSC metayapılandırmalarını içeren bir **Dscmetaconfigs** klasörünüz olmalıdır (yönetici olarak).

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="onboarding-securely-using-registration"></a>Kayıt kullanarak güvenli bir şekilde ekleme

Makineler, WMF 5 DSC kayıt protokolü aracılığıyla Azure Otomasyonu hesabına güvenli bir şekilde eklenebilir. Bu protokol, bir DSC düğümünün Azure Otomasyonu durum yapılandırması da dahil olmak üzere bir PowerShell DSC çekme veya rapor sunucusunda kimlik doğrulamasına olanak tanır. Düğüm, kayıt URL 'sindeki sunucuya kaydolur ve kayıt anahtarı kullanarak kimlik doğrular. Kayıt sırasında DSC düğümü ve DSC çekme/rapor sunucusu, düğüm için sunucu kayıt sonrası kimlik doğrulaması için kullanılacak benzersiz bir sertifika üzerinde anlaşır. Bu işlem, eklendi düğümlerinin bir diğerinin taklit etmesini engeller; Örneğin, bir düğüm tehlikeye girerse ve kötü amaçlı olarak davranmışsa. Kayıttan sonra, kayıt anahtarı kimlik doğrulaması için yeniden kullanılmaz ve düğümden silinir.

Azure portal durum yapılandırması kayıt Protokolü için gereken bilgileri, **Hesap ayarları** altındaki **anahtarlar** ' dan edinebilirsiniz. 

![Azure Otomasyonu anahtarları ve URL 'SI](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- Kayıt URL 'si, anahtarlar sayfasındaki URL alanıdır.
- Kayıt anahtarı, anahtarlar sayfasındaki **birincil erişim anahtarı** alanının veya **ikincil erişim anahtarı** alanının değeridir. Her iki anahtar de kullanılabilir.

Ek güvenlik için, bir Otomasyon hesabının birincil ve ikincil erişim anahtarlarını anahtarlar sayfasında dilediğiniz zaman yeniden oluşturabilirsiniz. Anahtar yeniden oluşturma, gelecekteki düğüm kayıtlarının önceki anahtarları kullanmasını önler.

## <a name="re-registering-a-node"></a>Bir düğümü yeniden kaydetme

Bir makineyi Azure Otomasyonu durum yapılandırması 'nda DSC düğümü olarak kaydettikten sonra, bu düğümü gelecekte yeniden kaydetmeniz gerekebileceği birkaç neden vardır.

- **Sertifika yenileme.** Windows Server 2019 ' den önceki Windows Server sürümleri için her düğüm, bir yıl sonra süresi dolan kimlik doğrulaması için otomatik olarak benzersiz bir sertifika olarak anlaşır. Bir sertifikanın süresi Yenilemesiz olarak dolarsa, düğüm Azure Otomasyonu ile iletişim kuramaz ve işaretlenir `Unresponsive`. Şu anda PowerShell DSC kayıt Protokolü, sertifikaları süresi dolmak üzere otomatik olarak yenileyemez ve bir yılın zamanından sonra düğümleri yeniden kaydetmeniz gerekir. Yeniden kaydolmadan önce, her bir düğümün WMF 5 RTM çalıştırdığından emin olun. 

    Yeniden kayıt, sertifika sona erme zamanından 90 gün veya daha az bir süre önce veya sertifika geçerlilik süresi dolduktan sonra herhangi bir noktada, oluşturulup kullanılmakta olan yeni bir sertifikayla sonuçlanır. Windows Server 2019 ve üzeri sürümlerde bu soruna yönelik bir çözüm bulunur.

- **DSC LCM değerlerinde yapılan değişiklikler.** Örneğin, `ConfigurationMode`düğümün ilk kaydı sırasında AYARLANMıŞ [PowerShell DSC LCM değerlerini](/powershell/scripting/dsc/managing-nodes/metaConfig4) değiştirmeniz gerekebilir. Şu anda, bu DSC Aracısı değerlerini yalnızca yeniden kayıt yoluyla değiştirebilirsiniz. Tek istisna, düğüme atanan düğüm yapılandırma değeridir. Bunu Azure Automation DSC doğrudan üzerinde değiştirebilirsiniz.

Bu belgede açıklanan ekleme yöntemlerinden herhangi birini kullanarak düğümü başlangıçta hemen kaydettiğiniz şekilde yeniden kaydedebilirsiniz. Bir düğümün yeniden kaydolmadan önce Azure Otomasyonu durum yapılandırması 'ndan kaydını kaldırmanız gerekmez.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Azure sanal makinesi ekleme sorunlarını giderme

Azure Otomasyonu durum yapılandırması, Azure Windows VM 'lerini yapılandırma yönetimi için kolayca eklemenizi sağlar. Azure VM 'nin Istenen durum yapılandırma uzantısı, Azure Otomasyonu durum yapılandırması ile VM 'yi kaydetmek için kullanılır. Azure VM Istenen durum yapılandırma uzantısı zaman uyumsuz olarak çalıştığından, ilerleme durumunu izlemek ve yürütme sorunlarını gidermek önemli olabilir.

> [!NOTE]
> Azure VM Istenen durum yapılandırması uzantısını kullanan Azure Otomasyonu durum yapılandırmasına Azure Windows VM 'yi ekleme yöntemi, Azure Otomasyonu 'nun kayıtlı olarak gösterilmesi için bir saate kadar sürebilir. Bu gecikme, VM 'nin Azure Otomasyonu durum yapılandırması 'na eklenmesi için gerekli olan Azure VM Istenen durum yapılandırma uzantısı tarafından, VM 'deki WMF 5 ' in yüklenmesinin kaynaklanmaktadır.

Azure VM Istenen durum yapılandırma uzantısının durumunu sorun gidermek veya görüntülemek için:

1. Azure portal, eklendi olan VM 'ye gidin.
2. **Ayarlar**altında **Uzantılar** ' a tıklayın. 
3. Şimdi işletim sisteminize bağlı olarak **DSC** veya **dscforlinux**' u seçin. 
4. Daha fazla ayrıntı için **ayrıntılı durumu görüntüle**' ye tıklayabilirsiniz.

Sorun giderme hakkında daha fazla bilgi için bkz. [Azure Otomasyonu Istenen durum yapılandırması (DSC) ile ilgili sorunları giderme](./troubleshoot/desired-state-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz. [Azure Otomasyonu durum yapılandırması ile çalışmaya](automation-dsc-getting-started.md)başlama.
- Hedef düğümlere atayabilmeniz için DSC yapılandırmalarını derleme hakkında bilgi edinmek için bkz. [Azure Otomasyonu durum yapılandırmasında yapılandırmaları derleme](automation-dsc-compile.md).
- PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
- Fiyatlandırma bilgileri için bkz. [Azure Otomasyonu durum yapılandırması fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).
- Azure Otomasyonu durum yapılandırması 'nı sürekli bir dağıtım ardışık düzeninde kullanmanın bir örneği için bkz. [kullanım örneği: Azure Otomasyonu durum yapılandırması ve Chocolatey kullanarak sanal makinelere sürekli dağıtım](automation-dsc-cd-chocolatey.md).
