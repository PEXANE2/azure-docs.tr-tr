---
title: BLOB depolama verilerini Azure Depolama Gezgini Team Data Science Işlemiyle taşıma
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76720920"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Azure Depolama Gezgini kullanarak Azure Blob depolama alanına veya buradan veri taşıma
Azure Depolama Gezgini, Microsoft 'un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle çalışmanıza olanak sağlayan ücretsiz bir araçtır. Bu konu başlığı altında, Azure Blob depolama alanından verileri karşıya yüklemek ve indirmek için nasıl kullanılacağı açıklanmaktadır. Araç [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)adresinden indirilebilir.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> [Azure 'Daki veri bilimi sanal makineleri](virtual-machines.md)tarafından belirtilen BETIKLERLE ayarlanmış VM kullanıyorsanız, Azure Depolama Gezgini VM 'de zaten yüklüdür.
> 
> [!NOTE]
> Azure Blob depolama 'ya yönelik kapsamlı bir giriş için [Azure Blob temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx)' ne bakın.   
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu belgede, bu hesap için bir Azure aboneliğiniz, bir depolama hesabınız ve karşılık gelen depolama anahtarı olduğu varsayılır. Verileri karşıya yüklemeden/indirmeden önce Azure depolama hesabı adınızı ve hesap anahtarınızı bilmeniz gerekir. 

* Bir Azure aboneliği ayarlamak için bkz. [ücretsiz bir aylık deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Depolama hesabı oluşturma ve hesap ve anahtar bilgilerini alma hakkında yönergeler için bkz. [Azure Storage hesapları hakkında](../../storage/common/storage-create-storage-account.md). Azure Depolama Gezgini aracı ile hesaba bağlanmak için bu anahtarın gerekli olduğu şekilde, depolama hesabınızın erişim anahtarını aklınızda olun.
* Azure Depolama Gezgini araç [Microsoft Azure Depolama Gezgini](https://storageexplorer.com/)indirilebilir. Yüklemesi sırasında varsayılanları kabul edin.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Azure Depolama Gezgini kullan
Aşağıdaki adımlar Azure Depolama Gezgini kullanarak verileri karşıya yüklemeyi/indirmeyi belgeleyin. 

1. Microsoft Azure Depolama Gezgini başlatın.
2. **Hesabınızda oturum açın...** sihirbazında **Azure hesap ayarları** simgesini seçin, sonra **bir hesap ekleyin** ve kimlik bilgilerinizi girin. 
![Azure depolama hesabı ekleme](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. **Azure depolama 'Ya bağlanma** Sihirbazı 'nı açmak Için **Azure depolama 'ya Bağlan** simgesini seçin. !["Azure depolama 'ya Bağlan" a tıklayın](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Azure Depolama hesabınızdaki erişim anahtarını **Azure Storage 'A Bağlan** sihirbazına girin ve ardından **İleri**' ye gidin. ![Azure depolama hesabından erişim anahtarı girin](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. **Hesap adı** kutusuna depolama hesabı adını girin ve ardından **İleri**' yi seçin. ![Harici depolama Ekle](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Eklenen depolama hesabı artık görüntülenmelidir. Bir depolama hesabında blob kapsayıcısı oluşturmak için, bu hesaptaki **BLOB kapsayıcıları** düğümüne sağ tıklayın, **BLOB kapsayıcısı oluştur**' u seçin ve bir ad girin.
7. Bir kapsayıcıya veri yüklemek için hedef kapsayıcıyı seçin ve **karşıya yükle** düğmesine tıklayın.
![Depolama hesapları](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. **Dosyalar** kutusunun sağ tarafındaki **..** . öğesine tıklayın, dosya sisteminden karşıya yüklenecek bir veya birden çok dosya seçin ve dosyaları karşıya yüklemeye başlamak için **karşıya yükle** ' ye tıklayın. ![Dosyaları karşıya yükle](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Verileri indirmek için, karşılık gelen kapsayıcıda bir blobu **seçip İndir ' e tıklayın.** ![Dosyaları indirme](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

