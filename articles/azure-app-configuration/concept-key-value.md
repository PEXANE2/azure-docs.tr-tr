---
title: Azure Uygulama Yapılandırması anahtar değer mağazanı anlama
description: Yapılandırma verilerinin Azure Uygulama Yapılandırması'nda nasıl depolanır olduğunu anlayın.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/19/2020
ms.openlocfilehash: 0b83a35d912c97ae25bc2d69d076e8eae8ca490f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523613"
---
# <a name="keys-and-values"></a>Anahtarlar ve değerler

Azure Uygulama Yapılandırması yapılandırma verilerini anahtar değer çiftleri olarak depolar. Anahtar değer çiftleri, geliştiriciler tarafından kullanılan uygulama ayarlarının basit ve esnek bir gösterimidir.

## <a name="keys"></a>Anahtarlar

Anahtarlar anahtar değeri çiftleri için tanımlayıcı olarak kullanılır ve karşılık gelen değerleri depolamak ve almak için kullanılır. Gibi `/` bir karakter sınırlayıcı kullanarak hiyerarşik bir ad alanına anahtarları düzenlemek için `:`yaygın bir uygulamadır. Uygulamanız için en uygun kuralı kullanın. App Configuration tuşları bir bütün olarak ele alırım. Adlarının nasıl yapılandırıldığına karar vermek veya onlar üzerinde herhangi bir kural uygulamak için anahtarları ayrıştırmaz.

Aşağıda, hiyerarşi ye dönüştürülen anahtar adlara iki örnek verilmiştir:

* Bileşen hizmetlerine dayalı

        AppName:Service1:ApiEndpoint
        AppName:Service2:ApiEndpoint

* Dağıtım bölgelerine göre

        AppName:Region1:DbEndpoint
        AppName:Region2:DbEndpoint

Uygulama çerçeveleri içinde yapılandırma verilerinin kullanılması, anahtar değerler için belirli adlandırma düzenlerini dikte edebilir. Örneğin, Java'nın Bahar Bulutu `Environment` çerçevesi, bir Bahar uygulamasına ayar sağlayan kaynakları tanımlar.  Bunlar, *uygulama adı* ve *profilini*içeren değişkenler tarafından parametrelendirilmiştir. Bahar Bulutu ile ilgili yapılandırma verilerinin anahtarları genellikle bir sınır dışılayıcı yla ayrılmış bu iki öğeyle başlar.

Uygulama Yapılandırması'nda depolanan anahtarlar büyük/küçük harf duyarlı, unicode tabanlı dizeleri dir. App1 ve *App1* tuşları bir Uygulama Yapılandırma mağazasında farklıdır. *app1* Bazı çerçeveler yapılandırma anahtarlarını duyarsızca işlediğinden, uygulama içindeki yapılandırma ayarlarını kullandığınızda bunu aklınızda bulundurun. Anahtarları ayırt etmek için servis talebi kullanmanızı önermiyoruz.

Anahtar adlarında `*`unicode karakterini , ve `,`. `\`  Bu ayrılmış karakterlerden birini eklemeniz gerekiyorsa, kullanarak `\{Reserved Character}`ondan kurtulun. 

Anahtar değeri çiftinde 10 KB'lik birleşik boyut sınırı vardır. Bu sınır, anahtardaki tüm karakterleri, değerini ve ilişkili tüm isteğe bağlı öznitelikleri içerir. Bu sınır içinde, anahtarlar için birçok hiyerarşik düzeye sahip olabilirsiniz.

### <a name="design-key-namespaces"></a>Anahtar ad alanlarını tasarla

Yapılandırma verileri için kullanılan adlandırma tuşlarına iki genel yaklaşım vardır: düz veya hiyerarşik. Bu yöntemler uygulama kullanımı açısından benzerdir, ancak hiyerarşik adlandırma bir takım avantajlar sunar:

* Okuması daha kolay. Hiyerarşik anahtar ad işlevini tümcedeki boşluklar olarak sınırlandıranlar. Ayrıca kelimeler arasında doğal molalar sağlar.
* Yönetilmesi daha kolay. Anahtar ad hiyerarşisi mantıksal yapılandırma veri gruplarını temsil eder.
* Kullanımı daha kolay. Hiyerarşik bir yapıda tuşları eşleştiren ve yapılandırma verilerinin yalnızca bir bölümünü alan bir sorgu yazmak daha kolaydır. Ayrıca, birçok yeni programlama çerçevesi, uygulamanızın belirli yapılandırma kümelerinden yararlanabileceği hiyerarşik yapılandırma verileri için yerel desteğe sahiptir.

Uygulama Yapılandırması'ndaki anahtarları hiyerarşik olarak birçok şekilde düzenleyebilirsiniz. [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)gibi tuşları düşünün. Her hiyerarşik anahtar, sınır aşanlar tarafından birleştirilmiş bir veya daha fazla bileşenden oluşan bir kaynak *yoludur.* Uygulamanızın, programlama dilinizin veya çerçevenizin neye ihtiyacı olduğunu temel alan sınır layıcı olarak hangi karakteri kullanacağınızı seçin. Uygulama Yapılandırması'nda farklı tuşlar için birden fazla sınırlayıcı kullanın.

### <a name="label-keys"></a>Etiket anahtarları

App Configuration'daki temel değerler isteğe bağlı olarak bir etiket özniteliğine sahip olabilir. Etiketler, anahtar değerlerini aynı anahtarla ayırt etmek için kullanılır. *A* ve *B* etiketli anahtar *uygulaması,* bir Uygulama Yapılandırma mağazasında iki ayrı anahtar oluşturur. Varsayılan olarak, anahtar bir değerin etiketi yoktur. Etiket siz bir anahtar değeri açıkça `\0` başvurmak için kullanın `%00`(URL olarak kodlanmış).

Etiket, anahtarın türevlerini oluşturmak için kullanışlı bir yol sağlar. Etiketlerin yaygın kullanımı, aynı anahtar için birden çok ortam belirtmektir:

    Key = AppName:DbEndpoint & Label = Test
    Key = AppName:DbEndpoint & Label = Staging
    Key = AppName:DbEndpoint & Label = Production

### <a name="version-key-values"></a>Sürüm anahtar değerleri

Uygulama Yapılandırması anahtar değerlerini otomatik olarak sürüm yapmaz. Etiketleri, anahtar değerinin birden çok sürümü oluşturmak için bir yol olarak kullanın. Örneğin, belirli bir yazılım yapısıyla ilişkili temel değerleri tanımlamak için etiketlere bir uygulama sürüm numarası veya Git işleme kimliği girişi yapabilirsiniz.

Etiketlerde `*`unicode karakterini , ve `,`. `\` Bu karakterler ayrılmıştır. Ayrılmış bir karakter eklemek için, kullanarak `\{Reserved Character}`kaçmak gerekir.

### <a name="query-key-values"></a>Anahtar değerlerini sorgula

Her anahtar değeri, anahtarı ve bir etiket le `null`benzersiz olarak tanımlanır. Bir desen belirterek anahtar değerler için bir Uygulama Yapılandırma deposu sorgulayın. Uygulama Yapılandırma deposu, desenle eşleşen tüm temel değerleri ve bunların karşılık gelen değerlerini ve özniteliklerini döndürür. APP Configuration'a REST API aramalarında aşağıdaki anahtar desenleri kullanın:

| Anahtar | |
|---|---|
| `key`atlanır veya`key=*` | Tüm tuşlara uygun |
| `key=abc` | Eşler anahtar adı **abc** tam olarak |
| `key=abc*` | **ABC** ile başlayan anahtar adlarla eşleşir |
| `key=abc,xyz` | Anahtar adları **abc** veya **xyz**eşleşir. Beş CV ile sınırlıdır |

Ayrıca aşağıdaki etiket desenlerini de ekleyebilirsiniz:

| Etiketle | |
|---|---|
| `label`atlanır veya`label=*` | Herhangi bir etiketle eşleşen`null` |
| `label=%00` | Etiketlerle eşleşir `null` |
| `label=1.0.0` | Etiketleri **1.0.0** ile tam olarak eşleşir |
| `label=1.0.*` | **1.0** ile başlayan etiketlerle eşleşir. |
| `label=%00,1.0.0` | Etiketlerle `null` veya **1.0.0**ile eşleşir, beş CV ile sınırlıdır |

## <a name="values"></a>Değerler

Anahtarlara atanan değerler de unicode dizeleridir. Değerler için tüm unicode karakterleri kullanabilirsiniz. Her değerle ilişkili isteğe bağlı kullanıcı tanımlı bir içerik türü vardır. Uygulamanızın düzgün işlemesine yardımcı olan bir değer le ilgili bilgileri depolamak için bu özniteliği kullanın.

Bir Uygulama Yapılandırma deposunda depolanan yapılandırma verileri istirahatte ve aktarım sırasında şifrelenir. Tuşlar istirahatte şifrelenmez. Uygulama Yapılandırması Azure Key Vault için yedek bir çözüm değildir. Uygulama sırlarını içinde saklama.

## <a name="next-steps"></a>Sonraki adımlar

* [Belirli bir noktanın anlık görüntüsü](./concept-point-time-snapshot.md)  
* [Özellik yönetimi](./concept-feature-management.md)  
