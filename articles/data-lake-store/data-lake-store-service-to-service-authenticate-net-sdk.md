---
title: 'Hizmete hizmet kimlik doğrulaması: .NET SDK ile Azure Veri Gölü Depolama Gen1 ile Azure Active Directory | Microsoft Dokümanlar'
description: .NET SDK'yı kullanarak Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması nasıl elde edilebildiğini öğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 96c496ef67e26a3079577bf52e9d019d963467b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265538"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>.NET SDK kullanarak Azure Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK’sını kullanma](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API kullanma](data-lake-store-service-to-service-authenticate-rest-api.md)
>
>

Bu makalede, .NET SDK'yı Azure Veri Gölü Depolama Gen1 ile hizmete kimlik doğrulaması yapmak için nasıl kullanacağınızı öğrenirsiniz. .NET SDK'yı kullanarak Data Lake Storage Gen1 ile son kullanıcı kimlik doğrulaması için [,.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-net-sdk.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2013 ve üzeri**. Aşağıdaki talimatlar Visual Studio 2019'u kullanınız.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Etkin Dizin "Web" Uygulaması oluşturun.** [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile Hizmetten Hizmete kimlik doğrulamaadımlarını](data-lake-store-service-to-service-authenticate-using-active-directory.md)tamamlamış olmalısınız.

## <a name="create-a-net-application"></a>.NET uygulaması oluşturma
1. Visual Studio'da **Dosya** menüsünü, **Yeni'yi**ve ardından **Project'i**seçin.
2. **Konsol Uygulaması (.NET Framework) seçeneğini**belirleyin ve ardından **İleri'yi**seçin.
3. **Proje adına**, `CreateADLApplication`girin ve sonra **Oluştur'u**seçin.

4. NuGet paketlerini projenize ekleyin.

   1. Çözüm Gezgini'nde proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın.
   2. **NuGet Paket Yöneticisi** sekmesinde, **Paket kaynağının** **nuget.org** ayarlandıklarına ve **ön sürüm onay** kutusunu n için denildiğinden emin olun.
   3. Aşağıdaki NuGet paketlerini arayıp yükleyin:

      * `Microsoft.Azure.Management.DataLake.Store` - Bu öğreticide v2.1.3-preview kullanılır.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - Bu öğreticide v2.2.12 kullanılır.

        ![NuGet kaynağı ekleme](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Yeni bir Azure Veri Gölü hesabı oluşturma")
   4. **NuGet Paket Yöneticisini**kapatın.

5. **Program.cs** öğesini açın, var olan kodu silin ve ardından ad alanlarına başvurular eklemek için aşağıdaki deyimleri ekleyin.

```csharp
using System;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading;
using System.Collections.Generic;
using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

## <a name="service-to-service-authentication-with-client-secret"></a>İstemci sırrıyla servise hizmet kimlik doğrulaması
Bu parçacığı .NET istemci uygulamanıza ekleyin. Yer tutucu değerlerini bir Azure AD web uygulamasından alınan değerlerle (ön koşul olarak listelenen) değiştirin. Bu parçacık, Azure AD web uygulaması için istemci gizli/anahtarını kullanarak Data Lake Storage Gen1 ile uygulamanızın kimliğini **etkileşimsiz** olarak doğrulamanızı sağlar.

```csharp
private static void Main(string[] args)
{
    // Service principal / application authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    string TENANT = "<AAD-directory-domain>";
    string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
    System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
    System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
    string secret_key = "<AAD_WEB_APP_SECRET_KEY>";
    var armCreds = GetCreds_SPI_SecretKey(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, secret_key);
    var adlCreds = GetCreds_SPI_SecretKey(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, secret_key);
}
```

Önceki parçacık bir yardımcı işlevi `GetCreds_SPI_SecretKey`kullanır. Bu yardımcı işlevin kodu [burada GitHub'da](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_secretkey)kullanılabilir.

## <a name="service-to-service-authentication-with-certificate"></a>Sertifika ile servise hizmet kimlik doğrulaması

Bu parçacığı .NET istemci uygulamanıza ekleyin. Yer tutucu değerlerini bir Azure AD web uygulamasından alınan değerlerle (ön koşul olarak listelenen) değiştirin. Bu parçacık, Azure AD web uygulaması sertifikasını kullanarak Data Lake Storage Gen1 ile uygulamanızın **kimliğini etkileşimsiz** olarak doğrulamanızı sağlar. Azure AD uygulaması nın nasıl oluşturulacağına ilişkin talimatlar [için](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate)bkz.

```csharp
private static void Main(string[] args)
{
    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    string TENANT = "<AAD-directory-domain>";
    string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
    System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
    System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
    var cert = new X509Certificate2(@"d:\cert.pfx", "<certpassword>");
    var armCreds = GetCreds_SPI_Cert(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, cert);
    var adlCreds = GetCreds_SPI_Cert(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, cert);
}
```

Önceki parçacık bir yardımcı işlevi `GetCreds_SPI_Cert`kullanır. Bu yardımcı işlevin kodu [burada GitHub'da](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_cert)kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, .NET SDK kullanarak Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için hizmete kimlik doğrulamayı nasıl kullanacağınızı öğrendiniz. Şimdi Veri Gölü Depolama Gen1 ile çalışmak için .NET SDK'nın nasıl kullanılacağı hakkında konuşulan aşağıdaki makalelere bakabilirsiniz.

* [.NET SDK kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md)
* [.NET SDK kullanarak Veri Gölü Depolama Gen1 veri işlemleri](data-lake-store-data-operations-net-sdk.md)
