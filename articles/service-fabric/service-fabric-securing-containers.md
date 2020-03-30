---
title: Sertifikaları kapsayıcıya alma
description: Sertifika dosyalarını Service Fabric konteyner hizmetine aktarmayı şimdi öğrenin.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: da4babd8f9d1a25a8514d0c6f1526b43a9723854
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614120"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Sertifika dosyalarını Hizmet Kumaşı üzerinde çalışan bir kapsayıcıya alma

Bir sertifika belirterek konteyner hizmetlerinizi güvence altına alabilirsiniz. Service Fabric, Windows veya Linux kümesindeki düğümlere (sürüm 5.7 veya daha yüksek) yüklenen bir sertifikaya erişmek için kapsayıcı içindeki hizmetler için bir mekanizma sağlar. Sertifika, cluster'ın tüm düğümlerine LocalMachine altında bir sertifika deposuna yüklenmelidir. Sertifikaya karşılık gelen özel anahtar kullanılabilir, erişilebilir ve - Windows'da - dışa aktarılabilir olmalıdır. Sertifika bilgileri, aşağıdaki snippet'in gösterdiği gibi `ContainerHostPolicies` etiketin altındaki başvuru bildiriminde verilmiştir:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Windows kümeleri için, uygulamayı başlatırken, çalışma zamanı başvurulan her sertifikayı ve karşılık gelen özel anahtarı rasgele oluşturulan bir parolayla güvenli bir PFX dosyasına dışa aktarır. PFX ve parola dosyalarına sırasıyla aşağıdaki ortam değişkenleri kullanılarak kapsayıcı nın içinde erişilebilir: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

Linux kümeleri için sertifikalar (PEM) X509StoreName tarafından belirtilen mağazadan kapsayıcıya kopyalanır. Linux'ta karşılık gelen ortam değişkenleri şunlardır:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

Alternatif olarak, sertifikaları zaten gerekli formda aldıysanız ve kapsayıcının içinde erişmek istiyorsanız, uygulama paketinizin içinde bir veri paketi oluşturabilir ve uygulama bildiriminizin içinde aşağıdakileri belirtebilirsiniz:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

Kapsayıcı hizmeti veya işlemi, sertifika dosyalarını kapsayıcıya almakla yükümlüdür. Sertifikayı almak için komut `setupentrypoint.sh` dosyalarını kullanabilir veya kapsayıcı işlemi içinde özel kod yürütebilirsiniz. PFX dosyasını almak için C# örnek kodu aşağıda veda edebilirsiniz:

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
Bu PFX sertifikası, uygulamanın veya hizmetin doğruluğunu doğrulamak veya diğer hizmetlerle güvenli iletişim sağlamak için kullanılabilir. Varsayılan olarak, dosyalar yalnızca System'e ait olur. Hizmetin gerektirdiği şekilde diğer hesaplara acl layabilirsiniz.

Bir sonraki adım olarak, aşağıdaki makaleleri okuyun:

* [Windows Server 2016'da Service Fabric'e Windows kapsayıcısı dağıtma](service-fabric-get-started-containers.md)
* [Linux'ta Service Fabric'e Docker konteyneri dağıtma](service-fabric-get-started-containers-linux.md)
