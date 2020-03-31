---
title: .NET Framework 4.7.2 yükseltmesi nedeniyle Azure Veri Gölü Analytics U-SQL iş hatalarını giderme
description: .NET Framework 4.7.2'ye yükseltme nedeniyle U-SQL iş hatalarını giderin.
services: data-lake-analytics
author: guyhay
ms.author: guyhay
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: troubleshooting
ms.workload: big-data
ms.date: 10/11/2019
ms.openlocfilehash: f909419810cbd837e57b19a13b2df6ae9ad2ee97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79213579"
---
# <a name="azure-data-lake-analytics-is-upgrading-to-the-net-framework-v472"></a>Azure Veri Gölü Analitiği .NET Framework v4.7.2'ye yükseltiyor

Azure Veri Gölü Analizi varsayılan çalışma süresi .NET Framework v4.5.2'den .NET Framework v4.7.2'ye yükseltilir. Bu değişiklik, U-SQL kodunuz özel derlemeler kullanıyorsa ve bu özel derlemeler .NET kitaplıklarını kullanıyorsa, değişiklikleri kırma riski oluşturur.

.NET Framework 4.5.2'den sürüm 4.7.2'ye yapılan bu yükseltme, U-SQL çalışma zamanında (varsayılan çalışma süresi) dağıtılan .NET Framework'ün artık her zaman 4,7.2 olacağı anlamına gelir. .NET Framework sürümleri için yan yana seçenek yoktur.

.NET Framework 4.7.2'ye yapılan bu yükseltme tamamlandıktan sonra, sistemin yönetilen kodu sürüm 4.7.2 olarak çalışacak, kullanıcı sağlanan kitaplıklar U-SQL özel derlemeleri gibi derlemenin olduğu sürüme uygun geriye dönük uyumlu modda çalışacaktır için oluşturulur.

- Derleme DL'leriniz sürüm 4.5.2 için oluşturulursa, dağıtılan çerçeve bunları 4,5.2 kitaplıkları olarak ele alır ve (birkaç istisna dışında) 4.5.2 anlambilimi sağlar.
- Artık .NET Framework 4.7.2'yi hedefliyorsanız, sürüm 4.7.2 özelliklerinden yararlanan U-SQL özel derlemelerini kullanabilirsiniz.

.NET Framework 4.7.2'ye yapılan bu yükseltme nedeniyle, .NET özel derlemelerini kullanan U-SQL işlerinizde çığır açan değişiklikler başlatma potansiyeli vardır. Aşağıdaki yordamı kullanarak geriye dönük uyumluluk sorunlarını kontrol etmenizi öneririz.

## <a name="how-to-check-for-backwards-compatibility-issues"></a>Geriye dönük uyumluluk sorunları nasıl kontrol ediletilir?

U-SQL özel derlemelerinizde .NET kodunuzda .NET uyumluluk denetimlerini çalıştırarak geriye dönük uyumluluk bozma sorunlarının potansiyelini denetleyin.

> [!Note]
> Araç gerçek kırılma değişikliklerini algılamaz. yalnızca .NET API'leri olarak adlandırılan ve (belirli girişler için) sorunlara neden olabilecek leri tanımlar. Bir sorun dan haberdar olursanız, kodunuz hala iyi olabilir, ancak daha fazla ayrıntı iade etmelisiniz.

1. .NET DL'lerinizde geriye dönük uyumluluk denetleyicisini
   1. [.NET Taşınabilirlik Çözümleyicisi Visual Studio Extension](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) adresindeki Visual Studio Uzantısını Kullanma
   1. [GitHub dotnetapiport'tan](https://github.com/microsoft/dotnet-apiport)bağımsız aracı indirme ve kullanma. Tek başına araç çalıştırmak için talimatlar [GitHub dotnetapiport kesme değişiklikleri](https://github.com/microsoft/dotnet-apiport/blob/dev/docs/HowTo/BreakingChanges.md) bulunmaktadır
   1. 4.7.2 için. uyumluluk, `read isRetargeting == True` olası sorunları tanımlar.
2. Araç, kodunuzun olası geriye dönük uyumsuzluklardan etkilenip etkilenmediğini gösterirse (bazı yaygın uyumsuzluk örnekleri aşağıda listelenmiştir),
   1. Kodunuzu çözümleme ve kodunuzu etkilenen API'lere değerleri geçirip geçirmeme
   1. Çalışma zamanı denetimi gerçekleştirin. AdLA'da çalışma zamanı dağıtımı yan yana yapılmaz. VisualStudio'nun yerel çalışmasını yerel bir .NET Framework 4.7.2 ile temsili bir veri kümesine karşı kullanarak yükseltmeden önce çalışma zamanı denetimi gerçekleştirebilirsiniz.
3. Gerçekten bir geriye dönük uyumsuzluk tarafından etkilenir, bunu düzeltmek için gerekli adımları (örneğin veri veya kod mantığı sabitleme gibi) atın.

Çoğu durumda, geriye dönük uyumsuzlukluluk etkilenmemelidir.

## <a name="timeline"></a>Zaman çizelgesi

Yeni çalışma zamanının dağıtımını burada [Runtime sorun giderme](runtime-troubleshoot.md)olarak ve daha önce başarılı olan herhangi bir işe bakarak denetleyebilirsiniz.

### <a name="what-if-i-cant-get-my-code-reviewed-in-time"></a>Kodumu zamanında gözden geçiremezsem ne olur?

İşinizi eski çalışma zamanı sürümüne (4.5.2 hedeflemesi hedeflenen) karşı gönderebilirsiniz, ancak .NET Framework yan yana özelliklerinin olmaması nedeniyle, yine de yalnızca 4.5.2 uyumluluk modunda çalışır. Bu davranış nedeniyle geriye dönük uyumluluk sorunlarıyla karşılaşabilirsiniz.

### <a name="what-are-the-most-common-backwards-compatibility-issues-you-may-encounter"></a>Karşılaşabileceğiniz en yaygın geriye dönük uyumluluk sorunları nelerdir

Denetleyicinin belirleyebileceği en yaygın geriye dönük uyumsuzluklar (bu listeyi kendi dahili ADLA işlerimizde denetleyerek oluşturduk), hangi kitaplıklar etkilenir (not: kitaplıkları yalnızca dolaylı olarak çağırabileceğiniz, bu nedenle işlerinizi etkileyip etkilemediğini kontrol etmek için gerekli eylemi #1 önemli dir) ve çözüm için olası eylemlerdir. Not: Kendi işlerimiz için hemen hemen her durumda, uyarılar çoğu kırılma değişikliklerindar doğası nedeniyle yanlış pozitif olduğu ortaya çıktı.

- IAsyncResult.CompletedSynchronously özelliği tamamlamak için ortaya çıkan görev için doğru olmalıdır
  - TaskFactory.FromAsync'i ararken, iAsyncResult.CompletedSynchronously özelliğinin uygulanması, elde edilen görevin tamamlanması için doğru olmalıdır. Diğer bir süre, uygulama eşzamanlı olarak tamamlanırsa, özelliğin doğru olarak döndürülmesi gerekir. Daha önce özellik denetlenmedi.
  - Etkilenen Kütüphaneler: mscorlib, System.Threading.Tasks
  - Önerilen Eylem: TaskFactory.FromAsync doğru doğru döndürür emin olun

- DataObject.GetData artık UTF-8 olarak veri alır
  - .NET Framework 4'e yönelik veya .NET Framework 4.5.1 veya önceki sürümlerde çalışan uygulamalar için DataObject.GetData, HTML biçimli verileri ASCII dizesi olarak alır. Sonuç olarak, ASCII olmayan karakterler (ASCII kodları 0x7F'den büyük olan karakterler) iki rasgele karakterle temsil edilir.#N##N#.NET Framework 4.5 veya sonraki lerini hedefleyen `DataObject.GetData` ve .NET Framework 4.5.2 üzerinde çalışan uygulamalar için HTML biçimli verileri 0x7F'den daha büyük karakterleri doğru temsil eden UTF-8 olarak alır.
  - Etkilenen Kütüphaneler: Glo
  - Önerilen Eylem: Alınan verilerin istediğiniz biçim olduğundan emin olun

- XmlWriter geçersiz vekil çiftleri atar
  - .NET Framework 4.5.2 veya önceki sürümleri hedefleyen uygulamalar için, özel durum geri dönüş işlemesini kullanarak geçersiz bir vekil çifti yazmak her zaman bir özel durum oluşturmaz. .NET Framework 4.6'yı hedefleyen uygulamalar için, geçersiz bir vekil `ArgumentException`çifti yazmaya çalışmak bir .
  - Etkilenen Kütüphaneler: System.Xml, System.Xml.ReaderWriter
  - Önerilen Eylem: Bağımsız değişken özel durum neden olacak geçersiz bir vekil çifti yazmadığınızdan emin olun

- HtmlTextWriter öğeyi `<br/>` doğru işlemez
  - .NET Framework 4.6'dan `HtmlTextWriter.RenderBeginTag()` başlayarak, arama ve `HtmlTextWriter.RenderEndTag()` bir `<BR />` öğeyle yalnızca bir `<BR />` tane (iki yerine) doğru bir şekilde ekler
  - Etkilenen Kütüphaneler: System.Web
  - Önerilen Eylem: Üretim işinde rastgele bir `<BR />` davranış görülmemesi için görmeyi beklediğiniz miktarı eklediğinizden emin olun

- CreateDefaultAuthorizationContext'ı null bağımsız değişkenle arama
  - Null authorizationPolicies argümanı `CreateDefaultAuthorizationContext(IList<IAuthorizationPolicy>)` ile bir çağrı ile döndürülen Yetkilendirme Bağlamının uygulanması .NET Framework 4.6'daki uygulamasını değiştirmiştir.
  - Etkilenen Kütüphaneler: System.IdentityModel
  - Önerilen Eylem: Null yetkilendirme ilkesi olduğunda yeni beklenen davranışı ele aldığınızdan emin olun
  
- RSACng artık standart olmayan anahtar boyutundaki RSA tuşlarını doğru bir şekilde yükler
  - 4.6.2'den önceki .NET Framework sürümlerinde, RSA sertifikaları için standart olmayan anahtar boyutlarına `GetRSAPublicKey()` `GetRSAPrivateKey()` sahip müşteriler bu anahtarlara ve uzantı yöntemleriyle erişemez. "İstenen anahtar boyutu desteklenmiyor" iletisi `CryptographicException` ile bir atılıyor. .NET Framework 4.6.2 ile bu sorun giderilmiştir. Benzer `RSA.ImportParameters()` şekilde, `RSACng.ImportParameters()` ve şimdi 's atmadan `CryptographicException`standart olmayan anahtar boyutları ile çalışır.
  - Etkilenen Kütüphaneler: mscorlib, System.Core
  - Önerilen Eylem: RSA anahtarlarının beklendiği gibi çalıştığından emin olun

- Yol kolon denetimleri daha sıkıdır
  - .NET Framework 4.6.2'de, daha önce desteklenmeyen yolları (hem uzunluk hem de biçim olarak) desteklemek için bir dizi değişiklik yapıldı. Uygun sürücü ayırıcısı (üst üste) sözdizimi için kontroller daha doğru yapıldı, hangi tolere edilmesi için kullanılan birkaç seçin Yol API'lerinde bazı URI yollarını engelleme yan etkisi vardı.
  - Etkilenen Kütüphaneler: mscorlib, System.Runtime.Extensions
  - Önerilen Eylem:

- İddialara ÇağrıLarKimlik yapıcıları
  - .NET Framework 4.6.2 ile başlayarak, parametreli `T:System.Security.Claims.ClaimsIdentity` yapıcıların `T:System.Security.Principal.IIdentity` `P:System.Security.Claims.ClaimsIdentify.Actor` özelliği nasıl ayarlayıştAma şeklinde bir değişiklik vardır. `T:System.Security.Principal.IIdentity` Bağımsız değişken bir `T:System.Security.Claims.ClaimsIdentity` nesneise `P:System.Security.Claims.ClaimsIdentify.Actor` ve bu `T:System.Security.Claims.ClaimsIdentity` nesnenin `null`özelliği `P:System.Security.Claims.ClaimsIdentify.Actor` değilse, özellik `M:System.Security.Claims.ClaimsIdentity.Clone` yöntem kullanılarak eklenir. Çerçeve 4.6.1 ve önceki sürümlerde, `P:System.Security.Claims.ClaimsIdentify.Actor` özellik varolan bir başvuru olarak eklenir. Bu değişiklik ten dolayı, .NET Framework 4.6.2 ile `T:System.Security.Claims.ClaimsIdentity` başlayarak, yeni `P:System.Security.Claims.ClaimsIdentify.Actor` nesnenin `T:System.Security.Principal.IIdentity` `P:System.Security.Claims.ClaimsIdentify.Actor` özelliği oluşturucubağımsız değişkeninin özelliğine eşit değildir. .NET Framework 4.6.1 ve önceki sürümlerinde eşittir.
  - Etkilenen Kütüphaneler: mscorlib
  - Önerilen Eylem: ClaimsIdentity'in yeni çalışma zamanında beklendiği gibi çalıştığından emin olun

- DataContractJsonSerializer ile kontrol karakterlerinin serileştirilmesi artık ECMAScript V6 ve V8 ile uyumludur
  - .NET çerçevesi 4.6.2 ve önceki sürümlerinde DataContractJsonSerializer, \b, \f ve \t gibi bazı özel denetim karakterlerini ECMAScript V6 ve V8 standartlarıyla uyumlu bir şekilde serihale getiremedi. .NET Framework 4.7 ile başlayarak, bu kontrol karakterlerinin serileştirilmesi ECMAScript V6 ve V8 ile uyumludur.
  - Etkilenen Kütüphaneler: System.Runtime.Serialization.Json
  - Önerilen Eylem: DataContractJsonSerializer ile aynı davranışı sağlamak
