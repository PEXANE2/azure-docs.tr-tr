---
title: Azure dosya paylaşımı oluşturma
titleSuffix: Azure Files
description: Azure portal, PowerShell veya Azure CLı kullanarak Azure dosya paylaşma oluşturma.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ca4904940c7974e83445892b9597a9d3c36b44ee
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75452883"
---
# <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Azure dosyalarında [Azure Portal](https://portal.azure.com/), Azure Storage PowerShell cmdlet 'Leri, Azure depolama istemci kitaplıkları veya azure depolama REST API kullanarak bir paylaşma oluşturabilirsiniz. Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:
- Azure portal kullanarak bir Azure dosya paylaşma oluşturma
- PowerShell kullanarak bir Azure dosya paylaşma oluşturma
- Azure CLı kullanarak Azure dosya paylaşma oluşturma

## <a name="prerequisites"></a>Ön koşullar
Bir Azure dosya paylaşma oluşturmak için zaten var olan bir depolama hesabı kullanabilir veya [Yeni bir Azure depolama hesabı oluşturabilirsiniz](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). PowerShell ile bir Azure dosya paylaşma oluşturmak için, depolama hesabınızın hesap anahtarı ve adı gerekir. PowerShell veya CLı kullanmayı planlıyorsanız bir depolama hesabı anahtarına ihtiyaç duyarsınız.

> [!NOTE]
> 5 TiB 'den büyük dosya paylaşımları oluşturmak istiyorsanız bkz. [büyük dosya paylaşımlarını etkinleştirme](storage-files-how-to-create-large-file-share.md).

## <a name="create-a-file-share-through-the-azure-portal"></a>Azure portal aracılığıyla bir dosya paylaşma oluşturma

1. Azure portal **depolama hesabı** bölmesine gidin.
    ![depolama hesabı bölmesi](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

1. **+ Dosya paylaşma** düğmesini seçin.
    Dosya paylaşma Ekle düğmesini ![](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

1. **Ad** ve **Kota**bilgilerini girin.
    Yeni dosya paylaşımının adını ve kotasını ![](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

1. Yeni dosya paylaşımınızı görüntüleyin.
    Yeni dosya paylaşımının Kullanıcı arabirimindeki ![](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

1. Bir dosyayı karşıya yükleyin.
    Gezinti çubuğundaki karşıya yükle düğmesini ![](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

1. Dosya paylaşımınıza gözatıp dizinlerinizi ve dosyalarınızı yönetin.
    dosya paylaşımlarının klasör görünümünü ![](./media/storage-how-to-create-file-share/create-file-share-portal6.png)

## <a name="create-a-file-share-through-powershell"></a>PowerShell aracılığıyla dosya paylaşma oluşturma

Azure PowerShell cmdlet 'lerini indirip yükleyin. Yükleme noktası ve yükleme yönergeleri için Azure PowerShell yükleme [ve yapılandırma](https://docs.microsoft.com/powershell/azure/overview) konusuna bakın.

> [!Note]  
> En son Azure PowerShell modülünü indirip yüklemenizi veya yükseltmesini öneririz.

1. Yeni bir depolama hesabı oluşturun.
    Depolama hesabı, Azure dosya paylaşımlarını ve BLOB 'lar ya da kuyruklar gibi depolama kaynaklarını dağıtabileceğiniz, paylaşılan bir depolama havuzudur.

    ```PowerShell
    $resourceGroup = "myresourcegroup"
    $storAcctName = "myuniquestorageaccount"
    $region = "westus2"
    $storAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storAcctName -SkuName Standard_LRS -Location $region -Kind StorageV2
    ```

1. Yeni bir dosya paylaşma oluşturun.        
    ```PowerShell
    $shareName = "myshare"
    $share = New-AzStorageShare -Context $storAcct.Context -Name $shareName
    ```

> [!Note]  
> Dosya paylaşımınızın adı küçük harflerden oluşmalıdır. Dosya paylaşımlarını ve dosyaları adlandırma hakkında tüm ayrıntılar için bkz. [adlandırma ve başvuru paylaşımları, dizinler, dosyalar ve meta veriler](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="create-a-file-share-through-the-cli"></a>CLı aracılığıyla dosya paylaşma oluşturma

1. Azure CLı 'yi indirin ve yükleyin.
    Bkz. [Azure CLI 'Yı yüklemeyi](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [Azure CLI ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

1. Paylaşma oluşturmak istediğiniz depolama hesabına bir bağlantı dizesi oluşturun.
    Aşağıdaki örnekte ```<storage-account>``` ve ```<resource_group>``` depolama hesabı adı ve kaynak grubuyla değiştirin:

   ```azurecli
    current_env_conn_string=$(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

1. Dosya paylaşımından oluşturun.
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string > /dev/null
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Windows üzerinde bir dosya paylaşımının bağlantısını oluşturma ve bağlama](storage-how-to-use-files-windows.md)
* [Linux 'ta bir dosya paylaşımının bağlantısını oluşturma ve bağlama](storage-how-to-use-files-linux.md)
* [MacOS 'ta bir dosya paylaşımının bağlantısını oluşturma ve bağlama](storage-how-to-use-files-mac.md)

Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın:

* [Depolama dosyaları hakkında SSS](storage-files-faq.md)
* [Windows 'da Azure dosyaları sorunlarını giderme](storage-troubleshoot-windows-file-connection-problems.md)
* [Linux 'ta Azure dosyaları sorunlarını giderme](storage-troubleshoot-linux-file-connection-problems.md)
