---
title: Dynamics 365'e bağlanın
description: Dynamics 365 (çevrimiçi) REST API'leri ve Azure Mantık Uygulamaları ile kayıt oluşturma ve yönetme
services: logic-apps
ms.suite: integration
author: Mattp123
ms.author: matp
ms.reviewer: estfan, logicappspm
ms.topic: conceptual
ms.date: 08/18/2018
tags: connectors
ms.openlocfilehash: 9837b68fbfba783a468712d8ba1883b198af4954
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789878"
---
# <a name="manage-dynamics-365-records-with-azure-logic-apps"></a>Azure Logic Apps ile Dynamics 365 kayıtlarını yönetme

Azure Logic Apps ve Dynamics 365 bağlayıcısı sayesinde, Dynamics 365’teki kayıtlarınızı temel alarak otomatik görevler ve iş akışları oluşturabilirsiniz. İş akışlarınız, Dynamics 365 hesabınızda kayıtlar oluşturabilir, öğeleri güncelleştirebilir, kayıtlar döndürebilir ve daha fazlasını yapabilir. Dynamics 365'ten yanıt alan ve çıktıyı diğer eylemler için kullanılabilir hale getiren eylemleri mantık uygulamalarınıza ekleyebilirsiniz. Örneğin, Dynamics 365'te bir öğe güncelleştirildiğinde, Office 365'i kullanarak e-posta gönderebilirsiniz.

Bu makalede, Dynamics 365'te yeni bir müşteri adayı kaydı oluşturulduğunda Dynamics 365'te görev oluşturan bir mantık uygulaması nasıl oluşturulabileceğiniz gösterilmektedir.
Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Dinamik [365 hesabı](https://dynamics.microsoft.com)

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Dynamics 365 hesabınıza erişmek istediğiniz mantık uygulaması. Dynamics 365 tetikleyicisi ile mantık uygulamanızı başlatmak için boş bir [mantık uygulamasına](../logic-apps/quickstart-create-first-logic-app-workflow.md)ihtiyacınız var.

## <a name="add-dynamics-365-trigger"></a>Dynamics 365 tetikleyiciekle

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

İlk olarak, Dynamics 365'te yeni bir müşteri adayı kaydı göründüğünde devreye soal veren bir Dynamics 365 tetikleyicisi ekleyin.

1. Azure [portalında,](https://portal.azure.com)boş mantık uygulamanızı zaten açık değilse, Mantık Uygulama Tasarımcısı'nda açın.

1. Arama kutusuna filtreniz olarak "Dynamics 365" girin. Bu örnekte, tetikleyiciler listesinin altında şu tetikleyiciyi seçin: **Bir kayıt oluşturulduğunda**

   ![Tetikleyiciyi seçin](./media/connectors-create-api-crmonline/select-dynamics-365-trigger.png)

1. Dynamics 365'te oturum açmanız istenirse, hemen oturum açın.

1. Bu tetikleyici ayrıntıları sağlayın:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Kuruluş Adı** | Evet | Kuruluşunuzun Dinamikler 365 örneğinin adı, örneğin, "Contoso" |
   | **Varlık Adı** | Evet | İzlenecek varlığın adı, örneğin, "Müşteri Adayları" | 
   | **Frequency** | Evet | Tetikleyiciyle ilgili güncelleştirmeleri denetlerken aralıklarla kullanılacak zaman birimi |
   | **Interval** | Evet | Bir sonraki kontrolden önce geçen saniye, dakika, saat, gün, hafta veya ay sayısı |
   ||| 

   ![Tetikleyici ayrıntıları](./media/connectors-create-api-crmonline/trigger-details.png)

## <a name="add-dynamics-365-action"></a>Dynamics 365 eylem ekle

Şimdi, yeni müşteri adayı kaydı için bir görev kaydı oluşturan Dynamics 365 eylemini ekleyin.

1. Tetikleyicinizin altında **Yeni adımı**seçin.

1. Arama kutusuna filtreniz olarak "Dynamics 365" girin. Eylemler listesinden şu eylemi seçin: **Yeni bir kayıt oluşturma**

   ![Eylem seçin](./media/connectors-create-api-crmonline/select-action.png)

1. Bu eylem ayrıntılarını sağlayın:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Kuruluş Adı** | Evet | Tetikleyicinizde aynı örnek olmak zorunda olmayan, ancak bu örnekte "Contoso" olan kaydı oluşturmak istediğiniz Dynamics 365 örneği |
   | **Varlık Adı** | Evet | Kaydı oluşturmak istediğiniz varlık, örneğin, "Görevler" |
   | | |

   ![Eylem ayrıntıları](./media/connectors-create-api-crmonline/action-details.png)

1. **Eyleminizde Konu** kutusu göründüğünde, dinamik içerik listesinin görünmesi için **Konu** kutusunun içine tıklayın. Bu listeden, yeni müşteri adayı kaydıyla ilişkili görev kaydına dahil edilecek alan değerlerini seçin:

   | Alan | Açıklama |
   |-------|-------------|
   | **Soyadı** | Kayıttaki birincil kişi olarak müşteri adayının soyadı |
   | **Konu başlığı** | Kayıttaki müşteri adayının açıklayıcı adı |
   | | |

   ![Görev kayıt ayrıntıları](./media/connectors-create-api-crmonline/create-record-details.png)

1. Tasarımcı araç çubuğunda, mantık uygulamanız için **Kaydet'i** seçin. 

1. Mantık uygulamasını el ile başlatmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin.

   ![Mantıksal uygulamanızı çalıştırma](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

1. Şimdi Dynamics 365'te bir müşteri adayı kaydı oluşturun, böylece mantık uygulamanızın iş akışını tetikleyebilirsiniz.

## <a name="add-filter-or-query"></a>Filtre veya sorgu ekleme

Dynamics 365 eyleminde verileri nasıl filtreleeceğinizi belirtmek için, bu eylemde **gelişmiş seçenekleri göster'i** seçin. Daha sonra sorguya göre bir filtre veya sipariş ekleyebilirsiniz.
Örneğin, yalnızca etkin hesapları almak ve bu kayıtları hesap adına göre sıralamak için bir filtre sorgusu kullanabilirsiniz. Bu görev için aşağıdaki adımları izleyin:

1. **Filtre sorgusunun**altında, bu OData filtresi sorgusuna girin:`statuscode eq 1`

2. **Düzen Le,** dinamik içerik listesi göründüğünde **Hesap Adı'nı**seçin. 

   ![Filtre ve sipariş belirtin](./media/connectors-create-api-crmonline/advanced-options.png)

Daha fazla bilgi için şu Dynamics 365 Müşteri Etkileşimi Web API sistem sorgu seçeneklerine bakın:

* [$filter](https://docs.microsoft.com/dynamics365/customer-engagement/developer/webapi/query-data-web-api#filter-results)
* [$orderby](https://docs.microsoft.com/dynamics365/customer-engagement/developer/webapi/query-data-web-api#order-results)

### <a name="best-practices-for-advanced-options"></a>Gelişmiş seçenekleri için en iyi yöntemler

Bir eylem veya tetikleyicide bir alan için değer belirttiğiniz zaman, değeri el ile girdiğinizde veya dinamik içerik listesinden değeri seçseniz de değerin veri türüyle eşleşmesi gerekir.

Bu tablo, bazı alan türlerini ve değerleri için gerekli veri türlerini açıklar.

| Alan türü | Gerekli veri türü | Açıklama | 
|------------|--------------------|-------------|
| Metin alanları | Tek satırlı metin | Bu alanlar, metin türüne sahip tek bir metin satırı veya dinamik içerik gerektirir. <p><p>*Örnek alanlar*: **Açıklama** ve **Kategori** | 
| Tamsayı alanları | Tam sayı | Bazı alanlar, isterteme türüne sahip, istertime veya dinamik içeriğe ihtiyaç duyar. <p><p>*Örnek alanlar*: **Yüzde Tam** ve **Süre** | 
| Tarih alanları | Tarih ve Saat | Bazı alanlar, tarih türüne sahip mm/dd/yyyy biçimi veya dinamik içeriğe sahip bir tarih gerektirir. <p><p>*Örnek alanlar*: **Oluşturulan**, **Başlangıç Tarihi**, Fiili **Başlangıç**, **Fiili Bitiş**ve **Vade Tarihi** | 
| Hem kayıt kimliği hem de arama türü gerektiren alanlar | Birincil anahtar | Başka bir varlık kaydına başvuran bazı alanlar hem kayıt kimliği hem de arama türü gerektirir. | 
||||

Bu alan türlerini genişleterek, Dynamics 365 tetikleyicilerinde örnek alanlar ve hem kayıt kimliği hem de arama türü gerektiren eylemler verilmiştir. Bu gereksinim, dinamik listeden seçtiğiniz değerlerin çalışmayamayacağı anlamına gelir.

| Alan | Açıklama |
|-------|-------------|
| **Sahibi** | Geçerli bir kullanıcı kimliği veya takım kayıt kimliği olmalıdır. |
| **Sahip Tipi** | Ya da `systemusers` `teams`. |
| **İlgili** | Hesap kimliği veya ilgili kişi kayıt kimliği gibi geçerli bir kayıt kimliği olmalıdır. |
| **Türüne İlişkin** | Bir arama türü olmalı, `accounts` gibi `contacts`veya . |
| **Müşteri** | Hesap kimliği veya ilgili kişi kayıt kimliği gibi geçerli bir kayıt kimliği olmalıdır. |
| **Müşteri Tipi** | Gibi `accounts` veya `contacts`. |
|||

Bu örnekte, **yeni bir kayıt oluştur** adlı eylem yeni bir görev kaydı oluşturur:

![Kayıt titreleri ve arama türleri ile görev kaydı oluşturma](./media/connectors-create-api-crmonline/create-record-advanced.png)

Bu eylem, **Sahip** alanındaki kayıt kimliğine ve **Sahip Türü** alanındaki arama türüne göre görev kaydını belirli bir kullanıcı kimliğine veya takım kayıt kimliğine atar:

![Sahibi kayıt kimliği ve arama türü](./media/connectors-create-api-crmonline/owner-record-id-and-lookup-type.png)

Bu eylem, **Ilgili** alana eklenen kayıt kimliği yle ilişkili bir hesap kaydı ve **Benzer Tür** alanına arama türü de ekler:

![Kayıt kimliği ve arama türü ile ilgili olarak](./media/connectors-create-api-crmonline/regarding-record-id-lookup-type-account.png)

## <a name="find-record-id"></a>Kayıt kimliğini bulma

Kayıt kimliğini bulmak için aşağıdaki adımları izleyin:

1. Dynamics 365'te, hesap kaydı gibi bir kayıt açın.

2. Eylemler araç çubuğunda aşağıdaki adımlardan birini seçin:

   * **Pop Out'u**seçin. ![popout kaydı](./media/connectors-create-api-crmonline/popout-record.png) 
   * Varsayılan e-posta programınızda tam URL kopyalayabilmeniz için **E-POSTA BAĞLANTISI'nı** seçin.

   Kayıt kimliği URL'de ve `%7b` `%7d` kodlama karakterleri arasında görünür:

   ![Kayıt kimliğini bulma](./media/connectors-create-api-crmonline/find-record-ID.png)

## <a name="troubleshoot-failed-runs"></a>Başarısız çalıştırma sorunlarını giderme

Mantık uygulamanızdaki başarısız adımları bulmak ve incelemek için, mantık uygulamanızın çalışan geçmişini, durumunu, girdilerini, çıktılarını ve benzeri geçmişlerini görüntüleyebilirsiniz.

1. Azure portalında, mantık uygulamanızın ana menüsünde **Genel Bakış'ı**seçin. Mantık uygulamanızın tüm çalıştırma durumlarını gösteren **Geçmiş Çalıştır** bölümünde, daha fazla bilgi için başarısız bir çalıştırma seçin.

   ![Mantık uygulaması çalıştırma durumu](./media/connectors-create-api-crmonline/run-history.png)

1. Daha fazla ayrıntı görüntüleyebilmeniz için başarısız bir adımı genişletin.

   ![Başarısız adımı genişletme](./media/connectors-create-api-crmonline/expand-failed-step.png)

1. Hatanın arkasındaki nedeni bulmanıza yardımcı olabilecek giriş ler ve çıktılar gibi adımın ayrıntılarını gözden geçirin.

   ![Başarısız adım - giriş ler ve çıktılar](./media/connectors-create-api-crmonline/expand-failed-step-inputs-outputs.png)

Sorun giderme mantığı uygulamaları hakkında daha fazla bilgi [için](../logic-apps/logic-apps-diagnosing-failures.md)bkz.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) dosyasında açıklandığı gibi tetikleyiciler, eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/dynamicscrmonline/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
