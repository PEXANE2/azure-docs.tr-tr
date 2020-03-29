---
title: 'Uygulama yükseltmesi: veri serileştirme'
description: Veri serileştirme ve yuvarlama uygulama yükseltmeleri nasıl etkilediği için en iyi uygulamalar.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.openlocfilehash: 7dc60c28b56982f82c1ac90db55ac752977ea2d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75457488"
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Veri serileştirmesi uygulama yükseltmeyi nasıl etkiler?
[Yuvarlanan uygulama yükseltmesinde](service-fabric-application-upgrade.md)yükseltme, düğümlerin bir alt kümesine, bir defada bir yükseltme etki alanına uygulanır. Bu işlem sırasında, bazı yükseltme etki alanları uygulamanızın yeni sürümünde ve bazı yükseltme etki alanları uygulamanızın eski sürümünde dir. Kullanıma alma sırasında, uygulamanızın yeni sürümü verilerinizin eski sürümünü okuyabilmeli ve uygulamanızın eski sürümü verilerinizin yeni sürümünü okuyabilmeli. Veri biçimi ileri ve geri uyumlu değilse, yükseltme başarısız olabilir veya daha kötüsü, veriler kaybolabilir veya bozulabilir. Bu makalede, veri biçiminizi oluşturan nedir tartışılır ve verilerinizin ileri ve geri uyumlu olmasını sağlamak için en iyi uygulamaları sunar.

## <a name="what-makes-up-your-data-format"></a>Veri biçiminizi oluşturan nedir?
Azure Hizmet Kumaşı'nda, kalıcı ve çoğaltılan veriler C# sınıflarınızdan gelir. [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)kullanan uygulamalar için, bu veriler güvenilir sözlüklerde ve kuyruklarda bulunan nesnelerdir. [Güvenilir Aktörler](service-fabric-reliable-actors-introduction.md)kullanan uygulamalar için, bu aktör için destek durumudur. Bu C# sınıfları kalıcı ve çoğaltılabilir serileştirilebilir olmalıdır. Bu nedenle, veri biçimi serileştirilmiş alanlar ve özellikleri yanı sıra nasıl serileştirilir tarafından tanımlanır. Örneğin, bir `IReliableDictionary<int, MyClass>` veri serileştirilmiş `int` ve serileştirilmiş. `MyClass`

### <a name="code-changes-that-result-in-a-data-format-change"></a>Veri biçimi değişikliğiyle sonuçlanan kod değişiklikleri
Veri biçimi C# sınıfları tarafından belirlendiğinden, sınıflardaki değişiklikler veri biçimi değişikliğine neden olabilir. Yuvarlanan bir yükseltmenin veri biçimi değişikliğini işleyebilir emin olmak için dikkatli olunmalıdır. Veri biçiminde değişikliklere neden olabilecek örnekler:

* Alan veya özellik ekleme veya kaldırma
* Alanları veya özellikleri yeniden adlandırma
* Alan veya özellik türlerini değiştirme
* Sınıf adını veya ad alanını değiştirme

### <a name="data-contract-as-the-default-serializer"></a>Varsayılan serileştirici olarak Veri Sözleşmesi
Serileştirici genellikle verileri okumaktan ve veriler daha eski veya *daha yeni* bir sürümde olsa bile, geçerli sürüme deserializing sorumludur. Varsayılan serileştirici, iyi tanımlanmış sürüm kuralları olan [Veri Sözleşmesi serileştiricisidir.](https://msdn.microsoft.com/library/ms733127.aspx) Güvenilir Koleksiyonlar serileyicinin geçersiz kılınmasını sağlar, ancak Güvenilir Aktörler şu anda geçersiz dir. Veri serileştirici haddeleme yükseltmeleri etkinleştirmede önemli bir rol oynar. Veri Sözleşmesi serilaştırıcı, Service Fabric uygulamaları için önerdiğimiz serileştiricidir.

## <a name="how-the-data-format-affects-a-rolling-upgrade"></a>Veri biçimi yuvarlanan yükseltmeyi nasıl etkiler?
Yuvarlanma yükseltmesi sırasında, serileştiricinin verilerinizin daha eski veya *daha yeni* bir sürümüyle karşılaşabileceği iki ana senaryo vardır:

1. Düğüm yükseltildikten ve yeniden başlatıldıktan sonra, yeni serileştirici eski sürüm tarafından diske kalıcı verileri yükler.
2. Yuvarlanma yükseltmesi sırasında küme, kodunuzu eski ve yeni sürümlerinin bir karışımını içerir. Yinelemeler farklı yükseltme etki alanlarında yerleştirilebildiği ve yinelemeler birbirine veri gönderdiğinden, verilerinizin yeni ve/veya eski sürümüyle serileştiricinizin yeni ve/veya eski sürümüyle karşılaşılabilir.

> [!NOTE]
> Buradaki "yeni sürüm" ve "eski sürüm", kodunuzu çalışan sürümüne atıfta bulunur. "Yeni serileştirici" uygulamanızın yeni sürümünde çalıştırıyorum serializer kodu anlamına gelir. "Yeni veriler", uygulamanızın yeni sürümünden serileştirilmiş C# sınıfına başvurur.
> 
> 

Kod ve veri biçiminin iki sürümü hem ileri hem de geri uyumlu olmalıdır. Bunlar uyumlu değilse, yuvarlama yükseltmesi başarısız olabilir veya veriler kaybolabilir. Kod veya serializer diğer sürümle karşılaştığında özel durumlar veya hata lar atabileceğinden, yuvarlanma yükseltmesi başarısız olabilir. Örneğin, yeni bir özellik eklendiği, ancak eski seri leştirme sırasında eski serileştirici atılırsa veriler kaybedilebilir.

Veri Sözleşmesi, verilerinizin uyumlu olmasını sağlamak için önerilen çözümdür. Alan eklemek, kaldırmak ve değiştirmek için iyi tanımlanmış sürüm kuralları vardır. Ayrıca bilinmeyen alanları ile başa çıkmak için destek vardır, serileştirme ve deserialization sürecine kanca, ve sınıf kalıtım ile ilgili. Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/ms733127.aspx)

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio kullanarak Uygulama yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme ile size yol.

[Powershell kullanarak Uygulamayükseltme PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) kullanarak bir uygulama yükseltme ile size yol.

[Yükseltme](service-fabric-application-upgrade-parameters.md)Parametreleri'ni kullanarak uygulamanızın nasıl yükselttikini kontrol edin.

[Gelişmiş Konular'a](service-fabric-application-upgrade-advanced.md)atıfta bulunarak uygulamanızı yükseltirken gelişmiş işlevselliği nasıl kullanacağınızı öğrenin.

[Sorun Giderme Uygulama Yükseltmeleri'ndeki](service-fabric-application-upgrade-troubleshooting.md)adımlara atıfta bulunarak uygulama yükseltmelerinde sık karşılaşılan sorunları giderin.

