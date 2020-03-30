---
title: Güvenilir Hizmetler bildirimleri
description: Güvenilir Devlet Yöneticisi ve Güvenilir Sözlük için Hizmet Kumaş Güvenilir Hizmetler bildirimleri için kavramsal dokümantasyon
author: mcoskun
ms.topic: conceptual
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: 1f3239ea1da252ccd84c6572b562756c8fd1677d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639573"
---
# <a name="reliable-services-notifications"></a>Güvenilir Hizmetler bildirimleri
Bildirimler, istemcilerin ilgilendikleri bir nesnede yapılan değişiklikleri izlemesine olanak sağlar. İki tür nesne bildirimleri destekler: *Güvenilir Durum Yöneticisi* ve Güvenilir *Sözlük.*

Bildirimleri kullanmanın yaygın nedenleri şunlardır:

* İkincil dizinler veya yinelemenin durumunun birleştirilmiş filtrelenmiş görünümleri gibi önemlileştirilmiş görünümler oluşturma. Buna örnek, Güvenilir Sözlük'teki tüm anahtarların sıralanmış dizinidir.
* Son bir saat içinde eklenen kullanıcı sayısı gibi izleme verileri gönderme.

Bildirimler, uygulama işlemlerinin bir parçası olarak ateşlenir. Bu nedenle, bildirimler mümkün olduğunca hızlı işlenmeli ve eşzamanlı olaylar pahalı işlemleri içermemelidir.

## <a name="reliable-state-manager-notifications"></a>Güvenilir Durum Yöneticisi bildirimleri
Güvenilir Durum Yöneticisi aşağıdaki olaylar için bildirimler sağlar:

* İşlem
  * İşleme
* Eyalet müdürü
  * Yeniden derleme
  * Güvenilir bir durum eklenmesi
  * Güvenilir bir devletin kaldırılması

Güvenilir Durum Yöneticisi, geçerli uçak içi işlemleri izler. Bildirimin atılmasına neden olan işlem durumundaki tek değişiklik, işlenen bir işlemdir.

Güvenilir Durum Yöneticisi, Güvenilir Sözlük ve Güvenilir Kuyruk gibi güvenilir durumlar koleksiyonunu korur. Bu koleksiyon değiştiğinde Güvenilir Durum Yöneticisi bildirimleri söndürer: güvenilir bir durum eklenir veya kaldırılır veya koleksiyonun tamamı yeniden oluşturulur.
Güvenilir Durum Yöneticisi koleksiyonu üç durumda yeniden oluşturulur:

* Kurtarma: Yineleme başlatıldığında, önceki durumunu diskten kurtarır. Kurtarma sonunda, kurtarılan güvenilir durumlar kümesini içeren bir olayı ateşlemek için **NotifyStateManagerChangedEventArgs'ı** kullanır.
* Tam kopya: Yinelemenin yapılandırma kümesine katılmadan önce, oluşturulması gerekir. Bazen, bu boşta ikincil yineleme uygulanacak birincil yinelemegüvenilir Durum Yöneticisi durumunun tam bir kopyasını gerektirir. İkincil yinelemedeki Güvenilir Durum Yöneticisi, birincil yinelemeden elde ettiği güvenilir durum kümesini içeren bir olayı ateşlemek için **NotifyStateManagerChangedEventArgs** kullanır.
* Geri Yükleme: Olağanüstü durum kurtarma senaryolarında, yinelemenin durumu **RestoreAsync**üzerinden bir yedeklemeden geri yüklenebilir. Bu gibi durumlarda, birincil yinelemedeki Güvenilir Durum Yöneticisi, yedeklemeden geri yüklenmiş güvenilir durum kümesini içeren bir olayı ateşlemek için **NotifyStateManagerChangedEventArgs** kullanır.

İşlem bildirimleri ve/veya durum yöneticisi bildirimleri için kaydolmak için, Güvenilir Durum Yöneticisi'nde **TransactionChanged** veya **StateManagerChanged** olaylarına kaydolmanız gerekir. Bu olay işleyicilerine kaydolmak için ortak bir yer, devlet hizmetinin oluşturucusudur. Oluşturucuya kaydolduğunuzda, **IReliableStateManager'ın**ömrü boyunca bir değişikliğin neden olduğu hiçbir bildirimi kaçırmazsınız.

```csharp
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

**TransactionChanged** olay işleyicisi, olay hakkında ayrıntılı bilgi vermek için **NotifyTransactionChangedEventArgs** kullanır. Değişiklik türünü belirten eylem özelliğini (örneğin, **NotifyTransactionChangedAction.Commit)** içerir. Ayrıca değiştirilen hareket için bir başvuru sağlayan hareket özelliğini de içerir.

> [!NOTE]
> Bugün, **TransactionChanged** olayları yalnızca hareket işlenirse yükseltilir. Eylem daha sonra **NotifyTransactionChangedAction.Commit**eşittir. Ancak gelecekte, diğer işlem durumu değişiklikleri için olaylar yükseltilebilir. Eylemi denetlemenizi ve yalnızca beklediğiniz bir olay olduğunda eylemi işlemenizi öneririz.
> 
> 

Aşağıda örnek bir **İşlem Değiştirildi** olay işleyicisi.

```csharp
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

**StateManagerChanged** olay işleyicisi, olay hakkında ayrıntılı bilgi vermek için **NotifyStateManagerChangedEventArgs** kullanır.
**NotifyStateManagerChangedEventArgs'ın** iki alt sınıfı vardır: **NotifyStateManagerRebuildEventArgs** ve **NotifyStateManagerSingleEntityChangedEventArgs**.
**NotifyStateManagerChangedEventArgs'daki** eylem özelliğini, **NotifyStateManagerChangedEventArgs'ı** doğru alt sınıfa atmak için kullanırsınız:

* **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
* **NotifyStateManagerChangedAction.Add** and **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Aşağıda bir örnek **StateManagerChanged** bildirim işleyicisi.

```csharp
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStateManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Güvenilir Sözlük bildirimleri
Güvenilir Sözlük aşağıdaki olaylar için bildirimler sağlar:

* Yeniden Oluşturma: **ReliableDictionary,** kurtarılan veya kopyalanan yerel bir durum veya yedeklemeden durumunu kurtardığında çağrılır.
* Clear: **ClearAsync** yöntemi ile **ReliableDictionary'in** durumu temizlendiğinde çağrılır.
* Ekle: **Güvenilir Sözlük'e**bir öğe eklendiğinde çağrılır.
* Güncelleştirme: **IReliableDictionary'deki** bir öğe güncelleştirildiğinde çağrılır.
* Kaldır: **IReliableDictionary'deki** bir öğe silindiğinde çağrılır.

Güvenilir Sözlük bildirimleri almak **için, IReliableDictionary'de** **DictionaryChanged** olay işleyicisine kaydolmanız gerekir. Bu olay işleyicilerine kaydolmak için ortak bir yer **ReliableStateManager.StateManagerChanged** ekle bildiriminde dir.
**IReliableStateManager'a** **IReliableDictionary** eklendiğinde kaydolmak, hiçbir bildirimi kaçırmamanızı sağlar.

```csharp
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
        }
    }
}
```

> [!NOTE]
> **ProcessStateManagerSingleEntityNotification,** önceki **OnStateManagerChangedHandler** örneğinin aradığı örnek yöntemdir.
> 
> 

Önceki kod **IReliableNotificationAsyncCallback** arabirimini **DictionaryChanged**ile birlikte ayarlar. **Çünkü NotifyDictionaryRebuildArgs** bir **IAsyncEnumerable** arayüzü içerir -hangi asynchronously numaralandırılması gerekir -yeniden bildirimler **OnDictionaryChangedHandler**yerine **RebuildNotificationAsyncCallback** üzerinden ateşlenir .

```csharp
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> Önceki kodda, yeniden oluşturma bildirimini işlemenin bir parçası olarak, önce tutulan toplu durum temizlenir. Güvenilir koleksiyon yeni bir durumla yeniden oluşturulduğundan, önceki tüm bildirimlerin bir önemi yoktur.
> 
> 

**DictionaryChanged** olay işleyicisi, olay hakkında ayrıntılı bilgi vermek için **NotifyDictionaryChangedEventArgs** kullanır.
**NotifyDictionaryChangedEventArgs'ın** beş alt sınıfı vardır. **NotifyDictionaryChangedEventArgs'daki** eylem özelliğini kullanarak **NotifyDictionaryChangedEventArgs'ı** doğru alt sınıfa atın:

* **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
* **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
* **NotifyDictionaryChangedAction.Add**: **NotifyDictionaryItemAddedEventArgs**
* **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
* **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```csharp
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Öneriler
* Bildirim olaylarını mümkün olduğunca hızlı *tamamlayın.*
* *Do not* Senkron olayların bir parçası olarak pahalı işlemler (örneğin, G/Ç işlemleri) yürütmeyin.
* *Do* Olayı işlemeden önce eylem türünü denetleyin. Gelecekte yeni eylem türleri eklenebilir.

Akılda tutulması gereken bazı şeyler şunlardır:

* Bildirimler, bir işlemin yürütülmesinin bir parçası olarak ateşlenir. Örneğin, geri yükleme işleminin son adımı olarak bir geri yükleme bildirimi ateşlenir. Bildirim olayı işlenene kadar geri yükleme işlemi tamamlanmaz.
* Bildirimler uygulama işlemlerinin bir parçası olarak ateşlendirilmelerinden, istemciler yalnızca yerel olarak işlenen işlemler için bildirimler görür. Ve işlemler yalnızca yerel olarak kaydedilecek (diğer bir deyişle, günlüğe kaydedilmiş) garanti olduğundan, gelecekte geri alınabilir veya geri alınmayabilir.
* Yeniden yapma yolunda, uygulanan her işlem için tek bir bildirim ateşlenir. Bu, T1 işlemi Create(X), Delete(X) ve Create(X) içeriyorsa, X'in oluşturulması için bir bildirim, biri silme için, diğeri de bu sırada yeniden oluşturma için bir bildirim alacağınız anlamına gelir.
* Birden çok işlem içeren hareketler için işlemler, kullanıcıdan birincil yinelemede alındıkları sırayla uygulanır.
* Yanlış ilerleme işleminin bir parçası olarak, bazı işlemler geri alınabilir. Bildirimler, yinelemenin durumunu kararlı bir noktaya geri döndürecek şekilde, bu tür geri alma işlemleri için yükseltilir. Geri al bildirimleri önemli bir fark, yinelenen anahtarları olan olayların biraraya toplanmış olmasıdır. Örneğin, hareket T1 geri alınıyorsa, Delete(X) için tek bir bildirim görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar](service-fabric-work-with-reliable-collections.md)
* [Güvenilir Hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
* [Güvenilir Hizmetler yedekleme ve geri yükleme (olağanüstü durum kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir Koleksiyonlar için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

