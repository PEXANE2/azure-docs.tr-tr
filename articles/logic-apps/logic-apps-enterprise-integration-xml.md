---
title: XML iletileri ve düz dosyalar
description: Kurumsal Tümleştirme Paketi ile Azure Mantıksal Uygulamalarında XML mesajlarını işleme, doğrulama ve dönüştürme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/27/2017
ms.openlocfilehash: 3cb3472572abbd891f8d36ea036b371c1224f38c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792140"
---
# <a name="xml-messages-and-flat-files-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta XML mesajları ve düz dosyalar

[Azure Logic Apps'ta,](logic-apps-overview.md)Kurumsal Tümleştirme Paketi'ni kullanarak gönderdiğiniz ve aldığınız XML iletilerini işleyebilirsiniz. BizTalk Server'ı kullandıysanız, Kurumsal Tümleştirme Paketi iletileri dönüştürmek ve doğrulamak, düz dosyalarla çalışmak ve hatta bir iletideki belirli özellikleri zenginleştirmek veya ayıklamak için XPath'i kullanmak için benzer özellikler sağlar. Bu alanda yeniyseniz, bu özellikler mantık uygulamanızın iş akışındaki iletileri işleme şeklinizi genişletir. Örneğin, bir işletmeden işletmeye (B2B) senaryonuz varsa ve belirli XML şemalarıyla çalışıyorsanız, şirketinizin bu iletileri nasıl işlediğini geliştirmek için Kurumsal Tümleştirme Paketini kullanabilirsiniz.

Örneğin, Kurumsal Tümleştirme Paketi şu özellikleri içerir:

* [XML doğrulama](logic-apps-enterprise-integration-xml-validation.md): Gelen veya giden bir XML iletisi belirli bir şemaya karşı doğrulayın.

* [XML dönüşümü](logic-apps-enterprise-integration-transform.md): Haritaları kullanarak gereksinimlerinize veya bir ortağın gereksinimlerine göre bir XML iletisini dönüştürün veya özelleştirin.

* [Düz dosya kodlama ve düz dosya çözme](logic-apps-enterprise-integration-flatfile.md): Düz bir dosyayı kodlama veya çözme.

  Örneğin, SAP IDOC dosyalarını düz dosya biçiminde kabul eder ve gönderir. Birçok tümleştirme platformu, Logic Apps da dahil olmak üzere XML iletileri oluşturur. Böylece, XML dosyalarını düz dosyalara "dönüştürmek" için düz dosya kodlayıcısını kullanan bir mantık uygulaması oluşturabilirsiniz.

* [XPath](workflow-definition-language-functions-reference.md#xpath): İletiyi zenginleştirin ve iletiden belirli özellikleri ayıklayın. Daha sonra, iletiyi bir hedefe veya ara bitiş noktasına yönlendirmek için ayıklanan özellikleri kullanabilirsiniz.

## <a name="sample"></a>Örnek

Azure [Logic Apps'taki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline) XML özelliklerini kullanarak tam işlevsel bir mantık uygulaması (GitHub örneği) dağıtın.

## <a name="next-steps"></a>Sonraki adımlar

Kurumsal Entegrasyon [Paketi](logic-apps-enterprise-integration-overview.md) hakkında daha fazla bilgi edinin
