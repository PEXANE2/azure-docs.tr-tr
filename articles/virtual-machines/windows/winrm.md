---
title: Azure VM için WinRM erişimi ayarlama
description: Kaynak Yöneticisi dağıtım modelinde oluşturulan bir Azure sanal makinesiyle kullanım için WinRM erişimi kurulum.
services: virtual-machines-windows
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 9718e85b-d360-4621-90b8-0b0b84a21208
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 06/16/2016
ms.author: mimckitt
ms.openlocfilehash: 317e9376e0b8242758cd6e3f455b3f3dc9c0dc78
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879573"
---
# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Azure Kaynak Yöneticisi'nde Sanal Makineler için WinRM erişimi ayarlama

WinRM bağlantısıyla bir VM ayarlamak için atmanız gereken adımlar şunlardır:

1. Anahtar kasası oluşturma
2. Otomatik olarak imzalanan sertifika oluşturma
3. İmzalı sertifikanızı Key Vault'a yükleyin
4. Anahtar Kasası'nda kendi imzalı sertifikanızın URL'sini alın
5. VM oluştururken kendi imzalı sertifikaURL'nize başvurun

 

## <a name="step-1-create-a-key-vault"></a>Adım 1: Anahtar Kasası Oluşturma
Anahtar Kasası oluşturmak için aşağıdaki komutu kullanabilirsiniz

```
New-AzKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Adım 2: Kendi imzalı sertifika oluşturma
Bu PowerShell komut dosyasını kullanarak kendi imzalı bir sertifika oluşturabilirsiniz

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Adım 3: Kendi imzaladığınız sertifikanızı Anahtar Kasasına yükleyin
Sertifikayı adım 1'de oluşturulan Anahtar Kasası'na yüklemeden önce, Microsoft.Compute kaynak sağlayıcısının anlayacağı bir biçime dönüştürülmesi gerekir. Aşağıdaki PowerShell komut dosyası bunu yapmanıza olanak sağlar

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Adım 4: Anahtar Kasası'nda kendi imzalı sertifikanızın URL'sini alın
Microsoft.Compute kaynak sağlayıcısı, VM'yi karşılarken Key Vault'un içindeki gizli url'ye ihtiyaç duyar. Bu, Microsoft.Compute kaynak sağlayıcısının sırrı karşıdan yüklemesini ve VM'de eşdeğer sertifika oluşturmasını sağlar.

> [!NOTE]
> Gizli URL de sürümü içermelidir. Örnek bir URL aşağıda\/görünüyor https: /contosovault.vault.azure.net:443/secrets/contososecret/01h9db0df2cd4300a20ence585a6s7ve

#### <a name="templates"></a>Şablonlar
Aşağıdaki kodu kullanarak şablondaki URL bağlantısını alabilirsiniz

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>PowerShell
Aşağıdaki PowerShell komutunu kullanarak bu URL'yi alabilirsiniz

    $secretURL = (Get-AzKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Adım 5: VM oluştururken kendi imzaladığınız sertifikaURL'nize başvurun
#### <a name="azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi Şablonları
Şablonlar aracılığıyla bir VM oluştururken, sertifika aşağıdaki gibi sırlar bölümünde ve winRM bölümünde başvurulan alır:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Yukarıdaki ler için örnek bir şablon burada bulunabilir [201-vm-winrm-keyvault-windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Bu şablonun kaynak kodu [GitHub'da](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows) bulunabilir

#### <a name="powershell"></a>PowerShell
    $vm = New-AzVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -ProvisionVMAgent -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Adım 6: VM'ye bağlanma
VM'ye bağlanmadan önce makinenizin WinRM uzaktan yönetimi için yapılandırıldığından emin olmanız gerekir. PowerShell'i yönetici olarak başlatın ve ayarlı olduğunuzdan emin olmak için aşağıdaki komutu uygulayın.

    Enable-PSRemoting -Force

> [!NOTE]
> Yukarıdaki ler işe yaramazsa WinRM hizmetinin çalıştığından emin olmanız gerekebilir. Bunu kullanarak yapabilirsiniz`Get-Service WinRM`
> 
> 

Kurulum yapıldıktan sonra aşağıdaki komutu kullanarak VM'ye bağlanabilirsiniz

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate
