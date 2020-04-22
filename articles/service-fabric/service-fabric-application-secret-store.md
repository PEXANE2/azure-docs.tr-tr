---
title: Azure Hizmet Kumaş Merkezi Secrets Mağaza
description: Bu makalede, Azure Hizmet Dokusunda Merkezi Sırlar Mağazası'nın nasıl kullanılacağı açıklanmaktadır.
ms.topic: conceptual
ms.date: 07/25/2019
ms.openlocfilehash: 4087e7ccdcb2281c4a08af155d35a10c66147a85
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770424"
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
REST API'sini kullanarak gizli bir kaynak oluşturabilirsiniz.
  > [!NOTE] 
  > Küme windows kimlik doğrulaması kullanıyorsa, REST isteği güvenli olmayan HTTP kanalı üzerinden gönderilir. Öneri, güvenli uç noktaları olan X509 tabanlı bir küme kullanmaktır.

REST API'sini kullanarak gizli bir `supersecret` kaynak oluşturmak `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview`için BIR PUT isteğinde bulunun. Gizli bir kaynak oluşturmak için küme sertifikasına veya yönetici istemci sertifikasına ihtiyacınız var.

```powershell
$json = '{"properties": {"kind": "inlinedValue", "contentType": "text/plain", "description": "supersecret"}}'
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint> -Body $json
```

## <a name="set-the-secret-value"></a>Gizli değeri ayarlama

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
