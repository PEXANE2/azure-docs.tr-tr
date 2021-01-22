---
author: baanders
description: Azure dijital TWINS öğreticileri için dosya ekleme-örnek proje önkoşulları
ms.service: digital-twins
ms.topic: include
ms.date: 1/20/2021
ms.author: baanders
ms.openlocfilehash: 5a1baf9631f2d30dd14ff16d2d34beda04605c6c
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98660530"
---
## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce **[ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun** .

Ayrıca, başlamadan önce geliştirme **makinenize [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/), sürüm 16,5 veya üstünü yükleyebilirsiniz** . Daha eski bir sürümü zaten yüklüyse, makinenizde *Visual Studio yükleyicisi* uygulamayı açabilir ve yüklemenizi güncelleştirmek için istemleri izleyebilirsiniz.

Öğretici, C# dilinde yazılmış örnek bir proje tarafından çalıştırılır. Örnek şurada bulunur: [Azure Digital TWINS uçtan uca örnekler](/samples/azure-samples/digital-twins-samples/digital-twins-samples). Örnek bağlantısına gidip başlık altındaki *kodu görüntüle* düğmesini seçerek makinenizde **örnek projeyi alın** . Bu işlem sizi, örnek olarak indirebileceğiniz GitHub deposuna götürür *.* *Kod* düğmesini seçip zip ' i *indirin*.

:::image type="content" source="../articles/digital-twins/media/includes/download-repo-zip.png" alt-text="GitHub 'da dijital-TWINS-örnek deposunun görünümü. Kod düğmesi seçilidir ve posta Indirme düğmesinin vurgulandığı küçük bir iletişim kutusu oluşturulur." lightbox="../articles/digital-twins/media/includes/download-repo-zip.png":::

Bu, bir yükler *. ZIP* klasörünü makinenize **digital-twins-samples-master.zip**. Klasörü sıkıştırmayı açın ve dosyaları ayıklayın.

### <a name="prepare-an-azure-digital-twins-instance"></a>Azure dijital TWINS örneği hazırlama

[!INCLUDE [Azure Digital Twins: instance prereq](digital-twins-prereq-instance.md)]

[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](digital-twins-local-credentials-outer.md)]
