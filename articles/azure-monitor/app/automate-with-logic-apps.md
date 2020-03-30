---
title: Logic Apps'ı kullanarak Azure Uygulama Öngörüleri işlemlerini otomatikleştirin
description: Mantık uygulamanıza Application Insights bağlayıcısını ekleyerek tekrarlanabilir işlemleri nasıl hızlı bir şekilde otomatikleştirebileceğinizi öğrenin.
ms.topic: conceptual
ms.date: 03/11/2019
ms.openlocfilehash: 4a0944e661932d86fac75f78c4faf5be751806c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473175"
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Logic Apps'ı kullanarak Uygulama Öngörüleri süreçlerini otomatikleştirin

Hizmetinizin düzgün çalışıp çalışmadığını kontrol etmek için telemetri verilerinizde aynı sorguları sürekli olarak çalıştırdığınızı mı buluyorsunuz? Eğilimleri ve anormallikleri bulmak için bu sorguları otomatikleştirmek ve sonra bunların etrafında kendi iş akışları oluşturmak için mi arıyorsunuz? Logic Apps için Azure Application Insights bağlayıcısı bu amaç için doğru araçtır.

> [!NOTE]
> Azure Uygulama Öngörüleri bağlayıcısı, API anahtarı na ihtiyaç etmek yerine Azure Active Directory ile entegre edilmiş ve ayrıca Bir Log Analytics çalışma alanından veri almanıza olanak tanıyan [Azure Monitor bağlayıcısıyla](../platform/logicapp-flow-connector.md) değiştirildi.

Bu tümleştirme yle, tek bir kod satırı yazmadan çok sayıda işlemi otomatikleştirebilirsiniz. Herhangi bir Uygulama Öngörüleri işlemini hızlı bir şekilde otomatikleştirmek için Application Insights konektörünü içeren bir mantık uygulaması oluşturabilirsiniz. 

Ek eylemler de ekleyebilirsiniz. Azure Uygulama Hizmeti'nin Mantıksal Uygulamalar özelliği yüzlerce eylemi kullanılabilir hale getirir. Örneğin, bir mantık uygulaması kullanarak otomatik olarak bir e-posta bildirimi gönderebilir veya Azure DevOps'lerde bir hata oluşturabilirsiniz. Ayrıca, mantık uygulamanızı oluşturma işlemini hızlandırmaya yardımcı olmak için kullanılabilir birçok [şablondan](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) birini de kullanabilirsiniz. 

## <a name="create-a-logic-app-for-application-insights"></a>Uygulama Öngörüleri için bir mantık uygulaması oluşturma

Bu eğitimde, bir web uygulamasının verilerindeki öznitelikleri gruplandırmak için Analytics otomatik küme algoritmasını kullanan bir mantık uygulaması oluşturmayı öğrenirsiniz. Akış, sonuçları uygulama öngörüleri analizi ni ve Mantık Uygulamalarını birlikte nasıl kullanabileceğinize dair yalnızca bir örnek olan e-posta yoluyla otomatik olarak gönderir. 

### <a name="step-1-create-a-logic-app"></a>Adım 1: Bir mantık uygulaması oluşturma
1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Kaynak Oluştur'u**tıklatın, **Web + Mobile'ı**seçin ve ardından Mantık **Uygulaması'nı**seçin.

    ![Yeni mantık uygulaması penceresi](./media/automate-with-logic-apps/1createlogicapp.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Adım 2: Mantık uygulamanız için bir tetikleyici oluşturma
1. Mantık **App Designer** penceresinde, **ortak bir tetikleyici ile Başlat**altında, **Yineleme**seçin.

    ![Mantık Uygulaması Tasarımcısı penceresi](./media/automate-with-logic-apps/2logicappdesigner.png)

1. **Aralık** kutusunda, **1** yazın ve ardından,**Sıklık** kutusu, **Gün'u**seçin.

    ![Mantık Uygulama Tasarımcısı "Nüks" penceresi](./media/automate-with-logic-apps/3recurrence.png)

### <a name="step-3-add-an-application-insights-action"></a>Adım 3: Uygulama Öngörüleri eylemi ekleme
1. **Yeni adımı**tıklatın.

1. Eylem arama kutusu **seçin'de** **Azure Uygulama Öngörüleri**yazın.

1. **Eylemler**altında, **Azure Uygulama Öngörüleri - Analytics sorgusunun Visualize'ı**tıklatın.

    ![Mantık Uygulama Tasarımcısı "Eylem seçin" penceresi](./media/automate-with-logic-apps/4visualize.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a>Adım 4: Uygulama Öngörüleri kaynağına bağlanma

Bu adımı tamamlamak için kaynağınız için bir uygulama kimliği ve API anahtarı gerekir. Aşağıdaki diyagramda gösterildiği gibi bunları Azure portalından alabilirsiniz:

![Azure portalında uygulama kimliği](./media/automate-with-logic-apps/5apiaccess.png)

![Azure portalında uygulama kimliği](./media/automate-with-logic-apps/6apikey.png)

Bağlantınız, uygulama kimliğiniz ve API anahtarı için bir ad sağlayın.

![Mantık App Designer akış bağlantı penceresi](./media/automate-with-logic-apps/7connection.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Adım 5: Analytics sorgusunu ve grafik türünü belirtin
Aşağıdaki örnekte, sorgu son gün içinde başarısız istekleri seçer ve bunları işlemin bir parçası olarak oluşan özel durumlar ile ilişkilendirer. Analytics, operation_Id tanımlayıcısını temel alan başarısız istekleri ilişkilendirer. Sorgu daha sonra otomatik küme algoritmasını kullanarak sonuçları bölümlere böler. 

Kendi sorgularınızı oluşturduğunuzda, akışınıza eklemeden önce analytics'te düzgün çalışıp çalışmadıklarını doğrulayın.

1. **Sorgu** kutusuna aşağıdaki Analytics sorgusunu ekleyin:

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

1. Grafik **Türü** kutusunda **Html Tablosu'nu**seçin.

    ![Analitik sorgu yapılandırma penceresi](./media/automate-with-logic-apps/8query.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a>Adım 6: E-posta göndermek için mantık uygulamasını yapılandırın

1. **Yeni adımı**tıklatın.

1. Arama kutusuna **Office 365 Outlook**yazın.

1. **Office 365 Outlook 'u tıklatın - E-posta gönderin.**

    ![Office 365 Outlook seçimi](./media/automate-with-logic-apps/9sendemail.png)

1. **E-posta gönder** penceresinde aşağıdakileri yapın:

   a. Alıcının e-posta adresini yazın.

   b. E-posta için bir konu yazın.

   c. **Gövde** kutusunda herhangi bir yere tıklayın ve ardından sağdaki dinamik içerik menüsünde **Gövde'yi**seçin.
    
   d. Yeni **parametre bırak'ı ekle'yi** tıklatın ve Ekler ve HTML'yi seçin.

      ![Office 365 Outlook yapılandırması](./media/automate-with-logic-apps/10emailbody.png)

      ![Office 365 Outlook yapılandırması](./media/automate-with-logic-apps/11emailparameter.png)

1. Dinamik içerik menüsünde aşağıdakileri yapın:

    a. **Ek Adı'nı**seçin.

    b. **Ek İçeriği'ni**seçin.
    
    c. Is **HTML** kutusunda **Evet'i**seçin.

      ![Office 365 e-posta yapılandırma ekranı](./media/automate-with-logic-apps/12emailattachment.png)

### <a name="step-7-save-and-test-your-logic-app"></a>Adım 7: Mantık uygulamanızı kaydedin ve test edin
* Değişikliklerinizi kaydetmek için **Kaydet'i** tıklatın.

Tetikleyicinin mantık uygulamasını çalıştırmasını bekleyebilir veya **Çalıştır'ı**seçerek mantık uygulamasını hemen çalıştırabilirsiniz.

![Mantık uygulaması oluşturma ekranı](./media/automate-with-logic-apps/13save.png)

Mantık uygulamanız çalıştığında, e-posta listesinde belirttiğiniz alıcılar aşağıdaki gibi görünen bir e-posta alır:

![Mantık uygulaması e-posta iletisi](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Analytics sorguları](../../azure-monitor/log-query/get-started-queries.md)oluşturma hakkında daha fazla bilgi edinin.
- [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps) hakkında daha fazla bilgi edinin.



<!--Link references-->





