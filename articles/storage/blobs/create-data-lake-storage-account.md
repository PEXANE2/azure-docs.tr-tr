---
title: Azure Data Lake Storage 2. için bir depolama hesabı oluşturun
description: Azure Data Lake Storage 2. ile kullanmak üzere bir depolama hesabı oluşturmayı öğrenin.
author: normesta
ms.topic: how-to
ms.author: normesta
ms.date: 08/31/2020
ms.service: storage
ms.reviewer: stewu
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 6b63933fc625079bb490942cf3a32232a484fe38
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89270325"
---
# <a name="create-a-storage-account-to-use-with-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. ile kullanmak için bir depolama hesabı oluşturma

Data Lake Storage 2. yeteneklerini kullanmak için hiyerarşik bir ad alanına sahip bir depolama hesabı oluşturun.

## <a name="choose-a-storage-account-type"></a>Depolama hesabı türü seçin

Data Lake Storage özellikleri aşağıdaki depolama hesabı türlerinde desteklenir:

- Genel amaçlı v2
- BlockBlobStorage

Aralarında nasıl seçim yapılacağı hakkında daha fazla bilgi için bkz. [depolama hesabına genel bakış](../common/storage-account-overview.md).

## <a name="create-a-storage-account-with-a-hierarchical-namespace"></a>Hiyerarşik ad alanı olan bir depolama hesabı oluşturma

**Hiyerarşik ad alanı** ayarı etkin olan bir [genel amaçlı v2 hesabı](../common/storage-account-create.md) ya da [blockblobstorage](storage-blob-create-account-block-blob.md) hesabı oluşturun.

**Depolama hesabı oluştur** sayfasının **Gelişmiş** sekmesinde **hiyerarşik ad alanı** ayarını etkinleştirerek, hesabı oluştururken Data Lake Storage yeteneklerini açın. Hesabı oluştururken bu ayarı etkinleştirmeniz gerekir. Daha sonra etkinleştiremezsiniz.

Aşağıdaki görüntüde, bu ayar **depolama hesabı oluştur** sayfasında gösterilmektedir.

> [!div class="mx-imgBorder"]
> ![Hiyerarşik ad alanı ayarı](./media/create-data-lake-storage-account/hierarchical-namespace-feature.png)

Data Lake Storage ile kullanmak istediğiniz mevcut bir depolama hesabınız varsa ve hiyerarşik ad alanı ayarı devre dışı bırakılmışsa, verileri ayarı etkinleştirilmiş yeni bir depolama hesabına geçirmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabına genel bakış](../common/storage-account-overview.md)
- [Büyük veri gereksinimleri için Azure Data Lake Storage 2. kullanma](data-lake-storage-data-scenarios.md)
- [Azure Data Lake Storage 2. Nesil'de Erişim Denetimi](data-lake-storage-access-control.md)