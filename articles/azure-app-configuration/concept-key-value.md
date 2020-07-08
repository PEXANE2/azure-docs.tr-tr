---
title: Azure uygulama yapılandırma anahtar-değer deposunu anlama
description: Yapılandırma verilerinin Azure Uygulama yapılandırması 'nda nasıl depolandığını anlayın.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/19/2020
ms.openlocfilehash: b8f8bda52be63a4176411855dd9ff9919e9e31f5
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85856674"
---
# <a name="keys-and-values"></a>Anahtarlar ve değerler

Azure Uygulama yapılandırması, yapılandırma verilerini anahtar-değer çiftleri olarak depolar. Anahtar-değer çiftleri, geliştiriciler tarafından kullanılan uygulama ayarlarının basit ve esnek bir gösterimidir.

## <a name="keys"></a>Anahtarlar

Anahtarlar anahtar-değer çiftleri için tanımlayıcı olarak görev yapar ve karşılık gelen değerleri depolamak ve almak için kullanılır. Ya da gibi bir karakter sınırlayıcısı kullanarak, anahtarları hiyerarşik bir ad alanında düzenlemek yaygın bir uygulamadır `/` `:` . Uygulamanıza en uygun kuralı kullanın. Uygulama yapılandırması, anahtarları bir bütün olarak ele alır. Adlarının nasıl yapılandırıldığını veya üzerinde herhangi bir kuralın nasıl zorlanacağını anlamak için anahtarları ayrıştırmaz.

Aşağıda, bir hiyerarşiye yapılandırılmış anahtar adlarından oluşan iki örnek verilmiştir:

* Bileşen hizmetlerine göre

```aspx
        AppName:Service1:ApiEndpoint
        AppName:Service2:ApiEndpoint
```

* Dağıtım bölgelerine göre

```aspx
        AppName:Region1:DbEndpoint
        AppName:Region2:DbEndpoint
```

Uygulama çerçeveleri içindeki yapılandırma verilerinin kullanımı, anahtar değerleri için belirli adlandırma düzenlerini dikte edebilir. Örneğin, Java 'nın Spring Cloud Framework, `Environment` bir Spring uygulamasına ayarlar sağlayan kaynakları tanımlar.  Bunlar, *uygulama adı* ve *profili*içeren değişkenlere göre parametrelenir. Bulutta ilgili yapılandırma verileri için anahtarlar genellikle sınırlayıcıyla ayrılmış bu iki öğe ile başlar.

Uygulama yapılandırmasında depolanan anahtarlar büyük/küçük harfe duyarlıdır ve Unicode tabanlı dizelerdir. *APP1* ve *APP1* anahtarları bir uygulama yapılandırma deposunda farklıdır. Bir uygulama içinde yapılandırma ayarlarını kullandığınızda bu durumu aklınızda tutun çünkü bazı çerçeveler yapılandırma anahtarlarını işle-insensitively. Anahtarları ayırt etmek için büyük/küçük harf kullanılması önerilmez.

, Ve dışında, anahtar adlarında herhangi bir Unicode karakteri kullanabilirsiniz `*` `,` `\` .  Bu ayrılmış karakterlerden birini eklemeniz gerekiyorsa, bunu kullanarak kaçış `\{Reserved Character}` . 

Anahtar-değer çiftinde 10 KB 'lık Birleşik boyut sınırı vardır. Bu sınır, anahtardaki tüm karakterleri, değerini ve ilişkili tüm isteğe bağlı öznitelikleri içerir. Bu sınırın içinde, anahtarlar için birçok hiyerarşi düzeyine sahip olabilirsiniz.

### <a name="design-key-namespaces"></a>Anahtar ad alanlarını tasarlama

Yapılandırma verileri için kullanılan anahtarları adlandırırken iki genel yaklaşım vardır: düz veya hiyerarşik. Bu yöntemler bir uygulama kullanım açısından benzerdir, ancak hiyerarşik adlandırma çok sayıda avantaj sunar:

* Daha kolay okunabilir. Hiyerarşik anahtar adındaki sınırlayıcılar bir tümcede boşluk olarak işlev. Ayrıca, sözcükler arasında doğal kesmeler de sağlar.
* Daha kolay yönetilebilir. Anahtar adı hiyerarşisi, mantıksal yapılandırma verileri gruplarını temsil eder.
* Kullanımı daha kolay. Hiyerarşik bir yapıda, model ile eşleşen bir sorgu yazmak ve yapılandırma verilerinin yalnızca bir kısmını almak daha basittir. Ayrıca, birçok yeni programlama çerçevesi, uygulamanızın belirli yapılandırma kümelerinden kullanılabilmesini sağlamak üzere hiyerarşik yapılandırma verileri için yerel desteğe sahiptir.

Uygulama yapılandırmasındaki anahtarları birçok yolla hiyerarşik olarak düzenleyebilirsiniz. Bu anahtarları [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)olarak düşünün. Her hiyerarşik anahtar, sınırlayıcılarla birlikte birleştirilmiş bir veya daha fazla bileşenden oluşan bir kaynak *yoludur* . Uygulamanıza, programlama diline veya çerçeve gereksinimlerinize göre sınırlayıcı olarak kullanılacak karakteri seçin. Uygulama yapılandırmasındaki farklı anahtarlar için birden fazla sınırlayıcı kullanın.

### <a name="label-keys"></a>Etiket anahtarları

Uygulama yapılandırmasındaki anahtar değerler, isteğe bağlı olarak bir Label özniteliğine sahip olabilir. Etiketler, anahtar değerlerini aynı anahtarla ayırt etmek için kullanılır. *A* ve *B* etiketleri ile bir Key *APP1* , bir uygulama yapılandırma deposundaki iki ayrı anahtarı oluşturur. Varsayılan olarak, bir anahtar değerinin etiketi yoktur. Etiket olmadan bir anahtar değerine açık olarak başvurmak için `\0` (URL kodlamalı olarak) kullanın `%00` .

Etiket, bir anahtarın türevlerini oluşturmak için kullanışlı bir yol sağlar. Etiketlerin yaygın kullanımı, aynı anahtar için birden çok ortam belirtmektir:

```aspx
    Key = AppName:DbEndpoint & Label = Test
    Key = AppName:DbEndpoint & Label = Staging
    Key = AppName:DbEndpoint & Label = Production
```

### <a name="version-key-values"></a>Sürüm anahtarı değerleri

Uygulama yapılandırması, anahtar değerlerini otomatik olarak değiştirmez. Anahtar değerinin birden çok sürümünü oluşturmak için Etiketler kullanın. Örneğin, belirli bir yazılım derlemesi ile ilişkili anahtar değerlerini belirlemek için, etiketlerde bir uygulama sürümü numarası veya git COMMIT ID girişi yapabilirsiniz.

Etiketler içinde,, ve dışında herhangi bir Unicode karakteri `*` kullanabilirsiniz `,` `\` . Bu karakterler ayrılmıştır. Ayrılmış bir karakter eklemek için, kullanarak bunu atlamanız gerekir `\{Reserved Character}` .

### <a name="query-key-values"></a>Sorgu anahtarı değerleri

Her anahtar değeri, anahtarı ve olabilecek bir etiket tarafından benzersiz şekilde tanımlanır `null` . Bir model belirterek anahtar değerleri için bir uygulama yapılandırma deposu sorgulayın. Uygulama yapılandırma deposu, düzen ve bunlara karşılık gelen değerler ve özniteliklerle eşleşen tüm anahtar değerlerini döndürür. Uygulama yapılandırmasına yönelik REST API çağrılarında aşağıdaki temel desenleri kullanın:

| Anahtar | |
|---|---|
| `key`atlanır veya`key=*` | Tüm anahtarlarla eşleşir |
| `key=abc` | **ABC** anahtar adı tam olarak eşleşir |
| `key=abc*` | **ABC** ile başlayan anahtar adlarını eşleştirir |
| `key=abc,xyz` | **ABC** veya **xyz**anahtar adlarını eşleştirir. Beş CSV Ile sınırlıdır |

Ayrıca aşağıdaki etiket düzenlerini de dahil edebilirsiniz:

| Etiketle | |
|---|---|
| `label`atlanır veya`label=*` | İçeren herhangi bir etiketle eşleşir.`null` |
| `label=%00` | Eşleşir `null` etiketi |
| `label=1.0.0` | Label **1.0.0** ile tam olarak eşleşir |
| `label=1.0.*` | 1,0 ile başlayan etiketlerle eşleşir **.** |
| `label=%00,1.0.0` | `null`, Beş CSV ile sınırlı olan Etiketler veya **1.0.0**eşleşir |

## <a name="values"></a>Değerler

Anahtarlara atanan değerler de Unicode dizeleridir. Değerler için tüm Unicode karakterlerini kullanabilirsiniz. Her bir değerle ilişkili, Kullanıcı tanımlı isteğe bağlı bir içerik türü vardır. Uygulamanızın düzgün işlemesini sağlayan bir değer hakkındaki bilgileri depolamak için bu özniteliği kullanın.

Bir uygulama yapılandırma deposunda depolanan yapılandırma verileri, bekleyen ve aktarım sırasında şifrelenir. Anahtarlar bekleyen olarak şifrelenmez. Uygulama yapılandırması, Azure Key Vault için bir değiştirme çözümü değildir. Uygulama gizli dizileri üzerinde depolamamayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Belirli bir noktanın anlık görüntüsü](./concept-point-time-snapshot.md)  
* [Özellik yönetimi](./concept-feature-management.md)  
