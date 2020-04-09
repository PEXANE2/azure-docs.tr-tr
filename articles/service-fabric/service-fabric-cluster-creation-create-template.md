---
title: Azure Hizmet Dokusu küme şablonu oluşturma
description: Hizmet Kumaşı kümesi için Kaynak Yöneticisi şablonu oluşturmayı öğrenin. İstemci kimlik doğrulaması için güvenliği, Azure Anahtar Kasası'nı ve Azure Etkin Dizinini (Azure AD) yapılandırın.
ms.topic: conceptual
ms.date: 08/16/2018
ms.openlocfilehash: 6cf0f9c3b8b54db7bd27ec8dd9c9d59d849c74cc
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985380"
---
# <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Hizmet Dokusu küme Kaynak Yöneticisi şablonu oluşturma

[Azure Hizmet Kumaşı kümesi,](service-fabric-deploy-anywhere.md) mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal makineler kümesidir. Azure'da çalışan Bir Hizmet Dokusu kümesi Bir Azure kaynağıdır ve Kaynak Yöneticisi kullanılarak dağıtılır, yönetilir ve izlenir.  Bu makalede, Azure'da çalışan bir Hizmet Dokusu kümesi için Kaynak Yöneticisi şablonu nasıl oluşturululur.  Şablon tamamlandığında, [kümeyi Azure'da dağıtabilirsiniz.](service-fabric-cluster-creation-via-arm.md)

Küme ilk ayarlandığında küme güvenliği yapılandırılır ve daha sonra değiştirilemez. Küme kurmadan önce [Service Fabric küme güvenlik senaryolarını][service-fabric-cluster-security]okuyun. Azure'da Service Fabric, kümenizi ve uç noktalarını güvence altına almak, istemcileri doğrulamak ve verileri şifrelemek için x509 sertifikasını kullanır. Azure Etkin Dizin iyönetimi bitiş noktalarına erişimi güvence altına almak için de önerilir. Kümeyi oluşturmadan önce Azure AD kiracıları ve kullanıcıları oluşturulmalıdır.  Daha fazla bilgi [için, istemcilerin kimliğini doğrulamak için Azure AD'yi ayarla'yı](service-fabric-cluster-creation-setup-aad.md)okuyun.

Üretim iş yüklerini çalıştırmak için bir üretim kümesi dağıtmadan önce, önce [Üretime hazırlık denetim listesini](service-fabric-production-readiness-checklist.md)okuduğunuzdan emin olun.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma
Örnek Kaynak Yöneticisi şablonları [GitHub'daki Azure örneklerinde](https://github.com/Azure-Samples/service-fabric-cluster-templates)kullanılabilir. Bu şablonlar küme şablonunuzun başlangıç noktası olarak kullanılabilir.

Bu [makalede, beş düğümlü güvenli küme][service-fabric-secure-cluster-5-node-1-nodetype] örnek şablonu ve şablon parametreleri kullanılır. *Azuredeploy.json* ve *azuredeploy.parameters.json'u* bilgisayarınıza indirin ve en sevdiğiniz metin düzenleyicisinde her iki dosyayı da açın.

> [!NOTE]
> Ulusal bulutlar için (Azure Devlet, Azure Çin, Azure `fabricSettings` Almanya), şablonunuza aşağıdakileri de eklemelisiniz: `AADLoginEndpoint` `AADTokenEndpointFormat` ve `AADCertEndpointFormat`.

## <a name="add-certificates"></a>Sertifika ekleme
Sertifika anahtarlarını içeren anahtar kasasına başvurarak küme Kaynak Yöneticisi şablonuna sertifika eklersiniz. Bu anahtar kasa parametrelerini ve değerlerini Kaynak Yöneticisi şablon parametreleri dosyasına ekleyin *(azuredeploy.parameters.json*).

### <a name="add-all-certificates-to-the-virtual-machine-scale-set-osprofile"></a>Sanal makine ölçeği kümesi osProfile tüm sertifikaları ekleyin
Kümede yüklü olan her sertifika, ölçek kümesi kaynağının (Microsoft.Compute/virtualMachineScaleSets) **osProfile** bölümünde yapılandırılmalıdır. Bu eylem, kaynak sağlayıcısına sertifikayı VM'lere yüklemesini bildirir. Bu yükleme, hem küme sertifikasını hem de uygulamalarınız için kullanmayı planladığınız uygulama güvenlik sertifikalarını içerir:

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

### <a name="configure-the-service-fabric-cluster-certificate"></a>Hizmet Kumaşı küme sertifikasını yapılandırma

Küme kimlik doğrulama sertifikası, sanal makine ölçeği kümesi kaynağındaki sanal makine ölçeği kümekümeleri için Hizmet Kumaşı küme kaynağında (Microsoft.ServiceFabric/clusters) hem de Hizmet Kumaşı uzantısında yapılandırılmalıdır. Bu düzenleme, Service Fabric kaynak sağlayıcısının küme kimlik doğrulaması ve yönetim bitiş noktaları için sunucu kimlik doğrulaması için kullanılmak üzere yapılandırmasına olanak tanır.

#### <a name="add-the-certificate-information-the-virtual-machine-scale-set-resource"></a>Sertifika bilgilerini sanal makine ölçeği ayar kaynağı ekleme

```json
{
  "apiVersion": "[variables('vmssApiVersion')]",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt_',variables('vmNodeType0Name'))]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "commonNames": ["[parameters('certificateCommonName')]"],
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

#### <a name="add-the-certificate-information-to-the-service-fabric-cluster-resource"></a>Sertifika bilgilerini Hizmet Kumaşı küme kaynağına ekleme

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
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
    },
    ...
  }
}
```

## <a name="add-azure-ad-configuration-to-use-azure-ad-for-client-access"></a>İstemci erişimi için Azure AD'yi kullanmak için Azure AD yapılandırması ekleme

Sertifika anahtarlarını içeren anahtar kasasına başvurarak bir küme Kaynak Yöneticisi şablonuna Azure AD yapılandırmasını eklersiniz. Bu Azure AD parametrelerini ve değerlerini Kaynak Yöneticisi şablon parametreleri dosyasına ekleyin (*azuredeploy.parameters.json*). 

> [!NOTE]
> Linux'ta, kümeoluşturmadan önce Azure AD kiracıları ve kullanıcıları oluşturulmalıdır.  Daha fazla bilgi [için, istemcilerin kimliğini doğrulamak için Azure AD'yi ayarla'yı](service-fabric-cluster-creation-setup-aad.md)okuyun.

```json
{
  "apiVersion": "2018-02-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificateCommonNames": {
        "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": ""
        }
        ],
        "x509StoreName": "[parameters('certificateStoreValue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

## <a name="populate-the-parameter-file-with-the-values"></a>Parametre dosyasını değerlerle doldurma

Son olarak, parametre dosyasını doldurmak için anahtar kasasından ve Azure AD PowerShell komutlarından çıktı değerlerini kullanın.

Azure hizmet kumaşı RM PowerShell modüllerini kullanmayı planlıyorsanız, küme sertifikası bilgilerini doldurmanız gerekmez. Sistemin küme güvenliği için kendi imzalı sertifikasını oluşturmasını istiyorsanız, bunları null olarak saklamanız. 

> [!NOTE]
> RM modüllerinin bu boş parametre değerlerini alıp doldurması için, parametreler inadları aşağıdaki adlarla çok eşleşir

```json
"clusterCertificateThumbprint": {
    "value": ""
},
"certificateCommonName": {
    "value": ""
},
"clusterCertificateUrlValue": {
    "value": ""
},
"sourceVaultvalue": {
    "value": ""
},
```

Uygulama sertifikalarını kullanıyorsanız veya anahtar kasasına yüklediğiniz varolan bir küme kullanıyorsanız, bu bilgileri alıp doldurmanız gerekir.

RM modülleri sizin için Azure AD yapılandırmasını oluşturamaz, bu nedenle müşteri erişimi için Azure AD'yi kullanmayı planlıyorsanız, bu yapılandırmayı doldurmanız gerekir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```

## <a name="test-your-template"></a>Şablonunuzu test edin
Kaynak Yöneticisi şablonunuzu parametre dosyasıyla sınamak için aşağıdaki PowerShell komutunu kullanın:

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

Sorunlarla karşılaştığınızda ve şifreli iletiler alırsanız, seçenek olarak "Hata Ayıklama"yı kullanın.

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json -Debug
```

Aşağıdaki diyagram, anahtar kasanızın ve Azure AD yapılandırmanızın Kaynak Yöneticisi şablonunuza nerede uyduğunu gösterir.

![Kaynak Yöneticisi bağımlılık haritası][cluster-security-arm-dependency-map]

## <a name="next-steps"></a>Sonraki adımlar
Kümeniz için bir şablonunuzun olduğuna göre, [kümeyi Azure'a nasıl dağıtabileceğinizi](service-fabric-cluster-creation-via-arm.md)öğrenin.  Henüz yapmadıysanız, üretim kümesini dağıtmadan önce [Üretime hazırlık denetim listesini](service-fabric-production-readiness-checklist.md) okuyun.

Bu makalede dağıtılan kaynakların JSON sözdizimi ve özellikleri hakkında bilgi edinmek için bkz:

* [Microsoft.ServiceFabric/kümeler](/azure/templates/microsoft.servicefabric/clusters)
* [Microsoft.Storage/storageHesapları](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft.Network/publicIPAdresleri](/azure/templates/microsoft.network/publicipaddresses)
* [Microsoft.Network/loadBalancers](/azure/templates/microsoft.network/loadbalancers)
* [Microsoft.Compute/virtualMachineScaleSets](/azure/templates/microsoft.compute/virtualmachinescalesets)

<!-- Links -->
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-create-template/cluster-security-arm-dependency-map.png
