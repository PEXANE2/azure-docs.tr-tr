---
title: B2B kurumsal tümleştirmesi için XML'i doğrulayın
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta şemalar kullanarak XML'i doğrulayın
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 10/21/2019
ms.openlocfilehash: ff21b059e712489c1914b2d12c6aa6a3d78d66d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792171"
---
# <a name="validate-xml-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta B2B kurumsal tümleştirmesi için XML'i doğrulayın

Genellikle B2B senaryolarında, bir anlaşmadaki ticaret ortaklarının, herhangi bir veri işleme başlamadan önce değiş tokuş ettikleri iletilerin geçerli olduğundan emin olmaları gerekir. Kurumsal Tümleştirme Paketi'nde kullanılabilen XML doğrulama eylemini kullanarak belgeleri önceden tanımlanmış bir şema karşısında doğrulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz aboneliğiniz yoksa, ücretsiz [bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* XML doğrulama eylemini kullanmak istediğiniz boş veya varolan bir mantık uygulaması. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları ve](../logic-apps/logic-apps-overview.md) Quickstart nedir'yi inceleyin: İlk [mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Azure aboneliğinizle ilişkili bir [tümleştirme hesabı,](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) XML doğrulama eylemini kullanmayı planladığınız mantık uygulamasına bağlıdır ve XML içeriğini doğrulamak için kullanmak istediğiniz şemayı içerir. Hem mantık uygulamanız hem de tümleştirme hesabınız aynı konumda veya Azure bölgesinde bulunmalıdır.

## <a name="add-xml-validation-action"></a>XML doğrulama eylemi ekleme

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. Logic App Designer'da, arama kutusunda boş bir mantık uygulamanız varsa, filtreniz olarak girin `HTTP request` ve BIR HTTP isteği ne zaman tetikleyici **alınır'ı** seçin. Aksi halde, bir sonraki adıma devam edin.

1. İş akışınızdaki son adımaltında **Yeni adımı**seçin.

   Varolan adımlar arasında bir eylem eklemek için, artı işareti (**+**) görünmesi için işaretçinizi bu adımları bağlayan okun üzerine taşıyın. Bu artı işareti seçin ve sonra **bir eylem ekle'yi**seçin.

1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna filtreniz olarak girin. `xml validation` Eylemler listesinden **XML Doğrulama'yı**seçin.

   !["XML Doğrulama" eylemini bulma ve seçme](./media/logic-apps-enterprise-integration-xml-validation/select-xml-validation-action.png)

1. Doğrulamak istediğiniz XML içeriğini belirtmek için, dinamik içerik listesinin görünmesi için **İçerik** kutusunun içini tıklatın.

   ![Dinamik içerik listesini açma](./media/logic-apps-enterprise-integration-xml-validation/open-dynamic-content-list.png)

   Dinamik içerik listesi, iş akışındaki önceki adımlardaki çıktıları temsil eden özellik belirteçlerini gösterir. Listede beklenen bir özellik görünmüyorsa, daha **fazla bilgi al'ı**seçip seçemeyeceğiniz tetikleyiciveya eylem başlığını kontrol edin.

1. Dinamik içerik listesinden, doğrulamak istediğiniz içeriğe sahip özelliği seçin.

   Bu örnek, tetikleyiciden **Gövde** çıktısını seçer.

   ![Doğrulamak için içeriği seçin](./media/logic-apps-enterprise-integration-xml-validation/select-content-to-validate.png)

1. Doğrulama için kullanmak istediğiniz şemayı belirtmek **için, Şema Adı** listesini açın ve bağlı tümleştirme hesabınıza eklediğiniz doğrulama şemasını seçin.

   ![Doğrulama için kullanılacak şemayı seçin](./media/logic-apps-enterprise-integration-xml-validation/select-validation-schema.png)

1. Mantıksal uygulamanızı kaydedin.

   Artık onayını ayarlamayı bitirdin. Gerçek bir uygulamada, doğrulanmış verileri SalesForce gibi bir iş satırı (LOB) uygulamasında depolamak isteyebilirsiniz. Doğrulanmış çıktıyı Salesforce'a göndermek için bir eylem ekleyin.

1. Doğrulama eyleminizi sınamak için, mantık uygulamanızın iş akışını tetiklemek için bir istek gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Kurumsal Entegrasyon [Paketi](../logic-apps/logic-apps-enterprise-integration-overview.md) hakkında daha fazla bilgi edinin