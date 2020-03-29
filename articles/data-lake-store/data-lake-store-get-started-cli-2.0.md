---
title: Azure Veri Gölü Depolama Gen1 | Microsoft Dokümanlar
description: Veri Gölü Depolama Gen1 hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure CLI'yi kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 9431cc7fa12b86371ce6b2325aca8e13d264442e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60885355"
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli"></a>Azure CLI'yi kullanarak Azure Veri Gölü Deposu'na başlayın

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [Powershell](data-lake-store-get-started-powershell.md)
> * [Azure CLI](data-lake-store-get-started-cli-2.0.md)
>
> 

Azure Veri Gölü Depolama Gen1 hesabı oluşturmak ve klasör oluşturma, veri dosyalarını yükleme ve indirme, hesabınızı silme vb. gibi temel işlemleri gerçekleştirmek için Azure CLI'yi nasıl kullanacağınızı öğrenin. Veri Gölü Depolama Gen1 hakkında daha fazla bilgi için, [Veri Gölü Depolama Gen1 Genel Bakış](data-lake-store-overview.md)bakın.

Azure CLI, Azure kaynaklarını yönetmek için Azure tarafından sunulan komut satırı deneyimidir. MacOS, Linux ve Windows’da kullanılabilir. Daha fazla bilgi için Azure [CLI'ye Genel Bakış](https://docs.microsoft.com/cli/azure)bölümüne bakın. Komutların ve sözdiziminin tam listesi için [Azure Veri Gölü Depolama Gen1 CLI başvurusuna](https://docs.microsoft.com/cli/azure/dls) da bakabilirsiniz.


## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure CLI** - Yönergeler için [Azure CLI'yi yükle'ye](https://docs.microsoft.com/cli/azure/install-azure-cli) bakın.

## <a name="authentication"></a>Kimlik doğrulaması

Bu makalede, son kullanıcı kullanıcısı olarak oturum açtığınızda Veri Gölü Depolama Gen1 ile daha basit bir kimlik doğrulama yaklaşımı kullanılır. Veri Gölü Depolama Gen1 hesabına ve dosya sistemine erişim düzeyi, oturum açmış kullanıcının erişim düzeyine göre yönetilir. Ancak, **son kullanıcı kimlik doğrulaması** veya **hizmete hizmet**kimlik doğrulaması olan Data Lake Storage Gen1 ile kimlik doğrulaması yapmak için başka yaklaşımlar da vardır. Kimlik doğrulaması gerçekleştirmeyle ilgili yönergeler ve daha fazla bilgi için [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md) veya [Hizmetten hizmete kimlik doğrulaması](data-lake-store-authenticate-using-active-directory.md) bölümlerine göz atın.


## <a name="log-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

1. Azure aboneliğinizde oturum açın.

    ```azurecli
    az login
    ```

    Sonraki adımda kullanmak üzere bir kod alırsınız. https://aka.ms/devicelogin sayfasını açmak için bir web tarayıcısı kullanın ve kimlik doğrulaması yapmak için kodu girin. Kimlik bilgilerinizi kullanarak oturum açmanız istenir.

2. Oturum açtığınızda, pencerede hesabınızla ilişkili tüm Azure abonelikleri listelenir. Belirli bir aboneliği kullanmak için aşağıdaki komutu kullanın.
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-storage-gen1-account"></a>Azure Veri Gölü Depolama Gen1 hesabı oluşturma

1. Yeni bir kaynak grubu oluşturun. Aşağıdaki komut içinde kullanmak istediğiniz parametre değerlerini sağlayın. Konum adı boşluk içeriyorsa adı tırnak işaretleri içine alın. Örneğin, "Doğu ABD 2". 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. Veri Gölü Depolama Gen1 hesabını oluşturun.
   
    ```azurecli
    az dls account create --account mydatalakestoragegen1 --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabında klasör oluşturma

Verileri yönetmek ve depolamak için Azure Veri Gölü Depolama Gen1 hesabınızın altında klasörler oluşturabilirsiniz. Veri Gölü Depolama Gen1 hesabının kökünde **mynewfolder** adlı bir klasör oluşturmak için aşağıdaki komutu kullanın.

```azurecli
az dls fs create --account mydatalakestoragegen1 --path /mynewfolder --folder
```

> [!NOTE]
> `--folder` parametresi, komutun bir klasör oluşturmasını sağlar. Bu parametre yoksa, komut Veri Gölü Depolama Gen1 hesabının kökünde mynewfolder adlı boş bir dosya oluşturur.
> 
>

## <a name="upload-data-to-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabına veri yükleme

Verileri doğrudan kök düzeyinden veya hesap içinde oluşturduğunuz bir klasöre Veri Gölü Depolama Gen1'e yükleyebilirsiniz. Aşağıdaki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz klasöre (**mynewfolder**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınızda C:\sampledata\ gibi yerel bir dizinde depolayın.

```azurecli
az dls fs upload --account mydatalakestoragegen1 --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> Hedef için dosya adı dahil olmak üzere yolun tamamını belirtmeniz gerekir.
> 
>


## <a name="list-files-in-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabındaki dosyaları listele

Veri Gölü Depolama Gen1 hesabındaki dosyaları listelemek için aşağıdaki komutu kullanın.

```azurecli
az dls fs list --account mydatalakestoragegen1 --path /mynewfolder
```

Bunun çıktısının aşağıdakine benzer olması gerekir:

    [
        {
            "accessTime": 1491323529542,
            "aclBit": false,
            "blockSize": 268435456,
            "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "length": 1589881,
            "modificationTime": 1491323531638,
            "msExpirationTime": 0,
            "name": "mynewfolder/vehicle1_09142014.csv",
            "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "pathSuffix": "vehicle1_09142014.csv",
            "permission": "770",
            "replication": 1,
            "type": "FILE"
        }
    ]

## <a name="rename-download-and-delete-data-from-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabındaki verileri yeniden adlandırma, indirme ve silme 

* **Bir dosyayı yeniden adlandırmak için**aşağıdaki komutu kullanın:
  
    ```azurecli
    az dls fs move --account mydatalakestoragegen1 --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* **Bir dosyayı indirmek için**aşağıdaki komutu kullanın. Belirttiğiniz hedef yolun önceden var olduğundan emin olun.
  
    ```azurecli     
    az dls fs download --account mydatalakestoragegen1 --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > Bu komut, henüz mevcut değilse hedef klasörü oluşturur.
    > 
    >

* **Bir dosyayı silmek için**aşağıdaki komutu kullanın:
  
    ```azurecli
    az dls fs delete --account mydatalakestoragegen1 --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    Hem **mynewfolder** klasörünü hem de **vehicle1_09142014_copy.csv** dosyasını tek bir komutla silmek istiyorsanız --recurse parametresini kullanın

    ```azurecli
    az dls fs delete --account mydatalakestoragegen1 --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabı için izinler ve ALA'larla çalışma

Bu bölümde Azure CLI'yi kullanarak ACD'leri ve izinleri nasıl yönetisiniz hakkında bilgi edinin. ALA'ların Azure Veri Gölü Depolama Gen1'de nasıl uygulandığı hakkında ayrıntılı bir tartışma için, [Azure Veri Gölü Depolama Gen1'de Erişim denetimine](data-lake-store-access-control.md)bakın.

* **Bir dosya veya klasörün sahibini güncelleştirmek için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access set-owner --account mydatalakestoragegen1 --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* **Bir dosya veya klasörün izinlerini güncelleştirmek için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access set-permission --account mydatalakestoragegen1 --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* **Belirli bir yolun ACL’lerini almak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access show --account mydatalakestoragegen1 --path /mynewfolder/vehicle1_09142014.csv
    ```

    Çıktının aşağıdakine benzer olması gerekir:

        {
            "entries": [
            "user::rwx",
            "group::rwx",
            "other::---"
          ],
          "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "permission": "770",
          "stickyBit": false
        }

* **ACL’ye yönelik bir giriş ayarlamak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access set-entry --account mydatalakestoragegen1 --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* **ACL’ye yönelik bir girişi kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-entry --account mydatalakestoragegen1 --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* **Varsayılan ACL’nin tamamını kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-all --account mydatalakestoragegen1 --path /mynewfolder --default-acl
    ```

* **Varsayılan olmayan bir ACL’nin tamamını kaldırmak için** aşağıdaki komutu kullanın:

    ```azurecli
    az dls fs access remove-all --account mydatalakestoragegen1 --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-storage-gen1-account"></a>Veri Gölü Depolama Gen1 hesabını silme
Bir Veri Gölü Depolama Gen1 hesabını silmek için aşağıdaki komutu kullanın.

```azurecli
az dls account delete --account mydatalakestoragegen1
```

İstendiğinde, hesabı silmek için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar
* [Büyük veri gereksinimleri için Azure Veri Gölü Depolama Gen1'i kullanma](data-lake-store-data-scenarios.md) 
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
