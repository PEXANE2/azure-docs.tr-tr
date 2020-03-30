---
title: Görüntüler & depolar hakkında
description: Azure kapsayıcı kayıt defterleri, depolar ve kapsayıcı resimlerinin temel kavramlarına giriş.
ms.topic: article
ms.date: 09/10/2019
ms.openlocfilehash: ea6e2577d3eee91626dd613617a0b79e4ff3d6a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247065"
---
# <a name="about-registries-repositories-and-images"></a>Kayıt defterleri, depolar ve görüntüler hakkında

Bu makalede, konteyner kayıt defterleri, depolar ve kapsayıcı görüntüleri ve ilgili yapılar temel kavramları tanıtışlar. 

## <a name="registry"></a>Kayıt Defteri

Konteyner *kayıt defteri,* kapsayıcı görüntülerini depolayan ve dağıtan bir hizmettir. Docker Hub, açık kaynak topluluğunu destekleyen ve genel bir resim kataloğu olarak hizmet veren genel bir konteyner kayıt defteridir. Azure Konteyner Kayıt Defteri, kullanıcılara, ağ kapatma dağıtımları, sanal ağ ve [güvenlik duvarı yapılandırması,](container-registry-vnet.md)etiket [kilitleme](container-registry-image-lock.md)ve diğer birçok gelişmiş özellik için küresel dağıtımı ve güvenilirliği destekleyen tümleşik kimlik doğrulama, [coğrafi çoğaltma](container-registry-geo-replication.md) ile görüntülerini doğrudan kontrol altına almalarını sağlar. 

Azure Kapsayıcı Kayıt Defteri, Docker kapsayıcı resimlerine ek olarak, Open Container Initiative (OCI) görüntü biçimleri de dahil olmak üzere ilgili [içerik yapılarını](container-registry-image-formats.md) destekler.

## <a name="content-addressable-elements-of-an-artifact"></a>Bir yapının içerik adreslenebilir öğeleri

Azure kapsayıcı kayıt defterindeki bir yapının adresi aşağıdaki öğeleri içerir. 

`[loginUrl]/[namespace]/[artifact:][tag]`

* **loginUrl** - kayıt sahibinin tam nitelikli adı. Azure kapsayıcı kayıt defterindeki kayıt sahibi, *myregistry*.azurecr.io (tümü küçük harf) biçimindedir. Yapılarını bir Azure kapsayıcı kayıt defterine çekmek veya itmek için Docker veya diğer istemci araçlarını kullanırken giriş Url'sini belirtmeniz gerekir. 
* **ad alanı** - İlgili görüntülerin veya yapıtların slash-sınırlı mantıksal gruplandırma - örneğin, bir çalışma grubu veya uygulama için
* **artefakt** - Belirli bir resim veya artifakı için bir deponun adı
* **etiket** - Bir depoda saklanan bir görüntünün veya yapının belirli bir sürümü


Örneğin, Azure kapsayıcı kayıt defterindeki bir resmin tam adı aşağıdaki gibi görünebilir:

*myregistry.azurecr.io/marketing/campaign10-18/email-sender:v2*

Bu öğeler hakkında ayrıntılı bilgi için aşağıdaki bölümlere bakın.

## <a name="repository-name"></a>Depo adı

Konteyner *kayıtları, depoları,* kapsayıcı görüntülerinin toplarını veya aynı ada sahip diğer yapıları yönetir, ancak farklı etiketleri yönetir. Örneğin, aşağıdaki üç resim "acr-helloworld" deposundadır:


- *acr-helloworld:en son*
- *acr-helloworld:v1*
- *acr-helloworld:v2*

Depo adları [ad boşluklarını](container-registry-best-practices.md#repository-namespaces)da içerebilir. Ad alanları, görüntüleri ileri eğik çizgi-sınırlandırılmış depo adlarını kullanarak gruplandırmanıza olanak sağlar, örneğin:

- *pazarlama/kampanya10-18/web:v2*
- *pazarlama/kampanya10-18/api:v3*
- *pazarlama/kampanya10-18/email-gönderen:v2*
- *ürün-iade/web-gönderim:20180604*
- *ürün-iade/miras entegratörü:20180715*

## <a name="image"></a>Görüntü

Bir kapsayıcı görüntüsü veya kayıt defteri içindeki diğer yapı, bir veya daha fazla etiketle ilişkilidir, bir veya daha fazla katmanı vardır ve bir manifesto tarafından tanımlanır. Bu bileşenlerin birbiriyle nasıl ilişkili olduğunu anlamak, kayıt defterinizi etkili bir şekilde yönetmenize yardımcı olabilir.

### <a name="tag"></a>Etiket

Bir görüntünün veya başka bir yapının *etiketi,* onun sürümünü belirtir. Depo içindeki tek bir yapıya bir veya birden çok etiket atanabilir ve "etiketlenmemiş" de olabilir. Diğer bir deyişle, görüntünün verileri (katmanları) kayıt defterinde kalırken, görüntüdeki tüm etiketleri silebilirsiniz.

Depo (veya depo ve ad alanı) artı bir etiket görüntünün adını tanımlar. İtme veya çekme işleminde adını belirterek görüntüyü itebilir ve çekebilirsiniz.

Kapsayıcı görüntülerini nasıl etiketlediğiniz, bunları geliştirmek veya dağıtmak için senaryolarınız tarafından yönlendirilir. Örneğin, temel resimlerinizi korumak için kararlı etiketler ve görüntüleri dağıtmak için benzersiz etiketler önerilir. Daha fazla bilgi [için, kapsayıcı görüntülerini etiketleme ve sürüm önerilerine](container-registry-image-tag-version.md)bakın.

### <a name="layer"></a>Katman

Kapsayıcı görüntüler, her biri Dockerfile'da görüntüyü tanımlayan bir satıra karşılık gelen bir veya daha fazla *katmandan*oluşur. Kayıt defterindeki görüntüler ortak katmanları paylaşarak depolama verimliliğini artırır. Örneğin, farklı depolarda bulunan birkaç resim aynı Alp Linux taban katmanını paylaşabilir, ancak bu katmanın yalnızca bir kopyası kayıt defterinde depolanır.

Katman paylaşımı, ortak katmanları paylaşan birden çok görüntüyle düğümlere katman dağıtımında da en iyi duruma getirilmiştir. Örneğin, düğüm üzerindeki bir görüntü taban olarak Alp Linux katmanını içeriyorsa, aynı katmana başvuran farklı bir görüntünün sonraki çekme leri katmanı düğüme aktarmaz. Bunun yerine, düğümüzerinde zaten var olan katmana başvurur.

Güvenli yalıtım ve olası katman işlemeye karşı koruma sağlamak için katmanlar kayıt defterleri arasında paylaşılmaz.

### <a name="manifest"></a>Bildirim

Bir kapsayıcı kayıt defterine itilen her kapsayıcı görüntüsü veya yapı bir *manifesto*ile ilişkilidir. Görüntü itildiğinde kayıt defteri tarafından oluşturulan bildirim, görüntüyü benzersiz bir şekilde tanımlar ve katmanlarını belirtir. Azure CLI komutu [az acr deposu gösteri manifestoları][az-acr-repository-show-manifests]ile bir depo için bildirimleri listeleyebilirsiniz:

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName>
```

Örneğin, "acr-helloworld" deposu için bildirimleri listele:

```azurecli
az acr repository show-manifests --name myregistry --repository acr-helloworld
```

```output
[
  {
    "digest": "sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108",
    "tags": [
      "latest",
      "v3"
    ],
    "timestamp": "2018-07-12T15:52:00.2075864Z"
  },
  {
    "digest": "sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57",
    "tags": [
      "v2"
    ],
    "timestamp": "2018-07-12T15:50:53.5372468Z"
  },
  {
    "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
    "tags": [
      "v1"
    ],
    "timestamp": "2018-07-11T21:38:35.9170967Z"
  }
]
```

### <a name="manifest-digest"></a>Manifest özet

Manifestolar benzersiz bir SHA-256 karma veya *manifest digest*tarafından tanımlanır. Etiketli olsun veya olmasın, her resim veya yapı özetiyle tanımlanır. Görüntünün katman verileri başka bir görüntüyle aynı olsa bile özet değeri benzersizdir. Bu mekanizma, aynı etiketlenmiş görüntüleri tekrar tekrar bir kayıt defterine itmenize olanak sağlar. Örneğin, her görüntü benzersiz `myimage:latest` özetiyle tanımlandığından, kayıt defterinize hatasız olarak tekrar tekrar itebilirsiniz.

Çekme işleminde özetini belirterek bir görüntüyü kayıt defterinden çekebilirsiniz. Bazı sistemler, aynı şekilde etiketlenmiş bir görüntü daha sonra kayıt defterine itilse bile, görüntü sürümünün çekilmesini garanti ettiği için sindirilebilir şekilde yapılandırılabilir.

Örneğin, manifesto özetine göre "acr-helloworld" deposundan bir görüntü çekin:

`docker pull myregistry.azurecr.io/acr-helloworld@sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108`

> [!IMPORTANT]
> Değiştirilen görüntüleri sürekli olarak aynı etiketlerle iterseniz, etiketlenmemiş, ancak kayıt defterinizde yine de yer tüketen, adsız resimler oluşturabilirsiniz. Etikete göre görüntüleri listelediğinizde veya görüntülediğinizde, Etiketlenmemiş resimler Azure CLI'de veya Azure portalında gösterilmez. Ancak, katmanları hala var ve kayıt defterinizde yer tüketir. Etiketlenmemiş bir görüntünün silmesi, bildirim yalnızca veya sonuncusu olduğunda kayıt defteri alanını boşaltArak belirli bir katmanı işaret eder. Etiketlenmemiş görüntüler tarafından kullanılan alanı boşaltma hakkında bilgi [için](container-registry-delete.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Kapsayıcı Kayıt Defteri'nde [görüntü depolama](container-registry-storage.md) ve desteklenen [içerik biçimleri](container-registry-image-formats.md) hakkında daha fazla bilgi edinin.

<!-- LINKS - Internal -->
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests


