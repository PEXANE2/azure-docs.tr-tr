---
title: Azure uygulama yapılandırma anahtar-değer deposunu anlama
description: Yapılandırma verilerini anahtar-değer olarak depolayan Azure Uygulama yapılandırması 'nda anahtar-değer depolama alanını anlayın. Anahtar değerleri, uygulama ayarlarının bir gösterimidir.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/19/2020
ms.openlocfilehash: b1998532c3d9e4272d91280d57d9ea2f6e7a262c
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88586401"
---
# <a name="keys-and-values"></a>Anahtarlar ve değerler

Azure Uygulama yapılandırması, yapılandırma verilerini anahtar-değer olarak depolar. Anahtar değerleri, geliştiriciler tarafından kullanılan uygulama ayarlarının basit ve esnek bir gösterimidir.

## <a name="keys"></a>Anahtarlar

Anahtarlar anahtar değerleri için tanımlayıcı olarak görev yapar ve karşılık gelen değerleri depolamak ve almak için kullanılır. Ya da gibi bir karakter sınırlayıcısı kullanarak, anahtarları hiyerarşik bir ad alanında düzenlemek yaygın bir uygulamadır `/` `:` . Uygulamanıza en uygun kuralı kullanın. Uygulama yapılandırması, anahtarları bir bütün olarak ele alır. Adlarının nasıl yapılandırıldığını veya üzerinde herhangi bir kuralın nasıl zorlanacağını anlamak için anahtarları ayrıştırmaz.

Bileşen hizmetlerine dayalı bir hiyerarşiye yapılandırılmış anahtar adları örneği aşağıda verilmiştir:

```aspx
    AppName:Service1:ApiEndpoint
    AppName:Service2:ApiEndpoint
```

Uygulama çerçeveleri içindeki yapılandırma verilerinin kullanımı, anahtar değerleri için belirli adlandırma düzenlerini dikte edebilir. Örneğin, Java 'nın Spring Cloud Framework, `Environment` bir Spring uygulamasına ayarlar sağlayan kaynakları tanımlar.  Bunlar, *uygulama adı* ve *profili*içeren değişkenlere göre parametrelenir. Bulutta ilgili yapılandırma verileri için anahtarlar genellikle sınırlayıcıyla ayrılmış bu iki öğe ile başlar.

Uygulama yapılandırmasında depolanan anahtarlar büyük/küçük harfe duyarlıdır ve Unicode tabanlı dizelerdir. *APP1* ve *APP1* anahtarları bir uygulama yapılandırma deposunda farklıdır. Bir uygulama içinde yapılandırma ayarlarını kullandığınızda bu durumu aklınızda tutun çünkü bazı çerçeveler yapılandırma anahtarlarını işle-insensitively. Anahtarları ayırt etmek için büyük/küçük harf kullanılması önerilmez.

Dışında, anahtar adlarında herhangi bir Unicode karakteri kullanabilirsiniz `%` . Anahtar adı `.` ya `..` da olamaz. Anahtar değerinde 10 KB 'lık Birleşik boyut sınırı vardır. Bu sınır, anahtardaki tüm karakterleri, değerini ve ilişkili tüm isteğe bağlı öznitelikleri içerir. Bu sınırın içinde, anahtarlar için birçok hiyerarşi düzeyine sahip olabilirsiniz.

### <a name="design-key-namespaces"></a>Anahtar ad alanlarını tasarlama

Yapılandırma verileri için kullanılan anahtarları adlandırırken iki genel yaklaşım vardır: düz veya hiyerarşik. Bu yöntemler bir uygulama kullanım açısından benzerdir, ancak hiyerarşik adlandırma çok sayıda avantaj sunar:

* Daha kolay okunabilir. Hiyerarşik anahtar adındaki sınırlayıcılar bir tümcede boşluk olarak işlev. Ayrıca, sözcükler arasında doğal kesmeler de sağlar.
* Daha kolay yönetilebilir. Anahtar adı hiyerarşisi, mantıksal yapılandırma verileri gruplarını temsil eder.
* Kullanımı daha kolay. Hiyerarşik bir yapıda, model ile eşleşen bir sorgu yazmak ve yapılandırma verilerinin yalnızca bir kısmını almak daha basittir. Ayrıca, birçok yeni programlama çerçevesi, uygulamanızın belirli yapılandırma kümelerinden kullanılabilmesini sağlamak üzere hiyerarşik yapılandırma verileri için yerel desteğe sahiptir.

Uygulama yapılandırmasındaki anahtarları birçok yolla hiyerarşik olarak düzenleyebilirsiniz. Bu anahtarları [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)olarak düşünün. Her hiyerarşik anahtar, sınırlayıcılarla birlikte birleştirilmiş bir veya daha fazla bileşenden oluşan bir kaynak *yoludur* . Uygulamanıza, programlama diline veya çerçeve gereksinimlerinize göre sınırlayıcı olarak kullanılacak karakteri seçin. Uygulama yapılandırmasındaki farklı anahtarlar için birden fazla sınırlayıcı kullanın.

### <a name="label-keys"></a>Etiket anahtarları

Uygulama yapılandırmasındaki anahtar değerleri, isteğe bağlı olarak bir Label özniteliğine sahip olabilir. Etiketler, anahtar değerlerini aynı anahtarla ayırt etmek için kullanılır. *A* ve *B* etiketleri ile bir Key *APP1* , bir uygulama yapılandırma deposundaki iki ayrı anahtarı oluşturur. Varsayılan olarak, bir anahtar değeri etiket içermez. Etiketi olmayan bir anahtar değere açık olarak başvurmak için `\0` (URL kodlamalı olarak `%00` ) kullanın.

Etiket, bir anahtarın türevlerini oluşturmak için kullanışlı bir yol sağlar. Etiketlerin yaygın kullanımı, aynı anahtar için birden çok ortam belirtmektir:

```
    Key = AppName:DbEndpoint & Label = Test
    Key = AppName:DbEndpoint & Label = Staging
    Key = AppName:DbEndpoint & Label = Production
```

### <a name="version-key-values"></a>Sürüm anahtarı-değerler

Anahtar-değer 'nin birden çok sürümünü oluşturmak için etiketleri kullanın. Örneğin, belirli bir yazılım derlemesi ile ilişkili anahtar değerlerini belirlemek için, etiketlerde bir uygulama sürümü numarası veya git COMMIT ID girişi yapabilirsiniz.

> [!NOTE]
> Değişiklik sürümleri arıyorsanız, uygulama yapılandırması bir anahtar-değer değişikliklerinin tüm değişiklikleri otomatik olarak geçmiş belirli bir süre içinde tutar. Daha fazla ayrıntı için bkz. [Noktadan noktaya anlık görüntü](./concept-point-time-snapshot.md) .

### <a name="query-key-values"></a>Sorgu anahtarı-değerleri

Her anahtar değeri, anahtarı ve olabilecek bir etiket tarafından benzersiz şekilde tanımlanır `\0` . Bir model belirterek anahtar değerleri için bir uygulama yapılandırma deposu sorgulayın. Uygulama yapılandırma deposu, karşılık gelen değerleri ve öznitelikleri dahil olmak üzere tüm anahtar değerlerini döndürür. Uygulama yapılandırmasına yönelik REST API çağrılarında aşağıdaki temel desenleri kullanın:

| Anahtar | Açıklama |
|---|---|
| `key` atlanır veya `key=*` | Tüm anahtarlarla eşleşir |
| `key=abc` | **ABC** anahtar adı tam olarak eşleşir |
| `key=abc*` | **ABC** ile başlayan anahtar adlarını eşleştirir |
| `key=abc,xyz` | **ABC** veya **xyz**anahtar adlarını eşleştirir. Beş CSV Ile sınırlıdır |

Ayrıca aşağıdaki etiket düzenlerini de dahil edebilirsiniz:

| Etiketle | Açıklama |
|---|---|
| `label` atlanır veya `label=*` | İçeren herhangi bir etiketle eşleşir. `\0` |
| `label=%00` | Eşleşir `\0` etiketi |
| `label=1.0.0` | Label **1.0.0** ile tam olarak eşleşir |
| `label=1.0.*` | 1,0 ile başlayan etiketlerle eşleşir **.** |
| `label=%00,1.0.0` | `\0`, Beş CSV ile sınırlı olan Etiketler veya **1.0.0**eşleşir |

> [!NOTE]
> `*`, `,` ve, `\` sorgularda ayrılmış karakterlerdir. Anahtar adlarınızda veya etiketlerinizde ayrılmış bir karakter kullanılıyorsa, sorgularda kullanarak bunu atlamanız gerekir `\{Reserved Character}` .

## <a name="values"></a>Değerler

Anahtarlara atanan değerler de Unicode dizeleridir. Değerler için tüm Unicode karakterlerini kullanabilirsiniz.

### <a name="use-content-type"></a>Content-Type kullanın
Uygulama yapılandırmasındaki her anahtar-değer bir Content-Type özniteliğine sahiptir. Bu özniteliği, uygulamanızın doğru şekilde işlemesini sağlayan anahtar değerindeki değer türü hakkındaki bilgileri depolamak için isteğe bağlı olarak kullanabilirsiniz. İçerik türü için herhangi bir biçim kullanabilirsiniz. Uygulama yapılandırması, özellik bayrakları, Key Vault başvuruları ve JSON anahtar değerleri gibi yerleşik veri türleri için [medya türlerini]( https://www.iana.org/assignments/media-types/media-types.xhtml) (MIME türleri olarak da bilinir) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Belirli bir noktanın anlık görüntüsü](./concept-point-time-snapshot.md)
* [Özellik yönetimi](./concept-feature-management.md)
* [Olay işleme](./concept-app-configuration-event.md)
