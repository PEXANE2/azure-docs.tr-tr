---
title: Azure Veri Gölü Depolama Gen1 ile başlayın - PowerShell | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1 hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure PowerShell'i kullanın.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 42ddab6991b418af3e41da9966cdab69ded87461
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73837887"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure Veri Gölü Depolama Gen1 ile başlayın

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [Powershell](data-lake-store-get-started-powershell.md)
> * [Azure CLI](data-lake-store-get-started-cli-2.0.md)
>
>

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Veri Gölü Depolama Gen1 hesabı oluşturmak ve klasör oluşturma, veri dosyalarını yükleme ve indirme, hesabınızı silme vb. gibi temel işlemleri gerçekleştirmek için Azure PowerShell'i nasıl kullanacağınızı öğrenin. Veri Gölü Depolama Gen1 hakkında daha fazla bilgi için, [Veri Gölü Depolama Gen1 Genel Bakış](data-lake-store-overview.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="authentication"></a>Kimlik doğrulaması

Bu makalede, Azure hesap kimlik bilgilerinizi girmeniz istendiği Data Lake Storage Gen1 ile daha basit bir kimlik doğrulama yaklaşımı kullanılır. Veri Gölü Depolama Gen1 hesabına ve dosya sistemine erişim düzeyi, oturum açmış kullanıcının erişim düzeyine göre yönetilir. Ancak, son kullanıcı kimlik doğrulaması veya hizmete hizmet kimlik doğrulaması olan Data Lake Storage Gen1 ile kimlik doğrulaması yapmak için başka yaklaşımlar da vardır. Kimlik doğrulaması gerçekleştirmeyle ilgili yönergeler ve daha fazla bilgi için [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md) veya [Hizmetten hizmete kimlik doğrulaması](data-lake-store-authenticate-using-active-directory.md) bölümlerine göz atın.

## <a name="create-a-data-lake-storage-gen1-account"></a>Data Lake Storage 1. Nesil hesabı oluşturma

1. Masaüstünüzde yeni bir Windows PowerShell penceresi açın. Azure hesabınızda oturum açmak, aboneliği ayarlamak ve Veri Gölü Depolama Gen1 sağlayıcısını kaydetmek için aşağıdaki snippet'i girin. Oturum açmanız istendiğinde, abonelik yöneticilerinden/sahibinden biri olarak oturum açtığınızdan emin olun:

    ```PowerShell
    # Log in to your Azure account
    Connect-AzAccount

    # List all the subscriptions associated to your account
    Get-AzSubscription

    # Select a subscription
    Set-AzContext -SubscriptionId <subscription ID>

    # Register for Azure Data Lake Storage Gen1
    Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
    ```

1. Veri Gölü Depolama Gen1 hesabı, bir Azure kaynak grubuyla ilişkilidir. Bir kaynak grubu oluşturarak başlayın.

    ```PowerShell
    $resourceGroupName = "<your new resource group name>"
    New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"
    ```

    ![Azure Kaynak Grubu Oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Azure Kaynak Grubu oluşturma")

1. Bir Veri Gölü Depolama Gen1 hesabı oluşturun. Belirttiğiniz ad yalnızca küçük harflerden ve rakamlardan oluşmalıdır.

    ```PowerShell
    $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 account name>"
    New-AzDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStorageGen1Name -Location "East US 2"
    ```

    ![Data Lake Storage 1. Nesil hesabı oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Data Lake Storage 1. Nesil hesabı oluşturma")

1. Hesabın başarıyla oluşturulduğunu doğrulayın.

    ```PowerShell
    Test-AzDataLakeStoreAccount -Name $dataLakeStorageGen1Name
    ```

    Bu cmdlet'in çıktısı **True** olmalıdır.

## <a name="create-directory-structures"></a>Dizin yapıları oluşturma

Verileri yönetmek ve depolamak için Data Lake Storage Gen1 hesabınız altında dizinler oluşturabilirsiniz.

1. Bir kök dizin belirtin.

    ```PowerShell
    $myrootdir = "/"
    ```

1. Belirtilen kökün altında **mynewdirectory** adlı yeni bir dizin oluşturun.

    ```PowerShell
    New-AzDataLakeStoreItem -Folder -AccountName $dataLakeStorageGen1Name -Path $myrootdir/mynewdirectory
    ```

1. Yeni dizinin başarıyla oluşturulduğunu doğrulayın.

    ```PowerShell
    Get-AzDataLakeStoreChildItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir
    ```

    Aşağıdaki ekran görüntüsünde gösterildiği gibi bir çıkış göstermelidir:

    ![Dizini Doğrulama](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Dizini Doğrulama")

## <a name="upload-data"></a>Karşıya veri yükleme

Verilerinizi doğrudan kök düzeyinde Veri Gölü Depolama Gen1'e veya hesap içinde oluşturduğunuz bir dizine yükleyebilirsiniz. Bu bölümdeki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz dizine (**mynewdirectory**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınızda C:\sampledata\ gibi yerel bir dizinde depolayın.

```PowerShell
Import-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name `
   -Path "C:\sampledata\vehicle1_09142014.csv" `
   -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv
```

## <a name="rename-download-and-delete-data"></a>Verileri yeniden adlandırma, indirme ve silme

Bir dosyayı yeniden adlandırmak için aşağıdaki komutu kullanın:

```PowerShell
Move-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name `
    -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv `
    -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv
```

Bir dosyayı indirmek için aşağıdaki komutu kullanın.

```PowerShell
Export-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name `
    -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv `
    -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"
```

Bir dosyayı silmek için aşağıdaki komutu kullanın:

```PowerShell
Remove-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name `
    -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv
```

İstendiğinde, öğeyi silmek için **Y** yazın. Birden fazla dosyayı silmek istiyorsanız tüm yolları virgülle ayrılmış olarak sağlayabilirsiniz.

```PowerShell
Remove-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name `
    -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv
```

## <a name="delete-your-account"></a>Hesabınızı silin

Veri Gölü Depolama Gen1 hesabınızı silmek için aşağıdaki komutu kullanın.

```PowerShell
Remove-AzDataLakeStoreAccount -Name $dataLakeStorageGen1Name
```

İstendiğinde, hesabı silmek için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gölü Depolama Gen1 ile PowerShell'i kullanmak için performans alamı kılavuzu](data-lake-store-performance-tuning-powershell.md)
* [Büyük veri gereksinimleri için Azure Veri Gölü Depolama Gen1'i kullanma](data-lake-store-data-scenarios.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
