---
title: Sertifika ortak adını kullanarak küme oluşturma
description: Şablondan sertifika ortak adını kullanarak Hizmet Kumaşı kümesini nasıl oluşturabilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: 4a4448c88fa9493979f075f6b9c669927dd1d39e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614562"
---
# <a name="deploy-a-service-fabric-cluster-that-uses-certificate-common-name-instead-of-thumbprint"></a>Küçük parmak izi yerine sertifika ortak adını kullanan bir Hizmet Dokusu kümesi dağıtma
Hiçbir iki sertifika aynı parmak izine sahip olamaz, bu da küme sertifikası rollover veya yönetimi zorlaştırır. Ancak, birden çok sertifika aynı ortak ada veya özneye sahip olabilir.  Sertifika ortak adlarını kullanan bir küme, sertifika yönetimini çok daha basit hale getirir. Bu makalede, sertifika parmak izi yerine sertifika ortak adını kullanmak için bir Hizmet Dokusu kümenasıl dağıtılan açıklanmaktadır.
 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-a-certificate"></a>Sertifika alın
İlk olarak, bir [sertifika yetkilisinden (CA)](https://wikipedia.org/wiki/Certificate_authority)sertifika alın.  Sertifikanın ortak adı, sahip olduğunuz ve bir etki alanı kayıt şirketinden satın aldığınız özel etki alanı için olmalıdır. Örneğin, "azureservicefabricbestpractices.com"; Microsoft çalışanı olmayanlar MS etki alanları için sertifika sağlayamamaktadır, bu nedenle LB veya Trafik Yöneticinizin DNS adlarını sertifikanız için ortak adlar olarak kullanamazsınız ve özel etki alanınızın Azure'da çözülebilmesi için bir [Azure DNS Bölgesi](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns) sağlamanız gerekir. Ayrıca, portalınızın kümenizin özel etki alanı diğer adını yansıtmasını istiyorsanız, sahip olduğunuz özel etki alanınızı kümenizin "yönetimiBitiş Noktası" olarak bildirmek isteyeceksiniz.

Test amacıyla, ücretsiz veya açık sertifika yetkilisinden CA imzalı sertifika alabilirsiniz.

> [!NOTE]
> Azure portalında Hizmet Kumaşı kümesi dağıtılırken oluşturulanlar da dahil olmak üzere kendi imzalı sertifikalar desteklenmez. 

## <a name="upload-the-certificate-to-a-key-vault"></a>Sertifikayı anahtar kasasına yükleme
Azure'da, Bir Hizmet Kumaşı kümesi sanal makine ölçeği kümesinde dağıtılır.  Sertifikayı anahtar kasasına yükleyin.  Küme dağıtıldığında, sertifika kümenin üzerinde çalışan sanal makine ölçeğine yüklenir.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    
```

## <a name="download-and-update-a-sample-template"></a>Örnek bir şablonu indirin ve güncelleyin
Bu [makalede, 5-düğüm güvenli küme örnek](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure) şablonu ve şablon parametreleri kullanır. *Azuredeploy.json* ve *azuredeploy.parameters.json* dosyalarını bilgisayarınıza indirin.

### <a name="update-parameters-file"></a>Parametreler dosyalarını güncelleştir
İlk olarak, bir metin düzenleyicisinde *azuredeploy.parameters.json* dosyasını açın ve aşağıdaki parametre değerini ekleyin:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
"certificateIssuerThumbprint": {
    "value": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
},
```

Ardından, önceki komut dosyası tarafından döndürülenlere *CommonName*, *sourceVaultValue*ve *sertifikaUrlValue* parametre değerlerini ayarlayın:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
"certificateIssuerThumbprint": {
    "value": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
},
"sourceVaultValue": {
  "value": "/subscriptions/<subscription>/resourceGroups/testvaultgroup/providers/Microsoft.KeyVault/vaults/testvault"
},
"certificateUrlValue": {
  "value": "https://testvault.vault.azure.net:443/secrets/testcert/5c882b7192224447bbaecd5a46962655"
},
```

### <a name="update-the-template-file"></a>Şablon dosyasını güncelleştirme
Ardından, *azuredeploy.json* dosyasını bir metin düzenleyicisinde açın ve sertifika ortak adını desteklemek için üç güncelleştirme yapın.

1. **Parametreler** bölümünde, bir *sertifikaCommonName* parametresi ekleyin:
    ```json
    "certificateCommonName": {
      "type": "string",
      "metadata": {
        "description": "Certificate Commonname"
      }
    },
    "certificateIssuerThumbprint": {
      "type": "string",
      "metadata": {
        "description": "Certificate Authority Issuer Thumpbrint for Commonname cert"
      }
    },
    ```

    Ayrıca *sertifikaThumbprint*kaldırmayı düşünün, artık gerekli olabilir.

2. *sfrpApiVersion* değişkeninin değerini "2018-02-01" olarak ayarlayın:
    ```json
    "sfrpApiVersion": "2018-02-01",
    ```

3. **Microsoft.Compute/virtualMachineScaleSets** kaynağında, küçük parmak izi yerine sertifika ayarlarında ortak adı kullanmak için sanal makine uzantısını güncelleştirin.  **virtualMachineProfile**->**uzantılarındaProfil**->**extensions**->uzantıları**özellikleri**->**ayarları**->**sertifikası**, ekle 
    ```json
       "commonNames": [
        "[parameters('certificateCommonName')]"
       ],
    ```

    ve `"thumbprint": "[parameters('certificateThumbprint')]",`kaldırın.

    ```json
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              "type": "ServiceFabricNode",
              "autoUpgradeMinorVersion": true,
              "protectedSettings": {
                "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
              },
              "publisher": "Microsoft.Azure.ServiceFabric",
              "settings": {
                "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                "nodeTypeRef": "[variables('vmNodeType0Name')]",
                "dataPath": "D:\\SvcFab",
                "durabilityLevel": "Bronze",
                "enableParallelJobs": true,
                "nicPrefixOverride": "[variables('subnet0Prefix')]",
                "certificate": {
                  "commonNames": [
                     "[parameters('certificateCommonName')]"
                  ],
                  "x509StoreName": "[parameters('certificateStoreValue')]"
                }
              },
              "typeHandlerVersion": "1.0"
            }
          },
    ```

4. **Microsoft.ServiceFabric/clusters** kaynağında, API sürümünü "2018-02-01" olarak güncelleyin.  Ayrıca, **commonAd** özelliğine sahip bir **sertifikaCommonNames** ayarı ekleyin ve aşağıdaki örnekte olduğu gibi **sertifika** ayarını (parmak izi özelliğiyle) kaldırın:
   ```json
   {
       "apiVersion": "2018-02-01",
       "type": "Microsoft.ServiceFabric/clusters",
       "name": "[parameters('clusterName')]",
       "location": "[parameters('clusterLocation')]",
       "dependsOn": [
       "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
       ],
       "properties": {
       "addonFeatures": [
           "DnsService",
           "RepairManager"
       ],        
       "certificateCommonNames": {
           "commonNames": [
           {
               "certificateCommonName": "[parameters('certificateCommonName')]",
               "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprint')]"
           }
           ],
           "x509StoreName": "[parameters('certificateStoreValue')]"
       },
       ...
   ```
   > [!NOTE]
   > 'sertifikaIssuerThumbprint' alanı, belirli bir konu ortak adı olan sertifikaların beklenen verenlerin belirtilmesine olanak sağlar. Bu alan, SHA1 parmak izlerinin virgülden ayrılmış numaralandırmayı kabul eder. Bunun sertifika doğrulamasının güçlendirilmesi olduğunu unutmayın - İhraççı belirtilmemiş sayılsa veya boşsa, zinciri oluşturulabilirse ve validatör tarafından güvenilen bir kökte son bulursa, sertifika kimlik doğrulaması için kabul edilir. İhraççı belirtilmişse, doğrudan verenin parmak izi, köke güvenilip güvenilmediğine bakılmaksızın, bu alanda belirtilen değerlerden herhangi biri ile eşleşirse sertifika kabul edilir. Bir PKI'nın aynı konu için sertifika vermek için farklı sertifika yetkilileri kullanabileceğini ve bu nedenle belirli bir konu için beklenen tüm veren parmak izlerini belirtmeniz gerektiğini lütfen unutmayın.
   >
   > İhraççının belirtilmesi en iyi uygulama olarak kabul edilir; atlayarak çalışmaya devam edecektir - güvenilir bir kök kadar zincirleme sertifikalar için - bu davranış sınırlamaları vardır ve yakın gelecekte aşamalı olabilir. Ayrıca, Azure'da dağıtılan ve özel bir PKI tarafından verilen ve konuya göre bildirilen X509 sertifikalarıyla güvence altına alınamayan kümelerin Azure Hizmet Kumaşı hizmeti (kümeden hizmete iletişim için) tarafından doğrulanamayabileceğini de unutmayın, PKI'nın Sertifika İlkesi bulunamaz, kullanılabilir ve erişilebilir değildir. 

## <a name="deploy-the-updated-template"></a>Güncelleştirilmiş şablonu dağıtma
Değişiklikleri yaptıktan sonra güncelleştirilmiş şablonu yeniden dağıtın.

```powershell
# Variables.
$groupname = "testclustergroup"
$clusterloc="southcentralus"  
$id="<subscription ID"

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $id 

# Create a new resource group and deploy the cluster.
New-AzResourceGroup -Name $groupname -Location $clusterloc

New-AzResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\AzureDeploy.Parameters.json" -TemplateFile "C:\temp\cluster\AzureDeploy.json" -Verbose
```

## <a name="next-steps"></a>Sonraki adımlar
* Küme [güvenliği](service-fabric-cluster-security.md)hakkında bilgi edinin.
* Küme sertifikasını nasıl [devratacaklarını](service-fabric-cluster-rollover-cert-cn.md) öğrenin
* [Küme sertifikalarını güncelleştirme ve yönetme](service-fabric-cluster-security-update-certs-azure.md)
* Sertifika parmak [izinden ortak ada kümedeğiştirerek](service-fabric-cluster-change-cert-thumbprint-to-cn.md) Sertifika Yönetimini Basitleştir

[image1]: .\media\service-fabric-cluster-change-cert-thumbprint-to-cn\PortalViewTemplates.png
