---
title: PowerShell ile Azure Tablo depolama işlemlerini gerçekleştirme | Microsoft Docs
description: PowerShell kullanarak Azure Tablo depolama hesabından veri oluşturma, sorgulama, silme gibi genel görevleri çalıştırmayı öğrenin.
services: cosmos-db
author: roygara
ms.service: cosmos-db
ms.topic: article
ms.date: 04/05/2019
ms.author: rogarana
ms.subservice: cosmosdb-table
ms.openlocfilehash: 4591cded820bbefb741d55a22d10a91bd4fff383
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868516"
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure Tablo depolama işlemlerini gerçekleştirme 
[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Azure Tablo depolama, yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini depolamak ve sorgulamak için kullanabileceğiniz bir NoSQL veri deposudur. Hizmetin ana bileşenleri tablolar, varlıklar ve özelliklerdir. Bir tablo, varlıkların koleksiyonudur. Bir varlık bir özellikler kümesidir. Her varlık, tüm ad-değer çiftleri olan en fazla 252 özelliğe sahip olabilir. Bu makalede, Azure Tablo depolama hizmeti kavramlarını zaten bildiğiniz varsayılmaktadır. Ayrıntılı bilgi için bkz. [Tablo hizmeti veri modelini anlama](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) ve [.NET kullanarak Azure Tablo depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md).

Bu nasıl yapılır makalesi, genel Azure Tablo depolama işlemlerini içerir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo Alma
> * Tablo varlıkları ekleme
> * Tablo sorgulama
> * Tablo varlıklarını Sil
> * Bir tablo silme

Bu nasıl yapılır makalesi, işiniz bittiğinde kolayca kaldırabilmeniz için yeni bir kaynak grubunda yeni bir Azure depolama hesabı oluşturmayı gösterir. Bunun yerine var olan bir depolama hesabı kullanıyorsanız bunu yapabilirsiniz.

Örnekler için az PowerShell Modules `Az.Storage (1.1.0 or greater)` ve `Az.Resources (1.2.0 or greater)`gerekir. Bir PowerShell penceresinde, sürümü bulmak için `Get-Module -ListAvailable Az*` çalıştırın. Hiçbir şey görüntülenmiyorsa veya yükseltmeniz gerekiyorsa, bkz. [ınstall Azure PowerShell Module](/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Bu Azure özelliğinin PowerShell 'den kullanılması için `Az` modülünün yüklü olması gerekir. Geçerli `AzTable` sürümü eski Azurerd modülüyle uyumlu değil.
> Gerekirse [az Module 'ü yüklemeye yönelik en son yükleme yönergelerini](/powershell/azure/install-az-ps) izleyin.

Azure PowerShell yüklendikten veya güncelleştirildikten sonra, varlıkları yönetmeye yönelik komutlara sahip olan, **Aztable**modülünü yüklemelisiniz. Bu modülü yüklemek için PowerShell 'i yönetici olarak çalıştırın ve **install-Module** komutunu kullanın.

> [!IMPORTANT]
> Modül adı uyumluluk nedenleriyle, PowerShell Galerisi `AzureRmStorageTables` eski ad altında aynı modülü yayımlamayı sürdürdük. Bu belge yalnızca yeni ada başvuru oluşturacak.

```powershell
Install-Module AzTable
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

`Add-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Add-AzAccount
```

## <a name="retrieve-list-of-locations"></a>Konumların listesini al

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu örnekler **eastus**kullanır. Bu değeri gelecekteki kullanım için değişken **konumunda** depolayın.

```powershell
Get-AzLocation | select Location
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutuyla bir kaynak grubu oluşturun. 

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kaynak grubu adını ileride kullanılmak üzere bir değişkende depolayın. Bu örnekte, *eastus* bölgesinde *pshtablesrg* adlı bir kaynak grubu oluşturulur.

```powershell
$resourceGroup = "pshtablesrg"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Depolama hesabı oluştur

[New-AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount)kullanarak yerel olarak yedekli depolama (LRS) ile standart bir genel amaçlı depolama hesabı oluşturun. Benzersiz bir depolama hesabı adı belirttiğinizden emin olun. Ardından, depolama hesabını temsil eden bağlamı alın. Bir depolama hesabında davranırken, kimlik bilgilerinizi tekrar tekrar sağlamak yerine bağlama başvurabilirsiniz.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Yeni tablo oluştur

Tablo oluşturmak için [New-AzStorageTable](/powershell/module/az.storage/New-AzStorageTable) cmdlet 'ini kullanın. Bu örnekte, tablo `pshtesttable`olarak adlandırılır.

```powershell
$tableName = "pshtesttable"
New-AzStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Depolama hesabındaki tabloların listesini alma

[Get-AzStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable)kullanarak depolama hesabındaki tabloların listesini alın.

```powershell
Get-AzStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Belirli bir tabloya yönelik bir başvuru alın

Bir tabloda işlem gerçekleştirmek için, belirli tabloya bir başvuruya ihtiyacınız vardır. [Get-AzStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable)kullanarak bir başvuru alın.

```powershell
$storageTable = Get-AzStorageTable –Name $tableName –Context $ctx
```

## <a name="reference-cloudtable-property-of-a-specific-table"></a>Belirli bir tablonun CloudTable özelliği başvurusu

> [!IMPORTANT]
> **Aztable** PowerShell modülüyle çalışırken cloudtable kullanımı zorunludur. Bu nesnenin başvurusunu almak için **Get-AzTableTable** komutunu çağırın. Bu komut, zaten mevcut değilse tabloyu da oluşturur.

**Aztable**kullanarak bir tablo üzerinde işlem gerçekleştirmek için belirli bir tablonun cloudtable özelliğine başvurunuz olması gerekir.

```powershell
$cloudTable = (Get-AzStorageTable –Name $tableName –Context $ctx).CloudTable
```

[!INCLUDE [storage-table-entities-powershell-include](../../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Bir tablo silme

Bir tabloyu silmek için [Remove-AzStorageTable](/powershell/module/az.storage/Remove-AzStorageTable)komutunu kullanın. Bu cmdlet, tüm verileri dahil olmak üzere tabloyu kaldırır.

```powershell
Remove-AzStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu nasıl yapılır 'ın başlangıcında yeni bir kaynak grubu ve depolama hesabı oluşturduysanız, kaynak grubunu kaldırarak Bu alıştırmada oluşturduğunuz tüm varlıkları kaldırabilirsiniz. Bu komut, Grup içinde bulunan tüm kaynakların yanı sıra kaynak grubunun kendisini de siler.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde, PowerShell ile sık kullanılan Azure Tablo depolama işlemleri hakkında bilgi edindiniz ve aşağıdakiler dahil olmak üzere: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo Alma
> * Tablo varlıkları ekleme
> * Tablo sorgulama
> * Tablo varlıklarını Sil
> * Bir tablo silme

Daha fazla bilgi için aşağıdaki makalelere bakın

* [Depolama PowerShell cmdlet’leri](/powershell/module/az.storage#storage)

* [PowerShell 'den Azure tablolarıyla çalışma-AzureRmStorageTable/AzTable PS modülü v 2.0](https://paulomarquesc.github.io/working-with-azure-storage-tables-from-powershell)

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
