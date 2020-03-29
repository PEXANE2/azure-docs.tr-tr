---
title: 'Son kullanıcı kimlik doğrulaması: .NET SDK ile Azure Veri Gölü Depolama Gen1 ile Azure Active Directory | Microsoft Dokümanlar'
description: .NET SDK ile Azure Active Directory'yi kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması nasıl elde edilenöğrenin
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
ms.openlocfilehash: 215b839c21c2590c08ac2f4250086eaf97914ce1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66243723"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>.NET SDK kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Java'yı kullanma](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK’sını kullanma](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python’u kullanma](data-lake-store-end-user-authenticate-python.md)
> * [REST API kullanma](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

Bu makalede, Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması yapmak için .NET SDK'yı nasıl kullanacağınızı öğrenirsiniz. .NET SDK'yı kullanarak Data Lake Storage Gen1 ile servise doğrudoğrulama için [,.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile servise hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-net-sdk.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2013 ve üzeri**. Aşağıdaki talimatlar Visual Studio 2019'u kullanınız.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Etkin Dizini "Yerel" Uygulama Oluşturun.** [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile Son kullanıcı kimlik doğrulamaadımlarını](data-lake-store-end-user-authenticate-using-active-directory.md)tamamlamış olmalısınız.

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

5. Açık **Program.cs**
6. Using deyimlerini aşağıdaki satırlarla değiştirin:

    ```csharp
    using System;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Collections.Generic;
            
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Store.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```     

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
Bu parçacığı .NET istemci uygulamanıza ekleyin. Yer tutucu değerlerini, Azure AD yerel uygulamasından alınan değerlerle (ön koşul olarak listelenen) değiştirin. Bu parçacık, Uygulamanızın kimliğini Veri Gölü Depolama Gen1 ile **etkileşimli olarak** doğrulamanızı sağlar, bu da Azure kimlik bilgilerinizi girmeniz istendiği anlamına gelir.

Kullanım kolaylığı için aşağıdaki parçacık, istemci kimliği ve uri'yi herhangi bir Azure aboneliği için geçerli olan yeniden yönlendirme için varsayılan değerleri kullanır. Aşağıdaki snippet'te yalnızca kiracı kimliğinizin değerini sağlamanız gerekir. Kiracı [kimliğini al'da](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)verilen talimatları kullanarak Kiracı Kimliğini alabilirsiniz.
    
- Main() işlevini aşağıdaki kodla değiştirin:

    ```csharp
    private static void Main(string[] args)
    {
        //User login via interactive popup
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        string MY_DOCUMENTS = System.Environment.GetFolderPath(System.Environment.SpecialFolder.MyDocuments);
        string TOKEN_CACHE_PATH = System.IO.Path.Combine(MY_DOCUMENTS, "my.tokencache");
        var tokenCache = GetTokenCache(TOKEN_CACHE_PATH);
        var armCreds = GetCreds_User_Popup(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, tokenCache);
        var adlCreds = GetCreds_User_Popup(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, tokenCache);
    }
    ```

Bir önceki parçacık hakkında bilinmesi gereken birkaç şey:

* Önceki snippet bir yardımcı işlevleri `GetTokenCache` `GetCreds_User_Popup`kullanır ve. Bu yardımcı işlevlerin kodu [burada GitHub'da](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#gettokencache)mevcuttur.
* Snippet, öğreticiyi daha hızlı tamamlamanıza yardımcı olmak için, varsayılan olarak tüm Azure abonelikleri için kullanılabilen yerel bir uygulama istemci kimliği kullanır. Böylece **bu kod parçacığını uygulamanızda olduğu gibi kullanabilirsiniz**.
* Ancak, kendi Azure AD etki alanınızı ve uygulama istemci kimliğinizi kullanmak istemiyorsanız, bir Azure AD yerel uygulaması oluşturmanız ve ardından oluşturduğunuz uygulamaya ait Azure AD kiracı kimliği, istemci kimliği ve yeniden yönlendirme URI’sini kullanmanız gerekir. Bkz. Talimatlar [için Data Lake Storage Gen1 ile son kullanıcı kimlik doğrulaması için Etkin Dizin Uygulaması Oluşturun.](data-lake-store-end-user-authenticate-using-active-directory.md)

  
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, .NET SDK kullanarak Azure Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak için son kullanıcı kimlik doğrulamasını nasıl kullanacağınızı öğrendiniz. Artık .NET SDK'nın Azure Veri Gölü Depolama Gen1 ile çalışmak için nasıl kullanılacağı hakkında aşağıdaki makalelere bakabilirsiniz.

* [.NET SDK kullanarak Veri Gölü Depolama Gen1 hesap yönetimi işlemleri](data-lake-store-get-started-net-sdk.md)
* [.NET SDK kullanarak Veri Gölü Depolama Gen1 veri işlemleri](data-lake-store-data-operations-net-sdk.md)

