---
title: Azure Hizmet Kumaş Örgü Uygulama Sırlarını Yönetme
description: Bir Service Fabric Mesh uygulamasını güvenli bir şekilde oluşturup dağıtmak için uygulama sırlarını yönetin.
ms.date: 4/2/2019
ms.topic: conceptual
ms.openlocfilehash: d7946092a0bebe374404870fcd711ad33cc98b11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75461924"
---
# <a name="manage-service-fabric-mesh-application-secrets"></a>Servis Kumaş Örgü Uygulama Sırlarını Yönet
Hizmet Fabric Mesh, Azure kaynakları olarak Sırları destekler. Service Fabric Mesh sırrı, depolama bağlantı dizeleri, parolalar veya güvenli bir şekilde depolanması ve iletilmesi gereken diğer değerler gibi hassas metin bilgileri olabilir. Bu makalede, Sırlar dağıtmak ve korumak için Service Fabric Secure Store Service nasıl kullanılacağı nı gösterir.

Mesh uygulaması Secret aşağıdakilerden oluşur:
* Metin sırlarını depolayan bir kap olan **Sırlar** kaynağı. **Sırlar** kaynağında bulunan sırlar güvenli bir şekilde saklanır ve iletilir.
* **Sırlar** kaynak kapsayıcısında depolanan bir veya daha fazla **Sırlar/Değerler** kaynakları. Her **Sırlar/Değerler** kaynağı bir sürüm numarasıyla ayırt edilir. **Sırlar/Değerler** kaynağının bir sürümünü değiştiremezsiniz, yalnızca yeni bir sürümü eklersiniz.

Sırları Yönetme aşağıdaki adımlardan oluşur:
1. InlinedValue tür ve SecretsStoreRef içeriğiTip tanımları kullanarak bir Azure Kaynak Modeli YAML veya JSON dosyasında bir Mesh **Secrets** kaynağı bildirin.
2. Sırlar **kaynağında** (adım 1'den) depolanacak bir Azure Kaynak Modeli YAML veya JSON dosyasında Mesh **Sırları/Değerleri** kaynaklarını bildirin.
3. Mesh gizli değerlerine başvurmak için Mesh uygulamasını değiştirin.
4. Gizli değerleri tüketmek için Mesh uygulamasını dağıtın veya yuvarlayın.
5. Güvenli Mağaza Hizmeti yaşam döngüsü yönetimi için Azure "az" CLI komutlarını kullanın.

## <a name="declare-a-mesh-secrets-resource"></a>Mesh Sırları kaynağını bildir
Bir Mesh Secrets kaynağı, InlinedValue tür tanımı kullanılarak Azure Kaynak Modeli JSON veya YAML dosyasında bildirilir. Mesh Secrets kaynağı Güvenli Mağaza Hizmeti kaynaklı sırları destekler. 
>
Aşağıdaki, Bir JSON dosyasında Mesh Secrets kaynaklarının nasıl beyan edilecek lerine bir örnektir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "WestUS",
      "metadata": {
        "description": "Location of the resources (e.g. westus, eastus, westeurope)."
      }
    }
  },
  "sfbpHttpsCertificate": {
      "type": "string",
      "metadata": {
        "description": "Plain Text Secret Value that your container ingest"
      }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx",
      "type": "Microsoft.ServiceFabricMesh/secrets",
      "location": "[parameters('location')]", 
      "dependsOn": [],
      "properties": {
        "kind": "inlinedValue",
        "description": "SFBP Application Secret",
        "contentType": "text/plain",
      }
    }
  ]
}
```
Bir YAML dosyasında Mesh Secrets kaynaklarının nasıl beyan edilene ne kadar açık olduğuna bir örnek aşağıda verilmiştir:
```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
          secrets:
            - name: MySecret.txt
            description: My Mesh Application Secret
            secret_type: inlinedValue
            content_type: SecretStoreRef
            value: mysecret
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

## <a name="declare-mesh-secretsvalues-resources"></a>Mesh Sırlar/Değerler kaynaklarını bildir
Mesh Secrets/Values kaynakları, önceki adımda tanımlanan Kafes Sırları kaynaklarına bağımlıdır.

"Kaynaklar" bölümü "değer:" ve "ad:" alanları arasındaki ilişkiye ilişkin olarak: "ad:" dizesinin üst üste ile sınırlandırılmış ikinci bölümü, bir sır için kullanılan sürüm numarasıdır ve iki nokta üst üste gelen ad, bir Bağımlılık. Örneğin, öğe ```name: mysecret:1.0```için, sürüm numarası 1.0 ```mysecret``` ve adı daha ```"value": "mysecret"```önce tanımlanan eşleşmesi gerekir.

>
Aşağıda, Bir JSON dosyasında Mesh Sırları/Değerleri kaynaklarının nasıl beyan edileceklerine bir örnek verilmiştir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "WestUS",
      "metadata": {
        "description": "Location of the resources (e.g. westus, eastus, westeurope)."
      }
    }
  },
  "sfbpHttpsCertificate": {
      "type": "string",
      "metadata": {
        "description": "Plain Text Secret Value that your container ingest"
      }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx",
      "type": "Microsoft.ServiceFabricMesh/secrets",
      "location": "[parameters('location')]", 
      "dependsOn": [],
      "properties": {
        "kind": "inlinedValue",
        "description": "SFBP Application Secret",
        "contentType": "text/plain",
      }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "sfbpHttpsCertificate.pfx/2019.02.28",
      "type": "Microsoft.ServiceFabricMesh/secrets/values",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/secrets/sfbpHttpsCertificate.pfx"
      ],
      "properties": {
        "value": "[parameters('sfbpHttpsCertificate')]"
      }
    }
  ],
}
```
Bir YAML dosyasında Kafes Sırları/Değerleri kaynaklarının nasıl beyan edilebildiğinin bir örneği aşağıda verilmiştir:
```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
          Secrets:
            - name: MySecret.txt
            description: My Mesh Application Secret
            secret_type: inlinedValue
            content_type: SecretStoreRef
            value: mysecret
            - name: mysecret:1.0
            description: My Mesh Application Secret Value
            secret_type: value
            content_type: text/plain
            value: "P@ssw0rd#1234"
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

## <a name="modify-mesh-application-to-reference-mesh-secret-values"></a>Mesh Gizli değerlerine başvurmak için Mesh uygulamasını değiştirme
Servis Kumaş Kafes uygulamaları, Güvenli Mağaza Hizmeti Gizli değerlerini tüketmek için aşağıdaki iki dizeye dikkat edilmelidir:
1. Microsoft.ServiceFabricMesh/Secrets.name dosyanın adını içerir ve düz metinde Sırlar değerini içerir.
2. Windows veya Linux ortam değişkeni "Fabric_SettingPath", Güvenli Mağaza Hizmeti Sırları değerlerini içeren dosyaların erişilebildiği dizin yolunu içerir. Bu, Windows tarafından barındırılan Mesh uygulamaları için sırasıyla "C:\Ayarlar" ve Linux tarafından barındırılan Mesh uygulamaları için "/var/settings" dir.

## <a name="deploy-or-use-a-rolling-upgrade-for-mesh-application-to-consume-secret-values"></a>Gizli değerleri kullanmak için Mesh uygulaması için yuvarlanma yükseltmesi dağıtma veya kullanma
Sırlar ve/veya sürümlere sürümler/Değerler oluşturma, dağıtımlar bildirilen kaynak modeliyle sınırlıdır. Bu kaynakları oluşturmanın tek yolu, **az örgü dağıtım** komutunu kullanarak bir kaynak modeli JSON veya YAML dosyasını aşağıdaki gibi geçirmektir:

```azurecli-interactive
az mesh deployment create –-<template-file> or --<template-uri>
```

## <a name="azure-cli-commands-for-secure-store-service-lifecycle-management"></a>Güvenli Mağaza Hizmeti yaşam döngüsü yönetimi için Azure CLI komutları

### <a name="create-a-new-secrets-resource"></a>Yeni bir Sırlar kaynağı oluşturma
```azurecli-interactive
az mesh deployment create –-<template-file> or --<template-uri>
```
Şablon **dosyasını** veya **şablon-uri'yi** (ancak her ikisini birden değil) geçirin.

Örnek:
- az örgü dağıtım oluşturma --c:\MyMeshTemplates\SecretTemplate1.txt
- az örgü dağıtım oluşturmak\/--https: /www.fabrikam.com/MyMeshTemplates/SecretTemplate1.txt

### <a name="show-a-secret"></a>Bir Sırrı Göster
Sırrın açıklamasını (ancak değeri değil) döndürür.
```azurecli-interactive
az mesh secret show --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="delete-a-secret"></a>Bir Sırrı Silme

- Bir Mesh uygulaması tarafından başvurulmakta olan bir sır silinemez.
- Sırlar kaynağını silmek tüm Sırlar/Kaynaklar sürümlerini siler.
  ```azurecli-interactive
  az mesh secret delete --Resource-group <myResourceGroup> --secret-name <mySecret>
  ```

### <a name="list-secrets-in-subscription"></a>Abonelikteki Sırları Listele
```azurecli-interactive
az mesh secret list
```
### <a name="list-secrets-in-resource-group"></a>Kaynak Grubunda Sırları Listele
```azurecli-interactive
az mesh secret list -g <myResourceGroup>
```
### <a name="list-all-versions-of-a-secret"></a>Bir Sırrın Tüm Sürümlerini Listele
```azurecli-interactive
az mesh secretvalue list --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="show-secret-version-value"></a>Gizli Sürüm Değerini Göster
```azurecli-interactive
az mesh secretvalue show --Resource-group <myResourceGroup> --secret-name <mySecret> --version <N>
```

### <a name="delete-secret-version-value"></a>Gizli Sürüm Değerini Sil
```azurecli-interactive
az mesh secretvalue delete --Resource-group <myResourceGroup> --secret-name <mySecret> --version <N>
```

## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh hakkında daha fazla bilgi edinmek için genel bakışı okuyun:
- [Hizmet Kumaş Örgü genel bakış](service-fabric-mesh-overview.md)
