---
title: Azure Hizmet Kumaşı kümesi oluşturma
description: Azure Kaynak Yöneticisi'ni kullanarak Azure'da güvenli bir Hizmet Dokusu kümesini nasıl ayarlayamanızı öğrenin.  Varsayılan şablonu kullanarak veya kendi küme şablonunuzu kullanarak bir küme oluşturabilirsiniz.
ms.topic: conceptual
ms.date: 08/16/2018
ms.openlocfilehash: 8cf14230f3abd37d91f1ec369f597ee594876100
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624122"
---
# <a name="create-a-service-fabric-cluster-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak Hizmet Dokusu kümesi oluşturma 
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure portalında](service-fabric-cluster-creation-via-portal.md)
>
>

[Azure Hizmet Kumaşı kümesi,](service-fabric-deploy-anywhere.md) mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal makineler kümesidir.  Azure'da çalışan Bir Hizmet Dokusu kümesi, Azure kaynağıdır ve Azure Kaynak Yöneticisi kullanılarak dağıtılır. Bu makalede, Kaynak Yöneticisi'ni kullanarak Azure'da güvenli bir Hizmet Dokusu kümesinin nasıl dağıtılancaya kadar dağıtılanınca açıklanmaktadır. Varsayılan küme şablonu veya özel bir şablon kullanabilirsiniz.  Zaten özel bir şablonunuzun yoksa, [nasıl oluşturulabileceğinizi öğrenebilirsiniz.](service-fabric-cluster-creation-create-template.md)

Kümenin güvenliğini sağlamak için seçilen güvenlik türü (örneğin: Windows kimliği, X509 vb.) kümenin ilk oluşturulması için belirtilmelidir ve bundan sonra değiştirilemez. Küme kurmadan önce [Service Fabric küme güvenlik senaryolarını][service-fabric-cluster-security]okuyun. Azure'da Service Fabric, kümenizi ve uç noktalarını güvence altına almak, istemcileri doğrulamak ve verileri şifrelemek için x509 sertifikasını kullanır. Azure Etkin Dizin iyönetimi bitiş noktalarına erişimi güvence altına almak için de önerilir. Daha fazla bilgi [için, istemcilerin kimliğini doğrulamak için Azure AD'yi ayarla'yı](service-fabric-cluster-creation-setup-aad.md)okuyun.

Üretim iş yüklerini çalıştırmak için bir üretim kümesi oluşturuyorsanız, önce [üretime hazırlık denetim listesini](service-fabric-production-readiness-checklist.md)okumanızı öneririz.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar 
Bu makalede, bir küme dağıtmak için Service Fabric RM powershell veya Azure CLI modüllerini kullanın:

* [Azure PowerShell 4.1 ve üzeri][azure-powershell]
* [Azure CLI sürüm 2.0 ve üzeri][azure-CLI]

Servis Kumaşı modülleri için referans belgelerini burada bulabilirsiniz:
* [Az.ServiceFabric](https://docs.microsoft.com/powershell/module/az.servicefabric)
* [az SF CLI modülü](https://docs.microsoft.com/cli/azure/sf?view=azure-cli-latest)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Bu makaledeki komutlardan herhangi birini çalıştırmadan önce önce Azure'da oturum açın.

```powershell
Connect-AzAccount
Set-AzContext -SubscriptionId <subscriptionId>
```

```azurecli
az login
az account set --subscription $subscriptionId
```

## <a name="create-a-new-cluster-using-a-system-generated-self-signed-certificate"></a>Kendi imzalı sertifika oluşturulan bir sistem kullanarak yeni bir küme oluşturma

Sistem tarafından imzalanmış bir sertifikayla güvenli bir küme oluşturmak için aşağıdaki komutları kullanın. Bu komut, küme güvenliği ve bu sertifikayı kullanarak yönetim işlemlerini gerçekleştirmek için yönetici erişimi ayarlamak için kullanılan bir birincil küme sertifikası ayarlar.  Kendi imzalı sertifikalar, test kümelerini güvence altına almak için yararlıdır.  Üretim kümeleri, sertifika yetkilisinden (CA) bir sertifika ile güvence altına alınmalıdır.

### <a name="use-the-default-cluster-template-that-ships-in-the-module"></a>Modülde bulunan varsayılan küme şablonuna sahip

Varsayılan şablonu kullanarak en az parametreyi belirterek, hızlı bir küme oluşturmak için aşağıdaki komutu kullanın.

Kullanılan şablon [Azure Hizmet Kumaşı şablon örneklerinde kullanılabilir: windows şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ve [Ubuntu şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

Aşağıdaki komut windows veya Linux kümeleri oluşturabilirsiniz, buna göre işletim sistemi belirtmeniz gerekir. PowerShell/CLI komutları da belirtilen *CertificateOutputFolder'da*sertifika çıktısı; ancak, sertifika klasörü zaten oluşturulan emin olun. Komut vm SKU gibi diğer parametreleri de alır.

> [!NOTE]
> Aşağıdaki PowerShell komutu yalnızca Azure `Az` PowerShell modülüyle çalışır. Azure Kaynak Yöneticisi PowerShell sürümünün geçerli sürümünü denetlemek için aşağıdaki PowerShell komutunu çalıştırın "Az Modülü Al". Azure Kaynak Yöneticisi PowerShell sürümünüzü yükseltmek için [bu bağlantıyı](/powershell/azure/install-Az-ps) izleyin. 
>
>

PowerShell'i kullanarak kümeyi dağıtın:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password123!@#" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword="Password4321!@#" | ConvertTo-SecureString -AsPlainText -Force
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"
$certOutputFolder="c:\certificates"

New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -OS $os -VmPassword $vmpassword -VmUserName $vmuser
```

Azure CLI'yi kullanarak kümeyi dağıtın:

```azurecli
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare CertSubjectName="mylinux.westus.cloudapp.azure.com"
declare vmpassword="Password!1"
declare certpassword="Password!4321"
declare vmuser="myadmin"
declare vmOs="UbuntuServer1604"
declare certOutputFolder="c:\certificates"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certpassword  \
    --vault-name $vaultName --vault-resource-group $resourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath --vm-os $vmOs  \
    --vm-password $vmpassword --vm-user-name $vmuser
```

### <a name="use-your-own-custom-template"></a>Kendi özel şablonunuzu kullanma

İhtiyaçlarınıza uygun özel bir şablon yazmanız gerekiyorsa, [Azure Hizmet Dokusu şablon örneklerinde](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master)bulunan şablonlardan biriyle başlamanız önerilir. Küme şablonunuzu nasıl [özelleştirileştirileştirin.][customize-your-cluster-template]

Zaten özel bir şablonunuz varsa, şablondaki ve parametre dosyasındaki üç sertifikayla ilgili tüm parametrelerin aşağıdaki gibi adlandırıldığını ve değerlerin aşağıdaki gibi geçersiz olduğunu iki kez denetleyin:

```json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```

PowerShell'i kullanarak kümeyi dağıtın:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"

New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```

Azure CLI'yi kullanarak kümeyi dağıtın:

```azurecli
declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath
```

## <a name="create-a-new-cluster-using-your-own-x509-certificate"></a>Kendi X.509 sertifikanızı kullanarak yeni bir küme oluşturun

Kümenizi güvence altına almak için kullanmak istediğiniz bir sertifikanız varsa küme oluşturmak için aşağıdaki komutu kullanın.

Bu, başka amaçlarla da kullanmak üzere olacağınız CA imzalı bir sertifikaysa, özellikle anahtar kasanız için ayrı bir kaynak grubu sağlamanız önerilir. Anahtar kasasını kendi kaynak grubuna koymanızı öneririz. Bu eylem, anahtarlarınızı ve sırlarınızı kaybetmeden Hizmet Kumaşı kümenizi içeren kaynak grubu da dahil olmak üzere bilgi işlem ve depolama kaynak gruplarını kaldırmanızı sağlar. **Anahtar kasanızı içeren kaynak grubu, onu kullanan *kümeyle aynı bölgede olmalıdır.***

### <a name="use-the-default-five-node-one-node-type-template-that-ships-in-the-module"></a>Varsayılan beş düğüm, modülde gemi bir düğüm türü şablonu kullanın
Kullanılan şablon Azure örneklerinde kullanılabilir [: Windows şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) ve [Ubuntu şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Ubuntu-1-NodeTypes-Secure)

PowerShell'i kullanarak kümeyi dağıtın:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$vmpassword=("Password!4321" | ConvertTo-SecureString -AsPlainText -Force) 
$vmuser="myadmin"
$os="WindowsServer2016DatacenterwithContainers"

New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -KeyVaultResourceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile C:\MyCertificates\chackocertificate3.pfx -CertificatePassword $certPassword -OS $os -VmPassword $vmpassword -VmUserName $vmuser 
```

Azure CLI'yi kullanarak kümeyi dağıtın:

```azurecli
declare vmPassword="Password!1"
declare certPassword="Password!1"
declare vmUser="myadmin"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare certificate-file="c:\certificates\mycert.pem"
declare vmOs="UbuntuServer1604"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $certPassword  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --vm-os vmOs \
    --vm-password $vmPassword --vm-user-name $vmUser
```

### <a name="use-your-own-custom-cluster-template"></a>Kendi özel küme şablonunuzu kullanma
İhtiyaçlarınıza uygun özel bir şablon yazmanız gerekiyorsa, [Azure Hizmet Dokusu şablon örneklerinde](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master)bulunan şablonlardan biriyle başlamanız önerilir. Küme şablonunuzu nasıl [özelleştirileştirileştirin.][customize-your-cluster-template]

Zaten özel bir şablonunuz varsa, şablondaki ve parametre dosyasındaki üç sertifikayla ilgili tüm parametrelerin aşağıdaki gibi adlandırıldığını ve değerlerin aşağıdaki gibi geçersiz olduğunu iki kez kontrol ettiğinizden emin olun.

```json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```

PowerShell'i kullanarak kümeyi dağıtın:

```powershell
$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$vaultName="myvault"
$vaultResourceGroupName="myvaultrg"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$os="WindowsServer2016DatacenterwithContainers"
$parameterFilePath="c:\mytemplates\mytemplateparm.json"
$templateFilePath="c:\mytemplates\mytemplate.json"
$certificateFile="C:\MyCertificates\chackonewcertificate3.pem"

New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -Location $resourceGroupLocation -TemplateFile $templateFilePath -ParameterFile $parameterFilePath -KeyVaultResourceGroupName $vaultResourceGroupName -KeyVaultName $vaultName -CertificateFile $certificateFile -CertificatePassword $certPassword
```

Azure CLI'yi kullanarak kümeyi dağıtın:

```azurecli
declare certPassword="Password!1"
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare vaultResourceGroupName="myvaultrg"
declare vaultName="myvault"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-file $certificate-file --certificate-password $password  \
    --vault-name $vaultName --vault-resource-group $vaultResourceGroupName  \
    --template-file $templateFilePath --parameter-file $parametersFilePath 
```

### <a name="use-a-pointer-to-a-secret-uploaded-into-a-key-vault"></a>Anahtar kasasına yüklenen gizli bir işaretçi kullanma

Varolan bir anahtar kasasını kullanmak için, bilgi işlem kaynak sağlayıcısının ondan sertifika almasına ve küme düğümlerine yüklemesine izin vermek için dağıtım için anahtar kasasının [etkinleştirilmesi](../key-vault/key-vault-manage-with-cli2.md#bkmk_KVperCLI) gerekir.

PowerShell'i kullanarak kümeyi dağıtın:

```powershell
Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

$parameterFilePath="c:\mytemplates\mytemplate.json"
$templateFilePath="c:\mytemplates\mytemplateparm.json"
$secretID="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

New-AzServiceFabricCluster -ResourceGroupName $resourceGroupName -SecretIdentifier $secretID -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 
```

Azure CLI'yi kullanarak kümeyi dağıtın:

```azurecli
declare $resourceGroupName = "testRG"
declare $parameterFilePath="c:\mytemplates\mytemplate.json"
declare $templateFilePath="c:\mytemplates\mytemplateparm.json"
declare $secretID="https://test1.vault.azure.net:443/secrets/testcertificate4/55ec7c4dc61a462bbc645ffc9b4b225f"

az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --secret-identifier $secretID  \
    --template-file $templateFilePath --parameter-file $parameterFilePath 
```

## <a name="next-steps"></a>Sonraki adımlar
Bu noktada, Azure'da çalışan güvenli bir kümeniz olur. Ardından, [kümenize bağlanın](service-fabric-connect-to-secure-cluster.md) ve uygulama sırlarını nasıl [yönetiniz](service-fabric-application-secret-management.md)gerektiğini öğrenin.

Şablon kullanmak için JSON sözdizimi ve özellikleri için [Microsoft.ServiceFabric/clusters şablon başvurusuna](/azure/templates/microsoft.servicefabric/clusters)bakın.

<!-- Links -->
[azure-powershell]:https://docs.microsoft.com/powershell/azure/install-Az-ps
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[customize-your-cluster-template]: service-fabric-cluster-creation-create-template.md
