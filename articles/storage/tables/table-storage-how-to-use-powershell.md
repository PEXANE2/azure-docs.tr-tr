---
title: PowerShell ile Azure Tablo depolama işlemleri gerçekleştirin | Microsoft Dokümanlar
description: PowerShell'i kullanarak Azure Table depolama hesabından veri oluşturma, sorgulama, silme gibi yaygın görevleri nasıl çalıştırığınızı öğrenin.
author: roygara
ms.service: storage
ms.topic: article
ms.date: 04/05/2019
ms.author: rogarana
ms.subservice: tables
ms.openlocfilehash: 746044aa835df52e61c234c8b5ca61164fffbbc5
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80545959"
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure Tablo depolama işlemleri gerçekleştirin 
[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Azure Tablo depolama, yapılandırılmış, ilişkisel olmayan büyük veri kümelerini depolamak ve sorgulamak için kullanabileceğiniz bir NoSQL veri deposudur. Hizmetin ana bileşenleri tablolar, varlıklar ve özelliklerdir. Tablo varlıkların bir koleksiyondur. Varlık bir özellik kümesidir. Her varlığın tümü ad değeri çiftleri olan en fazla 252 özelliği olabilir. Bu makalede, Azure Tablo Depolama Hizmeti kavramlarını zaten bildiğiniz varsayar. Ayrıntılı bilgi için tablo [hizmeti veri modelini anlama](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) ve [.NET kullanarak Azure Tablo depolamasına başlayın.](../../cosmos-db/table-storage-how-to-use-dotnet.md)

Bu nasıl yapılsa makale, ortak Azure Tablosu depolama işlemlerini kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlıkları ekleme
> * Tabloyu sorgula
> * Tablo varlıklarını silme
> * Bir tablo silme

Bu nasıl yapılır makalesi, yeni bir kaynak grubunda yeni bir Azure Depolama hesabı oluşturmanızı gösterir, böylece işlem bittiğinde hesabınızı kolayca kaldırabilirsiniz. Varolan bir Depolama hesabı kullanmak isterseniz, bunun yerine bunu yapabilirsiniz.

Örnekler Az PowerShell `Az.Storage (1.1.0 or greater)` modülleri `Az.Resources (1.2.0 or greater)`gerektirir ve . PowerShell penceresinde, sürümü `Get-Module -ListAvailable Az*` bulmak için çalıştırın. Hiçbir şey görüntülenmiyorsa veya yükseltmeniz gerekiyorsa, [Azure PowerShell modüllerini yükle'ye](/powershell/azure/install-az-ps)bakın.

> [!IMPORTANT]
> PowerShell'in bu Azure özelliğini `Az` kullanmak için modülün yüklü olması gerekir. Geçerli sürümü `AzTable` eski AzureRM modülüyle uyumlu değildir.
> Gerekirse [Az modülü yüklemek için en son yükleme yönergeleriizleyin.](/powershell/azure/install-az-ps)

Azure PowerShell yüklendikten veya güncelleştirildikten sonra, varlıkları yönetme komutlarına sahip **AzTable**modüllerini yüklemeniz gerekir. Bu modülü yüklemek için PowerShell'i yönetici olarak çalıştırın ve **Install-Module** komutunu kullanın.

> [!IMPORTANT]
> Modül adı uyumluluğu nedenleriyle bu aynı modülü PowerShell Gallery'de eski adıyla `AzureRmStorageTables` yayınlıyoruz. Bu belge yalnızca yeni ada başvurur.

```powershell
Install-Module AzTable
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Add-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Add-AzAccount
```

## <a name="retrieve-list-of-locations"></a>Konumlistesini al

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu örnekler **eastus**kullanın. Bu değeri ileride kullanmak üzere değişken **konumda** saklayın.

```powershell
Get-AzLocation | select Location
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutuyla bir kaynak grubu oluşturun. 

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kaynak grubu adını ileride kullanmak üzere bir değişkende saklayın. Bu örnekte, *eastus* bölgesinde *pshtablesrg* adında bir kaynak grubu oluşturulur.

```powershell
$resourceGroup = "pshtablesrg"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Depolama hesabı oluştur

[New-AzStorageAccount'u](/powershell/module/az.storage/New-azStorageAccount)kullanarak yerel olarak yedekli depolama (LRS) içeren standart bir genel amaçlı depolama hesabı oluşturun. Benzersiz bir depolama hesabı adı belirttiğinden emin olun. Ardından, depolama hesabını temsil eden bağlamı alın. Bir depolama hesabında hareket ederken, kimlik bilgilerinizi tekrar tekrar sağlamak yerine içeriğe başvuruda bulunabilirsiniz.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Yeni bir tablo oluşturma

Tablo oluşturmak için [Yeni Depolama Tablosu](/powershell/module/az.storage/New-AzStorageTable) cmdlet'ini kullanın. Bu örnekte, tablo `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Depolama hesabındaki tabloların listesini alma

[Get-AzStorageTable'ı](/powershell/module/azure.storage/Get-AzureStorageTable)kullanarak depolama hesabındaki tabloların listesini alın.

```powershell
Get-AzStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Belirli bir tabloya başvuru alma

Bir tabloda işlemleri gerçekleştirmek için belirli bir tabloya başvuruda bulunulması gerekir. [Get-AzStorageTable'ı](/powershell/module/azure.storage/Get-AzureStorageTable)kullanarak referans alın.

```powershell
$storageTable = Get-AzStorageTable –Name $tableName –Context $ctx
```

## <a name="reference-cloudtable-property-of-a-specific-table"></a>Belirli bir tablonun Başvuru CloudTable özelliği

> [!IMPORTANT]
> **AzTable** PowerShell modülü ile çalışırken CloudTable kullanımı zorunludur. Bu nesneye başvuru almak için **Get-AzStorageTable** komutunu arayın. Bu komut, zaten yoksa tabloyu da oluşturur.

**AzTable**kullanarak bir tabloda işlemleri gerçekleştirmek için, belirli bir tablonun CloudTable özelliğine bir başvuru yapmanız gerekir.

```powershell
$cloudTable = (Get-AzStorageTable –Name $tableName –Context $ctx).CloudTable
```

[!INCLUDE [storage-table-entities-powershell-include](../../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Bir tablo silme

Bir tabloyu silmek için [Kaldır-AzStorageTable'ı](/powershell/module/az.storage/Remove-AzStorageTable)kullanın. Bu cmdlet, tüm verileri de dahil olmak üzere tabloyu kaldırır.

```powershell
Remove-AzStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu nasıl yapılacağının başında yeni bir kaynak grubu ve depolama hesabı oluşturduysanız, kaynak grubunu kaldırarak bu alıştırmada oluşturduğunuz tüm varlıkları kaldırabilirsiniz. Bu komut, kaynak grubunun kendisi kadar grup içinde bulunan tüm kaynakları da siler.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılabilir makalesinde, PowerShell ile ortak Azure Tablo depolama işlemleri hakkında şunları öğrendiniz: 

> [!div class="checklist"]
> * Bir tablo oluşturma
> * Tablo alma
> * Tablo varlıkları ekleme
> * Tabloyu sorgula
> * Tablo varlıklarını silme
> * Bir tablo silme

Daha fazla bilgi için aşağıdaki makalelere bakın

* [Depolama PowerShell cmdlet’leri](/powershell/module/az.storage#storage)

* [PowerShell'den Azure Tablolarla Çalışma - AzureRmStorageTable/AzTable PS Modülü v2.0](https://paulomarquesc.github.io/working-with-azure-storage-tables-from-powershell)

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
