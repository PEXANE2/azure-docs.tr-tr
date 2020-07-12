---
title: Azure Service Fabric-Service Fabric uygulama Anahtar Kasası başvurularını kullanma
description: Bu makalede, uygulama gizli dizileri için Service Fabric KeyVaultReference desteğinin nasıl kullanılacağı açıklanmaktadır.
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: f1ac3ac50c5ac7cbabb03561c5db7f9c14150de4
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86246172"
---
#  <a name="keyvaultreference-support-for-service-fabric-applications-preview"></a>Service Fabric uygulamalar için KeyVaultReference desteği (Önizleme)

Bulut uygulamaları derlerken, uygulamanız için gereken gizli dizileri güvenli bir şekilde nasıl depolayabileceği yaygın bir zorluk vardır. Örneğin, kapsayıcı deposu kimlik bilgilerini anahtar kasasında depolamak ve uygulama bildiriminde buna başvurmak isteyebilirsiniz. Service Fabric KeyVaultReference, Service Fabric yönetilen kimliği kullanır ve keykasagizli dizileri başvurusunu kolaylaştırır. Bu makalenin geri kalanında, KeyVaultReference Service Fabric kullanımı ve bazı tipik kullanımlar yer almaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Uygulama için yönetilen kimlik (MıT)
    
    Service Fabric KeyVaultReference desteği uygulamanın yönetilen kimliğini kullanır ve bu nedenle KeyVaultReferences 'ı kullanmak için uygulamalar yönetilen kimlik kullanmalıdır. Uygulamanız için yönetilen kimliği etkinleştirmek üzere bu [belgeyi](concepts-managed-identity.md) izleyin.

- Orta gizli dizi deposu (CSS).

    Orta gizli dizi deposu (CSS) Service Fabric şifreli yerel gizli diziler önbelleğidir. CSS, bir parola, belirteç ve anahtar gibi hassas verileri bellekte şifreli olarak tutan bir yerel gizli dizi deposu önbelleğidir. Bir kez getirildikten sonra KeyVaultReference, CSS 'de önbelleğe alınır.

    `fabricSettings`KeyVaultReference desteği için gerekli tüm özellikleri etkinleştirmek üzere altındaki küme yapılandırmanıza aşağıda ekleyin.

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
                }
                ]
            },
            {
                "name":  "ManagedIdentityTokenService",
                "parameters":  [
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                }
                ]
            }
            ]
    ```

    > [!NOTE] 
    > CSS için ayrı bir şifreleme sertifikası kullanılması önerilir. Bunu "merkezde Secretservice" bölümünün altına ekleyebilirsiniz.
    

    ```json
        {
            "name": "EncryptionCertificateThumbprint",
            "value": "<EncryptionCertificateThumbprint for CSS>"
        }
    ```
Değişikliklerin etkili olabilmesi için yükseltme ilkesini, yükseltmenin kümede ilerledikçe her düğümde Service Fabric çalışma zamanının zorla yeniden başlatılmasını belirtmek için de değiştirmeniz gerekir. Bu yeniden başlatma, yeni etkinleştirilmiş sistem hizmetinin her düğüm üzerinde başlatılmış ve çalışır olmasını sağlar. Aşağıdaki kod parçacığında, forceRestart önemli ayardır; ayarların geri kalanı için mevcut değerlerinizi kullanın.
```json
"upgradeDescription": {
    "forceRestart": true,
    "healthCheckRetryTimeout": "00:45:00",
    "healthCheckStableDuration": "00:05:00",
    "healthCheckWaitDuration": "00:05:00",
    "upgradeDomainTimeout": "02:00:00",
    "upgradeReplicaSetCheckTimeout": "1.00:00:00",
    "upgradeTimeout": "12:00:00"
}
```
- Anahtar Kasası 'na uygulamanın yönetilen kimlik erişim iznini verme

    Anahtar Kasası 'na yönetilen kimlik erişimi vermeyi öğrenmek için bu [belgeye](how-to-grant-access-other-resources.md) başvurun. Ayrıca, sistem tarafından atanan yönetilen kimlik kullanıyorsanız, yönetilen kimlik yalnızca uygulama dağıtımından sonra oluşturulur.

## <a name="keyvault-secret-as-application-parameter"></a>Uygulama parametresi olarak Anahtar Kasası gizli anahtarı
Uygulamanın, Anahtar Kasası 'nda depolanan arka uç veritabanı parolasını okuması gerektiğini, Service Fabric KeyVaultReference desteğinin de kolaylaştırdığını varsayalım. Aşağıdaki örnek, `DBPassword` Service Fabric KeyVaultReference desteğini kullanarak keykasasından gizli dizi okur.

- settings.xml bir bölüm ekleyin

    `DBPassword`Türü ve değeri olan parametreyi tanımlayın `KeyVaultReference``<KeyVaultURL>`

    ```xml
    <Section Name="dbsecrets">
        <Parameter Name="DBPassword" Type="KeyVaultReference" Value="https://vault200.vault.azure.net/secrets/dbpassword/8ec042bbe0ea4356b9b171588a8a1f32"/>
    </Section>
    ```
- İçindeki ApplicationManifest.xml yeni bölüme başvur`<ConfigPackagePolicies>`

    ```xml
    <ServiceManifestImport>
        <Policies>
        <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="ttkappuser" />
        <ConfigPackagePolicies CodePackageRef="Code">
            <!--Linux container example-->
            <ConfigPackage Name="Config" SectionName="dbsecrets" EnvironmentVariableName="SecretPath" MountPoint="/var/secrets"/>
            <!--Windows container example-->
            <!-- <ConfigPackage Name="Config" SectionName="dbsecrets" EnvironmentVariableName="SecretPath" MountPoint="C:\secrets"/> -->
        </ConfigPackagePolicies>
        </Policies>
    </ServiceManifestImport>
    ```

- Uygulamanızda KeyVaultReference kullanma

    Hizmet örneklemesi Service Fabric, uygulamanın yönetilen kimliğini kullanarak KeyVaultReference parametresini çözer. Altında listelenen her bir parametre, `<Section  Name=dbsecrets>` EnvironmentVariable SecretPath tarafından işaret edilen klasör altında bir dosya olacaktır. C# kod parçacığı altında, uygulamanızda DBPassword 'ın nasıl okunacağı gösterilmektedir.

    ```C#
    string secretPath = Environment.GetEnvironmentVariable("SecretPath");
    using (StreamReader sr = new StreamReader(Path.Combine(secretPath, "DBPassword"))) 
    {
        string dbPassword =  sr.ReadToEnd();
        // dbPassword to connect to DB
    }
    ```
    > [!NOTE] 
    > Kapsayıcı senaryosu için bağlama noktasını kullanarak nerede `secrets` takılmasını kontrol edebilirsiniz.

## <a name="keyvault-secret-as-environment-variable"></a>Ortam değişkeni olarak Anahtar Kasası gizli anahtarı

Service Fabric ortam değişkenleri artık KeyVaultReference türünü destekliyor. örnek aşağıdaki örnekte, bir ortam değişkeninin Anahtar Kasası 'nda depolanan bir gizli dizi ile nasıl bağlanacağı gösterilmektedir.

```xml
<EnvironmentVariables>
      <EnvironmentVariable Name="EventStorePassword" Type="KeyVaultReference" Value="https://ttkvault.vault.azure.net/secrets/clustercert/e225bd97e203430d809740b47736b9b8"/>
</EnvironmentVariables>
```

```C#
string eventStorePassword =  Environment.GetEnvironmentVariable("EventStorePassword");
```
## <a name="keyvault-secret-as-container-repository-password"></a>Kapsayıcı deposu parolası olarak Anahtar Kasası gizli anahtarı
KeyVaultReference, kapsayıcı havuzunun kimlik bilgileri için desteklenen bir türdür, aşağıdaki örnek, bir Anahtar Kasası başvurusunun kapsayıcı deposu parolası olarak nasıl kullanılacağını göstermektedir.
```xml
 <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="user1" Type="KeyVaultReference" Password="https://ttkvault.vault.azure.net/secrets/containerpwd/e225bd97e203430d809740b47736b9b8"/>
      </ContainerHostPolicies>
```
## <a name="faq"></a>SSS
- KeyVaultReference desteği için yönetilen kimliğin etkinleştirilmesi gerekir, anahtar Vaultreference yönetilen kimliği etkinleştirmeden kullanılırsa, uygulama etkinleştirme başarısız olur.

- Sistem tarafından atanan kimlik kullanıyorsanız, bu, yalnızca uygulama dağıtıldıktan sonra oluşturulur ve bu, döngüsel bir bağımlılık oluşturur. Uygulamanız dağıtıldıktan sonra, Anahtar Kasası 'na sisteme atanmış kimlik erişimi izni verebilirsiniz. Sistem tarafından atanan kimliği {Cluster}/{Application Name}/{ServiceName} adıyla bulabilirsiniz

- Keykasasının, Service Fabric kümeniz ile aynı abonelikte olması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Keykasası belgeleri](../key-vault/index.yml)
