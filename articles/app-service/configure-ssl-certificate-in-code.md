---
title: Kodda bir TLS/SSL sertifikası kullan
description: Kodunuzda istemci sertifikalarının nasıl kullanılacağını öğrenin. İstemci sertifikası ile uzak kaynaklarla kimlik doğrulaması yapın veya bunlarla şifreleme görevlerini çalıştırın.
ms.topic: article
ms.date: 11/04/2019
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: d76bac60bae11f0843d81de523030154af62a373
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80811704"
---
# <a name="use-a-tlsssl-certificate-in-your-code-in-azure-app-service"></a>Kodunuzda bir TLS/SSL sertifikası kullanın Azure App Service

Uygulama kodunuzda, [App Service için eklediğiniz ortak veya özel sertifikalara](configure-ssl-certificate.md)erişebilirsiniz. Uygulama kodunuz istemci olarak davranabilir ve sertifika kimlik doğrulaması gerektiren bir dış hizmete erişebilir ya da şifreleme görevlerini gerçekleştirmek zorunda kalabilir. Bu nasıl yapılır kılavuzunda, uygulama kodunuzda ortak veya özel sertifikaların nasıl kullanılacağı gösterilmektedir.

Kodunuzda sertifika kullanmaya yönelik bu yaklaşım, uygulamanızın **temel** katmanda veya üzerinde olmasını GEREKTIREN App Service TLS işlevselliğini kullanır. Uygulamanız **ücretsiz** veya **paylaşılan** katmanındaysa, [sertifika dosyasını uygulama deponuza dahil](#load-certificate-from-file)edebilirsiniz.

TLS/SSL sertifikalarınızı App Service yönettiğinizde, sertifikaları ve uygulama kodunuzu ayrı olarak koruyabilir ve hassas verilerinizi koruyabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu izlemek için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Uygulamanıza bir sertifika ekleyin](configure-ssl-certificate.md)

## <a name="find-the-thumbprint"></a>Parmak izini bulma

<a href="https://portal.azure.com" target="_blank">Azure Portal</a>, sol menüden **uygulama hizmetleri**' ni seçin  >  **\<app-name>** .

Uygulamanızın sol gezinti bölmesinde, **TLS/SSL ayarları**' nı seçin ve ardından **özel anahtar sertifikaları (. pfx** ) veya **ortak anahtar sertifikaları (. cer)** seçeneğini belirleyin.

Kullanmak istediğiniz sertifikayı bulun ve parmak izini kopyalayın.

![Sertifika parmak izini kopyalama](./media/configure-ssl-certificate/create-free-cert-finished.png)

## <a name="make-the-certificate-accessible"></a>Sertifikayı erişilebilir yapma

Uygulama kodunuzda bir sertifikaya erişmek için, `WEBSITE_LOAD_CERTIFICATES` <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>aşağıdaki komutu çalıştırarak parmak izini uygulama ayarına ekleyin:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Tüm sertifikalarınızı erişilebilir hale getirmek için değerini olarak ayarlayın `*` .

## <a name="load-certificate-in-windows-apps"></a>Windows uygulamalarında sertifika yükleme

`WEBSITE_LOAD_CERTIFICATES`Uygulama ayarı, belirtilen sertifikaları Windows sertifika deposundaki Windows barındırılan uygulamanız için erişilebilir hale getirir ve konum, [fiyatlandırma katmanına](overview-hosting-plans.md)bağlıdır:

- **Yalıtılmış** katman- [Yerel machine\.](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) 
- Diğer tüm katmanlar- [geçerli Kullanıcı\.](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores)

C# kodunda sertifika parmak izine göre sertifikaya erişirsiniz. Aşağıdaki kod parmak izine sahip bir sertifika yükler `E661583E8FABEF4C0BEF694CBC41C28FB81CD870` .

```csharp
using System;
using System.Linq;
using System.Security.Cryptography.X509Certificates;

string certThumbprint = "E661583E8FABEF4C0BEF694CBC41C28FB81CD870";
bool validOnly = false;

using (X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser))
{
  certStore.Open(OpenFlags.ReadOnly);

  X509Certificate2Collection certCollection = certStore.Certificates.Find(
                              X509FindType.FindByThumbprint,
                              // Replace below with your certificate's thumbprint
                              certThumbprint,
                              validOnly);
  // Get the first cert with the thumbprint
  X509Certificate2 cert = certCollection.OfType<X509Certificate>().FirstOrDefault();

  if (cert is null)
      throw new Exception($"Certificate with thumbprint {certThumbprint} was not found");

  // Use certificate
  Console.WriteLine(cert.FriendlyName);
  
  // Consider to call Dispose() on the certificate after it's being used, avaliable in .NET 4.6 and later
}
```

Java kodunda, "Windows-MY" mağazasındaki sertifikaya, konu ortak adı alanını kullanarak erişirsiniz (bkz. [ortak anahtar sertifikası](https://en.wikipedia.org/wiki/Public_key_certificate)). Aşağıdaki kod, bir özel anahtar sertifikasının nasıl yükleneceğini göstermektedir:

```java
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import java.security.KeyStore;
import java.security.cert.Certificate;
import java.security.PrivateKey;

...
KeyStore ks = KeyStore.getInstance("Windows-MY");
ks.load(null, null); 
Certificate cert = ks.getCertificate("<subject-cn>");
PrivateKey privKey = (PrivateKey) ks.getKey("<subject-cn>", ("<password>").toCharArray());

// Use the certificate and key
...
```

Windows sertifika deposu için desteklemeyen veya desteklemeyen diller için bkz. [dosyadan sertifika yükleme](#load-certificate-from-file).

## <a name="load-certificate-in-linux-apps"></a>Linux uygulamalarında sertifika yükleme

`WEBSITE_LOAD_CERTIFICATES`Uygulama ayarları, belirtilen sertifikaları Linux barındırılan uygulamalarınızın (özel kapsayıcı uygulamaları dahil) dosya olarak erişilebilir hale getirir. Dosyalar aşağıdaki dizinler altında bulunur:

- Özel Sertifikalar- `/var/ssl/private` ( `.p12` Dosyalar)
- Ortak Sertifikalar- `/var/ssl/certs` ( `.der` Dosyalar)

Sertifika dosyası adları, sertifika parmak izlerdir. Aşağıdaki C# kodu, bir Linux uygulamasına ortak bir sertifikanın nasıl yükleneceğini göstermektedir.

```csharp
using System;
using System.IO;
using System.Security.Cryptography.X509Certificates;

...
var bytes = File.ReadAllBytes("/var/ssl/certs/<thumbprint>.der");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

Node.js, PHP, Python, Java veya Ruby içindeki bir dosyadan bir TLS/SSL sertifikası yüklemeyi öğrenmek için ilgili dile veya Web platformuna yönelik belgelere bakın.

## <a name="load-certificate-from-file"></a>Sertifikayı dosyadan yükle

El ile karşıya yüklediğiniz bir sertifika dosyası yüklemeniz gerekiyorsa, örneğin [Git](deploy-local-git.md)yerine [FTPS](deploy-ftp.md) 'yi kullanarak sertifikayı karşıya yüklemek daha iyidir. Gizli verileri, kaynak denetiminden özel bir sertifika gibi tutmanız gerekir.

> [!NOTE]
> Windows üzerinde ASP.NET ve ASP.NET Core, bir dosyadan sertifika yükleseniz bile sertifika deposuna erişmelidir. Bir Windows .NET uygulamasına bir sertifika dosyası yüklemek için, geçerli kullanıcı profilini <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>aşağıdaki komutla yükleyin:
>
> ```azurecli-interactive
> az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
> ```
>
> Kodunuzda sertifika kullanmaya yönelik bu yaklaşım, uygulamanızın **temel** katmanda veya üzerinde olmasını GEREKTIREN App Service TLS işlevselliğini kullanır.

Aşağıdaki C# örneği, uygulamanızda göreli bir yoldan ortak bir sertifika yükler:

```csharp
using System;
using System.IO;
using System.Security.Cryptography.X509Certificates;

...
var bytes = File.ReadAllBytes("~/<relative-path-to-cert-file>");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

Node.js, PHP, Python, Java veya Ruby içindeki bir dosyadan bir TLS/SSL sertifikası yüklemeyi öğrenmek için ilgili dile veya Web platformuna yönelik belgelere bakın.

## <a name="more-resources"></a>Diğer kaynaklar

* [Azure App Service 'de TLS/SSL bağlaması ile özel bir DNS adının güvenliğini sağlama](configure-ssl-bindings.md)
* [HTTPS'yi zorunlu tutma](configure-ssl-bindings.md#enforce-https)
* [TLS 1.1/1.2 zorlama](configure-ssl-bindings.md#enforce-tls-versions)
* [SSS: sertifikalar App Service](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)
