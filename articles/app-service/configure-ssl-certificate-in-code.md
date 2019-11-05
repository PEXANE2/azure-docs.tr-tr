---
title: Uygulama kodunda SSL sertifikası kullan-Azure App Service | Microsoft Docs
description: İstemci sertifikalarının, bunları gerektiren uzak kaynaklara bağlanmak için nasıl kullanılacağını öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: gwallace
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/16/2019
ms.author: cephalin
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: 1f042f72f82d2198472fe81670c697c0c4b28321
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73513698"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Uygulama kodunuzda bir SSL sertifikası kullanın Azure App Service

App Service uygulama kodunuz, istemci olarak davranabilir ve sertifika kimlik doğrulaması gerektiren bir dış hizmete erişebilir. Bu nasıl yapılır kılavuzunda, uygulama kodunuzda ortak veya özel sertifikaların nasıl kullanılacağı gösterilmektedir.

Kodunuzda sertifika kullanmaya yönelik bu yaklaşım, uygulamanızın **temel** katmanda veya üzerinde olmasını GEREKTIREN App Service SSL işlevselliğini kullanır. Alternatif olarak, [sertifika dosyasını uygulama deponuza dahil](#load-certificate-from-file)edebilirsiniz, ancak özel sertifikalar için önerilen bir uygulama değildir.

SSL sertifikalarınızı App Service izin vermenizde, sertifikaları ve uygulama kodunuzu ayrı olarak koruyabilir ve hassas verilerinizi koruyabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu izlemek için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Uygulamanıza bir sertifika ekleyin](configure-ssl-certificate.md)

## <a name="find-the-thumbprint"></a>Parmak izini bulma

<a href="https://portal.azure.com" target="_blank">Azure Portal</a>, sol menüden app **Services** >  **\<app-name >** ' ı seçin.

Uygulamanızın sol gezinti bölmesinde, **TLS/SSL ayarları**' nı seçin ve ardından **özel anahtar sertifikaları (. pfx** ) veya **ortak anahtar sertifikaları (. cer)** seçeneğini belirleyin.

Kullanmak istediğiniz sertifikayı bulun ve parmak izini kopyalayın.

![Sertifika parmak izini kopyalama](./media/configure-ssl-certificate/create-free-cert-finished.png)

## <a name="load-the-certificate"></a>Sertifikayı yükleme

Uygulama kodunuzda bir sertifika kullanmak için, <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>aşağıdaki komutu çalıştırarak parmak izini `WEBSITE_LOAD_CERTIFICATES` uygulama ayarına ekleyin:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Tüm sertifikalarınızı erişilebilir hale getirmek için değeri `*`olarak ayarlayın.

> [!NOTE]
> Bu ayar, belirtilen sertifikaları çoğu fiyatlandırma katmanı için [geçerli User\store](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) içine koyar, ancak **yalıtılmış** katmanda (uygulama bir [App Service ortamı](environment/intro.md)çalıştırılır), sertifikaları [Yerel makine\'](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) a koyar. saklayabilir.
>

Yapılandırılan sertifikalar artık kodunuz tarafından kullanılmak üzere hazırdır.

## <a name="load-the-certificate-in-code"></a>Sertifikayı kodda yükle

Sertifikanız erişilebilir olduğunda, sertifika parmak izine göre C# koda erişebilirsiniz. Aşağıdaki kod, parmak izine sahip bir sertifika yükler `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

<a name="file"></a>
## <a name="load-certificate-from-file"></a>Sertifikayı dosyadan yükle

Uygulama dizininizden bir sertifika dosyası yüklemeniz gerekiyorsa, örneğin [Git](deploy-local-git.md)yerine [FTPS](deploy-ftp.md) 'yi kullanarak karşıya yüklemek daha iyidir. Gizli verileri, kaynak denetiminden özel bir sertifika gibi tutmanız gerekir.

Dosyayı doğrudan .NET kodunuzda yükleseniz de, kitaplık geçerli kullanıcı profilinin yüklenip yüklenmediğini doğrular. Geçerli kullanıcı profilini yüklemek için `WEBSITE_LOAD_USER_PROFILE` uygulama ayarını <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>aşağıdaki komutla ayarlayın.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
```

Bu ayar ayarlandıktan sonra aşağıdaki C# örnek, `mycert.pfx` adlı bir sertifikayı uygulamanızın deposunun `certs` dizininden yükler.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```

## <a name="more-resources"></a>Diğer kaynaklar

* [SSL bağlaması ile özel bir DNS adının güvenliğini sağlama](configure-ssl-bindings.md)
* [HTTPS zorlama](configure-ssl-bindings.md#enforce-https)
* [TLS 1.1/1.2 'yi zorla](configure-ssl-bindings.md#enforce-tls-versions)
* [SSS: sertifikalar App Service](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)
