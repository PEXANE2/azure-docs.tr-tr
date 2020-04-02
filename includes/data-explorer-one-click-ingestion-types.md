---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 30/03/2020
ms.author: orspodek
ms.openlocfilehash: 9ae820097f8515dc44c67d95366f96c241cb77a1
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80523056"
---
## <a name="select-an-ingestion-type"></a>Bir yutma türü seçin

**Yutma türü için**aşağıdaki seçeneklerden birini seçin:
   * **depolama dan** - **Bağlantı** depolama alanına, depolama hesabının URL ekleyin. Özel depolama hesapları için [Blob SAS URL'yi](/azurevs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) kullanın.
   
      ![Depolamadan tek tıklamayla alma](media/data-explorer-one-click-ingestion-types/from-storage-blob.png)

    * **dosyadan** - Dosyayı bulmak için **Gözat'ı** seçin veya dosyayı alana sürükleyin.
  
      ![Dosyadan tek tıklatma](media/data-explorer-one-click-ingestion-types/from-file.png)

    * **kapsayıcıdan** - **Bağlantı'dan depolama** alanına, kapsayıcının [SAS URL'sini](/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) ekleyin ve isteğe bağlı olarak örnek boyutunu girin.

      ![Konteynerden tek tıklamayla alma](media/data-explorer-one-click-ingestion-types/from-container.png)

  Verilerin bir örneği görüntülenir. İsterseniz, yalnızca belirli karakterlerle biten dosyaları göstermek için filtreleyebilirsiniz. Filtreleri ayarladığınızda, önizleme otomatik olarak güncellenir.
  
  Örneğin, sözcük *verileriyle* başlayan ve *.csv.gz* uzantısıyla biten tüm dosyalara filtre uygulayabilirsiniz.

  ![Tek tıklamayla yutma filtresi](media/data-explorer-one-click-ingestion-types/from-container-with-filter.png)
