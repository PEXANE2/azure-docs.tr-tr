---
title: Azure'da kimlik doğrulama için Azure Otomasyonu'nda Azure AD'yi kullanma
description: Azure'un kimlik doğrulaması sağlayıcısı olarak Azure Otomasyonu içinde Azure AD'yi nasıl kullanacağınızı öğrenin.
services: automation
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: 90338a1ffa79e6c2347832cb2e74633db02ec72d
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548334"
---
# <a name="use-azure-ad-in-azure-automation-to-authenticate-to-azure"></a>Azure'da kimlik doğrulama için Azure Otomasyonu'nda Azure AD'yi kullanma

[Azure Etkin Dizin (AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) hizmeti, kullanıcı yönetimi, etki alanı yönetimi ve tek oturum açma yapılandırması gibi bir dizi yönetim görevine olanak tanır. Bu makalede, Azure Otomasyonu'nda Azure AD'nin Azure kimlik doğrulaması sağlayıcısı olarak nasıl kullanılacağı açıklanmaktadır. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="installing-azure-ad-modules"></a>Azure AD modüllerini yükleme

Aşağıdaki PowerShell modülleri aracılığıyla Azure AD'yi etkinleştirebilirsiniz:

* Grafik için Azure Active Directory PowerShell (AzureRM ve Az modülleri). Azure Otomasyonu, AzureRM modülü ve en son yükseltmesi olan Az modülü ile birlikte iletin. İşlevsellik, Azure AD kullanıcısını (OrgId) kimlik bilgisi tabanlı kimlik doğrulaması kullanarak Azure'a etkileşimli olmayan kimlik doğrulaması içerir. Bkz. [Azure AD 2.0.2.76](https://www.powershellgallery.com/packages/AzureAD/2.0.2.76).

* Windows PowerShell (MSOnline modülü) için Microsoft Azure Etkin Dizin. Bu modül, Office 365 de dahil olmak üzere Microsoft Online ile etkileşimsağlar.

>[!NOTE]
>PowerShell Core, MSOnline modüllerini desteklemez. Modül cmdlets kullanmak için, Windows PowerShell bunları çalıştırmak gerekir. MSOnline modülü yerine Grafik modülleri için yeni Azure Active Directory PowerShell'i kullanmanız alabilirsiniz. 

### <a name="preinstallation"></a>Önyükleme

Azure AD modüllerini bilgisayarınıza yüklemeden önce:

* AzureRM/Az modülünün önceki sürümlerini ve MSOnline modüllerini kaldırın. 

* Yeni PowerShell modüllerinin doğru çalışmasını sağlamak için Microsoft Çevrimiçi Hizmetler Oturum Açma Yardımcısı'nı kaldırın.  

### <a name="install-the-azurerm-and-az-modules"></a>AzureRM ve Az modüllerini yükleme

>[!NOTE]
>Bu modüllerle çalışmak için PowerShell sürüm 5.1 veya daha sonra windows'un 64 bit sürümünü kullanmanız gerekir. 

1. Windows Yönetim Çerçevesi (WMF) 5.1'i yükleyin. Bkz. [WMF 5.1'i Yükle ve Yapılandır.](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure?view=powershell-7)

2. [PowerShellGet ile Windows'da Azure PowerShell'i](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.13.0)Yükleyin'deki yönergeleri kullanarak AzureRM ve/veya Az'ı yükleyin.

### <a name="install-the-msonline-module"></a>MSOnline modüllerini yükleyin

>[!NOTE]
>MSOnline modülünün yüklenmesi için Office 365 yönetici rolünün bir üyesi olmalısınız. Bkz. [Yönetici rolleri Hakkında](https://docs.microsoft.com/microsoft-365/admin/add-users/about-admin-roles?view=o365-worldwide).

1. Microsoft .NET Framework 3.5.x özelliğinin bilgisayarınızda etkin olduğundan emin olun. Bilgisayarınızın daha yeni bir sürümü yüklü olabilir, ancak .NET Framework'ün eski sürümleriyle geriye dönük uyumluluk etkinleştirilebilir veya devre dışı edilebilir. 

2. [Microsoft Çevrimiçi Hizmetler Oturum Açma Yardımcısı'nın](https://www.microsoft.com/download/details.aspx?id=41950)64 bit sürümünü yükleyin.

3. Yükseltilmiş bir Windows PowerShell komut istemi oluşturmak için Windows PowerShell'i yönetici olarak çalıştırın.

4. [MSOnline 1.0'dan](http://www.powershellgallery.com/packages/MSOnline/1.0)Azure Etkin Dizini dağıtın.

5. NuGet sağlayıcısını yüklemeniz istenirse Y yazın ve ENTER tuşuna basın.

6. [Modülü PSGallery'den](https://www.powershellgallery.com/)yüklemeniz istenirse Y yazın ve ENTER tuşuna basın.

### <a name="install-support-for-pscredential"></a>PSCredential için destek yükleyin

Azure Otomasyonu, bir kimlik kıymetini temsil etmek için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0) sınıfını kullanır. Komut dosyalarınız `PSCredential` `Get-AutomationPSCredential` cmdlet kullanarak nesneleri alır. Daha fazla bilgi için [Azure Otomasyonu'ndaki Kimlik Bilgileri varlıklarına](shared-resources/credentials.md)bakın.

## <a name="assigning-a-subscription-administrator"></a>Abonelik yöneticisi atama

Azure aboneliği için bir yönetici atamanız gerekir. Bu kişi abonelik kapsamı için Sahibi rolü vardır. [Azure Otomasyonu'nda Rol tabanlı erişim denetimine](automation-role-based-access-control.md)bakın. 

## <a name="changing-the-azure-ad-users-password"></a>Azure AD kullanıcının parolasını değiştirme

Azure AD kullanıcısının parolasını değiştirmek için:

1. Azure'dan çıkış yapın.

2. Tam kullanıcı adını (etki alanı dahil) ve geçici bir parola kullanarak, yöneticinin Azure'da azure'da oturum açmasını ve Azure AD kullanıcısının yeni oluşturulmasını sağlar. 

3. İstendiğinde yöneticiden parolayı değiştirmesini isteyin.

## <a name="configuring-azure-automation-to-use-the-azure-ad-user-to-manage-the-azure-subscription"></a>Azure aboneliğini yönetmek için Azure REKLAM kullanıcısını kullanacak şekilde Azure Otomasyonu yapılandırma

Azure Otomasyonu'nun Azure AD ile iletişim kurabilmesi için Azure REKLAMı ile ilişkili kimlik bilgilerini Azure AD'ye almanız gerekir. Bu kimlik bilgilerine örnek olarak kiracı kimliği, abonelik kimliği ve benzeri örnekler verilebilir. Azure ve Azure AD arasındaki bağlantı hakkında daha fazla şey için [bkz.](https://docs.microsoft.com/azure/devops/organizations/accounts/connect-organization-to-azure-ad?view=azure-devops)

## <a name="creating-a-credential-asset"></a>Kimlik bilgisi kıymeti oluşturma

Azure AD için Azure kimlik bilgileri kullanılabilirken, çalışma kitaplarının ve Desire State Configuration (DSC) komut dosyalarının bunlara erişebilmeleri için Azure AD kimlik bilgilerini güvenli bir şekilde depolamak için bir Azure Otomasyon kimlik bilgisi varlık oluşturmanın zamanı geldi. Bunu Azure portalını veya PowerShell cmdlets'i kullanarak yapabilirsiniz.

### <a name="create-the-credential-asset-in-azure-portal"></a>Azure portalında kimlik bilgisi kıymetini oluşturma

Kimlik bilgisi kıymetini oluşturmak için Azure portalını kullanabilirsiniz. Bu işlemi **Paylaşılan Kaynaklar**altında **Kimlik Bilgilerini** kullanarak Otomasyon hesabınızdan yapın. [Azure Otomasyonu'ndaki Kimlik Bilgileri varlıklarına](shared-resources/credentials.md)bakın.

### <a name="create-the-credential-asset-with-windows-powershell"></a>Windows PowerShell ile kimlik bilgisi kıymetini oluşturma

Windows PowerShell'de yeni bir kimlik bilgisi varlığı hazırlamak için, komut dosyanız önce atanan kullanıcı adı ve parolayı kullanarak bir `PSCredential` nesne oluşturur. Komut dosyası daha [sonra, Yeni AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) cmdlet'e yapılan bir çağrı yla varlığı oluşturmak için bu nesneyi kullanır. Alternatif olarak, komut dosyası, kullanıcıdan bir ad ve parola yazmaya teşvik etmek için [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) cmdlet'i arayabilir. [Azure Otomasyonu'ndaki Kimlik Bilgileri varlıklarına](shared-resources/credentials.md)bakın. 

## <a name="managing-azure-resources-from-an-azure-automation-runbook"></a>Azure kaynaklarını Azure Otomasyon uyrkitabından yönetme

Azure kaynaklarını Azure Otomasyon runbook'larından kimlik bilgileri kıymetini kullanarak yönetebilirsiniz. Aşağıda, bir Azure aboneliğinde sanal makineleri durdurmak ve başlatmak için kullanılacak kimlik bilgisi kıymetini toplayan örnek bir PowerShell runbook verilmiştir. Bu runbook `Get-AutomationPSCredential` ilk olarak Azure'da kimlik doğrulamak için kullanılacak kimlik bilgisini almak için kullanılır. Daha sonra kimlik belgesini kullanarak Azure'a bağlanmak için [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.6.1) cmdlet'i çağırır. Komut dosyası, çalışmak için aboneliği seçmek için [Select-AzureSubscription](https://docs.microsoft.com/powershell/module/servicemanagement/azure/select-azuresubscription?view=azuresmps-4.0.0) cmdlet'ini kullanır. 

```azurepowershell
Workflow Stop-Start-AzureVM 
{ 
    Param 
    (    
        [Parameter(Mandatory=$true)][ValidateNotNullOrEmpty()] 
        [String] 
        $AzureSubscriptionId, 
        [Parameter(Mandatory=$true)][ValidateNotNullOrEmpty()] 
        [String] 
        $AzureVMList="All", 
        [Parameter(Mandatory=$true)][ValidateSet("Start","Stop")] 
        [String] 
        $Action 
    ) 
     
    $credential = Get-AutomationPSCredential -Name 'AzureCredential' 
    Connect-AzAccount -Credential $credential 
    Select-AzureSubscription -SubscriptionId $AzureSubscriptionId 
 
    if($AzureVMList -ne "All") 
    { 
        $AzureVMs = $AzureVMList.Split(",") 
        [System.Collections.ArrayList]$AzureVMsToHandle = $AzureVMs 
    } 
    else 
    { 
        $AzureVMs = (Get-AzVM).Name 
        [System.Collections.ArrayList]$AzureVMsToHandle = $AzureVMs 
 
    } 
 
    foreach($AzureVM in $AzureVMsToHandle) 
    { 
        if(!(Get-AzVM | ? {$_.Name -eq $AzureVM})) 
        { 
            throw " AzureVM : [$AzureVM] - Does not exist! - Check your inputs " 
        } 
    } 
 
    if($Action -eq "Stop") 
    { 
        Write-Output "Stopping VMs"; 
        foreach -parallel ($AzureVM in $AzureVMsToHandle) 
        { 
            Get-AzVM | ? {$_.Name -eq $AzureVM} | Stop-AzVM -Force 
        } 
    } 
    else 
    { 
        Write-Output "Starting VMs"; 
        foreach -parallel ($AzureVM in $AzureVMsToHandle) 
        { 
            Get-AzVM | ? {$_.Name -eq $AzureVM} | Start-AzVM 
        } 
    } 
}
```  

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu'nda [Kimlik Bilgileri varlıklarında](shared-resources/credentials.md)Otomasyon kimlik bilgileri hakkında bilgi bulabilirsiniz.
* Otomasyon modülleriyle nasıl çalışacağımızı öğrenmek için [Azure Otomasyonu'ndaki modülleri yönet'e](shared-resources/modules.md) bakın.
* Azure Otomasyonu'nda bir runbook başlatmak için kullanılabilecek yöntemler hakkında daha fazla bilgi edinmek için azure [otomasyonunda runbook başlatma](automation-starting-a-runbook.md)'ya bakın.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.