---
title: Tümleştirme hesabı artefakt meta verilerini yönetme
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'taki tümleştirme hesaplarından yapı meta verileri ekleme veya alma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/17/2019
ms.openlocfilehash: bc119f1ce8efb821781dabfb9dd259cc5c8d9c23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792477"
---
# <a name="manage-artifact-metadata-in-integration-accounts-with-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Logic Apps ve Kurumsal Tümleştirme Paketi ile tümleştirme hesaplarındaki yapı meta verilerini yönetme

Tümleştirme hesaplarındaki yapılar için özel meta veriler tanımlayabilir ve mantık uygulamanızın kullanım süresi boyunca bu meta verileri alabilirsiniz. Örneğin, ortaklar, anlaşmalar, şemalar ve haritalar gibi yapılar için meta veriler sağlayabilirsiniz - tüm bunlar anahtar değer çiftleri kullanarak meta verileri saklar. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Meta veri eklemek istediğiniz yapıları içeren temel bir [tümleştirme hesabı,](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) örneğin: 

  * [İş Ortağı](logic-apps-enterprise-integration-partners.md)
  * [Anlaşma](logic-apps-enterprise-integration-agreements.md)
  * [Şema](logic-apps-enterprise-integration-schemas.md)
  * [Harita](logic-apps-enterprise-integration-maps.md)

* Kullanmak istediğiniz tümleştirme hesabı ve yapı meta verilerine bağlı bir mantık uygulaması. Mantık uygulamanız zaten bağlı değilse, [mantık uygulamalarını entegrasyon hesaplarına nasıl bağladığınızı](logic-apps-enterprise-integration-create-integration-account.md#link-account)öğrenin. 

  Henüz bir mantık uygulamanız yoksa, [mantık uygulamaları oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin. 
  Yapı meta verilerini yönetmek için kullanmak istediğiniz tetikleyiciyi ve eylemleri ekleyin. Veya, sadece bir şeyleri denemek için, mantık uygulamanıza **İstek** veya **HTTP** gibi bir tetikleyici ekleyin.

## <a name="add-metadata-to-artifacts"></a>Yapılara meta veri ekleme

1. Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın. Entegrasyon hesabınızı bulun ve açın.

1. Meta veri eklemek istediğiniz yapıyı seçin ve **Edit'i**seçin. Bu yapının meta veri ayrıntılarını girin, örneğin:

   ![Meta veri girin](media/logic-apps-enterprise-integration-metadata/add-partner-metadata.png)

1. İşiniz bittiğinde **Tamam**’ı seçin.

1. Tümleştirme hesabı için JavaScript Nesne Gösterimi (JSON) tanımındaki bu meta verileri görüntülemek için, JSON düzenleyicisinin açabilmesi için **JSON olarak Edit'i** seçin: 

   ![Ortak meta veriler için JSON](media/logic-apps-enterprise-integration-metadata/partner-metadata.png)

## <a name="get-artifact-metadata"></a>Yapı meta verilerini alma

1. Azure portalında, istediğiniz tümleştirme hesabına bağlı mantık uygulamasını açın. 

1. Mantık Uygulama Tasarımcısı'nda, meta verileri tetikleyicinin altına almak için adımı veya iş akışındaki son eylemi ekliyorsanız, Yeni adım**bir eylem ekleyin'i** **New step** > seçin. 

1. Arama kutusuna "tümleştirme hesabı"nı girin. Arama kutusunun **altında, Tümü'ni**seçin. Eylemler listesinden şu eylemi seçin: **Tümleştirme Hesabı Artifakı Araması - Tümleştirme Hesabı**

   !["Entegrasyon Hesabı Artifakı Arama" seçeneğini belirleyin](media/logic-apps-enterprise-integration-metadata/integration-account-artifact-lookup.png)

1. Bulmak istediğiniz yapı için bu bilgileri sağlayın:

   | Özellik | Gerekli | Değer | Açıklama | 
   |----------|---------|-------|-------------| 
   | **Artefakt Türü** | Evet | **Şema**, **Harita**, **İş Ortağı**, **Sözleşme**, veya özel bir tür | İstediğinin türemi | 
   | **Artefakt Adı** | Evet | <*artefakt adı*> | İstediğinin yapıtın adı | 
   ||| 

   Örneğin, bir ticaret ortağı artifakı için meta verileri almak istediğinizi varsayalım:

   ![Yapı türünü seçin ve yapı adını girin](media/logic-apps-enterprise-integration-metadata/artifact-lookup-information.png)

1. Bu meta verileri işlemek için istediğiniz eylemi ekleyin( örneğin:

   1. **Tümleştirme Hesabı Artefakt Arama** eyleminin altında, **Sonraki adım'ı**seçin ve **eylem ekle'yi**seçin. 

   1. Arama kutusuna "http" girin. Arama kutusunun altında, **Yerleşikler'i**seçin ve şu eylemi seçin: **HTTP - HTTP**

      ![HTTP eylemi ekleme](media/logic-apps-enterprise-integration-metadata/http-action.png)

   1. Yönetmek istediğiniz yapı meta verileri için bilgi sağlayın. 

      Örneğin, bu konuya daha `routingUrl` önce eklenen meta verileri almak istediğinizi varsayalım. Belirtebileceğiniz özellik değerleri şunlardır: 

      | Özellik | Gerekli | Değer | Açıklama | 
      |----------|----------|-------|-------------| 
      | **Yöntem** | Evet | <*çalışmadan çalıştır*> | Eserin üzerinde çalıştırmak için HTTP işlemi. Örneğin, bu HTTP eylemi **GET** yöntemini kullanır. | 
      | **URI** | Evet | <*meta veri konumu*> | Aldığınız yapıdaki `routingUrl` meta veri değerine erişmek için, örneğin bir ifade kullanabilirsiniz: <p>`@{outputs('Integration_Account_Artifact_Lookup')['properties']['metadata']['routingUrl']}` | 
      | **Üst Bilgiler** | Hayır | <*üstbilgi değerleri*> | Http eylemine geçmek istediğiniz tetikleyiciden herhangi bir üstbilgi çıkışı. Örneğin, tetikleyicinin `headers` özellik değerini geçmek için: örneğin bir ifade kullanabilirsiniz: <p>`@triggeroutputs()['headers']` | 
      | **Gövde** | Hayır | <*gövde içeriği*> | HTTP eyleminin `body` özelliğinden geçirmek istediğiniz diğer içerikler. Bu örnek, yapının `properties` değerlerini HTTP eylemine geçirir: <p>1. Dinamik içerik listesinin görünmesi için **Gövde** özelliğinin içine tıklayın. Özellik görünmüyorsa, **daha fazlasını gör'üni**seçin. <br>2. Dinamik içerik listesinden, **Entegrasyon Hesabı Artifakı Araması**altında, **Özellikleri**seçin. | 
      |||| 

      Örnek:

      ![HTTP eylemi için değerleri ve ifadeleri belirtin](media/logic-apps-enterprise-integration-metadata/add-http-action-values.png)

   1. HTTP eylemi için sağladığınız bilgileri kontrol etmek için mantık uygulamanızın JSON tanımını görüntüleyin. Logic App Designer araç çubuğunda, uygulamanın JSON tanımının görünmesi için **Kod görünümünü** seçin:

      ![Mantık uygulaması JSON tanımı](media/logic-apps-enterprise-integration-metadata/finished-logic-app-definition.png)

      Mantık Uygulama Tasarımcısı'na geri döndükten sonra, kullandığınız tüm ifadeler artık çözümlenmiş gibi görünür, örneğin:

      ![Mantık Uygulama Tasarımcısı'nda çözümlenmiş ifadeler](media/logic-apps-enterprise-integration-metadata/resolved-expressions.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Anlaşmalar hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-agreements.md)
