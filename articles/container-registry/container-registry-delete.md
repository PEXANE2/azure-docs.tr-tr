---
title: Görüntü kaynaklarını silme
description: Azure CLI komutlarını kullanarak kapsayıcı görüntü verilerini silerek kayıt defteri boyutunun nasıl etkin bir şekilde yönetileceklerine ilişkin ayrıntılar.
ms.topic: article
ms.date: 07/31/2019
ms.openlocfilehash: 449a1c09bf88e3e0e0aeca4d3b687371d2a6b91a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78403352"
---
# <a name="delete-container-images-in-azure-container-registry-using-the-azure-cli"></a>Azure CLI'yi kullanarak Azure Kapsayıcı Kayıt Defteri'ndeki kapsayıcı görüntülerini silme

Azure kapsayıcı kayıt defterinizin boyutunu korumak için, eski resim verilerini düzenli aralıklarla silmeniz gerekir. Üretime dağıtılan bazı kapsayıcı görüntüleri uzun süreli depolama gerektirebilir, ancak diğerleri genellikle daha hızlı silinebilir. Örneğin, otomatik bir yapı ve test senaryosunda, kayıt defteriniz hiç dağıtılmayan görüntülerle hızla doldurulabilir ve yapı ve test geçişini tamamladıktan kısa bir süre sonra temizlenebilir.

Resim verilerini birkaç farklı şekilde silebileceğinizden, her silme işleminin depolama kullanımını nasıl etkilediğini anlamak önemlidir. Bu makalede, görüntü verilerini silerken için çeşitli yöntemler yer alıyor:

* Bir [depoyu](#delete-repository)silme : Depoiçindeki tüm görüntüleri ve tüm benzersiz katmanları siler.
* [Etikete](#delete-by-tag)göre sil : Görüntüyü, etiketi, görüntünün referans ettiği tüm benzersiz katmanları ve görüntüyle ilişkili diğer tüm etiketleri siler.
* Manifest [sindirimi](#delete-by-manifest-digest)ile silme : Görüntüyü, görüntünün atıfta bulunduğu tüm benzersiz katmanları ve görüntüyle ilişkili tüm etiketleri siler.

Silme işlemlerini otomatikleştirmeye yardımcı olmak için örnek komut dosyaları sağlanır.

Bu kavramlara giriş yapmak [için, kayıt defterleri, depolar ve resimler hakkında](container-registry-concepts.md)bkz.

## <a name="delete-repository"></a>Deposilme

Bir deponun silinmesi, tüm etiketler, benzersiz katmanlar ve bildirimler dahil olmak üzere depodaki tüm görüntüleri siler. Bir depoyu sildiğinizde, bu depodaki benzersiz katmanlar atıfta bulunan görüntülerin kullandığı depolama alanını kurtarırsınız.

Aşağıdaki Azure CLI komutu "acr-helloworld" deposunu siler ve depoiçindeki tüm etiketleri ve bildirimleri gösterir. Silinen bildirimler tarafından başvurulan katmanlar kayıt defterindeki diğer resimlertarafından başvurulmuyorsa, katman verileri de silinir ve depolama alanı kurtarılır.

```azurecli
 az acr repository delete --name myregistry --repository acr-helloworld
```

## <a name="delete-by-tag"></a>Etikete göre silme

Silme işleminde depo adını ve etiketini belirterek tek tek görüntüleri bir depodan silebilirsiniz. Etiketle sildiğinizde, görüntüdeki benzersiz katmanlar tarafından kullanılan depolama alanını kurtarırsınız (kayıt defterindeki diğer resimler tarafından paylaşılmaz katmanlar).

Etikete göre silmek için [az acr deposunu kullanın][az-acr-repository-delete] ve `--image` parametredeki resim adını belirtin. Görüntüye özgü tüm katmanlar ve resimle ilişkili diğer etiketler silinir.

Örneğin, "acr-helloworld:latest" görüntüsünü kayıt defterinden "myregistry" olarak siler:

```azurecli
az acr repository delete --name myregistry --image acr-helloworld:latest
```

```output
This operation will delete the manifest 'sha256:0a2e01852872580b2c2fea9380ff8d7b637d3928783c55beb3f21a6e58d5d108' and all the following images: 'acr-helloworld:latest', 'acr-helloworld:v3'.
Are you sure you want to continue? (y/n):
```

> [!TIP]
> *Etikete göre* silme, bir etiketi silme (tagging'i çözme) ile karıştırılmamalıdır. Azure CLI komutu [az acr deposu untag][az-acr-repository-untag]ile etiketi silebilirsiniz. Bir resmin etiketini siz bıraktığınızda hiçbir alan serbest bırakılır, çünkü [onun manifestosu](container-registry-concepts.md#manifest) ve katman verileri kayıt defterinde kalır. Yalnızca etiket başvurusu kendisi silinir.

## <a name="delete-by-manifest-digest"></a>Manifest özete göre silme

[Bir bildirim özeti](container-registry-concepts.md#manifest-digest) bir, hiç veya birden çok etiketle ilişkilendirilebilir. Özetle sildiğinizde, bildirimtarafından başvurulan tüm etiketler ve görüntüye özgü katman verileri silinir. Paylaşılan katman verileri silinmez.

Özete göre silmek için, öncelikle silmek istediğiniz görüntüleri içeren depoiçin manifesto özetlerini listele. Örnek:

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
  }
]
```

Ardından, [az acr deposu silme][az-acr-repository-delete] komutunda silmek istediğiniz özeti belirtin. Komut biçimi şu şekildedir:

```azurecli
az acr repository delete --name <acrName> --image <repositoryName>@<digest>
```

Örneğin, önceki çıktıda listelenen son bildirimi silmek için ("v2" etiketiyle):

```azurecli
az acr repository delete --name myregistry --image acr-helloworld@sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57
```

```output
This operation will delete the manifest 'sha256:3168a21b98836dda7eb7a846b3d735286e09a32b0aa2401773da518e7eba3b57' and all the following images: 'acr-helloworld:v2'.
Are you sure you want to continue? (y/n): 
```

Görüntü, `acr-helloworld:v2` bu resme özgü katman verileri gibi kayıt defterinden silinir. Bir bildirim birden çok etiketle ilişkiliyse, ilişkili tüm etiketler de silinir.

## <a name="delete-digests-by-timestamp"></a>Zaman damgası ile özetleri silme

Bir depo nun veya kayıt defterinin boyutunu korumak için, belirli bir tarihten daha eski bildirim özetlerini düzenli aralıklarla silmeniz gerekebilir.

Aşağıdaki Azure CLI komutu, artan sırada, belirli bir zaman damgası üzerinde eski bir depoda tüm bildirim özetilisteler. `<acrName>` Değiştirin `<repositoryName>` ve ortamınıza uygun değerlerle değiştirin. Zaman damgası, bu örnekte olduğu gibi tam bir tarih-saat ifadesi veya bir tarih olabilir.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> \
--orderby time_asc -o tsv --query "[?timestamp < '2019-04-05'].[digest, timestamp]"
```

Eski özetözetlerini tanımladıktan sonra, belirtilen zaman damgası eski manifest özetlerini silmek için aşağıdaki Bash komut dosyasını çalıştırabilirsiniz. Bu Azure CLI ve **xargs**gerektirir. Varsayılan olarak, komut dosyası silme işlemi gerçekleştirmez. Görüntü `ENABLE_DELETE` silmeişlemini etkinleştirmek için `true` değeri değiştirin.

> [!WARNING]
> Aşağıdaki örnek komut dosyasını dikkatli kullanın-silinmiş görüntü verileri KURTARılamaz. Görüntüleri özete göre çeken sistemleriniz varsa (görüntü adının aksine), bu komut dosyalarını çalıştırmamalısınız. Manifesto özetlerini silerken, bu sistemlerin görüntüleri kayıt defterinizden çekmesini önler. Bunun yerine manifesto tarafından çekerek, *benzersiz* bir etiketleme düzeni, [önerilen en iyi uygulama](container-registry-image-tag-version.md)benimseyerek düşünün. 

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
# TIMESTAMP can be a date-time string such as 2019-03-15T17:55:00.
REGISTRY=myregistry
REPOSITORY=myrepository
TIMESTAMP=2019-04-05  

# Delete all images older than specified timestamp.

if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
    --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY \
   --orderby time_asc --query "[?timestamp < '$TIMESTAMP'].[digest, timestamp]" -o tsv
fi
```

## <a name="delete-untagged-images"></a>Etiketlenmemiş görüntüleri silme

[Manifest özeti](container-registry-concepts.md#manifest-digest) bölümünde belirtildiği gibi, varolan bir etiket kullanarak değiştirilmiş bir görüntüyü iterek daha önce itilen görüntüyü **etiketleri untags,** bir öksüz (veya "sarkan") görüntü ile sonuçlanan. Daha önce itilen görüntünün manifestosu ve katman verileri kayıt defterinde kalır. Aşağıdaki olaylar dizisini göz önünde bulundurun:

1. Push görüntü *acr-helloworld* etiketi ile **son**:`docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Depo *acr-helloworld*için kontrol manifestoları :

   ```azurecli
   az acr repository show-manifests --name myregistry --repository acr-helloworld
   
   ```
   
   ```output
   [
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

1. *Acr-helloworld* Dockerfile değiştirin
1. Push görüntü *acr-helloworld* etiketi ile **son**:`docker push myregistry.azurecr.io/acr-helloworld:latest`
1. Depo *acr-helloworld*için kontrol manifestoları :

   ```azurecli
   az acr repository show-manifests --name myregistry --repository acr-helloworld
   ```
   
   ```output
   [
     {
       "digest": "sha256:7ca0e0ae50c95155dbb0e380f37d7471e98d2232ed9e31eece9f9fb9078f2728",
       "tags": [
         "latest"
       ],
       "timestamp": "2018-07-11T21:38:35.9170967Z"
     },
     {
       "digest": "sha256:d2bdc0c22d78cde155f53b4092111d7e13fe28ebf87a945f94b19c248000ceec",
       "tags": [],
       "timestamp": "2018-07-11T21:32:21.1400513Z"
     }
   ]
   ```

Sıradaki son adımın çıktısında görebileceğiniz gibi, artık özelliği boş `"tags"` bir liste olan bir yetim bildirimi var. Bu bildirim, başvurur olduğu benzersiz katman verileriyle birlikte kayıt defterinde hala bulunur. **Bu tür yetim görüntüleri ve katman verilerini silmek için, özet egöre silmeniz gerekir.**

## <a name="delete-all-untagged-images"></a>Tüm etiketlenmemiş görüntüleri silme

Aşağıdaki Azure CLI komutunu kullanarak deponuzdaki tüm etiketsiz resimleri listeleyebilirsiniz. `<acrName>` Değiştirin `<repositoryName>` ve ortamınıza uygun değerlerle değiştirin.

```azurecli
az acr repository show-manifests --name <acrName> --repository <repositoryName> --query "[?tags[0]==null].digest"
```

Bu komut dosyasındaki bu komutu kullanarak, bir depodaki tüm etiketsiz görüntüleri silebilirsiniz.

> [!WARNING]
> Aşağıdaki örnek komut dosyalarını dikkatli kullanın-silinen görüntü verileri KURTARılamaz. Görüntüleri özete göre çeken sistemleriniz varsa (görüntü adının aksine), bu komut dosyalarını çalıştırmamalısınız. Etiketlenmemiş görüntülerin silmesi, bu sistemlerin resimleri kayıt defterinizden çekmesini önler. Bunun yerine manifesto tarafından çekerek, *benzersiz* bir etiketleme düzeni, [önerilen en iyi uygulama](container-registry-image-tag-version.md)benimseyerek düşünün.

**Azure CLI Bash içinde**

Aşağıdaki Bash komut dosyası, bir depodaki tüm etiketlenmemiş görüntüleri siler. Bu Azure CLI ve **xargs**gerektirir. Varsayılan olarak, komut dosyası silme işlemi gerçekleştirmez. Görüntü `ENABLE_DELETE` silmeişlemini etkinleştirmek için `true` değeri değiştirin.

```bash
#!/bin/bash

# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to 'true' to enable image delete
ENABLE_DELETE=false

# Modify for your environment
REGISTRY=myregistry
REPOSITORY=myrepository

# Delete all untagged (orphaned) images
if [ "$ENABLE_DELETE" = true ]
then
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY  --query "[?tags[0]==null].digest" -o tsv \
    | xargs -I% az acr repository delete --name $REGISTRY --image $REPOSITORY@% --yes
else
    echo "No data deleted."
    echo "Set ENABLE_DELETE=true to enable image deletion of these images in $REPOSITORY:"
    az acr repository show-manifests --name $REGISTRY --repository $REPOSITORY --query "[?tags[0]==null]" -o tsv
fi
```

**PowerShell'de Azure CLI**

Aşağıdaki PowerShell komut dosyası, bir depodaki tüm etiketlenmemiş görüntüleri siler. PowerShell ve Azure CLI gerektirir. Varsayılan olarak, komut dosyası silme işlemi gerçekleştirmez. Görüntü `$enableDelete` silmeişlemini etkinleştirmek için `$TRUE` değeri değiştirin.

```powershell
# WARNING! This script deletes data!
# Run only if you do not have systems
# that pull images via manifest digest.

# Change to '$TRUE' to enable image delete
$enableDelete = $FALSE

# Modify for your environment
$registry = "myregistry"
$repository = "myrepository"

if ($enableDelete) {
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null].digest" -o tsv `
    | %{ az acr repository delete --name $registry --image $repository@$_ --yes }
} else {
    Write-Host "No data deleted."
    Write-Host "Set `$enableDelete = `$TRUE to enable image deletion."
    az acr repository show-manifests --name $registry --repository $repository --query "[?tags[0]==null]" -o tsv
}
```


## <a name="automatically-purge-tags-and-manifests-preview"></a>Etiketleri ve bildirimleri otomatik olarak temizleme (önizleme)

Azure CLI komutlarını komut dosyası na alternatif olarak, belirli bir süreden eski tüm etiketleri silmek veya belirli bir ad filtresiyle eşleşmek için isteğe bağlı veya zamanlanmış ACR görevi çalıştırın. Daha fazla bilgi için [bkz.](container-registry-auto-purge.md)

İsteğe bağlı olarak, etiketlenmemiş bildirimleri yönetmek için her kayıt defteri için bir [bekletme ilkesi](container-registry-retention-policy.md) ayarlayın. Bir bekletme ilkesini etkinleştirdiğinizde, kayıt defterinde ilişkili etiketleri olmayan görüntü bildirimleri ve temel katman verileri, belirli bir süre sonra otomatik olarak silinir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Kapsayıcı Kayıt Defteri'nde görüntü depolama hakkında daha fazla bilgi için [Azure Kapsayıcı Kayıt Defteri'ndeki Kapsayıcı resim depolama](container-registry-storage.md)alanına bakın.

<!-- IMAGES -->
[manifest-digest]: ./media/container-registry-delete/01-manifest-digest.png

<!-- LINKS - External -->
[docker-manifest-inspect]: https://docs.docker.com/edge/engine/reference/commandline/manifest/#manifest-inspect
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-repository-untag]: /cli/azure/acr/repository#az-acr-repository-untag
