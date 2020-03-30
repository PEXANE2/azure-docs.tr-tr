---
title: Kapsayıcı örnek günlüklerini & olayları alın
description: Kapsayıcı sorunlarını gidermeye yardımcı olmak için Azure Kapsayıcı Örnekleri'nde kapsayıcı günlüklerini ve etkinliklerini nasıl alabildiğini öğrenin
ms.topic: article
ms.date: 12/30/2019
ms.custom: mvc
ms.openlocfilehash: 0991b9cb1f99606910dbdf2c87b111f67da6da7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78249992"
---
# <a name="retrieve-container-logs-and-events-in-azure-container-instances"></a>Azure Container Instances kapsayıcı günlüklerini ve olaylarını alma

Azure Kapsayıcı Örnekleri'nde hatalı davranan bir kapsayıcınız olduğunda, günlüklerini [az kapsayıcı günlükleriyle][az-container-logs]görüntüleyerek başlayın ve [az kapsayıcı ekle][az-container-attach]ile standart ve standart hatasını akışlayın. Ayrıca Azure portalında kapsayıcı örnekleriiçin günlükleri ve olayları görüntüleyebilir veya kapsayıcı grupları için günlük ve olay verilerini [Azure Monitor günlüklerine](container-instances-log-analytics.md)gönderebilirsiniz.

## <a name="view-logs"></a>Günlükleri görüntüleme

Bir kapsayıcı içinde uygulama kodundan günlükleri görüntülemek için, [az konteyner günlükleri][az-container-logs] komutunu kullanabilirsiniz.

Aşağıda, komut satırı geçersiz bir URL sağladıktan sonra, [komut satırını bir kapsayıcı örneğinde ayarla komut satırındaki](container-instances-start-command.md#azure-cli-example)örnek görev tabanlı kapsayıcıdan günlük çıktısı verilmiştir:

```azurecli
az container logs --resource-group myResourceGroup --name mycontainer
```

```output
Traceback (most recent call last):
  File "wordcount.py", line 11, in <module>
    urllib.request.urlretrieve (sys.argv[1], "foo.txt")
  File "/usr/local/lib/python3.6/urllib/request.py", line 248, in urlretrieve
    with contextlib.closing(urlopen(url, data)) as fp:
  File "/usr/local/lib/python3.6/urllib/request.py", line 223, in urlopen
    return opener.open(url, data, timeout)
  File "/usr/local/lib/python3.6/urllib/request.py", line 532, in open
    response = meth(req, response)
  File "/usr/local/lib/python3.6/urllib/request.py", line 642, in http_response
    'http', request, response, code, msg, hdrs)
  File "/usr/local/lib/python3.6/urllib/request.py", line 570, in error
    return self._call_chain(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 504, in _call_chain
    result = func(*args)
  File "/usr/local/lib/python3.6/urllib/request.py", line 650, in http_error_default
    raise HTTPError(req.full_url, code, msg, hdrs, fp)
urllib.error.HTTPError: HTTP Error 404: Not Found
```

## <a name="attach-output-streams"></a>Çıkış akışları ekleme

[Az kapsayıcı ekleme komutu,][az-container-attach] kapsayıcı başlatma sırasında tanılama bilgileri sağlar. Kapsayıcı başladıktan sonra STDOUT ve STDERR'yi yerel konsolunuza aktarıyor.

Örneğin, işlemek için büyük bir metin dosyasının geçerli bir URL'sini sağladıktan sonra, [kapsayıcı örneğindeki komut satırını ayarla'daki](container-instances-start-command.md#azure-cli-example)görev tabanlı kapsayıcıdan çıktı aşağıda verilmiştir:

```azurecli
az container attach --resource-group myResourceGroup --name mycontainer
```

```output
Container 'mycontainer' is in state 'Unknown'...
Container 'mycontainer' is in state 'Waiting'...
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2019-03-21 19:42:39+00:00) pulling image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
Container 'mycontainer1' is in state 'Running'...
(count: 1) (last timestamp: 2019-03-21 19:42:39+00:00) pulling image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
(count: 1) (last timestamp: 2019-03-21 19:42:52+00:00) Successfully pulled image "mcr.microsoft.com/azuredocs/aci-wordcount:latest"
(count: 1) (last timestamp: 2019-03-21 19:42:55+00:00) Created container
(count: 1) (last timestamp: 2019-03-21 19:42:55+00:00) Started container

Start streaming logs:
[('the', 22979),
 ('I', 20003),
 ('and', 18373),
 ('to', 15651),
 ('of', 15558),
 ('a', 12500),
 ('you', 11818),
 ('my', 10651),
 ('in', 9707),
 ('is', 8195)]
```

## <a name="get-diagnostic-events"></a>Tanılama etkinlikleri alın

Kapsayıcınız başarılı bir şekilde dağıtılamazsa, Azure Kapsayıcı Örnekleri kaynak sağlayıcısı tarafından sağlanan tanılama bilgilerini gözden geçirin. Kapsayıcınızın olaylarını görüntülemek için [az kapsayıcı göster][az-container-show] komutunu çalıştırın:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

Çıktı, dağıtım olaylarıyla birlikte kapsayıcınızın temel özelliklerini içerir (burada kesilmiş olarak gösterilmiştir):

```JSON
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "mcr.microsoft.com/azuredocs/aci-helloworld",
      ...
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:22+00:00",
            "lastTimestamp": "2019-03-21T19:46:22+00:00",
            "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-helloworld\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:28+00:00",
            "lastTimestamp": "2019-03-21T19:46:28+00:00",
            "message": "Successfully pulled image \"mcr.microsoft.com/azuredocs/aci-helloworld\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:31+00:00",
            "lastTimestamp": "2019-03-21T19:46:31+00:00",
            "message": "Created container",
            "name": "Created",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2019-03-21T19:46:31+00:00",
            "lastTimestamp": "2019-03-21T19:46:31+00:00",
            "message": "Started container",
            "name": "Started",
            "type": "Normal"
          }
        ],
        "previousState": null,
        "restartCount": 0
      },
      "name": "mycontainer",
      "ports": [
        {
          "port": 80,
          "protocol": null
        }
      ],
      ...
    }
  ],
  ...
}
```
## <a name="next-steps"></a>Sonraki adımlar
Azure Kapsayıcı Örnekleri için [sık karşılaşılan kapsayıcı ve dağıtım sorunlarını](container-instances-troubleshooting.md) nasıl gidereceklerini öğrenin.

Kapsayıcı grupları için günlük ve olay verilerini [Azure Monitor günlüklerine](container-instances-log-analytics.md)nasıl göndereceğinizi öğrenin.

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show