---
title: Kapsayıcı örneği üzerinde lizleştirme araştırması ayarlama
description: Azure Container Instances ' de uygun olmayan kapsayıcıları yeniden başlatmak için Lida araştırmaların nasıl yapılandırılacağını öğrenin
ms.topic: article
ms.date: 06/08/2018
ms.openlocfilehash: 96d98d18a3f0ac666fb2c057216f7844b176d177
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481680"
---
# <a name="configure-liveness-probes"></a>Canlılık yoklaması yapılandırma

Kapsayıcılı uygulamalar uzun süre çalışabilir, bu da kapsayıcıyı yeniden başlatarak onarılması gerekebilecek hatalı durumlara neden olabilir. Azure Container Instances, kapsayıcı grubunuzdaki Kapsayıcılarınızı, kritik işlevler çalışmıyorsa yeniden başlatılacak şekilde yapılandırabileceğiniz şekilde, lilik araştırmalarını destekler. Limek araştırması bir [Kubernetes araştırması](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)gibi davranır.

Bu makalede, sanal sağlıksız bir kapsayıcının otomatik olarak yeniden başlatılmasını gösteren, limize Me araştırması içeren bir kapsayıcı grubunun nasıl dağıtılacağı açıklanır.

Azure Container Instances Ayrıca, trafiğin yalnızca kendisine hazır olduğu zaman bir kapsayıcıya ulaşmasını sağlamak için yapılandırabileceğiniz [hazırlık araştırmalarını](container-instances-readiness-probe.md)destekler.

## <a name="yaml-deployment"></a>YAML dağıtımı

Aşağıdaki kod parçacığında `liveness-probe.yaml` bir dosya oluşturun. Bu dosya, sonunda sağlıksız hale gelen NGNX kapsayıcısından oluşan bir kapsayıcı grubunu tanımlar.

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

Yukarıdaki YAML yapılandırmasıyla Bu kapsayıcı grubunu dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Başlat komutu

Dağıtım, kapsayıcı ilk kez çalışmaya başladığında çalıştırılacak başlangıç komutunu tanımlar, bu, bir dizi dizeyi kabul eden `command` özelliği tarafından tanımlanır. Bu örnekte, bir bash oturumu başlatacak ve bu komutu geçirerek `/tmp` dizininde `healthy` adlı bir dosya oluşturacaktır:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 Daha sonra dosyayı silmeden önce 30 saniye sürer ve ardından 10 dakikalık bir uyku moduna girer.

### <a name="liveness-command"></a>Lida komutu

Bu dağıtım, lietler denetimi görevi gören bir `exec` lietler komutunu destekleyen bir `livenessProbe` tanımlar. Bu komut sıfır olmayan bir değerle çıkış alıyorsa, kapsayıcı sonlandırılır ve yeniden başlatılır, `healthy` dosyası bulunamadı. Bu komut, çıkış kodu 0 ile başarılı bir şekilde çıkılırken hiçbir işlem yapılmaz.

`periodSeconds` özelliği, uygun komutu her 5 saniyede bir yürütebilmelidir.

## <a name="verify-liveness-output"></a>Libir çıktıyı doğrula

İlk 30 saniye içinde, start komutu tarafından oluşturulan `healthy` dosya vardır. Liki komutu `healthy` dosyanın varlığını denetlediğinde, durum kodu sıfır döndürür ve başarılı olarak sinyal verir, böylece yeniden başlatma gerçekleşmez.

30 saniye sonra, `cat /tmp/healthy` başarısız olur, sağlıksız ve olayları sonlandırma olayları oluşmasına neden olur.

Bu olaylar Azure portal veya Azure CLı 'dan görüntülenebilir.

![Portalın sağlıksız olayı][portal-unhealthy]

Azure portal olayları görüntüleyerek, `Unhealthy` türündeki olaylar, uygun olmayan komut başarısız olduğunda tetiklenecektir. Sonraki olay `Killing`türünde olacaktır ve bir yeniden başlatmanın başlaması için bir kapsayıcı silmeyi belirtir. Bu olay gerçekleştiğinde kapsayıcının yeniden başlatma sayısı artar.

Yeniden başlatmalar, genel IP adresleri ve düğüme özgü içerikler gibi kaynakların korunabilmesi için yerinde tamamlanır.

![Portal yeniden başlatma sayacı][portal-restart]

Destekleneme araştırması sürekli olarak başarısız olur ve çok fazla yeniden başlatma tetiklerse, Kapsayıcınız bir üstel geri dönme gecikmesi girer.

## <a name="liveness-probes-and-restart-policies"></a>Lizleştirme araştırmaları ve yeniden başlatma ilkeleri

Yeniden başlatma ilkeleri, lilezleştirme araştırmaları tarafından tetiklenen yeniden başlatma davranışının yerini alır Örneğin, bir `restartPolicy = Never` *ve* bir lizleştirme araştırması ayarlarsanız, başarısız bir denetim nedeniyle kapsayıcı grubu yeniden başlatmaz. Kapsayıcı grubu bunun yerine kapsayıcı grubunun `Never`yeniden başlatma ilkesine bağlı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Önkoşul olmayan bir işlev düzgün çalışmıyorsa, görev tabanlı senaryolar otomatik yeniden başlatmaları etkinleştirmek için bir araştırma gerektirebilir. Görev tabanlı kapsayıcılar çalıştırma hakkında daha fazla bilgi için, bkz. [Azure Container Instances Kapsayıcılı görevleri çalıştırma](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
