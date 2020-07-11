---
title: Kapsayıcı örneği üzerinde hazırlık araştırması ayarlama
description: Azure Container Instances içindeki kapsayıcıların istekleri yalnızca hazırlandıklarında almasını sağlamak için bir araştırma yapılandırmayı öğrenin
ms.topic: article
ms.date: 07/02/2020
ms.openlocfilehash: 3e89086d66f284df35e36dc8f1d68bb09264843f
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86169672"
---
# <a name="configure-readiness-probes"></a>Hazır olma yoklamalarını yapılandırma

Trafiğe sunan Kapsayıcılı uygulamalar için, kapsayıcının gelen istekleri işlemeye hazırlanmaya yönelik olduğunu doğrulamak isteyebilirsiniz. Azure Container Instances, kapsayıcınıza belirli koşullar altında erişilebilmesi için yapılandırma eklemek üzere hazırlık araştırmalarını destekler. Hazırlık araştırması bir [Kubernetes hazırlık araştırması](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)gibi davranır. Örneğin, bir kapsayıcı uygulamasının başlangıç sırasında büyük bir veri kümesi yüklemesi ve bu süre içinde istek almasını istemediğiniz bir işlem olması gerekebilir.

Bu makalede, bir hazırlık araştırması içeren bir kapsayıcı grubunun nasıl dağıtılacağı açıklanır. bu sayede bir kapsayıcının yalnızca araştırma başarılı olduğunda trafik almasını sağlar.

Azure Container Instances Ayrıca, sağlıksız bir kapsayıcının otomatik olarak yeniden başlatılmasına neden olacak şekilde yapılandırabileceğiniz [lizleştirme yoklamalarını](container-instances-liveness-probe.md)da destekler.

> [!NOTE]
> Şu anda bir sanal ağa dağıtılan bir kapsayıcı grubunda hazırlık araştırması kullanamazsınız.

## <a name="yaml-configuration"></a>YAML yapılandırması

Örnek olarak, `readiness-probe.yaml` bir hazırlık araştırması içeren aşağıdaki kod parçacığına sahip bir dosya oluşturun. Bu dosya, küçük bir Web uygulaması çalıştıran bir kapsayıcıdan oluşan bir kapsayıcı grubunu tanımlar. Uygulama, ortak `mcr.microsoft.com/azuredocs/aci-helloworld` görüntüden dağıtılır. Bu Kapsayıcılı uygulama, Azure CLı ve diğer hızlı başlangıçlarla [Azure 'da bir kapsayıcı örneği dağıtma](container-instances-quickstart.md) bölümünde de gösterilmiştir.

```yaml
apiVersion: 2019-12-01
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

Dağıtım, `command` kapsayıcının ilk kez çalışmaya başladığı zaman çalışan bir başlangıç komutu tanımlayan bir özelliği içerir. Bu özellik bir dize dizisini kabul eder. Bu komut, Web uygulamasının çalıştığı ancak kapsayıcının hazırlanmadığında bir zaman benzetimini yapar. 

İlk olarak, bir kabuk oturumu başlatır ve `node` Web uygulamasını başlatmak için bir komut çalıştırır. Ayrıca, 240 saniye boyunca uyku moduna geçmek için bir komut başlatır, sonrasında dizin içinde adlı bir dosya oluşturulur `ready` `/tmp` :

```console
node /usr/src/app/index.js & (sleep 240; touch /tmp/ready); wait
```

### <a name="readiness-command"></a>Hazır olma komutu

Bu YAML dosyası `readinessProbe` `exec` , hazırlık denetimi görevi gören bir hazır olma komutunu destekleyen bir tanımlar. Bu örnek, dizindeki dosyanın mevcut olması için komut testlerini hazır olarak onaylar `ready` `/tmp` .

`ready`Dosya mevcut olmadığında, hazırlık komutu sıfır olmayan bir değerle çıkar; kapsayıcı çalışmaya devam eder ancak erişilemez. Komut 0 çıkış koduyla başarıyla çıktığında, kapsayıcıya erişilmeye hazırlanın. 

`periodSeconds`Özelliği, hazırlık komutunun her 5 saniyede bir yürütülmesi gerektiğini belirler. Hazırlık araştırması, kapsayıcı grubunun ömrü boyunca çalışır.

## <a name="example-deployment"></a>Örnek dağıtım

Önceki YAML yapılandırmasıyla bir kapsayıcı grubu dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create --resource-group myResourceGroup --file readiness-probe.yaml
```

## <a name="view-readiness-checks"></a>Hazırlık denetimlerini görüntüle

Bu örnekte, ilk 240 saniye boyunca, dosyanın varlığını denetlediğinde hazırlık komutu başarısız olur `ready` . Durum kodu, kapsayıcının hazırlantığına işaret eder.

Bu olaylar Azure portal veya Azure CLı 'dan görüntülenebilir. Örneğin, Portal, `Unhealthy` hazır olma komutu başarısız olduğunda, bu türdeki olayların tetiklenmesini gösterir. 

![Portalın sağlıksız olayı][portal-unhealthy]

## <a name="verify-container-readiness"></a>Kapsayıcının hazır olduğunu doğrulama

Kapsayıcıyı başlattıktan sonra, başlangıçta erişilebilir durumda olmadığından emin olabilirsiniz. Sağlamadıktan sonra kapsayıcı grubunun IP adresini alın:

```azurecli
az container show --resource-group myResourceGroup --name readinesstest --query "ipAddress.ip" --out tsv
```

Hazırlık araştırması başarısız olsa da siteye erişmeyi deneyin:

```bash
wget <ipAddress>
```

Çıkış, siteyi başlangıçta erişilebilir olmadığını gösterir:
```
$ wget 192.0.2.1
--2019-10-15 16:46:02--  http://192.0.2.1/
Connecting to 192.0.2.1... connected.
HTTP request sent, awaiting response... 
```

240 saniye sonra, hazırlık komutu başarılı olur ve kapsayıcının hazır olduğunu işaret edin. Şimdi, komutunu çalıştırdığınızda başarılı olur `wget` :

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

Kapsayıcı hazırsanız, bir Web tarayıcısı kullanarak IP adresine göz atarak Web uygulamasına da erişebilirsiniz.

> [!NOTE]
> Hazırlık araştırması, kapsayıcı grubunun ömrü boyunca çalışmaya devam eder. Hazırlık komutu daha sonra başarısız olursa kapsayıcının yeniden erişilemez hale gelir. 
> 

## <a name="next-steps"></a>Sonraki adımlar

Hazır olma durumu araştırması, bağımlı kapsayıcılardan oluşan çok Kapsayıcılı grupları kapsayan senaryolarda yararlı olabilir. Çok Kapsayıcılı senaryolar hakkında daha fazla bilgi için [Azure Container Instances Içindeki kapsayıcı grupları](container-instances-container-groups.md)konusuna bakın.

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-readiness-probe/readiness-probe-failed.png
