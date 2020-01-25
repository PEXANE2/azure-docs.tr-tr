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
Azure Depolama Gezgini, Microsoft'un Windows, macOS ve Linux'ta Azure depolama verileriyle çalışmanıza olanak sağlayan ücretsiz bir araçtır. Bu konuda, yüklemek ve Azure blob depolama alanından verileri indirmek için nasıl kullanılacağını açıklar. Aracı indirilebileceğini [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Tarafından sağlanan betikleri ile oluşturulan VM kullanıyorsanız [azure'da veri bilimi sanal makineleri](virtual-machines.md), ardından Azure Depolama Gezgini VM üzerinde zaten yüklü.
> 
> [!NOTE]
> Azure blob depolama için bir tam giriş için başvurmak [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu belge, bir Azure aboneliği, bir depolama hesabı ve karşılık gelen depolama anahtarını ilgili hesabın sahibi olduğunuzu varsayar. Verileri karşıya yüklemeden/indirmeden önce Azure depolama hesabı adınızı ve hesap anahtarınızı bilmeniz gerekir. 

* Bir Azure aboneliğini ayarlama hakkında bilgi için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Depolama hesabı oluşturma ve hesap ve anahtar bilgilerini alma hakkında yönergeler için bkz. [Azure Storage hesapları hakkında](../../storage/common/storage-create-storage-account.md). Bu anahtarı Azure Depolama Gezgini aracı ile hesaba bağlanmak için gereken depolama hesabınızın erişim anahtarı not edin.
* Azure Depolama Gezgini aracını indirilebileceğini [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/). Varsayılan yükleme işlemi sırasında kabul edin.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Azure Depolama Gezgini'ni kullanma
Aşağıdaki adımlar, nasıl Azure Depolama Gezgini'ni kullanarak verileri karşıya yükleme/indirme belgeleyin. 

1. Microsoft Azure Depolama Gezgini'ni başlatın.
2. Ortaya çıkarmak için **hesabınızda oturum açın...**  seçin **Azure hesap ayarları** simgesine ve ardından **Hesap Ekle** ve kimlik bilgilerini girin. 
Azure Storage hesabı eklemek ![](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. **Azure depolama 'Ya bağlanma** Sihirbazı 'nı açmak Için **Azure depolama 'ya Bağlan** simgesini seçin. !["Azure depolama 'ya Bağlan" seçeneğine tıklayın](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Azure Depolama hesabınızdaki erişim anahtarını **Azure Storage 'A Bağlan** sihirbazına girin ve ardından **İleri**' ye gidin. ![Azure Storage hesabından erişim anahtarı girin](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Depolama hesabı adını girin **hesap adı** kutusuna ve ardından **sonraki**. ![Dış depolama Ekle](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Eklenen depolama hesabı artık görüntülenmelidir. Bir depolama hesabında blob kapsayıcısı oluşturmak için **Blob kapsayıcıları** seçin, hesap düğümünde **Blob kapsayıcısı Oluştur**ve bir ad girin.
7. Bir kapsayıcıya veri yüklemek için hedef kapsayıcıyı seçin ve **karşıya yükle** düğmesine tıklayın.
![Depolama hesapları](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Tıklayarak **...**  sağındaki **dosyaları** kutusunda, dosya sisteminden karşıya yükleyin ve bir veya birden çok dosya seçin **karşıya** dosyalar karşıya yüklenirken başlamaya.![ Dosyaları karşıya yükleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Verileri, blob indirmek ve'ı tıklatın, karşılık gelen kapsayıcı seçerek indirmek için **indirme**. ![Dosyaları indirme](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

