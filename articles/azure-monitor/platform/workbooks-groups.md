---
title: Azure Izleyici çalışma kitapları grupları
description: Azure Izleyici çalışma kitaplarında grupları kullanma.
services: azure-monitor
author: lgayhardt
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: lagayhar
ms.openlocfilehash: b6377cdcdb5816426eba62fdbef79eeb42659dcc
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82892088"
---
# <a name="how-to-use-groups-in-workbooks"></a>Çalışma kitaplarında grupları kullanma

Çalışma kitabındaki bir Grup öğesi, bir çalışma kitabındaki bir adım kümesini mantıksal olarak gruplandırmanıza olanak tanır.

Çalışma kitaplarındaki gruplar birkaç şey için yararlıdır:

- Düzen
  - Öğelerin dikey olarak düzenlenmesini istediğiniz senaryolarda, tümünün yığılacağı bir öğe grubu oluşturabilir ve grubun stillendirilmesini bir yüzde genişliği olacak şekilde ayarlamak yerine tüm tek öğelerin yüzde genişliği olarak ayarlanmasını sağlayabilirsiniz.
- Görünürlük
  - Birçok öğenin birlikte gizlenmesi/gösterilmesi istediğiniz senaryolarda, her bir öğe için görünürlük ayarlarını yapmak yerine tüm öğe grubunun görünürlüğünü ayarlayabilirsiniz. Bu, sekme kullanan şablonlarda yararlı olabilir, böylece bir grup, sekmenin içeriği olarak kullanılabilir ve tüm grup, seçilen sekme tarafından ayarlanan bir parametreye göre gizlenebilir/gösterilir.
- Performans
  - Birçok bölüm veya sekmeye sahip çok büyük bir şablonunuz olduğu durumlarda, her bir bölümü kendi alt şablonuna dönüştürebilir ve en üst düzey şablon içindeki tüm alt şablonları yüklemek için grupları kullanabilirsiniz. Alt şablonların içeriği, Kullanıcı bu grupları görünür hale gelene kadar yüklenmez veya çalıştırılmaz. [Büyük bir şablonu birçok şablonla bölme](#how-to-split-a-large-template-into-many-templates)hakkında daha fazla bilgi edinin.

## <a name="using-groups"></a>Grupları kullanma

Çalışma kitabınıza bir grup eklemek için *Ekle* ' yi ve ardından *Grup Ekle*' yi seçin.

![Ekle ' yi seçin.](./media/workbooks-groups/add-group.png)

Aşağıda iki öğe içeren, okuma modundaki bir grubun ekran görüntüsü verilmiştir: metin öğesi ve sorgu öğesi.  

![Okuma modunda gruplandırın.](./media/workbooks-groups/groups-view.png)

Çalışma kitabını düzenlediğinizde, bu iki öğenin gerçekten bir Grup öğesi içinde olduğunu görebilirsiniz:

![Düzenleme modunda gruplandırın. ](./media/workbooks-groups/groups-edit.png)

Yukarıdaki ekran görüntüsünde, Grup düzenleme modundadır, bu da iki öğe içerir (kesikli alanın içinde). Her bir adım, düzenleme veya okuma modunda, birbirinden bağımsız olabilir. Örneğin, sorgu adımı okuma modundayken metin adımı düzenleme modundadır.

## <a name="scoping"></a>Kapsamlar

Şu anda, bir grup çalışma kitabında yeni bir kapsam olarak kabul edilir. Grupta oluşturulan Parametreler yalnızca Grup içinde görünür. Bu aynı zamanda birleştirme için de geçerlidir, verileri yalnızca kendi grubu içinde veya üst düzeyde görebilirsiniz.

## <a name="group-types"></a>Grup türleri

Çalışma kitabı grubu öğesi bir çalışma kitabına bir öğe grubu eklemenize olanak sağlar. Bir çalışma kitabının yazarı olarak, hangi grup türünü olacağını belirtirsiniz. İki tür grup vardır:

- Yapılamaz
  - Çalışma kitabındaki Grup, gruptaki öğelerin içeriğini eklemenize, kaldırmanıza veya düzenlemenize olanak tanır. Bu en yaygın olarak düzen ve görünürlük amaçlarıyla kullanılır.
- Şablondan
  - Çalışma kitabındaki Grup, başka bir şablonun içeriğinden KIMLIĞINE göre yüklenir. Çalışma zamanında çalışma kitabında bu şablonun içeriği yüklenir ve birleştirilir. Düzenleme modunda, öğenin bir dahaki sefer daha sonra yeniden yüklendiği için grubun içeriğini değiştiremezsiniz.  

## <a name="load-types"></a>Yük türleri

Bir grup içeriğinin yüklenebilmenin birkaç farklı yolu vardır. Bir çalışma kitabının yazarı olarak, bir grubun içeriğinin ne zaman ve nasıl yükleneceğini belirtmeniz gerekir.

### <a name="lazy-the-default"></a>Lazy (varsayılan)

Grup yalnızca öğe görünür olduğunda yüklenir. Bu, bir grubun sekme öğeleri tarafından kullanılmasına izin verir. Sekme hiç seçilmezse, Grup hiçbir şekilde görünmez olmaz, bu nedenle içerik yüklenmez.

Şablondan oluşturulan gruplar için, şablonun içeriği alınamaz ve grup görünür hale gelene kadar gruptaki öğeler oluşturulmaz. Kullanıcı, içerik alınırken tüm grup için ilerleme durumunu görür.

### <a name="explicit"></a>Anlaşılır

Bu modda, grubun nerede olacağı ve Kullanıcı içeriği yüklemek için düğmeye açıkça tıkladığı sürece hiçbir içerik alınmaz veya oluşturulmaz. Bu, içeriğin hesaplama veya nadiren kullanım açısından pahalı olabileceği senaryolarda faydalıdır. Yazar, düğme üzerinde görünecek metni belirtebilir.

Aşağıda, yapılandırılmış "daha fazla yükle" düğmesini gösteren açık yükleme ayarlarının bir ekran görüntüsü verilmiştir.

![Açık yükleme ayarları](./media/workbooks-groups/groups-explicitly-loaded.png)

Çalışma kitabına yüklenmeden önce Grup:

![Çalışma kitabına yüklenmeden önce açık grup](./media/workbooks-groups/groups-explicitly-loaded-before.png)

Çalışma kitabına yüklendikten sonra Grup:

![Çalışma kitabına yüklendikten sonra açık grup](./media/workbooks-groups/groups-explicitly-loaded-after.png)

### <a name="always"></a>Her zaman

Bu modda, grubun içeriği her zaman çalışma kitabı yüklenir yüklenmez yüklenir ve oluşturulur. Bu en yaygın olarak, içeriğin her zaman görünür olacağı Düzen amacıyla bir grup kullanılırken kullanılır.

## <a name="using-templates-inside-a-group"></a>Bir grup içindeki şablonları kullanma

Bir grup bir şablondan yüklenecek şekilde yapılandırıldığında, bu içerik varsayılan olarak yavaş yüklenir, yalnızca grup görünür olduğunda yüklenir.

Bir şablon bir gruba yüklendiğinde, çalışma kitabı, zaten grupta bulunan parametrelerle yüklenen şablonda belirtilen parametreleri birleştirmeye çalışır. Aynı ada sahip çalışma kitabında zaten bulunan parametreler, yüklenmekte olan şablondan birleştirilir. Bir parametre adımındaki tüm parametreler birleştirilmişse, tüm parametre adımı ortadan kaldırılır.

### <a name="example-1-all-parameters-have-identical-names"></a>Örnek 1: tüm parametrelerin adları aynı

En üstte iki parametre bulunan bir şablon düşünün.

- `TimeRange`-bir zaman aralığı parametresi.
- `Filter`-bir metin parametresi.

![Parametreleri Düzenle öğesi: "üst düzey parametreler"](./media/workbooks-groups/groups-top-level-params.png)

Sonra bir Grup öğesi, kendi iki parametresine sahip ikinci bir şablonu ve parametreleri aynı olarak adlandırılan bir metin adımını yükler:

![İkinci şablon için bir parametre öğesini Düzenle](./media/workbooks-groups/groups-merged-away.png)

İkinci şablon gruba yüklendiğinde, yinelenen parametreler birleştirilir. Parametrelerin tümü birleştirildiğinden, iç parametreler adımı da birleştirilir ve bu da yalnızca metin adımını içeren gruba yol açar.

### <a name="example-2-one-parameter-has-an-identical-name"></a>Örnek 2: bir parametre aynı ada sahip

En üstte iki parametre bulunan bir grubun şablonunu göz önünde bulundurun.

- `TimeRange`-bir zaman aralığı parametresi.
- `FilterB`-bir metin parametresi, en üstteki şablon gibi `Filter` değildir.

![Bir grup öğesini birleştirilmiş parametrelerin sonucuyla birlikte Düzenle](./media/workbooks-groups/groups-wont-merge-away.png)

Grubun ıtemı şablonu yüklendiğinde, `TimeRange` parametresi gruptan birleştirilir. Sonra çalışma kitabının başlangıç parametreleri adımı ve `TimeRange` `Filter`ile birlikte olacaktır ve grubun parametre adımı yalnızca şunları içerir`FilterB`

![ayrılmayacak parametrelerin sonucu](./media/workbooks-groups/groups-wont-merge-away-result.png)

Yüklenen şablon, ve `TimeRange` `Filter` (yerine `FilterB`) içeriyorsa, sonuçta elde edilen çalışma kitabında bir parametre adımı ve yalnızca metin adımının kalan bir grubu bulunur.

## <a name="how-to-split-a-large-template-into-many-templates"></a>Büyük bir şablonu birçok şablonla bölme

Performansı artırmak için, büyük bir şablonu, bazı içerik yavaş veya Kullanıcı tarafından talep üzerine yükleyen birden çok daha küçük şablonlara bölmek faydalıdır. Bu, üst düzey şablon çok daha küçük olacağından ilk yükü daha hızlı hale getirir.

Bir şablonu parçalara böldüğünde, şablonu her zaman tek tek çalışan çok sayıda şablona (alt şablonlar) bölmeniz gerekecektir. Bu nedenle, üst düzey şablonda diğer adımların kullandığı `TimeRange` bir parametre varsa, alt şablonun de tam ada sahip bir parametreyi tanımlayan bir Parameters adımı olmalıdır. Bu, alt şablonların bağımsız olarak çalışmasına ve gruplardaki daha büyük şablonların içinde yüklenmesine izin verir.

Daha büyük bir şablonu birden çok alt şablon olarak açmak için:

1.  Paylaşılan parametrelerden sonra çalışma kitabının üst kısmına yakın yeni bir boş grup oluşturun. Bu yeni Grup, sonunda bir alt şablon haline gelir.
2. Paylaşılan parametreler adımının bir kopyasını oluşturun ve sonra kopyayı 1. adımda oluşturulan gruba taşımak için *gruba taşı* ' yı kullanın. Bu parametre adımı alt şablonun dış şablondan bağımsız olarak çalışmasına izin verir ve dış şablon içinde yüklendiğinde birleştirilir.
    > [!NOTE]
    > Alt şablon, kendilerine hiçbir daha görünür durumda olmayan alt şablonları hiçbir şekilde düşünmüyorsanız, bu parametrelerin birleştirilmesinin gerekli olması gerekmez. Ancak, daha sonra yapmanız gerekirse, bunları düzenleme veya hata ayıklama işlemlerini çok zor hale getirir.

3. Alt şablonda olmasını istediğiniz çalışma kitabındaki her öğeyi, 1. adımda oluşturulan gruba taşıyın.
4. 3. adımda taşınan tek adımlarla koşullu görünürlükleriniz varsa, bu, dış grubun (sekmelerde kullanılan gibi) görünürlüğü olacak şekilde koşullu görünürlüğe sahiptir. Bunları Grup içindeki öğelerden kaldırın ve bu görünürlük ayarını grubun kendisine ekleyin. Değişiklik kaybını önlemek ve/veya JSON içeriğinin bir kopyasını dışarı aktarmak ve kaydetmek için buraya kaydedin.
5. Bu grubun bir şablondan yüklenmesini istiyorsanız gruptaki *düzenleme* araç çubuğu düğmesini kullanabilirsiniz. Bu, yeni bir pencerede çalışma kitabı olarak yalnızca bu grubun içeriğini açar. Daha sonra bu dosyayı uygun şekilde kaydedebilir ve bu çalışma kitabı görünümünü kapatabilirsiniz (tarayıcıyı kapatmazsanız, yalnızca düzenlediğiniz çalışma kitabına geri dönmek için bu görünümü kapatın).
6. Daha sonra Grup adımını Şablondan Yükle olarak değiştirebilir ve şablon KIMLIĞI alanını 5. adımda oluşturduğunuz çalışma kitabına/şablona ayarlayabilirsiniz. Çalışma kitapları kimlikleriyle çalışmak için kaynağın paylaşılan bir çalışma kitabı kaynak KIMLIĞI olması gerekir. *Yükle* ' ye basın ve bu grubun içeriği artık bu dış çalışma kitabının içine kaydedilmesi yerine bu alt şablondan yüklenecektir.

## <a name="next-steps"></a>Sonraki adımlar
- [Çalışma kitaplarına genel bakış](workbooks-overview.md)
- [Çalışma kitaplarında JSONPath kullanma](workbooks-jsonpath.md)