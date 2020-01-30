---
ms.openlocfilehash: b5fec8bbc0db78454b080a411702014bd96f7db9
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76887770"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Bulut uzamsal bağlayıcısını bulma

Daha önce kaydedilen bir bulut uzamsal çıpası, Azure uzamsal Tutturucuların kullanılmasına yönelik başlıca nedenlerden biridir. Bulut uzamsal bağlayıcısını bulmanın birkaç farklı yolu vardır. Her seferinde bir izleyici üzerinde bir strateji kullanabilirsiniz.
- Tutturucuların tanımlayıcısını bulun.
- Daha önce bulunan bir tutturucuya bağlı olan bağlantıları bulun. [Buradan](/azure/spatial-anchors/concepts/anchor-relationships-way-finding/)bağlayıcı ilişkileri hakkında bilgi edinebilirsiniz.
- [Kaba reyerelleştirme](/azure/spatial-anchors/concepts/coarse-reloc/)kullanarak bağlayıcıyı bulun.

Bulut uzamsal bağlayıcılarını tanımlayıcıya göre konumlandırdıysanız, bulut uzamsal bağlantı tanımlayıcısını uygulamanızın arka uç hizmetinde depolamak ve bunu düzgün şekilde doğrulayabilecek tüm cihazların erişimine açık hale getirmek isteyeceksiniz. Buna bir örnek için bkz. [öğretici: cihaz genelinde uzamsal bağlayıcıları paylaşma](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

`AnchorLocateCriteria` bir nesne örneği oluşturun, aradığınız tanımlayıcıları ayarlayın ve `AnchorLocateCriteria`sağlayarak oturumdaki `CreateWatcher` yöntemini çağırın.