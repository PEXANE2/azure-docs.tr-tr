---
title: Azure Anahtar Kasası sertifikası oluşturma | Azure Marketi
description: Azure tarafından dağıtılan bir VHD'den vm'nin nasıl kaydedildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: dc106ecf40367172f28c0f339bbcce8bddb50a34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278084"
---
# <a name="create-certificates-for-azure-key-vault"></a>Azure Anahtar Kasası için sertifikalar oluşturun

Bu makalede, Azure tarafından barındırılan bir sanal makineye (VM) Windows Uzaktan Yönetimi (WinRM) bağlantısı oluşturmak için gereken kendi imzalı sertifikaların nasıl sağlandığı açıklanmaktadır. Bu işlem üç adımdan oluşur:

1.  Güvenlik sertifikasını oluşturun. 
2.  Bu sertifikayı depolamak için Azure Anahtar Kasası'nı oluşturun. 
3.  Sertifikaları bu anahtar kasasına saklayın. 

Bu çalışma için yeni veya varolan bir Azure kaynak grubu kullanabilirsiniz.  Eski yaklaşım aşağıdaki açıklama kullanılır.



[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="create-the-certificate"></a>Sertifikayı oluşturma

Sertifika dosyasını (.pfx) yerel bir klasörde oluşturmak için aşağıdaki Azure Powershell komut dosyasını edin ve çalıştırın.  Aşağıdaki parametreler için değerleri değiştirmeniz gerekir:

|  **Parametre**        |   **Açıklama**                                                               |
|  -------------        |   ---------------                                                               |
| `$certroopath` | .pfx dosyasını kaydetmek için yerel klasör  |
| `$location`    | Azure standart coğrafi konumlardan biri  |
| `$vmName`      | Planlanan Azure sanal makinesinin adı   |
| `$certname`    | Sertifikanın adı; planlanan VM tam nitelikli etki alanı adı maç gerekir  |
| `$certpassword` | Sertifikalar için parola, planlanan VM için kullanılan şifre eşleşmelidir  |
|  |  |

```powershell
    # Certification creation script 
    
    # pfx certification stored path
    $certroopath = "C:\certLocation"
    
    #location of the resource group
    $location = "westus"
    
    # Azure virtual machine name that we are going to create
    $vmName = "testvm000000906"
    
    # Certification name - should match with FQDN of Windows Azure creating VM 
    $certname = "$vmName.$location.cloudapp.azure.com"
    
    # Certification password - should be match with password of Windows Azure creating VM 
    $certpassword = "SecretPassword@123"
    
    $cert=New-SelfSignedCertificate -DnsName "$certname" -CertStoreLocation cert:\LocalMachine\My
    $pwd = ConvertTo-SecureString -String $certpassword -Force -AsPlainText
    $certwithThumb="cert:\localMachine\my\"+$cert.Thumbprint
    $filepath="$certroopath\$certname.pfx"
    Export-PfxCertificate -cert $certwithThumb -FilePath $filepath -Password $pwd
    Remove-Item -Path $certwithThumb 

```
> [!TIP]
> Çeşitli parametrelerin değerleri korunacak şekilde bu adımlar sırasında aynı PowerShell konsol oturumunu etkin tutun.

> [!WARNING]
> Bu komut dosyalarını kaydederseniz, yalnızca güvenli bir konumda saklayın, çünkü güvenlik bilgileri (parola) içerir.


## <a name="create-the-key-vault"></a>Anahtar kasasını oluşturma

[Anahtar kasa dağıtım şablonunun](./cpp-key-vault-deploy-template.md) içeriğini yerel makinenizdeki bir dosyaya kopyalayın. (aşağıdaki örnek komut dosyasında, `C:\certLocation\keyvault.json`bu kaynak .)  Azure Anahtar Kasası örneği ve ilişkili kaynak grubu oluşturmak için aşağıdaki Azure Powershell komut dosyasını oluşturun ve çalıştırın.  Aşağıdaki parametreler için değerleri değiştirmeniz gerekir:

|  **Parametre**        |   **Açıklama**                                                               |
|  -------------        |   ---------------                                                               |
| `$postfix`            | Dağıtım tanımlayıcılarına eklenen rasgele sayısal dize                     |
| `$rgName`             | Oluşturmak için Azure kaynak grubu (RG) adı                                        |
|  `$location`          | Azure standart coğrafi konumlardan biri                                  |
| `$kvTemplateJson`     | Anahtar kasası için Kaynak Yöneticisi şablonu içeren dosya yolu (keyvault.json) |
| `$kvname`             | Yeni anahtar kasasının adı                                                       |
|  |  |

```powershell
    # Creating Key vault in resource group
    
    # "Random" number for deployment identifiers
    $postfix = "0101048"
    
    # Resource group name
    $rgName = "TestRG$postfix"
    
    # Location of Resource Group
    $location = "westus"
    
    # Key vault template location
    $kvTemplateJson = "C:\certLocation\keyvault.json"
    
    # Key vault name
    $kvname = "isvkv$postfix"
    
    # code snippet to get the Azure user object ID
    try
       {
        $accounts = Get-AzureAccount
        $accountNum = 0
        $accounts.Id | %{ ++$accountNum; Write-Host $accountNum $_}
        Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
        [Int] $accountChoice = Read-Host 
        
        While($accountChoice -lt 1 -or $accountChoice -gt $accounts.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red 
            Write-Host "`nPlease select User, e.g. 1:" -ForegroundColor DarkYellow
            [Int] $accountChoice = Read-Host
        }
        
        $accountSelected = $accounts[$accountChoice-1]
        echo $accountSelected
        $id = $accountSelected.Id
                              
        Write-Host "User $id Selected"
        $myobjectId=(Get-AzADUser -Mail $id)[0].Id
      }
      catch
      {
      Write-Host $_.Exception.Message    
      Break
      } 

    # code snippet to get Azure User Tenant Id
      # SELECT Subscriptions
        #**************************************
        try
        {
        $subslist=Get-AzureSubscription
        for($i=1; $i -le $subslist.Length;$i++)
        {
           Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
        }
        Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
        [int] $selectedsub=Read-Host
    
        While($selectedsub -lt 1 -or $selectedsub -gt $subslist.Length)
        {
            Write-Host "incorrect input" -ForegroundColor Red 
            for($i=1; $i -le $subslist.Length;$i++)
             {
              Write-Host ($i.ToString() +":"+ $subslist[$i-1].SubscriptionName)
             }
            Write-Host "`nPlease pick subscription from above, e.g. 1:" -ForegroundColor DarkYellow
           [int] $selectedsub=Read-Host
        }
        if($selectedsub -ge 1 -and $selectedsub -le $subslist.Length)
        {
        $mysubid=$subslist[$selectedsub-1].SubscriptionId
        $mysubName=$subslist[$selectedsub-1].SubscriptionName
        $mytenantId=$subslist[$selectedsub-1].TenantId
        Write-Host "$mysubName selected"
        }
        }
        catch
        {
        Write-Host $_.Exception.Message    
        Break
        }
    
    # Create a resource group
     Write-Host "Creating Resource Group $rgName"
     Create-ResourceGroup -rgName $rgName -location $location
     Write-Host "-----------------------------------" 
    
    # Create key vault and configure access
    New-AzResourceGroupDeployment -Name "kvdeploy$postfix" -ResourceGroupName $rgName -TemplateFile $kvTemplateJson -keyVaultName $kvname -tenantId $mytenantId -objectId $myobjectId
    
    Set-AzKeyVaultAccessPolicy -VaultName $kvname -ObjectId $myobjectId -PermissionsToKeys all -PermissionsToSecrets all 
        
```

## <a name="store-the-certificate"></a>Sertifikayı depolama

Artık .pfx dosyasında bulunan sertifikaları aşağıdaki komut dosyasını çalıştırarak yeni anahtar kasasına saklayabilirsiniz. 

```powershell
    #push certificate to key vault secret

     $fileName =$certroopath+"\$certname"+".pfx" 
      
     $fileContentBytes = get-content $fileName -Encoding Byte
     $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    
            $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certpassword"
    }
"@
            echo $certpassword
            $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
            $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
            $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
            $objAzureKeyVaultSecret=Set-AzureKeyVaultSecret -VaultName $kvname -Name "ISVSecret$postfix" -SecretValue $secret
            echo $objAzureKeyVaultSecret.Id 
    
```


## <a name="next-steps"></a>Sonraki adımlar

Daha sonra [kullanıcı VM görüntünüzden bir VM dağıtırsınız.](./cpp-deploy-vm-user-image.md)
