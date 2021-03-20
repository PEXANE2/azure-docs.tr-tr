---
title: Çakışan Küreler uygulamanızda hata ayıklama
description: Yerel bir geliştirme kümesindeki Çakışan Küreler üzerinde geliştirme ve hata ayıklama yaparak hizmetlerinizin güvenilirliğini ve performansını artırabilirsiniz.
ms.topic: conceptual
ms.date: 11/02/2017
ms.custom: devx-track-java
ms.openlocfilehash: d321e0c10b66a15e6cb309cefe711602fa12957c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91534121"
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Çakışan Küreler kullanarak Java Service Fabric uygulamanızda hata ayıklama
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Tutulma/Java](service-fabric-debugging-your-application-java.md)
> 

1. [Service Fabric geliştirme ortamınızı ayarlama](service-fabric-get-started-linux.md)adımlarını izleyerek bir yerel geliştirme kümesi başlatın.

2. Hata ayıklamak istediğiniz hizmetin entryPoint.sh güncelleştirin, böylece Java işlemini uzaktan hata ayıklama parametreleriyle başlatır. Bu dosya, şu konumda bulunabilir: `ApplicationName\ServiceNamePkg\Code\entrypoint.sh` . Bu örnekte, hata ayıklama için 8001 numaralı bağlantı noktası ayarlanmıştır.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Hata ayıklanan hizmet için örnek sayısını veya çoğaltma sayısını 1 ' e ayarlayarak uygulama bildirimini güncelleştirin. Bu ayar, hata ayıklama için kullanılan bağlantı noktası için çakışmaları önler. Örneğin, şu işlemleri izleyerek durum bilgisi olmayan hizmetler için `InstanceCount="1"` ayarını yapın ve durum bilgisi olan hizmetler için hedefi ve en az çoğaltma kümesi boyutlarını 1 olarak ayarlayın: `TargetReplicaSetSize="1" MinReplicaSetSize="1"`.

4. Uygulamayı dağıtın.

5. Çakışan Küreler IDE 'de, **çalıştır > hata ayıklama yapılandırması-> uzak Java uygulaması ve giriş bağlantısı özellikleri** ' ni seçin ve özellikleri aşağıdaki gibi ayarlayın:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  İstenilen noktalarda kesme noktaları ayarlayın ve uygulamada hata ayıklayın.

Uygulama kilitleniyorlarsa, coredumps etkinleştirmek isteyebilirsiniz. `ulimit -c`Bir kabukta yürütün ve 0 döndürürse coredumps etkin değildir. Sınırsız coredumps etkinleştirmek için şu komutu yürütün: `ulimit -c unlimited` . Ayrıca, komutunu kullanarak durumu doğrulayabilirsiniz `ulimit -a` .  Coredump oluşturma yolunu güncelleştirmek isterseniz yürütün `echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern` . 

### <a name="next-steps"></a>Sonraki adımlar

* [Linux Azure Tanılama kullanarak günlükleri toplayın](./service-fabric-diagnostics-event-aggregation-lad.md).
* [Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
