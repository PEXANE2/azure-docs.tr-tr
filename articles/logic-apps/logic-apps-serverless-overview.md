---
title: Genel Bakış - Bulut tabanlı uygulamalar ve çözümler için Azure Sunucusuz
description: Azure Logic Apps ve Azure İşlevlerini kullanarak altyapı konusunda endişelenmeden bulut tabanlı uygulamalar ve çözümler oluşturmayı öğrenin
services: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: klam, estfan, logicappspm
ms.topic: article
ms.date: 03/30/2017
ms.openlocfilehash: 0f20bb5fb249ad6bac862afe2b0e8eee4b32e2a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666559"
---
# <a name="azure-serverless-overview-for-building-cloud-based-apps-and-solutions-with-azure-logic-apps-and-azure-functions"></a>Azure Sunucusuz: Azure Logic Apps ve Azure Fonksiyonları ile bulut tabanlı uygulamalar ve çözümler oluşturmaya genel bakış

[Sunucusuz](https://azure.microsoft.com/solutions/serverless/) uygulamalar, artırılmış geliştirme hızı, azaltılmış kod, basitlik ve ölçek gibi avantajlar sunar. Bu makale, sunucusuz çözümlerin ve Azure sunucusuz tekliflerin farklı özelliklerini kapsar.

## <a name="what-is-serverless"></a>Sunucusuz nedir?

Sunucusuz hiçbir sunucu olduğu anlamına gelmez, ama daha ziyade geliştiriciler sunucular hakkında endişelenmenize gerek yok. Geleneksel uygulama geliştirmenin büyük bir kısmı, uygulamanın taleplerini karşılamak için ölçekleme, barındırma ve izleme çözümleri yle ilgili soruları yanıtlamaktır. Sunucusuz ile, bu sorular çözümün bir parçası olarak halledilir. Ayrıca, sunucusuz uygulamalar tüketim tabanlı bir planda faturalandırılır. Uygulama hiç kullanılmamışsa, herhangi bir ücret alınmaz. Bu özellikler, geliştiricilerin yalnızca çözümün iş mantığına odaklanmalarına yardımcı olur.

Sunucusuzlar için temel Azure hizmetleri [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) ve Azure [İşlevleri'dir.](https://azure.microsoft.com/services/functions/) Her iki çözüm de daha önce açıklanan ilkelere uyar ve geliştiricilerin en az kodla sağlam bulut uygulamaları oluşturmasına yardımcı olur.

## <a name="what-is-azure-logic-apps"></a>Azure Logic Apps nedir?

[Azure Logic Apps,](logic-apps-overview.md) buluttaki ölçeklenebilir tümleştirmeleri ve iş akışlarını basitleştirmenin ve uygulamanın bir yolunu sağlar. Bu hizmet, iş akışı adı verilen bir dizi adım olarak işleminizi modellemek ve otomatikleştirmek için görsel bir tasarımcı sağlar. Bulut hizmetlerinde ve şirket içi sistemlerde sunucusuz bir uygulamayı diğer API'lere hızla bağlayan birçok [bağlayıcı](../connectors/apis-list.md) vardır. Her mantık uygulaması "Dynamics CRM'ye bir hesap eklendiğinde" gibi bir tetikleyiciyle başlar. Tetikleyici yangınlardan sonra, iş akışı eylem, dönüşüm ve koşullu mantık birleşimlerini çalıştırabilir. Logic Apps, bir işlemde farklı Azure Işlevlerini yönetirken, özellikle de işlem harici bir sistem veya API ile etkileşim gerektirdiğinde mükemmel bir seçimdir.

Logic Apps'a başlamak için [ilk mantık uygulamanızı oluşturmaya](quickstart-create-first-logic-app-workflow.md)başlayın. Logic Apps hakkında daha fazla teknik bilgi için [geliştirici referansına](logic-apps-workflow-definition-language.md)bakın.

## <a name="what-is-azure-functions"></a>Azure İşlevleri nedir?

Azure İşlevler, bulutta kod parçalarını veya "işlevleri" kolayca çalıştırmak için bir hizmettir. Tüm uygulama veya gerekli altyapı hakkında endişelenmeden yalnızca geçerli sorun için gerekli kodu yazabilirsiniz. Işlevler geliştirmeyi daha verimli hale getirebilir ve C#, F#, Node.js, Python veya PHP gibi seçtiğiniz geliştirme dilini kullanabilirsiniz. Yalnızca kodunuzu çalıştırdığınız süre için ödeme yaparsınız ve Azure ölçeklendirmeleri gerektiği gibi.

Azure İşlevlerini başlatmak için [ilk Azure İşlevinizi Oluştur](../azure-functions/functions-create-first-azure-function.md)ile başlayın. Fonksiyonlar hakkında daha fazla teknik bilgi için [geliştirici başvurusuna](../azure-functions/functions-reference.md)bakın.

## <a name="how-can-i-build-and-deploy-serverless-apps-in-azure"></a>Azure'da sunucusuz uygulamaları nasıl oluşturabilir ve dağıtabilirim?

Azure, sunucusuz uygulamalar geliştirmek, dağıtmak ve yönetmek için zengin araçlar sağlar. Visual Studio veya [Visual Studio Code'daki araçlarla](logic-apps-serverless-get-started-vs.md) [Visual Studio Code](quickstart-create-logic-apps-visual-studio-code.md)doğrudan Azure portalında uygulama oluşturabilirsiniz. Uygulamanızı oluştursanız, [Azure Kaynak Yöneticisi şablonları ile bu uygulamayı hızlı bir şekilde dağıtabilirsiniz.](logic-apps-deploy-azure-resource-manager-templates.md) Azure ayrıca, Azure portalı üzerinden, API veya SDK'lar aracılığıyla veya Azure Monitor günlükleri ve Uygulama Öngörüleri için tümleşik araç la erişebileceğiniz izleme sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio'da sunucusuz bir uygulama oluşturun](logic-apps-serverless-get-started-vs.md)
* [Sunucusuz bir müşteri öngörüleri panosu oluşturma](logic-apps-scenario-social-serverless.md)
* [Mantıksal uygulama dağıtımı otomatikleştirme](logic-apps-azure-resource-manager-templates-overview.md)
