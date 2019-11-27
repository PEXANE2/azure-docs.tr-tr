---
title: Azure Farmtts 'ye Genel Bakış
description: Azure Farmtts 'ye genel bakış sağlar
author: uhabiba04
ms.topic: overview
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: ff28ae7dbff40910ac0431fb47f7f0be3e1569c0
ms.sourcegitcommit: 36eb583994af0f25a04df29573ee44fbe13bd06e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74538136"
---
# <a name="overview-of-azure-farmbeats"></a>Azure Farmtts 'ye Genel Bakış

Azure Farmzts, Azure hizmetleri ve özellikleri koleksiyonudur ve akıllı veri odaklı çözümleri hızla oluşturmanıza yardımcı olacak şekilde tasarlanmıştır. Azure Farmınts, derin veri Mühendisliği kaynaklarına yatırım yapmanıza gerek kalmadan sensörler, Drones, kameralar, uydu gibi çeşitli kaynaklardan tarım ile ilgili verileri almanızı, toplamanıza ve işlemenizi sağlayan bir Azure Marketi sunumudur.

> [!NOTE]
> Azure Farmtts Şu anda genel önizlemededir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Azure Farmtts, bir hizmet düzeyi sözleşmesi olmadan sağlanır. Destek için [Azure Farmtempts forumunu](https://aka.ms/FarmBeatsMSDN ) kullanın.

Azure Farmusing 'leri kullanarak, algılayıcı, uydu, Drones gibi çeşitli kaynaklardan verileri bir grup bağlamında (ilgilendiğiniz coğrafi alan) alabilirsiniz.

Şunları yapabileceksiniz:

- Çeşitli sağlayıcılardan toplam agricultürel veri kümeleri
- Farklı veri kümeleri kullanarak Fusing yapay zeka/makine dili (AI/ML) modellerini hızlı bir şekilde oluşturun

Azure Farm, aşağıdakileri yapmak için sağlam ve kolay bir yol sağlar:

- GeoJSON dosyası kullanarak Grup için bir eşleme oluşturun.
- Kültür dizinini ve su dizinini kullanarak, uydu imagery 'yi temel alan grup durumunu değerlendirin.
- Kaç algılayıcı ve bunların yerleştirileceği hakkında öneriler alın.
- Çeşitli algılayıcı satıcılarından algılayıcılar tarafından toplanan verileri görselleştirerek grup koşulunu izleyin.
- Uydu ve algılayıcı verilerini Fusion 'a göre SOIL nemi haritasını alın.
- Toplu veri kümelerinin üstünde AI/ML modelleri oluşturarak eyleme dönüştürülebilir Öngörüler elde edin.
- Grup durumu Danışma belgeleri sunarak dijital tarım çözümünüzü oluşturun veya geliştirin.

Azure Farmtempts bileşenleri, bu makalenin aşağıdaki bölümlerinde ele alınmıştır.

## <a name="data-hub"></a>Veri Merkezi

Azure Farmders veri merkezi, sağlayıcılar arasında çeşitli agriclaral veri kümelerinin toplanmasının, normalleştirilmesi ve kümeleştirilmesi sağlayan bir API katmanıdır. Bu önizlemeden itibaren, iki algılayıcı sağlayıcısı [Davis aletlerinin](https://www.davisinstruments.com/product/enviromonitor-gateway/), [Teralytic](https://teralytic.com/), One uydu canlandırın sağlayıcısı [Sentinel-2](https://sentinel.esa.int/web/sentinel/home)ve üç adet drone Imagery sağlayıcısı [senseuçarak](https://www.sensefly.com/) , [SlantRange](https://slantrange.com/) , [dji](https://dji.com/)özelliğinden yararlanabilirsiniz. Veri merkezi bir API platformu olarak tasarlanmıştır ve Azure Farmpts ile tümleştirilebilen birçok sağlayıcı ile çalışıyoruz, bu nedenle Çözümünüzü oluştururken daha fazla seçeneğiniz vardır.

## <a name="accelerator"></a>Hızlandır

Veri hub 'ında oluşturulan, Kullanıcı arabiriminizi ve model geliştirmeyi başlatan örnek bir çözüm. Bu Web uygulaması, alınan algılayıcı verilerinin görselleştirmeyi göstermek için API 'lerden yararlanır ve model çıkışının harita olarak görselliğini gösterir. Örneğin, hızlandırıcıyı hızlı bir şekilde bir grup oluşturmak ve bu grup için bir algılayıcı yerleştirme eşlemesi ya da kolayca almak için kullanabilirsiniz.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için bkz. Azure Farmtts [blogu](https://aka.ms/AzureFarmBeats) ve [forumları](https://aka.ms/FarmBeatsMSDN) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Farmtts 'yi kullanmaya başlamak için, dağıtım işlemini başlatmak üzere [Azure Marketi](https://aka.ms/FarmBeatsMarketplace) ' ni ziyaret edin.
