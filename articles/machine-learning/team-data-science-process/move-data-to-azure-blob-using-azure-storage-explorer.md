---
title: Azure Depolama Gezgini ile Blob depolama verilerini taşıma - Ekip Veri Bilimi Süreci
description: Azure blob depolamadan veri yüklemek ve indirmek için Azure Depolama Gezgini'ni nasıl kullanacağınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720920"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Azure Depolama Gezgini'ni kullanarak verileri Azure Blob Depolama alanına taşıyın
Azure Depolama Gezgini, Microsoft'un Windows, macOS ve Linux'taki Azure Depolama verileriyle çalışmanızı sağlayan ücretsiz bir araçtır. Bu konu, Azure blob depolamadan veri yüklemek ve indirmek için nasıl kullanılacağını açıklar. Araç [Microsoft Azure Depolama Gezgini'nden](https://storageexplorer.com/)indirilebilir.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> [Azure'daki Data Science Virtual makineleri](virtual-machines.md)tarafından sağlanan komut dosyalarıyla ayarlanmış VM kullanıyorsanız, Azure Depolama Gezgini VM'ye zaten yüklenmiş demektir.
> 
> [!NOTE]
> Azure blob depolamasına tam bir giriş için [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob Hizmeti'ne](https://msdn.microsoft.com/library/azure/dd179376.aspx)bakın.   
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu belge, bir Azure aboneliğiniz, depolama hesabınız ve bu hesap için karşılık gelen depolama anahtarınız olduğunu varsayar. Verileri yüklemeden/indirmeden önce Azure Depolama hesap adınızı ve hesap anahtarınızı bilmeniz gerekir. 

* Azure aboneliği ayarlamak için [ücretsiz bir aylık deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)bakın.
* Depolama hesabı oluşturma ve hesap ve önemli bilgileri alma yla ilgili talimatlar için Azure [Depolama hesapları hakkında'ya](../../storage/common/storage-create-storage-account.md)bakın. Azure Depolama Gezgini aracıyla hesaba bağlanmak için bu tuşa ihtiyaç duyduğunuziçin depolama hesabınız için erişim anahtarını not alın.
* Azure Depolama Gezgini aracı [Microsoft Azure Depolama Gezgini'nden](https://storageexplorer.com/)indirilebilir. Yükleme sırasında varsayılanları kabul edin.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Azure Depolama Gezgini'ni kullanma
Aşağıdaki adımlar, Azure Depolama Gezgini'ni kullanarak verilerin nasıl yüklendiğini/indirilmesini belgelemelidir. 

1. Microsoft Azure Depolama Gezgini'ni başlatın.
2. **Hesabınızda Oturum Aç'ı** getirmek için... sihirbazı Azure **hesap ayarları** simgesini seçin, ardından **hesap ekleyin** ve kimlik bilgilerinizi girin. 
![Azure Depolama hesabı ekleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. **Azure Depolamaya Bağlan** sihirbazını açmak **için Azure Depolama'ya Bağlan** simgesini seçin. !["Azure Depolamaya Bağlan" seçeneğini tıklayın](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Erişim anahtarını Azure Depolama'ya **Bağla** sihirbazına ve **ardından İleri'ye**Azure Depolama hesabınızdan girin. ![Azure Depolama hesabından erişim anahtarını girin](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. **Hesap adı** kutusuna depolama hesabı adını girin ve **sonra İleri'yi**seçin. ![Harici depolama alanı ekleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Eklenen depolama hesabı şimdi görüntülenmelidir. Depolama hesabında bir blob kapsayıcısı oluşturmak için, bu hesaptaki **Blob Kapsayıcılar** düğümüne sağ tıklayın, **Blob Kapsayıcısı Oluştur'u**seçin ve bir ad girin.
7. Bir kapsayıcıya veri yüklemek için hedef kapsayıcıyı seçin ve **Yükle** düğmesini tıklatın.
![Depolama hesapları](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. **Dosyalar** kutusunun sağındaki **...** düğmesine tıklayın, dosya sisteminden yüklemek için bir veya birden fazla dosya seçin ve dosyaları yüklemeye başlamak için **Yükle'yi** tıklatın. ![Dosya yükleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Verileri indirmek için, karşıdan yükleme ve **İndir'i**tıklatın ilgili kapsayıcıdaki blob'u seçerek . ![Dosyaları indirme](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

