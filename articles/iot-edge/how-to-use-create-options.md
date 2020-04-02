---
title: Modüller için createOptions yazın - Azure IoT Edge | Microsoft Dokümanlar
description: Modülleri çalışma zamanında yapılandırmak için dağıtım bildiriminde createOptions nasıl kullanılır?
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/01/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: c07e161042a497a232cbd5e3f11128893a095381
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80550350"
---
# <a name="how-to-configure-container-create-options-for-iot-edge-modules"></a>IoT Edge modülleri için kapsayıcı oluşturma seçenekleri nasıl yapılandırılır?

Dağıtım bildirimindeki **createOptions** parametresi, modül kapsayıcılarını çalışma zamanında yapılandırmanızı sağlar. Bu parametre, modüller üzerindeki denetiminizi genişletir ve modülün ana bilgisayarın kaynaklarına erişimine izin verme veya kısıtlama veya ağ yapılandırma gibi görevlere olanak tanır.

IoT Edge modülleri, IoT Edge cihazınızda Docker uyumlu kaplar olarak uygulanır. Docker kapsayıcı oluşturmak için birçok seçenek sunar ve bu seçenekler IoT Edge modülleri için de geçerlidir. Daha fazla bilgi için [Docker kapsayıcısı seçenekleri oluşturmaya](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate)bakın.

## <a name="format-create-options"></a>Oluşturma seçeneklerini biçimlendirme

IoT Edge dağıtım bildirimi, JSON olarak biçimlendirilmiş oluşturma seçeneklerini kabul eder. Örneğin, her edgeHub modülü için otomatik olarak dahil edilen oluşturma seçeneklerini ele alalım:

```json
"createOptions": {
  "HostConfig": {
    "PortBindings": {
      "5671/tcp": [
        {
          "HostPort": "5671"
        }
      ],
      "8883/tcp": [
        {
          "HostPort": "8883"
        }
      ],
      "443/tcp": [
        {
          "HostPort": "443"
        }
      ]
    }
  }
}
```

Bu edgeHub örneği, kapsayıcıdaki açıkta kalan bağlantı noktalarını ana aygıttaki bir bağlantı noktasına eşlemek için **HostConfig.PortBindings** parametresini kullanır.

Visual Studio veya Visual Studio Code için Azure IoT Araçları uzantılarını kullanıyorsanız, oluşturma seçeneklerini **deployment.template.json** dosyasına JSON biçiminde yazabilirsiniz. Ardından, uzantıyı IoT Edge çözümcüllerini oluşturmak veya dağıtım bildirimini oluşturmak için kullandığınızda, JSON'u sizin için IoT Edge çalışma zamanının beklediği biçimde dize eder. Örnek:

```json
"createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
```

Oluşturma seçeneklerini yazmak için bir `docker inspect` ipucu komutu kullanmaktır. Geliştirme sürecinizin bir parçası olarak, modülü `docker run <container name>`yerel olarak çalıştırın. Modülü istediğiniz gibi çalıştırdıktan sonra çalıştırın. `docker inspect <container name>` Bu komut, modül ayrıntılarını JSON formatında çıkar. Yapılandırdığınız parametreleri bulun ve JSON'u kopyalayın. Örnek:

[![Docker incelemesi edgeHub](./media/how-to-use-create-options/docker-inspect-edgehub-inline-and-expanded.png) sonuçları](./media/how-to-use-create-options/docker-inspect-edgehub-inline-and-expanded.png#lightbox)

## <a name="common-scenarios"></a>Genel senaryolar

Kapsayıcı oluşturma seçenekleri birçok senaryoya olanak sağlar, ancak IoT Edge çözümleri oluştururken en sık karşılaşılan bazı seçenekler şunlardır:

* [Modüllere ana bilgisayar depolamasına erişim verme](how-to-access-host-storage-from-module.md)
* [Harita ana bağlantı noktası modül bağlantı noktasına](#map-host-port-to-module-port)
* [Modül belleği ve CPU kullanımını kısıtlama](#restrict-module-memory-and-cpu-usage)

### <a name="map-host-port-to-module-port"></a>Harita ana bağlantı noktası modül bağlantı noktasına

Modülünüzün IoT Edge çözümü dışındaki bir hizmetle iletişim kurması gerekiyorsa ve bunu yapmak için ileti yönlendirmesi kullanmıyorsa, ana bilgisayar bağlantı noktasını bir modül bağlantı noktasına eşlemeniz gerekir.

>[!TIP]
>Bu bağlantı noktası eşlemesi, aynı aygıtta modülden modüle iletişim için gerekli değildir. A modülü, B modülünde barındırılan bir API'yi sorgulaması gerekiyorsa, bunu herhangi bir bağlantı noktası eşlemesi olmadan yapabilir. B modülü, örneğin, dockerfile bir bağlantı `EXPOSE 8080`noktası ortaya çıkarmak gerekir: . Daha sonra A modülü, örneğin B modülünün adını `http://ModuleB:8080/api`kullanarak API'yi sorgulayabilir: .

İlk olarak, modülün içindeki bir bağlantı noktasının bağlantıları dinlemek için maruz kaldığından emin olun. Bunu dockerfile'deki bir [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose) yönergesi kullanarak yapabilirsiniz. Örneğin, `EXPOSE 8080`. Açık pozlama yönergesi belirtilmemişse TCP protokolüne varsayılan olarak veya UDP belirtebilirsiniz.

Ardından, Docker kapsayıcısının **HostConfig** grubunda ki **PortBindings** ayarını kullanarak modüldeki açıkta kalan bağlantı noktasını ana aygıttaki bir bağlantı noktasına eşlemek için [seçenekler oluşturun.](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) Örneğin, modülün içindeki port 8080'i maruz bıraktıysanız ve bunu ana bilgisayar aygıtının 80 no'lu bağlantı noktasıyla eşlemek istiyorsanız, template.json dosyasındaki oluşturma seçenekleri aşağıdaki örnek gibi görünür:

```json
"createOptions": {
  "HostConfig": {
    "PortBindings": {
      "8080/tcp": [
        {
          "HostPort": "80"
        }
      ]
    }
  }
}
```

Dağıtım bildirimi için dizelendirildikten sonra, aynı yapılandırma aşağıdaki örnek gibi görünür:

```json
"createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8080/tcp\":[{\"HostPort\":\"80\"}]}}}"
```

### <a name="restrict-module-memory-and-cpu-usage"></a>Modül belleği ve CPU kullanımını kısıtlama

Bir modülün kullanabileceği ana bilgisayar kaynaklarının ne kadarını kullanabileceğini bildirebilirsiniz. Bu denetim, bir modülün çok fazla bellek veya CPU kullanamamasını sağlamak ve diğer işlemlerin aygıtta çalışmasını önlemek için yararlıdır. **HostConfig** grubunda [Docker kapsayıcı oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) yle bu ayarları yönetebilirsiniz:

* **Bellek**: Baytlarda bellek sınırı. Örneğin, 268435456 bayt = 256 MB.
* **MemorySwap**: Toplam bellek sınırı (bellek + takas). Örneğin, 536870912 bayt = 512 MB
* **CpuPeriod**: Bir CPU döneminin mikrosaniye cinsinden uzunluğu. Varsayılan değer 100000'dir, bu nedenle, örneğin, 25000 değeri bir kapsayıcıyı CPU kaynaklarının %25'i ile sınırlar.

template.json biçiminde, bu değerler aşağıdaki örnek gibi görünür:

```json
"createOptions": {
  "HostConfig": {
    "Memory": 268435456,
    "MemorySwap": 536870912,
    "CpuPeriod": 25000
  }
}
```

Son dağıtım bildirimi için dizelendirildikten sonra, bu değerler aşağıdaki örnek gibi görünür:

```json
"createOptions":"{\"HostConfig\":{\"Memory\":268435456,\"MemorySwap\":536870912,\"CpuPeriod\":25000}}"
```

## <a name="next-steps"></a>Sonraki adımlar

İş başındaki oluşturma seçeneklerine daha fazla örnek için aşağıdaki IoT Edge örneklerine bakın:

* [Ahududu Pi 3'te Özel Görme ve Azure IoT Edge](https://github.com/Azure-Samples/Custom-vision-service-iot-edge-raspberry-pi)
* [Azure IoT Edge blob depolama örneği](https://github.com/Azure-Samples/azure-iotedge-blobstorage-sample)
