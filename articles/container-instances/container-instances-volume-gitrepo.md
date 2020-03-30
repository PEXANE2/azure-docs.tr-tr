---
title: GitRepo hacmini konteyner grubuna monte etme
description: Bir Git deposunu konteyner örneklerinize klonlamak için bir gitRepo hacmini nasıl monte edeceğiz öğrenin
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 405cacd7a1649f95640a8dabf476729e101d03f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252095"
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Azure Kapsayıcı Örneklerinde gitRepo hacmini takma

Bir Git deposunu konteyner örneklerinize klonlamak için *bir gitRepo* hacmini nasıl monte edeceğiz öğrenin.

> [!NOTE]
> Bir *gitRepo* hacmi montaj şu anda Linux kapsayıcıları ile sınırlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışırken, güncel platform farklılıklarını [genel bakışta](container-instances-overview.md#linux-and-windows-containers)bulabilirsiniz.

## <a name="gitrepo-volume"></a>gitRepo hacmi

*GitRepo* hacmi bir dizin bağlar ve konteyner başlatma da içine belirtilen Git deposu klonlar. Kapsayıcı örneklerinizde bir *gitRepo* birimi kullanarak, uygulamalarınızda bunu yapmak için kod eklemekten kaçınabilirsiniz.

Bir *gitRepo* birimi monte ettiğinizde, birimi yapılandırmak için üç özellik ayarlayabilirsiniz:

| Özellik | Gerekli | Açıklama |
| -------- | -------- | ----------- |
| `repository` | Evet | Klonlanacak Git deposu `http://` `https://`nun dahil olduğu veya dahil olmak üzere tam URL'si.|
| `directory` | Hayır | Deponun klonlandığı dizin. Yol içermemeli veya "`..`" ile başlamamalıdır.  "`.`" yazarsanız, depo birimin dizinine klonlanır. Aksi takdirde, Git deposu ses dizini içinde verilen adın bir alt dizine klonlanır. |
| `revision` | Hayır | Revizyonun taahhüt hash klonlanacak. Belirtilmemişse, `HEAD` düzeltme klonlanır. |

## <a name="mount-gitrepo-volume-azure-cli"></a>GitRepo dağı hacmi: Azure CLI

[Azure CLI](/cli/azure)ile kapsayıcı örneklerini dağıttığınızda gitRepo birimi `--gitrepo-url` takmak için az kapsayıcı `--gitrepo-mount-path` [oluşturma][az-container-create] komutuna ve parametrelerine bilgi ver. İsteğe bağlı olarak, klonlanacak birim içindeki dizini (`--gitrepo-dir`) ve klonlanacak düzeltmenin`--gitrepo-revision`karmasını ( ) belirtebilirsiniz.

Bu örnek komut, Microsoft [aci-helloworld][aci-helloworld] `/mnt/aci-helloworld` örnek uygulamasını kapsayıcı örneğinde klonlar:

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

GitRepo hacminin monte edildiğini doğrulamak için, [az konteyner exec][az-container-exec] ile konteyner bir kabuk başlatmak ve dizini listeleyin:

```azurecli
az container exec --resource-group myResourceGroup --name hellogitrepo --exec-command /bin/sh
```

```output
/usr/src/app # ls -l /mnt/aci-helloworld/
total 16
-rw-r--r--    1 root     root           144 Apr 16 16:35 Dockerfile
-rw-r--r--    1 root     root          1162 Apr 16 16:35 LICENSE
-rw-r--r--    1 root     root          1237 Apr 16 16:35 README.md
drwxr-xr-x    2 root     root          4096 Apr 16 16:35 app
```

## <a name="mount-gitrepo-volume-resource-manager"></a>Mount gitRepo hacmi: Kaynak Yöneticisi

[Azure Kaynak Yöneticisi şablonuyla](/azure/templates/microsoft.containerinstance/containergroups)kapsayıcı örneklerini dağıttığınızda gitRepo birimi `volumes` takmak için, `properties` önce diziyi şablonun kapsayıcı grubu bölümünde doldurun. Daha sonra, *gitRepo* hacmini monte etmek istediğiniz konteyner grubundaki her kapsayıcı `volumeMounts` için, `properties` diziyi konteyner tanımının bölümünde doldurun.

Örneğin, aşağıdaki Kaynak Yöneticisi şablonu tek bir kapsayıcıdan oluşan bir kapsayıcı grubu oluşturur. Kapsayıcı, *gitRepo* birim blokları tarafından belirtilen iki GitHub deposunu klonlar. İkinci birim, klonlamak için bir dizin belirten ek özellikler ve kloniçin belirli bir düzeltme nin karma işlemeiçerir.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-gitrepo.json -->
[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

Önceki şablonda tanımlanan iki klonlanmış deponun ortaya çıkan dizin yapısı:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Azure Kaynak Yöneticisi şablonu yla kapsayıcı örneği dağıtımının bir örneğini görmek için, [Azure Kapsayıcı Örnekleri'nde çok kapsayıcılı grupları dağıt'a](container-instances-multi-container-group.md)bakın.

## <a name="private-git-repo-authentication"></a>Özel Git repo kimlik doğrulaması

Özel bir Git deposu için gitRepo birimi takmak için, depo URL'sinde kimlik bilgilerini belirtin. Genellikle, kimlik bilgileri bir kullanıcı adı ve depoya kapsamlı erişim sağlayan kişisel erişim belirteci (PAT) şeklindedir.

Örneğin, özel bir `--gitrepo-url` GitHub deposunun Azure CLI parametresi aşağıdakilere benzer ("gituser" GitHub kullanıcı adıdır ve "abcdef1234fdsa4321abcdef" kullanıcının kişisel erişim belirtecidir):

```console
--gitrepo-url https://gituser:abcdef1234fdsa4321abcdef@github.com/GitUser/some-private-repository
```

Azure Repos Git deposu için, geçerli bir PAT ile birlikte herhangi bir kullanıcı adı (aşağıdaki örnekte olduğu gibi "azurereposuser" kullanabilirsiniz) belirtin:

```console
--gitrepo-url https://azurereposuser:abcdef1234fdsa4321abcdef@dev.azure.com/your-org/_git/some-private-repository
```

GitHub ve Azure Repos'lar için kişisel erişim belirteçleri hakkında daha fazla bilgi için aşağıdakilere bakın:

GitHub: [Komut satırı için kişisel erişim jetonu oluşturma][pat-github]

Azure Repos: [Erişimin kimliğini doğrulamak için kişisel erişim belirteçleri oluşturun][pat-repos]

## <a name="next-steps"></a>Sonraki adımlar

Azure Kapsayıcı Örnekleri'nde diğer birim türlerini nasıl monte edebilirsiniz öğrenin:

* [Azure Container Instances'ta Azure dosya paylaşımı bağlama](container-instances-volume-azure-files.md)
* [Azure Kapsayıcı Örneklerinde boşdir hacmini takma](container-instances-volume-emptydir.md)
* [Azure Kapsayıcı Örneklerinde gizli bir ses birimi takma](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-helloworld]: https://github.com/Azure-Samples/aci-helloworld
[pat-github]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[pat-repos]: https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
