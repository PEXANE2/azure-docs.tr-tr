---
title: Azure Service Fabric çoğaltmalar ve örnekler
description: Yaşam döngüleri ve işlevlerine genel bir bakış da dahil olmak üzere Service Fabric çoğaltmalar ve örnekler hakkında bilgi edinin.
author: appi101
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: aprameyr
ms.openlocfilehash: cf21af43de553a2802289e44eaece12952d077d3
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84710754"
---
# <a name="replicas-and-instances"></a>Çoğaltmalar ve örnekler 
Bu makalede, durum bilgisi olan hizmetlerin ve durum bilgisi olmayan hizmetlerin çoğaltmalarının yaşam döngüsüne genel bakış sunulmaktadır.

## <a name="instances-of-stateless-services"></a>Durum bilgisi olmayan hizmetlerin örnekleri
Durum bilgisi olmayan bir hizmetin örneği, kümenin düğümlerinden birinde çalışan hizmet mantığının bir kopyasıdır. Bölüm içindeki bir örnek, kendi **InstanceId**'si tarafından benzersiz şekilde tanımlanır. Bir örneğin yaşam döngüsü aşağıdaki diyagramda modellenmiştir:

![Örnek yaşam döngüsü](./media/service-fabric-concepts-replica-lifecycle/instance.png)

### <a name="inbuild-ib"></a>InBuild (ıB)
Küme Kaynak Yöneticisi örnek için bir yerleştirmeyi belirledikten sonra, bu yaşam döngüsü durumuna girer. Örnek, düğümde başlatılır. Uygulama ana bilgisayarı başlatılır, örnek oluşturulur ve sonra açılır. Başlatma bittikten sonra örnek, hazırlık durumuna geçer. 

Bu örneğin uygulama konağı veya düğümü kilitlenirse, bırakılan durumuna geçer.

### <a name="ready-rd"></a>Ready (RD)
Hazırlama durumunda, örnek düğümünde çalışır duruma gelir. Bu örnek güvenilir bir hizmettir, **RunAsync** çağırılır. 

Bu örneğin uygulama konağı veya düğümü kilitlenirse, bırakılan durumuna geçer.

### <a name="closing-cl"></a>Kapanış (CL)
Kapanış durumunda Azure Service Fabric, bu düğümdeki örneği kapatma sürecinde. Bu kapatılma birçok nedenden kaynaklanıyor olabilir. Örneğin, bir uygulama yükseltme, Yük Dengeleme veya silinmekte olan hizmet. Kapatmadan sonra bırakma durumuna geçiş yapar.

### <a name="dropped-dd"></a>Bırakıldı (gg)
Bırakılan durumda, örnek artık düğümde çalışmıyor. Bu noktada, Service Fabric Bu örnek hakkında, sonunda de Silinen meta verileri korur.

> [!NOTE]
> Üzerinde **ForceRemove** seçeneği kullanılarak herhangi bir durumdan bırakılan duruma geçiş yapılabilir `Remove-ServiceFabricReplica` .
>

## <a name="replicas-of-stateful-services"></a>Durum bilgisi olan hizmetlerin çoğaltmaları
Durum bilgisi olan bir hizmetin çoğaltması, kümenin düğümlerinden birinde çalışan hizmet mantığının bir kopyasıdır. Ayrıca, çoğaltma bu hizmetin durumunun bir kopyasını tutar. İki ilişkili kavram durum bilgisi olan çoğaltmaların yaşam döngüsünü ve davranışını anlatmaktadır:
- Çoğaltma yaşam döngüsü
- Çoğaltma rolü

Aşağıdaki tartışmada kalıcı durum bilgisi olan hizmetler açıklanmaktadır. Geçici (veya bellek içi) durum bilgisi olan hizmetler için, aşağı ve bırakılmış durumlar eşdeğerdir.

![Çoğaltma yaşam döngüsü](./media/service-fabric-concepts-replica-lifecycle/replica.png)

### <a name="inbuild-ib"></a>InBuild (ıB)
Bir InBuild çoğaltması, çoğaltma kümesini birleştirmek için oluşturulan veya hazırlanan bir yinelemedir. Çoğaltma rolüne bağlı olarak, ıB 'in farklı anlamları vardır. 

Uygulama konağı veya bir InBuild çoğaltması için düğüm kilitlenirse, aşağı durumuna geçer.

   - **Birincil InBuild çoğaltmaları**: birincil InBuild, bir bölümün ilk çoğaltmalardır. Bu çoğaltma genellikle bölüm oluşturulurken oluşur. Birincil InBuild çoğaltmaları, bir bölümün tüm çoğaltmaları yeniden başlatıldığında veya bırakıldığında da ortaya çıkar.

   - **Idde Ikincil InBuild çoğaltmaları**: Bunlar, küme kaynak yöneticisi tarafından oluşturulan yeni çoğaltmalarda veya kapatılmış olan ve kümeye geri eklenmesi gereken çoğaltmalardır. Bu çoğaltmalar, çoğaltma kümesine ActiveSecondary olarak katılabilmesi ve çekirdek işlem onaylama işlemlerine katılabilmesi için birincil tarafından gerçekleştirilir veya oluşturulmuştur.

   - **Activesecondary InBuild çoğaltmaları**: Bu durum bazı sorgularda gözlemlenmiştir. Bu, çoğaltma kümesinin değiştirilmemesinin, ancak çoğaltmanın oluşturulması gereken bir iyileştirmedir. Çoğaltma, normal durum makinesi geçişlerini (çoğaltma rollerinin bölümünde açıklandığı gibi) izler.

### <a name="ready-rd"></a>Ready (RD)
Hazırlama çoğaltması, işleme ve çekirdek onaylama işlemlerine katılan bir yinelemedir. Hazırlama durumu, birincil ve etkin ikincil çoğaltmalar için geçerlidir.

Uygulama konağı veya bir ready çoğaltma için düğüm kilitlenirse, bu durum aşağı doğru duruma geçer.

### <a name="closing-cl"></a>Kapanış (CL)
Bir çoğaltma, aşağıdaki senaryolarda kapatma durumunu girer:

- **Çoğaltma kodu kapatılıyor**: Service Fabric bir çoğaltma için çalışan kodu kapatması gerekebilir. Bu kapatılma birçok nedenden dolayı olabilir. Örneğin, bir uygulama, yapı veya altyapı yükseltmesi ya da çoğaltma tarafından bildirilen bir hata nedeniyle meydana gelebilir. Çoğaltma kapanışı tamamlandığında, çoğaltma Kapalı durumuna geçer. Diskte depolanan bu çoğaltmayla ilişkili kalıcı durum temizlenmez.

- **Çoğaltma kümeden kaldırılıyor**: Service Fabric kalıcı durumu kaldırmak ve bir çoğaltma için çalışan kodu kapatmak zorunda kalabilir. Bu kapatılma, örneğin yük dengelemesi gibi birçok nedenden dolayı olabilir.

### <a name="dropped-dd"></a>Bırakıldı (gg)
Bırakılan durumda, örnek artık düğümde çalışmıyor. Ayrıca düğüm üzerinde hiçbir durum kalmadı. Bu noktada, Service Fabric Bu örnek hakkında, sonunda de Silinen meta verileri korur.

### <a name="down-d"></a>Aşağı (D)
Aşağı durumda, çoğaltma kodu çalışmıyor, ancak söz konusu çoğaltmanın kalıcı durumu bu düğümde bulunuyor. Bir çoğaltma pek çok nedenden dolayı kapatılabilir. Örneğin, düğüm, çoğaltma kodunda bir kilitlenme, bir uygulama yükseltme veya çoğaltma hatası olabilir.

Aşağı bir çoğaltma Service Fabric tarafından, örneğin yükseltmenin düğüm üzerinde ne zaman bittiği gibi) açılır.

Çoğaltma rolü, aşağı durumunda ilgili değildir.

### <a name="opening-op"></a>Açılıyor (OP)
Aşağı bir çoğaltma, Service Fabric çoğaltmayı yeniden geri getirmek için gerektiğinde açma durumunu girer. Örneğin, bu durum, uygulama için bir kod yükseltmesinin bir düğümde sona erdikten sonra olabilir. 

Uygulama konağı veya bir açma çoğaltması için düğüm kilitlenirse, bu durum aşağı doğru duruma geçer.

Çoğaltma rolü, açma durumunda ilgili değildir.

### <a name="standby-sb"></a>Bekleme (SB)
Bekleme çoğaltması, kalıcı bir hizmetin kapatılmış ve daha sonra açtığı bir çoğaltmadır. Çoğaltma kümesine başka bir çoğaltma eklemesi gerekiyorsa, bu çoğaltma Service Fabric tarafından kullanılabilir (çoğaltma zaten durum kısmına sahip olduğundan ve yapı işlemi daha hızlı olduğundan). StandByReplicaKeepDuration süresi dolduktan sonra, bekleme çoğaltması atılır.

Uygulama konağı veya bir bekleme çoğaltması için düğüm kilitlenirse, bu durum aşağı doğru duruma geçer.

Çoğaltma rolü, bekleme durumunda uygun değildir.

> [!NOTE]
> Kapatılmış veya bırakılmış olmayan herhangi bir çoğaltma, olduğu kabul *edilir.*
>

> [!NOTE]
> Üzerinde **ForceRemove** seçeneği kullanılarak herhangi bir durumdan bırakılan duruma geçiş yapılabilir `Remove-ServiceFabricReplica` .
>

## <a name="replica-role"></a>Çoğaltma rolü 
Çoğaltma rolü, çoğaltma kümesindeki işlevini belirler:

- **Birincil (P)**: çoğaltma kümesinde okuma ve yazma işlemlerini gerçekleştirmekten sorumlu bir birincil vardır. 
- **Activesecondary**: Bunlar birincili durum güncelleştirmeleri alan, bunları uygulayan ve geri bildirimleri gönderen çoğaltmalardır. Çoğaltma kümesinde birden çok etkin ikincil vardır. Bu etkin ikincil nesnelerin sayısı hizmetin işleyebileceği hata sayısını belirler.
- **Idytali (ı)**: bu çoğaltmalar birincil tarafından oluşturuluyor. Bunlar, Active Secondary 'e yükseltibilmeleri için Birincilden durum alıyor. 
- **Hiçbiri (N)**: Bu çoğaltmaların çoğaltma kümesinde sorumluluğu yoktur.
- **Bilinmiyor (U)**: bu, Service Fabric bir **changerole** API çağrısı almadan önce bir çoğaltmanın ilk rolüdür.

Aşağıdaki diyagramda, çoğaltma rolü geçişleri ve bunların gerçekleşebileceği bazı örnek senaryolar gösterilmektedir:

![Çoğaltma rolü](./media/service-fabric-concepts-replica-lifecycle/role.png)

- U-> P: yeni birincil çoğaltma oluşturma.
- U-> ı: yeni bir boş çoğaltma oluşturma.
- U-> N: bir bekleme çoğaltmasını silme.
- I-> S: boşta kalan ikincil öğenin, bu özelliğin çekirdeğe katkıda bulunmak için etkin ikincil sürümüne yükseltilmesi.
- I-> P: boşta olan ikincili birinciye yükseltme. Bu, boşta kalan ikincil değer birincil olan doğru aday olduğunda özel yeniden yapılandırmalarda meydana gelebilir.
- I-> N: boşta kalan ikincil çoğaltmayı silme.
- S-> P: etkin ikincili birincili yükseltme. Bu, birincil veya küme Kaynak Yöneticisi tarafından başlatılan birincil bir hareketin yük devretmesinin nedeni olabilir. Örneğin, bir uygulama yükseltmesine veya yük dengelemeye yanıt olarak gelebilir.
- S-> N: etkin ikincil çoğaltmayı silme.
- P-> S: birincil çoğaltmanın Indirgenmesi. Bu, Küme Kaynak Yöneticisi tarafından başlatılan birincil bir hareketten kaynaklanıyor olabilir. Örneğin, bir uygulama yükseltmesine veya yük dengelemeye yanıt olarak gelebilir.
- P-> N: birincil çoğaltmayı silme.

> [!NOTE]
> [Reliable Actors](service-fabric-reliable-actors-introduction.md) ve [Reliable Services](service-fabric-reliable-services-introduction.md)gibi daha üst düzey programlama modelleri geliştiriciden çoğaltma rolü kavramını gizler. Aktörde rol kavramı gereksizdir. Hizmetler 'de çoğu senaryo için büyük ölçüde basitleştirilmiştir.
>

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

[Reliable Services yaşam döngüsü - C#](service-fabric-reliable-services-lifecycle.md)

