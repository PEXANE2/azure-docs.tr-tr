---
ms.openlocfilehash: b5fec8bbc0db78454b080a411702014bd96f7db9
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76887770"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Bulut uzamsal çapası bulma

Azure Uzamsal Çapaları kullanmanın başlıca nedenlerinden biri, önceden kaydedilmiş bir bulut uzamsal çapasını bulabilmektir. Bir bulut uzamsal çapa sıyrık bulmak için birkaç farklı yolu vardır. Bir anda bir gözcü üzerinde bir strateji kullanabilirsiniz.
- Bağlantı larını tanımlayıcıya göre bulun.
- Daha önce bulunan bir çapaya bağlı çapaları bulun. [Burada](/azure/spatial-anchors/concepts/anchor-relationships-way-finding/)çapa ilişkileri hakkında bilgi edinebilirsiniz.
- [Kaba yeniden yerelleştirme](/azure/spatial-anchors/concepts/coarse-reloc/)kullanarak çapa bulun.

Bulut uzamsal bağlantı larını tanımlayıcıya göre konumlandırıyorsanız, bulut uzamsal bağlantı tanımlayıcısını uygulamanızın arka uç hizmetinde depolamak ve doğru doğrudoğrula doğrulayabilen tüm aygıtlar için erişilebilir hale getirmek isteyebilirsiniz. Bunun bir örneği için, [bkz.](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/)

Bir `AnchorLocateCriteria` nesneyi anında ayarlayın, aradığınız tanımlayıcıları ayarlayın ve oturumdaki `CreateWatcher` yöntemi' nizi `AnchorLocateCriteria`sağlayarak.