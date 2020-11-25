---
ms.openlocfilehash: c2b1e1d87600e83733eb8bdbf5bc6e2e63376cf4
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993028"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Bulut uzamsal bağlayıcısını bulma

Daha önce kaydedilen bir bulut uzamsal çıpası, Azure uzamsal Tutturucuların kullanılmasına yönelik başlıca nedenlerden biridir. Bulut uzamsal bağlayıcısını bulmanın birkaç farklı yolu vardır. Her seferinde bir izleyici üzerinde bir strateji kullanabilirsiniz.
- Tutturucuların tanımlayıcısını bulun.
- Daha önce bulunan bir tutturucuya bağlı olan bağlantıları bulun. [Buradan](../articles/spatial-anchors/concepts/anchor-relationships-way-finding.md)bağlayıcı ilişkileri hakkında bilgi edinebilirsiniz.
- [Kaba reyerelleştirme](../articles/spatial-anchors/concepts/coarse-reloc.md)kullanarak bağlayıcıyı bulun.

> [!NOTE]
> Her bir bağlayıcıyı her bulduğunuzda, Azure uzamsal bağlantıları, bağlayıcının görsel bilgilerini artırmak için toplanan ortam verilerini kullanmaya çalışır. Bir bağlayıcıyı bulmakta sorun yaşıyorsanız, bir tutturucu oluşturmak ve farklı açılardan ve aydınlatma koşullarından birkaç kez bulmak yararlı olabilir.

Bulut uzamsal bağlayıcılarını tanımlayıcıya göre konumlandırdıysanız, bulut uzamsal bağlantı tanımlayıcısını uygulamanızın arka uç hizmetinde depolamak ve bunu düzgün şekilde doğrulayabilecek tüm cihazların erişimine açık hale getirmek isteyeceksiniz. Buna bir örnek için bkz. [öğretici: cihaz genelinde uzamsal bağlayıcıları paylaşma](../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md).

Bir nesne örneği oluşturun `AnchorLocateCriteria` , aradığınız tanımlayıcıları ayarlayın ve öğesini `CreateWatcher` sağlayarak oturumdaki yöntemi çağırın `AnchorLocateCriteria` .