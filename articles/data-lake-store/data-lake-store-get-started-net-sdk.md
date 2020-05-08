---
title: .NET ile Azure Data Lake Storage 1. hesabını yönetme
description: Azure Data Lake Storage 1. hesap yönetimi işlemleri için .NET SDK 'nın nasıl kullanılacağını öğrenin.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: d7ea2b7fffc4edff6cae895e08b955d2e21ff5f2
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692111"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-net-sdk"></a>.NET SDK kullanarak Azure Data Lake Storage 1. hesap yönetimi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Bu makalede, .NET SDK kullanarak Azure Data Lake Storage 1. hesap yönetimi işlemlerini nasıl gerçekleştireceğinizi öğreneceksiniz. Hesap yönetimi işlemleri bir Data Lake Storage 1. hesabı oluşturmayı, bir Azure aboneliğindeki hesapları listelemeyi, hesapları silmeyi vb. içerir.

.NET SDK kullanarak Data Lake Storage 1. veri yönetimi işlemlerini gerçekleştirme hakkında yönergeler için, bkz. [.NET SDK kullanarak Data Lake Storage 1. dosya sistemi işlemleri](data-lake-store-data-operations-net-sdk.md).

## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2013 veya üzeri**. Aşağıdaki yönergelerde Visual Studio 2019 kullanılır.

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-net-application"></a>.NET uygulaması oluşturma
1. Visual Studio 'da **Dosya** menüsünü, **Yeni**' yi ve ardından **Proje**' yi seçin.
2. **Konsol uygulaması (.NET Framework)** öğesini seçin ve ardından **İleri**' yi seçin.
3. **Proje adı**alanına girin `CreateADLApplication`ve ardından **Oluştur**' u seçin.

4. NuGet paketlerini projenize ekleyin.

   1. Çözüm Gezgini'nde proje adına sağ tıklayın ve **NuGet Paketlerini Yönet**'e tıklayın.
   2. **NuGet Paket Yöneticisi** sekmesinde, **paket kaynağının** **NuGet.org** olarak ayarlandığından ve **ön sürümü dahil** et onay kutusunun işaretli olduğundan emin olun.
   3. Aşağıdaki NuGet paketlerini arayıp yükleyin:

      * `Microsoft.Azure.Management.DataLake.Store` - Bu öğreticide v2.1.3-preview kullanılır.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication` - Bu öğreticide v2.2.12 kullanılır.

        ![NuGet kaynağı ekleme](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Yeni bir Azure Data Lake hesabı oluşturun")
   4. **NuGet Paket Yöneticisi 'ni**kapatın.
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

## <a name="authentication"></a>Kimlik Doğrulaması

* Uygulamanız için son kullanıcı kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Storage 1. Ile Son Kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-net-sdk.md).
* Uygulamanıza yönelik hizmetten hizmete kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-net-sdk.md).

## <a name="create-client-object"></a>İstemci nesnesi oluşturma
Aşağıdaki kod parçacığı, hizmet için hesap oluşturma, hesap silme vb. gibi hesap yönetimi isteklerini vermek için kullanılan Data Lake Storage 1. hesabı istemci nesnesini oluşturur.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(armCreds) { SubscriptionId = _subId };
    
## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. Nesil hesabı oluşturma
Aşağıdaki kod parçacığı, Data Lake Storage 1. hesabı istemci nesnesini oluştururken belirttiğiniz Azure aboneliğinde bir Data Lake Storage 1. hesabı oluşturur.

    // Create Data Lake Storage Gen1 account
    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

## <a name="list-all-data-lake-storage-gen1-accounts-within-a-subscription"></a>Abonelik içindeki tüm Data Lake Storage 1. hesaplarını listeleme
Sınıf tanımına aşağıdaki yöntemi ekleyin. Aşağıdaki kod parçacığında, belirli bir Azure aboneliği içindeki tüm Data Lake Storage 1. hesapları listelenir.

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

## <a name="delete-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. hesabı silme
Aşağıdaki kod parçacığı, daha önce oluşturduğunuz Data Lake Storage 1. hesabını siler.

    // Delete Data Lake Storage Gen1 account
    _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);

## <a name="see-also"></a>Ayrıca bkz.
* [.NET SDK kullanarak Data Lake Storage 1. dosya sistemi işlemleri](data-lake-store-data-operations-net-sdk.md)
* [Data Lake Storage 1. .NET SDK başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/data-lake-store?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
