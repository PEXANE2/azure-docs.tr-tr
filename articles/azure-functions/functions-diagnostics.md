---
title: Azure Işlevleri tanılamaları genel bakış
description: Azure Işlevleri Tanılama ile işlev uygulamanızla ilgili sorunları nasıl giderebileceğinizi öğrenin.
author: yunjchoi
ms.topic: article
ms.date: 11/01/2019
ms.author: yunjchoi
ms.custom: na
ms.openlocfilehash: fb329273d442c023233fee2e7e01aabe5f5bff8c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83122383"
---
# <a name="azure-functions-diagnostics-overview"></a>Azure Işlevleri tanılamaları genel bakış

Bir işlev uygulaması çalıştırırken, hataları tetikleyebilmesi için 4xx hatasından kaynaklanan tüm sorunlar için hazırlıklı olmak istersiniz. Azure Işlevleri tanılama, işlev uygulamanızda yapılandırma veya ek maliyet olmadan sorun gidermenize yardımcı olacak akıllı ve etkileşimli bir deneyimdir. İşlev uygulamanız ile ilgili sorunlar yaşıyorsanız, Azure Işlevleri Tanılaması, nelerin yanlış olduğunu gösterir. Sizi daha kolay ve hızlı bir şekilde gidermek ve sorunu gidermek için size doğru bilgilere kılavuzluk eder. Bu makalede, yaygın işlev uygulaması sorunlarını daha hızlı tanılamak ve çözmek için Azure Işlevleri tanılamayı kullanma hakkında temel bilgiler verilmektedir.

## <a name="start-azure-functions-diagnostics"></a>Azure Işlevleri tanılamayı başlatma

Azure Işlevleri tanılamayı başlatmak için:

1. [Azure Portal](https://portal.azure.com)işlev uygulamanıza gidin.
1. Azure Işlevleri tanılamayı açmak için **Tanıla ve sorunları çöz '** ü seçin.
1. Giriş sayfası kutucuğunda anahtar sözcükleri kullanarak işlev uygulamanızın sorununu en iyi şekilde açıklayan bir kategori seçin. Ayrıca, arama çubuğunda sorununuzu en iyi açıklayan bir anahtar sözcük yazabilirsiniz. Örneğin, `execution` işlev uygulaması yürütmeyle ilgili tanılama raporlarının listesini görmek ve bunları doğrudan giriş sayfasından açmak için yazabilirsiniz.

   :::image type="content" source="./media/functions-diagnostics/functions-app-search-azure-functions-diagnostics.png" alt-text="Azure Işlevleri tanılamayı arayın." border="true":::

## <a name="use-the-interactive-interface"></a>Etkileşimli arabirimi kullanma

İşlev uygulamanızın sorunuyla en iyi şekilde hizalanan bir giriş sayfası kategorisi seçtiğinizde, Genie adlı Azure Işlevleri Tanılama ' etkileşimli arabirimi, uygulamanızın sorununu tanılamak ve çözmek için sizi yönlendirebilir. İlgilendiğiniz sorun kategorisinin tam Tanılama raporunu görüntülemek için Genie tarafından sunulan kutucuk kısayollarını kullanabilirsiniz. Kutucuk kısayolları, tanılama ölçümlerinize erişmenin doğrudan bir yolunu sağlar.

:::image type="content" source="./media/functions-diagnostics/genie.png" alt-text="Genie, Azure Işlevleri tanılamaları ' arabirimidir." border="false":::

Bir kutucuk seçtikten sonra kutucukta açıklanan sorunla ilgili konuların bir listesini görebilirsiniz. Bu konular, tüm rapordaki önemli bilgi parçacıklarını sağlar. Sorunları daha fazla araştırmak için aşağıdaki konulardan birini seçin. Ayrıca, tek bir sayfadaki tüm konuları araştırmak için **tam raporu görüntüle** seçeneğini belirleyebilirsiniz.

:::image type="content" source="./media/functions-diagnostics/preview-of-diagnostic-report.png" alt-text="Tanılama raporunun önizlemesi" border="false":::

## <a name="view-a-diagnostic-report"></a>Tanılama raporunu görüntüle

Bir konu seçtikten sonra, işlev uygulamanıza özgü bir tanılama raporu görüntüleyebilirsiniz. Tanılama raporları, uygulamanız ile ilgili herhangi bir sorun olup olmadığını göstermek için durum simgeleri kullanır. Sorunun ayrıntılı açıklaması, Önerilen Eylemler, ilgili ölçümler ve yararlı belgeler görürsünüz. Özelleştirilmiş tanılama raporları, işlev uygulamanızda çalıştırılan bir denetim serisinden oluşturulur. Tanılama raporları, işlev uygulamanızda sorun giderme için yararlı bir araç olabilir ve sorunu çözmeye yönelik size rehberlik eder.

## <a name="find-the-problem-code"></a>Sorun kodunu bulun

Betik tabanlı işlevler için, özel durumlara veya hatalara neden olan kod satırında daraltmak üzere **işlev uygulaması azaltma veya raporlama hataları** altında **Işlev yürütme ve hataları** kullanabilirsiniz. Bu aracı, kök nedeni almak ve belirli bir kod satırından sorunları gidermek için kullanabilirsiniz. Bu seçenek, önceden derlenmiş C# ve Java işlevleri için kullanılamaz.

:::image type="content" source="./media/functions-diagnostics/diagnostic-report-on-function-execution-errors.png" alt-text="İşlev yürütme hatalarında tanılama raporu" border="false":::

:::image type="content" source="./media/functions-diagnostics/function-exception.png" alt-text="Özel durum ayrıntılarının görünümü." border="false":::

## <a name="next-steps"></a>Sonraki adımlar

[UserVoice](https://feedback.azure.com/forums/355860-azure-functions)'Ta Azure işlevleri tanılamaları hakkında sorular sorabilir veya geri bildirim sağlayabilirsiniz. `[Diag]`Geri bildirimlerinizin başlığına dahil edin.

> [!div class="nextstepaction"]
> [İşlev uygulamalarınızı izleyin](functions-monitoring.md)
