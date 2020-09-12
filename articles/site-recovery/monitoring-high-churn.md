---
title: Sanal makinelerde dalgalanma desenlerini izleme
description: Azure Site Recovery kullanarak korunan sanal makinelerde karmaşıklık düzenlerini izlemeyi öğrenin
author: Sharmistha-Rai
manager: gaggupta
ms.topic: how-to
ms.date: 09/09/2020
ms.author: sharrai
ms.openlocfilehash: 51f45b5c0b8a2729f64eb376e06875719474cbfc
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89664685"
---
# <a name="monitoring-churn-patterns-on-virtual-machines"></a>Sanal makinelerde dalgalanma desenlerini izleme

Bu makalede, bir sanal makinede dalgalanma desenlerini izlemek için kullanılabilecek çeşitli araçlara genel bir bakış sunulmaktadır. Uygun araçları kullanarak, tam olarak hangi uygulamanın yüksek karmaşıklığa neden olduğunu öğrenmek kolaydır ve bu uygulama için daha fazla eylem gerçekleştirilebilir.

## <a name="for-azure-virtual-machines-windows-or-linux"></a>Azure sanal makineler için (Windows veya Linux)

Makineniz Azure 'da barındırılıyorsa ve depolama için yönetilen veya yönetilmeyen bir disk kullanıyorsa, disk ölçümlerini izleyerek performansı kolayca izleyebilirsiniz. Bu, doğru disk seçimini, uygulama kullanım örüntüize uyacak şekilde izlemenizi ve yapmanızı sağlar. Uyarı, tanı ve derleme otomasyonu oluşturmak için de kullanabilirsiniz. [Daha fazla bilgi edinin](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

Makinelerinizi Azure Site Recovery ile koruduktan sonra, Azure Izleyici günlüklerini ve Log Analytics kullanarak makinelerinizi izleyebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/monitor-log-analytics).

Ayrıca kullanabileceğiniz bazı işletim sistemine özgü araçlar da vardır.

## <a name="for-windows-machines"></a>Windows makineleri için

Bir makineniz varsa veya Windows işletim sistemi çalıştıran şirket içi olmanız durumunda, daha fazla kullanılabilir araç daha vardır.

Görev Yöneticisi 'nde disk kullanımını denetlemeye göre, **Kaynak İzleyicisi** ve **performans izleyicisine**her zaman başvurabilirsiniz. Bu araçlar Windows makinelerde zaten mevcuttur.

### <a name="resource-monitor"></a>Kaynak İzleyicisi

**Kaynak İzleyicisi** , donanım ve yazılım kaynaklarının gerçek zamanlı kullanımı hakkındaki bilgileri görüntüler. Bir Windows makinesinde Kaynak İzleyicisi çalıştırmak için aşağıdaki adımları izleyin:

1. Win + R tuşlarına basın ve _Resmon_yazın.
1. Resmon olduktan sonra, Kaynak İzleyicisi, pencere disk sekmesine geç ' i açar. Şu görünümü verir-

    ![Kaynak İzleyicisi disk sekmesi](./media/monitoring-high-churn/resmon-disk-tab.png)

1. Net bir resim elde etmek için bu sekmenin sürekli olarak izlenmesi gerekir. Yukarıdaki örnekte _wmiprv.exe_ yüksek olduğunu görüyoruz.

Makinenizde yüksek karmaşıklığa neden olan uygulamaları tanımladıktan sonra, bu uygulamalarla ilgili karmaşıklığın üstesinden gelmesi için gerekli işlemleri yapabilirsiniz.

### <a name="performance-monitor"></a>Performans İzleyicisi

**Performans İzleyicisi** , BIR bilgisayardaki CPU veya bellek kullanımı gibi çeşitli etkinlikleri izler. Bir Windows makinesinde performans Izleyicisini çalıştırmak için aşağıdaki adımları izleyin:

1. Win + R tuşlarına basın ve _Perfmon_yazın.
1. Perfmon, diğer bir deyişle performans Izleyicisi, pencere açıldığında, aşağıdaki görünüme sahiptir-

    ![Performans Izleyicisi adım](./media/monitoring-high-churn/perfmon-step1.png)

1. Sağ taraftaki **Izleme Araçları** klasörünü genişletin ve Performans İzleyicisi ' ne tıklayın. Bu işlem, geçerli performansla ilgili gerçek zamanlı bilgiler sağlayan aşağıdaki görünümü açar-

    ![Performans Izleyicisi Step2](./media/monitoring-high-churn/perfmon-step1.png)

1. Bu grafik şu anda yalnızca bir izleyiciyi izliyor, diğer bir deyişle, grafiğin altındaki tabloda belirtilen '% Işlemci zamanı '. Aracın üstündeki **' + '** öğesine tıklayarak izleme için daha fazla öğe ekleyebilirsiniz.
1. Aşağıda, daha fazla sayaç eklediğimiz için performans Izleyicisi 'nin nasıl göründüğü bir görsel yer verilmiştir.

    ![Performans Izleyicisi Step3](./media/monitoring-high-churn/perfmon-step3.png)

Performans Izleyicisi hakkında [buradan](https://docs.microsoft.com/dynamics365/business-central/dev-itpro/administration/monitor-use-performance-monitor-collect-event-trace-data)daha fazla bilgi edinin.

## <a name="for-linux-machines"></a>Linux makineleri için

Bir makineniz varsa veya Linux işletim sistemi çalıştıran şirket içi olmanız durumunda, karmaşıklık düzenlerini izlemek için kullanabileceğiniz birkaç araç daha vardır.

### <a name="iotop"></a>İotop

En çok kullanılan araçlardan biri _iotop_'dir. Gerçek zamanlı disk etkinliklerini görüntülemek için bir yardımcı programdır. G/ç gerçekleştiren işlemlerini, kullandıkları disk bant genişliğiyle birlikte listeleyebilir.

Komut istemi ' ni açın ve komutunu çalıştırın `iotop` .

### <a name="iostat"></a>Iostat

Iostat, sistem girişi ve çıkış depolama cihazı istatistiklerini toplayıp gösteren basit bir araçtır. Bu araç genellikle cihazlar, yerel diskler, uzak diskler dahil olmak üzere depolama cihazı performans sorunlarını izlemek için kullanılır.

Komut istemi ' ni açın ve komutunu çalıştırın `iostat` .

## <a name="next-steps"></a>Sonraki adımlar

[Azure izleyici](monitor-log-analytics.md)ile nasıl izleneceğini öğrenin.
