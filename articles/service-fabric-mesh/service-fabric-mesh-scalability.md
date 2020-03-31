---
title: Azure Hizmet Kumaş Kafesi uygulamalarının ölçeklenebilirliği
description: Uygulamaları Service Fabric Mesh'e dağıtmanın avantajlarından biri, hizmetlerinizi el ile veya otomatik ölçeklendirme ilkeleriyle kolayca ölçeklendirebilmektir.
author: dkkapur
ms.author: dekapur
ms.date: 10/26/2018
ms.topic: conceptual
ms.openlocfilehash: 474eda904df653d514fd2ee59fa046f1f87a66aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259181"
---
# <a name="scaling-service-fabric-mesh-applications"></a>Ölçekleme Servisi Kumaş Örgü uygulamaları

Uygulamaları Service Fabric Mesh'e dağıtmanın en önemli avantajlarından biri, hizmetlerinizi kolayca ölçeklendirebilme veya dışarı edebilme yeteneğidir. Bu, hizmetlerinizdeki çeşitli miktarda yükü işlemek veya kullanılabilirliği artırmak için kullanılmalıdır. Hizmetlerinizi el ile ölçeklendirebilir veya dışarıda ölçeklendirebilir veya otomatik ölçeklendirme ilkelerini kurabilirsiniz.

## <a name="manual-scaling-instances"></a>El ile ölçekleme örnekleri

Uygulama kaynağı için dağıtım şablonunda her hizmet, hizmetin dağıtılmasını istediğiniz sayıyı ayarlamak için kullanılabilen bir *replicaCount* özelliğine sahiptir. Bir uygulama, birden çok hizmetten oluşabilir; her hizmet, birlikte dağıtılan ve yönetilen benzersiz bir *replicaCount* sayısına sahiptir. Hizmet çoğaltmaları sayısını ölçeklendirmek için, dağıtım şablonunda ölçeklendirmek istediğiniz her bir hizmet için *replicaCount* değerini veya parametreler dosyasını değiştirin. Ardından uygulamayı yükseltin.

El ile ölçeklendirme hizmetleri örnekleri için [bkz.](service-fabric-mesh-tutorial-template-scale-services.md)

## <a name="autoscaling-service-instances"></a>Hizmet örneklerini otomatik ölçekleme
Otomatik ölçeklendirme, hizmet örneğinizin (yatay ölçeklendirme) sayısını dinamik olarak ölçeklendirmek için Hizmet Dokusu'nun ek bir özelliğidir. Otomatik ölçekleme büyük esneklik sağlar ve CPU veya bellek kullanımına dayalı servis örneklerinin sağlanmasını veya kaldırılmasını sağlar.  Otomatik ölçekleme, iş yükünüz için doğru sayıda servis örneğini çalıştırmanızı ve maliyet için optimize etmenizi sağlar.

Hizmet kaynak dosyasındaki hizmet başına otomatik ölçekleme ilkesi tanımlanır. Her ölçekleme ilkesi iki bölümden oluşur:

- Hizmetin ölçekleme ne zaman gerçekleştirileceğini açıklayan bir ölçekleme tetikleyicisi. Hizmetin ne zaman ölçeklendireceğini belirleyen üç etken vardır. *Daha düşük yük eşiği,* hizmetin ne zaman ölçeklendirileceğini belirleyen bir değerdir. Bölümlerin tüm örneklerinin ortalama yükü bu değerden daha düşükse, hizmet ölçeklendirilir. *Üst yük eşiği,* hizmetin ne zaman ölçeklendirileceğini belirleyen bir değerdir. Bölümün tüm örneklerinin ortalama yükü bu değerden daha yüksekse, hizmet ölçeklendirilir. *Ölçekleme aralığı,* tetikleyicinin ne sıklıkta (saniye cinsinden) denetleneceğini belirler. Tetikleyici kontrol edildikten sonra, ölçekleme gerekiyorsa mekanizma uygulanır. Ölçekleme gerekli değilse, hiçbir işlem yapılmaz. Her iki durumda da, ölçekleme aralığı sona ermeden önce tetikleyici yeniden denetlenmez.

- Tetiklendiğinde ölçeklemenin nasıl gerçekleştirileceğini açıklayan ölçekleme mekanizması. *Ölçek artış,* mekanizma tetiklendiğinde kaç örnek ekleneceğini veya kaldırılacağını belirler. *Maksimum örnek sayısı* ölçekleme için üst sınırı tanımlar. Örnek sayısı bu sınıra ulaşırsa, hizmet yükten bağımsız olarak ölçeklendirilmeyecektir. *Minimum örnek sayısı* ölçekleme için alt sınırı tanımlar. Bölümün örnek sayısı bu sınıra ulaşırsa, hizmet yükten bağımsız olarak ölçeklendirilmeyecektir.

Hizmetiniz için otomatik ölçeklendirme ilkesini nasıl ayarlayabileceğinizi öğrenmek için [otomatik ölçeklendirme hizmetlerini](service-fabric-mesh-howto-auto-scale-services.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama modeli hakkında bilgi için [Service Fabric kaynaklarına](service-fabric-mesh-service-fabric-resources.md) bakın
