---
title: Azure İşlevleri tanılama genel bakış
description: Azure İşlevleri tanılama ile işlev uygulamanızla ilgili sorunları nasıl giderebileceğinizi öğrenin.
author: yunjchoi
ms.topic: article
ms.date: 11/01/2019
ms.author: yunjchoi
ms.custom: na
ms.openlocfilehash: 41acc5703f6b6e5cdeedf2afd0dc61b23bc579c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834044"
---
# <a name="azure-functions-diagnostics-overview"></a>Azure İşlevleri tanılama genel bakış

Bir işlev uygulaması çalıştırırken, 4xx hatalarından tetikleyici hatalara kadar ortaya çıkabilecek sorunlara karşı hazırlıklı olmak istersiniz. Azure İşlevler tanılama, işlev uygulamanızı yapılandırma veya ekstra ücret ödemeden sorun gidermenize yardımcı olan akıllı ve etkileşimli bir deneyimdir. İşlev uygulamanızla ilgili sorunlarla karşılaştığınızda, Azure İşlevleri tanılama, sorunu daha kolay ve hızlı bir şekilde sorun giderme ve çözme niz için doğru bilgilere rehberlik etmede neyin yanlış olduğunu işaret eder. Bu makalede, ortak işlev uygulaması sorunlarını daha hızlı tanılamak ve çözmek için Azure İşlevleri tanılamalarının nasıl kullanılacağına iliş hale getirilmektedir.

## <a name="start-azure-functions-diagnostics"></a>Azure İşlevleri tanılamayı başlatın

Azure İşlevler tanısına erişmek için:

1. [Azure portalındaki](https://portal.azure.com)işlev uygulamanıza gidin.
2. Platform **özellikleri** sekmesini seçin.
3. Azure İşlevleri tanılamayı açan **Kaynak Yönetimi**altında **Tanıla ve sorunları çöz'ü** seçin.
4. Ana sayfa döşemesindeki anahtar kelimeleri kullanarak işlev uygulamanızın sorununu en iyi açıklayan bir kategori seçin. Ayrıca, arama çubuğunda sorununuzu en iyi açıklayan bir anahtar kelime de yazabilirsiniz. Örneğin, işlev uygulama `execution` yürütme ile ilgili tanılama raporlarının listesini görmek için yazabilir ve bunları doğrudan ana sayfadan açabilirsiniz.

![Giriş sayfası](./media/functions-diagnostics/homepage.png)

## <a name="use-the-interactive-interface"></a>Etkileşimli arabirimi kullanma

İşlev uygulamanızın sorunuyla en iyi hizalayan bir ana sayfa kategorisi seçtiğinizde, Azure İşleme tanılama biriminin etkileşimli arabirimi Cin, uygulamanızın tanılama ve sorunlarını çözme konusunda size yol görebilir. İlgilendiğiniz sorun kategorisinin tam tanılama raporunu görüntülemek için Cin tarafından sağlanan döşeme kısayollarını kullanabilirsiniz. Döşeme kısayolları, tanılama ölçümlerinize doğrudan erişmenizi sağlar.

![Cin](./media/functions-diagnostics/genie.png)

Bir döşeme seçtikten sonra, döşemede açıklanan sorunla ilgili konuların listesini görebilirsiniz. Bu konular, raporun tamamından kayda değer bilgiler sağlar. Sorunları daha fazla araştırmak için bu konulardan herhangi birini seçebilirsiniz. Ayrıca, tüm konuları tek bir sayfadaki incelemek için **Tam Raporu Görüntüle'yi** seçebilirsiniz.

![Tanılama raporunun önizlemesi](./media/functions-diagnostics/preview-of-diagnostic-report.png)

## <a name="view-a-diagnostic-report"></a>Tanılama raporunu görüntüleme

Bir konu seçtikten sonra, işlev uygulamanıza özgü bir tanılama raporunu görüntüleyebilirsiniz. Tanılama raporları, uygulamanızla ilgili belirli bir sorun olup olmadığını belirtmek için durum simgelerini kullanır. Sorunun ayrıntılı açıklamasını, önerilen eylemleri, ilgili ölçümleri ve yararlı dokümanları görürsünüz. Özelleştirilmiş tanılama raporları, işlev uygulamanızda çalıştırılan bir dizi denetimden oluşturulur. Tanılama raporları, işlev uygulamanızdaki sorunları saptamak ve sorunu çözmeniz için size rehberlik etmek için yararlı bir araç olabilir.

## <a name="find-the-problem-code"></a>Sorun kodunu bulma

Komut dosyası tabanlı işlevler için, özel **durumlara** veya hatalara neden olan kod çizgisini daraltmak için İşlev Uygulaması Aşağı Veya Raporlama Hataları altında **İşlev Yürütme** ve Hataları kullanabilirsiniz. Bu özellik, kök nedeni almak ve belirli bir kod satırından sorunları gidermek için yararlı bir araç olabilir. Bu seçenek önceden derlenmiş C# ve Java işlevleri için kullanılamaz.

![İşlev yürütme hataları hakkında tanılama raporu](./media/functions-diagnostics/diagnostic-report-on-function-execution-errors.png)

![İşlev özel durumu](./media/functions-diagnostics/function-exception.png)

## <a name="next-steps"></a>Sonraki adımlar

[UserVoice'ta](https://feedback.azure.com/forums/355860-azure-functions)Azure İşlevleri tanılama hakkında soru sorabilir veya geri bildirim de alabilirsiniz. Lütfen `[Diag]` geri bildiriminizin başlığına ekleyin.

> [!div class="nextstepaction"]
> [İşlev uygulamalarınızı izleme](functions-monitoring.md)
