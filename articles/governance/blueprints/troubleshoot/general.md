---
title: Sık karşılaşılan hataları giderme
description: İlke ihlalleri ve plan parametresi işlevleri gibi planlar oluşturma, atama ve kaldırma sorunlarıyla nasıl giderilen öğrenin.
ms.date: 01/15/2020
ms.topic: troubleshooting
ms.openlocfilehash: 7306e344a479008a87164a954c4444d375950b0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76157092"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Azure Planları'nı kullanarak hataları giderme

Planları oluştururken, atarken veya kaldırırken hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hatalar ve bunları nasıl çözeceğiniaçıklanır.

## <a name="finding-error-details"></a>Hata ayrıntılarını bulma

Birçok hata, bir kapsama bir plan atamanın sonucu olacaktır. Bir atama başarısız olduğunda, plan başarısız dağıtım hakkında ayrıntıları sağlar. Bu bilgiler, sorunu giderilebilmek ve bir sonraki dağıtımın başarılı olması için gösterir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki sayfadan **Atanmış planları** seçin ve başarısız atamayı bulmak için plan atamalarını filtrelemek için arama kutusunu kullanın. Ayrıca, başarısız olan tüm atamaların birlikte gruplandırılmadığını görmek için atamalar tablosunu **Geçici Durum** sütununa göre sıralayabilirsiniz.

1. _Başarısız_ durumu yla plana sol tıklayın veya sağ tıklatın ve **atama ayrıntılarını görüntüle'yi**seçin.

1. Atamanın başarısız olduğuna dair kırmızı bir afiş, plan atama sayfasının en üstündedir. Daha fazla bilgi almak için afiş her yerde tıklayın.

Hatanın bir yapının neden olduğu bir yapının nedeni olması yaygındır, bir bütün olarak plandan değil. Bir yapı Bir Anahtar Vault oluştururve Azure İlkesi Anahtar Vault oluşturmayı engellerse, tüm atama başarısız olur.

## <a name="general-errors"></a>Genel hatalar

### <a name="scenario-policy-violation"></a><a name="policy-violation"></a>Senaryo: İlke İhlali

#### <a name="issue"></a>Sorun

Şablon dağıtımı ilke ihlali nedeniyle başarısız oldu.

#### <a name="cause"></a>Nedeni

Bir ilke, dağıtımla birkaç nedenden dolayı çakışabilir:

- Oluşturulan kaynak ilke (genellikle SKU veya konum kısıtlamaları) ile sınırlıdır
- Dağıtım, ilke tarafından yapılandırılan alanları ayarlıyor (etiketlerle ortak)

#### <a name="resolution"></a>Çözüm

Planı, hata ayrıntılarındaki ilkelerle çakışmasın şekilde değiştirin. Bu değişiklik mümkün değilse, alternatif bir seçenek, planın artık ilkeyle çakışması için ilke atamasının kapsamının değiştirilmesidir.

### <a name="scenario-blueprint-parameter-is-a-function"></a><a name="escape-function-parameter"></a>Senaryo: Plan parametresi bir fonksiyondur

#### <a name="issue"></a>Sorun

İşlevler olan plan parametreleri yapılara geçirilmeden önce işlenir.

#### <a name="cause"></a>Nedeni

Bir işlev kullanan bir plan `[resourceGroup().tags.myTag]`parametresini, örneğin, bir yapıya geçirme, işlevin dinamik işlev yerine yapı üzerinde ayarlanan işlenmiş sonucuyla sonuçlanır.

#### <a name="resolution"></a>Çözüm

Bir işlevi parametre olarak geçirmek için, plan `[` parametresi gibi `[[resourceGroup().tags.myTag]`görünecek şekilde tüm dizeden kaçın. Kaçış karakteri, Blueprints'in planı işlerken değeri bir dize olarak işlemesine neden olur. Planlar daha sonra işlevi artifakı üzerine yerleştirir ve beklendiği gibi dinamik olmasını sağlar. Daha fazla bilgi için [Azure Kaynak Yöneticisi şablonlarında Sözdizimi ve ifadelere](../../../azure-resource-manager/templates/template-expressions.md)bakın.

## <a name="delete-errors"></a>Hataları silme

### <a name="scenario-assignment-deletion-timeout"></a><a name="assign-delete-timeout"></a>Senaryo: Atama silme zaman acısı

#### <a name="issue"></a>Sorun

Plan atamasının silinmesi tamamlanmaz.

#### <a name="cause"></a>Nedeni

Bir plan ataması silindiğinde terminal olmayan bir durumda sıkışıp kalabilir. Bu durum, plan ataması tarafından oluşturulan kaynaklar hala silinmeyi beklerken veya durum kodunu Azure Planları'na döndürmediğinde oluşur.

#### <a name="resolution"></a>Çözüm

Terminal dışı bir durumda ki plan _atamaları, 6 saatlik_ bir zaman anından sonra otomatik olarak **Başarısız** olarak işaretlenir. Zaman ayarı plan atamasının durumunu ayarladıktan sonra silme yeniden denenebilir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
- [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.