---
title: Azure AD Uygulaması önkoşulları ile Azure disk şifrelemesi (önceki sürüm)
description: Bu makalede, IaaS VM 'Leri için Microsoft Azure disk şifrelemesi kullanma önkoşulları sağlanır.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 09/30/2019
ms.custom: seodec18
ms.openlocfilehash: 07db6c5cecf2b8dec803e73d5775edef3f8906fa
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71828625"
---
# <a name="azure-disk-encryption-prerequisites-previous-release"></a>Azure disk şifrelemesi önkoşulları (önceki sürüm)

**Azure disk şifrelemesi 'nin yeni sürümü, VM disk şifrelemeyi etkinleştirmek için bir Azure AD uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümde, artık şifrelemeyi Etkinleştir adımını kullanarak Azure AD kimlik bilgilerini sağlamanız gerekmez. Yeni sürüm kullanılarak Azure AD uygulama parametreleri olmadan tüm yeni VM 'Ler şifrelenmelidir. Yeni sürümü kullanarak VM disk şifrelemesini etkinleştirme yönergelerini görüntülemek için bkz. [Azure disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md). Azure AD uygulama parametreleriyle zaten şifrelenmiş VM 'Ler hala desteklenmektedir ve AAD sözdizimi ile sürdürülmeye devam etmelidir.**

 Bu makalede, Azure disk şifrelemesi önkoşulları, Azure disk şifrelemesi 'ni kullanabilmeniz için önce yerinde olması gereken öğeleri açıklar. Azure disk şifrelemesi, genel önkoşulların yanı sıra [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) ile tümleşiktir ve anahtar kasasındaki şifreleme anahtarlarını yönetmek için kimlik doğrulaması sağlamak üzere BIR Azure AD uygulaması kullanır. Ayrıca, Key Vault ve Azure AD uygulaması kurmak veya yapılandırmak için [Azure PowerShell](/powershell/azure/overview) ya da [Azure CLI](/cli/azure/) 'yi kullanmak isteyebilirsiniz.

Azure [disk şifrelemesi genel bakış](azure-security-disk-encryption-overview.md) makalesinde açıklanan desteklenen senaryolar Için Azure IaaS VM 'Lerinde Azure disk şifrelemesini etkinleştirmeden önce önkoşulların yerinde olduğundan emin olun. 

> [!WARNING]
> - Bazı öneriler veri, ağ veya işlem kaynak kullanımını artırabilir, bu da ek lisans veya abonelik maliyetlerine neden olur. Desteklenen bölgelerde Azure 'da kaynak oluşturmak için geçerli bir etkin Azure aboneliğiniz olması gerekir.
> - Bu VM 'yi şifrelemek için Azure [ad uygulaması Ile Azure disk şifrelemesi](azure-security-disk-encryption-prerequisites-aad.md) 'ni daha önce KULLANDıYSANıZ, VM 'nizi şifrelemek için bu seçeneği kullanmaya devam etmeniz gerekir. Bu desteklenen bir senaryo olmadığından, bu şifrelenmiş VM 'de [Azure disk şifrelemesi](azure-security-disk-encryption-prerequisites.md) 'ni kullanamazsınız. Bu, bu şifrelenmiş VM için AAD uygulamasından uzağa geçiş henüz desteklenmiyor. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-operating-systems"></a>Supported operating systems

### <a name="windows"></a>Windows

- Windows istemcisi: Windows 8 ve üzeri.
- Windows Server: Windows Server 2008 R2 ve üzeri.  
 
> [!NOTE] 
> Windows Server 2008 R2, şifreleme için .NET Framework 4,5 ' nin yüklenmesini gerektirir; Windows Server 2008 R2 x64 tabanlı sistemler için isteğe bağlı güncelleştirme Microsoft .NET Framework 4.5.2 ([KB2901983](https://www.catalog.update.microsoft.com/Search.aspx?q=KB2901983)) ile Windows Update yüklersiniz.  
>  
> Windows Server 2012 R2 Core ve Windows Server 2016 Core, şifreleme için sanal makinede BdeHdCfg bileşeninin yüklü olmasını gerektirir.

### <a name="linux"></a>Linux 

Azure disk şifrelemesi, [Azure tarafından onaylanan Linux dağıtımların](../virtual-machines/linux/endorsed-distros.md)bir alt kümesinde desteklenir ve bu, tüm Linux Server olası dağıtımların bir alt kümesidir.

![Azure disk şifrelemesini destekleyen Linux sunucu dağıtımlarından oluşan Venn diyagramı](./media/azure-security-disk-encryption-faq/ade-supported-distros.png)

Azure tarafından onaylanan Linux sunucu dağıtımları, Azure disk şifrelemesini desteklemez ve onaylama işlemleri için yalnızca aşağıdaki dağıtımlar ve sürümler Azure disk şifrelemesini destekler:

| Linux dağıtımı | Version | Şifreleme için desteklenen birim türü|
| --- | --- |--- |
| Ubuntu | 18,04| İşletim sistemi ve veri diski |
| Ubuntu | 16,04| İşletim sistemi ve veri diski |
| Ubuntu | 14.04.5</br>[Azure 'da ayarlanmış çekirdek, 4,15 veya üzeri bir sürüme güncelleştirildi](azure-security-disk-encryption-tsg.md#bkmk_Ubuntu14) | İşletim sistemi ve veri diski |
| RHEL for | 7,7 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL for | 7,6 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL for | 7,5 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL for | 7,4 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL for | 7,3 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL for | 7,2 | İşletim sistemi ve veri diski (aşağıdaki nota bakın) |
| RHEL for | 6,8 | Veri diski (aşağıdaki nota bakın) |
| RHEL for | 6,7 | Veri diski (aşağıdaki nota bakın) |
| CentOS | 7,7 | İşletim sistemi ve veri diski |
| CentOS | 7,6 | İşletim sistemi ve veri diski |
| CentOS | 7,5 | İşletim sistemi ve veri diski |
| CentOS | 7,4 | İşletim sistemi ve veri diski |
| CentOS | 7,3 | İşletim sistemi ve veri diski |
| CentOS | 7.2 n | İşletim sistemi ve veri diski |
| CentOS | 6,8 | Veri diski |
| openSUSE | 42,3 | Veri diski |
| SLES | 12-SP4 | Veri diski |
| SLES | 12-SP3 | Veri diski |

> [!NOTE]
> Yeni ADE uygulama, RHEL7 Kullandıkça Öde görüntüleri için RHEL OS ve veri diski için desteklenir. ADE Şu anda RHEL kendi abonelik (KCG) görüntüleri için desteklenmiyor. Daha fazla bilgi için bkz. [Linux Için Azure disk şifrelemesi](azure-security-disk-encryption-linux.md) .

- Azure disk şifrelemesi, anahtar kasalarınızın ve sanal makinelerinizin aynı Azure bölgesinde ve abonelikte bulunmasını gerektirir. Kaynakları ayrı bölgelerde yapılandırmak, Azure disk şifrelemesi özelliğinin etkinleştirilmesinde hata oluşmasına neden olur.

#### <a name="additional-prerequisites-for-linux-iaas-vms"></a>Linux IaaS VM 'Leri için ek önkoşullar 

- Azure disk şifrelemesi, sistemde dm-crypt ve VFAT modüllerinin bulunmasını gerektirir. VFAT 'i varsayılan görüntüden kaldırmak veya devre dışı bırakmak, sistemin anahtar birimini okumasını ve sonraki yeniden başlatmalarda disklerin kilidini açmak için gereken anahtarı almasını engeller. VFAT modülünü sistemden kaldırmak için sistem sağlamlaştırma adımları Azure disk şifrelemesi ile uyumlu değildir. 
- Şifrelemeyi etkinleştirmeden önce, şifrelenecek veri disklerinin/etc/fstabin içinde düzgün listelenmesi gerekir. "/Dev/sdX" biçimindeki cihaz adları, özellikle şifreleme uygulandıktan sonra, yeniden başlatmalar genelinde aynı diskle ilişkilendirilemediğinden, bu giriş için kalıcı bir blok cihaz adı kullanın. Bu davranış hakkında daha fazla bilgi için bkz. [LINUX VM cihaz adı değişikliklerinde sorun giderme](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- /Etc/fstab ayarlarının bağlama için doğru yapılandırıldığından emin olun. Bu ayarları yapılandırmak için, Mount-a komutunu çalıştırın veya VM 'yi yeniden başlatın ve bu şekilde uzak yeniden bağlama 'yı tetikleyin. Bu tamamlandıktan sonra, sürücünün hala bağlı olduğunu doğrulamak için lsblk komutunun çıkışını kontrol edin. 
  - /Etc/fstab dosyası Şifrelemeyi etkinleştirmeden önce sürücüyü doğru bağmazsa, Azure disk şifrelemesi onu düzgün bir şekilde bağlanamaz.
  - Azure Disk Şifrelemesi işlemi, bağlama bilgilerini şifreleme sürecinin bir parçası olarak/etc/fstab ' dan ve kendi yapılandırma dosyasına taşıyacaktır. Veri sürücüsü şifrelemesi tamamlandıktan sonra/etc/fstab içinde eksik girişi görmek için uyarıda yok.
  - Şifrelemeyi başlatmadan önce, bağlı veri disklerine yazmak ve devre dışı bırakmak için yeniden başlatmadan sonra otomatik olarak yeniden başlatabilmeleri için tüm hizmetleri ve süreçlerini durdurmayı unutmayın. Bunlar, dosyaları bu bölümlerde açık tutabilir, böylece şifreleme yordamının yeniden bağlanmasını önler ve bu da şifrelemenin başarısız olmasına neden olur. 
  - Yeniden başlatmadan sonra, Azure disk şifrelemesi işleminin yeni şifrelenmiş diskleri bağlaması zaman alır. Yeniden başlatmadan sonra hemen kullanılamayacak. İşlem, diğer işlemlerin erişebilmesi için önce şifrelenmiş sürücüleri açmak, kilitlerini açmak ve ardından bağlamak için zaman gerektirir. Bu işlem, sistem özelliklerine bağlı olarak yeniden başlatma sonrasında bir dakikadan uzun sürebilir.

Veri disklerini bağlamak ve gerekli/etc/fstab girdilerini oluşturmak için kullanılabilen komutların bir örneği, [Bu betik dosyasının 244-248. satırlarında](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L244-L248)bulunabilir. 

## <a name="bkmk_GPO"></a>Ağ ve grup ilkesi

**Eski AAD parametresi söz dizimini kullanarak Azure Disk Şifrelemesi özelliğini etkinleştirmek için IaaS VM 'lerinin aşağıdaki ağ uç noktası yapılandırma gereksinimlerini karşılaması gerekir:** 
  - Anahtar kasanıza bağlanmak üzere bir belirteç almak için, IaaS VM 'nin bir Azure Active Directory uç noktasına bağlanabilmesi gerekir @no__t -0login. microsoftonline. com @ no__t-1.
  - Şifreleme anahtarlarını anahtar kasanıza yazmak için, IaaS VM 'sinin Anahtar Kasası uç noktasına bağlanabilmesi gerekir.
  - IaaS sanal makinesi, Azure uzantı deposunu barındıran bir Azure depolama uç noktasına ve VHD dosyalarını barındıran bir Azure depolama hesabına bağlanabilmelidir.
  -  Güvenlik ilkeniz, Azure VM 'lerinden Internet 'e erişimi sınırlayıp, önceki URI 'yi çözümleyebilir ve IP 'lere giden bağlantılara izin vermek için belirli bir kuralı yapılandırabilirsin. Daha fazla bilgi için bkz. [Azure Key Vault bir güvenlik duvarı arkasında](../key-vault/key-vault-access-behind-firewall.md).
  - Windows 'da, TLS 1,0 açıkça devre dışıysa ve .NET sürümü 4,6 veya üzeri bir sürüme güncelleştirilmemiş ise, aşağıdaki kayıt defteri değişikliği ADE 'nin daha son TLS sürümünü seçmesini sağlayacak:
    
        [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001

        [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001` 
     

 


**Grup ilkesi:**
 - Azure disk şifrelemesi çözümü, Windows IaaS VM 'Leri için BitLocker dış anahtar koruyucusunu kullanır. Etki alanına katılmış VM 'Ler için TPM koruyucuları uygulayan herhangi bir grup ilkesi göndermeyin. "Uyumlu TPM olmadan BitLocker 'a Izin ver" Grup ilkesi hakkında bilgi için bkz. [bitlocker Grup İlkesi başvurusu](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#bkmk-unlockpol1).

-  Özel grup ilkesiyle etki alanına katılmış sanal makinelerde BitLocker ilkesi şu ayarı içermelidir: [BitLocker kurtarma bilgileri 'nin Kullanıcı depolamasını yapılandırma-> Izin ver 256-bit kurtarma anahtarı](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). BitLocker için özel Grup İlkesi ayarları uyumsuz olduğunda Azure disk şifrelemesi başarısız olur. Doğru ilke ayarına sahip olmayan makinelerde, yeni ilkeyi uygulayın, yeni ilkeyi güncelleştirmeye zorlayın (gpupdate. exe/Force) ve yeniden başlatma gerekebilir.  


## <a name="bkmk_PSH"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) , Azure kaynaklarınızı yönetmek için [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) modelini kullanan bir cmdlet kümesi sağlar. [Azure Cloud Shell](../cloud-shell/overview.md)tarayıcınızda kullanabilir veya bunu, herhangi bir PowerShell oturumunda kullanmak için aşağıdaki yönergeleri kullanarak yerel makinenize yükleyebilirsiniz. Yerel olarak zaten yüklüyse, Azure disk şifrelemesini yapılandırmak için Azure PowerShell en son sürümünü kullandığınızdan emin olun.

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Yerel makinenizde kullanım için Azure PowerShell yüklemesi yapın (isteğe bağlı):  
1. [Azure PowerShell yükleyip yapılandırın](/powershell/azure/install-az-ps). 

2. [Azure Active Directory PowerShell modülünü](/powershell/azure/active-directory/install-adv2#installing-the-azure-ad-module)yükler. 

     ```powershell
     Install-Module AzureAD
     ```

3. Modüllerin yüklü sürümlerini doğrulayın.
      ```powershell
      Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
      Get-Module AzureAD -ListAvailable | Select-Object -Property Name,Version,Path
      ```
4. [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet 'Ini kullanarak Azure 'da oturum açın.
     
     ```powershell
     Connect-AzAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzAccount –Environment (Get-AzEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzSubscription and 
     specify it with Set-AzContext.  #>
     Get-AzSubscription
     Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

5.  Azure AD [Connect-AzureAD](/powershell/module/azuread/connect-azuread)'e bağlanın.
     
     ```powershell
     Connect-AzureAD
     ```

6. Gerekirse Azure PowerShell ve [Azuread](/powershell/module/azuread) [ile çalışmaya](/powershell/azure/get-started-azureps) başlama konusunu gözden geçirin.

## <a name="bkmk_CLI"></a>Azure CLı

[Azure clı 2,0](/cli/azure) , Azure kaynaklarını yönetmeye yönelik bir komut satırı aracıdır. CLı, verileri esnek bir şekilde sorgulamak, engelleyici olmayan işlemler olarak uzun süreli işlemleri desteklemek ve komut dosyasını kolay hale getirmek için tasarlanmıştır. [Azure Cloud Shell](../cloud-shell/overview.md)tarayıcınızda kullanabilir veya bunu yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz.

1. [Azure CLI](/cli/azure/install-azure-cli) 'yi yerel makinenizde kullanım için (isteğe bağlı) kullanın:

2. Yüklü sürümü doğrulayın.
     
     ```azurecli-interactive
     az --version
     ``` 

3. [Az Login](/cli/azure/authenticate-azure-cli)kullanarak Azure 'da oturum açın.
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Gerekirse [Azure clı 2,0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli) konusunu inceleyin. 


## <a name="prerequisite-workflow-for-key-vault-and-the-azure-ad-app"></a>Key Vault ve Azure AD uygulaması için önkoşul iş akışı

Azure disk şifrelemesi için Key Vault ve Azure AD önkoşullarını zaten biliyorsanız, [Azure disk şifrelemesi önkoşulları PowerShell betiğini](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 )kullanabilirsiniz. Önkoşul betiğini kullanma hakkında daha fazla bilgi için bkz. [Azure disk şifrelemesi eki](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Bir Anahtar Kasası oluşturun. 
2. Bir Azure AD uygulaması ve hizmet sorumlusu ayarlayın.
3. Azure AD uygulaması için Anahtar Kasası erişim ilkesini ayarlayın.
4. Anahtar Kasası Gelişmiş erişim ilkeleri ayarlayın.
 
## <a name="bkmk_KeyVault"></a>Anahtar Kasası oluşturma 
Azure disk şifrelemesi, Anahtar Kasası aboneliğinizdeki disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) ile tümleşiktir. Bir Anahtar Kasası oluşturabilir veya mevcut bir Azure disk şifrelemesi kullanabilirsiniz. Anahtar kasaları hakkında daha fazla bilgi için bkz. [Azure Key Vault kullanmaya başlama](../key-vault/key-vault-get-started.md) ve [anahtar kasanızın güvenliğini sağlama](../key-vault/key-vault-secure-your-key-vault.md). Bir Anahtar Kasası oluşturmak için Kaynak Yöneticisi şablonu, Azure PowerShell veya Azure CLı kullanabilirsiniz. 


>[!WARNING]
>Şifreleme gizli dizileri 'nin bölgesel sınırların dışına bulunmadığından emin olmak için, Azure disk şifrelemesi Key Vault ve VM 'Lerin aynı bölgede birlikte bulunması gerekir. Şifrelenecek VM ile aynı bölgedeki bir Key Vault oluşturun ve kullanın. 


### <a name="bkmk_KVPSH"></a>PowerShell ile Anahtar Kasası oluşturma

[New-Azkeykasa](/powershell/module/az.keyvault/New-azKeyVault) cmdlet 'ini kullanarak Azure PowerShell bir Anahtar Kasası oluşturabilirsiniz. Key Vault ek cmdlet 'ler için, bkz [. az. Keykasası](/powershell/module/az.keyvault/). 

1. Gerekirse [Azure aboneliğinize bağlanın](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Gerekirse, [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup)ile yeni bir kaynak grubu oluşturun.  Veri merkezi konumlarını listelemek için [Get-AzLocation](/powershell/module/az.resources/get-azlocation)kullanın. 
     
     ```azurepowershell-interactive
     # Get-AzLocation 
     New-AzResourceGroup –Name 'MyKeyVaultResourceGroup' –Location 'East US'
     ```

3. [New-Azkeykasasını](/powershell/module/az.keyvault/New-azKeyVault) kullanarak yeni bir Anahtar Kasası oluşturma
    
      ```azurepowershell-interactive
     New-AzKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -Location 'East US'
     ```

4. Daha sonra diskleri şifrelerken kullanılmak üzere döndürülen **kasa adı**, **kaynak grubu adı**, **kaynak kimliği**, **kasa URI 'si**ve **nesne kimliği** ' ni unutmayın. 


### <a name="bkmk_KVCLI"></a>Azure CLı ile Anahtar Kasası oluşturma
[Az keykasa](/cli/azure/keyvault#commands) komutlarını kullanarak anahtar kasanızı Azure CLI ile yönetebilirsiniz. Bir Anahtar Kasası oluşturmak için [az keykasacreate](/cli/azure/keyvault#az-keyvault-create)komutunu kullanın.

1. Gerekirse [Azure aboneliğinize bağlanın](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Gerekirse, [az Group Create](/cli/azure/group#az-group-create)ile yeni bir kaynak grubu oluşturun. Konumları listelemek için [az Account List-Locations](/cli/azure/account#az-account-list) kullanın 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MyKeyVaultResourceGroup" -l "East US"
     ```

3. [Az keykasa Create](/cli/azure/keyvault#az-keyvault-create)komutunu kullanarak yeni bir Anahtar Kasası oluşturun.
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --location "East US"
     ```

4. **Kasa adı** (ad), **kaynak grubu adı**, **kaynak kimliği** (kimlik), **kasa URI 'Si**ve daha sonra kullanılmak üzere döndürülen **nesne kimliği** ' ni unutmayın. 

### <a name="bkmk_KVRM"></a>Kaynak Yöneticisi şablonuyla Anahtar Kasası oluşturma

[Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)kullanarak bir Anahtar Kasası oluşturabilirsiniz.

1. Azure hızlı başlangıç şablonunda **Azure 'A dağıt**' a tıklayın.
2. Abonelik, kaynak grubu, kaynak grubu konumu, Key Vault adı, nesne KIMLIĞI, yasal koşullar ve anlaşma ' ı seçin ve ardından **satın al**' a tıklayın. 


## <a name="bkmk_ADapp"></a>Bir Azure AD uygulaması ve hizmet sorumlusu ayarlama 
Azure 'da çalışan bir VM 'de şifrelemenin etkinleştirilmesi gerektiğinde, Azure disk şifrelemesi anahtar kasanıza şifreleme anahtarları oluşturur ve yazar. Anahtar Kasanızda şifreleme anahtarlarını yönetmek için Azure AD kimlik doğrulaması gerekir. Bu amaçla bir Azure AD uygulaması oluşturun. Kimlik doğrulama amacıyla, istemci gizli tabanlı kimlik doğrulaması veya [istemci sertifikası tabanlı Azure AD kimlik doğrulaması](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md)kullanabilirsiniz.


### <a name="bkmk_ADappPSH"></a>Azure PowerShell ile bir Azure AD uygulaması ve hizmet sorumlusu ayarlama 
Aşağıdaki komutları yürütmek için [Azure AD PowerShell modülünü](/powershell/azure/active-directory/install-adv2)alın ve kullanın. 

1. Gerekirse [Azure aboneliğinize bağlanın](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. [New-AzADApplication](/powershell/module/az.resources/new-azadapplication) PowerShell cmdlet 'ini kullanarak BIR Azure AD uygulaması oluşturun. MyApplicationHomePage ve MyApplicationUri istediğiniz herhangi bir değer olabilir.

     ```azurepowershell
     $aadClientSecret = "My AAD client secret"
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force
     $azureAdApplication = New-AzADApplication -DisplayName "My Application Display Name" -HomePage "https://MyApplicationHomePage" -IdentifierUris "https://MyApplicationUri" -Password $aadClientSecretSec
     $servicePrincipal = New-AzADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId
     ```

3. $AzureAdApplication. ApplicationId, Azure AD ClientID 'dir ve $aadClientSecret, daha sonra Azure disk şifrelemeyi etkinleştirmek için kullanacağınız istemci gizli anahtarı olur. Azure AD İstemci gizliliğini uygun şekilde koruma. @No__t çalıştırmak-0, ApplicationId gösterecektir.


### <a name="bkmk_ADappCLI"></a>Azure CLı ile bir Azure AD uygulaması ve hizmet sorumlusu ayarlama

Hizmet sorumlularını, [az ad SP](/cli/azure/ad/sp) komutlarını kullanarak Azure CLI ile yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli).

1. Gerekirse [Azure aboneliğinize bağlanın](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Yeni bir hizmet sorumlusu oluşturun.
     
     ```azurecli-interactive
     az ad sp create-for-rbac --name "ServicePrincipalName" --password "My-AAD-client-secret" --skip-assignment 
     ```
3.  Döndürülen uygulama kimliği, diğer komutlarda kullanılan Azure AD ClientID 'dir. Bu, az keykasası Set-Policy için kullanacağınız SPN 'YI de kullanabilirsiniz. Parola, daha sonra Azure disk şifrelemeyi etkinleştirmek için kullanmanız gereken istemci sırrı olur. Azure AD İstemci gizliliğini uygun şekilde koruma.
 
### <a name="bkmk_ADappRM"></a>Azure portal bir Azure AD uygulaması ve hizmet sorumlusu ayarlama
Azure AD uygulaması oluşturmak için, [kaynak makalesine erişebilen Azure Active Directory bir uygulama ve hizmet sorumlusu oluşturmak için portalı kullanma](../active-directory/develop/howto-create-service-principal-portal.md) adımlarını kullanın. Aşağıda listelenen her adımın tamamlanması doğrudan makale bölümüne götürür. 

1. [Gerekli izinleri doğrulama](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)
2. [Azure Active Directory uygulaması oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) 
     - Uygulamayı oluştururken istediğiniz ad ve oturum açma URL 'sini kullanabilirsiniz.
3. [Uygulama kimliğini ve kimlik doğrulama anahtarını alın](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in). 
     - Kimlik doğrulama anahtarı, istemci sırrı ve set-AzVMDiskEncryptionExtension için AadClientSecret olarak kullanılır. 
        - Kimlik doğrulama anahtarı, uygulama tarafından Azure AD 'de oturum açmak için kimlik bilgileri olarak kullanılır. Azure portal, bu gizliliğe anahtarlar denir, ancak anahtar kasalarıyla ilgili hiçbir ilişki yoktur. Bu gizli anahtarı uygun şekilde koruyun. 
     - Uygulama KIMLIĞI daha sonra set-AzVMDiskEncryptionExtension için Aadclitıd ve set-AzKeyVaultAccessPolicy için ServicePrincipalName olarak kullanılacaktır. 

## <a name="bkmk_KVAP"></a>Azure AD uygulaması için Anahtar Kasası erişim ilkesini ayarlama
Şifreleme gizli dizilerini belirtilen bir Key Vault yazmak için Azure disk şifrelemesi, Key Vault gizli dizileri yazma izinlerine sahip Azure Active Directory uygulamanın Istemci KIMLIĞI ve Istemci gizli anahtarı gerektirir. 

> [!NOTE]
> Azure disk şifrelemesi, Azure AD İstemci uygulamanıza aşağıdaki erişim ilkelerini yapılandırmanızı gerektirir: _WrapKey_ ve izinleri _ayarlayın_ .

### <a name="bkmk_KVAPPSH"></a>Azure PowerShell Azure AD uygulaması için Anahtar Kasası erişim ilkesini ayarlama
Azure AD uygulamanız, kasadaki anahtarlara veya gizli anahtara erişmek için haklara ihtiyaç duyuyor. Uygulamaya izin vermek için [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet 'ini kullanarak, istemci kimliğini (uygulama kaydedildiğinde oluşturulan) _– servicePrincipalName_ parametre değeri olarak kullanın. Daha fazla bilgi edinmek için [Azure Key Vault](https://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx)blog gönderisine bakın. adım adım. 

1. Gerekirse [Azure aboneliğinize bağlanın](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. PowerShell ile AD uygulaması için Anahtar Kasası erişim ilkesini ayarlayın.

     ```azurepowershell
     $keyVaultName = 'MySecureVault'
     $aadClientID = 'MyAadAppClientID'
     $KVRGname = 'MyKeyVaultResourceGroup'
     Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname
     ```

### <a name="bkmk_KVAPCLI"></a>Azure CLı ile Azure AD uygulaması için Anahtar Kasası erişim ilkesini ayarlama
Erişim ilkesini ayarlamak için [az keykasası Set-Policy](/cli/azure/keyvault#az-keyvault-set-policy) komutunu kullanın. Daha fazla bilgi için bkz. [clı 2,0 kullanarak Key Vault yönetme](../key-vault/key-vault-manage-with-cli2.md#authorizing-an-application-to-use-a-key-or-secret).

1. Gerekirse [Azure aboneliğinize bağlanın](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Aşağıdaki komutla gizli anahtar almak ve anahtarları kaydırmak için Azure CLı erişimi aracılığıyla oluşturduğunuz hizmet sorumlusuna izin verin:
 
     ```azurecli-interactive
     az keyvault set-policy --name "MySecureVault" --spn "<spn created with CLI/the Azure AD ClientID>" --key-permissions wrapKey --secret-permissions set
     ```

### <a name="bkmk_KVAPRM"></a>Portal ile Azure AD uygulaması için Anahtar Kasası erişim ilkesini ayarlama

1. Anahtar kasanızın bulunduğu kaynak grubunu açın.
2. Anahtar kasanızı seçin, **erişim ilkeleri**' ne gidin ve **Yeni Ekle**' ye tıklayın.
3. **Asıl seçin**altında oluşturduğunuz Azure AD uygulamasını arayın ve seçin. 
4. **Anahtar izinleri**Için, **şifreleme işlemleri**altında **Sarla tuşu** ' nı işaretleyin.
5. **Gizli izinler**Için, **gizli yönetim işlemleri**altında **Ayarla** ' yı işaretleyin.
6. Erişim ilkesini kaydetmek için **Tamam** ' ı tıklatın. 

![Azure Key Vault şifreleme işlemleri-Wrap tuşu](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault gizli izinleri-ayarla](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

## <a name="bkmk_KVper"></a>Anahtar Kasası Gelişmiş erişim ilkelerini ayarlama
Azure platformunun, birimleri önyüklemek ve şifrelerini çözmek için sanal makine için kullanılabilir hale getirmek üzere anahtar kasasındaki şifreleme anahtarlarına veya gizli anahtarlara erişmesi gerekir. Anahtar kasasında disk şifrelemeyi etkinleştirin veya dağıtımlar başarısız olur.  

### <a name="bkmk_KVperPSH"></a>Azure PowerShell ile Anahtar Kasası Gelişmiş erişim ilkeleri ayarlama
 Anahtar Kasası için disk şifrelemeyi etkinleştirmek üzere [set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) Anahtar Kasası PowerShell cmdlet 'ini kullanın.

  - **Key Vault disk şifrelemesi Için etkinleştir:** Azure disk şifrelemesi için EnabledForDiskEncryption gereklidir.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDiskEncryption
     ```

  - **Gerekirse dağıtım için Key Vault etkinleştirin:** Bu Anahtar Kasası kaynak oluşturma bölümünde başvuruluyorsa, örneğin bir sanal makine oluştururken, Microsoft. COMPUTE kaynak sağlayıcısı 'nın bu anahtar kasasından gizli dizileri almasını sağlar.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDeployment
     ```

  - **Gerekirse, şablon dağıtımı için Key Vault etkinleştirin:** Bu anahtar kasası bir şablon dağıtımında başvuruluyorsa, Azure Resource Manager Bu anahtar kasasından gizli dizileri almasını sağlar.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForTemplateDeployment
     ```

### <a name="bkmk_KVperCLI"></a>Azure CLı kullanarak Anahtar Kasası Gelişmiş erişim ilkelerini ayarlama
Anahtar Kasası için disk şifrelemeyi etkinleştirmek üzere [az keykasatıon Update](/cli/azure/keyvault#az-keyvault-update) kullanın. 

 - **Key Vault disk şifrelemesi Için etkinleştir:** Etkin-disk şifrelemesi gereklidir. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Gerekirse dağıtım için Key Vault etkinleştirin:** Sanal makinelerin kasadan gizli dizi olarak depolanan sertifikaları almasına izin verin.
     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Gerekirse, şablon dağıtımı için Key Vault etkinleştirin:** Kaynak Yöneticisi kasalardan gizli dizileri almasına izin verin.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a>Azure portal aracılığıyla Anahtar Kasası Gelişmiş erişim ilkeleri ayarlama

1. Keykasanızı seçin, **erişim ilkeleri**' ne gidin ve **Gelişmiş erişim Ilkelerini göstermek için tıklayın**.
2. **Birim şifrelemesi Için Azure disk şifrelemesi 'ne erişimi etkinleştir**etiketli kutuyu seçin.
3. **Dağıtım Için Azure sanal makinelerine erişimi etkinleştir** ' i seçin ve/veya gerekirse **şablon dağıtımı Için Azure Resource Manager erişimi etkinleştirin**. 
4. **Kaydet**'e tıklayın.

![Azure Anahtar Kasası Gelişmiş erişim ilkeleri](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a>Anahtar şifreleme anahtarı ayarlama (isteğe bağlı)
Şifreleme anahtarları için ek bir güvenlik katmanı için anahtar şifreleme anahtarı (KEK) kullanmak istiyorsanız, anahtar kasanıza bir KEK ekleyin. Anahtar kasasında anahtar şifreleme anahtarı oluşturmak için [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet 'ini kullanın. Ayrıca, şirket içi anahtar yönetimi HSM 'nizden bir KEK içeri aktarabilirsiniz. Daha fazla bilgi için bkz. [Key Vault belgeleri](../key-vault/key-vault-hsm-protected-keys.md). Anahtar şifreleme anahtarı belirtildiğinde Azure disk şifrelemesi, Key Vault yazmadan önce şifreleme gizli dizilerini kaydırmak için bu anahtarı kullanır. 

* Anahtar oluştururken bir RSA anahtar türü kullanın. Azure disk şifrelemesi henüz eliptik eğri anahtarlarını kullanmayı desteklemiyor.

* Anahtar Kasası gizli ve KEK URL 'Lerinin sürümü oluşturulmalıdır. Azure, sürüm oluşturma kısıtlaması uygular. Geçerli gizli ve KEK URL 'Leri için aşağıdaki örneklere bakın:

  * Geçerli bir gizli dizi URL 'SI örneği: *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Geçerli bir KEK URL örneği: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure disk şifrelemesi, Anahtar Kasası gizli dizileri ve KEK URL 'Lerinin bir parçası olarak bağlantı noktası numaraları belirtilmesini desteklemez. Desteklenmeyen ve desteklenen Anahtar Kasası URL 'Leri örnekleri için aşağıdaki örneklere bakın:

  * Kabul edilmeyen Anahtar Kasası URL 'SI *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Kabul edilebilir Anahtar Kasası URL 'SI: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="bkmk_KEKPSH"></a>Azure PowerShell ile anahtar şifreleme anahtarı ayarlama 
PowerShell betiğini kullanmadan önce, betikteki adımları anlamak için Azure disk şifrelemesi önkoşulları hakkında bilgi sahibi olmanız gerekir. Örnek betik, ortamınız için değişiklikler gerektirebilir. Bu betik, tüm Azure disk şifrelemesi önkoşullarını oluşturur ve var olan bir IaaS sanal makinesini şifreler, anahtar şifreleme anahtarını kullanarak disk şifreleme anahtarını sarmalama. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MyKeyVaultResourceGroup', and 'MySecureVault' with your values.
     # Use Get-AzLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzResourceGroup
     
     $Loc = 'MyLocation';
     $KVRGname = 'MyKeyVaultResourceGroup';
     $KeyVaultName = 'MySecureVault'; 
     New-AzResourceGroup –Name  $KVRGname –Location $Loc;
     New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc;
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname;
     $KeyVaultResourceId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName  $KVRGname).VaultUri;
     
 # Step 2: Create the AD application and service principal.
     # Fill in 'MyAADClientSecret', "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
     # MyApplicationHomePage and the MyApplicationUri can be any values you wish.
     
     $aadClientSecret =  'MyAADClientSecret';
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force;
     $azureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -Password $aadClientSecretSec
     $servicePrincipal = New-AzADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId;
     $aadClientID = $azureAdApplication.ApplicationId;
     
 #Step 3: Enable the vault for disk encryption and set the access policy for the Azure AD application.
     
     Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption;
     Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName  $KVRGname;
     
 #Step 4: Create a new key in the key vault with the Add-AzKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 5: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values. 
     
     $VMName = 'MySecureVM';
      $VMRGName = 'MyVirtualMachineResourceGroup';
     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```

## <a name="bkmk_Cert"></a>Sertifika tabanlı kimlik doğrulaması (isteğe bağlı)
Sertifika kimlik doğrulamasını kullanmak isterseniz, anahtar kasanıza bir tane yükleyebilir ve istemciye dağıtabilirsiniz. PowerShell betiğini kullanmadan önce, betikteki adımları anlamak için Azure disk şifrelemesi önkoşulları hakkında bilgi sahibi olmanız gerekir. Örnek betik, ortamınız için değişiklikler gerektirebilir.

     
 ```powershell

 # Fill in "MyKeyVaultResourceGroup", "MySecureVault", and 'MyLocation' ('My location' only if needed)

   $KVRGname = 'MyKeyVaultResourceGroup'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption. No need to set 'My location' in this case.

   $Loc = 'MyLocation'
   New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption

   #Setting some variables with the key vault information 
   $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId

   # Create the Azure AD application and associate the certificate with it. 
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname
   
   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert".  

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for -EnabledForDeployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDeployment

   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values.

   $VMName = 'MySecureVM'
   $VMRGName = 'MyVirtualMachineResourceGroup'
   $CertUrl = (Get-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGName).ResourceId
   $VM = Get-AzVM -ResourceGroupName $VMRGName -Name $VMName 
   $VM = Add-AzVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzVM -VM $VM -ResourceGroupName $VMRGName 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
 ```

## <a name="bkmk_CertKEK"></a>Sertifika tabanlı kimlik doğrulaması ve bir KEK (isteğe bağlı)

Sertifika kimlik doğrulamasını kullanmak ve şifreleme anahtarını bir KEK ile kaydırmak isterseniz, aşağıdaki betiği örnek olarak kullanabilirsiniz. PowerShell betiğini kullanmadan önce, betikteki adımları anlamak için önceki tüm Azure disk şifrelemesi önkoşullarını bilmeniz gerekir. Örnek betik, ortamınız için değişiklikler gerektirebilir.

> [!IMPORTANT]
> Azure AD Sertifika tabanlı kimlik doğrulaması şu anda Linux sanal makinelerinde desteklenmez.



     
 ```powershell
# Fill in 'MyKeyVaultResourceGroup', 'MySecureVault', and 'MyLocation' (if needed)

   $KVRGname = 'MyKeyVaultResourceGroup'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption.

   $Loc = 'MyLocation'
   New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption

   # Create the Azure AD application and associate the certificate with it.  
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   ## Give access for setting secrets and wraping keys
   Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname

   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert". 

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for deployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDeployment

   #Setting some variables with the key vault information and generating a KEK 
   # FIll in 'KEKName'
   
   $KEKName ='KEKName'
   $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId
   $KEK = Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $KEKName -Destination "Software"
   $KeyEncryptionKeyUrl = $KEK.Key.kid



   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values.

   $VMName = 'MySecureVM';
   $VMRGName = 'MyVirtualMachineResourceGroup';
   $CertUrl = (Get-AzKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGName).ResourceId
   $VM = Get-AzVM -ResourceGroupName $VMRGName -Name $VMName 
   $VM = Add-AzVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzVM -VM $VM -ResourceGroupName $VMRGName

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

 
## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Windows için Azure disk şifrelemesini etkinleştirme](azure-security-disk-encryption-windows-aad.md)

> [!div class="nextstepaction"]
> [Linux için Azure disk şifrelemesini etkinleştirme](azure-security-disk-encryption-linux-aad.md)
