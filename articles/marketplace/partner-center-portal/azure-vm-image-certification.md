---
title: VHD 'den dağıtılan test sanal makinesi (VM)-Azure Marketi
description: Ticari Market 'te bir sanal makine teklifini test etme ve gönderme hakkında bilgi edinin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 04/09/2020
ms.openlocfilehash: 3d4ec077ac0e92d26cf82ba96593a76a21ed885f
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87324684"
---
# <a name="test-virtual-machine-vm-deployed-from-vhd"></a>VHD 'den dağıtılan test sanal makinesi (VM)

Bu makalede, VHD görüntüsünün Azure Marketi yayımlama gereksinimlerini karşıladığından emin olmak için önceki bölümde oluşturulan Genelleştirilmiş VHD[görüntüsünden (VM](create-azure-vm-technical-asset.md) ) bir Azure sanal makinesinin (VM) nasıl dağıtılacağı ve test edileceğini açıklanmaktadır.

VHD görüntünüzü Azure Marketi 'nde kullanabileceğiniz bir uyumluluk raporu oluşturmak için bu adımları uygulayın.

1. Azure Key Vault için uzak VM yönetimi için gereken bir sertifika oluşturun ve dağıtın.
2. [Azure VM teknik varlığı oluşturma](create-azure-vm-technical-asset.md)bölümünde oluşturulan Genelleştirilmiş VHD görüntüsünden BIR Azure VM dağıtın.
3. VHD görüntüsünün yayımlanmaya ve VM 'Leri dağıtmak için kullanılabilir olduğundan emin olmak için dağıtılan sanal makinede testler çalıştırın.

## <a name="running-scripts"></a>Betikleri çalıştırma

Bu makale, PowerShell 'de çalıştırılacak üç komut dosyası içerir. Masaüstü PowerShell en iyi şekilde işe yarar; ancak Azure Cloud Shell, PowerShell seçeneği seçili olarak da kullanılabilir (pencerenin sol üst kısmında).

1. PowerShell 'in betikleri çalıştırmak için yapılandırıldığından emin olun.

    - Her zaman PowerShell 'i **yönetici olarak çalıştır** seçeneğiyle açın.
    - Şu komut dosyalarını çalıştırabilmeniz gerekir: `Set-ExecutionPolicy` ve `RemoteSigned` .

2. [Azure CLI’yı yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

3. Azure PowerShell az Module ' i yükler.
    1. **Seçenek A**: henüz Modül yüklenmedi.
        - `Install-Module -Name Az -AllowClobber -Scope AllUsers`

        Daha fazla bilgi için bkz. [Azure PowerShell modülünü Install](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.2.0).

    2. **Seçenek B**: Şu anda AzureRM modülü kullanılıyor.

        - Kaldırma-Azurerd
        - Install-Module-adı az-AllowClobber-Scope AllUsers
        - Enable-Azurermalıas-Scope CurrentUser

        Daha fazla bilgi için bkz. [Azurerd 'Den az ' a geçiş Azure PowerShell](https://docs.microsoft.com/powershell/azure/migrate-from-azurerm-to-az?view=azps-4.2.0).

4. Oturum parametrelerini kaydedin.

Bu bölümdeki betikler oturum değişkenlerini/parametrelerini kullanır. Oturumu kapatırsanız parametreler silinir. Parametre değeri hatalarından kaçınmak üzere tüm betikleri çalıştırmak için bir oturum kullanmanızı öneririz. Bu mümkün değilse, yeni bir oturumu açarken parametreleri yeniden başlatmanız gerekir, özellikle de daha sonraki betikler için.

## <a name="create-and-deploy-certificates-for-azure-key-vault"></a>Azure Key Vault için sertifika oluşturma ve dağıtma

Bu bölümde, Azure 'da barındırılan bir sanal makineye Windows Uzaktan Yönetimi (WinRM) bağlantısı kurmak için gereken kendinden imzalı sertifikaların nasıl oluşturulacağı ve dağıtılacağı açıklanmaktadır.

### <a name="create-certificates-for-azure-key-vault"></a>Azure Key Vault için sertifikalar oluşturma

Bu işlem üç adımdan oluşur:

1. Güvenlik sertifikası oluşturun.
2. Sertifikayı depolamak için Azure Key Vault oluşturun.
3. Sertifikaları anahtar kasasında depolayın.

Bu iş için yeni veya var olan bir Azure Kaynak grubu kullanabilirsiniz.

#### <a name="create-the-security-certificate"></a>Güvenlik sertifikası oluşturma

Sertifika dosyasını (. pfx) yerel bir klasörde oluşturmak için bu betiği çalıştırın. Sertifika, VHD görüntüsünden dağıtılacak planlı Azure VM 'sine aittir. VM 'nin, komut dosyası parametreleriyle belirtilen bir ad, konum ve parolaya ihtiyacı olacak. Tabloda gösterilen betik parametrelerinin doğru değerlerini belirtmek için aşağıdaki Azure PowerShell **sertifika oluşturma betiğini** düzenleyin.

| **Parametre** | **Açıklama** |
| --- | --- |
| $certroopath | . Pfx dosyasının kaydedileceği yerel klasör. |
| $location | Azure Standart coğrafi konumlarından biri. |
| $vmName | Planlı Azure sanal makinesinin adı. |
| $certname | Sertifikanın adı; planlı sanal makinenin tam etki alanı adıyla eşleşmelidir. |
| $certpassword | Sertifikalar için parola, planlanan VM için kullanılan parolayla eşleşmelidir. |
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
> Farklı parametrelerin değerlerini korumak için bu adımlar sırasında aynı Azure PowerShell konsol oturumunu açık ve çalışır durumda tutun.

> [!WARNING]
> Bu betiği kaydederseniz, güvenlik bilgileri (parola) içerdiğinden, yalnızca güvenli bir konuma kaydedin.

#### <a name="create-the-azure-key-vault-to-store-the-certificate"></a>Sertifikayı depolamak için Azure Anahtar Kasası oluşturma

Aşağıdaki şablonun içeriğini yerel makinenizde bir dosyaya kopyalayın. Aşağıdaki örnek betikte bu kaynak `C:\certLocation\keyvault.json` ).

```JSON
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

Bir Azure Key Vault ve ilişkili kaynak grubu oluşturmak için aşağıdaki Azure PowerShell betiğini düzenleyin ve çalıştırın. Aşağıdaki tabloda gösterilen parametrelerin değerlerini değiştirin

| **Parametre** | **Açıklama** |
| --- | --- |
| $postfix | Dağıtım tanımlayıcılarına eklenen rastgele sayısal dize. |
| $rgName | Oluşturulacak Azure Kaynak grubu (RG) adı. |
| $location | Azure Standart coğrafi konumlarından biri. |
| $kvTemplateJson | Anahtar Kasası için Kaynak Yöneticisi şablonu içeren dosyanın yolu (keyvault.js). |
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
     az group create --name $rgName --location $location
     Write-Host "-----------------------------------"

    # Create key vault and configure access
    New-AzResourceGroupDeployment -Name "kvdeploy$postfix" -ResourceGroupName $rgName -TemplateFile $kvTemplateJson -keyVaultName $kvname -tenantId $mytenantId -objectId $myobjectId

    Set-AzKeyVaultAccessPolicy -VaultName $kvname -ObjectId $myobjectId -PermissionsToKeys Decrypt,Encrypt,UnwrapKey,WrapKey,Verify,Sign,Get,List,Update,Create,Import,Delete,Backup,Restore,Recover,Purge -PermissionsToSecrets Get,List,Set,Delete,Backup,Restore,Recover,Purge

```

#### <a name="store-the-certificates-to-the-key-vault"></a>Sertifikaları anahtar kasasında depolayın

. Pfx dosyasında bulunan sertifikaları bu betiği kullanarak yeni anahtar kasasına depolayın:

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

## <a name="deploy-an-azure-vm-from-your-generalized-vhd-image"></a>Genelleştirilmiş VHD görüntüsünden bir Azure VM dağıtma

Bu bölümde, yeni bir Azure VM kaynağı oluşturmak için genelleştirilmiş bir VHD görüntüsünün nasıl dağıtılacağı açıklanmaktadır. Bu işlem için, sağlanan Azure Resource Manager şablonu ve Azure PowerShell betiği kullanacağız.

### <a name="prepare-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu hazırlama

VHD dağıtımı için aşağıdaki Azure Resource Manager şablonlarından birini (Windows veya Linux için), VHDtoImage.jsadlı yerel bir dosyaya kopyalayın. Sonraki betik bu JSON 'ı kullanmak için yerel makinede konum ister.

#### <a name="for-windows-based-vms"></a>Windows tabanlı VM 'Ler için

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

#### <a name="for-linux-based-vms"></a>Linux tabanlı VM 'Ler için

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
            "defaultValue": "linux",
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
                        "enabled": false,
                        "storageUri": "[concat('http://', parameters('userStorageAccountName'), '.blob.core.windows.net')]"
                    }
                }
                }
            }
        ]
    }

```

Bu parametrelerin değerlerini sağlamak için aşağıdaki betiği kopyalayın ve düzenleyin:

| **Parametre** | **Açıklama** |
| --- | --- |
| ResourceGroupName | Mevcut Azure Kaynak grubu adı. Genellikle, anahtar kasanız ile aynı RG 'yi kullanın. |
| TemplateFile | VHDtoImage.jsdosyanın tam yol adı. |
| userStorageAccountName | Depolama hesabının adı. |
| Snameforpublicıp | Genel IP için DNS adı; küçük harfle yazılmalıdır. |
| subscriptionId | Azure abonelik tanımlayıcısı. |
| Konum | Kaynak grubunun standart Azure coğrafi konumu. |
| vmName | Sanal makinenin adı. |
| vaultName | Anahtar kasasının adı. |
| vaultResourceGroup | Anahtar kasasının kaynak grubu. |
| Sertifika URL 'si | Sertifikanın, anahtar kasasında depolanan sürümü de dahil olmak üzere Web adresi (URL), örneğin: `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` . |
| vhdUrl | Sanal sabit diskin Web adresi. |
| vmSize | Sanal makine örneğinin boyutu. |
| Publicıpaddressname | Genel IP adresinin adı. |
| virtualNetworkName | Sanal ağın adı. |
| nicName | Sanal ağ için ağ arabirim kartının adı. |
| adminUserName | Yönetici hesabının Kullanıcı adı. |
| adminPassword | Yönetici parolası. |
|   |   |

### <a name="deploy-an-azure-vm"></a>Azure VM dağıtma

Ve değişkenleri için değerler sağlamak üzere aşağıdaki betiği kopyalayın ve düzenleyin `$storageaccount` `$vhdUrl` . Mevcut Genelleştirilmiş VHD 'nizden bir Azure VM kaynağı oluşturmak için yürütün.

```PowerShell

# storage account of existing generalized VHD

$storageaccount = "testwinrm11815"

# generalized VHD URL

$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

# Full pathname to the file VHDtoImage.json. inserted these highlighted lines
$templateFile = "$certroopath\VHDtoImage.json"

# Size of the virtual machine instance.
$vmSize = "Standard_D2s_v3"

# Name of the public IP address.
$publicIPAddressName = "myPublicIP1"

# Name of the virtual network
$virtualNetworkName = "myVNET1"

# Name of the network interface card for the virtual network
$nicName = "myNIC1"

# Username of the administrator account
$adminUserName = "isv"

# The OS of the virtual machine
$osType = "windows"

echo "New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id -vhdUrl "$vhdUrl" -vmSize "Standard\_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

# deploying VM with existing VHD

New-AzResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl “$objAzureKeyVaultSecret.Id” -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName"myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword “$pwd"

```

## <a name="run-tests-on-the-deployed-vm"></a>Dağıtılan VM 'de testleri çalıştırma

### <a name="download-and-run-the-certification-test-tool"></a>Sertifika test aracını indirme ve çalıştırma

Azure Sertifikalı sertifika test aracı, yerel bir Windows makinesinde çalışan ancak Azure tabanlı bir Windows veya Linux VM 'yi test eden bir kendi kendine test aracıdır. Kullanıcı VM Görüntünüzün Microsoft Azure birlikte kullanılabileceğini ve VHD 'nizi hazırlama konusunda rehberlik ve gereksinimlerin karşılandığını gösterir. Bu araç, sanal makinenizin Azure Marketi gereksinimlerine göre yayımlamaya hazırlanmasını sağlar. "

1. [Azure Sertifikalı en son sertifika test aracı](https://www.microsoft.com/download/details.aspx?id=44299)'nı indirip yükleyin.
2. Sertifika aracını açın ve ardından **Yeni test Başlat**' ı seçin.
3. **Test bilgileri** ekranında, test çalıştırması Için bir **Test adı** girin.
4. Windows Server veya Linux VM 'niz için **platformu** seçin. Platform seçiminiz, kalan seçenekleri etkiler.
5. VM 'niz bu veritabanı hizmetini kullanıyorsa **Azure SQL veritabanı Için test** onay kutusunu seçin.

### <a name="connect-the-certification-tool-to-a-vm-image"></a>Sertifika aracını bir VM görüntüsüne bağlama

Araç, [Azure PowerShell](https://docs.microsoft.com/powershell/) ile Windows tabanlı VM 'lere bağlanır ve [SSH.net](https://www.ssh.com/ssh/protocol/)üzerinden Linux VM 'lerine bağlanır. Linux veya Windows olmak üzere aşağıdaki iki seçenekten birini seçin.

#### <a name="option-1-connect-the-certification-tool-to-a-linux-vm-image"></a>Seçenek 1: sertifika aracını bir Linux VM görüntüsüne bağlama

1. **SSH kimlik doğrulama** modunu seçin: parola kimlik doğrulaması veya anahtar dosyası kimlik doğrulaması.
2. Parola tabanlı kimlik doğrulaması kullanıyorsanız, **VM DNS adı**, **Kullanıcı adı**ve **parola**değerlerini girin. Varsayılan **SSH bağlantı noktası** numarasını da değiştirebilirsiniz.

    ![Azure Sertifikalı test aracı, Linux VM görüntüsünün parola doğrulaması](media/avm-cert2.png)

3. Anahtar dosya tabanlı kimlik doğrulaması kullanıyorsanız, **VM DNS adı**, **Kullanıcı adı**ve **özel anahtar** konumu değerlerini girin. Ayrıca bir **parola** dahil edebilir veya varsayılan **SSH bağlantı noktası** numarasını değiştirebilirsiniz.

#### <a name="option-2-connect-the-certification-tool-to-a-windows-based-vm-image"></a>2. seçenek: sertifika aracını Windows tabanlı bir VM görüntüsüne bağlama

1. Tam **VM DNS adını** (örneğin, MyVMName.cloudapp.net) girin.
2. **Kullanıcı adı** ve **parola**değerlerini girin.

    ![Azure Sertifikalı test aracı, Windows tabanlı VM görüntüsünün parola doğrulaması](media/avm-cert4.png)

### <a name="run-a-certification-test"></a>Sertifika testi çalıştırma

VM Görüntünüz için sertifika aracında parametre değerlerini verdikten sonra, sanal makinenize geçerli bir bağlantı oluşturmak için **Bağlantıyı Sına** ' yı seçin. Bir bağlantı doğrulandıktan sonra, testi başlatmak için **İleri** ' yi seçin. Test tamamlandığında, test sonuçları bir tabloda gösterilir. Durum sütununda her test için (başarılı/başarısız/uyarı) gösterilir. Testlerin herhangi biri başarısız olursa, görüntünüz sertifikalı _değildir_ . Bu durumda, gereksinimleri ve hata iletilerini gözden geçirin, önerilen değişiklikleri yapın ve testi yeniden çalıştırın.

Otomatik test tamamlandıktan sonra, **Soru formu** ekranının, **Genel değerlendirme** ve **çekirdek özelleştirmenin**iki sekmesinde VM Görüntünüz hakkında ek bilgiler sağlayın ve ardından **İleri**' yi seçin.

Son ekran, bir Linux VM görüntüsü için SSH erişim bilgileri gibi daha fazla bilgi ve özel durumlar arıyorsanız, başarısız olan değerlendirmelerin bir açıklamasını sağlar.

Son olarak, yürütülen test çalışmalarının test sonuçlarını ve günlük dosyalarını, soru formuna yönelik yanıtlarınızla birlikte indirmek için **rapor oluştur** ' u seçin. Sonuçları VHD 'larınız ile aynı kapsayıcıya kaydedin.

## <a name="next-step"></a>Sonraki adım

- [Ortak SAS URI sorunları ve düzeltmeleri](common-sas-uri-issues.md)
