---
title: Azure Servis Kumaşı - Servis Kumaşı kullanma uygulaması KeyVault referansları
description: Bu makalede, uygulama sırları için hizmet-kumaş KeyVaultReference desteği nasıl kullanılacağı açıklanmaktadır.
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: f7d8a083ea5ec4b66c29d392ee98927915465875
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76545492"
---
#  <a name="keyvaultreference-support-for-service-fabric-applications-preview"></a>Service Fabric uygulamaları için KeyVaultReference desteği (önizleme)

Bulut uygulamaları yaparken karşılaşılan yaygın bir sorun, uygulamanızın gerektirdiği sırları güvenli bir şekilde nasıl depolayabilmektir. Örneğin, kapsayıcı depo kimlik bilgilerini keyvault'ta depolamak ve uygulama bildiriminde başvurmak isteyebilirsiniz. Service Fabric KeyVaultReference, Service Fabric Managed Identity kullanır ve keyvault sırlarına başvuruyapmayı kolaylaştırır. Bu makalenin geri kalanı, Service Fabric KeyVaultReference'ın nasıl kullanılacağını ayrıntılarıyla açıklar ve bazı tipik kullanımları içerir.

## <a name="prerequisites"></a>Ön koşullar

- Uygulama için Yönetilen Kimlik (MIT)
    
    Service Fabric KeyVaultReference desteği uygulamanın Yönetilen Kimliğini kullanır ve bu nedenle KeyVaultReferences'ı kullanmayı planlayan uygulamalar Yönetilen Kimlik'i kullanmalıdır. Uygulamanız için yönetilen kimliği etkinleştirmek için bu [belgeyi](concepts-managed-identity.md) izleyin.

- Merkezi Secrets Store (CSS).

    Merkezi Secrets Store (CSS) Service Fabric şifreli yerel sırlar önbelleğidir. CSS, parola, belirteçler ve anahtarlar gibi hassas verileri bellekte şifrelenmiş tutan yerel bir gizli depo önbelleğidir. KeyVaultReference, bir kez getirilen, CSS önbelleğe alınır.

    KeyVaultReference desteği için `fabricSettings` gerekli tüm özellikleri etkinleştirmek için aşağıdaki küme yapılandırmanıza ekleyin.

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
    > CSS için ayrı bir şifreleme sertifikası kullanılması önerilir. "CentralSecretService" bölümüne ekleyebilirsiniz.
    

    ```json
        {
            "name": "EncryptionCertificateThumbprint",
            "value": "<EncryptionCertificateThumbprint for CSS>"
        }
    ```
Değişikliklerin etkili olması için, yükseltme küme de ilerledikçe her düğümde Hizmet Kumaşı çalışma zamanının güçlü bir şekilde yeniden başlatılmasını belirtmek için yükseltme ilkesini değiştirmeniz gerekir. Bu yeniden başlatma, yeni etkinleştirilen sistem hizmetinin başlatılmasını ve her düğümüzerinde çalışmasını sağlar. Aşağıdaki snippet'te forceRestart temel ayardır; ayarların geri kalanı için varolan değerleri kullanın.
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
- Başvurunun yönetilen kimlik erişim iznini keyvault'a verme

    Keyvault'a yönetilen kimlik erişiminin nasıl verilebildiğini görmek için bu [belgeye](how-to-grant-access-other-resources.md) başvurun. Ayrıca, Sistem Atanmış Yönetilen Kimlik kullanıyorsanız, yönetilen kimlik yalnızca uygulama dağıtımından sonra oluşturulur.

## <a name="keyvault-secret-as-application-parameter"></a>Uygulama parametresi olarak keyvault gizli
Diyelim ki uygulama nın keyvault'ta saklanan arka uç veritabanı şifresini okuması gerekiyor, Service Fabric KeyVaultReference desteği bunu kolaylaştırıyor. Aşağıdaki örnekte Service Fabric KeyVaultReference desteğini kullanarak keyvault'tan gizli olarak okunur. `DBPassword`

- settings.xml'ye bölüm ekle

    Tür `DBPassword` `KeyVaultReference` ve Değer ile parametre tanımlama`<KeyVaultURL>`

    ```xml
    <Section Name="dbsecrets">
        <Parameter Name="DBPassword" Type="KeyVaultReference" Value="https://vault200.vault.azure.net/secrets/dbpassword/8ec042bbe0ea4356b9b171588a8a1f32"/>
    </Section>
    ```
- ApplicationManifest.xml'deki yeni bölüme başvurun`<ConfigPackagePolicies>`

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

- Uygulamanızda KeyVaultReference'ı kullanma

    Servis anında Servis Kumaşı, uygulamanın yönetilen kimliğini kullanarak KeyVaultReference Parametresini çözecektir. Altında `<Section  Name=dbsecrets>` listelenen her parametre, EnvironmentVariable SecretPath tarafından işaret edilen klasörün altında bir dosya olacaktır. Aşağıda C# kod snippet nasıl uygulamanızda DBPassword okumak için gösterir.

    ```C#
    string secretPath = Environment.GetEnvironmentVariable("SecretPath");
    using (StreamReader sr = new StreamReader(Path.Combine(secretPath, "DBPassword"))) 
    {
        string dbPassword =  sr.ReadToEnd();
        // dbPassword to connect to DB
    }
    ```
    > [!NOTE] 
    > Kapsayıcı senaryosu için, monte edilecek yeri `secrets` denetlemek için MountPoint'i kullanabilirsiniz.

## <a name="keyvault-secret-as-environment-variable"></a>Çevre değişkeni olarak Keyvault sırrı

Service Fabric ortam değişkenleri artık KeyVaultReference türünü destekler, aşağıdaki örnekte bir ortam değişkeninin KeyVault'ta depolanan bir gizliye nasıl bağlanılmak üzere değiştirileceği gösterilmektedir.

```xml
<EnvironmentVariables>
      <EnvironmentVariable Name="EventStorePassword" Type="KeyVaultReference" Value="https://ttkvault.vault.azure.net/secrets/clustercert/e225bd97e203430d809740b47736b9b8"/>
</EnvironmentVariables>
```

```C#
string eventStorePassword =  Environment.GetEnvironmentVariable("EventStorePassword");
```
## <a name="keyvault-secret-as-container-repository-password"></a>Konteyner deposu parolası olarak Keyvault gizli
KeyVaultReference kapsayıcı Depo Kimlik Bilgileri için desteklenen bir türdür, aşağıdaki örnekte bir keyvault başvurusu kapsayıcı deposu parolası olarak nasıl kullanılacağını gösterir.
```xml
 <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="user1" Type="KeyVaultReference" Password="https://ttkvault.vault.azure.net/secrets/containerpwd/e225bd97e203430d809740b47736b9b8"/>
      </ContainerHostPolicies>
```
## <a name="faq"></a>SSS
- KeyVaultReference desteği için yönetilen kimliğin etkinleştirilmesi gerekir, Yönetilen Kimlik etkinleştirmeden KeyVaultReference kullanılırsa uygulama etkinleştirme başarısız olur.

- Sistem atanmış kimlik kullanıyorsanız, yalnızca uygulama dağıtıldıktan sonra oluşturulur ve bu dairesel bir bağımlılık oluşturur. Başvurunuz dağıtıldıktan sonra, anahtar kasasına atanan sisteme kimlik erişim izni verebilirsiniz. {cluster}/{application name}/{servicename} adıyla atanan sistemi bulabilirsiniz.

- Keyvault'un servis kumaş kümenizle aynı abonelikte olması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure KeyVault Belgeleri](https://docs.microsoft.com/azure/key-vault/)
