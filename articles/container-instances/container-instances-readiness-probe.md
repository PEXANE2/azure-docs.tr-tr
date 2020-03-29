---
title: Konteyner örneğinde hazırlık sondasını ayarlama
description: Azure Kapsayıcı Örnekleri'ndeki kapsayıcıların yalnızca hazır olduklarında istek aldığından emin olmak için sondayı nasıl yapılandırıştırmayı öğrenin
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 64bb4a3e429ce820835abbf8e235600e592f7868
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76935689"
---
# <a name="configure-readiness-probes"></a>Hazır olma yoklamalarını yapılandırma

Trafiğe hizmet veren kapsayıcı uygulamaları için, kapsayıcınızın gelen istekleri işlemeye hazır olduğunu doğrulamak isteyebilirsiniz. Azure Kapsayıcı Örnekleri, kapsayıcınıza belirli koşullar altında erişilemesin diye yapılandırmaları içerecek hazır sondaları destekler. Hazırlık sondası [Bir Kubernetes hazırlık sondası gibi davranmaz.](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) Örneğin, bir kapsayıcı uygulamasının başlangıç sırasında büyük bir veri kümesiyüklemesi gerekebilir ve bu süre içinde istek almasını istemezsin.

Bu makalede, bir kapsayıcı yalnızca sonda başarılı olduğunda trafik alır, böylece bir hazır sonda içeren bir kapsayıcı grubu nasıl dağıtılanın açıklar.

Azure Kapsayıcı Örnekleri, sağlıksız bir kapsayıcının otomatik olarak yeniden başlatılmasına neden olacak şekilde yapılandırabileceğiniz [canlılık sondalarını](container-instances-liveness-probe.md)da destekler.

> [!NOTE]
> Şu anda sanal ağa dağıtılan bir kapsayıcı grubunda hazır sonda kullanamazsınız.

## <a name="yaml-configuration"></a>YAML yapılandırması

Örnek olarak, hazırlık `readiness-probe.yaml` sondası içeren aşağıdaki parçacıkiçeren bir dosya oluşturun. Bu dosya, küçük bir web uygulaması çalıştıran bir kapsayıcıdan oluşan bir kapsayıcı grubunu tanımlar. Uygulama genel `mcr.microsoft.com/azuredocs/aci-helloworld` görüntüden dağıtılır. Bu kapsayıcı uygulaması, Azure CLI ve diğer hızlı başlangıçlar [kullanılarak Azure'daki bir kapsayıcı örneğinde](container-instances-quickstart.md) de gösterilmiştir.

```yaml
apiVersion: 2018-10-01
location: eastus
name: readinesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      command:
        - "/bin/sh"
        - "-c"
        - "node /usr/src/app/index.js & (sleep 240; touch /tmp/ready); wait"
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      readinessProbe:
        exec:
          command:
          - "cat"
          - "/tmp/ready"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

### <a name="start-command"></a>Başlat komutu

Dağıtım, kapsayıcı `command` ilk çalışmaya başladığında çalışan bir başlangıç komutunu tanımlayan bir özellik içerir. Bu özellik bir dizi dize kabul eder. Bu komut, web uygulamasının çalıştığı ancak kapsayıcının hazır olmadığı bir zamanı simüle eder. 

İlk olarak, bir kabuk oturumu `node` başlar ve web uygulamasını başlatmak için bir komut çalıştırın. Ayrıca 240 saniye boyunca uyku komutu başlatır ve ardından `ready` `/tmp` dizin içinde çağrılan bir dosya oluşturur:

```console
node /usr/src/app/index.js & (sleep 240; touch /tmp/ready); wait
```

### <a name="readiness-command"></a>Hazırlık komutu

Bu YAML dosyası, `readinessProbe` hazırlık `exec` denetimi olarak hareket eden bir hazırlık komutunu destekleyen bir dosya tanımlar. Bu örnek hazır komutu, `ready` dizininde `/tmp` dosyanın varlığını sınar.

`ready` Dosya yoksa, hazırlık komutu sıfır olmayan bir değerle çıkar; kapsayıcı çalışmaya devam ediyor, ancak erişilenemiyor. Komut çıkış kodu 0 ile başarıyla çıktığında, kapsayıcıya erişilmeye hazırdır. 

Özellik `periodSeconds` hazır komutu her 5 saniyede bir yürütülmesi gerektiğini belirtir. Hazır prob konteyner grubunun ömrü boyunca çalışır.

## <a name="example-deployment"></a>Örnek dağıtım

Önceki YAML yapılandırmasına sahip bir kapsayıcı grubunu dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create --resource-group myResourceGroup --file readiness-probe.yaml
```

## <a name="view-readiness-checks"></a>Hazırlık denetimlerini görüntüleme

Bu örnekte, ilk 240 saniye boyunca, dosyanın `ready` varlığını denetler zaman hazırlık komutu başarısız olur. Durum kodu, kapsayıcının hazır olmadığını niçin döndürür.

Bu etkinlikler Azure portalından veya Azure CLI'den görüntülenebilir. Örneğin, portal, hazırlık komutu başarısız olduktan sonra tetiklenen tür `Unhealthy` deki olayları gösterir. 

![Portal sağlıksız olay][portal-unhealthy]

## <a name="verify-container-readiness"></a>Kapsayıcı hazır durumunu doğrulama

Kapsayıcıyı işe başladıktan sonra, başlangıçta erişilemediğini doğrulayabilirsiniz. Tedarikten sonra, konteyner grubunun IP adresini alın:

```azurecli
az container show --resource-group myResourceGroup --name readinesstest --query "ipAddress.ip" --out tsv
```

Hazırlık sondası başarısız olurken siteye erişmeye çalışın:

```bash
wget <ipAddress>
```

Çıktı, sitenin başlangıçta erişilemediğini gösterir:
```
$ wget 192.0.2.1
--2019-10-15 16:46:02--  http://192.0.2.1/
Connecting to 192.0.2.1... connected.
HTTP request sent, awaiting response... 
```

240 saniye sonra, hazır komutu başarılı, konteyner sinyalhazırdır. Şimdi, komutu `wget` çalıştırdığınızda, başarılı olur:

```
$ wget 192.0.2.1
--2019-10-15 16:46:02--  http://192.0.2.1/
Connecting to 192.0.2.1... connected.
HTTP request sent, awaiting response...200 OK
Length: 1663 (1.6K) [text/html]
Saving to: ‘index.html.1’

index.html.1                       100%[===============================================================>]   1.62K  --.-KB/s    in 0s      

2019-10-15 16:49:38 (113 MB/s) - ‘index.html.1’ saved [1663/1663] 
```

Kapsayıcı hazır olduğunda, bir web tarayıcısı kullanarak IP adresine göz atarak web uygulamasına da erişebilirsiniz.

> [!NOTE]
> Hazır prob konteyner grubunun ömrü boyunca devam eder. Hazırlık komutu daha sonra başarısız olursa, kapsayıcıya yeniden erişilemez hale gelir. 
> 

## <a name="next-steps"></a>Sonraki adımlar

Hazırlık sondası, bağımlı kapsayıcılardan oluşan çok kapsayıcılı grupları içeren senaryolarda yararlı olabilir. Çok kapsayıcı senaryoları hakkında daha fazla bilgi için [Azure Kapsayıcı Örnekleri'ndeki Kapsayıcı gruplarına](container-instances-container-groups.md)bakın.

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-readiness-probe/readiness-probe-failed.png
