---
title: Kapsayıcı örneğinde ortam değişkenlerini ayarlama
description: Azure Kapsayıcı Örnekleri'nde çalıştırdığınız kapsayıcılarda ortam değişkenlerini nasıl ayarlayabileceğinizi öğrenin
ms.topic: article
ms.date: 04/17/2019
ms.openlocfilehash: c3c76ba0c6131a8ab3de68c13c9dfddaf7e8749a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247234"
---
# <a name="set-environment-variables-in-container-instances"></a>Kapsayıcı örneklerinde ortam değişkenlerini ayarlama

Kapsayıcı örneklerinizde ortam değişkenleri ayarlamanız, kapsayıcı tarafından çalıştırılan uygulamanın veya betiğin dinamik yapılandırmasını sağlamanıza imkan tanır. Bu komut satırı `--env` bağımsız değişkenine `docker run`benzer. 

Bir kapsayıcıda ortam değişkenleri ayarlamak için, bir kapsayıcı örneği oluştururken bunları belirtin. Bu makalede, [Azure CLI](#azure-cli-example), [Azure PowerShell](#azure-powershell-example)ve [Azure portalı](#azure-portal-example)ile bir kapsayıcı başlattığınızda ortam değişkenleri ayarlama örnekleri gösterilmektedir. 

Örneğin, Microsoft [aci-wordcount][aci-wordcount] kapsayıcı görüntüsünü çalıştırıyorsanız, aşağıdaki ortam değişkenlerini belirterek davranışını değiştirebilirsiniz:

*NumWords*: STDOUT'a gönderilen kelime sayısı.

*MinLength*: Bir sözcükteki en az karakter sayısının sayılması. Daha yüksek bir sayı "of" ve "the" gibi yaygın sözcükleri yok sayar.

Çevre değişkenleri olarak sırları geçirmeniz gerekiyorsa, Azure Kapsayıcı Örnekleri hem Windows hem de Linux kapsayıcıları için [güvenli değerleri](#secure-values) destekler.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-cli-example"></a>Azure CLI örneği

[aci-wordcount][aci-wordcount] kapsayıcının varsayılan çıktısını görmek için, önce bu [az kapsayıcı oluşturma komutuyla][az-container-create] çalıştırın (ortam değişkenleri belirtilmedi):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure
```

Çıktıyı değiştirmek için, `--environment-variables` *NumWords* ve *MinLength* değişkenleri için değerleri belirterek eklenen bağımsız değişkenle ikinci bir kapsayıcı başlatın. (Bu örnek, CLI'yi Bir Bash kabuğunda veya Azure Bulut Su şuramasında çalıştırdığınızı varsayalım. Windows Komut İstemi'ni kullanıyorsanız, değişkenleri çift tırnak `--environment-variables "NumWords"="5" "MinLength"="8"`işaretleri yle belirtin, örneğin .)

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables 'NumWords'='5' 'MinLength'='8'
```

Her iki kapsayıcının durumu *Sonlandırıldı* (durumu kontrol etmek için [az kapsayıcı gösterisini][az-container-show] kullanın) olarak gösterildikten sonra, çıktıyı görmek için günlüklerini [az kapsayıcı günlükleriyle][az-container-logs] görüntüler.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

Kapsayıcıların çıktısı, ortam değişkenlerini ayarlayarak ikinci kapsayıcının komut dosyası davranışını nasıl değiştirdiğinizi gösterir.

**mycontainer1**
```output
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

**mycontainer2**
```output
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Azure PowerShell örneği

PowerShell'de ortam değişkenlerini ayarlamak CLI'ye `-EnvironmentVariable` benzer, ancak komut satırı bağımsız değişkenini kullanır.

İlk olarak, bu [Yeni-AzContainerGroup][new-Azcontainergroup] komutu ile varsayılan yapılandırmasında [aci-wordcount][aci-wordcount] kapsayıcı başlatın:

```azurepowershell-interactive
New-AzContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image mcr.microsoft.com/azuredocs/aci-wordcount:latest
```

Şimdi aşağıdaki [New-AzContainerGroup][new-Azcontainergroup] komutunu çalıştırın. Bu, bir dizi değişkenini doldurmadan sonra *NumWords* ve *MinLength* ortam değişkenlerini belirtir: `envVars`

```azurepowershell-interactive
$envVars = @{'NumWords'='5';'MinLength'='8'}
New-AzContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image mcr.microsoft.com/azuredocs/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Her iki kapsayıcının durumu *sonlandırıldıktan* sonra (durumu kontrol etmek için [Get-AzContainerInstanceLog'u][azure-instance-log] kullanın) [Get-AzContainerInstanceLog][azure-instance-log] komutuyla günlüklerini çekin.

```azurepowershell-interactive
Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

Her kapsayıcının çıktısı, ortam değişkenlerini ayarlayarak kapsayıcı tarafından çalıştırılabilen komut dosyasını nasıl değiştirdiğinizi gösterir.

```console
PS Azure:\> Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

Azure:\
PS Azure:\> Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Azure portalı örneği

Azure portalında bir kapsayıcı başlattığınızda ortam değişkenlerini ayarlamak için, kapsayıcıyı oluştururken **bunları Gelişmiş** sayfada belirtin.

1. **Gelişmiş** sayfada, Yeniden **Başlatma ilkesini** *on hatasına* ayarlayın
2. **Çevre değişkenleri**altında, ilk `5` değişken için bir değer `MinLength` le girin `8` `NumWords` ve ikinci değişken için bir değerle girin. 
1. Doğrulamak için **Gözden Geçir + oluştur'u** seçin ve ardından kapsayıcıyı dağıtın.

![Çevre değişkenini gösteren portal sayfası Etkinleştir düğmesi ve metin kutuları][portal-env-vars-01]

Kapsayıcının günlüklerini görüntülemek için **Ayarlar'ın** altında **Kapsayıcılar'ı**seçin, ardından Günlükler. **Containers** Önceki CLI ve PowerShell bölümlerinde gösterilen çıktıya benzer şekilde, komut dosyasının davranışının ortam değişkenleri tarafından nasıl değiştirildiğini görebilirsiniz. Her biri en az sekiz karakter uzunluğunda beş sözcük görüntülenir.

![Konteyner günlük çıktısını gösteren portal][portal-env-vars-02]

## <a name="secure-values"></a>Güvenli değerler

Güvenli değerlere sahip nesneler, uygulamanız için parolalar veya anahtarlar gibi hassas bilgileri tutmak için tasarlanmıştır. Ortam değişkenleri için güvenli değerler kullanmak, kapsayıcınızın görüntüsüne dahil etmekten daha güvenli ve esnektir. Başka bir seçenek, [Azure Kapsayıcı Örnekleri'nde gizli bir birim montaj'da](container-instances-volume-secret.md)açıklanan gizli birimleri kullanmaktır.

Güvenli değerlere sahip ortam değişkenleri kapsayıcınızın özelliklerinde görünmez, değerlerine yalnızca kapsayıcının içinden erişilebilir. Örneğin, Azure portalında veya Azure CLI'de görüntülenen kapsayıcı özellikleri, değerini değil, yalnızca güvenli bir değişkenin adını görüntüler.

Değişkenin `secureValue` türü için normal `value` yerine özelliği belirterek güvenli bir ortam değişkeni ayarlayın. Aşağıdaki YAML'de tanımlanan iki değişken iki değişken türünü gösterir.

### <a name="yaml-deployment"></a>YAML dağıtımı

Aşağıdaki `secure-env.yaml` parçacıkiçeren bir dosya oluşturun.

```yaml
apiVersion: 2018-10-01
location: eastus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - name: 'NOTSECRET'
          value: 'my-exposed-value'
        - name: 'SECRET'
          secureValue: 'my-secret-value'
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Yaml ile kapsayıcı grubunu dağıtmak için aşağıdaki komutu çalıştırın (kaynak grup adını gerektiği gibi ayarlayın):

```azurecli-interactive
az container create --resource-group myResourceGroup --file secure-env.yaml
```

### <a name="verify-environment-variables"></a>Ortam değişkenlerini doğrulama

Kapsayıcınızın ortam değişkenlerini sorgulamak için [az kapsayıcı göster][az-container-show] komutunu çalıştırın:

```azurecli-interactive
az container show --resource-group myResourceGroup --name securetest --query 'containers[].environmentVariables'
```

JSON yanıtı hem güvenli ortam değişkeninin anahtarını hem de değerini, ancak yalnızca güvenli ortam değişkeninin adını gösterir:

```json
[
  [
    {
      "name": "NOTSECRET",
      "secureValue": null,
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET",
      "secureValue": null,
      "value": null
    }
  ]
]
```

Çalışan bir kapsayıcıda bir komutun yürütülmesini sağlayan [az kapsayıcı exec][az-container-exec] komutu yla, güvenli ortam değişkeninin ayarlandığını doğrulayabilirsiniz. Kapsayıcıda etkileşimli bir bash oturumu başlatmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name securetest --exec-command "/bin/bash"
```

Kapsayıcı içinde etkileşimli bir kabuk açtıktan sonra `SECRET` değişkenin değerine erişebilirsiniz:

```console
root@caas-ef3ee231482549629ac8a40c0d3807fd-3881559887-5374l:/# echo $SECRET
my-secret-value
```

## <a name="next-steps"></a>Sonraki adımlar

Birkaç kapsayıcıyla büyük bir veri kümesini toplu işleme gibi görev tabanlı senaryolar, çalışma zamanında özel ortam değişkenlerinden yararlanabilir. Görev tabanlı kapsayıcıları çalıştırma hakkında daha fazla bilgi için [bkz.](container-instances-restart-policy.md)

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/az.containerinstance/get-azcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-Az-ps
[new-Azcontainergroup]: /powershell/module/az.containerinstance/new-azcontainergroup
[portal]: https://portal.azure.com
