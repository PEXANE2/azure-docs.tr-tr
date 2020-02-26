---
title: Azure Service Fabric merkezi gizli dizi deposu
description: Bu makalede, Azure Service Fabric 'da merkezi gizlilikler deposunun nasıl kullanılacağı açıklanır.
ms.topic: conceptual
ms.date: 07/25/2019
ms.openlocfilehash: 11fb94a9fba40e6f2474ad64f5eb0c454be28ca0
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589173"
---
# <a name="central-secrets-store-in-azure-service-fabric"></a>Azure Service Fabric 'da merkezi gizlilikler Mağazası 
Bu makalede, Service Fabric uygulamalarında gizli diziler oluşturmak için Azure Service Fabric 'da merkezi gizlilikler deposunun (CSS) nasıl kullanılacağı açıklanır. CSS, bir parola, belirteç ve anahtar gibi hassas verileri bellekte şifreli olarak tutan bir yerel gizli dizi deposu önbelleğidir.

## <a name="enable-central-secrets-store"></a>Merkezi gizli dizi deposunu etkinleştir
CSS 'yi etkinleştirmek için `fabricSettings` altındaki küme yapılandırmanıza aşağıdaki betiği ekleyin. CSS için bir küme sertifikası dışında bir sertifika kullanmanızı öneririz. Şifreleme sertifikasının tüm düğümlerde yüklü olduğundan ve `NetworkService` sertifikanın özel anahtarı için okuma iznine sahip olduğundan emin olun.
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
## <a name="declare-a-secret-resource"></a>Gizli bir kaynak bildirin
Azure Resource Manager şablonunu veya REST API kullanarak gizli bir kaynak oluşturabilirsiniz.

### <a name="use-resource-manager"></a>Kaynak Yöneticisi kullan

Gizli kaynağı oluşturmak için Kaynak Yöneticisi kullanmak için aşağıdaki şablonu kullanın. Şablon, bir `supersecret` gizli kaynağı oluşturur, ancak gizli kaynak için henüz bir değer ayarlanmadı.


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

REST API kullanarak `supersecret` gizli bir kaynak oluşturmak için `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview`için bir PUT isteği yapın. Gizli bir kaynak oluşturmak için küme sertifikası veya yönetici istemci sertifikasına ihtiyacınız vardır.

```powershell
$json = '{"properties": {"kind": "inlinedValue", "contentType": "text/plain", "description": "supersecret"}}'
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint> -Body $json
```

## <a name="set-the-secret-value"></a>Gizli değeri ayarla

### <a name="use-the-resource-manager-template"></a>Kaynak Yöneticisi şablonunu kullanma

Gizli değeri oluşturmak ve ayarlamak için aşağıdaki Kaynak Yöneticisi şablonunu kullanın. Bu şablon, `supersecret` gizli kaynağı için gizli değeri sürüm `ver1`olarak ayarlar.
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

Gizli değeri ayarlamak için REST API kullanmak için aşağıdaki betiği kullanın.
```powershell
$Params = '{"properties": {"value": "mysecretpassword"}}'
Invoke-WebRequest -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret/values/ver1?api-version=6.4-preview -Method PUT -Body $Params -CertificateThumbprint <ClusterCertThumbprint>
```
### <a name="examine-the-secret-value"></a>Gizli değeri inceleyin
```powershell
Invoke-WebRequest -CertificateThumbprint <ClusterCertThumbprint> -Method POST -Uri "https:<clusterfqdn>/Resources/Secrets/supersecret/values/ver1/list_value?api-version=6.4-preview"
```
## <a name="use-the-secret-in-your-application"></a>Uygulamanızda gizli dizi kullanın

Service Fabric uygulamanızda gizli dizi kullanmak için bu adımları izleyin.

1. Aşağıdaki kod parçacığına sahip **Settings. xml** dosyasına bir bölüm ekleyin. Değerin {`secretname:version`} biçiminde olduğunu unutmayın.

   ```xml
     <Section Name="testsecrets">
      <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
     </Section>
   ```

1. **ApplicationManifest. xml**dosyasındaki bölümü içeri aktarın.
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

   Ortam değişkeni `SecretPath`, tüm parolaların depolandığı dizine işaret eder. `testsecrets` bölümü altında listelenen her bir parametre ayrı bir dosyada depolanır. Uygulama artık gizli anahtarı şu şekilde kullanabilir:
   ```C#
   secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
   ```
1. Gizli dizileri bir kapsayıcıya bağlayın. Bir bağlama noktası, kapsayıcının içinde kullanılabilir hale getirmek için gereken tek değişiklik `<ConfigPackage>`bir bağlama noktası `specify`.
Aşağıdaki kod parçacığı, değiştirilen **ApplicationManifest. xml**' dir.  

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
   Gizli dizileri, kapsayıcının içindeki bağlama noktası altında bulunur.

1. `Type='SecretsStoreRef`belirterek bir gizli dizi öğesini bir işlem ortam değişkenine bağlayabilirsiniz. Aşağıdaki kod parçacığı, `supersecret` sürümü `ver1` **Servicemanifest. xml**' de `MySuperSecret` ortam değişkenine nasıl bağlayacağınız konusunda bir örnektir.

   ```xml
   <EnvironmentVariables>
     <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
   </EnvironmentVariables>
   ```

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama ve hizmet güvenliği](service-fabric-application-and-service-security.md)hakkında daha fazla bilgi edinin.
