---
title: Kodda TLS/SSL sertifikası kullanma
description: Kodunuzda istemci sertifikalarını nasıl kullanacağınızı öğrenin. İstemci sertifikasına sahip uzak kaynaklarla kimlik doğrulaması yapın veya onlarla şifreleme görevleri çalıştırın.
ms.topic: article
ms.date: 11/04/2019
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: d76bac60bae11f0843d81de523030154af62a373
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811704"
---
# <a name="use-a-tlsssl-certificate-in-your-code-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde kodunuzda TLS/SSL sertifikası kullanma

Uygulama kodunuzda, Uygulama [Hizmeti'ne eklediğiniz genel veya özel sertifikalara](configure-ssl-certificate.md)erişebilirsiniz. Uygulama kodunuz bir istemci olarak hareket edebilir ve sertifika kimlik doğrulaması gerektiren harici bir hizmete erişebilir veya şifreleme görevleri gerçekleştirmesi gerekebilir. Bu nasıl yapılacağını kılavuzu, uygulama kodunuzdaki ortak veya özel sertifikaların nasıl kullanılacağını gösterir.

Kodunuzda sertifika kullanmaya bu yaklaşım, uygulamanızın **Temel** katmanda veya üzerinde olmasını gerektiren App Hizmeti'ndeki TLS işlevselliğini kullanır. Uygulamanız **Ücretsiz** veya **Paylaşılan** katmandaysa, [sertifika dosyasını uygulama deponuza ekleyebilirsiniz.](#load-certificate-from-file)

Uygulama Hizmeti'nin TLS/SSL sertifikalarınızı yönetmesine izin verdiğinde, sertifikaları ve uygulama kodunuzu ayrı olarak koruyabilir ve hassas verilerinizi koruyabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını takip etmek için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Uygulamanıza sertifika ekleme](configure-ssl-certificate.md)

## <a name="find-the-thumbprint"></a>Parmak izini bul

Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>sol menüden **Uygulama Hizmetleri** > **\<uygulama adını>' **yi seçin.

Uygulamanızın sol navigasyonundan **TLS/SSL ayarlarını**seçin, ardından **Özel Anahtar Sertifikaları (.pfx)** veya **Ortak Anahtar Sertifikaları 'nı (.cer)** seçin.

Kullanmak istediğiniz sertifikayı bulun ve parmak izini kopyalayın.

![Sertifikanın parmak izini kopyalama](./media/configure-ssl-certificate/create-free-cert-finished.png)

## <a name="make-the-certificate-accessible"></a>Sertifikayı erişilebilir hale getirin

Uygulama kodunuzdaki bir sertifikaya erişmek `WEBSITE_LOAD_CERTIFICATES` <a target="_blank" href="https://shell.azure.com" >için, Bulut Kabuğu'nda</a>aşağıdaki komutu çalıştırarak parmak izini uygulama ayarına ekleyin:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Tüm sertifikalarınızı erişilebilir kılmak için değeri `*`' ye ayarlayın.

## <a name="load-certificate-in-windows-apps"></a>Windows uygulamalarında yükleme sertifikası

Uygulama `WEBSITE_LOAD_CERTIFICATES` ayarı, belirtilen sertifikaları Windows sertifika deposunda Windows barındırılan uygulamanız için erişilebilir hale getirir ve konum [fiyatlandırma katmanına](overview-hosting-plans.md)bağlıdır:

- **İzole** katman - [Yerel Makine\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores). 
- Diğer tüm katmanlar - [Geçerli Kullanıcı\My](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores).

C# kodunda, sertifikaya sertifika parmak izinden erişebilirsiniz. Aşağıdaki kod, bir sertifikayı `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`parmak izi ile yükler.

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

Java kodunda, "Windows-MY" mağazasından Özne Ortak Adı alanını kullanarak sertifikaya erişirsiniz [(Bkz. Ortak anahtar sertifikası).](https://en.wikipedia.org/wiki/Public_key_certificate) Aşağıdaki kod, özel bir anahtar sertifikasının nasıl yüklenir olduğunu gösterir:

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

Windows sertifika deposu için destek vermeyen veya yetersiz destek sunan diller için [dosyadan yük sertifikasına](#load-certificate-from-file)bakın.

## <a name="load-certificate-in-linux-apps"></a>Linux uygulamalarında yükleme sertifikası

Uygulama `WEBSITE_LOAD_CERTIFICATES` ayarları, belirtilen sertifikaları Linux barındırılan uygulamalarınız (özel konteyner uygulamaları dahil) dosya olarak erişilebilir hale getirir. Dosyalar aşağıdaki dizinler altında bulunur:

- Özel sertifikalar `/var/ssl/private` `.p12` - ( dosyalar)
- Genel sertifikalar `/var/ssl/certs` `.der` - ( dosyalar)

Sertifika dosya adları sertifika parmak izleridir. Aşağıdaki C# kodu, bir Linux uygulamasında ortak sertifikanın nasıl yüklenilebildiğini gösterir.

```csharp
using System;
using System.IO;
using System.Security.Cryptography.X509Certificates;

...
var bytes = File.ReadAllBytes("/var/ssl/certs/<thumbprint>.der");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

Node.js, PHP, Python, Java veya Ruby'deki bir dosyadan TLS/SSL sertifikasının nasıl yüklenir olduğunu görmek için ilgili dil veya web platformuna ilişkin belgelere bakın.

## <a name="load-certificate-from-file"></a>Dosyadan yükleme sertifikası

El ile yüklediğiniz bir sertifika dosyasını yüklemeniz gerekiyorsa, sertifikayı [Git](deploy-local-git.md)yerine [FTPS](deploy-ftp.md) kullanarak yüklemeniz daha iyidir. Özel sertifika gibi hassas verileri kaynak denetimidışında tutmalısınız.

> [!NOTE]
> ASP.NET ve Windows'daki ASP.NET Core, bir dosyadan sertifika yükleseniz bile sertifika deposuna erişmelidir. Bir Sertifika dosyasını Windows .NET uygulamasına yüklemek için, geçerli kullanıcı profilini <a target="_blank" href="https://shell.azure.com" >Bulut Kabuğu'ndaki</a>aşağıdaki komutla yükleyin:
>
> ```azurecli-interactive
> az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
> ```
>
> Kodunuzda sertifika kullanmaya bu yaklaşım, uygulamanızın **Temel** katmanda veya üzerinde olmasını gerektiren App Hizmeti'ndeki TLS işlevselliğini kullanır.

Aşağıdaki C# örneği, uygulamanızdaki göreceli bir yoldan ortak sertifika yükler:

```csharp
using System;
using System.IO;
using System.Security.Cryptography.X509Certificates;

...
var bytes = File.ReadAllBytes("~/<relative-path-to-cert-file>");
var cert = new X509Certificate2(bytes);

// Use the loaded certificate
```

Node.js, PHP, Python, Java veya Ruby'deki bir dosyadan TLS/SSL sertifikasının nasıl yüklenir olduğunu görmek için ilgili dil veya web platformuna ilişkin belgelere bakın.

## <a name="more-resources"></a>Diğer kaynaklar

* [Azure Uygulama Hizmetinde TLS/SSL bağlama ile özel bir DNS adı güvenliğini sağlama](configure-ssl-bindings.md)
* [HTTPS zorlama](configure-ssl-bindings.md#enforce-https)
* [TLS 1.1/1.2 zorlama](configure-ssl-bindings.md#enforce-tls-versions)
* [SSS : Uygulama Hizmet Sertifikaları](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)
