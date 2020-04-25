---
title: HTTPS ile Microsoft ticari Market lider yönetimi
description: Bir HTTPS uç noktası için Microsoft ticari Market lider yönetimini yapılandırın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 1c3337e970fdbb22cb1ed88f105d5e7798a68f74
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133729"
---
# <a name="configure-lead-management-by-using-an-https-endpoint"></a>Bir HTTPS uç noktası kullanarak müşteri adayı yönetimini yapılandırma

>[!NOTE]
>Bu yönergelerde kullanılan güç otomatikleştirme Bağlayıcısı, Power otomatikleştirmek için ücretli bir abonelik gerektirir. Bu makaledeki yönergeleri izlemeden önce bunun için hesap yaptığınızdan emin olun.

Müşteri ilişkileri yönetimi (CRM) sisteminiz Iş Ortağı Merkezi 'nde Microsoft AppSource ve Azure Market müşteri adaylarını almak üzere açıkça desteklenmiyorsa, bu müşteri adaylarını işlemek için Power otomatikleştirmede bir HTTPS uç noktası kullanabilirsiniz. Bir HTTPS uç noktası ile, bu müşteri adayları bir e-posta bildirimi olarak gönderilebilir veya Power otomatikleştirmede desteklenen bir CRM sistemine yazılabilir. Bu makaledeki yönergeler, **sağlama yönetimi** > **https uç noktası URL 'si** alanı için yayımlama portalına gireceğiniz http post URL 'sini oluşturan Power otomatikleştirmesini kullanarak yeni bir akış oluşturmak için size yol gösterir. Ayrıca, çevrimiçi olarak kullanılabilen [Postman](https://www.getpostman.com/downloads/)adlı bir aracın yardımıyla akışınızı test etme yönergeleriyle ilgili yönergeler de mevcuttur.

## <a name="create-a-flow-by-using-power-automate"></a>Power otomatikleştir kullanarak akış oluşturma

1. [Power otomatikleştirir](https://flow.microsoft.com/) Web sayfasını açın. **Oturum aç**'ı seçin. Henüz bir hesabınız yoksa ücretsiz **kaydolun** ' ı seçerek ücretsiz bir Power otomatikleştir hesabı oluşturun.

1. Oturum açın ve menüden **Akışlarım** ' ı seçin.

1. **Boş 'ten + otomatik**' i seçin.

    ![Akışlarım + otomatik--boş](./media/commercial-marketplace-lead-management-instructions-https/my-flows-automated.png)

1. **Otomatik akış oluştur** penceresinde **Atla**' yı seçin. 

    ![Otomatik akış penceresi atlama düğmesi oluştur](./media/commercial-marketplace-lead-management-instructions-https/build-automated-flow.png)

1. **Bağlayıcılar ve Tetikleyiciler ara** alanında istek bağlayıcısını bulmak için **istek** girin.
1. **Tetikleyiciler**altında, **bir http isteği alındığında**öğesini seçin. 

    ![Tetikleyiciler menüsü](./media/commercial-marketplace-lead-management-instructions-https/request-connector.png)

1. **BIR http isteği alındığında** penceresinde, aşağıdaki JSON şemasını kopyalayıp **Istek gövdesi JSON şeması** metin kutusuna yapıştırın. Bu şema Microsoft tarafından müşteri adayı verilerinizi içerecek şekilde kullanılır.

    ![İstek gövdesi JSON şeması metin kutusu](./media/commercial-marketplace-lead-management-instructions-https/https-request-received.png)

    **JSON şeması**

    ```JSON
    {
      "$schema": "https://json-schema.org/draft-04/schema#",
      "definitions": {},
      "id": "http://example.com/example.json",
      "properties": {
        "ActionCode": {
          "id": "/properties/ActionCode",
          "type": "string"
        },
        "OfferTitle": {
          "id": "/properties/OfferTitle",
          "type": "string"
        },
        "LeadSource": {
          "id": "/properties/LeadSource",
          "type": "string"
        },
        "UserDetails": {
          "id": "/properties/UserDetails",
          "properties": {
            "Company": {
              "id": "/properties/UserDetails/properties/Company",
              "type": "string"
            },
            "Country": {
              "id": "/properties/UserDetails/properties/Country",
              "type": "string"
            },
            "Email": {
              "id": "/properties/UserDetails/properties/Email",
              "type": "string"
            },
            "FirstName": {
              "id": "/properties/UserDetails/properties/FirstName",
              "type": "string"
            },
            "LastName": {
              "id": "/properties/UserDetails/properties/LastName",
              "type": "string"
            },
            "Phone": {
              "id": "/properties/UserDetails/properties/Phone",
              "type": "string"
            },
            "Title": {
              "id": "/properties/UserDetails/properties/Title",
              "type": "string"
            }
          },
          "type": "object"
        }
      },
      "type": "object"
    }
    ```

>[!NOTE]
>Yapılandırmanın bu noktasında, bir CRM sistemine bağlanmayı ya da bir e-posta bildirimi yapılandırmayı seçebilirsiniz. Seçiminizi temel alarak geri kalan yönergeleri izleyin.

### <a name="connect-to-a-crm-system"></a>Bir CRM sistemine bağlanma

1. **+ Yeni adım**’ı seçin.
1. **Arama bağlayıcılar ve Eylemler '** in bulunduğu yeri arayarak istediğiniz CRM sistemini seçin. Yeni bir kayıt oluşturma eylemi ile **Eylemler** sekmesinde bunu seçin. Aşağıdaki ekran, örnek olarak **Yeni bir kayıt (Dynamics 365) oluşturur** .

    ![Yeni kayıt oluşturma](./media/commercial-marketplace-lead-management-instructions-https/create-new-record.png)

1. CRM sistemiyle ilişkili **kuruluş adını** belirtin. **Varlık adı** açılır listesinden **müşteri adayları** ' nı seçin.

    ![Müşteri adaylarını seçin](./media/commercial-marketplace-lead-management-instructions-https/select-leads.png)

1. Power otomatikleştirmek, müşteri adayı bilgilerini sağlamak için bir form gösterir. Dinamik içerik eklemeyi seçerek giriş isteğinden öğeleri eşleyebilirsiniz. Aşağıdaki ekranda bir örnek olarak **Offertitle** gösterilmektedir.

    ![Dinamik içerik ekle](./media/commercial-marketplace-lead-management-instructions-https/add-dynamic-content.png)

1. İstediğiniz alanları eşleyin ve ardından akışınızı kaydetmek için **Kaydet** ' i seçin. HTTP POST URL 'SI oluşturulur ve **BIR http isteği alındığında** penceresinde erişilebilir. HTTP POST URL 'sinin sağında bulunan kopyalama denetimini kullanarak bu URL 'YI kopyalayın. Tüm URL 'nin herhangi bir bölümünü kaçırmamak için kopyalama denetiminin kullanılması önemlidir. Yayımlama portalında lider yönetimini yapılandırırken bu URL 'YI kaydedin.

    ![Bir HTTP isteği alındığında](./media/commercial-marketplace-lead-management-instructions-https/when-http-request-received.png)

### <a name="set-up-email-notification"></a>E-posta bildirimini ayarlama

1. JSON şemasını tamamladığınıza göre **+ yeni adım**' ı seçin.
1. **Eylem seçin**altında **Eylemler**' i seçin.
1. **Eylemler** sekmesinde, **e-posta gönder (Office 365 Outlook)** seçeneğini belirleyin.

    >[!NOTE]
    >Farklı bir e-posta sağlayıcısı kullanmak istiyorsanız, bunun yerine eylem olarak **bir e-posta bildirimi (posta) Gönder** ' i arayın ve seçin.

    ![E-posta eylemi ekleme](./media/commercial-marketplace-lead-management-instructions-https/https-request-received-send-email.png)

1. **E-posta gönder** penceresinde, aşağıdaki gerekli alanları yapılandırın:

   - **Kime**: müşteri adaylarının gönderileceği en az bir geçerli e-posta adresi girin.
   - **Konu**: Power otomatikleştir, aşağıdaki ekranda gösterilen **leadsource** gibi dinamik içerik ekleme seçeneği sunar. Bir alan adı girerek başlayın. Ardından açılır penceredeki dinamik içerik seçme listesini seçin. 

        >[!NOTE] 
        > Alan adları eklediğinizde, her bir adı iki nokta üst üste (:)) takip edebilirsiniz. sonra yeni bir satır oluşturmak için **ENTER** ' u seçin. Alan adlarınızı ekledikten sonra, dinamik seçim listesinden ilgili her parametreyi ekleyebilirsiniz.

        ![Dinamik içerik kullanarak e-posta eylemi ekleme](./media/commercial-marketplace-lead-management-instructions-https/add-email-using-dynamic-content.png)

   - **Gövde**: dinamik içerik seçim listesinden, e-postanın gövdesinde istediğiniz bilgileri ekleyin. Örneğin, soyadı, ad, e-posta ve şirket kullanın. E-posta bildirimini ayarlamayı tamamladığınızda, aşağıdaki ekranda örneğe benzer şekilde görünür.


       ![E-posta bildirimi örneği](./media/commercial-marketplace-lead-management-instructions-https/send-an-email.png)

1. Akışınızı son vermek için **Kaydet** ' i seçin. HTTP POST URL 'SI oluşturulur ve **BIR http isteği alındığında** penceresinde erişilebilir. HTTP POST URL 'sinin sağında bulunan kopyalama denetimini kullanarak bu URL 'YI kopyalayın. Tüm URL 'nin herhangi bir bölümünü kaçırmamak için bu denetimin kullanılması önemlidir. Yayımlama portalında lider yönetimini yapılandırırken bu URL 'YI kaydedin.

   ![HTTP POST URL 'SI](./media/commercial-marketplace-lead-management-instructions-https/http-post-url.png)

### <a name="testing"></a>Test Etme

Çevrimiçi olarak indirilebilen [Postman](https://app.getpostman.com/app/download/win64)adlı bir araç kullanarak her şeyin beklendiği gibi çalıştığından test edebilirsiniz. Bu araç Windows için kullanılabilir. 

1. Postman 'ı başlatın ve test aracınızı ayarlamak için **Yeni** > **istek** ' ı seçin. 

   ![Test aracınızı ayarlama isteği](./media/commercial-marketplace-lead-management-instructions-https/postman-request.png)

1. **Istek kaydet** formunu doldurup oluşturduğunuz klasöre kaydedin.

   ![Istek formunu Kaydet](./media/commercial-marketplace-lead-management-instructions-https/postman-save-to-test.png)

1. Açılan listeden **gönderi** ' ı seçin. 

   ![Akışımı test etme](./media/commercial-marketplace-lead-management-instructions-https/test-my-flow.png)

1. Power otomatikleştirmek için oluşturduğunuz akıştan gelen HTTP POST URL 'sini, **istek URL 'Si girin**ifadesini yapıştırın.

   ![HTTP POST URL 'sini yapıştırın](./media/commercial-marketplace-lead-management-instructions-https/paste-http-post-url.png)

1. [Power otomatikleştirmeye](https://flow.microsoft.com/)geri dönün. Power otomatikleştir menü çubuğundan **Akışlarıma** giderek müşteri adaylarını göndermek için oluşturduğunuz akışı bulun. Daha fazla seçenek görmek için akış adının yanındaki üç noktayı seçin ve **Düzenle**' yi seçin.


1. Sağ üst köşedeki **Test** ' i seçin, **tetikleme eylemini gerçekleştir**' i seçin ve ardından **Test**' i seçin. Ekranın en üstünde testin başlatıldığını belirten bir gösterge görürsünüz.

   ![Tetikleyici eylemi seçeneğini gerçekleştirmem](./media/commercial-marketplace-lead-management-instructions-https/test-flow-trigger-action.png)

1. Postman uygulamanıza geri dönün ve **Gönder**' i seçin.

   ![Gönder düğmesi](./media/commercial-marketplace-lead-management-instructions-https/postman-send.png)

1. Akışınızı geri dönüp sonucu denetleyin. Her şey beklendiği gibi çalışırsa, akışın başarılı olduğunu belirten bir ileti görürsünüz.

   ![Sonuçları denetle](./media/commercial-marketplace-lead-management-instructions-https/my-flow-check-results.png)

1. Ayrıca bir e-posta almış olmanız gerekir. E-posta gelen kutunuzu denetleyin. 

    >[!NOTE] 
    >Testten bir e-posta görmüyorsanız, istenmeyen posta ve gereksiz klasörlerinizi kontrol edin. Aşağıdaki ekranda, e-posta bildirimini yapılandırdığınızda yalnızca eklediğiniz alan etiketlerini görürsünüz. Teklifinizden üretilmiş gerçek bir müşteri adayı olsaydı, gövdesinde ve konu satırında lider kişağından gerçek bilgileri de görürsünüz.

   ![E-posta alındı](./media/commercial-marketplace-lead-management-instructions-https/email-received.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Teklifinizi, HTTPS uç noktasına müşteri adayları gönderecek şekilde yapılandırma

Yayımlama portalında teklifiniz için lider yönetimi bilgilerini yapılandırmaya hazırsanız, bu adımları izleyin.

1. Teklifiniz için **teklif kurulumu** sayfasına gidin.
1. **Müşteri adayı yönetimi** bölümünde **Bağlan** ' ı seçin.
1. **Bağlantı ayrıntıları** açılır penceresinde, **müşteri adayı hedefi**için **https uç noktası** ' nı seçin. Önceki adımları izleyerek oluşturduğunuz akıştan HTTP POST URL 'sini **https uç noktası URL 'si** alanına yapıştırın.
1. **İletişim e-postası**altında, şirketinizdeki kişilerin yeni bir müşteri adayı alındığında e-posta bildirimleri alması gereken e-posta adreslerini girin. Birden çok e-postayı noktalı virgülle ayırarak sağlayabilirsiniz.
1. **Tamam**’ı seçin.

Bir müşteri adayı hedefine başarıyla bağlandığınızdan emin olmak için **Doğrula** düğmesini seçin. Başarılı olursa, müşteri adayı hedefinde bir test liderine sahip olacaksınız.

>[!NOTE] 
>Teklif için müşteri adaylarını almadan önce teklifin geri kalanını yapılandırmayı ve bunu yayımlamanız gerekir.

Müşteri adayları oluşturulduğunda, Microsoft bu akışı akışa gönderir. Müşteri adayları, yapılandırdığınız CRM sistemine veya e-posta adresine yönlendirilir.

![Lider yönetimi bağlantı düğmesi](./media/commercial-marketplace-lead-management-instructions-https/lead-management-connect.png)

![Bağlantı ayrıntıları müşteri adayı hedefi](./media/commercial-marketplace-lead-management-instructions-https/connection-details.png)

![Bağlantı ayrıntıları Iletişim e-postası](./media/commercial-marketplace-lead-management-instructions-https/https-connection-details.png)

