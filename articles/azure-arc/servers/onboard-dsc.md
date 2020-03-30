---
title: Windows PowerShell DSC kullanarak Bağlı Makine aracısını yükleme
description: Bu makalede, Windows PowerShell DSC kullanarak sunucular için Azure Arc kullanarak (önizleme) kullanarak makineleri Azure'a nasıl bağacağınızı öğreneceksiniz.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 03/12/2020
ms.topic: conceptual
ms.openlocfilehash: 1fb64463b0372202adb04c2deb304c389c7773b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79164689"
---
# <a name="how-to-install-the-connected-machine-agent-using-windows-powershell-dsc"></a>Windows PowerShell DSC kullanarak Bağlı Makine aracısı nasıl yüklenir?

[Windows PowerShell İstenen Durum Yapılandırmasını](https://docs.microsoft.com/powershell/scripting/dsc/getting-started/winGettingStarted?view=powershell-7) (DSC) kullanarak, bir Windows bilgisayarı için yazılım yüklemeve yapılandırmayı otomatikleştirebilirsiniz. Bu makalede, karma Windows makinelerinde Bağlı Makine aracısı sunucular için Azure Arc'ı yüklemek için DSC'nin nasıl kullanılacağı açıklanmaktadır.

## <a name="requirements"></a>Gereksinimler

- Windows PowerShell sürüm 4.0 veya üstü

- [AzureConnectedMachineDsc](https://www.powershellgallery.com/packages/AzureConnectedMachineDsc/1.0.1.0) DSC modülü

- Etkileşimli olmayan sunucular için makineleri Azure Arc'a bağlamak için bir hizmet ilkesi. Zaten sunucular için Arc için bir hizmet ilkesi oluşturmadıysanız, [ölçekte biniş için bir Hizmet Sorumlusu oluştur](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) bölümü altındaki adımları izleyin.

## <a name="install-the-connectedmachine-dsc-module"></a>ConnectedMachine DSC modüllerini kurun

1. Modülü el ile yüklemek için kaynak kodu indirin ve proje dizininin `$env:ProgramFiles\WindowsPowerShell\Modules folder`içeriğini . Veya PowerShellGet'ı kullanarak PowerShell galerisinden yüklemek için aşağıdaki komutu çalıştırın (PowerShell 5.0'da):

    ```powershell
    Find-Module -Name AzureConnectedMachineDsc -Repository PSGallery | Install-Module
    ```

2. Yüklemeyi onaylamak için aşağıdaki komutu çalıştırın ve kullanılabilir Azure Bağlı Makine DSC kaynaklarını gördüğünüzden emin olun.

    ```powershell
    Get-DscResource -Module AzureConnectedMachineDsc
    ```

   Çıktıda, aşağıdakilere benzer bir şey görmeniz gerekir:

   ![Bağlı Makine DSC modül kurulum örneği onayı](./media/onboard-dsc/confirm-module-installation.png)

## <a name="install-the-agent-and-connect-to-azure"></a>Aracıyı yükleyin ve Azure'a bağlanın

Bu modüldeki kaynaklar Azure Bağlı Makine Aracısı yapılandırmasını yönetmek için tasarlanmıştır. Ayrıca klasörde `AzureConnectedMachineDsc\examples` bulunan bir `AzureConnectedMachineAgent.ps1`PowerShell komut dosyası dahildir. İndirme ve yüklemeyi otomatikleştirmek ve Azure Arc ile bağlantı kurmak için topluluk kaynaklarını kullanır. Bu komut dosyası, [Azure portalı makalesinden Azure'a bağlayın karma makinelerde](onboard-portal.md) açıklanan benzer adımları gerçekleştirir.

Makinenin bir proxy sunucusu üzerinden hizmete iletişim kurması gerekiyorsa, aracıyı yükledikten sonra [burada](onboard-portal.md#configure-the-agent-proxy-setting)açıklanan bir komutu çalıştırmanız gerekir. Bu proxy sunucu sistemi `https_proxy`ortamı değişkenayarlar. Komutu el ile çalıştırmak yerine, [ComputeManagementDsc](https://www.powershellgallery.com/packages/ComputerManagementDsc/6.0.0.0) modüllerini kullanarak DSC ile bu adımı gerçekleştirebilirsiniz.

>[!NOTE]
>DSC'nin çalışmasına izin vermek için, Windows'un yerel barındırma yapılandırması çalıştırırken bile PowerShell uzaktan kumandalarını alacak şekilde yapılandırılması gerekir. Ortamınızı kolayca doğru şekilde yapılandırmak `Set-WsManQuickConfig -Force` için yükseltilmiş bir PowerShell Terminalinde çalışmanız gerekiyor.
>

Yapılandırma belgeleri (MOF dosyaları) `Start-DscConfiguration` cmdlet kullanılarak makineye uygulanabilir.

Kullanmak üzere PowerShell komut dosyasına geçtiğiniz parametreler aşağıda verilmiştir.

- `TenantId`: Azure AD özel örneğini temsil eden benzersiz tanımlayıcı (GUID).

- `SubscriptionId`: Makinelerin içinde olmasını istediğiniz Azure aboneliğinizin abonelik kimliği (GUID).

- `ResourceGroup`: Bağlı makinelerinizin ait olmasını istediğiniz kaynak grubu adı.

- `Location`: [Desteklenen Azure bölgelerine](overview.md#supported-regions)bakın. Bu konum, kaynak grubunun konumuyla aynı veya farklı olabilir.

- `Tags`: Bağlı makine kaynağına uygulanması gereken etiket dizisi dizisi.

- `Credential`: Service Principal kullanarak makineleri ölçekte kaydetmek için kullanılan **ApplicationId** ve **şifreiçeren** PowerShell kimlik [nesnesi.](onboard-service-principal.md) 

1. PowerShell konsolunda, `.ps1` dosyayı kaydettiğiniz klasöre gidin.

2. MOF belgesini derlemek için aşağıdaki PowerShell komutlarını çalıştırın (DSC yapılandırmalarını derleme hakkında bilgi için bkz: [DSC Yapılandırmaları:](https://docs.microsoft.com/powershell/scripting/dsc/configurations/configurations?view=powershell-7)

    ```powershell
    .\`AzureConnectedMachineAgent.ps1 -TenantId <TenantId GUID> -SubscriptionId <SubscriptionId GUID> -ResourceGroup '<ResourceGroupName>' -Location '<LocationName>' -Tags '<Tag>' -Credential <psCredential>
    ```

3. Bu, adlı `localhost.mof file` `C:\dsc`yeni bir klasörde bir oluşturur.

Aracıyı yükledikten ve sunucular için Azure Arc'a bağlanacak şekilde yapılandırdıktan sonra (önizleme), sunucunun başarıyla bağlandığını doğrulamak için Azure portalına gidin. Makinelerinizi [Azure portalında](https://aka.ms/hybridmachineportal)görüntüleyin.

## <a name="adding-to-existing-configurations"></a>Varolan yapılandırmalara ekleme

Bu kaynak, bir makine için uçlardan uca yapılandırmayı temsil etmek üzere varolan DSC yapılandırmalarına eklenebilir. Örneğin, bu kaynağı güvenli işletim sistemi ayarlarını ayarlayan bir yapılandırmaya eklemek isteyebilirsiniz.

PowerShell Gallery'nin [CompsiteResource](https://www.powershellgallery.com/packages/compositeresource/0.4.0) modülü, yapılandırmaları birleştirmeyi daha da basitleştirmek için örnek yapılandırmanın [bileşik bir kaynağını](https://docs.microsoft.com/powershell/scripting/dsc/resources/authoringResourceComposite?view=powershell-7) oluşturmak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi'ni](../../governance/policy/overview.md)kullanarak makinenizi nasıl yöneteceğimizi öğrenin , VM [konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md)gibi şeyler için, makinenin beklenen Log Analytics çalışma alanına rapor ettiğini doğrulamak, [VM'lerle Azure Monitor](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md)ile izlemeyi etkinleştirmek ve çok daha fazlası.

- [Log Analytics aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Makinede çalışan işletim sistemlerini ve iş yüklerini proaktif olarak izlemek, Otomasyon runbook'larını veya Update Management gibi çözümleri kullanarak yönetmek veya [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde Windows ve Linux için Log Analytics aracısı gereklidir.