---
title: 'Uygulama yükseltme: veri serileştirme'
description: Veri serileştirme ve bunların sıralı uygulama yükseltmelerini nasıl etkilediği için en iyi uygulamalar.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.openlocfilehash: d502e74139c543d4183a75faa6bea1948d9f3e56
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86247991"
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Veri serileştirmesi bir uygulama yükseltmesini nasıl etkiler
Bir sıralı [uygulama yükseltmesinde](service-fabric-application-upgrade.md), yükseltme bir tek seferde bir yükseltme etki alanı olmak üzere düğümlerin bir alt kümesine uygulanır. Bu işlem sırasında, bazı yükseltme etki alanları uygulamanızın daha yeni bir sürümüdür ve bazı yükseltme etki alanları uygulamanızın eski bir sürümüdür. Dağıtım sırasında, uygulamanızın yeni sürümü verilerinizin eski sürümünü okuyabilmelidir ve uygulamanızın eski sürümü verilerinizin yeni sürümünü okuyabilmelidir olması gerekir. Veri biçimi ileri ve geri uyumlu değilse, yükseltme başarısız olabilir veya daha kötü, veriler kaybolabilir veya bozulabilir. Bu makalede, veri biçiminizi ne oluşturduğunu ve verilerinizin ileriye ve geriye dönük olarak uyumlu olmasını sağlamak için en iyi yöntemleri sunan açıklanır.

## <a name="what-makes-up-your-data-format"></a>Veri biçiminizi ne yapar?
Azure Service Fabric 'de kalıcı ve çoğaltılan veriler C# sınıflarınızdan gelir. [Güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)kullanan uygulamalar için, bu veriler güvenilir sözlüklerdeki ve kuyruklardaki nesnelerdir. [Reliable Actors](service-fabric-reliable-actors-introduction.md)kullanan uygulamalar için, aktör için yedekleme durumudur. Bu C# sınıflarının kalıcı olması ve çoğaltılması için seri hale getirilebilir olması gerekir. Bu nedenle, veri biçimi serileştirilmiş olan alanlar ve özellikler tarafından ve nasıl serileştirildikleri gibi tanımlanır. Örneğin, bir `IReliableDictionary<int, MyClass>` veri seri hale getirilebilir `int` ve serileştirilmiş olur `MyClass` .

### <a name="code-changes-that-result-in-a-data-format-change"></a>Veri biçimi değişikliğine neden olan kod değişiklikleri
Veri biçimi C# sınıfları tarafından belirlendiği için sınıflardaki değişiklikler bir veri biçimi değişikliğine neden olabilir. Sıralı bir yükseltmenin veri biçimi değişikliğini işleyebileceğini güvence altına almak için dikkatli olunmalıdır. Veri biçimi değişikliklerine neden olabilecek örnekler:

* Alan veya özellik ekleme veya kaldırma
* Alanları veya özellikleri yeniden adlandırma
* Alan veya özellik türlerini değiştirme
* Sınıf adını veya ad alanını değiştirme

### <a name="data-contract-as-the-default-serializer"></a>Varsayılan serileştirici olarak veri sözleşmesi
Seri hale getirici, verileri okumaktan ve verilerin daha eski veya *daha yeni* bir sürümde olsa bile geçerli sürüme serileştirilmesinden genellikle sorumludur. Varsayılan serileştirici, iyi tanımlanmış sürüm oluşturma kurallarına sahip olan [veri sözleşmesi serileştiricsahiptir](/dotnet/framework/wcf/feature-details/using-data-contracts). Güvenilir koleksiyonlar seri hale getiricinin geçersiz kılınmasına izin verir, ancak Reliable Actors Şu anda değil. Veri serileştirici, çalışırken yükseltmeleri olanaklı hale getirmekte önemli bir rol oynar. Veri sözleşmesi serileştiricisi, Service Fabric uygulamaları için önerdiğimiz serileştiricsahiptir.

## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>Veri biçimi sıralı yükseltmeyi nasıl etkiler
Sıralı yükseltme sırasında, seri hale getiricinin verilerinizin daha eski veya *daha yeni* bir sürümüyle karşılaşabilecekleri iki ana senaryo vardır:

1. Bir düğüm yükseltildikten ve yedekleme başladıktan sonra, yeni serileştirici, kalıcı olan verileri eski sürüm tarafından diske yükler.
2. Çalışırken yükseltme sırasında, küme, kodunuzun eski ve yeni sürümlerinin bir karışımını içerir. Çoğaltmalar farklı yükseltme etki alanlarına yerleştirilebileceğinizden ve çoğaltmalar birbirlerine veri gönderediklerinden, verilerinizin yeni ve/veya eski sürümü seri hale getiricinin yeni ve/veya eski sürümü ile karşılaşabilir.

> [!NOTE]
> Burada "yeni sürüm" ve "eski sürüm", çalışan kodunuzun sürümüne başvurur. "Yeni serileştirici", uygulamanızın yeni sürümünde yürütülen seri hale getirici kodunu ifade eder. "Yeni veriler", uygulamanızın yeni sürümündeki serileştirilmiş C# sınıfına başvurur.
> 
> 

Kodun ve veri biçiminin iki sürümü hem ileriye hem de geriye dönük olarak uyumlu olmalıdır. Bunlar uyumlu değilse, hareketli yükseltme başarısız olabilir veya veriler kaybolabilir. Kod veya seri hale getirici diğer sürümle karşılaştığında özel durumlar veya bir hata oluşturabileceğinden, yuvarlama yükseltmesi başarısız olabilir. Örneğin, yeni bir özellik eklendiyse, ancak eski seri hale getirici serisini kaldırma sırasında atarsa veriler kaybolabilir.

Veri sözleşmesi, verilerinizin uyumlu olduğundan emin olmak için önerilen çözümdür. Alan ekleme, kaldırma ve değiştirme için iyi tanımlanmış sürüm kuralları içerir. Ayrıca, bilinmeyen alanlarla işleme, serileştirme ve seri kaldırma işlemine bağlama ve sınıf devralma ile ilgilenme desteği de vardır. Daha fazla bilgi için bkz. [veri sözleşmesini kullanma](/dotnet/framework/wcf/feature-details/using-data-contracts).

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio 'Yu kullanarak uygulamanızı yükseltmek](service-fabric-application-upgrade-tutorial.md) , Visual Studio kullanarak bir uygulama yükseltme işleminde size yol gösterir.

[PowerShell kullanarak uygulamanızı yükseltmek](service-fabric-application-upgrade-tutorial-powershell.md) , PowerShell kullanarak bir uygulama yükseltme işleminde size yol gösterir.

Uygulamanızın [yükseltme parametrelerini](service-fabric-application-upgrade-parameters.md)kullanarak nasıl yükseltileceğini denetleyin.

[Gelişmiş konulara](service-fabric-application-upgrade-advanced.md)başvurarak uygulamanızı yükseltirken gelişmiş işlevselliği nasıl kullanacağınızı öğrenin.

Uygulama [yükseltmelerinde sorun giderme](service-fabric-application-upgrade-troubleshooting.md)adımlarını izleyerek uygulama yükseltmelerinde karşılaşılan yaygın sorunları giderin.
