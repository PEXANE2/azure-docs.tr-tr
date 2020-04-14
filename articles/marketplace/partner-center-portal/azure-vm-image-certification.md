---
title: Azure sanal makine sertifikası - Azure Marketi
description: Ticari pazarda sanal makine teklifini nasıl test edip göndereceğinizi öğrenin.
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 009a8e3db097790788f71486431a3b5b05c488ea
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81265973"
---
# <a name="azure-virtual-machine-vm-image-certification"></a>Azure sanal makine (VM) görüntü sertifikası

> [!NOTE]
> Azure VM tekliflerinizin yönetimini Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin karşısına geçirilene kadar, tekliflerinizi yönetmek için lütfen Bulut İş Ortağı Portalı'nda [Azure Anahtar Kasası için sertifika Oluştur'daki](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-key-vault-cert) yönergeleri izlemeye devam edin.

Bu makalede, en son Azure Marketi yayımlama gereksinimlerini karşıladığından emin olmak için ticari pazarda sanal makine (VM) görüntüsünün nasıl test edilip gönderilen ler açıklanmaktadır.

VM teklifinizi göndermeden önce şu adımları tamamlayın:

1. Sertifikalar oluşturun ve dağıtın.
2. Genelleştirilmiş resminizi kullanarak bir Azure VM dağıtın.
3. Geçerlilikleri çalıştırın.

## <a name="create-and-deploy-certificates-for-azure-key-vault"></a>Azure Anahtar Kasası için sertifika oluşturma ve dağıtma

Bu bölümde, Windows Remote Management (WinRM) bağlantısı nın Azure tarafından barındırılan bir sanal makineye ayarlanması için gereken kendi imzalı sertifikaların nasıl oluşturulup dağıtılacaklandığı açıklanmaktadır.

### <a name="create-certificates-for-azure-key-vault"></a>Azure Anahtar Kasası için sertifikalar oluşturun

Bu işlem üç adımdan oluşur:

1. Güvenlik sertifikasını oluşturun.
2. Sertifikayı depolamak için Azure Anahtar Kasası'nı oluşturun.
3. Sertifikaları anahtar kasasına saklayın.

Bu çalışma için yeni veya varolan bir Azure kaynak grubu kullanabilirsiniz.

#### <a name="create-the-security-certificate"></a>Güvenlik sertifikası oluşturma

Sertifika dosyasını (.pfx) yerel bir klasörde oluşturmak için aşağıdaki Azure PowerShell komut dosyasını edin ve çalıştırın. Aşağıdaki tabloda gösterilen parametrelerin değerlerini değiştirin.

| **Parametre** | **Açıklama** |
| --- | --- |
| $certroopath | .pfx dosyasını kaydetmek için yerel klasör. |
| $location | Azure standart coğrafi konumlardan biri. |
| $vmName | Planlanan Azure sanal makinesinin adı. |
| $certname | Sertifikanın adı; planlanan VM'nin tam nitelikli alan adı ile eşleşmelidir. |
| $certpassword | Sertifikalar için parola, planlanan VM için kullanılan parola eşleşmelidir. |
| | |

```PowerShell
   # Certification creation script

    # pfx certification stored path
    $certroopath = "C:\certLocation"

    # location of the resource group
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
> Çeşitli parametrelerin değerlerini korumak için aynı Azure PowerShell konsol oturumunu bu adımlar sırasında açık ve çalışır durumda tutun.

> [!WARNING]
> Bu komut dosyalarını kaydederseniz, yalnızca güvenlik bilgileri (parola) içerdiğinden güvenli bir konuma kaydedin.

#### <a name="create-the-azure-key-vault-to-store-the-certificate"></a>Sertifikayı depolamak için Azure anahtar kasasını oluşturun

Aşağıdaki şablonun içeriğini yerel makinenizdeki bir dosyaya kopyalayın. Aşağıdaki örnek komut dosyasında, `C:\certLocation\keyvault.json`bu kaynak .

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "keyVaultName": {
      "type": "string",
      "defaultValue":"isvkv0001",
      "metadata": {
        "description": "Name of the Vault"
      }
    },
    "tenantId": {
      "type": "string",
      "defaultValue":"72f988bf-86f1-41af-91ab-2d7cd011db47",
      "metadata": {
        "description": "Tenant Id of the subscription. Get using Get-AzureSubscription cmdlet or Get Subscription API"
      }
    },
    "objectId": {
      "type": "string",
      "defaultValue":"d55739bf-d5d6-4ce0-be1c-49ade53c4315",
      "metadata": {
        "description": "Object Id of the AD user. Get using Get-AzureADUser or Get-AzureADServicePrincipal cmdlets"
      }
    },
    "keysPermissions": {
      "type": "array",
      "defaultValue": ["all"],
      "metadata": {
        "description": "Permissions to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
      }
    },
    "secretsPermissions": {
      "type": "array",
      "defaultValue": ["all"],
      "metadata": {
        "description": "Permissions to secrets in the vault. Valid values are: all, get, set, list, and delete."
      }
    },
    "skuName": {
      "type": "string",
      "defaultValue": "Standard",
      "allowedValues": [
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "SKU for the vault"
      }
    },
    "enableVaultForDeployment": {
      "type": "bool",
      "defaultValue": true,
      "allowedValues": [
        true,
        false
      ],
      "metadata": {
        "description": "Specifies if the vault is enabled for a VM deployment"
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "[parameters('keyVaultName')]",
      "apiVersion": "2015-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "enabledForDeployment": "[parameters('enableVaultForDeployment')]",
        "tenantId": "[parameters('tenantId')]",
        "accessPolicies": [
          {
            "tenantId": "[parameters('tenantId')]",
            "objectId": "[parameters('objectId')]",
            "permissions": {
              "keys": "[parameters('keysPermissions')]",
              "secrets": "[parameters('secretsPermissions')]"
            }
          }
        ],
        "sku": {
          "name": "[parameters('skuName')]",
          "family": "A"
        }
      }
    }
  ]
}

```

Azure Anahtar Kasası ve ilişkili kaynak grubu oluşturmak için aşağıdaki Azure PowerShell komut dosyasını oluşturun ve çalıştırın. Aşağıdaki tabloda gösterilen parametrelerin değerlerini değiştirin

| **Parametre** | **Açıklama** |
| --- | --- |
| $postfix | Dağıtım tanımlayıcılarına bağlı rasgele sayısal dize. |
| $rgName | Azure kaynak grubu (RG) adı oluşturmak için. |
| $location | Azure standart coğrafi konumlardan biri. |
| $kvTemplateJson | Anahtar kasası için Kaynak Yöneticisi şablonu içeren dosya yolu (keyvault.json). |
| $kvname | Yeni anahtar kasasının adı.|
|   |   |

```PowerShell
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

#### <a name="store-the-certificates-to-the-key-vault"></a>Sertifikaları anahtar kasasına saklayın

.pfx dosyasında yer alan sertifikaları bu komut dosyasını kullanarak yeni anahtar kasasına saklayın:

```PowerShell
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

## <a name="deploy-an-azure-vm-using-your-generalized-image"></a>Genelleştirilmiş resminizi kullanarak bir Azure VM dağıtma

Bu bölümde, yeni bir Azure VM kaynağı oluşturmak için genelleştirilmiş bir VHD görüntüsünün nasıl dağıtılanılacağa açıklanmaktadır. Bu işlem için, sağlanan Azure Kaynak Yöneticisi şablonu ve Azure PowerShell komut dosyasını kullanırız.

### <a name="prepare-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu hazırlama

VHD dağıtımı için aşağıdaki Azure Kaynak Yöneticisi şablonuna VHDtoImage.json adlı yerel bir dosyayı kopyalayın. Sonraki komut dosyası, bu JSON'ı kullanmak için yerel makinedeki konumu talep edecektir.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "userStorageAccountName": {
            "type": "string"
        },
        "userStorageContainerName": {
            "type": "string",
            "defaultValue": "vhds"
        },
        "dnsNameForPublicIP": {
            "type": "string"
        },
        "adminUserName": {
            "defaultValue": "isv",
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring",
            "defaultValue": "Password@123"
        },
        "osType": {
            "type": "string",
            "defaultValue": "windows",
            "allowedValues": [
                "windows",
                "linux"
            ]
        },
        "subscriptionId": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "vmSize": {
            "type": "string"
        },
        "publicIPAddressName": {
            "type": "string"
        },
        "vmName": {
            "type": "string"
        },
        "virtualNetworkName": {
            "type": "string"
        },
        "nicName": {
            "type": "string"
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "Name of the KeyVault"
            }
        },
        "vaultResourceGroup": {
            "type": "string",
            "metadata": {
                "description": "Resource Group of the KeyVault"
            }
        },
        "certificateUrl": {
            "type": "string",
            "metadata": {
                "description": "Url of the certificate with version in KeyVault e.g. https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7"
            }
        },
        "vhdUrl": {
            "type": "string",
            "metadata": {
                "description": "VHD Url..."
            }
        }
    },
        "variables": {
            "addressPrefix": "10.0.0.0/16",
            "subnet1Name": "Subnet-1",
            "subnet2Name": "Subnet-2",
            "subnet1Prefix": "10.0.0.0/24",
            "subnet2Prefix": "10.0.1.0/24",
            "publicIPAddressType": "Dynamic",
            "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
            "subnet1Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
            "osDiskVhdName": "[concat('http://',parameters('userStorageAccountName'),'.blob.core.windows.net/',parameters('userStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
        },
        "resources": [
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/publicIPAddresses",
                "name": "[parameters('publicIPAddressName')]",
                "location": "[parameters('location')]",
                "properties": {
                    "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                    "dnsSettings": {
                        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
                    }
                }
            },
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/virtualNetworks",
                "name": "[parameters('virtualNetworkName')]",
                "location": "[parameters('location')]",
                "properties": {
                    "addressSpace": {
                        "addressPrefixes": [
                            "[variables('addressPrefix')]"
                        ]
                    },
                    "subnets": [
                        {
                            "name": "[variables('subnet1Name')]",
                            "properties": {
                                "addressPrefix": "[variables('subnet1Prefix')]"
                            }
                        },
                        {
                            "name": "[variables('subnet2Name')]",
                            "properties": {
                                "addressPrefix": "[variables('subnet2Prefix')]"
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2015-05-01-preview",
                "type": "Microsoft.Network/networkInterfaces",
                "name": "[parameters('nicName')]",
                "location": "[parameters('location')]",
                "dependsOn": [
                    "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
                    "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "ipConfigurations": [
                        {
                            "name": "ipconfig1",
                            "properties": {
                                "privateIPAllocationMethod": "Dynamic",
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                                },
                                "subnet": {
                                    "id": "[variables('subnet1Ref')]"
                                }
                            }
                        }
                    ]
                }
            },
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Compute/virtualMachines",
                "name": "[parameters('vmName')]",
                "location": "[parameters('location')]",
                "dependsOn": [
                    "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
                ],
                "properties": {
                    "hardwareProfile": {
                        "vmSize": "[parameters('vmSize')]"
                    },
                    "osProfile": {
                        "computername": "[parameters('vmName')]",
                        "adminUsername": "[parameters('adminUsername')]",
                        "adminPassword": "[parameters('adminPassword')]",
                        "secrets": [
                            {
                                "sourceVault": {
                                    "id": "[resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                                },
                                "vaultCertificates": [
                                    {
                                        "certificateUrl": "[parameters('certificateUrl')]",
                                        "certificateStore": "My"
                                    }
                                ]
                            }
                        ],
                        "windowsConfiguration": {
                            "provisionVMAgent": "true",
                            "winRM": {
                                "listeners": [
                                    {
                                        "protocol": "http"
                                    },
                                    {
                                        "protocol": "https",
                                        "certificateUrl": "[parameters('certificateUrl')]"
                                    }
                                ]
                            },
                            "enableAutomaticUpdates": "true"
                        }
                    },
                    "storageProfile": {
                        "osDisk": {
                            "name": "[concat(parameters('vmName'),'-osDisk')]",
                            "osType": "[parameters('osType')]",
                            "caching": "ReadWrite",
                            "image": {
                                "uri": "[parameters('vhdUrl')]"
                            },
                            "vhd": {
                                "uri": "[variables('osDiskVhdName')]"
                            },
                            "createOption": "FromImage"
                        }
                    },
                    "networkProfile": {
                        "networkInterfaces": [
                            {
                                "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                            }
                        ]
                    },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true,
                        "storageUri": "[concat('http://', parameters('userStorageAccountName'), '.blob.core.windows.net')]"
                    }
                }
                }
            }
        ]
    }

```

Bu parametreler için değerler sağlamak için bu dosyayı edin:

| **Parametre** | **Açıklama** |
| --- | --- |
| ResourceGroupName | Varolan Azure kaynak grubu adı. Genellikle, anahtar kasanızla aynı RG'yi kullanın. |
| Templatefile | VHDtoImage.json dosyasına tam yol adı. |
| userStorageAccountName | Depolama hesabının adı. |
| sNameForPublicIP | Genel IP için DNS adı; küçük olmalıdır. |
| subscriptionId | Azure abonelik tanımlayıcısı. |
| Konum | Kaynak grubunun standart Azure coğrafi konumu. |
| vmName | Sanal makinenin adı. |
| vaultName | Anahtar kasasının adı. |
| vaultResourceGroup | Anahtar kasasının kaynak grubu. |
| sertifikaUrl | Anahtar kasasında depolanan sürüm de dahil olmak üzere sertifikanın web `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7`adresi (URL) örneğin: . |
| vhdUrl | Sanal sabit diskin web adresi. |
| vmSize | Sanal makine örneğinin boyutu. |
| publicIPAddressName | Genel IP adresinin adı. |
| virtualNetworkName | Sanal ağın adı. |
| nicName | Sanal ağ için ağ arabirim kartının adı. |
| adminUserName | Yönetici hesabının kullanıcı adı. |
| adminPassword | Yönetici parolası. |
|   |   |

### <a name="deploy-an-azure-vm"></a>Azure VM dağıtma

Ve değişkenler için değerler sağlamak için `$storageaccount` `$vhdUrl` aşağıdaki komut dosyasını kopyalayın ve edin. Varolan genelleştirilmiş VHD'nizden bir Azure VM kaynağı oluşturmak için çalıştırın.

```PowerShell

# storage account of existing generalized VHD

$storageaccount = "testwinrm11815"

# generalized VHD URL

$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

# deploying VM with existing VHD

New-AzResourceGroupDeployment -Name"dplisvvm$postfix" -ResourceGroupName"$rgName" -TemplateFile"C:\certLocation\VHDtoImage.json" -userStorageAccountName"$storageaccount" -dnsNameForPublicIP"$vmName" -subscriptionId"$mysubid" -location"$location" -vmName"$vmName" -vaultName"$kvname" -vaultResourceGroup"$rgName" -certificateUrl$objAzureKeyVaultSecret.Id  -vhdUrl"$vhdUrl" -vmSize"Standard\_A2" -publicIPAddressName"myPublicIP1" -virtualNetworkName"myVNET1" -nicName"myNIC1" -adminUserName"isv" -adminPassword$pwd

```

## <a name="run-validations"></a>Geçerlilikleri çalıştırma

Dağıtılan görüntüde doğrulamaları çalıştırmanın iki yolu vardır:

- Azure Sertifikalı için Sertifika Test Aracını Kullanma
- Kendi kendini test eden API'yi kullanma

### <a name="download-and-run-the-certification-test-tool"></a>Sertifika test aracını indirin ve çalıştırın

Azure Sertifikalı Sertifika Test Aracı yerel bir Windows makinesinde çalışır, ancak Azure tabanlı bir Windows veya Linux VM'yi sınar. Kullanıcı VM görüntünüzün Microsoft Azure ile kullanılabileceğini ve VHD'nizi hazırlama yla ilgili rehberlik ve gereksinimlerin karşılandığını onaylar. Aracın çıktısı, VM sertifikası istemek için Ortak Merkezi portalına yükleyeceğiniz bir uyumluluk raporudur.

1. Azure Sertifikalı için en son Sertifika Test Aracını indirin ve [yükleyin.](https://www.microsoft.com/download/details.aspx?id=44299)
2. Sertifika aracını açın ve **ardından Yeni Testi Başlat'ı**seçin.
3. Test **Bilgileri** ekranından, test çalışması için bir **Test Adı** girin.
4. Windows Server veya Linux gibi VM'niz için **Platform'u** seçin. Platform seçiminiz kalan seçenekleri etkiler.
5. VM'niz bu veritabanı hizmetini kullanıyorsa, **Azure SQL Veritabanı** için Test onay kutusunu seçin.

### <a name="connect-the-certification-tool-to-a-vm-image"></a>Sertifika aracını VM görüntüsüne bağlama

Araç, [Azure PowerShell](https://docs.microsoft.com/powershell/) ile Windows tabanlı VM'lere ve [SSH.Net](https://www.ssh.com/ssh/protocol/)aracılığıyla Linux VM'lere bağlanır.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>Sertifika aracını Linux VM görüntüsüne bağlama

1. **SSH Kimlik Doğrulama** modunu seçin: Parola Kimlik Doğrulaması veya Anahtar Dosyası Kimlik Doğrulaması.
2. Parola tabanlı kimlik doğrulaması kullanıyorsanız, **VM DNS Adı,** **Kullanıcı adı**ve **Parola**için değerleri girin. Varsayılan **SSH Bağlantı Noktası** numarasını da değiştirebilirsiniz.

    ![Azure Sertifikalı Test Aracı, Linux VM Image şifre kimlik doğrulaması](media/avm-cert2.png)

3. Anahtar dosya tabanlı kimlik doğrulamasını kullanıyorsanız, **VM DNS Adı,** **Kullanıcı adı**ve **Özel anahtar** konumu için değerleri girin. Ayrıca bir **Geçiş Cümlesi** ekleyebilirsiniz veya varsayılan **SSH Bağlantı Noktası** numarasını değiştirebilirsiniz.

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Sertifika aracını Windows tabanlı vm görüntüsüne bağlama**

1. Tam nitelikli **VM DNS adını** girin (örneğin, MyVMName.Cloudapp.net).
2. **Kullanıcı Adı** ve **Parola**için değerleri girin.

    ![Azure Sertifikalı Test Aracı, Windows tabanlı VM Image'ın parola kimlik doğrulaması](media/avm-cert4.png)

### <a name="run-a-certification-test"></a>Sertifika testi çalıştırma

Sertifika aracında VM görüntünüz için parametre değerlerini verdikten sonra, VM'inize geçerli bir bağlantı oluşturmak için **Test Bağlantısı'nı** seçin. Bağlantı doğrulandıktan sonra testi başlatmak için **İleri'yi** seçin. Test tamamlandığında, test sonuçları bir tabloda gösterilir. Durum sütunu her test için (Geç/Başarısız/Uyarı) gösterir. Testlerden herhangi biri başarısız olursa, resminiz sertifikalı _değildir._ Bu durumda, gereksinimleri ve hata iletilerini gözden geçirin, önerilen değişiklikleri yapın ve testi yeniden çalıştırın.

Otomatik test tamamlandıktan sonra, **Soru Formu** ekranının iki sekmesinde VM görüntünüz hakkında ek bilgi sağlayın, **Genel Değerlendirme** ve **Çekirdek Özelleştirme**, ve sonra **İleri'yi**seçin.

Son ekran, bir Linux VM görüntüsü için SSH erişim bilgileri ve özel durumlar arıyorsanız başarısız değerlendirmeler için bir açıklama gibi daha fazla bilgi sağlamanızı sağlar.

Son olarak, test sonuçlarını indirmek için **Rapor Oluştur'u** ve yürütülmüş test örnekleriiçin dosyaları ve soru formuna yanıtlarınız için dosyaları günlüğe kaydedin. Sonuçları VHD'lerinizle aynı kapsayıcıya kaydedin.

## <a name="next-step"></a>Sonraki adım

- [Her VHD için tek tip kaynak tanımlayıcıları (URI) oluşturma](https://aka.ms/AzureSASURI)
