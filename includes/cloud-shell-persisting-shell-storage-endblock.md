---
author: cynthn
ms.service: azure
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: c2ed33aea77b5478e8d17f6bd0213ef3e778b806
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188708"
---
## <a name="transfer-local-files-to-cloud-shell"></a>Yerel dosyaları Cloud Shell aktar
`clouddrive`Dizin Azure Portal depolama dikey penceresinde eşitlenir. Yerel dosyaları dosya paylaşımınızdan veya paylaşımdan aktarmak için bu dikey pencereyi kullanın. Cloud Shell içinden dosya güncelleştirme, dikey pencereyi yenilediğinizde dosya depolama GUI 'ye yansıtılır.

### <a name="download-files"></a>Dosyaları indirme

![Yerel dosyaların listesi](../articles/cloud-shell/media/persisting-shell-storage/download.png)
1. Azure portal, bağlı dosya paylaşımıyla gidin.
2. Hedef dosyayı seçin.
3. **İndir** düğmesini seçin.

### <a name="upload-files"></a>Dosyaları karşıya yükleme

![Karşıya yüklenecek yerel dosyalar](../articles/cloud-shell/media/persisting-shell-storage/upload.png)
1. Bağlı dosya paylaşımınıza gidin.
2. Ardından **Yükle** düğmesini seçin.
3. Karşıya yüklemek istediğiniz dosya veya dosyaları seçin.
4. Karşıya yüklemeyi onaylayın.

Artık Cloud Shell, dizininizde erişilebilir olan dosyaları görmeniz gerekir `clouddrive` .