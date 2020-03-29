---
title: ".NET SDK: Azure Veri Gölü Depolama Gen1'de hesap yönetimi işlemleri | Microsoft Dokümanlar"
description: Veri Gölü Depolama Gen1'de hesap yönetimi işlemleri gerçekleştirmek için Azure Veri Gölü Depolama Gen1 .NET SDK'yı kullanın1
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ea57d5a9-2929-4473-9d30-08227912aba7
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 8da40aa04381542c8c750c8d7e33c9a29879371d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65900869"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-net-sdk"></a>.NET SDK kullanarak Azure Veri Gölü Depolama Gen1'de hesap yönetimi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Bu makalede, .NET SDK'yı kullanarak Azure Veri Gölü Depolama Gen1'de hesap yönetimi işlemlerinin nasıl gerçekleştirildirilecegerektiğini öğrenirsiniz. Hesap yönetimi işlemleri arasında bir Veri Gölü Depolama Gen1 hesabı oluşturma, hesapları Bir Azure aboneliğinde listeleme, hesapları silme vb. yer alır.

.NET SDK'yı kullanarak Veri Gölü Depolama Gen1'de veri yönetimi işlemlerinin nasıl gerçekleştirileceklerine ilişkin talimatlar için [,.NET SDK'yı kullanarak Veri Gölü Depolama Gen1'deki Dosya Sistemi işlemlerine](data-lake-store-data-operations-net-sdk.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2013 ve üzeri**. Aşağıdaki talimatlar Visual Studio 2019'u kullanınız.

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

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

6. Değişkenleri bildirin ve yer tutucu değerlerini sağlayın. Ayrıca, sağladığınız yerel yolun ve dosya adının bilgisayar üzerinde var olduğundan emin olun.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORAGE-GEN1-NAME>.azuredatalakestore.net"; 
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; 
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";                    
                }
            }
        }

Makalenin geriye kalan bölümlerinde, kullanılabilir .NET yöntemlerinin, kimlik doğrulama, dosyayı karşıya yükleme vb. işlemleri gerçekleştirmek üzere nasıl kullanılacağını öğrenebilirsiniz.

## <a name="authentication"></a>Kimlik doğrulaması

* Uygulamanızın son kullanıcı kimlik doğrulaması için [,.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması'na](data-lake-store-end-user-authenticate-net-sdk.md)bakın.
* Uygulamanız için servise hizmet kimlik doğrulaması için [,.NET SDK'yı kullanarak Veri Gölü Depolama Gen1 ile servise hizmet kimlik doğrulaması'na](data-lake-store-service-to-service-authenticate-net-sdk.md)bakın.

## <a name="create-client-object"></a>İstemci nesnesi oluşturma
Aşağıdaki parçacık, hesap oluşturma, hesap silme vb. gibi hizmete hesap yönetimi isteklerini vermek için kullanılan Veri Gölü Depolama Gen1 hesap istemci nesnesini oluşturur.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(armCreds) { SubscriptionId = _subId };
    
## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. Nesil hesabı oluşturma
Aşağıdaki parçacık, Veri Gölü Depolama Gen1 hesap istemci nesnesini oluştururken sağladığınız Azure aboneliğinde bir Veri Gölü Depolama Gen1 hesabı oluşturur.

    // Create Data Lake Storage Gen1 account
    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

## <a name="list-all-data-lake-storage-gen1-accounts-within-a-subscription"></a>Abonelik içindeki tüm Veri Gölü Depolama Gen1 hesaplarını listele
Sınıf tanımına aşağıdaki yöntemi ekleyin. Aşağıdaki snippet, belirli bir Azure aboneliğindeki tüm Veri Gölü Depolama Gen1 hesaplarını listeler.

    // List all Data Lake Storage Gen1 accounts within the subscription
    public static List<DataLakeStoreAccountBasic> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List(_adlsAccountName);
        var accounts = new List<DataLakeStoreAccountBasic>(response);

        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }

        return accounts;
    }

## <a name="delete-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabını silme
Aşağıdaki parçacık, daha önce oluşturduğunuz Veri Gölü Depolama Gen1 hesabını siler.

    // Delete Data Lake Storage Gen1 account
    _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);

## <a name="see-also"></a>Ayrıca bkz.
* [.NET SDK kullanarak Veri Gölü Depolama Gen1 dosya sistemi işlemleri](data-lake-store-data-operations-net-sdk.md)
* [Veri Gölü Depolama Gen1 .NET SDK Referans](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
