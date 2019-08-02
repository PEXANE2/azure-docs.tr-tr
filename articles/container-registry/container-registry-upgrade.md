---
title: Klasik Azure Container Registry 'yi yükseltme
description: Yönetilmeyen klasik kapsayıcı kayıt defterinizi yükselterek temel, standart ve Premium yönetilen kapsayıcı kayıt defterlerinden oluşan genişletilmiş özellik kümesinden yararlanın.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 03/26/2019
ms.author: danlep
ms.openlocfilehash: 05c227e7de078c6bb371049f16e191598b9ca4e5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68310371"
---
# <a name="upgrade-a-classic-container-registry"></a>Klasik bir kapsayıcı kayıt defterini yükseltme

Azure Container Registry (ACR), [SKU 'lar olarak bilinen](container-registry-skus.md)çeşitli hizmet katmanlarında kullanılabilir. ACR 'nin ilk sürümü, temel, standart ve Premium SKU 'Larına ( *yönetilen* kayıt defterleri olarak bilinir) ait çeşitli özellikler bulunmayan, klasik tek BIR SKU 'su sunuluyor.

Klasik SKU kullanım dışı bırakılmıştır ve 2019 Nisan 'dan sonra kullanılamayacaktır. Bu makalede, gelişmiş özellik kümesinden yararlanabilmeniz için yönetilmeyen klasik kayıt defterinizi yönetilen SKU 'Lardan birine geçirme hakkında bilgi sağlanır.

## <a name="why-upgrade"></a>Neden yükseltirsiniz?

Klasik kayıt defteri SKU 'SU **kullanımdan kaldırılmıştır**ve **2019 Nisan**'dan sonra kullanılamayacaktır. Mevcut tüm klasik kayıt defterleri 2019 Nisan 'dan önce yükseltilmelidir. Klasik kayıt defterlerinin Portal yönetim özellikleri aşamalı olarak açılır. Yeni klasik kayıt defterlerinin oluşturulması 2019 Nisan 'dan sonra devre dışı bırakılacak.

Klasik yönetilmeyen kayıt defterlerinin planlı kullanımdan kaldırılması ve sınırlı özellikleri nedeniyle, tüm klasik kayıt defterleri yönetilen kayıt defterlerine (temel, standart veya Premium) yükseltilmelidir. Bu üst düzey SKU 'Lar, kayıt defterini Azure 'un özelliklerine daha ayrıntılı bir şekilde tümleşir. Farklı hizmet katmanlarındaki fiyatlandırma ve yetenekler hakkında daha fazla bilgi için bkz. [Container Registry SKU 'lar](container-registry-skus.md).

Klasik kayıt defteri, kayıt defterini oluştururken Azure 'un Azure aboneliğinizde otomatik olarak sağlamasını yapan depolama hesabına bağlıdır. Buna karşılık, temel, standart ve Premium SKU 'Lar sizin için görüntülerinizi depolamayı şeffaf bir şekilde işleyerek Azure 'un [Gelişmiş depolama özelliklerinden](container-registry-storage.md) yararlanır. Kendi aboneliğinizde ayrı bir depolama hesabı oluşturulmaz.

Yönetilen kayıt defteri depolaması aşağıdaki avantajları sağlar:

* Kapsayıcı görüntüleri, [bekleyen olarak şifrelenir](container-registry-storage.md#encryption-at-rest).
* Görüntüler, çok bölgeli çoğaltma ile görüntülerinizin [coğrafi olarak yedekli depolama alanı](container-registry-storage.md#geo-redundant-storage)kullanılarak depolanır (yalnızca Premium SKU).
* Daha yüksek düzeyde bir SKU seçtiğinizde daha yüksek aktarım hızı sağlayan [SKU 'lar arasında serbestçe geçiş](container-registry-skus.md#changing-skus)yapabilme özelliği. Her SKU ile, ihtiyaçlarınız artdıkça ACR verimlilik gereksinimlerinizi karşılayabilir.
* Kayıt defteri ve depolama alanı için Birleşik güvenlik modeli, Basitleştirilmiş hak yönetimi sağlar. İzinleri ayrı bir depolama hesabı için de yönetmek zorunda kalmadan yalnızca kapsayıcı kayıt defteri için yönetirsiniz.

ACR 'de görüntü depolama hakkında daha fazla bilgi için bkz. [Azure Container Registry Container Image Storage](container-registry-storage.md).

## <a name="migration-considerations"></a>Geçiş sırasında dikkat edilmesi gerekenler

Klasik bir kayıt defterini yönetilen bir kayıt defterine yükselttiğinizde Azure, aboneliğinizdeki ACR tarafından oluşturulan depolama hesabındaki tüm mevcut kapsayıcı görüntülerini Azure tarafından yönetilen bir depolama hesabına kopyalamanız gerekir. Kayıt defterinizin boyutuna bağlı olarak, bu işlem birkaç dakika sürebilir. Tahmin amacıyla, yaklaşık 0,5 GiB bir dakika boyunca geçiş süresi bekleniyor.

Dönüştürme işlemi sırasında, geçiş `docker push` işleminin son% 10 ' u sırasında işlemler devre dışı bırakılır. `docker pull`normal olarak çalışmaya devam eder.

Dönüştürme işlemi sırasında klasik kayıt defterinizi yedekleyen depolama hesabının içeriğini silmeyin veya değiştirmeyin. Bunun yapılması kapsayıcı görüntülerinin bozulmasına yol açabilir.

Geçiş işlemi tamamlandıktan sonra, aboneliğinizdeki depolama hesabı, klasik kayıt defterinizi ilk kez kullanan Azure Container Registry tarafından artık kullanılmamaktadır. Geçişin başarılı olduğunu doğruladıktan sonra, maliyeti en aza indirmeye yardımcı olmak için depolama hesabını silmeyi göz önünde bulundurun.

>[!IMPORTANT]
> Klasik 'ten yönetilen SKU 'Lardan birine yükseltme **tek yönlü bir işlemdir**. Klasik bir kayıt defterini temel, standart veya Premium 'a dönüştürdükten sonra, klasik öğesine döndüremezsiniz. Ancak, kayıt defteriniz için yeterli kapasiteye sahip yönetilen SKU 'Lar arasında serbestçe geçiş yapabilirsiniz.

## <a name="how-to-upgrade"></a>Yükseltme

Yönetilmeyen klasik bir kayıt defterini yönetilen SKU 'Lardan birine çeşitli yollarla yükseltebilirsiniz. Aşağıdaki bölümlerde, [Azure CLI][azure-cli] ve [Azure Portal][azure-portal]kullanma sürecini açıklıyoruz.

## <a name="upgrade-in-azure-cli"></a>Azure CLı 'da yükseltme

Azure CLı 'de klasik bir kayıt defterini yükseltmek için [az ACR Update][az-acr-update] komutunu yürütün ve kayıt defteri için yeni SKU 'yu belirtin. Aşağıdaki örnekte, *myclassicregistry* adlı bir klasik kayıt DEFTERI Premium SKU 'ya yükseltilir:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

Geçiş tamamlandığında aşağıdakine benzer bir çıktı görmeniz gerekir. Azure 'un artık bu kayıt defteri için görüntü depolamayı `storageAccount` yönettiğini `null`belirten "Premium" ve olduğunu unutmayın. `sku`

```JSON
{
  "adminUserEnabled": false,
  "creationDate": "2017-12-12T21:23:29.300547+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Maksimum kapasitesi klasik kayıt defterinizin boyutundan daha az olan bir yönetilen kayıt defteri SKU 'SU belirtirseniz aşağıdakine benzer bir hata iletisi alırsınız.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

Benzer bir hata alırsanız, [az ACR Update][az-acr-update] komutunu yeniden çalıştırın ve görüntülerinizi barındırabilecek bir sonraki en yüksek düzey SKU 'su olan önerilen SKU 'yu belirtin.

## <a name="upgrade-in-azure-portal"></a>Azure portal yükseltme

Klasik bir kayıt defterini Azure portal kullanarak yükselttiğinizde, Azure hangi SKU 'nuzu barındırabileceği bağlı olarak standart veya Premium SKU 'YU otomatik olarak seçer. Örneğin, kayıt defteriniz görüntülerde 100 GiB 'den az bir değer içeriyorsa, Azure otomatik olarak klasik kayıt defterini standart (100 GiB) olarak seçer ve dönüştürür.

Azure portal kullanarak klasik kayıt defterinizi yükseltmek için, kapsayıcı kayıt defterine **Genel Bakış ' a** gidin ve **yönetilen kayıt defterine Yükselt**' i seçin.

![Azure portal Kullanıcı arabirimindeki klasik kayıt defteri yükseltme düğmesi][update-classic-01-upgrade]

Yönetilen bir kayıt defterine yükseltmek istediğinizi onaylamak için **Tamam ' ı** seçin.

Geçiş sırasında Portal, kayıt defterinin **sağlama durumunun** güncelleştirildiğini *gösterir.* Daha önce belirtildiği gibi `docker push` , işlemler geçişin son% 10 ' u sırasında devre dışıdır. Geçiş sürerken klasik kayıt defteri tarafından kullanılan depolama hesabını silmeyin veya güncelleştirmeyin; bunun yapılması görüntünün bozulmasına neden olabilir.

![Azure portal Kullanıcı arabiriminde klasik kayıt defteri yükseltme ilerlemesi][update-classic-03-updating]

Geçiş tamamlandığında, **sağlama durumu** *başarılı*olarak gösterilir ve kayıt defteriyle normal işlemleri sürdürebilirsiniz.

![Azure portal Kullanıcı arabiriminde klasik kayıt defteri yükseltme tamamlanma durumu][update-classic-04-updated]

## <a name="next-steps"></a>Sonraki adımlar

Klasik bir kayıt defterini yönetilen bir kayıt defterine yükselttikten sonra, Azure artık klasik kayıt defterini çalıştıran depolama hesabını kullanmaz. Maliyeti azaltmak için, eski kapsayıcı görüntülerinizi içeren hesap içindeki depolama hesabını veya blob kapsayıcısını silmeyi göz önünde bulundurun.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade/update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade/update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade/update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade/update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com