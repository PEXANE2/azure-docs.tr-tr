---
title: Azure Hizmet Kumaşı aktör yaşam döngüsüne genel bakış
description: Hizmet Kumaş Güvenilir Aktör yaşam döngüsü, çöp toplama açıklar ve el ile aktörler ve devlet silme
author: amanbha
ms.topic: conceptual
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: b05da78091260297d94062c06cba100d01ce7e2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258323"
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Aktör yaşam döngüsü, otomatik çöp toplama ve manuel silme
Bir aktör, yöntemlerinden herhangi biri için ilk kez arama yapıldığında etkinleştirilir. Bir aktör devre dışı bırakılır (aktörler tarafından toplanan çöp ler) eğer yapılandırılabilir bir süre için kullanılmaz. Bir aktör ve durumu da herhangi bir zamanda el ile silinebilir.

## <a name="actor-activation"></a>Aktör etkinleştirme
Bir aktör etkinleştirildiğinde, aşağıdakiler oluşur:

* Bir aktör için bir çağrı geldiğinde ve bir zaten etkin olmadığında, yeni bir aktör oluşturulur.
* Eğer durumu koruyorsa aktörün durumu yüklenir.
* `OnActivateAsync` (C#) veya `onActivateAsync` (Java) yöntemi (aktör uygulamasında geçersiz kılınabilir) denir.
* Aktör şimdi aktif olarak kabul edilir.

## <a name="actor-deactivation"></a>Aktör devre dışı bırakma
Bir aktör devre dışı bırakıldığında, aşağıdakiler oluşur:

* Bir aktör belirli bir süre kullanılmadığında, Etkin Aktörler tablosundan kaldırılır.
* `OnDeactivateAsync` (C#) veya `onDeactivateAsync` (Java) yöntemi (aktör uygulamasında geçersiz kılınabilir) denir. Bu aktör için tüm zamanlayıcıları temizler. Durum değişiklikleri gibi aktör işlemleri bu yöntemden çağrılmamalıdır.

> [!TIP]
> Kumaş Aktörler çalışma zamanı [aktör etkinleştirme ve devre dışı bırakma ile ilgili](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters)bazı olaylar yayır. Tanılama ve performans izlemede yararlıdırlar.
>
>

### <a name="actor-garbage-collection"></a>Aktör çöp toplama
Bir aktör devre dışı bırakıldığında, aktör nesnesine başvurular serbest bırakılır ve ortak dil çalışma süresi (CLR) veya java sanal makine (JVM) çöp toplayıcısı tarafından normal olarak toplanabilir. Çöp toplama yalnızca aktör nesnesini temizler; aktörün Devlet Müdürü'nde depolanan durumu **kaldırmaz.** Aktör bir sonraki etkinleştirildiğinde, yeni bir aktör nesnesi oluşturulur ve durumu geri yüklenir.

Devre dışı bırakma ve çöp toplama amacıyla "kullanılıyor" olarak ne sayılır?

* Arama alma
* `IRemindable.ReceiveReminderAsync`yöntem çağrılmakta (yalnızca aktör anımsatıcılar kullanıyorsa geçerlidir)

> [!NOTE]
> aktör zamanlayıcı kullanır ve zamanlayıcı geri çağrılması çağrılırsa, "kullanılıyor" olarak **sayılmaz.**
>
>

Devre dışı bırakma nın ayrıntılarına girmeden önce aşağıdaki terimleri tanımlamak önemlidir:

* *Tara aralığı.* Bu, Aktörler'in çalışma zamanının devre dışı bırakılabilen ve çöp toplayabilen aktörler için Etkin Aktörler tablosunu taradığı aralıktır. Bunun varsayılan değeri 1 dakikadır.
* *Boşta zaman ası*. Bu, bir aktörün devre dışı bırakılabilmesi ve çöp lerin toplanabilmesi için kullanılmaması (boşta) kalması gereken süredir. Bunun varsayılan değeri 60 dakikadır.

Genellikle, bu varsayılanları değiştirmeniz gerekmez. Ancak, gerekirse, bu aralıklar Aktör `ActorServiceSettings` [Hizmeti](service-fabric-reliable-actors-platform.md)kaydederken değiştirilebilir:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
    }
}
```
Her etkin aktör için, aktör çalışma süresi boşta olan (yani kullanılmamış) süreyi izler. Aktör çalışma süresi, çöp toplanıp toplanamayacağı her bir aktörü `ScanIntervalInSeconds` kontrol eder ve `IdleTimeoutInSeconds`boşta olup olmadığını işaretler.

Bir aktör her kullanıldığında, boşta kalma süresi 0'a sıfırlanır. Bundan sonra, aktör çöp sadece tekrar için `IdleTimeoutInSeconds`boşta kalırsa toplanabilir. Bir aktör arabirimi yöntemi veya bir aktör anımsatıcı geri çağırma yürütülür s. Zamanlayıcı geri arama yürütülür sayılsa, aktör kullanılmış olarak kabul **edilmez.**

Aşağıdaki diyagram, bu kavramları göstermek için tek bir aktörün yaşam döngüsünü gösterir.

![Boşta zaman örneği][1]

Örnek, aktör yöntemi çağrılarının, anımsatıcıların ve zamanlayıcıların bu aktörün ömrü üzerindeki etkisini gösterir. Örnekle ilgili aşağıdaki hususlar belirtilmede değerlidir:

* ScanInterval ve IdleTimeout sırasıyla 5 ve 10 olarak ayarlanır. (Birimler burada önemli değil, çünkü amacımız sadece kavramı göstermektir.)
* Aktörlerin çöp olarak toplanması için tarar, 5'in tarar aralığına göre tanımlanan T=0,5,10,15,20,25'te gerçekleşir.
* Periyodik zamanlayıcı T=4,8,12,16,20,24'te ateş eder ve geri arama yürütülür. Aktörün boş zamanlarını etkilemez.
* T=7'deki bir aktör yöntemi boşta kalma süresini 0'a ayarlar ve aktörün çöp toplamasını geciktirir.
* Bir aktör anımsatıcı geri çağırma T =14'te yürütülür ve aktörün çöp toplamasını daha da geciktirir.
* T=25'teki çöp toplama tonu sırasında aktörün boşta kalma süresi sonunda 10'un boşzaman süresini aşar ve aktör çöp toplanır.

Bir aktör, bu yöntemin yürütülmesi nde ne kadar zaman harcanmış olursa olsun, yöntemlerinden birini uygularken asla çöp olarak toplanmaz. Daha önce de belirtildiği gibi, aktör arabirimi yöntemlerinin ve anımsatıcı geri aramalarının yürütülmesi, aktörün boşta kalma süresini 0'a sıfırlayarak çöp toplamayı önler. Zamanlayıcı geri aramalarının yürütülmesi boşta kalma süresini 0'a sıfırlamaz. Ancak, zamanlayıcı geri arama yürütme tamamlanana kadar aktörün çöp toplama ertelenir.

## <a name="manually-deleting-actors-and-their-state"></a>Aktörleri ve durumlarını el ile silme
Devre dışı bırakılan aktörlerin çöp toplaması yalnızca aktör nesnesini temizler, ancak bir aktörün Durum Yöneticisi'nde depolanan verileri kaldırmaz. Bir aktör yeniden etkinleştirildiğinde, verileri yine Devlet Yöneticisi aracılığıyla kullanılabilir hale getirilir. Aktörlerin verileri Devlet Yöneticisi'nde depoladığı ve devre dışı bırakıldığı ancak yeniden etkinleştirilmediği durumlarda, verilerini temizlemek gerekebilir.  Aktörlerin nasıl silinene ne kadar iyi örnekler için, [silme aktörlerini ve durumlarını](service-fabric-reliable-actors-delete-actors.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar
* [Aktör zamanlayıcıları ve anımsatıcılar](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör etkinlikleri](service-fabric-reliable-actors-events.md)
* [Aktör reentrancy](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# Örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java Örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
