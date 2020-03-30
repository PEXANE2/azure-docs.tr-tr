---
title: Kapsayıcı grubunu güncelleştirme
description: Azure Kapsayıcı Örnekleri kapsayıcı gruplarınızda çalışan kapsayıcıları nasıl güncelleştiriştireceklerini öğrenin.
ms.topic: article
ms.date: 09/03/2019
ms.openlocfilehash: f57ebcf050b5563b45f10af57c1721338df88ff9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533312"
---
# <a name="update-containers-in-azure-container-instances"></a>Azure Container Instances’taki kapsayıcıları güncelleştirme

Kapsayıcı örneklerinin normal çalışması sırasında, çalışan kapsayıcıları bir kapsayıcı [grubunda](container-instances-container-groups.md)güncelleştirmek için gerekli bulabilirsiniz. Örneğin, görüntü sürümünü güncelleştirmek, Bir DNS adını değiştirmek, ortam değişkenlerini güncelleştirmek veya uygulaması çatlayan bir kapsayıcının durumunu yenilemek isteyebilirsiniz.

> [!NOTE]
> Sonlandırılan veya silinen kapsayıcı grupları güncelleştirici olamaz. Bir kapsayıcı grubu sonlandırıldıktan (Başarılı veya Başarısız durumda) veya silindikten sonra, grubun yeni olarak dağıtılması gerekir.

## <a name="update-a-container-group"></a>Kapsayıcı grubunu güncelleştirme

Varolan bir grubu en az bir değiştirilmiş özelliği olan bir grubu yeniden dağıtarak çalışan kapsayıcı grubundaki kapsayıcıları güncelleştirin. Bir kapsayıcı grubunu güncellediğinizde, gruptaki tüm çalışan kapsayıcılar genellikle aynı temel kapsayıcı ana bilgisayarda yerinde yeniden başlatılır.

Oluşturma komutunu (veya Azure portalını kullanın) vererek varolan bir kapsayıcı grubunu yeniden dağıtın ve varolan bir grubun adını belirtin. Yeniden dağıtımı tetiklemek için create komutunu verdiyseniz grubun en az bir geçerli özelliğini değiştirin ve kalan özellikleri değiştirmeden bırakın (veya varsayılan değerleri kullanmaya devam edin). Tüm kapsayıcı grubu özellikleri yeniden dağıtım için geçerli değildir. Desteklenmeyen özellikler listesi için [silme gerektiren Özellikler'e](#properties-that-require-container-delete) bakın.

Aşağıdaki Azure CLI örneği, yeni bir DNS ad etiketiyle bir kapsayıcı grubunu güncelleştirir. Grubun DNS ad etiketi özelliği güncelleştirilebilen bir özellik olduğundan, kapsayıcı grubu yeniden dağıtılır ve kapsayıcıları yeniden başlatılır.

DNS ad etiketi *myapplication-evreleme*ile ilk dağıtım:

```azurecli-interactive
# Create container group
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication-staging
```

Kapsayıcı grubunu yeni bir DNS ad etiketi, *myapplication*ile güncelleştirin ve kalan özellikleri değişmeden bırakın:

```azurecli-interactive
# Update DNS name label (restarts container), leave other properties unchanged
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication
```

## <a name="update-benefits"></a>Avantajları güncelleştirin

Varolan bir kapsayıcı grubunu güncelleştirmenin birincil avantajı daha hızlı dağıtımdır. Varolan bir kapsayıcı grubunu yeniden dağıttığınızda, kapsayıcı görüntü katmanları önceki dağıtım tarafından önbelleğe alınanlardan çıkarılır. Yeni dağıtımlarla yapılan tüm görüntü katmanlarını kayıt defterinden yeni çekmek yerine, yalnızca değiştirilmiş katmanlar (varsa) çekilir.

Windows Server Core gibi daha büyük kapsayıcı görüntülerine dayanan uygulamalar, yenisini silmek ve dağıtmak yerine güncelleştirdiğinizde dağıtım hızında önemli bir iyileşme görebilir.

## <a name="limitations"></a>Sınırlamalar

Kapsayıcı grubunun tüm özellikleri güncelleştirmeleri desteklemez. Bir kapsayıcı grubunun bazı özelliklerini değiştirmek için önce silmeniz, sonra grubu yeniden dağıtmanız gerekir. Ayrıntılar için, [kapsayıcı silme gerektiren Özellikler'e](#properties-that-require-container-delete)bakın.

Kapsayıcı grubunu güncellediğinizde kapsayıcı grubundaki tüm kapsayıcılar yeniden başlatılır. Çok kapsayıcılı bir grupta belirli bir kapsayıcının güncelleştirmesini veya yerinde yeniden başlatılmasını gerçekleştiremezsiniz.

Bir kapsayıcının IP adresi genellikle güncelleştirmeler arasında değişmez, ancak aynı kalacağı garanti edilmez. Kapsayıcı grubu aynı alttaki ana bilgisayara dağıtıldığı sürece, kapsayıcı grubu IP adresini korur. Nadir olsa da ve Azure Kapsayıcı Örnekleri aynı ana bilgisayara yeniden dağıtmak için her türlü çabayı gösterse de, farklı bir ana bilgisayara yeniden dağıtıma neden olabilecek bazı Azure dahili olaylar vardır. Bu sorunu azaltmak için kapsayıcı örnekleriniz için her zaman bir DNS ad etiketi kullanın.

Sonlandırılan veya silinen kapsayıcı grupları güncelleştirici olamaz. Bir kapsayıcı grubu durdurulduktan *(Sonlandırılmış* durumda) veya silindikten sonra, grup yeni olarak dağıtılır.

## <a name="properties-that-require-container-delete"></a>Kapsayıcı silme gerektiren özellikler

Daha önce de belirtildiği gibi, tüm kapsayıcı grubu özellikleri güncelleştirilebilir. Örneğin, bağlantı noktalarını değiştirmek veya bir kapsayıcının ilkesini yeniden başlatmak için önce kapsayıcı grubunu silmeniz, sonra yeniden oluşturmanız gerekir.

Bu özellikler, yeniden dağıtımdan önce kapsayıcı grubu silme gerektirir:

* İşletim sistemi türü
* CPU
* Bellek
* Yeniden başlatma ilkesi
* Bağlantı Noktaları

Bir kapsayıcı grubunu silip yeniden oluşturduğunuzda, "yeniden dağıtılmama" değil, yeni bir oluşturma dır. Tüm görüntü katmanları, önceki bir dağıtım tarafından önbelleğe alınanlardan değil, kayıt defterinden yeni çıkarılır. Kapsayıcının IP adresi, altta yatan farklı bir ana bilgisayara dağıtılması nedeniyle de değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede birkaç kez bahsedilen **konteyner grubudur.** Azure Kapsayıcı Örnekleri'ndeki her kapsayıcı bir kapsayıcı grubunda dağıtılır ve kapsayıcı grupları birden fazla kapsayıcı içerebilir.

[Azure Container Instances’taki kapsayıcı grupları](container-instances-container-groups.md)

[Birden çok kapsayıcılı grup dağıtma](container-instances-multi-container-group.md)

[Azure Kapsayıcı Örnekleri'nde kapsayıcıları el ile durdurma veya başlatma](container-instances-stop-start.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[azure-cli-install]: /cli/azure/install-azure-cli
