---
title: Azure Hizmet Kumaşı'ndaki yinelemeler ve örnekler
description: Kullanım Döngüleri ve işlevlerine genel bakış da dahil olmak üzere Hizmet Kumaşı'ndaki yinelemeler ve örnekler hakkında bilgi edinin.
author: appi101
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: cf21af43de553a2802289e44eaece12952d077d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258570"
---
# <a name="replicas-and-instances"></a>Çoğaltmalar ve örnekler 
Bu makalede, devlet hizmetlerinin yinelemelerinin yaşam döngüsüne ve devletsiz hizmet örneklerine genel bir bakış verebilirsiniz.

## <a name="instances-of-stateless-services"></a>Devletsiz hizmet örnekleri
Bir devletsiz hizmet örneği, kümenin düğümlerinden birinde çalışan hizmet mantığının kopyasıdır. Bir bölüm içindeki bir örnek, **InstanceId'i**tarafından benzersiz bir şekilde tanımlanır. Bir örneğin yaşam döngüsü aşağıdaki diyagramda modellenmiştir:

![Örnek yaşam döngüsü](./media/service-fabric-concepts-replica-lifecycle/instance.png)

### <a name="inbuild-ib"></a>InBuild (IB)
Küme Kaynak Yöneticisi örnek için bir yerleşim belirledikten sonra, bu yaşam döngüsü durumuna girer. Örnek düğümüzerinde başlatılır. Uygulama ana bilgisayarı başlatılır, örnek oluşturulur ve sonra açılır. Başlangıç bittikten sonra, örnek hazır duruma geçiş eder. 

Bu örneğin uygulama ana bilgisayarı veya düğümü çatlarsa, bırakılan duruma geçiş eder.

### <a name="ready-rd"></a>Hazır (RD)
Hazır durumda, örnek çalışır durumda ve düğüm üzerinde çalışıyor. Bu örnek güvenilir bir hizmetse, **RunAsync** çağrılmıştır. 

Bu örneğin uygulama ana bilgisayarı veya düğümü çatlarsa, bırakılan duruma geçiş eder.

### <a name="closing-cl"></a>Kapanış (CL)
Kapanış durumunda, Azure Hizmet Dokusu bu düğümdeki örneği kapatma işlemindedir. Bu kapatma, uygulama yükseltmesi, yük dengeleme veya hizmetin silinme gibi birçok nedeni olabilir. Kapatma bittikten sonra, bırakılan duruma geçiş eder.

### <a name="dropped-dd"></a>Bırakılan (DD)
Bırakılan durumda, örnek artık düğümüzerinde çalışıyor. Bu noktada, Service Fabric bu örnekle ilgili meta verileri korur ve sonunda da silinir.

> [!NOTE]
> ForceRemove seçeneğini kullanarak herhangi bir durumdan bırakılan duruma **ForceRemove** geçiş yapmak `Remove-ServiceFabricReplica`mümkündür.
>

## <a name="replicas-of-stateful-services"></a>Devlet hizmetlerinin kopyaları
Durumlu bir hizmetin kopyası, kümenin düğümlerinden birinde çalışan hizmet mantığının kopyasıdır. Ayrıca, yineleme bu hizmetin durumunun bir kopyasını tutar. İki ilgili kavram, durum lu yinelemelerin yaşam döngüsünü ve davranışını açıklar:
- Çoğaltma yaşam döngüsü
- Yineleme rolü

Aşağıdaki tartışmada, kalıcı devlet hizmetleri açıklanmaktadır. Geçici (veya bellek içi) durumlu hizmetler için, aşağı ve bırakılan durumlar eşdeğerdir.

![Çoğaltma yaşam döngüsü](./media/service-fabric-concepts-replica-lifecycle/replica.png)

### <a name="inbuild-ib"></a>InBuild (IB)
InBuild yinelemesi, yineleme kümesine katılmak için oluşturulmuş veya hazırlanmış bir yinelemedir. Çoğaltma rolüne bağlı olarak, IB farklı semantik vardır. 

Uygulama ana bilgisayarı veya InBuild yinelemesi düğümü çöküyorsa, aşağı durumuna geçiş eder.

   - **Birincil InBuild yinelemeleri**: Birincil InBuild bir bölüm için ilk kopyaları vardır. Bu yineleme genellikle bölüm oluşturulurken gerçekleşir. Birincil InBuild yinelemeleri, bir bölümün tüm yinelemeleri yeniden başlatıldığında veya bırakıldığında da ortaya çıkar.

   - **IdleSecondary InBuild yinelemeleri**: Bunlar, Küme Kaynak Yöneticisi tarafından oluşturulan yeni yinelemeler veya aşağı inen ve kümeye geri eklenmesi gereken varolan yinelemelerdir. Bu yinelemeler, ActiveSecondary olarak çoğaltma kümesine katılmadan ve işlemlerin yeterli kabul edilmesine katılmadan önce birincil tarafından tohumlanır veya oluşturulur.

   - **ActiveSecondary InBuild yinelemeleri**: Bu durum bazı sorgularda gözlenir. Yineleme kümesinin değişmediği, ancak yinelemenin yapılması gereken bir optimizasyondur. Yinelemenin kendisi normal durum makinesi geçişlerini izler (çoğaltma rolleri bölümünde açıklandığı gibi).

### <a name="ready-rd"></a>Hazır (RD)
Hazır yineleme, çoğaltma ve yeterli sayıda işlemin kabul edilmesine katılan bir yinelemedir. Hazır durum birincil ve etkin ikincil yinelemeler için geçerlidir.

Hazır çoğaltma için uygulama ana bilgisayar veya düğüm çökerse, aşağı durumuna geçiş eder.

### <a name="closing-cl"></a>Kapanış (CL)
Yineleme aşağıdaki senaryolarda kapanış durumuna girer:

- **Yineleme nin kodunu kapatma**: Hizmet Dokusu'nun yineleme için çalışan kodu kapatması gerekebilir. Bu kapatma birçok nedenden dolayı olabilir. Örneğin, bir uygulama, kumaş veya altyapı yükseltmesi veya yineleme tarafından bildirilen bir hata nedeniyle gerçekleşebilir. Yineleme yakınladığında, yineleme aşağı durumuna geçiş eder. Diskte depolanan bu yinelemeyle ilişkili kalıcı durum temizlenmez.

- **Yinelemeyi kümeden kaldırma**: Service Fabric'in kalıcı durumu kaldırması ve yineleme için çalışan kodu kapatması gerekebilir. Bu kapatma, örneğin yük dengelemegibi birçok nedenden dolayı olabilir.

### <a name="dropped-dd"></a>Bırakılan (DD)
Bırakılan durumda, örnek artık düğümüzerinde çalışıyor. Düğümde de durum kalmadı. Bu noktada, Service Fabric bu örnekle ilgili meta verileri korur ve sonunda da silinir.

### <a name="down-d"></a>Aşağı (D)
Aşağı durumda, yineleme kodu çalışmıyor, ancak bu yineleme için kalıcı durum bu düğümüzerinde var. Yineleme birçok nedenden dolayı aşağı olabilir -örneğin, düğüm aşağı olmak, yineleme kodunda bir kilitlenme, uygulama yükseltmesi veya yineleme hataları.

Bir aşağı yineleme, örneğin yükseltme düğümde bittiğinde, gerektiğinde Service Fabric tarafından açılır.

Yineleme rolü aşağı durumda ilgili değildir.

### <a name="opening-op"></a>Açılış (OP)
Service Fabric'in yinelemeyi yeniden yükseltmesi gerektiğinde bir aşağı yineleme açılış durumuna girer. Örneğin, bu durum, bir düğüm üzerinde uygulama bitişini tamamlayan bir kod yükseltmesi sonra olabilir. 

Uygulama ana bilgisayarı veya açılış yinelemesi için düğüm çatlarsa, aşağı durumuna geçiş eder.

Yineleme rolü açılış durumunda ilgili değildir.

### <a name="standby-sb"></a>Bekleme (SB)
Bekleme Yinelemesi, inen ve daha sonra açılan kalıcı bir hizmetin bir kopyasıdır. Bu yineleme, yineleme kümesine başka bir yineleme eklemesi gerekiyorsa Service Fabric tarafından kullanılabilir (yineleme zaten devletin bir kısmına sahip olduğundan ve yapı işlemi daha hızlı olduğundan). StandByReplicaKeepDuration süresi dolduktan sonra bekleme yinelemesi atılır.

Uygulama ana bilgisayarı veya bekleme yinelemesi için düğüm çökerse, aşağı durumuna geçiş eder.

Yineleme rolü bekleme durumunda alakalı değildir.

> [!NOTE]
> Aşağı ya da bırakılan herhangi bir yineleme *kadar*olarak kabul edilir.
>

> [!NOTE]
> ForceRemove seçeneğini kullanarak herhangi bir durumdan bırakılan duruma **ForceRemove** geçiş yapmak `Remove-ServiceFabricReplica`mümkündür.
>

## <a name="replica-role"></a>Yineleme rolü 
Yinelemenin rolü yineleme kümesindeki işlevini belirler:

- **Birincil (P)**: Çoğaltma kümesinde okuma yazma işlemlerini gerçekleştirmekten sorumlu bir birincil vardır. 
- **ActiveSecondary (S)**: Bunlar, birincil durum güncelleştirmelerini alan, bunları uygulayan ve sonra bildirimleri geri gönderen yinelemelerdir. Çoğaltma kümesinde birden çok etkin geçici bölüm vardır. Bu etkin geçici geçici lerin sayısı, hizmetin işleyebilir hata sayısını belirler.
- **IdleSecondary (I)**: Bu yinelemeler birincil tarafından oluşturuluyor. Etkin ikincil liğe terfi edilmeden önce birincil den devlet alıyorlar. 
- **Yok (N)**: Bu yinelemelerin yineleme kümesinde bir sorumluluğu yoktur.
- **Bilinmeyen (U)**: Hizmet Kumaşından **ChangeRole** API çağrısı almadan önce bir yinelemenin ilk rolüdür.

Aşağıdaki diyagram, yineleme rol geçişlerini ve oluşabilecekleri bazı örnek senaryoları gösterir:

![Yineleme rolü](./media/service-fabric-concepts-replica-lifecycle/role.png)

- U -> P: Yeni bir birincil yinelemenin oluşturulması.
- U -> I: Yeni bir boşta çoğaltma oluşturma.
- U -> N: Bekleme yinelemesinin silinmesi.
- I -> S: Boşta kalan ikincil inaktif ikincil lerin teşviki, böylece kabullerinin yeterliliğe katkıda bulunması.
- I -> P: Boşta kalan ın birincilden birincile terfisi. Boşta ikincil birincil olmak için doğru aday olduğunda bu özel yeniden yapılandırmaları altında gerçekleşebilir.
- I -> N: Boşta kalan ikincil yinelemenin silinmesi.
- S -> P: Aktif ikincil den sekonderin birincile terfisi. Bunun nedeni, Küme Kaynak Yöneticisi tarafından başlatılan birincil veya birincil hareketin başarısız olması olabilir. Örneğin, bir uygulama yükseltme veya yük dengeleme yanıt olabilir.
- S -> N: Etkin ikincil yinelemenin silinmesi.
- P -> S: Birincil yinelemenin indirgeme. Bunun nedeni Küme Kaynak Yöneticisi tarafından başlatılan birincil bir hareket olabilir. Örneğin, bir uygulama yükseltme veya yük dengeleme yanıt olabilir.
- P -> N: Birincil yinelemenin silinmesi.

> [!NOTE]
> [Güvenilir Aktörler](service-fabric-reliable-actors-introduction.md) ve [Güvenilir Hizmetler](service-fabric-reliable-services-introduction.md)gibi üst düzey programlama modelleri, yineleme rolleri kavramını geliştiriciden gizler. Aktörler'de rol kavramı gereksizdir. Hizmetlerde, çoğu senaryo için büyük ölçüde basitleştirilmiştir.
>

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

[Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)

