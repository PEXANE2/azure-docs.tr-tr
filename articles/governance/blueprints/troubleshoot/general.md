---
title: Sık karşılaşılan hataları giderme
description: Şemaları oluşturma, atama ve kaldırma ile ilgili sorunların nasıl giderileceği hakkında bilgi edinin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/11/2018
ms.topic: troubleshooting
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 8cbefcbadc14c1249d2783f1539e40c99c3be66c
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70231556"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Azure şemaları 'nı kullanarak hatalarda sorun giderme

Şemaları oluştururken veya atarken hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hatalar ve bunların nasıl çözümleneceği açıklanmaktadır.

## <a name="finding-error-details"></a>Hata ayrıntılarını bulma

Birçok hata, bir kapsamda şema atamanın sonucu olacaktır. Bir atama başarısız olduğunda, şema başarısız dağıtım hakkında ayrıntılar sağlar. Bu bilgiler sorunu çözülecektir ve sonraki dağıtım başarılı olur.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki sayfadan **atanan** şemalar ' ı seçin ve başarısız atamayı bulmak için şema atamalarını filtrelemek için arama kutusunu kullanın. Ayrıca, tüm başarısız atamaları birlikte gruplanmış olarak görmek için Atamalar tablosunu **sağlama durumu** sütununa göre sıralayabilirsiniz.

1. _Hatalı_ duruma sahip şema üzerine sol tıklayın veya sağ tıklayıp **atama ayrıntılarını görüntüle**' yi seçin.

1. Atamanın başarısız olduğu kırmızı bir başlık, BLUEPRINT atama sayfasının en üstünde yer alan bir uyarı. Daha fazla ayrıntı edinmek için başlık üzerinde herhangi bir yere tıklayın.

Hatanın bir bütün olarak değil bir yapıtın neden olması yaygındır. Bir yapıt Key Vault oluşturursa ve Azure Ilkesi Key Vault oluşturmayı engelliyorsa, tüm atama başarısız olur.

## <a name="general-errors"></a>Genel hatalar

### <a name="policy-violation"></a>Senaryon İlke Ihlali

#### <a name="issue"></a>Sorun

İlke ihlali nedeniyle şablon dağıtımı başarısız oldu.

#### <a name="cause"></a>Nedeni

Bir ilke, birkaç nedenden dolayı dağıtımla çakışabilir:

- Oluşturulan kaynak ilkeyle kısıtlıdır (genellikle SKU veya konum kısıtlamaları)
- Dağıtım, ilke tarafından yapılandırılan alanları ayarlıyor (etiketlerle ortak)

#### <a name="resolution"></a>Çözüm

Şema, hata ayrıntılarında ilkelerle çakışmayacak şekilde değiştirin. Bu değişiklik mümkün değilse, ilke atamasının kapsamının, şema ile çakışmadan, artık ilkeyle çakışmamasını sağlamak için alternatif bir seçenek bulunur.

### <a name="escape-function-parameter"></a>Senaryon Blueprint parametresi bir işlevdir

#### <a name="issue"></a>Sorun

İşlevler olan Blueprint parametreleri yapıtlara geçirilmeden önce işlenir.

#### <a name="cause"></a>Nedeni

Bir yapıtı gibi bir işlevi `[resourceGroup().tags.myTag]`kullanan bir şema parametresini geçirmek, dinamik işlev yerine yapıtın üzerinde ayarlanan işlevin işlenmiş sonucuna neden olur.

#### <a name="resolution"></a>Çözüm

Bir işlevi bir parametre olarak kullanarak geçirmek için, tüm dizeyi `[` , şema parametresi gibi `[[resourceGroup().tags.myTag]`görünen şekilde kaçış. Kaçış karakteri, şema, şema işlerken değeri bir dize olarak işlemeye neden olur. Daha sonra, planlar, işlevin beklenen şekilde dinamik olmasını sağlayan yapıtı üzerine koyar. Daha fazla bilgi için bkz. [şablon dosya yapısı-söz dizimi](../../../azure-resource-manager/resource-group-authoring-templates.md#syntax).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
- [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.