---
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 8/12/2020
ms.author: sngun
ms.openlocfilehash: 5c5ed4e1bbe54c642202c19e1beb61de94b4f751
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88515252"
---
**1. kullanıcılara devre dışı bırakma SDK 'sının nasıl bildirilmesi gerekir?**

Microsoft, desteklenen bir SDK 'ya sorunsuz bir geçiş yapılmasını kolaylaştırmak için devre dışı bırakma SDK 'sının destek sonuna 12 aylık öncelikli bildirim sağlayacaktır. Ayrıca, müşteriler çeşitli iletişim kanalları (Azure portal, Azure güncelleştirmeleri ve atanan hizmet yöneticileri ile doğrudan iletişim aracılığıyla bilgilendirilir.

**2. müşteriler 12 aylık dönemde "yapılacak" Kullanımdan kaldırılmış Azure Cosmos DB SDK kullanarak uygulamalar yazar mı?** 

Evet, müşteriler 12 aylık bildirim döneminde "yapılacak" Kullanımdan kaldırılmış Azure Cosmos DB SDK 'sını kullanarak uygulamaları yazmak, dağıtmak ve değiştirmek için tam erişime sahip olur. 12 aylık bildirim döneminde, müşterilerin Azure Cosmos DB SDK 'nın daha yeni desteklenen bir sürümüne uygun şekilde geçirilmesi önerilir. 

**3. kullanımdan kaldırma tarihinden sonra, desteklenmeyen Azure Cosmos DB SDK kullanan uygulamalara ne olur?** 

Kullanımdan kaldırma tarihinden sonra, Azure Cosmos DB artık hata düzeltmeleri yapmayacaktır, yeni özellikler eklemeli ve kullanımdan kaldırılan SDK sürümlerine destek sağlamalıdır. Yükseltmemeyi tercih ediyorsanız, SDK 'nın kullanımdan kaldırılan sürümlerinden gönderilen istekler Azure Cosmos DB hizmeti tarafından sunulmayı sürdürür. 

**4. hangi SDK sürümlerinde en son özellikler ve güncelleştirmeler olacaktır?**

Yeni özellikler ve güncelleştirmeler, yalnızca en son desteklenen birincil SDK sürümünün en son ikincil sürümüne eklenecektir. Yeni özellikler, performans iyileştirmeleri ve hata düzeltmelerinin avantajlarından yararlanmak için her zaman en son sürümü kullanmanız önerilir. SDK 'nın eski, kullanımdan kaldırılmayan bir sürümünü kullanıyorsanız Azure Cosmos DB için istekleriniz çalışmaya devam eder, ancak yeni yetenekler için erişiminiz olmayacaktır.  

**5. uygulamamı bir kesme tarihinden önce güncelleştirediğimde ne yapmam gerekir?**

En son SDK 'ya mümkün olduğunca erken yükseltmeniz önerilir. Devre dışı bırakma için bir SDK etiketledikten sonra uygulamanızı güncellemek için 12 aya sahip olursunuz. Kullanımdan kaldırma tarihine göre güncelleştiremezsiniz, SDK 'nın kullanımdan kaldırılan sürümlerinden gönderilen istekler Azure Cosmos DB tarafından sunulmayacak ve böylece çalışan uygulamalarınız çalışmaya devam edecektir. Ancak, Azure Cosmos DB artık hata düzeltmeleri yapamayacak, yeni özellikler ekleyecek ve kullanımdan kaldırılan SDK sürümlerine destek sağlamalıdır. 

Destek planınız varsa ve teknik desteğe ihtiyacınız varsa lütfen bir destek bileti kaydederek [bizimle iletişim kurun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) .
    


