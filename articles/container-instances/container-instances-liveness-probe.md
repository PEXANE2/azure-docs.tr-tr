---
title: Konteyner örneğinde canlılık sondası ayarlama
description: Azure Kapsayıcı Örnekleri'nde sağlıksız kapsayıcıları yeniden başlatmak için canlılık sondalarını nasıl yapılandırılasınız öğrenin
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 11c6c9d39067c536bf4325f74eb24b2ab64ef515
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76934157"
---
# <a name="configure-liveness-probes"></a>Canlılık yoklaması yapılandırma

Kapsayıcı uygulamalar uzun süre çalışabilir ve bu da kapsayıcıyı yeniden başlatarak onarılması gerekebilecek kırık durumlara neden olabilir. Azure Kapsayıcı Örnekleri, kritik işlevsellik çalışmıyorsa kapsayıcı grubunuzdaki kapsayıcılarınızı yeniden başlatmak üzere yapılandırabilmeniz için canlılık sondalarını destekler. Canlılık sondası Bir [Kubernetes canlılık sondası](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)gibi olur.

Bu makalede, benzetilen sağlıksız bir kapsayıcının otomatik olarak yeniden başlatılmasını gösteren, canlılık sondası içeren bir kapsayıcı grubunun nasıl dağıtılanacağını açıklar.

Azure Kapsayıcı Örnekleri, trafiğin yalnızca hazır olduğunda bir kapsayıcıya ulaştığından emin olmak için yapılandırabileceğiniz [hazır sondaları](container-instances-readiness-probe.md)da destekler.

> [!NOTE]
> Şu anda sanal ağa dağıtılan bir kapsayıcı grubunda canlılık sondası kullanamazsınız.

## <a name="yaml-deployment"></a>YAML dağıtımı

Aşağıdaki `liveness-probe.yaml` parçacıkiçeren bir dosya oluşturun. Bu dosya, sonunda sağlıksız hale gelir bir NGNIX kapsayıcı oluşan bir kapsayıcı grubu tanımlar.

```yaml
apiVersion: 2018-10-01
location: eastus
name: livenesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: nginx
      command:
        - "/bin/sh"
        - "-c"
        - "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      livenessProbe:
        exec:
            command:
                - "cat"
                - "/tmp/healthy"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Yukarıdaki YAML yapılandırması ile bu kapsayıcı grubu dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Başlat komutu

Dağıtım, kapsayıcı `command` ilk çalışmaya başladığında çalışan bir başlangıç komutunu tanımlayan bir özellik içerir. Bu özellik bir dizi dize kabul eder. Bu komut, sağlıksız bir duruma giren kapsayıcıyı simüle eder.

İlk olarak, bir bash oturumu başlar `healthy` ve `/tmp` dizin içinde adlandırılan bir dosya oluşturur. Daha sonra dosyayı silmeden önce 30 saniye uyur, sonra 10 dakikalık bir uyku girer:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

### <a name="liveness-command"></a>Canlılık komutu

Bu dağıtım, `livenessProbe` canlılık `exec` denetimi gibi davranan bir canlılık komutunu destekleyen bir tanım tanımlar. Bu komut sıfır olmayan bir değerle çıkarsa, kapsayıcı öldürülür ve `healthy` yeniden başlatılır, sinyal dosyası bulunamaz. Bu komut çıkış kodu 0 ile başarıyla çıkarsa, hiçbir işlem yapılmaz.

`periodSeconds` Özellik, canlılık komutunu her 5 saniyede bir yürütmeyi belirtir.

## <a name="verify-liveness-output"></a>Canlılık çıktısını doğrulama

İlk 30 saniye içinde, başlat komutu tarafından oluşturulan `healthy` dosya var. Canlılık komutu dosyanın `healthy` varlığını denetlediğinde, durum kodu 0 döndürür ve başarı sinyali verir, böylece yeniden başlatma gerçekleşmez.

30 saniye sonra, `cat /tmp/healthy` komut başarısız olmaya başlar, sağlıksız ve öldürme olayları meydana neden.

Bu etkinlikler Azure portalından veya Azure CLI'den görüntülenebilir.

![Portal sağlıksız olay][portal-unhealthy]

Azure portalındaki olayları görüntüleyerek, canlılık komutunun başarısız lığa uğratılamaması üzerine türdeki `Unhealthy` olaylar tetiklenir. Sonraki olay türü, `Killing`yeniden başlatma başlayabilir, böylece bir kapsayıcı silme anlamına gelen. Bu olay her gerçekleştiğinde kapsayıcı artışlarının yeniden başlatma sayısı.

Yeniden başlatmalar yerinde tamamlanır, böylece ortak IP adresleri ve düğüme özgü içerikler gibi kaynaklar korunur.

![Portal yeniden başlatma sayacı][portal-restart]

Canlılık sondası sürekli olarak başarısız olursa ve çok fazla yeniden başlatmayı tetiklerse, kapsayıcınız üstel bir geri tepme gecikmesi girer.

## <a name="liveness-probes-and-restart-policies"></a>Canlılık sondaları ve yeniden başlatma ilkeleri

Yeniden başlatma ilkeleri, canlılık sondaları tarafından tetiklenen yeniden başlatma davranışının yerini ala. Örneğin, bir `restartPolicy = Never` *ve* bir canlılık sondası ayarlarsanız, kapsayıcı grubu başarısız bir canlılık denetimi nedeniyle yeniden başlatılamaz. Kapsayıcı grubu bunun yerine kapsayıcı grubunun yeniden başlatma `Never`ilkesine bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

Görev tabanlı senaryolar, ön koşul işlevi düzgün çalışmıyorsa otomatik yeniden başlatmayı etkinleştirmek için bir canlılık sondası gerektirebilir. Görev tabanlı kapsayıcıları çalıştırma hakkında daha fazla bilgi için azure [kapsayıcı örneklerinde kapsayıcılı görevleri çalıştır'a](container-instances-restart-policy.md)bakın.

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
