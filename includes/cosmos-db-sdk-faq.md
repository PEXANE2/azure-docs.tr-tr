---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 8/12/2020
ms.author: sngun
ms.openlocfilehash: 42d1e0f056457ba54e0102a4d23f42a81ebea08d
ms.sourcegitcommit: ac7029597b54419ca13238f36f48c053a4492cb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2020
ms.locfileid: "90068753"
---
**SDK'nın kullanımdan kaldırıldığı bana nasıl bildirilir?**

Microsoft desteklenen SDK'ya sorunsuz geçişi kolaylaştırmak için kullanımdan kaldırılan SDK'nın destek sonundan 12 ay önce bildirimde bulunacak. Size bu durumu çeşitli iletişim kanallarıyla bildireceğiz: Azure portal, Azure güncelleştirmeleri ve atanan hizmet yöneticilerine doğrudan iletişim.

**12 aylık dönem boyunca, kullanımdan kaldırılacak olan Azure Cosmos DB SDK'sını kullanarak uygulama yazabilir miyim?** 

Evet, 12 aylık bildirim dönemi boyunca kullanımdan kaldırılacak olan Azure Cosmos DB SDK'sını kullanarak uygulamaları yazabilir, dağıtabilir ve değiştirebilirsiniz. 12 aylık bildirim dönemi boyunca, uygun olduğunda Azure Cosmos DB SDK'sının desteklenen daha yeni bir sürümüne geçmenizi öneririz. 

**Kullanımdan kaldırma tarihinden sonra, desteklenmeyen Azure Cosmos DB SDK'sını kullanan uygulamalara ne olur?** 

Kullanımdan kaldırma tarihinden sonra Azure Cosmos DB artık kullanımdan kaldırılan SDK sürümlerinde hata düzeltmeleri yapmayacak, yeni özellikler eklemeyecek veya destek sağlamayacak. Yükseltmemeyi tercih ederseniz, SDK'nın kullanımdan kaldırılan sürümlerinden gönderilen isteklere Azure Cosmos DB hizmeti tarafından yanıt verilmeye devam edilecektir. 

**Hangi SDK sürümleri en son özellikleri ve güncelleştirmeleri içerecek?**

Yeni özellikler ve güncelleştirmeler yalnızca desteklenen en son ana SDK sürümünün en son alt sürümüne eklenecek. Yeni özelliklerden, performans geliştirmelerinden ve hata düzeltmelerinden yararlanmak için her zaman en son sürümü kullanmanızı öneririz. SDK'nın eski ama kullanımdan kaldırılmamış bir sürümünü kullanıyorsanız, Azure Cosmos DB'ye yönelik istekleriniz çalışmaya devam eder ama yeni özelliklerin hiçbirine erişemezsiniz.  

**Uygulamamı belirli bir kesme tarihinden önce güncelleştiremezsem ne yapmalıyım?**

Mümkün olan en kısa zamanda en son SDK'ya yükseltmenizi öneririz. SDK kullanımdan kaldırılmak üzere etiketlendikten sonra, uygulamanızı güncelleştirmek için 12 ayınız olacak. Kullanımdan kaldırma tarihine kadar güncelleştiremezseniz, SDK'nın kullanımdan kaldırılan sürümlerinden gönderilen isteklere Azure Cosmos DB tarafından yanıt verilmeye devam edecek, dolayısıyla çalışan uygulamalarınızda işlev kaybı olmayacak. Ama Azure Cosmos DB artık kullanımdan kaldırılan SDK sürümlerinde hata düzeltmeleri yapmayacak, yeni özellikler eklemeyecek veya destek sağlamayacak. 

Destek planınız varsa ve teknik desteğe gerek duyuyorsanız, bir destek bileti doldurarak [bizimle iletişime geçin](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
    


