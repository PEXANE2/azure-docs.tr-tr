---
title: Güvenilir Aktörler zamanlayıcılar ve anımsatıcılar
description: Her birinin ne zaman kullanılacağına ilişkin rehberlik de dahil olmak üzere Servis Kumaşı Güvenilir Aktörler için zamanlayıcılara ve anımsatıcılara giriş.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 02d6220b31ee9c991e8450759bf46759af6177a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639624"
---
# <a name="actor-timers-and-reminders"></a>Aktör zamanlayıcıları ve anımsatıcılar
Aktörler zamanlayıcıları veya anımsatıcıları kaydederek periyodik çalışmaları kendileri üzerinde zamanlayabilirler. Bu makalede, zamanlayıcıların ve anımsatıcıların nasıl kullanılacağı gösterilmektedir ve aralarındaki farkları açıklar.

## <a name="actor-timers"></a>Aktör zamanlayıcılar
Aktör zamanlayıcılar, geri arama yöntemlerinin Aktörler çalışma süresinin sağladığı sıra tabanlı eşzamanlılık garantilerine uymasını sağlamak için bir .NET veya Java zamanlayıcısı etrafında basit bir sarmalayıcı sağlar.

Aktörler, zamanlayıcılarını kaydetmek `registerTimer`ve kayıt `UnregisterTimer`dışı kalmak `unregisterTimer`için taban sınıflarında `RegisterTimer`(C#) veya (Java) ve (Java) veya (Java) yöntemlerini kullanabilir. Aşağıdaki örnekte zamanlayıcı API'lerinin kullanımı gösterilmektedir. API'ler .NET zamanlayıcısına veya Java zamanlayıcısına çok benzer. Bu örnekte, zamanlayıcı dolduğunda, Aktörler çalışma zamanı `MoveObject`(C#) `moveObject`veya (Java) yöntemini çağırır. Yöntem, sıra tabanlı eşzamanlılık saygı garantilidir. Bu, bu geri çağırma yürütmetamamlanana kadar başka hiçbir aktör yöntemi veya zamanlayıcı/anımsatıcı geri araması devam edeceği anlamına gelir.

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

Zamanlayıcının sonraki dönemi, geri arama yürütmetamamlandıktan sonra başlar. Bu, geri arama yürütüldüğünde zamanlayıcının durdurulduğu ve geri arama bitince başlatıldığu anlamına gelir.

Aktörler çalışma süresi, geri arama bittiğinde aktörün Eyalet Yöneticisi'nde yapılan değişiklikleri kaydeder. Durumu kaydederken bir hata oluşursa, bu aktör nesnesi devre dışı bırakılır ve yeni bir örnek etkinleştirilir.

Aktör çöp toplama nın bir parçası olarak devre dışı bırakıldığında tüm zamanlayıcılar durdurulur. Bundan sonra zamanlayıcı geri çağırması yapılmaz. Ayrıca, Aktörler çalışma süresi devre dışı bırakmadan önce çalışan zamanlayıcılar hakkında herhangi bir bilgi saklamaz. Gelecekte yeniden etkinleştirildiğinde ihtiyacı olan zamanlayıcıları kaydetmek aktöre kalırken. Daha fazla bilgi için [aktör çöp toplama](service-fabric-reliable-actors-lifecycle.md)bölümüne bakın.

## <a name="actor-reminders"></a>Aktör anımsatıcıları
Anımsatıcılar, belirli zamanlarda bir aktör üzerinde kalıcı geri aramaları tetikleyen bir mekanizmadır. İşlevsellikleri zamanlayıcılara benzer. Ancak zamanlayıcıların aksine, anımsatıcılar her koşulda tetiklenir ta ki aktör bunları açıkça silinene veya aktör açıkça silinene kadar. Özellikle, aktörler çalışma süresi aktör devlet sağlayıcısı nı kullanarak aktörün anımsatıcıları hakkında bilgi kalıcı olduğundan, anımsatıcılar aktör devre dışı bırakma ve başarısız lıklar arasında tetiklenir. Anımsatıcıların güvenilirliğinin aktör devlet sağlayıcısı tarafından sağlanan devlet güvenilirlik garantilerine bağlı olduğunu lütfen unutmayın. Bu, durum kalıcılığı Yok olarak ayarlanmış aktörler için, anımsatıcılar bir başarısızlık tan sonra ateş olmaz anlamına gelir. 

Bir anımsatıcı yı kaydetmek `RegisterReminderAsync` için, bir aktör aşağıdaki örnekte gösterildiği gibi taban sınıfta sağlanan yöntemi çağırır:

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

Bu örnekte, `"Pay cell phone bill"` anımsatıcı adıdır. Bu, aktörün bir anımsatıcıyı benzersiz olarak tanımlamak için kullandığı bir dizedir. `BitConverter.GetBytes(amountInDollars)`(C#) anımsatıcıyla ilişkili bağlamdır. Anımsatıcı geri çağırmasına, yani `IRemindable.ReceiveReminderAsync`(C#) veya `Remindable.receiveReminderAsync`(Java) bir argüman olarak aktöre geri geçirilir.

Anımsatıcılar kullanan aktörler, `IRemindable` aşağıdaki örnekte gösterildiği gibi arabirimi uygulamalıdır.

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

Bir anımsatıcı tetiklendiğinde, Güvenilir Aktörler çalışma `ReceiveReminderAsync`süresi Aktör'de (C#) veya `receiveReminderAsync`(Java) yöntemini çağırır. Bir aktör birden çok anımsatıcı kaydedebilir ve bu anımsatıcılardan herhangi biri tetiklendiğinde `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) yöntemi çağrılır. Aktör, hangi anımsatıcının tetiklediğini `ReceiveReminderAsync`anlamak için (C#) veya `receiveReminderAsync`(Java) yöntemine geçirilen anımsatıcı adını kullanabilir.

(C#) veya `ReceiveReminderAsync` `receiveReminderAsync`(Java) çağrısı sona erdiğinde Aktörler çalışma süresi aktörün durumunu kaydeder. Durumu kaydederken bir hata oluşursa, bu aktör nesnesi devre dışı bırakılır ve yeni bir örnek etkinleştirilir.

Bir anımsatıcının kaydını `UnregisterReminderAsync`çıkarmak için, `unregisterReminderAsync`bir aktör aşağıdaki örneklerde gösterildiği gibi (C#) veya (Java) yöntemini çağırır.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

Yukarıda gösterildiği `UnregisterReminderAsync`gibi, (C#) veya `unregisterReminderAsync`(Java) `IActorReminder`yöntemi (C#) veya `ActorReminder`(Java) arabirimi kabul eder. Aktör taban sınıfı, `GetReminder`anımsatıcı `getReminder`adını geçerek `IActorReminder`(C#) veya (Java) `ActorReminder`arabirimini almak için kullanılabilecek bir (C#) veya (Java) yöntemini destekler. Bu, `IActorReminder`aktörün `ActorReminder` `RegisterReminder`(C#) veya (Java) yöntemi çağrısından döndürülen (C#) veya `registerReminder`(Java) arabirimini devam etmesi gerekmedığından kullanışlıdır.

## <a name="next-steps"></a>Sonraki Adımlar
Güvenilir Aktör etkinlikleri ve reentrancy hakkında bilgi edinin:
* [Aktör etkinlikleri](service-fabric-reliable-actors-events.md)
* [Aktör reentrancy](service-fabric-reliable-actors-reentrancy.md)
