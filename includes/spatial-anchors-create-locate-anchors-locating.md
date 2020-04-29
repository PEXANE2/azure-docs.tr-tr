---
ms.openlocfilehash: b5fec8bbc0db78454b080a411702014bd96f7db9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76887770"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Bulut uzamsal bağlayıcısını bulma

Daha önce kaydedilen bir bulut uzamsal çıpası, Azure uzamsal Tutturucuların kullanılmasına yönelik başlıca nedenlerden biridir. Bulut uzamsal bağlayıcısını bulmanın birkaç farklı yolu vardır. Her seferinde bir izleyici üzerinde bir strateji kullanabilirsiniz.
- Tutturucuların tanımlayıcısını bulun.
- Daha önce bulunan bir tutturucuya bağlı olan bağlantıları bulun. [Buradan](/azure/spatial-anchors/concepts/anchor-relationships-way-finding/)bağlayıcı ilişkileri hakkında bilgi edinebilirsiniz.
- [Kaba reyerelleştirme](/azure/spatial-anchors/concepts/coarse-reloc/)kullanarak bağlayıcıyı bulun.

Bulut uzamsal bağlayıcılarını tanımlayıcıya göre konumlandırdıysanız, bulut uzamsal bağlantı tanımlayıcısını uygulamanızın arka uç hizmetinde depolamak ve bunu düzgün şekilde doğrulayabilecek tüm cihazların erişimine açık hale getirmek isteyeceksiniz. Buna bir örnek için bkz. [öğretici: cihaz genelinde uzamsal bağlayıcıları paylaşma](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Bir `AnchorLocateCriteria` nesne örneği oluşturun, aradığınız tanımlayıcıları ayarlayın ve öğesini sağlayarak oturumdaki `CreateWatcher` yöntemi çağırın. `AnchorLocateCriteria`