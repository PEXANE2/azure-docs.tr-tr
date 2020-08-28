---
title: Sertifikaları bir kapsayıcıya aktarma
description: Sertifika dosyalarını Service Fabric bir kapsayıcı hizmetine aktarmak için hemen öğrenin.
ms.topic: conceptual
ms.date: 2/23/2018
ms.custom: devx-track-csharp
ms.openlocfilehash: 426aa2ebbfb87fe2c80e0d1aff3eeecbe0e2472d
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89050752"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Service Fabric çalıştıran bir kapsayıcıya bir sertifika dosyasını içeri aktarma

> [!NOTE]
> Azure üzerinde çalışan Service Fabric kümeleri için, bir kapsayıcı içinden uygulama sertifikaları sağlamak üzere [Service Fabric uygulama tarafından yönetilen kimlik](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity) kullanılması önerilir. Yönetilen kimlik, hizmet düzeyinde gizli dizi ve sertifika yalıtımı sağlar ve uygulama sertifikası sağlamasının altyapının iş akışı yerine uygulamanın iş akışının bir parçası olmasını sağlar. CertificateRef mekanizması gelecek bir sürümde kullanım dışı olacaktır.

Bir sertifika belirterek kapsayıcı hizmetlerinizi güvenli hale getirebilirsiniz. Service Fabric, bir kapsayıcı içindeki hizmetler için bir Windows veya Linux kümesindeki düğümlere yüklenmiş bir sertifikaya (sürüm 5,7 veya üzeri) erişmek için bir mekanizma sağlar. Sertifikanın, kümenin tüm düğümlerinde LocalMachine altındaki bir sertifika deposunda yüklü olması gerekir. Sertifikaya karşılık gelen özel anahtar kullanılabilir, erişilebilir ve Windows-dışarı aktarılabilir olmalıdır. Aşağıdaki kod parçacığında gösterildiği gibi, sertifika bilgileri etiket altındaki uygulama bildiriminde verilmiştir `ContainerHostPolicies` :

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Windows kümeleri için, uygulamayı başlatırken, çalışma zamanı her bir başvurulan sertifikayı ve karşılık gelen özel anahtarını, rastgele oluşturulmuş bir parolayla güvenliği sağlanmış bir PFX dosyasına dışarı aktarır. Sırasıyla PFX ve parola dosyalarına aşağıdaki ortam değişkenleri kullanılarak kapsayıcı içinde erişilebilir: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

Linux kümelerinde, Sertifikalar (pek), X509StoreName tarafından belirtilen mağazadan üzerine, kapsayıcıya kopyalanır. Linux üzerinde karşılık gelen ortam değişkenleri şunlardır:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

Hem hem de `PEM` `PrivateKey` dosyasının sertifika ve şifrelenmemiş özel anahtar içerdiğini lütfen unutmayın.

Alternatif olarak, gerekli formda sertifikalara zaten sahipseniz ve kapsayıcının içinde bu sertifikaya erişmek istiyorsanız, uygulama paketinizdeki bir veri paketi oluşturabilir ve uygulama bildiriminiz içinde aşağıdakileri belirtebilirsiniz:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

Kapsayıcı hizmeti veya işlemi, sertifika dosyalarının kapsayıcıya aktarılmasından sorumludur. Sertifikayı içeri aktarmak için `setupentrypoint.sh` betikleri kullanabilir veya kapsayıcı işlemi içinde özel kod yürütebilirsiniz. Aşağıda PFX dosyasını içeri aktarmak Için C# dilinde örnek kod verilmiştir:

```csharp
string certificateFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_PFX");
string passwordFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_Password");
X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
password = password.Replace("\0", string.Empty);
X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
store.Open(OpenFlags.ReadWrite);
store.Add(cert);
store.Close();
```
Bu PFX sertifikası uygulamanın veya hizmetin kimliğini doğrulamak veya diğer hizmetlerle güvenli iletişim sağlamak için kullanılabilir. Varsayılan olarak, dosyalar yalnızca SISTEME erişim sağlar. Hizmet tarafından istenen diğer hesaplara ACL ekleyebilirsiniz.

Sonraki adım olarak, aşağıdaki makaleleri okuyun:

* [Windows Server 2016 ' de Windows kapsayıcısını Service Fabric dağıtma](service-fabric-get-started-containers.md)
* [Linux üzerinde Service Fabric bir Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
