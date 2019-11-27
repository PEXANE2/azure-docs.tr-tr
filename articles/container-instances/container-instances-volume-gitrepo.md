---
title: GitRepo birimini kapsayıcı grubuna bağlama
description: Bir Git deposunu kapsayıcı örneklerinizi kopyalamak için gitRepo birimi bağlama hakkında bilgi edinin
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 708fca185227292e7cdf33952bde6f42b3d4951f
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533228"
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Azure Container Instances bir gitRepo birimi bağlama

Bir Git deposunu kapsayıcı örneklerinizi kopyalamak için bir *gitRepo* birimi bağlama hakkında bilgi edinin.

> [!NOTE]
> *GitRepo* birimi bağlamak Şu anda Linux kapsayıcılarıyla kısıtlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalıştık, ancak geçerli platform farklarını [genel bakışta](container-instances-overview.md#linux-and-windows-containers)bulabilirsiniz.

## <a name="gitrepo-volume"></a>gitRepo birimi

*GitRepo* birimi bir dizini takar ve belirtilen Git deposunu kapsayıcı başlangıcında buna kopyalar. Container Instances içinde bir *gitRepo* birimi kullanarak, uygulamalarınızda bunu gerçekleştirmek için kod eklemekten kaçınabilirsiniz.

Bir *gitRepo* birimi bağladığınızda, birimi yapılandırmak için üç özellik belirleyebilirsiniz:

| Özellik | Gerekli | Açıklama |
| -------- | -------- | ----------- |
| `repository` | Yes | Klonlanacak git deposunun `http://` veya `https://`dahil olmak üzere tam URL.|
| `directory` | Hayır | Deponun klonlanacak dizin. Yol, "`..`" ile içermemelidir veya başlamamalıdır.  "`.`" belirtirseniz, depo birimin dizinine kopyalanır. Aksi takdirde, git deposu birim dizini içindeki verilen adın alt dizinine kopyalanır. |
| `revision` | Hayır | Klonlanacak düzeltmenin COMMIT karması. Belirtilmemişse, `HEAD` düzeltmesi klonlanır. |

## <a name="mount-gitrepo-volume-azure-cli"></a>GitRepo Volume bağlama: Azure CLı

[Azure CLI](/cli/azure)ile kapsayıcı örnekleri dağıtırken bir gitRepo birimi bağlamak için, [az container Create][az-container-create] komutuna `--gitrepo-url` ve `--gitrepo-mount-path` parametrelerini girin. İsteğe bağlı olarak, kopyalamak için birim içindeki dizini (`--gitrepo-dir`) ve kopyalanacak düzeltmenin (`--gitrepo-revision`) COMMIT karmasını belirtebilirsiniz.

Bu örnek komut, Microsoft [aci-HelloWorld][aci-helloworld] örnek uygulamasını kapsayıcı örneğindeki `/mnt/aci-helloworld` kopyalar:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name hellogitrepo \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --dns-name-label aci-demo \
    --ports 80 \
    --gitrepo-url https://github.com/Azure-Samples/aci-helloworld \
    --gitrepo-mount-path /mnt/aci-helloworld
```

GitRepo biriminin takıldığını doğrulamak için [az Container exec][az-container-exec] ile kapsayıcıda bir Shell başlatın ve dizini listeleyin:

```console
$ az container exec --resource-group myResourceGroup --name hellogitrepo --exec-command /bin/sh
/usr/src/app # ls -l /mnt/aci-helloworld/
total 16
-rw-r--r--    1 root     root           144 Apr 16 16:35 Dockerfile
-rw-r--r--    1 root     root          1162 Apr 16 16:35 LICENSE
-rw-r--r--    1 root     root          1237 Apr 16 16:35 README.md
drwxr-xr-x    2 root     root          4096 Apr 16 16:35 app
```

## <a name="mount-gitrepo-volume-resource-manager"></a>Bağlama gitRepo birimi: Kaynak Yöneticisi

Bir [Azure Resource Manager şablonuyla](/azure/templates/microsoft.containerinstance/containergroups)kapsayıcı örnekleri dağıtırken bir gitRepo birimi bağlamak için önce şablonun kapsayıcı grubu `properties` bölümünde `volumes` diziyi doldurun. Ardından, *gitRepo* birimini bağlamak istediğiniz kapsayıcı grubundaki her bir kapsayıcı için, kapsayıcı tanımının `properties` bölümündeki `volumeMounts` dizisini doldurun.

Örneğin, aşağıdaki Kaynak Yöneticisi şablonu tek bir kapsayıcıdan oluşan bir kapsayıcı grubu oluşturur. Kapsayıcı, *gitRepo* birim blokları tarafından belirtilen iki GitHub deposu klonlar. İkinci birim, kopyalamak üzere bir dizin belirten ek özellikler ve kopyalamak için belirli bir düzeltmenin COMMIT karmasını içerir.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-gitrepo.json -->
[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

Önceki şablonda tanımlanan iki kopyalanmış deponun elde edilen dizin yapısı:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Azure Resource Manager şablonuyla kapsayıcı örneği dağıtımına bir örnek görmek için bkz. [Azure Container Instances birden çok Kapsayıcılı grupları dağıtma](container-instances-multi-container-group.md).

## <a name="private-git-repo-authentication"></a>Özel Git deposu kimlik doğrulaması

Özel bir git deposu için bir gitRepo birimi bağlamak üzere depo URL 'sinde kimlik bilgilerini belirtin. Genellikle, kimlik bilgileri, depoya kapsamlı erişim veren bir Kullanıcı adı ve kişisel erişim belirteci (PAT) biçimindedir.

Örneğin, özel bir GitHub deposu için Azure CLı `--gitrepo-url` parametresi aşağıdakine benzer görünür (burada "gituser" GitHub kullanıcı adıdır ve "abcdef1234fdsa4321abcdef" kullanıcının kişisel erişim belirtecidir):

```azurecli
--gitrepo-url https://gituser:abcdef1234fdsa4321abcdef@github.com/GitUser/some-private-repository
```

Azure Repos git deposu için, herhangi bir Kullanıcı adı belirtin (aşağıdaki örnekte olduğu gibi "azurereposuser" öğesini) geçerli bir PAT ile birlikte kullanabilirsiniz:

```azurecli
--gitrepo-url https://azurereposuser:abcdef1234fdsa4321abcdef@dev.azure.com/your-org/_git/some-private-repository
```

GitHub ve Azure Repos için kişisel erişim belirteçleri hakkında daha fazla bilgi için aşağıdakilere bakın:

GitHub: [komut satırı için bir kişisel erişim belirteci oluşturma][pat-github]

Azure Repos: [erişim kimliğini doğrulamak için kişisel erişim belirteçleri oluşturun][pat-repos]

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Instances diğer birim türlerini nasıl bağlayacağınızı öğrenin:

* [Azure Container Instances'ta Azure dosya paylaşımı bağlama](container-instances-volume-azure-files.md)
* [Azure Container Instances bir emptyDir birimi bağlama](container-instances-volume-emptydir.md)
* [Azure Container Instances bir gizli birimi bağlama](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-helloworld]: https://github.com/Azure-Samples/aci-helloworld
[pat-github]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[pat-repos]: https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
