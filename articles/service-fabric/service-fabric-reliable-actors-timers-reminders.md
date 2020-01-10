---
title: Reliable Actors zamanlayıcılar ve anımsatıcılar
description: Her birinin ne zaman kullanılacağı hakkında rehberlik dahil olmak üzere Service Fabric Reliable Actors için zamanlayıcılar ve anımsatıcılara giriş.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 02d6220b31ee9c991e8450759bf46759af6177a3
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639624"
---
# <a name="actor-timers-and-reminders"></a>Aktör zamanlayıcılar ve anımsatıcıları
Aktör, zamanlayıcılar veya anımsatıcılar kaydederek kendi üzerinde düzenli işler zamanlayabilir. Bu makalede, zamanlayıcılar ve anımsatıcıların nasıl kullanılacağı gösterilir ve aralarındaki farklar açıklanmaktadır.

## <a name="actor-timers"></a>Aktör zamanlayıcılar
Aktör zamanlayıcılar, aktör çalışma zamanının sağladığı, geri çağırma yöntemlerinin, çift yönlü eşzamanlılık garantisi sağladığından emin olmak için bir .NET veya Java süreölçerinin etrafında basit bir sarmalayıcı sağlar.

Aktör, zamanlayıcılarını kaydetmek veC#kaydını silmek için temel sınıfında `RegisterTimer`() veyaC#`registerTimer`(Java) ve `UnregisterTimer`() veya `unregisterTimer`(Java) yöntemlerini kullanabilir. Aşağıdaki örnekte Zamanlayıcı API 'Lerinin kullanımı gösterilmektedir. API 'Ler .NET Zamanlayıcı veya Java zamanlayıcıya çok benzerdir. Bu örnekte, Zamanlayıcının süresi dolduğunda, aktör çalışma zamanı `MoveObject`(C#) veya `moveObject`(Java) yöntemini çağırır. Yöntemi, çift tabanlı eşzamanlılık açısından garanti edilir. Yani bu geri çağırma işlemi tamamlanana kadar başka bir aktör yöntemi veya zamanlayıcı/anımsatıcı geri çağırma işlemleri yapılmaz.

```csharp
class VisualObjectActor : Actor, IVisualObject
{
    private IActorTimer _updateTimer;

    public VisualObjectActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // Callback method
            null,                           // Parameter to pass to the callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time to delay before the callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of the callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```
```Java
public class VisualObjectActorImpl extends FabricActor implements VisualObjectActor
{
    private ActorTimer updateTimer;

    public VisualObjectActorImpl(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    @Override
    protected CompletableFuture onActivateAsync()
    {
        ...

        return this.stateManager()
                .getOrAddStateAsync(
                        stateName,
                        VisualObject.createRandom(
                                this.getId().toString(),
                                new Random(this.getId().toString().hashCode())))
                .thenApply((r) -> {
                    this.registerTimer(
                            (o) -> this.moveObject(o),                        // Callback method
                            "moveObject",
                            null,                                             // Parameter to pass to the callback method
                            Duration.ofMillis(10),                            // Amount of time to delay before the callback is invoked
                            Duration.ofMillis(timerIntervalInMilliSeconds));  // Time interval between invocations of the callback method
                    return null;
                });
    }

    @Override
    protected CompletableFuture onDeactivateAsync()
    {
        if (updateTimer != null)
        {
            unregisterTimer(updateTimer);
        }

        return super.onDeactivateAsync();
    }

    private CompletableFuture moveObject(Object state)
    {
        ...
        return this.stateManager().getStateAsync(this.stateName).thenCompose(v -> {
            VisualObject v1 = (VisualObject)v;
            v1.move();
            return (CompletableFuture<?>)this.stateManager().setStateAsync(stateName, v1).
                    thenApply(r -> {
                      ...
                      return null;});
        });
    }
}
```

Süreölçerin bir sonraki dönemi, geri çağırma yürütmeyi tamamladıktan sonra başlar. Bu, geri çağırma yürütülürken zamanlayıcının durdurulduğu ve geri çağırma tamamlandığında başlatıldığı anlamına gelir.

Aktör çalışma zamanı, geri çağırma tamamlandığında aktörün durum yöneticisine yapılan değişiklikleri kaydeder. Durumu kaydederken bir hata oluşursa, bu aktör nesnesi devre dışı bırakılır ve yeni bir örnek etkinleştirilir.

Aktör çöp toplama işleminin parçası olarak devre dışı bırakıldığında tüm zamanlayıcılar durdurulur. Sonrasında hiçbir süreölçer geri çağırma çağrılmaz. Ayrıca, aktör çalışma zamanı devre dışı bırakmadan önce çalışmakta olan zamanlayıcılar hakkındaki bilgileri korumaz. Bu, gelecekte yeniden etkinleştirildiğinde ihtiyacı olan tüm zamanlayıcıları kaydetmek için aktöre kadar. Daha fazla bilgi için [aktör çöp toplamanın](service-fabric-reliable-actors-lifecycle.md)bölümüne bakın.

## <a name="actor-reminders"></a>Aktör anımsatıcıları
Anımsatıcılar, bir aktör üzerinde belirtilen zamanlarda kalıcı geri çağırmaları tetiklemeye yönelik bir mekanizmadır. İşlevleri zamanlayıcılar ile benzerdir. Ancak zamanlayıcılar aksine, aktör açıkça silinmediği veya aktör açıkça silinene kadar tüm koşullarda anımsatıcılar tetiklenir. Özellikle de, aktör çalışma zamanı aktör durumu sağlayıcısı kullanarak aktörün anımsatıcıları hakkında bilgi sağladığından, bir aktör ve yük devretmeler genelinde uyarı tetiklenir. Lütfen anımsatıcıların güvenilirliğini aktör durumu sağlayıcısı tarafından sunulan durum güvenilirliği garantisi 'na bağlı olduğunu unutmayın. Bu, durum kalıcılığı yok olarak ayarlanmış aktörler için, bir yük devretmeden sonra, anımsatıcıların tetikleneceği anlamına gelir. 

Bir bir anımsatıcıyı kaydetmek için bir aktör, aşağıdaki örnekte gösterildiği gibi temel sınıfta verilen `RegisterReminderAsync` yöntemini çağırır:

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),    //The amount of time to delay before firing the reminder
        TimeSpan.FromDays(1));    //The time interval between firing of reminders
}
```

```Java
@Override
protected CompletableFuture onActivateAsync()
{
    String reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    ActorReminder reminderRegistration = this.registerReminderAsync(
            reminderName,
            state,
            dueTime,    //The amount of time to delay before firing the reminder
            period);    //The time interval between firing of reminders
}
```

Bu örnekte, `"Pay cell phone bill"` anımsatıcı adıdır. Bu, aktörün bir anımsatıcıyı benzersiz şekilde tanımlamak için kullandığı bir dizedir. `BitConverter.GetBytes(amountInDollars)`(C#), anımsatıcı ile ilişkili bağlamdır. Anımsatıcı geri çağırması için bir bağımsız değişken olarak aktör 'e geçirilir, yani `IRemindable.ReceiveReminderAsync`(C#) veya `Remindable.receiveReminderAsync`(Java).

Anımsatıcıları kullanan aktörlerin, aşağıdaki örnekte gösterildiği gibi `IRemindable` arabirimini uygulaması gerekir.

```csharp
public class ToDoListActor : Actor, IToDoListActor, IRemindable
{
    public ToDoListActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```
```Java
public class ToDoListActorImpl extends FabricActor implements ToDoListActor, Remindable
{
    public ToDoListActor(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture receiveReminderAsync(String reminderName, byte[] context, Duration dueTime, Duration period)
    {
        if (reminderName.equals("Pay cell phone bill"))
        {
            int amountToPay = ByteBuffer.wrap(context).getInt();
            System.out.println("Please pay your cell phone bill of " + amountToPay);
        }
        return CompletableFuture.completedFuture(true);
    }

```

Bir anımsatıcı tetiklendiğinde, Reliable Actors çalışma zamanı aktör üzerinde `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) yöntemini çağırır. Aktör birden çok anımsatıcıyı kaydedebilir ve bu anımsatıcılar tetiklendiğinde `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) yöntemi çağrılır. Aktör, hangi anımsatıcının tetiklendiğini anlamak için `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) yöntemine geçirilen anımsatıcı adını kullanabilir.

Aktör çalışma zamanı, `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) çağrısı tamamlandığında aktör durumunu kaydeder. Durumu kaydederken bir hata oluşursa, bu aktör nesnesi devre dışı bırakılır ve yeni bir örnek etkinleştirilir.

Bir anımsatıcının kaydını silmek için, bir aktör aşağıdaki örneklerde gösterildiğiC#gibi `UnregisterReminderAsync`() veya `unregisterReminderAsync`(Java) yöntemini çağırır.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

Yukarıda gösterildiği gibi, `UnregisterReminderAsync`(C#) veya `unregisterReminderAsync`(Java) yöntemi bir `IActorReminder`(C#) veya `ActorReminder`(Java) arabirimini kabul eder. Aktör temel sınıfı, anımsatıcı adına geçirerek `IActorReminder`C#(C#) veya `ActorReminder`(Java) arabirimini almak için kullanılabilecek bir `GetReminder`() veya `getReminder`(Java) yöntemini destekler. Bu, aktörün `RegisterReminder`(C#C#) veya `registerReminder`(Java) yöntem çağrısından döndürülen `IActorReminder`() veya `ActorReminder`(Java) arabirimini kalıcı hale getirmek zorunda olmadığı için kullanışlıdır.

## <a name="next-steps"></a>Sonraki Adımlar
Güvenilir aktör olayları ve yeniden giriş hakkında bilgi edinin:
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör yeniden girişi](service-fabric-reliable-actors-reentrancy.md)
