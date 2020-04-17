---
title: Azure AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma (önceki sürüm)
description: Bu makalede, IaaS VM'ler için Microsoft Azure Disk Şifrelemesi kullanmak için ön koşullar sağlanmaktadır.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/15/2019
ms.custom: seodec18
ms.openlocfilehash: 0ec46a1d2c7fca231b5cf6b045b634af50ee12a7
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459840"
---
# <a name="creating-and-configuring-a-key-vault-for-azure-disk-encryption-with-azure-ad-previous-release"></a>Azure AD ile Azure Disk Şifrelemesi için önemli bir kasa oluşturma ve yapılandırma (önceki sürüm)

**Azure Disk Şifreleme'nin yeni sürümü, VM disk şifrelemesini etkinleştirmek için Bir Azure AD uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle, şifrelemeyi etkinleştir medeni sırasında Azure AD kimlik bilgilerini sağlamanız artık gerekmez. Tüm yeni VM'ler, yeni sürümü kullanarak Azure AD uygulama parametreleri olmadan şifrelenmelidir. Yeni sürümü kullanarak VM disk şifrelemesini etkinleştirmek için yönergeleri görüntülemek için [Azure Disk Şifreleme'ye](disk-encryption-overview.md)bakın. Azure AD uygulama parametreleri ile zaten şifrelenmiş olan VM'ler hala desteklenir ve AAD sözdizimi ile korunmaya devam etmelidir.**

Azure Disk Şifreleme, disk şifreleme anahtarlarını ve sırlarını kontrol etmek ve yönetmek için Azure Key Vault'u kullanır.  Anahtar kasaları hakkında daha fazla bilgi için Azure Key Vault ve [Secure anahtar kasanızı](../../key-vault/general/secure-your-key-vault.md) [ile işe başlayın.](../../key-vault/key-vault-get-started.md) 

Azure AD (önceki sürüm) ile Azure Disk Şifrelemesi ile kullanılmak üzere önemli bir kasa oluşturma ve yapılandırma üç adım içerir:

1. Bir anahtar kasası oluşturma. 
2. Azure AD uygulaması ve hizmet ilkesi ayarlayın.
3. Azure Active Directory uygulaması için anahtar kasası erişim ilkesi ayarlama.
4. Anahtar kasası gelişmiş erişim ilkelerini ayarlama.
 
Ayrıca, isterseniz bir anahtar şifreleme anahtarı (KEK) oluşturabilir veya içe aktarabilirsiniz.

Araçları yükleme ve Azure'a bağlanma adımlarını oluşturmak için Azure Disk Şifreleme makalesi [için önemli bir kasa oluşturma](disk-encryption-key-vault.md) [ve](disk-encryption-key-vault.md#install-tools-and-connect-to-azure)yapılandırma ana bakın.

> [!Note]
> Bu makaledeki [adımlar, Azure Disk Şifreleme önkoşullar CLI komut dosyası](https://github.com/ejarvi/ade-cli-getting-started) ve [Azure Disk Şifreleme önkoşullar PowerShell komut otomatiktir.](https://github.com/Azure/azure-powershell/tree/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts)


## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma 
Azure Disk Şifreleme, anahtar kasa aboneliğinizdeki disk şifreleme anahtarlarını ve sırlarını kontrol etmenize ve yönetmenize yardımcı olmak için [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) ile entegre edilmiştir. Önemli bir kasa oluşturabilir veya Azure Disk Şifreleme için varolan bir kasa kullanabilirsiniz. Anahtar kasaları hakkında daha fazla bilgi için Azure Key Vault ve [Secure anahtar kasanızı](../../key-vault/general/secure-your-key-vault.md) [ile işe başlayın.](../../key-vault/key-vault-get-started.md) Önemli bir kasa oluşturmak için Kaynak Yöneticisi şablonu, Azure PowerShell veya Azure CLI kullanabilirsiniz. 


>[!WARNING]
>Şifreleme sırlarının bölgesel sınırları aşmadığından emin olmak için Azure Disk Şifreleme'nin aynı bölgede birlikte bulunabilmesi için Anahtar Kasası ve VM'lere ihtiyacı vardır. Şifrelenecek VM ile aynı bölgede bulunan bir Anahtar Kasası oluşturun ve kullanın. 


### <a name="create-a-key-vault-with-powershell"></a><a name="bkmk_KVPSH"></a>PowerShell ile önemli bir kasa oluşturun

[New-AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault) cmdlet'ini kullanarak Azure PowerShell ile önemli bir kasa oluşturabilirsiniz. Key Vault için ek cmdletler için [Az.KeyVault'a](/powershell/module/az.keyvault/)bakın. 

1. [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup)ile gerekirse yeni bir kaynak grubu oluşturun.  Veri merkezi konumlarını listelemek için [Get-AzLocation'ı](/powershell/module/az.resources/get-azlocation)kullanın. 
     
     ```azurepowershell-interactive
     # Get-AzLocation 
     New-AzResourceGroup –Name 'MyKeyVaultResourceGroup' –Location 'East US'
     ```

1. [New-AzKeyVault'u](/powershell/module/az.keyvault/New-azKeyVault) kullanarak yeni bir anahtar kasası oluşturun
    
      ```azurepowershell-interactive
     New-AzKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -Location 'East US'
     ```

4. Vault **Adı,** **Kaynak Grubu Adı,** **Kaynak Kimliği,** **Vault URI**ve diskleri şifrelediğinizde daha sonra kullanılmak üzere döndürülen Nesne **Kimliğini** not edin. 


### <a name="create-a-key-vault-with-azure-cli"></a><a name="bkmk_KVCLI"></a>Azure CLI ile önemli bir kasa oluşturma
[Az keyvault](/cli/azure/keyvault#commands) komutlarını kullanarak Azure CLI ile anahtar kasanızı yönetebilirsiniz. Anahtar tonozu oluşturmak için [az keyvault oluşturmak](/cli/azure/keyvault#az-keyvault-create)kullanın.

1. Az grubu oluşturmak ile gerekirse, yeni bir kaynak grubu [oluşturun.](/cli/azure/group#az-group-create) Konumları listelemek için [az hesap listesi konumlarını](/cli/azure/account#az-account-list) kullanın 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MyKeyVaultResourceGroup" -l "East US"
     ```

3. Az keyvault oluşturarak yeni bir anahtar [tonoz oluşturun.](/cli/azure/keyvault#az-keyvault-create)
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --location "East US"
     ```

4. Kasa **Adı** (adı), **Kaynak Grubu Adı,** **Kaynak Kimliği** (ID), Vault **URI**ve daha sonra kullanılmak üzere döndürülen **Nesne Kimliğini** not edin. 

### <a name="create-a-key-vault-with-a-resource-manager-template"></a><a name="bkmk_KVRM"></a>Kaynak Yöneticisi şablonu içeren anahtar kasası oluşturma

[Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)kullanarak bir anahtar kasası oluşturabilirsiniz.

1. Azure hızlı başlatma şablonunda, **Azure'a Dağıt'ı**tıklatın.
2. Abonelik, kaynak grubu, kaynak grubu konumu, Anahtar Kasa adı, Nesne Kimliği, yasal terimler ve anlaşma seçin ve ardından **Satın Al'ı**tıklatın. 


## <a name="set-up-an-azure-ad-app-and-service-principal"></a><a name="bkmk_ADapp"></a>Azure AD uygulaması ve hizmet ilkesi ayarlama 
Azure'da çalışan bir VM'de etkinleştirilebilmek için şifrelemeye ihtiyacınız olduğunda, Azure Disk Şifrelemesi anahtar kasanızın şifreleme anahtarlarını oluşturur ve yazar. Anahtar kasanızda şifreleme anahtarlarını yönetmek için Azure AD kimlik doğrulaması gerekir. Bu amaçla bir Azure AD uygulaması oluşturun. Kimlik doğrulama amacıyla, istemci gizli tabanlı kimlik doğrulaması veya [istemci sertifikası tabanlı Azure AD kimlik doğrulaması](../../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md)kullanabilirsiniz.


### <a name="set-up-an-azure-ad-app-and-service-principal-with-azure-powershell"></a><a name="bkmk_ADappPSH"></a>Azure PowerShell ile Azure AD uygulaması ve hizmet ilkesi ayarlama 
Aşağıdaki komutları çalıştırmak için Azure [AD PowerShell modüllerini](/powershell/azure/active-directory/install-adv2)alın ve kullanın. 

1. Azure AD uygulaması oluşturmak için [Yeni AzADApplication](/powershell/module/az.resources/new-azadapplication) PowerShell cmdlet'ini kullanın. MyApplicationHomePage ve MyApplicationUri istediğiniz herhangi bir değer olabilir.

     ```azurepowershell
     $aadClientSecret = "My AAD client secret"
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force
     $azureAdApplication = New-AzADApplication -DisplayName "My Application Display Name" -HomePage "https://MyApplicationHomePage" -IdentifierUris "https://MyApplicationUri" -Password $aadClientSecretSec
     $servicePrincipal = New-AzADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId
     ```

3. $azureAdApplication.ApplicationId, Azure AD ClientID'dir ve $aadClientSecret, Azure Disk Şifrelemesi'ni etkinleştirmek için daha sonra kullanacağınız istemci sırrıdır. Azure AD istemcisi sırrını uygun şekilde koruyun. Çalışan `$azureAdApplication.ApplicationId` size ApplicationID gösterecektir.


### <a name="set-up-an-azure-ad-app-and-service-principal-with-azure-cli"></a><a name="bkmk_ADappCLI"></a>Azure CLI ile Azure AD uygulaması ve hizmet ilkesi ayarlama

Az [reklam sp](/cli/azure/ad/sp) komutlarını kullanarak Azure CLI ile hizmet ilkelerinizi yönetebilirsiniz. Daha fazla bilgi için [bkz.](/cli/azure/create-an-azure-service-principal-azure-cli)

1. Yeni bir hizmet ilkesi oluşturun.
     
     ```azurecli-interactive
     az ad sp create-for-rbac --name "ServicePrincipalName" --password "My-AAD-client-secret" --skip-assignment 
     ```
3.  Döndürülen appId, diğer komutlarda kullanılan Azure AD ClientID'sidir. Ayrıca az keyvault ayar politikası için kullanacağınız SPN'dir. Parola, Azure Disk Şifrelemesi'ni etkinleştirmek için daha sonra kullanmanız gereken istemci sırrıdır. Azure AD istemcisi sırrını uygun şekilde koruyun.
 
### <a name="set-up-an-azure-ad-app-and-service-principal-though-the-azure-portal"></a><a name="bkmk_ADappRM"></a>Azure portalı olsa da bir Azure AD uygulaması ve hizmet ilkesi ayarlama
Bir Azure AD uygulaması oluşturmak için kaynaklar makalesine [erişebilen bir Azure Active Directory uygulaması ve hizmet ilkesi oluşturmak](../../active-directory/develop/howto-create-service-principal-portal.md) için Kullanım portalındaki adımları kullanın. Aşağıda listelenen her adım, sizi tamamlamak için doğrudan makale bölümüne götürecektir. 

1. [Gerekli izinleri doğrulama](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)
2. [Azure Etkin Dizin uygulaması oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) 
     - Uygulamayı oluştururken istediğiniz adı ve oturum açma URL'sini kullanabilirsiniz.
3. [Uygulama kimliğini ve kimlik doğrulama anahtarını alın.](../../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in) 
     - Kimlik doğrulama anahtarı istemci sırrıdır ve Set-AzVMDiskEncryptionExtension için AadClientSecret olarak kullanılır. 
        - Kimlik doğrulama anahtarı, uygulama tarafından Azure AD'de oturum açma için bir kimlik bilgisi olarak kullanılır. Azure portalında bu gizliye anahtar denir, ancak anahtar kasaları ile hiçbir ilişkisi yoktur. Bu sırrı uygun şekilde sabitle. 
     - Uygulama kimliği daha sonra Set-AzVMDiskEncryptionExtension için AadClientId ve Set-AzKeyVaultAccessPolicy için ServicePrincipalName olarak kullanılacaktır. 

## <a name="set-the-key-vault-access-policy-for-the-azure-ad-app"></a><a name="bkmk_KVAP"></a>Azure AD uygulaması için anahtar kasa erişim ilkesini ayarlama
Belirli bir Key Vault'a şifreleme sırları yazmak için Azure Disk Şifreleme'nin Anahtar Kasasına sır yazma izni olan Istemci Kimliği ve Azure Etkin Dizin uygulamasının Istemci Sırrı gerekir. 

> [!NOTE]
> Azure Disk Şifrelemesi, aşağıdaki erişim ilkelerini Azure AD istemci uygulamanızla yapılandırmanızı gerektirir: _Paket Anahtarı_ ve _Ayarizinler._

### <a name="set-the-key-vault-access-policy-for-the-azure-ad-app-with-azure-powershell"></a><a name="bkmk_KVAPPSH"></a>Azure PowerShell ile Azure AD uygulaması için anahtar kasa erişim ilkesini ayarlayın
Azure AD uygulamanızın kasadaki anahtarlara veya sırlara erişmek için haklara ihtiyacı vardır. _–ServicePrincipalName_ parametre değeri olarak istemci kimliğini (uygulama kaydedildiğinde oluşturulan) kullanarak uygulamaya izin vermek için [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet'i kullanın. Daha fazla bilgi için [Azure Key Vault](https://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx)adlı blog gönderisine bakın - Adım Adım . 

1. PowerShell ile AD uygulaması için anahtar kasa erişim ilkesini ayarlayın.

     ```azurepowershell
     $keyVaultName = 'MySecureVault'
     $aadClientID = 'MyAadAppClientID'
     $KVRGname = 'MyKeyVaultResourceGroup'
     Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $KVRGname
     ```

### <a name="set-the-key-vault-access-policy-for-the-azure-ad-app-with-azure-cli"></a><a name="bkmk_KVAPCLI"></a>Azure CLI ile Azure AD uygulaması için anahtar kasa erişim ilkesini ayarlayın
Erişim ilkesini ayarlamak için [az keyvault ayar ilkesini](/cli/azure/keyvault#az-keyvault-set-policy) kullanın. Daha fazla bilgi için [CLI 2.0 kullanarak Anahtar Kasası Yönet'e](../../key-vault/general/manage-with-cli2.md#authorizing-an-application-to-use-a-key-or-secret)bakın.

Aşağıdaki komutla sırları almak ve anahtarları sarmak için Azure CLI erişimi aracılığıyla oluşturduğunuz hizmet sorumlusuna verin:
 
     ```azurecli-interactive
     az keyvault set-policy --name "MySecureVault" --spn "<spn created with CLI/the Azure AD ClientID>" --key-permissions wrapKey --secret-permissions set
     ```

### <a name="set-the-key-vault-access-policy-for-the-azure-ad-app-with-the-portal"></a><a name="bkmk_KVAPRM"></a>Portalla Azure AD uygulaması için anahtar kasa erişim ilkesini ayarlayın

1. Anahtar kasanızla kaynak grubunu açın.
2. Anahtar kasanızı seçin, **Access İlkeleri'ne**gidin, ardından **Yeni Ekle'yi**tıklatın.
3. **Select ana prensibi**altında, oluşturduğunuz Azure AD uygulamasını arayın ve seçin. 
4. **Anahtar izinleri**için, Şifreleme İşlemleri altında Kaydırma **Anahtarı'nı**denetleyin. **Wrap Key**
5. **Gizli izinler için,** **Gizli Yönetim İşlemleri**altında **Set'i** kontrol edin.
6. Erişim ilkesini kaydetmek için **Tamam'ı** tıklatın. 

![Azure Key Vault şifreleme işlemleri - Wrap Key](./media/disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault Secret izinleri - Set](./media/disk-encryption/keyvault-portal-fig3b.png)

## <a name="set-key-vault-advanced-access-policies"></a><a name="bkmk_KVper"></a>Anahtar kasası gelişmiş erişim ilkelerini ayarlama
Azure platformu, birimlerin önyükleme ve şifreçözme için VM'de kullanılabilmesi için anahtar kasanızdaki şifreleme anahtarlarına veya sırlarına erişmeye ihtiyaç duyar. Anahtar kasasında disk şifrelemesini etkinleştirin veya dağıtımlar başarısız olur.  

### <a name="set-key-vault-advanced-access-policies-with-azure-powershell"></a><a name="bkmk_KVperPSH"></a>Azure PowerShell ile anahtar kasası gelişmiş erişim ilkelerini ayarlayın
 Anahtar kasası için disk şifrelemesini etkinleştirmek için PowerShell cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) tuşlarını kullanın.

  - **Disk şifrelemesi için Anahtar Kasasını etkinleştirin:** Azure Disk şifrelemesi için EnabledForDiskEncryption gereklidir.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDiskEncryption
     ```

  - **Gerekirse dağıtım için Anahtar Kasasını etkinleştirin:** Microsoft.Compute kaynak sağlayıcısının, örneğin sanal bir makine oluştururken kaynak oluşturmada bu anahtar kasasına başvurulduğunda bu anahtar kasasından sırları almasına olanak tanır.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDeployment
     ```

  - **Gerekirse şablon dağıtımı için Anahtar Kasasını etkinleştirin:** Bu anahtar kasasına bir şablon dağıtımında başvurulduğunda Azure Kaynak Yöneticisi'nin bu anahtar kasasından sır lar elde etmesini sağlar.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForTemplateDeployment
     ```

### <a name="set-key-vault-advanced-access-policies-using-the-azure-cli"></a><a name="bkmk_KVperCLI"></a>Azure CLI'yi kullanarak anahtar kasası gelişmiş erişim ilkelerini ayarlama
Anahtar kasası için disk şifrelemesini etkinleştirmek için [az keyvault güncelleştirmesini](/cli/azure/keyvault#az-keyvault-update) kullanın. 

 - **Disk şifrelemesi için Anahtar Kasasını etkinleştirin:** Disk için etkin şifreleme gereklidir. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Gerekirse dağıtım için Anahtar Kasasını etkinleştirin:** Sanal Makinelerin kasadan sır olarak depolanan sertifikaları almasına izin verin.
     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Gerekirse şablon dağıtımı için Anahtar Kasasını etkinleştirin:** Kaynak Yöneticisi'nin kasadan sırları almasına izin verin.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-template-deployment "true"
     ```


### <a name="set-key-vault-advanced-access-policies-through-the-azure-portal"></a><a name="bkmk_KVperrm"></a>Azure portalı üzerinden anahtar kasası gelişmiş erişim ilkelerini ayarlayın

1. Keyvault'unuzu seçin, **Access İlkeleri'ne**gidin ve **gelişmiş erişim ilkelerini göstermek için tıklatın.**
2. **Birim şifreleme için Azure Disk Şifrelemesine erişimi etkinleştir**etiketiyle kutuyu seçin.
3. **Dağıtım için Azure Sanal Makinelerine Erişimi Etkinleştir'i** ve/veya gerekirse şablon dağıtımı için Azure Kaynak **Yöneticisi'ne Erişimi**Etkinleştir'i'ni seçin. 
4. **Kaydet**’e tıklayın.

![Azure anahtar kasası gelişmiş erişim ilkeleri](./media/disk-encryption/keyvault-portal-fig4.png)


## <a name="set-up-a-key-encryption-key-optional"></a><a name="bkmk_KEK"></a>Anahtar şifreleme anahtarı ayarlama (isteğe bağlı)
Şifreleme anahtarları için ek bir güvenlik katmanı için bir anahtar şifreleme anahtarı (KEK) kullanmak istiyorsanız, anahtar kasanıza bir KEK ekleyin. Anahtar kasasında anahtar şifreleme anahtarı oluşturmak için [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet'i kullanın. Ayrıca şirket içi anahtar yönetimi HSM'nizden bir KEK de alabilirsiniz. Daha fazla bilgi için [Anahtar Kasa Belgeleri'ne](../../key-vault/keys/hsm-protected-keys.md)bakın. Bir anahtar şifreleme anahtarı belirtildiğinde, Azure Disk Şifreleme, Key Vault'a yazmadan önce şifreleme sırlarını sarmak için bu anahtarı kullanır. 

* Anahtar oluştururken RSA tuş türü kullanın. Azure Disk Şifreleme henüz Eliptik Eğri anahtarlarını kullanmayı desteklemez.

* Anahtar kasa sırrınız ve KEK URL'leriniz versiyona alınmalıdır. Azure bu sürüm kısıtlaması uygular. Geçerli gizli ve KEK URL'leri için aşağıdaki örneklere bakın:

  * Geçerli bir gizli URL örneği:*https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Geçerli bir KEK URL örneği:*https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk Şifreleme, önemli kasa sırları ve KEK URL'lerinin bir parçası olarak bağlantı noktası numaralarının belirtilmesine destek vermez. Desteklenmeyen ve desteklenen anahtar tonoz URL'lerine örnekler için aşağıdaki örneklere bakın:

  * Kabul edilemez anahtar kasa Sı*https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Kabul edilebilir anahtar kasa URL'si:*https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="set-up-a-key-encryption-key-with-azure-powershell"></a><a name="bkmk_KEKPSH"></a>Azure PowerShell ile önemli bir şifreleme anahtarı ayarlama 
PowerShell komut dosyasını kullanmadan önce, komut dosyasındaki adımları anlamak için Azure Disk Şifreleme ön koşulları hakkında bilginiz olmalıdır. Örnek komut dosyasının ortamınız için değişiklikler ekiolabilir. Bu komut dosyası, tüm Azure Disk Şifreleme ön koşulları oluşturur ve varolan bir IaaS VM'yi şifreleyerek, anahtar şifreleme anahtarını kullanarak disk şifreleme anahtarını saran bir dosyayı siler. 

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

## <a name="certificate-based-authentication-optional"></a><a name="bkmk_Cert"></a>Sertifika tabanlı kimlik doğrulama (isteğe bağlı)
Sertifika kimlik doğrulamasını kullanmak isterseniz, bir tanesini anahtar kasanıza yükleyebilir ve istemciye dağıtabilirsiniz. PowerShell komut dosyasını kullanmadan önce, komut dosyasındaki adımları anlamak için Azure Disk Şifreleme ön koşulları hakkında bilginiz olmalıdır. Örnek komut dosyasının ortamınız için değişiklikler ekiolabilir.

     
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

## <a name="certificate-based-authentication-and-a-kek-optional"></a><a name="bkmk_CertKEK"></a>Sertifika tabanlı kimlik doğrulama ve KEK (isteğe bağlı)

Sertifika kimlik doğrulamasını kullanmak ve şifreleme anahtarını kek ile sarmak istiyorsanız, aşağıdaki komut dosyasını örnek olarak kullanabilirsiniz. PowerShell komut dosyasını kullanmadan önce, komut dosyasındaki adımları anlamak için önceki Azure Disk Şifreleme ön koşullarının tümüne aşina olmalısınız. Örnek komut dosyasının ortamınız için değişiklikler ekiolabilir.

> [!IMPORTANT]
> Azure AD sertifikası tabanlı kimlik doğrulaması şu anda Linux VM'lerinde desteklenmez.



     
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

[Linux VM'lerde Azure AD ile Azure Disk Şifrelemesini etkinleştirme (önceki sürüm)](disk-encryption-linux-aad.md)
