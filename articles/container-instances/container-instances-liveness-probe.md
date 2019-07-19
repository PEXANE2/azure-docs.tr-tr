---
title: Azure Container Instances için lizleştirme araştırmalarını yapılandırma
description: Azure Container Instances ' de uygun olmayan kapsayıcıları yeniden başlatmak için Lida araştırmaların nasıl yapılandırılacağını öğrenin
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.openlocfilehash: 28205d6db85d7a5051f283445d95dd2375e174c8
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325862"
---
# <a name="configure-liveness-probes"></a>Canlılık yoklaması yapılandırma

Kapsayıcılı uygulamalar, kapsayıcıyı yeniden başlatarak onarılması gerekebilecek, bozuk durumlara neden olan uzun süreler için çalıştırılabilir. Azure Container Instances, yapılandırmaların, kritik işlevler çalışmıyorsa kapsayıcının yeniden başlatılmasına olanak sağlayacak şekilde yapılandırma eklemek için lilik araştırmaları destekler.

Bu makalede, sanal sağlıksız bir kapsayıcının otomatik olarak yeniden başlatılmasını gösteren, limize Me araştırması içeren bir kapsayıcı grubunun nasıl dağıtılacağı açıklanır.

## <a name="yaml-deployment"></a>YAML dağıtımı

Aşağıdaki kod `liveness-probe.yaml` parçacığına sahip bir dosya oluşturun. Bu dosya, sonunda sağlıksız hale gelen NGNX kapsayıcısından oluşan bir kapsayıcı grubunu tanımlar.

```yaml
apiVersion: 2018-06-01
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

Dağıtım, kapsayıcı ilk kez çalışmaya başladığında çalıştırılacak başlangıç komutunu tanımlar ve bir dize dizisini kabul eden `command` özelliği tarafından tanımlanır. Bu örnekte, bir bash oturumu başlatacak ve şu komutu geçirerek `healthy` `/tmp` dizin içinde adlı bir dosya oluşturacaktır:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

 Daha sonra dosyayı silmeden önce 30 saniye sürer ve sonra 10 dakikalık bir uyku moduna girer.

### <a name="liveness-command"></a>Lida komutu

Bu dağıtım, lietler denetimi olarak `exec` davranan bir lise komutunu destekleyen bir `livenessProbe` tanımlar. Bu komut sıfır olmayan bir değerle çıkış alıyorsa, kapsayıcı sonlandırılır ve yeniden başlatılır, `healthy` bu dosya bulunamamıştır. Bu komut, çıkış kodu 0 ile başarılı bir şekilde çıkılırken hiçbir işlem yapılmaz.

`periodSeconds` Özelliği, ebilmelidir komutunun her 5 saniyede bir yürütülmesi gerektiğini gösterir.

## <a name="verify-liveness-output"></a>Libir çıktıyı doğrula

İlk 30 saniye içinde, `healthy` start komutu tarafından oluşturulan dosya vardır. Liki komutu `healthy` dosyanın varlığını denetlediğinde, durum kodu sıfır döndürür, başarılı olarak sinyal verir ve yeniden başlatma gerçekleşmez.

30 saniye `cat /tmp/healthy` sonra, başarısız olarak başlayacak, sağlıksız ve olayların oluşmasına neden olur.

Bu olaylar Azure portal veya Azure CLı 'dan görüntülenebilir.

![Portalın sağlıksız olayı][portal-unhealthy]

Azure Portal olayları görüntüleyerek, türündeki `Unhealthy` olaylar, uygun olmayan komut başarısız olduğunda tetiklenecektir. Sonraki olay, bir yeniden başlatmanın başlaması `Killing`için bir kapsayıcı silme işlemini belirten türünde olacaktır. Bu her gerçekleştiğinde kapsayıcının yeniden başlatma sayısı artacaktır.

Yeniden başlatmalar, genel IP adresleri ve düğüme özgü içerikler gibi kaynakların korunabilmesi için yerinde tamamlanır.

![Portal yeniden başlatma sayacı][portal-restart]

Destekleneme araştırması sürekli olarak başarısız olur ve çok fazla yeniden başlatma tetiklerse, Kapsayıcınız bir üstel geri dönme gecikmesi girer.

## <a name="liveness-probes-and-restart-policies"></a>Lizleştirme araştırmaları ve yeniden başlatma ilkeleri

Yeniden başlatma ilkeleri, lilezleştirme araştırmaları tarafından tetiklenen yeniden başlatma davranışının yerini alır Örneğin, bir `restartPolicy = Never` *ve* bir limi araştırması ayarlarsanız, başarısız bir denetim durumunda kapsayıcı grubu yeniden başlatmaz. Kapsayıcı grubu bunun yerine kapsayıcı grubunun yeniden başlatma ilkesine göre `Never`değişir.

## <a name="next-steps"></a>Sonraki adımlar

Önkoşul olmayan bir işlev düzgün çalışmıyorsa, görev tabanlı senaryolar otomatik yeniden başlatmaları etkinleştirmek için bir araştırma gerektirebilir. Görev tabanlı kapsayıcılar çalıştırma hakkında daha fazla bilgi için, bkz. [Azure Container Instances Kapsayıcılı görevleri çalıştırma](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
