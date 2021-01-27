---
title: Azure Service Fabric-Service Fabric uygulama Anahtar Kasası başvurularını kullanma
description: Bu makalede, uygulama gizli dizileri için Service Fabric KeyVaultReference desteğinin nasıl kullanılacağı açıklanmaktadır.
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: a0e4ef0decae8cc9ab4dc5f8c69dfef854af81f3
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98898605"
---
# <a name="keyvaultreference-support-for-azure-deployed-service-fabric-applications"></a>Azure tarafından dağıtılan Service Fabric uygulamaları için KeyVaultReference desteği

Bulut uygulamaları oluştururken yaygın bir zorluk, gizli dizileri uygulamalarınıza güvenle dağıtmaktır. Örneğin, işlem hattı sırasında veya işlecine göre anahtar olmadan uygulamanıza bir veritabanı anahtarı dağıtmak isteyebilirsiniz. Service Fabric KeyVaultReference desteği, Key Vault depolanan gizli dizinin URL 'sine başvurarak, uygulamalarınıza gizli dizileri dağıtmayı kolaylaştırır. Service Fabric, bu gizli anahtarı uygulamanızın yönetilen kimliği adına getirmeyi ve uygulamayı gizli anahtar ile etkinleştirmeyi işleymeyecektir.

> [!NOTE]
> Service Fabric uygulamalar için KeyVaultReference desteği, Service Fabric sürüm 7,2 ' den başlayarak GA 'dir (Önizleme dışı). Bu özelliği kullanmadan önce bu sürüme yükseltmeniz önerilir.

> [!NOTE]
> Service Fabric uygulamalar için KeyVaultReference desteği yalnızca [Sürümlenmiş](../key-vault/general/about-keys-secrets-certificates.md#objects-identifiers-and-versioning) gizli dizileri destekler. Sürümsuz gizlilikler desteklenmez. Key Vault Service Fabric kümeniz ile aynı abonelikte olması gerekir. 

## <a name="prerequisites"></a>Önkoşullar

- Service Fabric uygulamalar için yönetilen kimlik

    Service Fabric KeyVaultReference support, uygulama adına gizli dizileri getirmek için uygulamanın yönetilen kimliğini kullanır, bu nedenle uygulamanız yönetilen bir kimlik aracılığıyla dağıtılmalıdır ve atanmalıdır. Uygulamanız için yönetilen kimliği etkinleştirmek üzere bu [belgeyi](concepts-managed-identity.md) izleyin.

- Orta gizli dizi deposu (CSS).

    Orta gizli dizi deposu (CSS) Service Fabric şifreli yerel gizli diziler önbelleğidir. Bu özellik, Key Vault alındıktan sonra gizli dizileri korumak ve kalıcı hale getirmek için CSS kullanır. Bu isteğe bağlı sistem hizmetinin etkinleştirilmesi, bu özelliği kullanmak için de gereklidir. CSS 'yi etkinleştirmek ve yapılandırmak için bu [belgeyi](service-fabric-application-secret-store.md) izleyin.

- Anahtar Kasası 'na uygulamanın yönetilen kimlik erişim iznini verme

    Anahtar Kasası 'na yönetilen kimlik erişimi vermeyi öğrenmek için bu [belgeye](how-to-grant-access-other-resources.md) başvurun. Ayrıca, sistem tarafından atanan yönetilen kimlik kullanıyorsanız, yönetilen kimlik yalnızca uygulama dağıtımından sonra oluşturulur. Bu, kimliğin kasaya erişim izni verilmeden önce uygulamanın gizli erişime erişmeye çalıştığı yarış koşullarını oluşturabilir. Sisteme atanan kimliğin adı olacaktır `{cluster name}/{application name}/{service name}` .
    
## <a name="use-keyvaultreferences-in-your-application"></a>Uygulamanızda KeyVaultReferences kullanın
KeyVaultReferences çeşitli yollarla tüketilebilir
- [Bir ortam değişkeni olarak](#as-an-environment-variable)
- [Kapsayıcınıza dosya olarak takılmış](#mounted-as-a-file-into-your-container)
- [Bir kapsayıcı deposu parolasına başvuru olarak](#as-a-reference-to-a-container-repository-password)

### <a name="as-an-environment-variable"></a>Bir ortam değişkeni olarak

```xml
<EnvironmentVariables>
      <EnvironmentVariable Name="MySecret" Type="KeyVaultReference" Value="<KeyVaultURL>"/>
</EnvironmentVariables>
```

```C#
string secret =  Environment.GetEnvironmentVariable("MySecret");
```

### <a name="mounted-as-a-file-into-your-container"></a>Kapsayıcınıza dosya olarak takılmış

- settings.xml bir bölüm ekleyin

    `MySecret`Türü ve değeri olan parametreyi tanımlayın `KeyVaultReference``<KeyVaultURL>`

    ```xml
    <Section Name="MySecrets">
        <Parameter Name="MySecret" Type="KeyVaultReference" Value="<KeyVaultURL>"/>
    </Section>
    ```

- İçindeki ApplicationManifest.xml yeni bölüme başvur `<ConfigPackagePolicies>`

    ```xml
    <ServiceManifestImport>
        <Policies>
        <IdentityBindingPolicy ServiceIdentityRef="MyServiceMI" ApplicationIdentityRef="MyApplicationMI" />
        <ConfigPackagePolicies CodePackageRef="Code">
            <!--Linux container example-->
            <ConfigPackage Name="Config" SectionName="MySecrets" EnvironmentVariableName="SecretPath" MountPoint="/var/secrets"/>
            <!--Windows container example-->
            <!-- <ConfigPackage Name="Config" SectionName="dbsecrets" EnvironmentVariableName="SecretPath" MountPoint="C:\secrets"/> -->
        </ConfigPackagePolicies>
        </Policies>
    </ServiceManifestImport>
    ```

- Hizmet kodundaki gizli dizileri tüketme

    Altında listelenen her bir parametre, `<Section  Name=MySecrets>` EnvironmentVariable SecretPath tarafından işaret edilen klasör altında bir dosya olacaktır. Aşağıdaki C# kod parçacığı, uygulamanızdan MySecret 'in nasıl okunacağını gösterir.

    ```C#
    string secretPath = Environment.GetEnvironmentVariable("SecretPath");
    using (StreamReader sr = new StreamReader(Path.Combine(secretPath, "MySecret"))) 
    {
        string secret =  sr.ReadToEnd();
    }
    ```
    > [!NOTE] 
    > Bağlama noktası, gizli değerleri içeren dosyaların takılcağı klasörü denetler.

### <a name="as-a-reference-to-a-container-repository-password"></a>Bir kapsayıcı deposu parolasına başvuru olarak

```xml
 <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="MyACRUser" Type="KeyVaultReference" Password="<KeyVaultURL>"/>
      </ContainerHostPolicies>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Keykasası belgeleri](../key-vault/index.yml)
* [Merkezi gizli depo hakkında bilgi edinin](service-fabric-application-secret-store.md)
* [Service Fabric uygulamalar için yönetilen kimlik hakkında bilgi edinin](concepts-managed-identity.md)
