---
title: Kapsayıcı örneğinde giriş noktasını geçersiz kılma
description: Azure kapsayıcı örneğini dağıttığınızda kapsayıcı görüntüsündeki giriş noktasını geçersiz kılmak için bir komut satırı ayarlama
ms.topic: article
ms.date: 04/15/2019
ms.openlocfilehash: d9554603f78a07fa44af51d8f39a91e1b3c39f70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247130"
---
# <a name="set-the-command-line-in-a-container-instance-to-override-the-default-command-line-operation"></a>Varsayılan komut satırı işlemini geçersiz kılmak için komut satırını kapsayıcı örneğinde ayarlama

Bir kapsayıcı örneği oluşturduğunuzda, isteğe bağlı olarak kapsayıcı görüntüsüne girilen varsayılan komut satırı yönergesini geçersiz kılmak için bir komut belirtin. Bu davranış komut `--entrypoint` satırı bağımsız değişkenine `docker run`benzer.

Kapsayıcı örnekleri için [ortam değişkenleri](container-instances-environment-variables.md) ayarlama gibi, başlangıç komut satırı belirtmek, her kapsayıcıyı göreve özgü yapılandırmayla dinamik olarak hazırlamanız gereken toplu iş işleri için yararlıdır.

## <a name="command-line-guidelines"></a>Komut satırı yönergeleri

* Varsayılan olarak, komut satırı kapsayıcıda *bir kabuk olmadan başlayan tek bir işlem* belirtir. Örneğin, komut satırı bir Python komut dosyası veya yürütülebilir dosya çalıştırabilir. İşlem ek parametreler veya bağımsız değişkenler belirtebilir.

* Birden çok komut yürütmek için, konteyner işletim sisteminde desteklenen bir kabuk ortamı ayarlayarak komut satırınızı başlatın. Örnekler:

  |İşletim sistemi  |Varsayılan kabuk  |
  |---------|---------|
  |Ubuntu     |   `/bin/bash`      |
  |Alpine     |   `/bin/sh`      |
  |Windows     |    `cmd`     |

  Sırayla çalıştırmak için birden çok komutu birleştirmek için kabuğun kurallarını izleyin.

* Kapsayıcı yapılandırmasına bağlı olarak, yürütülebilir komut satırına veya bağımsız değişkenlere tam bir yol ayarlamanız gerekebilir.

* Komut satırının uzun süren bir görev mi yoksa bir kez çalışan bir görev mi belirtdiğine bağlı olarak kapsayıcı örneği için uygun bir [yeniden başlatma ilkesi](container-instances-restart-policy.md) ayarlayın. Örneğin, bir çalıştırma `Never` görevinin `OnFailure` yeniden başlatma ilkesi veya bir çalıştırma görevi için önerilir. 

* Kapsayıcı görüntüsünde varsayılan giriş noktası kümesi hakkında bilgi gerekiyorsa, [docker görüntü inceleme](https://docs.docker.com/engine/reference/commandline/image_inspect/) komutunu kullanın.

## <a name="command-line-syntax"></a>Komut satırı sözdizimi

Komut satırı sözdizimi, örnekleri oluşturmak için kullanılan Azure API'sine veya aracına bağlı olarak değişir. Bir kabuk ortamı belirtirseniz, kabuğun komut sözdizimi kurallarına da uyun.

* [az konteyner oluşturma][az-container-create] komutu: `--command-line` Parametre ile bir dize geçirin. Örnek: `--command-line "python myscript.py arg1 arg2"`).

* [Yeni AzureRmContainerGroup][new-azurermcontainergroup] Azure PowerShell cmdlet: Parametre `-Command` ile bir dize geçirin. Örnek: `-Command "echo hello"`.

* Azure portalı: **Komut** kapsayıcı yapılandırmasının geçersiz kılma özelliğinde, tırnak işareti olmadan virgülle ayrılmış dizeleri bir liste sağlayın. Örnek: `python, myscript.py, arg1, arg2`). 

* Kaynak Yöneticisi şablonu veya YAML dosyası veya Azure SDK'larından biri: Komut satırı özelliğini bir dizi dize olarak belirtin. Örnek: Kaynak Yöneticisi `["python", "myscript.py", "arg1", "arg2"]` şablonundaki JSON dizisi. 

  [Dockerfile](https://docs.docker.com/engine/reference/builder/) sözdizimini biliyorsanız, bu biçim CMD talimatının *exec* biçimine benzer.

### <a name="examples"></a>Örnekler

|    |  Azure CLI   | Portal | Şablon | 
| ---- | ---- | --- | --- |
| Tek komut | `--command-line "python myscript.py arg1 arg2"` | **Komut geçersiz kılma:**`python, myscript.py, arg1, arg2` | `"command": ["python", "myscript.py", "arg1", "arg2"]` |
| Birden çok komut | `--command-line "/bin/bash -c 'mkdir test; touch test/myfile; tail -f /dev/null'"` |**Komut geçersiz kılma:**`/bin/bash, -c, mkdir test; touch test/myfile; tail -f /dev/null` | `"command": ["/bin/bash", "-c", "mkdir test; touch test/myfile; tail -f /dev/null"]` |

## <a name="azure-cli-example"></a>Azure CLI örneği

Örnek olarak, Shakespeare'in *Hamlet'indeki* metni analiz eden [microsoft/aci-wordcount][aci-wordcount] kapsayıcı görüntüsünün davranışını değiştirerek en sık bulunan sözcükleri bulun. *Hamlet'i*çözümlemek yerine, farklı bir metin kaynağını işaret eden bir komut satırı ayarlayabilirsiniz.

Varsayılan metni analiz ettiğinde [microsoft/aci-wordcount][aci-wordcount] kapsayıcısının çıktısını görmek için aşağıdaki [az kapsayıcı oluşturma][az-container-create] komutuyla çalıştırın. Başlat komut satırı belirtilmedi, bu nedenle varsayılan kapsayıcı komutu çalışır. Bu örnek, illüstrasyon amacıyla, en az beş harf uzunluğundaki en üstteki 3 sözcüğü bulmak için [ortam değişkenlerini](container-instances-environment-variables.md) ayarlar:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --environment-variables NumWords=3 MinLength=5 \
    --restart-policy OnFailure
```

Kapsayıcının durumu *Sonlandırıldı* (durumu kontrol etmek için [az kapsayıcı gösterisini][az-container-show] kullanın) olarak gösterildikten sonra, çıktıyı görmek için günlük [az kapsayıcı günlükleriyle][az-container-logs] birlikte görüntüleyin.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
```

Çıktı:

```console
[('HAMLET', 386), ('HORATIO', 127), ('CLAUDIUS', 120)]
```

Şimdi farklı bir komut satırı belirterek farklı metni çözümlemek için ikinci bir örnek kapsayıcı ayarlayın. Kapsayıcı tarafından yürütülen Python komut dosyası, *wordcount.py,* bir URL'yi bağımsız değişken olarak kabul eder ve varsayılan yerine bu sayfanın içeriğini işler.

Örneğin, *Romeo ve Juliet*en az beş harf uzunluğunda üst 3 kelime belirlemek için:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Yine, kapsayıcı *Sonlandırıldıktan*sonra, kapsayıcının günlüklerini göstererek çıktıyı görüntüleyin:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Çıktı:

```console
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Sonraki adımlar

Birkaç kapsayıcıyla büyük bir veri kümesini toplu işleme gibi görev tabanlı senaryolar, çalışma zamanında özel komut satırlarından yararlanabilir. Görev tabanlı kapsayıcıları çalıştırma hakkında daha fazla bilgi için [bkz.](container-instances-restart-policy.md)

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
