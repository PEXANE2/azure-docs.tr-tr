---
title: Azure Otomasyonu’nda Azure AD kullanarak Azure’a kimlik doğrulaması yapma
description: Azure 'da kimlik doğrulaması için sağlayıcı olarak Azure Otomasyonu 'nda Azure AD 'yi nasıl kullanacağınızı öğrenin.
services: automation
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: 90338a1ffa79e6c2347832cb2e74633db02ec72d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80548334"
---
# <a name="use-azure-ad-in-azure-automation-to-authenticate-to-azure"></a>Azure Otomasyonu’nda Azure AD kullanarak Azure’a kimlik doğrulaması yapma

[Azure Active Directory (ad)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) hizmeti, Kullanıcı yönetimi, etki alanı yönetimi ve çoklu oturum açma yapılandırması gibi çeşitli yönetim görevlerini mümkün bir şekilde sunar. Bu makalede, Azure 'da kimlik doğrulaması için sağlayıcı olarak Azure Otomasyonu 'nda Azure AD 'nin nasıl kullanılacağı açıklanır. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="installing-azure-ad-modules"></a>Azure AD modüllerini yükleme

Azure AD 'yi aşağıdaki PowerShell modülleri aracılığıyla etkinleştirebilirsiniz:

* Graph için Azure Active Directory PowerShell (Azurerd ve az modüller). Azure Otomasyonu Azurerd modülü ve son yükseltmesi olan az Module ile birlikte gönderilir. İşlevsellik, Azure AD Kullanıcı (OrgID) kimlik bilgileri tabanlı kimlik doğrulaması kullanılarak Azure 'a etkileşimli olmayan kimlik doğrulaması içerir. Bkz. [Azure AD 2.0.2.76](https://www.powershellgallery.com/packages/AzureAD/2.0.2.76).

* Windows PowerShell için Microsoft Azure Active Directory (MSOnline modülü). Bu modül, Office 365 dahil olmak üzere Microsoft Online ile etkileşim imkanı sunar.

>[!NOTE]
>PowerShell Core, MSOnline modülünü desteklemez. Modül cmdlet 'lerini kullanmak için Windows PowerShell 'den çalıştırmanız gerekir. MSOnline modülü yerine Graph modülleri için daha yeni Azure Active Directory PowerShell 'i kullanmanız önerilir. 

### <a name="preinstallation"></a>Önkurulum

Azure AD modüllerini bilgisayarınıza yüklemeden önce:

* Azurerd/az modülünün önceki sürümlerini ve MSOnline modülünü kaldırın. 

* Yeni PowerShell modüllerinin doğru çalışmasını sağlamak için Microsoft Online Services oturum açma Yardımcısı 'Nı kaldırın.  

### <a name="install-the-azurerm-and-az-modules"></a>Azurerd ve az modüllerini yükler

>[!NOTE]
>Bu modüllerle çalışmak için, Windows 'un 64 bit sürümüyle birlikte PowerShell sürüm 5,1 veya sonraki bir sürümünü kullanmanız gerekir. 

1. Windows Management Framework (WMF) 5,1 ' ü yükler. Bkz. [WMF 5,1 'Yi yükleyip yapılandırma](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure?view=powershell-7).

2. [PowerShellGet Ile Windows üzerinde Azure PowerShell Install](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.13.0)' daki yönergeleri kullanarak Azurerd ve/veya az ' yı yükledikten sonra.

### <a name="install-the-msonline-module"></a>MSOnline modülünü yükler

>[!NOTE]
>MSOnline modülünü yüklemek için Office 365 Yönetici rolünün bir üyesi olmanız gerekir. Bkz. [yönetici rolleri hakkında](https://docs.microsoft.com/microsoft-365/admin/add-users/about-admin-roles?view=o365-worldwide).

1. Bilgisayarınızda Microsoft .NET Framework 3.5. x özelliğinin etkinleştirildiğinden emin olun. Bilgisayarınızda daha yeni bir sürüm yüklü olabilir, ancak .NET Framework eski sürümleriyle geriye dönük uyumluluk etkinleştirilebilir veya devre dışı bırakılabilir. 

2. [Microsoft Online Services oturum açma Yardımcısı](https://www.microsoft.com/download/details.aspx?id=41950)'nın 64 bitlik sürümünü yükler.

3. Yükseltilmiş bir Windows PowerShell komut istemi oluşturmak için Windows PowerShell 'i yönetici olarak çalıştırın.

4. Azure Active Directory [MSOnline 1,0](http://www.powershellgallery.com/packages/MSOnline/1.0)' dan dağıtın.

5. NuGet sağlayıcısını yüklemek isteyip istemediğiniz sorulursa, Y yazın ve ENTER tuşuna basın.

6. Bu modülü [Psgallery](https://www.powershellgallery.com/)'den yüklemek isteyip istemediğiniz sorulursa, Y YAZıN ve ENTER tuşuna basın.

### <a name="install-support-for-pscredential"></a>PSCredential için destek yüklemesi

Azure Otomasyonu, bir kimlik bilgisi varlığını temsil etmek için [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0) sınıfını kullanır. Betikleriniz `Get-AutomationPSCredential` cmdlet `PSCredential` 'ini kullanarak nesneleri alır. Daha fazla bilgi için bkz. [Azure Automation 'Da kimlik bilgisi varlıkları](shared-resources/credentials.md).

## <a name="assigning-a-subscription-administrator"></a>Abonelik Yöneticisi atama

Azure aboneliği için bir yönetici atamanız gerekir. Bu kişi, abonelik kapsamı için sahip rolüne sahiptir. Bkz. [Azure Otomasyonu 'Nda rol tabanlı erişim denetimi](automation-role-based-access-control.md). 

## <a name="changing-the-azure-ad-users-password"></a>Azure AD kullanıcısının parolasını değiştirme

Azure AD kullanıcısının parolasını değiştirmek için:

1. Azure oturumunu kapatma.

2. Yöneticinin, tam Kullanıcı adı (etki alanı dahil) ve geçici bir parola kullanarak Azure 'da yeni oluşturulan Azure AD kullanıcısı olarak oturum açmasını sağlayabilirsiniz. 

3. İstendiğinde, yöneticiden parolayı değiştirmesini isteyin.

## <a name="configuring-azure-automation-to-use-the-azure-ad-user-to-manage-the-azure-subscription"></a>Azure Otomasyonu 'nu Azure AD kullanıcısını kullanarak Azure aboneliğini yönetmek için yapılandırma

Azure Otomasyonu 'nun Azure AD ile iletişim kurması için Azure AD 'ye Azure bağlantısıyla ilişkili kimlik bilgilerini almalısınız. Bu kimlik bilgilerine örnek olarak kiracı KIMLIĞI, abonelik KIMLIĞI ve beğen verilebilir. Azure ile Azure AD arasındaki bağlantı hakkında daha fazla bilgi için bkz. [kuruluşunuzu Azure Active Directory bağlama](https://docs.microsoft.com/azure/devops/organizations/accounts/connect-organization-to-azure-ad?view=azure-devops).

## <a name="creating-a-credential-asset"></a>Kimlik bilgisi varlığı oluşturma

Azure AD 'nin Azure kimlik bilgilerini kullanılabilir hale, runbook 'lar ve gereken durum yapılandırması (DSC) betiklerinin bunlara erişebilmesi için Azure AD kimlik bilgilerini güvenli bir şekilde depolamak üzere bir Azure Otomasyonu kimlik bilgisi varlığı oluşturma zamanı. Bunu, Azure portal ya da PowerShell cmdlet 'lerini kullanarak yapabilirsiniz.

### <a name="create-the-credential-asset-in-azure-portal"></a>Azure portal 'da kimlik bilgisi varlığı oluşturma

Kimlik bilgisi varlığını oluşturmak için Azure portal kullanabilirsiniz. Bu işlemi, **paylaşılan kaynaklar**altındaki **kimlik bilgilerini** kullanarak Otomasyon hesabınızdan yapın. Bkz. [Azure Automation 'Da kimlik bilgisi varlıkları](shared-resources/credentials.md).

### <a name="create-the-credential-asset-with-windows-powershell"></a>Windows PowerShell ile kimlik bilgisi varlığı oluşturma

Windows PowerShell 'de yeni bir kimlik bilgisi varlığı hazırlamak için, komut dosyası önce atanan `PSCredential` Kullanıcı adı ve parolasını kullanarak bir nesne oluşturur. Betik daha sonra bu nesneyi [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) cmdlet 'i çağrısıyla varlık oluşturmak için kullanır. Alternatif olarak, komut dosyası [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) cmdlet 'ini çağırabilir ve kullanıcıdan bir ad ve parola yazıp yazmayabilirler. Bkz. [Azure Automation 'Da kimlik bilgisi varlıkları](shared-resources/credentials.md). 

## <a name="managing-azure-resources-from-an-azure-automation-runbook"></a>Azure Otomasyonu runbook 'tan Azure kaynaklarını yönetme

Azure kaynaklarını Azure Otomasyonu runbook 'lardan, kimlik bilgisi varlığını kullanarak yönetebilirsiniz. Aşağıda, bir Azure aboneliğindeki sanal makineleri durdurmak ve başlatmak için kullanılacak kimlik bilgisi varlığını toplayan örnek bir PowerShell runbook 'u verilmiştir. Bu runbook ilk olarak `Get-AutomationPSCredential` Azure 'da kimlik doğrulaması yapmak için kullanılacak kimlik bilgilerini almak üzere kullanır. Ardından, kimlik bilgisini kullanarak Azure 'a bağlanmak için [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.6.1) cmdlet 'ini çağırır. Betiği, birlikte çalışmak üzere bir abonelik seçmek için [Select-azuyeniden göndermeyi Scription](https://docs.microsoft.com/powershell/module/servicemanagement/azure/select-azuresubscription?view=azuresmps-4.0.0) cmdlet 'ini kullanır. 

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

* [Azure Automation 'Da kimlik bilgisi varlıkları](shared-resources/credentials.md)' nda Otomasyon kimlik bilgileri varlıkları hakkında bilgi bulabilirsiniz.
* Otomasyon modülleriyle nasıl çalışabileceğinizi öğrenmek için bkz. [Azure Otomasyonu 'nda modülleri yönetme](shared-resources/modules.md) .
* Azure Otomasyonu 'nda bir runbook başlatmak için kullanılabilecek yöntemler hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu 'nda runbook başlatma](automation-starting-a-runbook.md).
* Dil başvurusu ve öğrenme modülleri dahil olmak üzere PowerShell hakkında daha fazla bilgi için bkz. [PowerShell belgeleri](https://docs.microsoft.com/powershell/scripting/overview).