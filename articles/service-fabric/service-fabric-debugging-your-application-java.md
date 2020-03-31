---
title: Eclipse'de uygulamanızı hata ayıklama
description: Yerel bir geliştirme kümesinde Eclipse'de bunları geliştirip hata ayıklayarak hizmetlerinizin güvenilirliğini ve performansını artırın.
author: suhuruli
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 15448a9bd8998a99e8fce578b05130694ecd5fd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614494"
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Eclipse kullanarak Java Servis Kumaş uygulama hata ayıklama
> [!div class="op_single_selector"]
> * [Görsel Stüdyo/CSharp](service-fabric-debugging-your-application.md) 
> * [Tutulma /Java](service-fabric-debugging-your-application-java.md)
> 

1. [Hizmet Kumaşı geliştirme ortamınızı ayarlama](service-fabric-get-started-linux.md)adımlarını izleyerek yerel bir geliştirme kümesi başlatın.

2. Hata ayıklamak istediğiniz hizmetin entryPoint.sh güncelleştirin, böylece java işlemini uzaktan hata ayıklama parametreleriyle başlatır. Bu dosya aşağıdaki konumda bulunabilir: `ApplicationName\ServiceNamePkg\Code\entrypoint.sh`. Bu örnekte, hata ayıklama için 8001 numaralı bağlantı noktası ayarlanmıştır.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Örnek sayısını veya hata ayıklanan hizmetin yineleme sayısını 1 olarak ayarlayarak Uygulama Bildirimini güncelleştirin. Bu ayar, hata ayıklama için kullanılan bağlantı noktası için çakışmaları önler. Örneğin, şu işlemleri izleyerek durum bilgisi olmayan hizmetler için `InstanceCount="1"` ayarını yapın ve durum bilgisi olan hizmetler için hedefi ve en az çoğaltma kümesi boyutlarını 1 olarak ayarlayın: `TargetReplicaSetSize="1" MinReplicaSetSize="1"`.

4. Uygulamayı dağıtın.

5. Eclipse IDE'de, **Uzak Java Uygulaması ve giriş bağlantı özelliklerini > > Hata Ayıklama Yapılandırmalarını** seçin ve özellikleri aşağıdaki gibi ayarlayın:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Kesme noktalarını istenilen noktalarda ayarlayın ve uygulamayı hata ayıklayın.

Uygulama çöküyorsa, çekirdek dökümlerini de etkinleştirmek isteyebilirsiniz. Bir `ulimit -c` kabuk ta çalıştırın ve 0 döndürürse, çekirdek dökümleri etkinleştirilmeyecektir. Sınırsız coredumps etkinleştirmek için, `ulimit -c unlimited`aşağıdaki komutu çalıştırın: . Ayrıca komutu `ulimit -a`kullanarak durumu doğrulayabilirsiniz.  Coredump oluşturma yolunu güncelleştirmek istiyorsanız, `echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern`çalıştırın. 

### <a name="next-steps"></a>Sonraki adımlar

* [Linux Azure Tanılama'yı kullanarak günlükleri toplayın.](service-fabric-diagnostics-how-to-setup-lad.md)
* [Hizmetleri yerel olarak izleyin ve tanıkoyun.](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
