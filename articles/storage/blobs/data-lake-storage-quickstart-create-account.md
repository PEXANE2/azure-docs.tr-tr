---
title: Create an Azure Data Lake Storage Gen2 storage account | Microsoft Docs
description: Quickly learn to create a new storage account with access to Data Lake Storage Gen2 using the Azure portal, Azure PowerShell, or the Azure CLI.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 57350bd00a33c36b5aef3a0ccd3034b4db3d2c55
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227960"
---
# <a name="create-an-azure-data-lake-storage-gen2-storage-account"></a>Create an Azure Data Lake Storage Gen2 storage account

Azure Data Lake Storage Gen2 [supports a hierarchical namespace](data-lake-storage-introduction.md) which provides a native directory-based container tailored to work with the Hadoop Distributed File System (HDFS). HDFS'den Data Lake Storage Gen2 verilerine erişim [ABFS sürücüsü](data-lake-storage-abfs-driver.md) aracılığıyla sağlanabilir.

This article demonstrates how to create an account using the Azure portal, Azure PowerShell, or via the Azure CLI.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

|           | Önkoşul |
|-----------|--------------|
|Portal     | Hiçbiri         |
|PowerShell | This article requires the PowerShell module Az.Storage version **0.7** or later. To find your current version, run the `Get-Module -ListAvailable Az.Storage` command. If after you run this command, no results appear, or if a version lower than **0.7** appears, then you'll have to upgrade your powershell module. See the [Upgrade your powershell module](#upgrade-your-powershell-module) section of this guide.
|CLI        | You can sign in to Azure and run Azure CLI commands in one of two ways: <ul><li>CLI komutlarını Azure portalında Azure Cloud Shell içinden çalıştırabilirsiniz </li><li>CLI yükleyip CLI komutlarını yerel olarak çalıştırabilirsiniz</li></ul>|

Komut satırında çalışırken Azure Cloud Shell'i çalıştırabilir veya CLI'yı yerel ortama yükleyebilirsiniz.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Azure portalının sağ üst tarafında yer alan menüdeki **Cloud Shell** düğmesine tıklayın:

[![Cloud Shell](./media/data-lake-storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

The button launches an interactive shell that you can use to run the steps in this article:

[![Portaldaki Cloud Shell penceresini gösteren ekran görüntüsü](./media/data-lake-storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. This article requires that you are running the Azure CLI version 2.0.38 or later. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-storage-account-with-azure-data-lake-storage-gen2-enabled"></a>Azure Data Lake Storage 2. Nesil etkin bir depolama hesabı oluşturma

An Azure storage account contains all of your Azure Storage data objects: blobs, files, queues, tables, and disks. The storage account provides a unique namespace for your Azure Storage data that is accessible from anywhere in the world over HTTP or HTTPS. Data in your Azure storage account is durable and highly available, secure, and massively scalable.

> [!NOTE]
> Data Lake Storage Gen2 özelliklerinden faydalanmak için yeni depolama hesaplarını **StorageV2 (genel amaçlı V2)** türünde oluşturmanız gerekir.  

Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure Depolama hesabına genel bakış](../common/storage-account-overview.md).

## <a name="create-an-account-using-the-azure-portal"></a>Azure portalı kullanarak bir hesap oluşturma

[Azure Portal](https://portal.azure.com)’ında oturum açın.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Her depolama hesabı bir Azure kaynak grubuna ait olmalıdır. Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır. This article shows how to create a new resource group.

Azure portalında genel amaçlı v2 bir depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

> [!NOTE]
> Hiyerarşik ad alanı şu anda tüm genel bölgelerde kullanılabilir.

1. Depolama hesabını oluşturmak istediğiniz aboneliği seçin.
2. In the Azure portal, choose the **Create a resource** button, then choose **Storage account**.
3. **Kaynak grubu** alanı altında **Yeni oluştur**’u seçin. Enter a name for your new resource group.
   
   Kaynak grubu, Azure hizmetlerinizi gruplandırmaya yönelik mantıksal bir kapsayıcıdır. Bir depolama hesabı oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya var olan bir kaynak grubu kullanma seçeneğiniz vardır.

4. Ardından, depolama hesabınız için bir ad girin. Seçtiğiniz ad Azure genelinde benzersiz olmalıdır. Ad ayrıca 3 - 24 karakter uzunluğunda olmalıdır ve yalnızca rakam ve küçük harf içerebilir.
5. Konum seçin.
6. Make sure that **StorageV2 (general purpose v2)** appears as selected in the **Account kind** drop-down list.
7. Optionally change the values in each of these fields: **Performance**, **Replication**, **Access tier**. To learn more about these options, see [Introduction to Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-introduction#azure-storage-services).
8. Choose the **Advanced** tab.
10. In the **Data Lake Storage Gen2** section set **Hierarchical namespace** to **Enabled**.
11. Click **Review + Create** to create the storage account.

Depolama hesabınız portaldan oluşturulmuş olur.

### <a name="clean-up-resources"></a>Kaynakları temizleme

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve listenin sağ tarafındaki **Daha fazla** düğmesine ( **...** ) sağ tıklayın.
3. **Kaynak grubunu sil**'i seçip onaylayın.

## <a name="create-an-account-using-powershell"></a>PowerShell kullanarak hesap oluşturma

First, install the latest version of the [PowerShellGet](/powershell/scripting/gallery/installing-psget) module.

Then, upgrade your powershell module, sign in to your Azure subscription, create a resource group, and then create a storage account.

### <a name="upgrade-your-powershell-module"></a>PowerShell modülünüzü yükseltme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

To interact with Data Lake Storage Gen2 by using PowerShell, you'll need to install module Az.Storage version **0.7** or later.

Start by opening a PowerShell session with elevated permissions.

Install the Az.Storage module

```powershell
Install-Module Az.Storage -Repository PSGallery -AllowClobber -Force
```

### <a name="sign-in-to-your-azure-subscription"></a>Sign in to your Azure Subscription

Use the `Login-AzAccount` command and follow the on-screen directions to authenticate.

```powershell
Login-AzAccount
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

To create a new resource group with PowerShell, use the [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) command: 

> [!NOTE]
> Hiyerarşik ad alanı şu anda tüm genel bölgelerde kullanılabilir.

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-quickstart-resource-group"
$location = "westus2"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-general-purpose-v2-storage-account"></a>Genel amaçlı v2 depolama hesabı oluşturma

To create a general-purpose v2 storage account from PowerShell with locally-redundant storage (LRS), use the [New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) command:

```powershell
$location = "westus2"

New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name "storagequickstart" `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind StorageV2 `
  -EnableHierarchicalNamespace $True
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

To remove the resource group and its associated resources, including the new storage account, use the [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) command: 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="create-an-account-using-azure-cli"></a>Azure CLI'yı kullanarak hesap oluşturma

To start Azure Cloud Shell, sign in to the [Azure portal](https://portal.azure.com).

If you want to sign in to your local installation of the CLI, run the login command:

```cli
az login
```

### <a name="add-the-cli-extension-for-azure-data-lake-gen-2"></a>Add the CLI extension for Azure Data Lake Gen 2

To interact with Data Lake Storage Gen2 by using the CLI, you'll have to add an extension to your shell.

To do that, enter the following command by using either the Cloud Shell or a local shell: `az extension add --name storage-preview`

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure CLI ile yeni bir kaynak grubu oluşturmak için [az group create](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group create `
    --name storage-quickstart-resource-group `
    --location westus2
```

> [!NOTE]
> > Hiyerarşik ad alanı şu anda tüm genel bölgelerde kullanılabilir.

### <a name="create-a-general-purpose-v2-storage-account"></a>Genel amaçlı v2 depolama hesabı oluşturma

Azure CLI’dan yerel olarak yedekli depolama ile genel amaçlı v2 depolama hesabı oluşturmak için [az storage account create](/cli/azure/storage/account) komutunu kullanın.

```azurecli-interactive
az storage account create `
    --name storagequickstart `
    --resource-group storage-quickstart-resource-group `
    --location westus2 `
    --sku Standard_LRS `
    --kind StorageV2 `
    --hierarchical-namespace true
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu ve yeni depolama hesabı dahil olmak üzere ilişkili kaynakları kaldırmak için [az group delete](/cli/azure/group) komutunu kullanın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

In this article, you've created a storage account with Data Lake Storage Gen2 capabilities. To learn how to upload and download blobs to and from your storage account, see the following topic.

* [AzCopy V10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
