---
title: Kapsayıcılar sistem durumu monitörleri yapılandırması için Azure Monitörü | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor'daki sistem durumu monitörlerinin ayrıntılı yapılandırmasını açıklayan içerik sağlanmaktadır.
ms.topic: conceptual
ms.date: 12/01/2019
ms.openlocfilehash: 99ea6e96f5a8a486784cb3d633a6e031b60eaad7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055713"
---
# <a name="azure-monitor-for-containers-health-monitor-configuration-guide"></a>Kapsayıcılar için Azure Monitörü sistem durumu izleme yapılandırma kılavuzu

Monitörler, kapsayıcılar için Azure Monitor'daki sistem durumunu ölçmenin ve hataları algılamanın birincil öğesidir. Bu makale, sağlık durumunun nasıl ölçüldüğü ve [Sağlık (önizleme)](container-insights-health.md) özelliğiyle Kubernetes kümenizin sistem durumunu izlemek ve raporlamak için sistem durumu modelini oluşturan öğeleri anlamanıza yardımcı olur.

>[!NOTE]
>Sistem Durumu özelliği şu anda genel önizlemede.
>

## <a name="monitors"></a>İzleyiciler

İzleyici, yönetilen bir nesnenin bazı yönlerinin sağlığını ölçer. İzleyicilerin her biri iki veya üç sistem durumuna sahiptir. Bir izleyici belirli bir zamanda potansiyel durumlarının sadece birinde olabilir. Kapsayıcı aracı tarafından yüklenen bir monitör, sağlıklı bir duruma başharfe çevrilir. Durum yalnızca başka bir durum için belirtilen koşullar algılanırsa değişir.

Belirli bir nesnenin genel sistem durumu sahip olduğu izleyicilerinin her birinin sistem durumundan yararlanılarak belirlenir. Bu hiyerarşi, kapsayıcılar için Azure Monitor'daki Sistem Durumu Hiyerarşisi bölmesinde gösterilmiştir. Sistem durumunun nasıl yuvarlandığıyla ilgili ilke, toplu monitörlerin yapılandırmasının bir parçasıdır.

## <a name="types-of-monitors"></a>Monitör türleri

|İzleme | Açıklama | 
|--------|-------------|
| Birim monitörü |Birim izleme, kaynağın veya uygulamanın bazı yönünü ölçer. Bu, kaynağın performansını veya kullanılabilirliğini belirlemek için bir performans sayacını denetler. |
|Toplam Monitör | Agrega, tek bir sağlık toplu sağlık durumu sağlamak için birden çok monitör grubunu izler. Birim monitörler genellikle belirli bir toplu monitör altında yapılandırılır. Örneğin, bir Düğüm toplam monitörü Düğüm CPU kullanımı, bellek kullanımı ve Düğüm durumunun durumunu yukarı yuvarlar.
 |

### <a name="aggregate-monitor-health-rollup-policy"></a>Toplu izleme durumu toplama ilkesi

Her bir toplu monitör, altındaki monitörlerin durumunu temel alan toplam monitörün durumunu belirlemek için kullanılan mantık olan bir sistem durumu toplama ilkesi tanımlar. Toplu bir monitör için olası sistem durumu toplama ilkeleri aşağıdaki gibidir:

#### <a name="worst-state-policy"></a>En kötü devlet ilkesi

Toplu izlemenin durumu, çocuk monitörünün durumuyla en kötü sağlık durumuyla eşleşir. Bu, toplu izleyenler tarafından kullanılan en yaygın ilkedir.

![Toplu monitör toplama en kötü durum örneği](./media/container-insights-health-monitoring-cfg/aggregate-monitor-rollup-worstof.png)

### <a name="percentage-policy"></a>Yüzde ilkesi

Kaynak nesne, en iyi durumda hedef nesnelerin belirli bir yüzdesi tek bir üyenin en kötü durumu eşleşir. Bu ilke, hedef nesnenin sağlıklı kabul edilemesi için hedef nesnelerin belirli bir yüzdesinin sağlıklı olması gerektiğinde kullanılır. Yüzde ilkesi monitörleri durumun önem derecesini azalan sırada sıralar ve toplam monitörün durumu En kötü N%durumu olarak hesaplanır (N yapılandırma parametresi *StateThresholdPercentage*tarafından belirlenir).

Örneğin, bir kapsayıcı görüntünün beş kapsayıcı örneği olduğunu varsayalım ve bunların tek tek durumları **Kritik**, **Uyarı**, **Sağlıklı**, **Sağlıklı**, **Sağlıklı**.  Kapsayıcı CPU kullanım monitörünün durumu **Kritik**olacaktır , çünkü kapsayıcıların %90'ının en kötü durumu önem inen önem sırasına göre sıralandığında **kritiktir.**

## <a name="understand-the-monitoring-configuration"></a>İzleme yapılandırmasını anlama

Kapsayıcılar için Azure Monitor, aşağıdaki gibi yapılandırılan bir dizi önemli izleme senaryosu içerir.

### <a name="unit-monitors"></a>Birim monitörler

|**Monitör adı** | Monitör türü | **Açıklama** | **Parametre** | **Değer** |
|-----------------|--------------|-----------------|---------------|-----------|
|Düğüm Bellek Kullanımı |Birim monitörü |Bu monitör, cadvisor bildirilen verileri kullanarak her dakika bir düğümün bellek kullanımını değerlendirir. |Ardışık ÖrneklerForStateTransition<br> Failifgreaterthanpercentage<br> Warnifgreaterthanpercentage | 3<br> 90<br> 80  ||
|Düğüm CPU Kullanımı |Birim Monitörü |Bu monitör, cadvisor bildirilen verileri kullanarak düğümün CPU kullanımını her dakika denetler. | Ardışık ÖrneklerForStateTransition<br> Failifgreaterthanpercentage<br> Warnifgreaterthanpercentage | 3<br> 90<br> 80  ||
|Düğüm Durumu |Birim monitörü |Bu monitör, Kubernetes tarafından bildirilen düğüm koşullarını denetler.<br> Şu anda aşağıdaki düğüm koşulları kontrol edilir: Disk Basıncı, Bellek Basıncı, PID Basıncı, Disk Dışında, Ağ kullanılamıyor, düğüm için hazır durum.<br> Yukarıdaki koşullar dışında, Disk veya *Ağ* *Kullanılamaz* dışında **doğruysa,** monitör **Kritik** durumuna değişir.<br> **Hazır** durumu dışında başka koşullar **da doğruysa,** monitör **Uyarı** durumuna dönüşür. | NodeConditionTypeForFailedState | outofdisk,networkavailable ||
|Konteyner bellek kullanımı |Birim monitörü |Bu monitör, kapsayıcı örneklerinin Bellek kullanımının (RSS) sistem durumu durumunu raporeder.<br> Her örneği tek bir eşikle karşılaştıran ve Configuration parametresi Tarafından belirlenen basit bir karşılaştırma **gerçekleştirirArdIsakÖrneklerForStateTransition**yapılandırması.<br> Durumu, kapsayıcı nın (StateThresholdPercentage) örneklerinin %90'ının en kötü durumu olarak hesaplanır ve kapsayıcı sağlık durumunun (yani Kritik, Uyarı, Sağlıklı) azalan önem sırasına göre sıralanır.<br> Bir kapsayıcı örneğinden kayıt alınmazsa, kapsayıcı örneğinin sistem durumu **Bilinmiyor**olarak bildirilir ve **Kritik** durum üzerinde sıralama sırasına göre daha yüksek önceliğe sahiptir.<br> Her bir kapsayıcı örneğinin durumu yapılandırmada belirtilen eşikler kullanılarak hesaplanır. Kullanım kritik eşiğin (%90) üzerindeyse, kritik eşikten daha azsa (%90) örnek **Kritik** durumdadır ancak uyarı eşiğinin (%80) üzerinde, o zaman örnek bir **Uyarı** durumundadır. Aksi takdirde, **Sağlıklı** durumdadır. |Ardışık ÖrneklerForStateTransition<br> FailiflessThanPercentage<br> Durum Eşik Yüzdesi<br> Warnifgreaterthanpercentage| 3<br> 90<br> 90<br> 80 ||
|Konteyner CPU kullanımı |Birim monitörü |Bu monitör, kapsayıcı örneklerinin CPU kullanımının sistem durumu durumunu birleştirdi.<br> Her örneği tek bir eşikle karşılaştıran ve Configuration parametresi Tarafından belirlenen basit bir karşılaştırma **gerçekleştirirArdIsakÖrneklerForStateTransition**yapılandırması.<br> Durumu, kapsayıcı nın (StateThresholdPercentage) örneklerinin %90'ının en kötü durumu olarak hesaplanır ve kapsayıcı sağlık durumunun (yani Kritik, Uyarı, Sağlıklı) azalan önem sırasına göre sıralanır.<br> Bir kapsayıcı örneğinden kayıt alınmazsa, kapsayıcı örneğinin sistem durumu **Bilinmiyor**olarak bildirilir ve **Kritik** durum üzerinde sıralama sırasına göre daha yüksek önceliğe sahiptir.<br> Her bir kapsayıcı örneğinin durumu yapılandırmada belirtilen eşikler kullanılarak hesaplanır. Kullanım kritik eşiğin (%90) üzerindeyse, kritik eşikten daha azsa (%90) örnek **Kritik** durumdadır ancak uyarı eşiğinin (%80) üzerinde, o zaman örnek bir **Uyarı** durumundadır. Aksi takdirde, **Sağlıklı** durumdadır. |Ardışık ÖrneklerForStateTransition<br> FailiflessThanPercentage<br> Durum Eşik Yüzdesi<br> Warnifgreaterthanpercentage| 3<br> 90<br> 90<br> 80 ||
|Sistem iş yükü bölmeleri hazır |Birim monitörü |Bu izleme, belirli bir iş yükündeki hazır durumdaki bölmelerin yüzdesini temel alan durumu raporlar. Kapsüllerin %100'ünden azı **Sağlıklı** bir durumdaysa durumu **Kritik** olarak ayarlanır |Ardışık ÖrneklerForStateTransition<br> FailiflessThanPercentage |2<br> 100 ||
|Kube API durumu |Birim monitörü |Bu monitör, Kube API hizmetinin durumunu bildirir. Kube API bitiş noktası nın kullanılamaması durumunda monitör kritik durumdadır. Bu özel monitör için durum, kube-api sunucusu için 'düğümler' bitiş noktasına bir sorgu yaparak belirlenir. Ok yanıt kodu dışında herhangi bir şey, monitörü **Kritik** duruma değiştirir. | Yapılandırma özelliği yok |||

### <a name="aggregate-monitors"></a>Agrega monitörler

|**Monitör adı** | **Açıklama** | **Algoritma** |
|-----------------|-----------------|---------------|
|Node |Bu monitör tüm düğüm monitörlerinin toplamıdır. En kötü sağlık durumu ile çocuk monitörünün durumu eşleşir:<br> Düğüm CPU kullanımı<br> Düğüm bellek kullanımı<br> Düğüm Durumu | En kötü|
|Düğüm havuzu |Bu monitör, düğüm havuzu *aracı havuzundaki*tüm düğümlerin birleştirilmiş sistem durumunu bildirir. Bu durum düğüm havuzunda düğümlerin% 80 en kötü durumuna dayalı olan üç devlet monitör, düğüm durumların şiddeti azalan sırayla sıralanır (yani, Kritik, Uyarı, Sağlıklı).|Yüzde |
|Düğümler (Düğüm havuzunun üst öğesi) |Bu, tüm düğüm havuzlarının toplam monitörüdür. Durumu, alt monitörlerinin en kötü durumuna (diğer bir deyişle kümede bulunan düğüm havuzlarına) dayanır. |En kötü |
|Küme (düğümlerin üst öğesi/<br> Kubernetes altyapısı) |Bu, çocuk monitörünün durumuyla en kötü sağlık durumuyla, yani kubernetes altyapısı ve düğümleriyle eşleşen üst monitördür. |En kötü |
|Kubernetes altyapısı |Bu, kümenin yönetilen altyapı bileşenlerinin sistem durumu durumunu birleştirdi. durumu, çocuk monitörü durumları yani kube-sistem iş yükleri ve API Server durumu 'en kötü' olarak hesaplanır. |En kötü|
|Sistem iş yükü |Bu monitör, kube-sistem iş yükünün sistem durumunu bildirir. Bu monitör, çocuk monitörünün durumuyla en kötü sağlık durumuyla, yani hazır durumdaki Podlarla (monitör ve iş yükündeki **kapsayıcılar)** eşleşir. |En kötü |
|Kapsayıcı |Bu, belirli bir iş yükünde bir kapsayıcının genel sağlık durumunu bildirir. Bu monitör, en kötü sağlık durumu ile çocuk monitörün durumu eşleşir, cpu **kullanımı** ve **Bellek kullanımı** monitörleri. |En kötü |

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes kümenizin sistem durumu durumunu görüntüleme hakkında bilgi edinmek için [monitör küme durumunu](container-insights-health.md) görüntüleyin.
