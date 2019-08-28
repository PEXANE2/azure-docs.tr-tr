---
title: Uygulama kodunda istemci SSL sertifikası kullan-Azure App Service | Microsoft Docs
description: İstemci sertifikalarının, bunları gerektiren uzak kaynaklara bağlanmak için nasıl kullanılacağını öğrenin.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/29/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 6820daf34e63fd48e83c645e7509a3256bc8435b
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70066985"
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Uygulama kodunuzda bir SSL sertifikası kullanın Azure App Service

Bu nasıl yapılır kılavuzunda, uygulama kodunuzda ortak veya özel sertifikaların nasıl kullanılacağı gösterilmektedir. Kullanım örneğine bir örnek, uygulamanızın sertifika kimlik doğrulaması gerektiren bir dış hizmete eriştiği durumdur.

Kodunuzda sertifika kullanmaya yönelik bu yaklaşım, uygulamanızın **temel** katmanda veya üzerinde olmasını GEREKTIREN App Service SSL işlevselliğini kullanır. Alternatif olarak, [sertifika dosyasını uygulama deponuza dahil](#load-certificate-from-file)edebilirsiniz, ancak özel sertifikalar için önerilen bir uygulama değildir.

SSL sertifikalarınızı App Service izin vermenizde, sertifikaları ve uygulama kodunuzu ayrı olarak koruyabilir ve hassas verilerinizi koruyabilirsiniz.

## <a name="upload-a-private-certificate"></a>Özel bir sertifikayı karşıya yükle

Özel bir sertifikayı karşıya yüklemeden önce, sunucu kimlik doğrulaması için yapılandırılması gerekli olmaması dışında, [tüm gereksinimleri karşıladığından](app-service-web-tutorial-custom-ssl.md#prepare-a-private-certificate)emin olun.

Karşıya yüklemeye hazırsanız, <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>aşağıdaki komutu çalıştırın.

```azurecli-interactive
az webapp config ssl upload --name <app-name> --resource-group <resource-group-name> --certificate-file <path-to-PFX-file> --certificate-password <PFX-password> --query thumbprint
```

Sertifika parmak izini kopyalayın ve [sertifikayı erişilebilir yapın](#make-the-certificate-accessible).

## <a name="upload-a-public-certificate"></a>Ortak sertifikayı karşıya yükle

Ortak Sertifikalar *. cer* biçiminde desteklenir. Ortak bir sertifikayı karşıya yüklemek için <a href="https://portal.azure.com" target="_blank">Azure Portal</a>ve uygulamanıza gidin.

**SSL ayarları** > **genel Sertifikalar (. cer)**  > seçeneğine tıklayarak uygulamanızın sol gezinti alanından**ortak sertifikayı karşıya yükleyin** .

**Ad**alanına sertifika için bir ad yazın. **Cer sertifika dosyası**' nda, cer dosyanızı seçin.

**Karşıya Yükle**'ye tıklayın.

![Ortak sertifikayı karşıya yükle](./media/app-service-web-ssl-cert-load/private-cert-upload.png)

Sertifika karşıya yüklendikten sonra, sertifika parmak izini kopyalayın ve [sertifikayı erişilebilir yapın](#make-the-certificate-accessible).

## <a name="import-an-app-service-certificate"></a>App Service sertifikasını içeri aktarma

Bkz. [Azure App Service IÇIN SSL sertifikası satın alma ve yapılandırma](web-sites-purchase-ssl-web-site.md).

Sertifika içeri aktarıldıktan sonra, sertifika parmak izini kopyalayın ve [sertifikayı erişilebilir yapın](#make-the-certificate-accessible).

## <a name="make-the-certificate-accessible"></a>Sertifikayı erişilebilir yapma

Uygulama kodunuzda karşıya yüklenmiş veya içeri aktarılmış bir sertifikayı kullanmak için, <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>aşağıdaki komutu çalıştırarak parmak izini `WEBSITE_LOAD_CERTIFICATES` uygulama ayarıyla erişilebilir yapın:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_CERTIFICATES=<comma-separated-certificate-thumbprints>
```

Tüm sertifikalarınızı erişilebilir hale getirmek için değerini olarak `*`ayarlayın.

> [!NOTE]
> Bu ayar, belirtilen sertifikaları çoğu fiyatlandırma katmanı için [geçerli User\store](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) içine koyar, ancak **yalıtılmış** katmanda (uygulama bir [App Service ortamı](environment/intro.md)çalıştırılır), sertifikaları [Yerel makine\'](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) a koyar. saklayabilir.
>

![Uygulama ayarını Yapılandır](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

İşlemi tamamladıktan sonra **Kaydet**’e tıklayın.

Yapılandırılan sertifikalar artık kodunuz tarafından kullanılmak üzere hazırdır.

## <a name="load-the-certificate-in-code"></a>Sertifikayı kodda yükle

Sertifikanız erişilebilir olduğunda, sertifika parmak izine göre C# koda erişebilirsiniz. Aşağıdaki kod parmak izine `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`sahip bir sertifika yükler.

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

Dosyayı doğrudan .NET kodunuzda yükleseniz de, kitaplık geçerli kullanıcı profilinin yüklenip yüklenmediğini doğrular. Geçerli kullanıcı profilini yüklemek için `WEBSITE_LOAD_USER_PROFILE` <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>uygulama ayarını aşağıdaki komutla ayarlayın.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_LOAD_USER_PROFILE=1
```

Bu ayar ayarlandıktan sonra aşağıdaki C# örnek, uygulamanızın deposunun `mycert.pfx` `certs` dizininden çağrılan bir sertifikayı yükler.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```
