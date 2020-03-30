---
title: Görüntüleri kilitleme
description: Bir kapsayıcı resmi veya deposu için öznitelikleri, azure kapsayıcı kayıt defterinde silinmeyecek veya üzerine yazılamaz şekilde ayarlayın.
ms.topic: article
ms.date: 09/30/2019
ms.openlocfilehash: da84767523bb6d948b71b1c1ad2ddaffb628354a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659705"
---
# <a name="lock-a-container-image-in-an-azure-container-registry"></a>Kapsayıcı görüntüsünü Azure kapsayıcı kayıt defterine kilitleme

Azure kapsayıcı kayıt defterinde, görüntü sürümünü veya deposu silinmeyecek veya güncelleştirilemeyecek şekilde kilitleyebilirsiniz. Bir resmi veya depoyu kilitlemek için, Azure CLI [komutu az acr deposu güncelleştirmesini][az-acr-repository-update]kullanarak özniteliklerini güncelleştirin. 

Bu makale, Azure CLI'yi Azure Bulut Su Şur'da veya yerel olarak çalıştırmanızı gerektirir (sürüm 2.0.55 veya daha sonra önerilir). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

> [!IMPORTANT]
> Bu makale, örneğin Azure portalındaki **Ayarlar > Kilitleri'ni** veya Azure CLI'deki `az lock` komutları kullanarak tüm kayıt defterini kilitlemek için geçerli değildir. Bir kayıt defteri kaynağını kilitlemek, depolarda veri oluşturmanızı, güncelleştirmenizi veya silmenizi engellemez. Bir kayıt defterini kilitlemek yalnızca çoğaltma ekleme veya silme veya kayıt defterinin kendisini silme gibi yönetim işlemlerini etkiler. [Beklenmeyen değişiklikleri önlemek için Kilit kaynaklarında](../azure-resource-manager/management/lock-resources.md)daha fazla bilgi.

## <a name="scenarios"></a>Senaryolar

Varsayılan olarak, Azure Kapsayıcı Kayıt Defteri'nde etiketlenmiş bir resim *tusuk,* böylece uygun izinlerle aynı etikete sahip bir resmi tekrar tekrar güncelleyebilir ve bir kayıt defterine itebilirsiniz. Kapsayıcı görüntüleri de gerektiği gibi [silinebilir.](container-registry-delete.md) Bu davranış, resim geliştirdiğinizde ve kayıt defteriniz için bir boyutu korumanız gerektiğinde yararlıdır.

Ancak, bir kapsayıcı görüntüsünü üretime *dağıttığınızda, değişmez* bir kapsayıcı görüntüsüne ihtiyacınız olabilir. Değişmez bir resim, yanlışlıkla silemediğiniz veya üzerine yazamadığınız bir resimdir.

Kayıt defterinizdeki görüntüleri etiketleme ve sürüm stratejileri için kapsayıcı görüntülerini etiketleme ve [sürüm önerilerine](container-registry-image-tag-version.md) bakın.

Depo özniteliklerini ayarlamak için [az acr deposu güncelleştirme][az-acr-repository-update] komutunu kullanın, böylece şunları yapabilirsiniz:

* Bir resim sürümünü veya tüm depoları kilitleme

* Bir resim sürümünü veya deposu silmeişleminden koruyun, ancak güncelleştirmeleri

* Görüntü sürümünde veya deponun tamamının okuma (çekme) işlemlerini önleme

Örnekler için aşağıdaki bölümlere bakın. 

## <a name="lock-an-image-or-repository"></a>Görüntüyü veya depoları kilitleme 

### <a name="show-the-current-repository-attributes"></a>Geçerli depo özniteliklerini göster
Bir deponun geçerli özniteliklerini görmek için aşağıdaki [az acr deposu gösteri][az-acr-repository-show] komutunu çalıştırın:

```azurecli
az acr repository show \
    --name myregistry --repository myrepo \
    --output jsonc
```

### <a name="show-the-current-image-attributes"></a>Geçerli görüntü özniteliklerini göster
Bir etiketin geçerli özniteliklerini görmek için aşağıdaki [az acr deposu gösteri][az-acr-repository-show] komutunu çalıştırın:

```azurecli
az acr repository show \
    --name myregistry --image image:tag \
    --output jsonc
```

### <a name="lock-an-image-by-tag"></a>Görüntüyü etikete göre kilitleme

*Myrepo/myimage:tag* image'ı *myregistry'de*kilitlemek için aşağıdaki [az acr deposu güncelleştirme][az-acr-repository-update] komutunu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --write-enabled false
```

### <a name="lock-an-image-by-manifest-digest"></a>Görüntüyü manifest sindirimine göre kilitleme

Manifest digest (SHA-256 karma, olarak `sha256:...`temsil), tarafından tanımlanan bir *myrepo / myimage* görüntü kilitlemek için aşağıdaki komutu çalıştırın. (Bir veya daha fazla resim etiketiyle ilişkili bildirim özetini bulmak için [az acr deposu nu göster-manifests][az-acr-repository-show-manifests] komutunu çalıştırın.)

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage@sha256:123456abcdefg \
    --write-enabled false
```

### <a name="lock-a-repository"></a>Depokilitleme

*Myrepo/myimage* deposunu ve tüm görüntüleri kilitlemek için aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --write-enabled false
```

## <a name="protect-an-image-or-repository-from-deletion"></a>Görüntüyü veya depoyu silmeişleminden koruma

### <a name="protect-an-image-from-deletion"></a>Görüntüyü silmeden koruyun

*Myrepo/myimage:tag* görüntüsünün güncellenmesine ancak silinmemesine izin vermek için aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --delete-enabled false --write-enabled true
```

### <a name="protect-a-repository-from-deletion"></a>Depoyu silmeden koruma

Aşağıdaki komut *myrepo/myimage* deposunu ayarlar, böylece silinemez. Tek tek görüntüler yine de güncellenebilir veya silinebilir.

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --delete-enabled false --write-enabled true
```

## <a name="prevent-read-operations-on-an-image-or-repository"></a>Görüntü veya depodaki okuma işlemlerini önleme

*Myrepo/myimage:tag* görüntüsünde okuma (çekme) işlemlerini önlemek için aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --read-enabled false
```

*Myrepo/myimage* deposundaki tüm görüntülerdeki okuma işlemlerini önlemek için aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --read-enabled false
```

## <a name="unlock-an-image-or-repository"></a>Bir resmin veya deponun kilidini açın

*Myrepo/myimage:tag* image'ın varsayılan davranışını geri yüklemek için silinip güncelleştirilebilsin, aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --delete-enabled true --write-enabled true
```

*Myrepo/myimage* deposunun varsayılan davranışını ve tüm görüntüleri silinip güncelleştirilebilmeleri için geri yüklemek için aşağıdaki komutu çalıştırın:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --delete-enabled true --write-enabled true
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir depodaki görüntü sürümlerinin silinmesini veya güncelleştirilmesini önlemek için [az acr deposu güncelleştirme][az-acr-repository-update] komutunu kullanmayı öğrendiniz. Ek öznitelikleri ayarlamak için [az acr deposu güncelleştirme][az-acr-repository-update] komutu başvurusuna bakın.

Görüntü sürümü veya deposu için ayarlanan öznitelikleri görmek için [az acr deposu gösteri][az-acr-repository-show] komutunu kullanın.

Silme işlemleri yle ilgili ayrıntılar için Azure [Kapsayıcı Kayıt Defteri'ndeki kapsayıcı resimlerini sil'e][container-registry-delete]bakın.

<!-- LINKS - Internal -->
[az-acr-repository-update]: /cli/azure/acr/repository#az-acr-repository-update
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-delete]: container-registry-delete.md

