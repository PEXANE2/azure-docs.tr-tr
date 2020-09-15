---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 8/12/2020
ms.author: sngun
ms.openlocfilehash: 42d1e0f056457ba54e0102a4d23f42a81ebea08d
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90068753"
---
**Devre dışı bırakma SDK 'sını nasıl bilgilendiririm?**

Microsoft, desteklenen bir SDK 'ya sorunsuz bir geçiş yapılmasını kolaylaştırmak için devre dışı bırakma SDK 'sının destek sonundan önce 12 aylık öncelikli bildirim sağlayacaktır. Çeşitli iletişim kanalları aracılığıyla sizi bilgilendireceğiz: Azure portal, Azure güncelleştirmeleri ve atanan hizmet yöneticilerine doğrudan iletişim.

**12 aylık dönem boyunca Kullanımdan kaldırılmış Azure Cosmos DB SDK kullanarak uygulamalar yazabilir miyim?** 

Evet, 12 aylık bildirim döneminde devre dışı Azure Cosmos DB SDK 'sını kullanarak uygulamaları yazabilir, dağıtabilir ve değiştirebilirsiniz. Uygun şekilde, 12 aylık bildirim döneminde Azure Cosmos DB SDK 'nın daha yeni bir sürümüne geçiş yapmanızı öneririz. 

**Kullanımdan kaldırma tarihinden sonra, desteklenmeyen Azure Cosmos DB SDK kullanan uygulamalara ne olur?** 

Kullanımdan kaldırma tarihinden sonra, Azure Cosmos DB artık hata düzeltmeleri yapmayacaktır, yeni özellikler eklemez veya kullanımdan kaldırılan SDK sürümlerine destek sağlamalıdır. Yükseltmemeyi tercih ediyorsanız, SDK 'nın kullanımdan kaldırılan sürümlerinden gönderilen istekler Azure Cosmos DB hizmeti tarafından sunulmayı sürdürür. 

**Hangi SDK sürümleri en son özelliklere ve güncelleştirmelere sahip olacak?**

Yeni özellikler ve Güncelleştirmeler yalnızca en son desteklenen birincil SDK sürümünün en son ikincil sürümüne eklenecektir. Yeni özelliklerden, performans iyileştirmelerinden ve hata düzeltmelerinden yararlanmak için her zaman en son sürümü kullanmanızı öneririz. SDK 'nın eski, kullanımdan kaldırılmayan bir sürümünü kullanıyorsanız, Azure Cosmos DB istekleriniz çalışmaya devam eder, ancak yeni yeteneklere erişemezsiniz.  

**Bir kesme tarihinden önce uygulamamı güncelleştirediğimde ne yapmam gerekir?**

En son SDK 'ya mümkün olduğunca erken yükseltmeniz önerilir. Devre dışı bırakma için bir SDK etiketledikten sonra, uygulamanızı güncellemek için 12 aya sahip olacaksınız. Kullanımdan kaldırma tarihine göre güncelleştiremezsiniz, SDK 'nın kullanımdan kaldırılan sürümlerinden gönderilen istekler Azure Cosmos DB tarafından sunulmayacak ve böylece çalışan uygulamalarınız çalışmaya devam edecektir. Ancak Azure Cosmos DB artık hata düzeltmeleri yapmayacaktır, yeni özellikler eklemez veya kullanımdan kaldırılan SDK sürümlerine destek sağlamalıdır. 

Destek planınız varsa ve teknik desteğe ihtiyacınız varsa bir destek bileti kaydederek [bizimle iletişim kurun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) .
    


