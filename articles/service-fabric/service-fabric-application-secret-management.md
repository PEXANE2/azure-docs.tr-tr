---
title: Azure Service Fabric uygulama gizli dizilerini yönetme
description: Service Fabric uygulamasındaki gizli değerlerin güvenliğini nasıl sağlayacağınızı öğrenin (platformdan bağımsız).
ms.topic: conceptual
ms.date: 01/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: a11869c3b606ed9e74ce4f598109139fa1bb4164
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89012832"
---
# <a name="manage-encrypted-secrets-in-service-fabric-applications"></a>Service Fabric uygulamalarında şifreli gizli dizileri yönetme
Bu kılavuz, Service Fabric uygulamasında gizli dizileri yönetme adımlarında size yol gösterir. Gizlilikler, depolama bağlantı dizeleri, parolalar veya düz metin olarak işlenmemelidir diğer değerler gibi herhangi bir hassas bilgi olabilir.

Service Fabric uygulamasında şifrelenmiş gizli dizileri kullanmak üç adımdan oluşur:
* Bir şifreleme sertifikası kurun ve gizli dizileri şifreleyin.
* Bir uygulamada şifrelenmiş gizli dizileri belirtin.
* Şifrelenmiş gizli dizileri hizmet kodundan çözün.

## <a name="set-up-an-encryption-certificate-and-encrypt-secrets"></a>Şifreleme sertifikası ayarlama ve gizli dizileri şifreleme
Şifreleme sertifikası ayarlama ve parolaları şifrelemek için kullanma, Windows ve Linux arasında farklılık gösterir.
* [Windows kümelerinde şifreleme sertifikası ayarlayın ve gizli dizileri şifreleyin.][secret-management-windows-specific-link]
* [Bir şifreleme sertifikası kurun ve Linux kümelerinde gizli dizileri şifreleyin.][secret-management-linux-specific-link]

## <a name="specify-encrypted-secrets-in-an-application"></a>Bir uygulamada şifrelenmiş gizli dizileri belirtme
Önceki adımda, bir sertifika ile gizli dizi şifrelemesini ve bir uygulamada kullanılmak üzere bir Base-64 kodlu dizeyi oluşturmayı açıklar. Bu temel 64 kodlu dize, bir hizmetin Settings.xml şifrelenmiş bir [parametre][parameters-link] olarak veya bir hizmetin ServiceManifest.xml şifreli bir [ortam değişkeni][environment-variables-link] olarak belirtilebilir.

Hizmetinizin Settings.xml yapılandırma dosyasında, özniteliği olarak ayarlanmış şifreli bir [parametre][parameters-link] belirtin `IsEncrypted` `true` :

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```
Hizmetinizin ServiceManifest.xml dosyasında, özniteliği olarak ayarlanmış bir şifrelenmiş [ortam değişkeni][environment-variables-link] belirtin `Type` `Encrypted` :
```xml
<CodePackage Name="Code" Version="1.0.0">
  <EnvironmentVariables>
    <EnvironmentVariable Name="MyEnvVariable" Type="Encrypted" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </EnvironmentVariables>
</CodePackage>
```

Gizli dizi, uygulama bildiriminde bir sertifika belirtilerek Service Fabric uygulamanıza da eklenmelidir. **ApplicationManifest.xml** bir **secretscercertificate** öğesi ekleyin ve istenen sertifikanın parmak izini ekleyin.

```xml
<ApplicationManifest … >
  ...
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```
> [!NOTE]
> Bir Secretscercertificate öğesini belirten uygulamayı etkinleştirdikten sonra, Service Fabric eşleşen sertifikayı bulacak ve uygulamanın özel anahtarına tam izinler altında uygulamanın çalıştırdığı kimliğe izin vermiş olur. Service Fabric Ayrıca sertifikayı değişiklikler için izler ve izinleri uygun şekilde yeniden uygular. Ortak ad tarafından belirtilen sertifikalara yönelik değişiklikleri algılamak için, Service Fabric tüm eşleşen sertifikaları bulan düzenli bir görevi çalıştırır ve önbelleğe alınmış bir parmak izi listesiyle karşılaştırır. Yeni bir parmak izi algılandığında, bu konu tarafından bir sertifikanın yenilenmesi anlamına gelir. Görev, kümenin her bir düğümünde dakikada bir kez çalışır.
>
> Secretscercertificate, konu tabanlı bildirimlere izin verse de, şifrelenmiş ayarların istemcideki ayarı şifrelemek için kullanılan anahtar çiftine bağlı olduğunu unutmayın. Özgün şifreleme sertifikasının (veya eşdeğer) konu tabanlı bildirimle eşleştiğinden ve uygulamanın barındırabileceği kümenin her düğümüne karşılık gelen özel anahtarı da dahil olmak üzere yüklü olduğundan emin olmanız gerekir. Konu tabanlı bildirimle eşleşen ve özgün şifreleme sertifikasıyla aynı anahtar çiftiyle oluşturulan tüm zaman geçerli sertifikaları eşdeğerleri kabul edilir.
>

### <a name="inject-application-secrets-into-application-instances"></a>Uygulama örneklerine uygulama gizli dizilerini Ekle
İdeal olarak, farklı ortamlara dağıtım mümkün olduğunca otomatikleştirilebilir. Bu, bir derleme ortamında gizli şifreleme gerçekleştirerek ve uygulama örnekleri oluştururken şifreli gizli dizileri parametre olarak sağlayarak gerçekleştirilebilir.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Settings.xml geçersiz kılınabilir parametreler kullan
Settings.xml yapılandırma dosyası, uygulama oluşturma zamanında sağlanabilen geçersiz kılınabilir parametrelere izin verir. `MustOverride`Bir parametre için değer sağlamak yerine özniteliğini kullanın:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

Settings.xml değerleri geçersiz kılmak için, ApplicationManifest.xml hizmet için bir geçersiz kılma parametresi bildirin:

```xml
<ApplicationManifest ... >
  <Parameters>
    <Parameter Name="MySecret" DefaultValue="" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides>
      <ConfigOverride Name="Config">
        <Settings>
          <Section Name="MySettings">
            <Parameter Name="MySecret" Value="[MySecret]" IsEncrypted="true" />
          </Section>
        </Settings>
      </ConfigOverride>
    </ConfigOverrides>
  </ServiceManifestImport>
 ```

Şimdi, uygulamanın bir örneği oluşturulurken değer bir *uygulama parametresi* olarak belirtilebilir. Bir yapı sürecinde kolay tümleştirme için, bir uygulama örneğinin oluşturulması PowerShell kullanılarak betikleştirilmiş veya C# dilinde yazılabilir.

PowerShell kullanarak, parametresi `New-ServiceFabricApplication` komut için bir [karma tablo](/previous-versions/windows/it-pro/windows-powershell-1.0/ee692803(v=technet.10))olarak sağlanır:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

C# kullanarak uygulama parametreleri `ApplicationDescription` şöyle belirtilir `NameValueCollection` :

```csharp
FabricClient fabricClient = new FabricClient();

NameValueCollection applicationParameters = new NameValueCollection();
applicationParameters["MySecret"] = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=";

ApplicationDescription applicationDescription = new ApplicationDescription(
    applicationName: new Uri("fabric:/MyApp"),
    applicationTypeName: "MyAppType",
    applicationTypeVersion: "1.0.0",
    applicationParameters: applicationParameters)
);

await fabricClient.ApplicationManager.CreateApplicationAsync(applicationDescription);
```

## <a name="decrypt-encrypted-secrets-from-service-code"></a>Şifrelenmiş gizli dizileri hizmet kodundan çöz
[Parametrelere][parameters-link] ve [ortam değişkenlerine][environment-variables-link] erişmek için API 'ler, şifrelenmiş değerlerin kolay bir şekilde çözülmesi için izin verir. Şifrelenmiş dize, şifreleme için kullanılan sertifikayla ilgili bilgiler içerdiğinden, sertifikayı el ile belirtmeniz gerekmez. Sertifikanın yalnızca hizmetin üzerinde çalıştığı düğüme yüklenmesi gerekir.

```csharp
// Access decrypted parameters from Settings.xml
ConfigurationPackage configPackage = FabricRuntime.GetActivationContext().GetConfigurationPackageObject("Config");
bool MySecretIsEncrypted = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].IsEncrypted;
if (MySecretIsEncrypted)
{
    SecureString MySecretDecryptedValue = configPackage.Settings.Sections["MySettings"].Parameters["MySecret"].DecryptValue();
}

// Access decrypted environment variables from ServiceManifest.xml
// Note: you do not have to call any explicit API to decrypt the environment variable.
string MyEnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

## <a name="next-steps"></a>Sonraki adımlar
* Service Fabric [gizli dizi deposu](service-fabric-application-secret-store.md) 
* [Uygulama ve hizmet güvenliği](service-fabric-application-and-service-security.md) hakkında daha fazla bilgi edinin

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[service fabric secrets store]: service-fabric-application-secret-store.md
