---
title: Etiketlenmemiş bildirimleri tutma ilkesi
description: Tanımlı bir süre sonra etiketlenmemiş bildirimlerin otomatik olarak silinmesi için Azure kapsayıcı kayıt defterinizde bekletme ilkesini nasıl etkinleştirdiğinizi öğrenin.
ms.topic: article
ms.date: 10/02/2019
ms.openlocfilehash: 912616b6ab95cdff91e70477c7d6de476ccfdfa7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74454822"
---
# <a name="set-a-retention-policy-for-untagged-manifests"></a>Etiketlenmemiş bildirimler için bekletme ilkesi ayarlama

Azure Kapsayıcı Kayıt Defteri, ilişkili etiketleri *(etiketlenmemiş bildirimler)* olmayan depolanmış görüntü bildirimleri için bir *bekletme ilkesi* ayarlama seçeneği sunar. Bekletme ilkesi etkinleştirildiğinde, kayıt defterindeki etiketlenmemiş bildirimler, ayarladığınız birkaç gün sonra otomatik olarak silinir. Bu özellik, kayıt defterinin gerekli olmayan yapılarla doldurulmasını önler ve depolama maliyetlerinden tasarruf etmenizi sağlar. `delete-enabled` Etiketlenmemiş bir bildirimin özniteliği `false`ayarlanmışsa, bildirim silinemez ve bekletme ilkesi geçerli değildir.

Bu makaledeki komut örneklerini çalıştırmak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.0.74 veya daha sonra gereklidir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

> [!IMPORTANT]
> Bu özellik şu anda önizlemede dir ve bazı [sınırlamalar geçerlidir.](#preview-limitations) Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

> [!WARNING]
> Bir bekletme ilkesini dikkatli bir şekilde ayarlayın-silinen görüntü verileri KURTARılamaz. Görüntüleri bildirim özetine göre çeken sistemleriniz varsa (görüntü adının aksine), etiketlenmemiş bildirimler için bir bekletme ilkesi belirlememelisiniz. Etiketlenmemiş görüntülerin silmesi, bu sistemlerin resimleri kayıt defterinizden çekmesini önler. Bunun yerine manifesto tarafından çekerek, *benzersiz* bir etiketleme düzeni, [önerilen en iyi uygulama](container-registry-image-tag-version.md)benimseyerek düşünün.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

* Yalnızca **Premium** kapsayıcı kayıt defteri bekletme ilkesiyle yapılandırılabilir. Kayıt defteri hizmet katmanları hakkında daha fazla bilgi için [Azure Konteyner Kayıt Defteri SK'leri'ne](container-registry-skus.md)bakın.
* Yalnızca etiketlenmemiş bildirimler için bekletme ilkesi ayarlayabilirsiniz.
* Bekletme ilkesi şu anda yalnızca ilke etkinleştirildikten *sonra* etiketlenmemiş bildirimler için geçerlidir. Kayıt defterinde varolan etiketlenmemiş bildirimler ilkeye tabi değildir. Varolan etiketlenmemiş bildirimleri silmek için [Azure Kapsayıcı Kayıt Defteri'ndeki kapsayıcı görüntülerini sil örneklerine](container-registry-delete.md)bakın.

## <a name="about-the-retention-policy"></a>Bekletme ilkesi hakkında

Azure Kapsayıcı Kayıt Defteri, kayıt defterindeki bildirimler için başvuru sayımı yapar. Bir bildirim etiketlendiğinde, bekletme ilkesini denetler. Bekletme ilkesi etkinleştirilirse, ilkede ayarlanan gün sayısına göre belirli bir tarihle birlikte bir bildirim silme işlemi sıraya alınır.

Ayrı bir sıra yönetimi işi, gerektiğinde ölçekleme yaparak iletileri sürekli olarak işler. Örnek olarak, 30 günlük bekletme ilkesine sahip bir kayıt defterinde 1 saat arayla iki bildirimi etiketsiz olarak gördüğünüzü varsayalım. İki ileti sıraya alınır. Daha sonra, 30 gün sonra, yaklaşık 1 saat arayla, iletiler sıradan alınır ve ipolitikasın hala geçerli olduğunu varsayarak işlenir.

## <a name="set-a-retention-policy---cli"></a>Bekletme ilkesi ni ayarlama - CLI

Aşağıdaki örnek, bir kayıt defterindeki etiketlenmemiş bildirimler için bekletme ilkesi ayarlamak için Azure CLI'yi nasıl kullanacağınızı gösterir.

### <a name="enable-a-retention-policy"></a>Bekletme ilkesini etkinleştirme

Varsayılan olarak, bir kapsayıcı kayıt defterinde hiçbir bekletme ilkesi ayarlanır. Bekletme ilkesini ayarlamak veya güncelleştirmek için Azure CLI'de [az acr config bekletme güncelleştirme][az-acr-config-retention-update] komutunu çalıştırın. Etiketlenmemiş bildirimleri korumak için 0 ile 365 arasında birkaç gün belirtebilirsiniz. Birkaç gün belirtmezseniz, komut varsayılan 7 gün olarak ayarlar. Bekletme süresinden sonra, kayıt defterindeki tüm etiketlenmemiş bildirimler otomatik olarak silinir.

Aşağıdaki örnek, kayıt *defteri kayıt defterindeki*etiketlenmemiş bildirimler için 30 günlük bir bekletme ilkesi belirler:

```azurecli
az acr config retention update --registry myregistry --status enabled --days 30 --type UntaggedManifests
```

Aşağıdaki örnek, etiketlendirildiği anda kayıt defterindeki herhangi bir bildirimi silmek için bir ilke belirler. Bu ilkeyi, 0 günlük bir bekletme süresi ayarlayarak oluşturun. 

```azurecli
az acr config retention update --registry myregistry --status enabled --days 0 --type UntaggedManifests
```

### <a name="validate-a-retention-policy"></a>Bekletme ilkesini doğrulama

Önceki ilkeyi 0 günlük bekletme süresiyle etkinleştirirseniz, etiketlenmemiş bildirimlerin silindiğini hızlı bir şekilde doğrulayabilirsiniz:

1. Bir test `hello-world:latest` görüntüsü görüntüsünü kayıt defterinize itin veya seçtiğiniz başka bir test görüntüsünü değiştirin.
1. Örneğin, `hello-world:latest` [az acr deposu etiketsiz][az-acr-repository-untag] komutunu kullanarak görüntüyü etiketleyin. Etiketlenmemiş bildirim kayıt defterinde kalır.
    ```azurecli
    az acr repository untag --name myregistry --image hello-world:latest
    ```
1. Birkaç saniye içinde, etiketlenmemiş bildirim silinir. Örneğin, [az acr deposu show-manifests][az-acr-repository-show-manifests] komutunu kullanarak, depodaki bildirimleri listeleyerek silme işlemini doğrulayabilirsiniz. Depodaki tek test görüntüsüyse, deponun kendisi silinir.

### <a name="disable-a-retention-policy"></a>Bekletme ilkesini devre dışı tutma

Bir kayıt defterinde ayarlanan bekletme ilkesini görmek için [az acr config bekletme göster][az-acr-config-retention-show] komutunu çalıştırın:

```azurecli
az acr config retention show --registry myregistry
```

Bir kayıt defterinde bekletme ilkesini devre dışı kılabilir, [az acr config bekletme güncelleştirme][az-acr-config-retention-update] komutunu çalıştırın ve ayarlayın: `--status disabled`

```azurecli
az acr config retention update --registry myregistry --status disabled --type UntaggedManifests
```

## <a name="set-a-retention-policy---portal"></a>Bekletme ilkesi ayarlama - portal

Azure [portalında](https://portal.azure.com)bir kayıt defterinin bekletme ilkesini de ayarlayabilirsiniz. Aşağıdaki örnek, bir kayıt defterindeki etiketlenmemiş bildirimler için bekletme ilkesi ayarlamak için portalı nasıl kullanacağınızı gösterir.

### <a name="enable-a-retention-policy"></a>Bekletme ilkesini etkinleştirme

1. Azure konteyner kayıt defterinize gidin. İlkeler **altında, Bekletme** (Önizleme) seçeneğini **belirleyin.**
1. **Durum'da**, **Etkin'i**seçin.
1. Etiketlenmemiş bildirimleri korumak için 0 ile 365 arasında birkaç gün seçin. **Kaydet'i**seçin.

![Azure portalında bekletme ilkesini etkinleştirme](media/container-registry-retention-policy/container-registry-retention-policy01.png)

### <a name="disable-a-retention-policy"></a>Bekletme ilkesini devre dışı tutma

1. Azure konteyner kayıt defterinize gidin. İlkeler **altında, Bekletme** (Önizleme) seçeneğini **belirleyin.**
1. **Durum'da**, **Devre Dışı' yı**seçin. **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Kapsayıcı Kayıt Defteri'nde [görüntüleri ve depoları silme](container-registry-delete.md) seçenekleri hakkında daha fazla bilgi edinin

* Bir kayıt defterinden seçili resim ve bildirimleri otomatik olarak nasıl [temizleyebilirsiniz](container-registry-auto-purge.md) öğrenin

* [Resim ve bildirimleri](container-registry-image-lock.md) kayıt defterine kilitleme seçenekleri hakkında daha fazla bilgi edinin

<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/


<!-- LINKS - internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-config-retention-update]: /cli/azure/acr/config/retention#az-acr-config-retention-update
[az-acr-config-retention-show]: /cli/azure/acr/config/retention#az-acr-config-retention-show
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
