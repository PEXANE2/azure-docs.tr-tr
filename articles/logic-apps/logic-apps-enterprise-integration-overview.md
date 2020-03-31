---
title: B2B kurumsal tümleştirme
description: Azure Logic Apps ve Kurumsal Tümleştirme Paketi'ni kullanarak kurumsal tümleştirme için otomatik B2B iş akışları oluşturma hakkında bilgi edinin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, logicappspm
ms.topic: overview
ms.date: 08/01/2019
ms.openlocfilehash: 9356cecb22672ae15505d14b5ac73d5baeef09e0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77191365"
---
# <a name="b2b-enterprise-integration-solutions-with-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Logic Apps ve Kurumsal Tümleştirme Paketi ile B2B kurumsal tümleştirme çözümleri

İşletmeler arası (B2B) çözümleri ve kuruluşlar arasında sorunsuz iletişim için, [Azure Mantık Uygulamaları](../logic-apps/logic-apps-overview.md)ile Kurumsal Tümleştirme Paketi'ni (EIP) kullanarak otomatik ölçeklenebilir kurumsal tümleştirme iş akışları oluşturabilirsiniz. Kuruluşlar farklı protokoller ve biçimler kullansa lar da, iletileri elektronik olarak değiştirebilirler. EIP, farklı biçimleri kuruluşların sistemlerinizin işleyebilir ve [AS2,](../logic-apps/logic-apps-enterprise-integration-as2.md) [X12](logic-apps-enterprise-integration-x12.md)ve [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md)dahil olmak üzere endüstri standardı protokolleri destekleyebilir bir biçime dönüştürür. Ayrıca, hem şifreleme hem de dijital imzalar kullanarak iletilerin güvenliğini artırabilirsiniz. EIP, bu [kurumsal entegrasyon konektörlerini](../connectors/apis-list.md#integration-account-connectors) ve bu endüstri standartlarını destekler:

* Elektronik Veri Değişimi (EDI)
* Kurumsal Uygulama Entegrasyonu (EAI)

Microsoft BizTalk Server veya Azure BizTalk Hizmetlerini biliyorsanız, EIP benzer kavramları izler ve bu da özelliklerin kullanımını kolaylaştırır. Ancak, önemli bir fark, EIP mimari b2B iletişim kullanılan eserlerin depolama ve yönetimini kolaylaştırmak için "entegrasyon hesapları" dayanmaktadır. Bu hesaplar, ortaklar, anlaşmalar, şemalar, haritalar ve sertifikalar gibi tüm yapılarınızı depolayan bulut tabanlı kapsayıcılardır. 

## <a name="why-use-the-enterprise-integration-pack"></a>Neden Kurumsal Entegrasyon Paketi'ni kullanıyorsun?

* EIP ile tüm yapılarınızı tek bir yerde saklayabilirsiniz - entegrasyon hesabınız.

* Azure Logic Apps ve bağlayıcıları kullanarak B2B iş akışları oluşturabilir ve hizmet olarak üçüncü taraf yazılım (SaaS) uygulamaları, şirket içi uygulamalar ve özel uygulamalarla entegre olabilirsiniz.

* Azure işlevleriyle mantık uygulamalarınız için özel kod oluşturabilirsiniz.

## <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?

EIP ile B2B mantık uygulaması iş akışları oluşturmaya başlamadan önce aşağıdaki öğelere ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Kullanmak istediğiniz yapıları içeren bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

* Haritalar ve şemalar oluşturmak için [Visual Studio 2015 2.0 ve Visual Studio 2015 için Microsoft Azure Logic Apps Kurumsal Tümleştirme Araçlarını](https://aka.ms/vsmapsandschemas) kullanabilirsiniz.

Bir tümleştirme hesabı oluşturduktan ve yapılarınızı ekledikten sonra, Azure portalında bir mantık uygulaması oluşturarak bu yapılarla B2B iş akışları oluşturmaya başlayabilirsiniz. Mantık uygulamalarında yeniyseniz, [temel bir mantık uygulaması oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)deneyin. Bu yapılarla çalışmak için, öncelikle entegrasyon hesabınızı mantık uygulamanıza bağladığınızdan emin olun. Bundan sonra, mantık uygulamanız entegrasyon hesabınıza erişebilir. Ayrıca Visual Studio veya [PowerShell'i](https://docs.microsoft.com/powershell/module/az.logicapp)kullanarak mantık uygulamaları oluşturabilir, yönetebilir ve dağıtabilirsiniz.

B2B mantık uygulamaları oluşturmaya başlamak için üst düzey adımlar şunlardır:

![B2B mantık uygulamaları oluşturmak için ön koşullar](./media/logic-apps-enterprise-integration-overview/overview.png)  

## <a name="try-now"></a>Hemen deneyin

[AS2 iletileri gönderen ve alan tam operasyonel örnek mantık uygulaması dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive)

## <a name="next-steps"></a>Sonraki adımlar

* [Ticaret ortakları oluşturun](logic-apps-enterprise-integration-partners.md)
* [Anlaşmalar oluşturma](../logic-apps/logic-apps-enterprise-integration-agreements.md)
* [Şema ekleme](logic-apps-enterprise-integration-schemas.md)
* [Haritalar ekleme](../logic-apps/logic-apps-enterprise-integration-maps.md)
* [BizTalk Services’dan geçirme](../logic-apps/logic-apps-move-from-mabs.md)
