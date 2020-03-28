---
title: "Öğretici: Azure'da TLS/SSL sertifikalarıyla bir Windows web sunucusunun güvenliğini sağlama"
description: Bu eğitimde, Azure Key Vault'ta depolanan TLS/SSL sertifikalarıyla IIS web sunucusunu çalıştıran bir Windows sanal makinesini korumak için Azure PowerShell'i nasıl kullanacağınızı öğreneceksiniz.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 5b084f8a226d1cfd5bab2cc81512fb51fa6bf41c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80154296"
---
# <a name="tutorial-secure-a-web-server-on-a-windows-virtual-machine-in-azure-with-tlsssl-certificates-stored-in-key-vault"></a>Öğretici: Key Vault'ta depolanan TLS/SSL sertifikalarıyla Azure'daki bir Windows sanal makinesinde bir web sunucusunun güvenliğini sağlama

> [!NOTE]
> Şu anda bu doküman yalnızca Genelleştirilmiş görüntüler için çalışır. Özel bir disk kullanarak bu öğretici deneiyorsanız bir hata alırsınız. 

Web sunucularını güvenli hale getirmek için, daha önce Güvenli Soketkatmanı (SSL) olarak bilinen bir Aktarım Katmanı Güvenliği (TLS), web trafiğini şifrelemek için sertifika kullanılabilir. Bu TLS/SSL sertifikaları Azure Key Vault'ta depolanabilir ve Azure'da Windows sanal makinelerine (VM) güvenli sertifika dağıtımına izin verir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Key Vault oluşturma
> * Sertifikaları oluşturma veya Key Vault’a yükleme
> * VM oluşturma ve ISS web sunucusunu yükleme
> * Sertifikayı VM'ye enjekte edin ve IIS'yi TLS bağlama ile yapılandırın


## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/powershell](https://shell.azure.com/powershell)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.


## <a name="overview"></a>Genel Bakış
Azure Key Vault, sertifikalar veya pasaportlar gibi şifreleme anahtarları ile gizli dizilerin güvenliğini sağlar. Key Vault, sertifika yönetimi işlemini kolaylaştırır ve bu sertifikalara erişen anahtarları denetiminizde tutmanıza olanak sağlar. Key Vault içinde otomatik olarak imzalanan bir sertifika oluşturabilir veya sahip olduğunuz güvenli bir sertifikayı karşıya yükleyebilirsiniz.

Oluşturulan sertifikaları içeren özel bir VM görüntüsü kullanmak yerine, sertifikaları çalışan bir VM’ye eklersiniz. Bu işlem, dağıtım sırasında web sunucusuna en güncel sertifikaların yüklenip yüklenmediğini kontrol eder. Ayrıca, bir sertifikayı yeniler veya değiştirirseniz yeni ve özel bir VM görüntüsü oluşturmanız da gerekmez. En güncel sertifikalar, siz ek VM oluşturdukça otomatik olarak eklenir. Bu işlem sırasında, sertifikalar Azure platformundan ayrılmaz veya bir betikte, komut satırı geçmişinde veya şablonda kullanıma sunulmaz.


## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma
Bir Anahtar Kasası ve sertifikalar oluşturmadan önce, [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek *Doğu ABD* konumunda *myResourceGroupSecureWeb* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Ardından, [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault)ile bir Anahtar Vault oluşturun. Her Key Vault benzersiz bir ad gerektirir ve küçük harflerle yazılmalıdır. Aşağıdaki örnekte yer alan `mykeyvault` değerini, kendi benzersiz Key Vault adınızla değiştirin:

```azurepowershell-interactive
$keyvaultName="mykeyvault"
New-AzKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Sertifika oluşturma ve sertifikayı Key Vault’ta depolama
Üretim kullanımı [için, Içe Aktar-AzKeyVaultCertificate](https://docs.microsoft.com/powershell/module/az.keyvault/import-azkeyvaultcertificate)ile güvenilir sağlayıcı tarafından imzalanmış geçerli bir sertifika almanız gerekir. Bu öğretici için aşağıdaki örnek, [New-AzKeyVaultCertificatePolicy'nin](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvaultcertificatepolicy)varsayılan sertifika ilkesini kullanan [Add-AzKeyVaultCertificate](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultcertificate) ile kendi imzalı bir sertifikayı nasıl oluşturabileceğinizi gösterir. 

```azurepowershell-interactive
$policy = New-AzKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Şimdi [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm)ile VM oluşturabilirsiniz. Aşağıdaki örnekte *EastUS* konumunda *myVM* adlı bir VM oluşturulur. Mevcut değilse, destekleyici ağ kaynakları oluşturulur. Güvenli web trafiği sağlanması için, cmdlet ayrıca *443* numaralı bağlantı noktasını açar.

```azurepowershell-interactive
# Create a VM
New-AzVm `
    -ResourceGroupName $resourceGroup `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred `
    -OpenPorts 443

# Use the Custom Script Extension to install IIS
Set-AzVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

VM’nin oluşturulması birkaç dakika sürer. Son adım, IIS web sunucusunu [Set-AzVmExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension)ile yüklemek için Azure Özel Komut Dosyası Uzantısı'nı kullanır.


## <a name="add-a-certificate-to-vm-from-key-vault"></a>Key Vault’tan VM'ye sertifika ekleme
Key Vault'tan VM'ye sertifika eklemek için [Get-AzKeyVaultSecret](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvaultsecret)ile sertifikanızın kimliğini alın. [Add-AzVMSecret](https://docs.microsoft.com/powershell/module/az.compute/add-azvmsecret)ile VM'ye sertifika ekleyin:

```azurepowershell-interactive
$certURL=(Get-AzKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a>Sertifikayı kullanmak üzere IIS'yi yapılandırma
IIS yapılandırmasını güncelleştirmek için [Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) ile Özel Komut Dosyası Uzantısı'nı yeniden kullanın. Bu güncelleştirme, Key Vault’tan IIS’ye eklenen sertifikayı uygular ve web bağlamasını yapılandırır:

```azurepowershell-interactive
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-the-secure-web-app"></a>Güvenli web uygulamasını sınama
[Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress)ile VM'nizin genel IP adresini edinin. Aşağıdaki örnek, daha önce oluşturulan `myPublicIP` için IP adresini alır:

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIPAddress" | select "IpAddress"
```

Artık bir web tarayıcısı açıp adres çubuğuna `https://<myPublicIP>` ifadesini girebilirsiniz. Otomatik olarak imzalanan sertifika kullandıysanız güvenlik uyarısını kabul etmek için, **Ayrıntılar**’ı seçin ve sonra **Web sayfasına gidin**:

![Web tarayıcısı güvenlik uyarısını kabul edin](./media/tutorial-secure-web-server/browser-warning.png)

Güvenli IIS siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

![Çalışan güvenli IIS sitesini görüntüleme](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, Azure Key Vault'ta depolanan TLS/SSL sertifikasına sahip bir IIS web sunucusu nun güvenliğini sağladınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure Key Vault oluşturma
> * Sertifikaları oluşturma veya Key Vault’a yükleme
> * VM oluşturma ve ISS web sunucusunu yükleme
> * Sertifikayı VM'ye enjekte edin ve IIS'yi TLS bağlama ile yapılandırın

Hazır sanal makine betik örneklerini görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Windows sanal makinesi betik örnekleri](./powershell-samples.md)
