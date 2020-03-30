---
title: PowerShell'de Azure Sıra depolama eylemleri gerçekleştirin
description: PowerShell ile Azure Kuyruk depolamada işlemler nasıl gerçekleştirililir?
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/15/2019
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: bd2f372bdcb949b64f748d186a9b060bb9cbec4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77087060"
---
# <a name="perform-azure-queue-storage-operations-with-azure-powershell"></a>Azure PowerShell ile Azure Kuyruk depolama işlemleri gerçekleştirme

Azure Sıra depolama, dünyanın her yerinden HTTP veya HTTPS üzerinden erişilebilen çok sayıda ileti depolamahizmetidir. Ayrıntılı bilgi için [bkz.](storage-queues-introduction.md) Bu nasıl yapılabilir makalesi ortak Sıra depolama işlemlerini kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
>
> * Bir kuyruk oluşturma
> * Sıra alma
> * İleti ekleme
> * İletiyi okuma
> * İleti silme
> * Bir kuyruk silme

Bu nasıl yapılsın, Azure PowerShell modülü Az sürüm 0.7 veya sonraki gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps).

Kuyruklar için veri düzlemi için PowerShell cmdlets yok. İleti ekleme, ileti okuma ve bir iletiyi silme gibi veri düzlemi işlemleri gerçekleştirmek için PowerShell'de açıkta olduğu için .NET depolama istemcisi kitaplığını kullanmanız gerekir. Bir ileti nesnesi oluşturursunuz ve ardından bu iletide işlemleri gerçekleştirmek için AddMessage gibi komutları kullanabilirsiniz. Bu makalede, bunu nasıl yapacağınızı gösterir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="retrieve-list-of-locations"></a>Konumlistesini al

Kullanmak istediğiniz konumdan emin değilseniz, kullanılabilir konumları listeleyebilirsiniz. Liste görüntülendikten sonra, kullanmak istediğiniz öğeyi bulun. Bu egzersiz **eastus**kullanacak. Bunu ileride kullanmak üzere değişken **konumda** saklayın.

```powershell
Get-AzLocation | Select-Object Location
$location = "eastus"
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutuyla bir kaynak grubu oluşturun.

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Kaynak grubu adını ileride kullanmak üzere bir değişkende saklayın. Bu örnekte, *eastus* bölgesinde *howtoqueuesrg* adlı bir kaynak grubu oluşturulur.

```powershell
$resourceGroup = "howtoqueuesrg"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Depolama hesabı oluştur

[New-AzStorageAccount'u](/powershell/module/az.storage/New-azStorageAccount)kullanarak yerel olarak yedekli depolama (LRS) içeren standart bir genel amaçlı depolama hesabı oluşturun. Kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alın. Depolama hesabında bir işlem gerçekleştirirken, kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz.

```powershell
$storageAccountName = "howtoqueuestorage"
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS

$ctx = $storageAccount.Context
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Aşağıdaki örnek, ilk olarak depolama hesabı adını ve erişim anahtarını içeren depolama hesabı bağlamını kullanarak Azure Depolama'ya bir bağlantı kurar. Daha sonra, 'howtoqueue' adlı bir kuyruk oluşturmak için [Yeni-AzStorageQueue](/powershell/module/az.storage/New-AzStorageQueue) cmdlet çağırır.

```powershell
$queueName = "howtoqueue"
$queue = New-AzStorageQueue –Name $queueName -Context $ctx
```

Azure Sıra Hizmeti için adlandırma kuralları hakkında bilgi [için](https://msdn.microsoft.com/library/azure/dd179349.aspx)bkz.

## <a name="retrieve-a-queue"></a>Sıra alma

Belirli bir kuyruğa veya Depolama hesabındaki tüm kuyrukların listesini sorgulayabilir ve alabilirsiniz. Aşağıdaki örnekler, depolama hesabındaki tüm kuyrukların ve belirli bir kuyruğun nasıl alındığını gösterir; her iki komutda [da Get-AzStorageQueue](/powershell/module/az.storage/Get-AzStorageQueue) cmdlet kullanılır.

```powershell
# Retrieve a specific queue
$queue = Get-AzStorageQueue –Name $queueName –Context $ctx
# Show the properties of the queue
$queue

# Retrieve all queues and show their names
Get-AzStorageQueue -Context $ctx | Select-Object Name
```

## <a name="add-a-message-to-a-queue"></a>Kuyruğa ileti ekleme

Kuyruktaki gerçek iletileri etkileyen işlemler PowerShell'de açıkta olduğu şekilde .NET depolama istemcikitaplığını kullanır. Kuyruğa ileti eklemek için, ileti nesnesinin [Microsoft.Azure.Storage.Queue.CloudQueueMessage](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.queue._cloud_queue_message) sınıfının yeni bir örneğini oluşturun. Ardından [AddMessage](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.queue._cloud_queue.addmessage) yöntemini çağırın. CloudQueueMessage bir dizeden (UTF-8 biçiminde) veya bir bayt dizisinden oluşturulabilir.

Aşağıdaki örnek, sıranıza nasıl ileti ekleyeceğinizgösteriz.

```powershell
# Create a new message using a constructor of the CloudQueueMessage class
$queueMessage = [Microsoft.Azure.Storage.Queue.CloudQueueMessage]::new("This is message 1")

# Add a new message to the queue
$queue.CloudQueue.AddMessageAsync($QueueMessage)

# Add two more messages to the queue
$queueMessage = [Microsoft.Azure.Storage.Queue.CloudQueueMessage]::new("This is message 2")
$queue.CloudQueue.AddMessageAsync($QueueMessage)

$queueMessage = [Microsoft.Azure.Storage.Queue.CloudQueueMessage]::new("This is message 3")
$queue.CloudQueue.AddMessageAsync($QueueMessage)
```

Azure Depolama [Gezgini'ni](https://storageexplorer.com)kullanıyorsanız, Azure hesabınıza bağlanabilir ve depolama hesabındaki kuyrukları görüntüleyebilir ve kuyruktaki iletileri görüntülemek için sıraya girebilirsiniz.

## <a name="read-a-message-from-the-queue-then-delete-it"></a>Bir iletiyi kuyruktan okuyun ve silin

İletiler ilk çıkış sırası en iyi şekilde okunur. Bu garanti değil. Sıradaki iletiyi okuduğunuzda, sıraya bakan diğer tüm işlemler için görünmez olur. Bu, kodunuz donanım veya yazılım hatası nedeniyle iletiyi işlemeyi başaramazsa, kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar.  

Bu **görünmezlik zaman aşımı,** iletinin yeniden işlenebilecek hale gelmeden önce ne kadar süre görünmez kalacağını tanımlar. Varsayılan değer 30 saniyedir.

Kodunuz iki adımda kuyruktaki bir iletiyi okur. [Microsoft.Azure.Storage.Queue.CloudQueue.GetMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessage) yöntemini aradiğinizde, sıradaki bir sonraki iletiyi alırsınız. **GetMessage**’dan dönen bir ileti bu kuyruktaki kod okuyan iletilere karşı görünmez olur. İletiyi kuyruktan kaldırmayı bitirmek için [Microsoft.Azure.Storage.Queue.CloudQueue.DeleteMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessage) yöntemini ararsınız.

Aşağıdaki örnekte, üç sıra iletisini okuyun, ardından 10 saniye bekleyin (görünmezlik zaman ası). Daha sonra üç iletiyi tekrar okuyarak, **deleteMessage'ı**arayarak mesajları okuduktan sonra silebilirsiniz. İletiler silindikten sonra sırayı okumaya çalışırsanız, $queueMessage NULL olarak döndürülür.

```powershell
# Set the amount of time you want to entry to be invisible after read from the queue
# If it is not deleted by the end of this time, it will show up in the queue again
$invisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Read the message from the queue, then show the contents of the message. Read the other two messages, too.
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result

# After 10 seconds, these messages reappear on the queue.
# Read them again, but delete each one after reading it.
# Delete the message.
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
$queueMessage = $queue.CloudQueue.GetMessageAsync($invisibleTimeout,$null,$null)
$queueMessage.Result
$queue.CloudQueue.DeleteMessageAsync($queueMessage.Result.Id,$queueMessage.Result.popReceipt)
```

## <a name="delete-a-queue"></a>Bir kuyruk silme

Bir sırayı ve içinde bulunan tüm iletileri silmek için Kaldır-AzStorageQueue cmdlet'i arayın. Aşağıdaki örnek, Kaldır-AzStorageQueue cmdlet kullanarak bu alıştırmada kullanılan belirli kuyruğun nasıl silinir olduğunu gösterir.

```powershell
# Delete the queue
Remove-AzStorageQueue –Name $queueName –Context $ctx
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu alıştırmada oluşturduğunuz tüm varlıkları kaldırmak için kaynak grubunu kaldırın. Bu, ayrıca grubun içerdiği tüm kaynakları da siler. Bu durumda, oluşturulan depolama hesabı ve kaynak grubunun kendisini kaldırır.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılabilir makalesinde, PowerShell ile temel Kuyruk depolama yönetimi hakkında şunları öğrendiniz:

> [!div class="checklist"]
>
> * Bir kuyruk oluşturma
> * Sıra alma
> * İleti ekleme
> * Sonraki iletiyi okuyun
> * İleti silme
> * Bir kuyruk silme

### <a name="microsoft-azure-powershell-storage-cmdlets"></a>Microsoft Azure PowerShell Depolama cmdlets

* [Depolama PowerShell cmdlet’leri](/powershell/module/az.storage)

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
