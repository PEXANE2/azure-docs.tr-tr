---
ms.openlocfilehash: 4df49b4e68769a907423c4edef9f8820df4d20b7
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006483"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Bulut uzamsal bağlayıcısını bulma

Daha önce kaydedilen bir bulut uzamsal çıpası, Azure uzamsal Tutturucuların kullanılmasına yönelik başlıca nedenlerden biridir. Bulut uzamsal bağlayıcısını bulmanın birkaç farklı yolu vardır. Her seferinde bir izleyici üzerinde bir strateji kullanabilirsiniz.
- Tutturucuların tanımlayıcısını bulun.
- Daha önce bulunan bir tutturucuya bağlı olan bağlantıları bulun. [Buradan](/azure/spatial-anchors/concepts/anchor-relationships-way-finding/)bağlayıcı ilişkileri hakkında bilgi edinebilirsiniz.
- [Kaba reyerelleştirme](/azure/spatial-anchors/concepts/coarse-reloc/)kullanarak bağlayıcıyı bulun.

> [!NOTE]
> Her bir bağlayıcıyı her bulduğunuzda, Azure uzamsal bağlantıları, bağlayıcının görsel bilgilerini artırmak için toplanan ortam verilerini kullanmaya çalışır. Bir bağlayıcıyı bulmakta sorun yaşıyorsanız, bir tutturucu oluşturmak ve farklı açılardan ve aydınlatma koşullarından birkaç kez bulmak yararlı olabilir.

Bulut uzamsal bağlayıcılarını tanımlayıcıya göre konumlandırdıysanız, bulut uzamsal bağlantı tanımlayıcısını uygulamanızın arka uç hizmetinde depolamak ve bunu düzgün şekilde doğrulayabilecek tüm cihazların erişimine açık hale getirmek isteyeceksiniz. Buna bir örnek için bkz. [öğretici: cihaz genelinde uzamsal bağlayıcıları paylaşma](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Bir `AnchorLocateCriteria` nesne örneği oluşturun, aradığınız tanımlayıcıları ayarlayın ve öğesini sağlayarak oturumdaki `CreateWatcher` yöntemi çağırın. `AnchorLocateCriteria`