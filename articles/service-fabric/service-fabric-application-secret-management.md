---
title: Azure Hizmet Kumaşı uygulama sırlarını yönetme
description: Service Fabric uygulamasında (platform-agnostik) gizli değerleri nasıl güvence altına alabildiğini öğrenin.
author: vturecek
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: vturecek
ms.openlocfilehash: 4d2138935122b9e08b21963519fce3f72466ab1f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414507"
---
# <a name="manage-encrypted-secrets-in-service-fabric-applications"></a>Service Fabric uygulamalarında şifrelenmiş sırları yönetme
Bu kılavuz, Service Fabric uygulamasında sırları yönetme adımlarında size yol göstersin. Sırlar, depolama bağlantı dizeleri, parolalar veya düz metinde işlenmemesi gereken diğer değerler gibi hassas bilgiler olabilir.

Service Fabric uygulamasında şifrelenmiş sırları kullanmak üç adım dan oluşur:
* Bir şifreleme sertifikası ayarlayın ve sırları şifreleyin.
* Bir uygulamada şifrelenmiş sırları belirtin.
* Servis kodundan şifrelenmiş sırların şifresini çöz.

## <a name="set-up-an-encryption-certificate-and-encrypt-secrets"></a>Şifreleme sertifikası ayarlama ve sırları şifreleme
Bir şifreleme sertifikası oluşturmak ve sırları şifrelemek için kullanmak Windows ve Linux arasında farklılık gösterir.
* [Windows kümelerinde bir şifreleme sertifikası ayarlayın ve sırları şifreleyin.][secret-management-windows-specific-link]
* [Bir şifreleme sertifikası ayarlayın ve Linux kümelerinde sırları şifreleyin.][secret-management-linux-specific-link]

## <a name="specify-encrypted-secrets-in-an-application"></a>Uygulamada şifrelenmiş sırları belirtin
Önceki adımda, bir sırrın sertifikayla nasıl şifrelenecek ve bir uygulamada kullanılmak üzere base-64 kodlanmış bir dize üretilme şekli açıklanmaktadır. Bu base-64 kodlanmış dize, bir hizmetin Settings.xml'inde şifrelenmiş [bir parametre][parameters-link] veya bir hizmetin ServiceManifest.xml'inde şifrelenmiş ortam [değişkeni][environment-variables-link] olarak belirtilebilir.

Hizmetinizin Settings.xml yapılandırma [dosyasında][parameters-link] şu `IsEncrypted` öznitelik te `true`sayar:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </Section>
</Settings>
```
Hizmetinizin ServiceManifest.xml [dosyasında][environment-variables-link] şu `Type` öznitelik te `Encrypted`sayar:
```xml
<CodePackage Name="Code" Version="1.0.0">
  <EnvironmentVariables>
    <EnvironmentVariable Name="MyEnvVariable" Type="Encrypted" Value="I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM=" />
  </EnvironmentVariables>
</CodePackage>
```

Sırlar, başvuru bildiriminde bir sertifika belirtilmek suretiyle Hizmet Kumaşı uygulamanıza da eklenmelidir. **ApplicationManifest.xml'e** **SecretsCertificate** öğesi ekleyin ve istediğiniz sertifikanın parmak izini ekleyin.

```xml
<ApplicationManifest … >
  ...
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```
> [!NOTE]
> SecretsCertificate belirten bir uygulama etkinleştirdikten sonra, Hizmet Kumaşeşleşen sertifikayı bulur ve başvurunun tam izinler altında sertifikanın özel anahtarına çalıştığını gösteren kimliği verir. Service Fabric ayrıca değişiklik sertifikasını izler ve izinleri buna göre yeniden uygular. Ortak ada göre bildirilen sertifikalar için değişiklikleri algılamak için, Service Fabric tüm eşleşen sertifikaları bulan ve önbelleğe alınmış bir parmak izi listesiyle karşılaştıran periyodik bir görev çalıştırır. Yeni bir parmak izi algılandığında, bu konuya göre bir sertifikanın yenilendiği anlamına gelir. Görev, kümenin her düğümünde dakikada bir kez çalışır.
>
> SecretsCertificate konu tabanlı bildirimlere izin verirken, şifrelenmiş ayarların istemcideki ayarı şifrelemek için kullanılan anahtar çiftine bağlı olduğunu unutmayın. Özgün şifreleme sertifikasının (veya eşdeğerinin) özne tabanlı bildirimle eşleştiğinden ve ilgili özel anahtarı da dahil olmak üzere, uygulamayı barındırabilecek kümenin her düğümüne yüklendiğinden emin olmalısınız. Özne tabanlı bildirimle eşleşen ve özgün şifreleme sertifikasıyla aynı anahtar çiftinden oluşturulmuş tüm zaman geçerliliği sertifikaları eşdeğerolarak kabul edilir.
>

### <a name="inject-application-secrets-into-application-instances"></a>Uygulama sırlarını uygulama örneklerine enjekte edin
İdeal olarak, farklı ortamlara dağıtım mümkün olduğunca otomatik olmalıdır. Bu, bir yapı ortamında gizli şifreleme gerçekleştirerek ve uygulama örnekleri oluştururken şifrelenmiş sırları parametre olarak sağlayarak gerçekleştirilebilir.

#### <a name="use-overridable-parameters-in-settingsxml"></a>Ayarlar.xml'de geçersiz kılınabilir parametreleri kullanın
Settings.xml yapılandırma dosyası, uygulama oluşturma zamanında sağlanabilecek aktarılabilir parametrelere izin verir. Bir `MustOverride` parametre için değer sağlamak yerine özniteliği kullanın:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MySettings">
    <Parameter Name="MySecret" IsEncrypted="true" Value="" MustOverride="true" />
  </Section>
</Settings>
```

Settings.xml'deki değerleri geçersiz kılmak için ApplicationManifest.xml'deki hizmet için geçersiz kılma parametresi bildirin:

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

Şimdi, uygulamanın bir örneğini oluştururken değer *uygulama parametresi* olarak belirtilebilir. Bir uygulama örneği oluşturma, bir yapı işleminde kolay tümleştirme için PowerShell kullanılarak komut dosyası oluşturulabilir veya C# olarak yazılabilir.

PowerShell kullanılarak, parametre `New-ServiceFabricApplication` komuta karma [tablo](https://technet.microsoft.com/library/ee692803.aspx)olarak verilir:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/MyApp -ApplicationTypeName MyAppType -ApplicationTypeVersion 1.0.0 -ApplicationParameter @{"MySecret" = "I6jCCAeYCAxgFhBXABFxzAt ... gNBRyeWFXl2VydmjZNwJIM="}
```

C# kullanılarak, uygulama parametreleri `ApplicationDescription` bir `NameValueCollection`olarak belirtilir:

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

## <a name="decrypt-encrypted-secrets-from-service-code"></a>Servis kodundan şifrelenmiş sırları çözme
[Parametrelere][parameters-link] ve [ortam değişkenlerine][environment-variables-link] erişmek için API'ler, şifrelenmiş değerlerin kolay şifresini çözmeye olanak sağlar. Şifrelenmiş dize şifreleme için kullanılan sertifika yla ilgili bilgiler içerdiğinden, sertifikayı el ile belirtmeniz gerekmez. Sertifikanın, hizmetin üzerinde çalışan düğüme yüklenmesi gerekir.

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
* Servis Kumaş [Sırları Mağaza](service-fabric-application-secret-store.md) 
* [Uygulama ve hizmet güvenliği](service-fabric-application-and-service-security.md) hakkında daha fazla bilgi edinin

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[service fabric secrets store]: service-fabric-application-secret-store.md