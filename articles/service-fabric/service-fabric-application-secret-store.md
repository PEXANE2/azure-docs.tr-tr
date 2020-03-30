---
title: Azure Hizmet Kumaş Merkezi Secrets Mağaza
description: Bu makalede, Azure Hizmet Dokusunda Merkezi Sırlar Mağazası'nın nasıl kullanılacağı açıklanmaktadır.
ms.topic: conceptual
ms.date: 07/25/2019
ms.openlocfilehash: 11fb94a9fba40e6f2474ad64f5eb0c454be28ca0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77589173"
---
# <a name="central-secrets-store-in-azure-service-fabric"></a>Azure Hizmet Kumaşında Merkezi Sırlar Mağazası 
Bu makalede, Hizmet Kumaşı uygulamalarında sır oluşturmak için Azure Hizmet Dokusu'nda Central Secrets Store'un (CSS) nasıl kullanılacağı açıklanmaktadır. CSS, parola, belirteçler ve anahtarlar gibi hassas verileri bellekte şifrelenmiş tutan yerel bir gizli depo önbelleğidir.

## <a name="enable-central-secrets-store"></a>Merkezi Sırlar Mağazasını Etkinleştir
CSS'yi `fabricSettings` etkinleştirmek için aşağıdaki komut dosyasını küme yapılandırmanıza ekleyin. CSS için küme sertifikası dışında bir sertifika kullanmanızı öneririz. Şifreleme sertifikasının tüm düğümlere yüklü olduğundan ve `NetworkService` sertifikanın özel anahtarının okuma iznine sahip olduğundan emin olun.
  ```json
    "fabricSettings": 
    [
        ...
    {
        "name":  "CentralSecretService",
        "parameters":  [
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                },
                {
                    "name":  "MinReplicaSetSize",
                    "value":  "3"
                },
                {
                    "name":  "TargetReplicaSetSize",
                    "value":  "3"
                },
                 {
                    "name" : "EncryptionCertificateThumbprint",
                    "value": "<thumbprint>"
                 }
                ,
                ],
            },
            ]
     }
        ...
     ]
```
## <a name="declare-a-secret-resource"></a>Gizli bir kaynak bildirme
Azure Kaynak Yöneticisi şablonu veya REST API'sini kullanarak gizli bir kaynak oluşturabilirsiniz.

### <a name="use-resource-manager"></a>Kaynak Yöneticisi'ni Kullan

Gizli kaynağı oluşturmak için Kaynak Yöneticisi'ni kullanmak için aşağıdaki şablonu kullanın. Şablon gizli bir `supersecret` kaynak oluşturur, ancak gizli kaynak için henüz bir değer ayarlı değildir.


```json
   "resources": [
      {
        "apiVersion": "2018-07-01-preview",
        "name": "supersecret",
        "type": "Microsoft.ServiceFabricMesh/secrets",
        "location": "[parameters('location')]", 
        "dependsOn": [],
        "properties": {
          "kind": "inlinedValue",
            "description": "Application Secret",
            "contentType": "text/plain",
          }
        }
      ]
```

### <a name="use-the-rest-api"></a>REST API kullanma

REST API'sini kullanarak gizli bir `supersecret` kaynak oluşturmak `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview`için BIR PUT isteğinde bulunun. Gizli bir kaynak oluşturmak için küme sertifikasına veya yönetici istemci sertifikasına ihtiyacınız var.

```powershell
$json = '{"properties": {"kind": "inlinedValue", "contentType": "text/plain", "description": "supersecret"}}'
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint> -Body $json
```

## <a name="set-the-secret-value"></a>Gizli değeri ayarlama

### <a name="use-the-resource-manager-template"></a>Kaynak Yöneticisi şablonunu kullanma

Gizli değeri oluşturmak ve ayarlamak için aşağıdaki Kaynak Yöneticisi şablonunu kullanın. Bu şablon, `supersecret` `ver1`sürüm olarak gizli kaynak için gizli değeri ayarlar.
```json
  {
  "parameters": {
  "supersecret": {
      "type": "string",
      "metadata": {
        "description": "supersecret value"
      }
   }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
        "name": "supersecret",
        "type": "Microsoft.ServiceFabricMesh/secrets",
        "location": "[parameters('location')]", 
        "dependsOn": [],
        "properties": {
          "kind": "inlinedValue",
            "description": "Application Secret",
            "contentType": "text/plain",
        }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "supersecret/ver1",
      "type": "Microsoft.ServiceFabricMesh/secrets/values",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/secrets/supersecret"
      ],
      "properties": {
        "value": "[parameters('supersecret')]"
      }
    }
  ],
  ```
### <a name="use-the-rest-api"></a>REST API kullanma

Gizli değeri ayarlamak için REST API'yi kullanmak için aşağıdaki komut dosyasını kullanın.
```powershell
$Params = '{"properties": {"value": "mysecretpassword"}}'
Invoke-WebRequest -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret/values/ver1?api-version=6.4-preview -Method PUT -Body $Params -CertificateThumbprint <ClusterCertThumbprint>
```
### <a name="examine-the-secret-value"></a>Gizli değeri inceleme
```powershell
Invoke-WebRequest -CertificateThumbprint <ClusterCertThumbprint> -Method POST -Uri "https:<clusterfqdn>/Resources/Secrets/supersecret/values/ver1/list_value?api-version=6.4-preview"
```
## <a name="use-the-secret-in-your-application"></a>Uygulamanızdaki sırrı kullanın

Service Fabric uygulamanızdaki sırrı kullanmak için aşağıdaki adımları izleyin.

1. **Settings.xml** dosyasına aşağıdaki snippet ile bir bölüm ekleyin. Burada değerin {`secretname:version`} biçiminde olduğunu unutmayın.

   ```xml
     <Section Name="testsecrets">
      <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
     </Section>
   ```

1. **ApplicationManifest.xml**bölümünden alma .
   ```xml
     <ServiceManifestImport>
       <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
       <ConfigOverrides />
       <Policies>
         <ConfigPackagePolicies CodePackageRef="Code">
           <ConfigPackage Name="Config" SectionName="testsecrets" EnvironmentVariableName="SecretPath" />
           </ConfigPackagePolicies>
       </Policies>
     </ServiceManifestImport>
   ```

   Ortam değişkeni, `SecretPath` tüm sırların depolandığı dizini gösterir. `testsecrets` Bölümün altında listelenen her parametre ayrı bir dosyada saklanır. Uygulama şimdi aşağıdaki gibi gizli kullanabilirsiniz:
   ```C#
   secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
   ```
1. Sırları bir konteynıra monte edin. Konteyner içinde kullanılabilir sırları yapmak için gerekli `specify` tek değişiklik `<ConfigPackage>`bir montaj noktasına.
Aşağıdaki snippet değiştirilmiş **ApplicationManifest.xml**olduğunu.  

   ```xml
   <ServiceManifestImport>
       <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
       <ConfigOverrides />
       <Policies>
         <ConfigPackagePolicies CodePackageRef="Code">
           <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="C:\secrets" EnvironmentVariableName="SecretPath" />
           <!-- Linux Container
            <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="/mnt/secrets" EnvironmentVariableName="SecretPath" />
           -->
         </ConfigPackagePolicies>
       </Policies>
     </ServiceManifestImport>
   ```
   Sırlar konteyner içinde montaj noktası altında mevcuttur.

1. Bir işlemi ortamı değişkenine bir sırrı `Type='SecretsStoreRef`' nı belirterek bağlayabilirsiniz. Aşağıdaki `supersecret` parçacık `ver1` **ServiceManifest.xml'deki**sürümü çevre değişkenine `MySuperSecret` nasıl bağlayacak nasıl bağlanabildiğini gösteren bir örnektir.

   ```xml
   <EnvironmentVariables>
     <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
   </EnvironmentVariables>
   ```

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama ve hizmet güvenliği](service-fabric-application-and-service-security.md)hakkında daha fazla bilgi edinin.
