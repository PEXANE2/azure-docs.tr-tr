---
title: Gizli hacmi konteyner grubuna monte etme
description: Konteyner örneklerinize erişmek için hassas bilgileri depolamak için gizli bir birimi nasıl monte edebilirsiniz öğrenin
ms.topic: article
ms.date: 04/03/2020
ms.openlocfilehash: 756828e71174246450245938595c8872afc62961
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657146"
---
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>Azure Kapsayıcı Örneklerinde gizli bir ses birimi takma

Bir kapsayıcı grubundaki kapsayıcılara hassas bilgiler sağlamak için *gizli* bir birim kullanın. *Gizli* birim, sırlarınızı kapsayıcı grubundaki kapsayıcılar tarafından erişilebilen birim içindeki dosyalarda saklar. Sırları *gizli* bir birimde depolayarak, uygulama kodunuza SSH anahtarları veya veritabanı kimlik bilgileri gibi hassas veriler eklemekten kaçınabilirsiniz.

* Bir konteyner grubunda sırlar ile dağıtıldıktan sonra, gizli bir birim *salt okunur.*
* Tüm gizli birimler [tmpfs][tmpfs]tarafından desteklenen , RAM destekli dosya sistemi; içerikleri hiçbir zaman geçici olmayan depolamaya yazılmaz.

> [!NOTE]
> *Gizli* birimler şu anda Linux kapsayıcıları ile sınırlıdır. [Set ortamı değişkenlerinde](container-instances-environment-variables.md)hem Windows hem de Linux kapsayıcıları için güvenli ortam değişkenlerini nasıl geçeceğimi öğrenin. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışırken, güncel platform farklılıklarını [genel bakışta](container-instances-overview.md#linux-and-windows-containers)bulabilirsiniz.

## <a name="mount-secret-volume---azure-cli"></a>Gizli ses ekibini aşma - Azure CLI

Azure CLI'yi kullanarak bir veya daha fazla sır `--secrets` `--secrets-mount-path` içeren bir kapsayıcıdağıtmak [için, az kapsayıcı oluşturma][az-container-create] komutuna ve parametrelere ekleyin. Bu örnek, *secret* "mysecret1" ve "mysecret2" sırlarını içeren iki dosyadan `/mnt/secrets`oluşan gizli bir ses ekidir:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name secret-volume-demo \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --secrets mysecret1="My first secret FOO" mysecret2="My second secret BAR" \
    --secrets-mount-path /mnt/secrets
```

Aşağıdaki [az kapsayıcı exec][az-container-exec] çıkışı, çalışan kapsayıcıda bir kabuk açarak, gizli birim içindeki dosyaları listeleyerek ve içeriğini gösterir:

```azurecli
az container exec \
  --resource-group myResourceGroup \
  --name secret-volume-demo --exec-command "/bin/sh"
```

```output
/usr/src/app # ls /mnt/secrets
mysecret1
mysecret2
/usr/src/app # cat /mnt/secrets/mysecret1
My first secret FOO
/usr/src/app # cat /mnt/secrets/mysecret2
My second secret BAR
/usr/src/app # exit
Bye.
```

## <a name="mount-secret-volume---yaml"></a>Montaj gizli hacim - YAML

Ayrıca Azure CLI ve [YAML şablonu](container-instances-multi-container-yaml.md)ile kapsayıcı grupları dağıtabilirsiniz. YAML şablonu tarafından dağıtılama, birden çok kapsayıcıdan oluşan kapsayıcı gruplarını dağıtırken tercih edilen yöntemdir.

Bir YAML şablonuyla dağıtırken, gizli değerler şablonda **Base64 kodlanmış** olmalıdır. Ancak, gizli değerler kapsayıcıdaki dosyalar içinde düz metin olarak görünür.

Aşağıdaki YAML şablonu, *gizli* bir birimde `/mnt/secrets`bir kapsayıcıya sahip bir kapsayıcı grubunu tanımlar. Gizli cildin sırlarını içeren iki dosyası var, "mysecret1" ve "mysecret2."

```yaml
apiVersion: '2018-10-01'
location: eastus
name: secret-volume-demo
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /mnt/secrets
        name: secretvolume1
  osType: Linux
  restartPolicy: Always
  volumes:
  - name: secretvolume1
    secret:
      mysecret1: TXkgZmlyc3Qgc2VjcmV0IEZPTwo=
      mysecret2: TXkgc2Vjb25kIHNlY3JldCBCQVIK
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

YAML şablonuyla dağıtmak için, önceki YAML'yi `deploy-aci.yaml`adlı bir dosyaya kaydedin , ardından [az kapsayıcı oluşturma][az-container-create] komutunu `--file` parametreyle çalıştırın:

```azurecli-interactive
# Deploy with YAML template
az container create \
  --resource-group myResourceGroup \
  --file deploy-aci.yaml
```

## <a name="mount-secret-volume---resource-manager"></a>Gizli hacim montajı - Kaynak Yöneticisi

CLI ve YAML dağıtımına ek olarak, Azure [Kaynak Yöneticisi şablonu](/azure/templates/microsoft.containerinstance/containergroups)kullanarak bir kapsayıcı grubu dağıtabilirsiniz.

İlk olarak, `volumes` diziyi şablonun `properties` kapsayıcı grubu bölümünde doldurun. Kaynak Yöneticisi şablonuyla dağıtırken, gizli değerler şablonda **Base64 kodlanmış** olmalıdır. Ancak, gizli değerler kapsayıcıdaki dosyalar içinde düz metin olarak görünür.

Ardından, *gizli* birimi monte etmek istediğiniz kapsayıcı grubundaki her kapsayıcı için, `volumeMounts` diziyi `properties` kapsayıcı tanımının bölümünde doldurun.

Aşağıdaki Kaynak Yöneticisi şablonu, gizli bir birimde `/mnt/secrets` *bir kapsayıcıya* sahip bir kapsayıcı grubunu tanımlar. Gizli cildin iki sırrı var, "mysecret1" ve "mysecret2."

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-secret.json -->
[!code-json[volume-secret](~/azure-docs-json-samples/container-instances/aci-deploy-volume-secret.json)]

Kaynak Yöneticisi şablonuyla dağıtmak için, önceki JSON'u adlı `deploy-aci.json`bir dosyaya kaydedin , ardından az dağıtım grubu [oluşturma][az-deployment-group-create] komutunu `--template-file` parametreyle çalıştırın:

```azurecli-interactive
# Deploy with Resource Manager template
az deployment group create \
  --resource-group myResourceGroup \
  --template-file deploy-aci.json
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="volumes"></a>Birimler

Azure Kapsayıcı Örnekleri'nde diğer birim türlerini nasıl monte edebilirsiniz öğrenin:

* [Azure Container Instances'ta Azure dosya paylaşımı bağlama](container-instances-volume-azure-files.md)
* [Azure Kapsayıcı Örneklerinde boşdir hacmini takma](container-instances-volume-emptydir.md)
* [Azure Kapsayıcı Örneklerinde gitRepo hacmini takma](container-instances-volume-gitrepo.md)

### <a name="secure-environment-variables"></a>Güvenli ortam değişkenleri

Kapsayıcılara (Windows kapsayıcıları dahil) hassas bilgiler sağlamanın bir diğer yöntemi de [güvenli ortam değişkenlerinin](container-instances-environment-variables.md#secure-values)kullanılmasıdır.

<!-- LINKS - External -->
[tmpfs]: https://wikipedia.org/wiki/Tmpfs

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-deployment-group-create]: /cli/azure/deployment/group#az-deployment-group-create
