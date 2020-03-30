---
title: Güvenilir Toplama nesneserileştirme
description: Varsayılan strateji ve özel serileştirmenin nasıl tanımlanılması da dahil olmak üzere Azure Hizmet Kumaşı Güvenilir Koleksiyonlar nesneserileştirme hakkında bilgi edinin.'
ms.topic: conceptual
ms.date: 5/8/2017
ms.openlocfilehash: 666e1bb45a9c75ee143f15a0d871d6ae1408eca9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639556"
---
# <a name="reliable-collection-object-serialization-in-azure-service-fabric"></a>Azure Hizmet Kumaşında Güvenilir Toplama nesneserileştirmesi
Güvenilir Koleksiyonlar, makine arızaları ve elektrik kesintileri arasında dayanıklı olduğundan emin olmak için öğelerini çoğaltır ve devam ettir.
Öğeleri çoğaltmak ve kalıcı hale getirmek için Güvenilir Koleksiyonlar'ın bunları seri hale getirebilmeleri gerekir.

Güvenilir Koleksiyonlar' Güvenilir Devlet Yöneticisi belirli bir tür için uygun serializer olsun.
Güvenilir Durum Yöneticisi yerleşik serializers içerir ve özel serializers belirli bir tür için kayıtlı olmasını sağlar.

## <a name="built-in-serializers"></a>Dahili Serializers

Güvenilir Durum Yöneticisi, varsayılan olarak verimli bir şekilde seri hale getirilebilmeleri için bazı yaygın türler için yerleşik serileştirici içerir. Diğer türler için, Güvenilir Durum Yöneticisi [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer(v=vs.110).aspx)kullanmak için geri düşüyor.
Yerleşik serileştiriciler, türlerinin değiştirilemeyeceğini bildiklerinden ve türü gibi türü hakkında bilgi eklemelerine gerek olmadığından daha verimlidir.

Güvenilir Durum Yöneticisi, aşağıdaki türler için yerleşik serializer'e sahiptir: 
- Guid
- bool
- byte
- sbyte
- bayt[]
- char
- string
- decimal
- double
- float
- int
- uint
- long
- ulong
- short
- ushort

## <a name="custom-serialization"></a>Özel Serileştirme

Özel serializers genellikle performansı artırmak veya tel ve disk üzerinden verileri şifrelemek için kullanılır. Diğer nedenlerin yanı sıra, özel serializers genellikle genel serializer daha verimli dir, çünkü türü hakkında bilgi serileştirmek gerekmez. 

[IReliableStateManager.TryAddStateSerializer\<T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestatemanager.tryaddstateserializer) verilen t türü için özel bir serializer kaydetmek için kullanılır. Bu kayıt, kurtarma başlamadan önce tüm Güvenilir Koleksiyonlar'ın kalıcı verilerini okumak için ilgili serileştiriciye erişebilmesini sağlamak için StatefulServiceBase'in yapımında gerçekleşmelidir.

```csharp
public StatefulBackendService(StatefulServiceContext context)
  : base(context)
  {
    if (!this.StateManager.TryAddStateSerializer(new OrderKeySerializer()))
    {
      throw new InvalidOperationException("Failed to set OrderKey custom serializer");
    }
  }
```

> [!NOTE]
> Özel serializers yerleşik serializers üzerinde öncelik verilir. Örneğin, int için özel bir serileştirici kaydedildiğinde, yerleşik int serileştirici yerine tamsayıları serihale getirmek için kullanılır.

### <a name="how-to-implement-a-custom-serializer"></a>Özel bir serializer nasıl uygulanır

Özel bir serializer [\<IStateSerializer T>](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.istateserializer-1) arabirimini uygulamak gerekir.

> [!NOTE]
> IStateSerializer\<T>, Temel değer adı verilen ek bir T alan Yazma ve Okuma için aşırı yükleme içerir. Bu API diferansiyel serileştirme içindir. Şu anda diferansiyel serileştirme özelliği açıkdeğil. Bu nedenle, diferansiyel serileştirme maruz ve etkinleştirilene kadar bu iki aşırı yükleme çağrılmaz.

Aşağıda dört özellik içeren OrderKey adlı örnek bir özel türü

```csharp
public class OrderKey : IComparable<OrderKey>, IEquatable<OrderKey>
{
    public byte Warehouse { get; set; }

    public short District { get; set; }

    public int Customer { get; set; }

    public long Order { get; set; }

    #region Object Overrides for GetHashCode, CompareTo and Equals
    #endregion
}
```

Aşağıda IStateSerializer\<OrderKey> bir örnek uygulamasıdır.
BaseValue'ı alan Okuma ve Yazma aşırı yüklemelerinin, iletme uyumluluğu için ilgili aşırı yüklemeleri çağırdığını unutmayın.

```csharp
public class OrderKeySerializer : IStateSerializer<OrderKey>
{
  OrderKey IStateSerializer<OrderKey>.Read(BinaryReader reader)
  {
      var value = new OrderKey();
      value.Warehouse = reader.ReadByte();
      value.District = reader.ReadInt16();
      value.Customer = reader.ReadInt32();
      value.Order = reader.ReadInt64();

      return value;
  }

  void IStateSerializer<OrderKey>.Write(OrderKey value, BinaryWriter writer)
  {
      writer.Write(value.Warehouse);
      writer.Write(value.District);
      writer.Write(value.Customer);
      writer.Write(value.Order);
  }
  
  // Read overload for differential de-serialization
  OrderKey IStateSerializer<OrderKey>.Read(OrderKey baseValue, BinaryReader reader)
  {
      return ((IStateSerializer<OrderKey>)this).Read(reader);
  }

  // Write overload for differential serialization
  void IStateSerializer<OrderKey>.Write(OrderKey baseValue, OrderKey newValue, BinaryWriter writer)
  {
      ((IStateSerializer<OrderKey>)this).Write(newValue, writer);
  }
}
```

## <a name="upgradability"></a>Yükseltilebilirlik
[Yuvarlanan uygulama yükseltmesinde](service-fabric-application-upgrade.md)yükseltme, düğümlerin bir alt kümesine, bir defada bir yükseltme etki alanına uygulanır. Bu işlem sırasında, bazı yükseltme etki alanları uygulamanızın yeni sürümünde ve bazı yükseltme etki alanları uygulamanızın eski sürümünde olacaktır. Kullanıma alma sırasında, uygulamanızın yeni sürümü verilerinizin eski sürümünü okuyabilmeli ve uygulamanızın eski sürümü verilerinizin yeni sürümünü okuyabilmeli. Veri biçimi ileri ve geri uyumlu değilse, yükseltme başarısız olabilir veya daha kötüsü, veriler kaybolabilir veya bozulabilir.

Yerleşik serializer kullanıyorsanız, uyumluluk konusunda endişelenmenize gerek yoktur.
Ancak, özel bir serializer veya DataContractSerializer kullanıyorsanız, veri sonsuz geri ve ileriye uyumlu olmalıdır.
Başka bir deyişle, serializer her sürümü seriize ve tür herhangi bir sürümünü de-serialize gerekir.

Veri Sözleşmesi kullanıcıları, alan ekleme, kaldırma ve değiştirme için iyi tanımlanmış sürüm kurallarına uymalıdır. Veri Sözleşmesi ayrıca bilinmeyen alanlarla başa çıkmak, serileştirme ve deserialization işlemine bağlanma ve sınıf kalıtımı ile başa çıkmak için de destek vardır. Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/ms733127.aspx)

Özel serializer kullanıcıları, geriye ve ileriye uyumlu olduğundan emin olmak için kullandıkları serileştiricinin yönergelerine uymalıdır.
Tüm sürümleri desteklemenin yaygın bir yolu, başlangıçta boyut bilgileri eklemek ve yalnızca isteğe bağlı özellikler eklemektir.
Bu şekilde her sürüm olabildiğince okuyabilir ve akışın kalan bölümünün üzerinden atlayabilir.

## <a name="next-steps"></a>Sonraki adımlar
  * [Serileştirme ve yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir Koleksiyonlar için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  * [Visual Studio kullanarak Uygulama yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme ile size yol.
  * [Powershell kullanarak Uygulamayükseltme PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) kullanarak bir uygulama yükseltme ile size yol.
  * [Yükseltme](service-fabric-application-upgrade-parameters.md)Parametreleri'ni kullanarak uygulamanızın nasıl yükselttikini kontrol edin.
  * [Gelişmiş Konular'a](service-fabric-application-upgrade-advanced.md)atıfta bulunarak uygulamanızı yükseltirken gelişmiş işlevselliği nasıl kullanacağınızı öğrenin.
  * [Sorun Giderme Uygulama Yükseltmeleri'ndeki](service-fabric-application-upgrade-troubleshooting.md)adımlara atıfta bulunarak uygulama yükseltmelerinde sık karşılaşılan sorunları giderin.
