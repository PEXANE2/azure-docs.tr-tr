---
title: Çakışan Küreler uygulamanızda hata ayıklama
description: Yerel bir geliştirme kümesindeki Çakışan Küreler üzerinde geliştirme ve hata ayıklama yaparak hizmetlerinizin güvenilirliğini ve performansını artırabilirsiniz.
author: suhuruli
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 15448a9bd8998a99e8fce578b05130694ecd5fd0
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614494"
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Çakışan Küreler kullanarak Java Service Fabric uygulamanızda hata ayıklama
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Tutulma/Java](service-fabric-debugging-your-application-java.md)
> 

1. [Service Fabric geliştirme ortamınızı ayarlama](service-fabric-get-started-linux.md)adımlarını izleyerek bir yerel geliştirme kümesi başlatın.

2. Hata ayıklamak istediğiniz hizmetin entryPoint.sh güncelleştirin, böylece Java işlemini uzaktan hata ayıklama parametreleriyle başlatır. Bu dosya şu konumda bulunabilir: `ApplicationName\ServiceNamePkg\Code\entrypoint.sh`. Bu örnekte, hata ayıklama için 8001 numaralı bağlantı noktası ayarlanmıştır.

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

Uygulama kilitleniyorlarsa, coredumps etkinleştirmek isteyebilirsiniz. `ulimit -c` bir kabukta yürütün ve 0 döndürürse coredumps etkin değildir. Sınırsız coredumps etkinleştirmek için şu komutu yürütün: `ulimit -c unlimited`. Ayrıca, komut `ulimit -a`kullanarak durumu doğrulayabilirsiniz.  Coredump oluşturma yolunu güncelleştirmek isterseniz `echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern`yürütün. 

### <a name="next-steps"></a>Sonraki adımlar

* [Linux Azure Tanılama kullanarak günlükleri toplayın](service-fabric-diagnostics-how-to-setup-lad.md).
* [Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
