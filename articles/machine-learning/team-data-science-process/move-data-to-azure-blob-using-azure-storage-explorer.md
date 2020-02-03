---
title: Azure Depolama Gezgini - Team Data Science Process ile BLOB Depolama veri taşıma
description: Azure Blob Storage 'dan verileri karşıya yüklemek ve indirmek için Azure Depolama Gezgini kullanmayı öğrenin.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: bfc63c6f5aca92fb7fda9e3ecf63ce4c332b12ef
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720920"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Ve Azure Depolama Gezgini'ni kullanarak Azure Blob depolama alanından verileri taşıma
Azure Depolama Gezgini, Microsoft'un Windows, macOS ve Linux'ta Azure depolama verileriyle çalışmanıza olanak sağlayan ücretsiz bir araçtır. Bu konuda, yüklemek ve Azure blob depolama alanından verileri indirmek için nasıl kullanılacağını açıklar. Araç [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)adresinden indirilebilir.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> [Azure 'Daki veri bilimi sanal makineleri](virtual-machines.md)tarafından belirtilen BETIKLERLE ayarlanmış VM kullanıyorsanız, Azure Depolama Gezgini VM 'de zaten yüklüdür.
> 
> [!NOTE]
> Azure Blob depolama 'ya yönelik kapsamlı bir giriş için [Azure Blob temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx)' ne bakın.   
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu belge, bir Azure aboneliği, bir depolama hesabı ve karşılık gelen depolama anahtarını ilgili hesabın sahibi olduğunuzu varsayar. Verileri karşıya yüklemeden/indirmeden önce Azure depolama hesabı adınızı ve hesap anahtarınızı bilmeniz gerekir. 

* Bir Azure aboneliği ayarlamak için bkz. [ücretsiz bir aylık deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Depolama hesabı oluşturma ve hesap ve anahtar bilgilerini alma hakkında yönergeler için bkz. [Azure Storage hesapları hakkında](../../storage/common/storage-create-storage-account.md). Bu anahtarı Azure Depolama Gezgini aracı ile hesaba bağlanmak için gereken depolama hesabınızın erişim anahtarı not edin.
* Azure Depolama Gezgini araç [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)indirilebilir. Varsayılan yükleme işlemi sırasında kabul edin.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Azure Depolama Gezgini'ni kullanma
Aşağıdaki adımlar, nasıl Azure Depolama Gezgini'ni kullanarak verileri karşıya yükleme/indirme belgeleyin. 

1. Microsoft Azure Depolama Gezgini'ni başlatın.
2. **Hesabınızda oturum açın...** sihirbazında **Azure hesap ayarları** simgesini seçin, sonra **bir hesap ekleyin** ve kimlik bilgilerinizi girin. 
Azure Storage hesabı eklemek ![](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. **Azure depolama 'Ya bağlanma** Sihirbazı 'nı açmak Için **Azure depolama 'ya Bağlan** simgesini seçin. !["Azure depolama 'ya Bağlan" seçeneğine tıklayın](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Azure Depolama hesabınızdaki erişim anahtarını **Azure Storage 'A Bağlan** sihirbazına girin ve ardından **İleri**' ye gidin. ![Azure Storage hesabından erişim anahtarı girin](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. **Hesap adı** kutusuna depolama hesabı adını girin ve ardından **İleri**' yi seçin. dış depolama](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png) eklemek ![
6. Eklenen depolama hesabı artık görüntülenmelidir. Bir depolama hesabında blob kapsayıcısı oluşturmak için, bu hesaptaki **BLOB kapsayıcıları** düğümüne sağ tıklayın, **BLOB kapsayıcısı oluştur**' u seçin ve bir ad girin.
7. Bir kapsayıcıya veri yüklemek için hedef kapsayıcıyı seçin ve **karşıya yükle** düğmesine tıklayın.
![Depolama hesapları](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. **Dosyalar** kutusunun sağ tarafındaki **..** . öğesine tıklayın, dosya sisteminden karşıya yüklenecek bir veya birden çok dosya seçin ve dosyaları karşıya yüklemeye başlamak için **karşıya yükle** ' ye tıklayın. Karşıya dosya yükleme![](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Verileri indirmek için, karşılık gelen kapsayıcıda bir blobu **seçip İndir ' e tıklayın.** dosyaları Indirmek ![](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

