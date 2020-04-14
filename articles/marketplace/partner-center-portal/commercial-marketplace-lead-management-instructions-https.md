---
title: HTTPS Bitiş Noktası | Azure Marketi
description: Bir HTTPS bitiş noktası için müşteri adayı yönetimini yapılandırın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 33359883df86091120295b93618a13476e428d2f
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262761"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>HTTPS bitiş noktasını kullanarak müşteri adayı yönetimini yapılandırma

>[!Note]
>Bu talimatlarda kullanılan Power Automate konektörü, Power Automate için ücretli bir abonelik gerektirir. Bu belgedeki talimatları takip etmeden önce lütfen bunu açıklayın.

Müşteri İlişkileri Yönetimi (CRM) sisteminiz Azure Marketi ve AppSource müşteri adaylarını almak için İş Ortağı Merkezi'nde açıkça desteklenmiyorsa, bu müşteri adaylarını işlemek için Power Automate'de bir HTTPS bitiş noktası kullanabilirsiniz. HTTPS bitiş noktası yla, bu müşteri adayları e-posta bildirimi olarak gönderilebilir veya Power Automate tarafından desteklenen bir Müşteri İlişkileri Yönetimi (CRM) sistemine yazılabilir. Bu makaledeki talimatlar, Lider Yönetimi > **HTTPS Endpoint URL** alanı için yayın portalına gireceğiniz HTTP POST URL'sini oluşturacağınız Power Automate'i kullanarak yeni bir akış oluşturmak için temel süreçte size yol açacaktır. Ayrıca, çevrimiçi bulunabilir [Postman](https://www.getpostman.com/downloads/) adlı bir araç yardımıyla akışınızı test edebilirsiniz nasıl talimatlar dahildir.

## <a name="create-a-flow-using-power-automate"></a>Power Automate kullanarak akış oluşturma

1. [Akış](https://flow.microsoft.com/) web sayfasını açın. **Oturum Aç'ı**seçin veya zaten bir hesabınız yoksa, ücretsiz Akış hesabı oluşturmak için **ücretsiz Kaydol'u** seçin.

2. Oturum açın ve menü çubuğunda **akışlarım'ı** seçin.

3. +Otomatik 'i seçin **- boştan**.

    ![Akışlarım + Otomatik - boş](./media/commercial-marketplace-lead-management-instructions-https/my-flows-automated.png)

4. Otomatik *akış* penceresi oluştur'da **Atla'yı**seçin. 

    ![Otomatik akış oluşturma - Atla](./media/commercial-marketplace-lead-management-instructions-https/build-automated-flow.png)

5. Arama **bağlayıcıları ve tetikleyiciler** alanında, İstek bağlayıcısını bulmak için "istek" yazın.
6. *Tetikleyiciler*altında, **bir HTTP isteği aldığında**seçin. 

    ![istek bağlayıcısı - Tetikleyiciler](./media/commercial-marketplace-lead-management-instructions-https/request-connector.png)

7. Bir *HTTP isteği geldiğinde* pencere kopyalayın ve Aşağıdaki JSON şema **sını İstek Gövdesi JSON Schema** metin kutusuna yapıştırın. Bu şema, Microsoft tarafından müşteri adayı verilerinizi içerecek şekilde kullanılır.

    ![istek bağlayıcısı - Tetikleyiciler](./media/commercial-marketplace-lead-management-instructions-https/https-request-received.png)

    **JSON şema**

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

>[!Note]
>Yapılandırmanın bu noktasında, bir CRM sistemine bağlanmak veya bir e-posta bildirimi yapılandırmak için seçebilirsiniz. Seçtiğinize göre kalan talimatları izleyin.

### <a name="to-connect-to-a-crm-system"></a>CRM sistemine bağlanmak için

1. **+ Yeni adım**’ı seçin.
2. *Arama bağlayıcıları ve eylemleri*dediği yerde arama yaparak seçtiğiniz CRM sistemini seçin ve yeni bir kayıt oluşturmak için eylemle birlikte *Eylemler* bölümünün altında seçin. Aşağıdaki ekran yakalama **Dinamikleri 365** gösterir - Örnek olarak yeni bir kayıt oluşturun.

    ![Yeni kayıt oluşturma](./media/commercial-marketplace-lead-management-instructions-https/create-new-record.png)

3. CRM sistemiyle ilişkili **Kuruluş Adını** sağlayın. **Varlık Adı** açılır listesinden **Müşteri Adayları'nı** seçin.

    ![Müşteri adaylarını seçme](./media/commercial-marketplace-lead-management-instructions-https/select-leads.png)

4. Akış, müşteri adayı bilgileri sağlamak için bir form gösterir. Dinamik İçerik eklemeyi seçerek giriş isteğindeki öğeleri eşleyebilirsiniz. Aşağıdaki ekran yakalama, **OfferTitle'ı** örnek olarak gösterir.

    ![Dinamik içerik ekle](./media/commercial-marketplace-lead-management-instructions-https/add-dynamic-content.png)

5. İstediğiniz alanları eşle ve ardından akışınızı kaydetmek için **Kaydet'i** seçin. Bir HTTP POST URL'si oluşturulur ve *BIR HTTP isteği ne zaman alınır* penceresinde erişilebilir. HTTP POST URL'sinin sağında bulunan kopyalama denetimini kullanarak bu URL'yi kopyalayın - bu, yanlışlıkla URL'nin herhangi bir bölümünü kaçırmamanız için önemlidir. Yayın portalında müşteri adayı yönetimini yapılandırırken bu URL'yi ihtiyacınız olan şekilde kaydedin.

    ![Bir HTTP isteği aldığında.](./media/commercial-marketplace-lead-management-instructions-https/when-http-request-received.png)

### <a name="to-set-up-email-notification"></a>E-posta bildirimini ayarlamak için

1. Şimdi JSON şema tamamladınız, seçin **+ Yeni adım**.
2. Bir **eylem seçin** **altında, Eylemler'i**seçin.
3. **Eylemler** **altında, e-posta gönder 'i (Office 365 Outlook)** seçeneğini belirleyin.

    >[!Note]
    >Farklı bir e-posta sağlayıcısı aramak istiyorsanız ve eylem olarak *e-posta bildirimi (Posta) gönder'i* seçin.

    ![E-posta eylemi ekleme](./media/commercial-marketplace-lead-management-instructions-https/https-request-received-send-email.png)

4. **E-posta gönder** penceresinde, aşağıdaki gerekli alanları yapılandırın:

   - **Için** - Müşteri adaylarının göndereceği en az bir geçerli e-posta adresi girin.
   - **Konu** - Akış, aşağıdaki ekran yakalamada **LeadSource** gibi Dinamik içerik ekleme seçeneği sunar. Açılan pencereden Dinamik İçerik seçim listesine tıklayarak bir alan adı yazarak başlayın. 

        >[!Note] 
        > Alan adları eklerken, her biri bir ":" ile takip edebilir ve sonra yeni bir satır oluşturmak için girin. Alan adlarınızı ekledikten sonra dinamik seçim listesinden ilişkili her parametreyi ekleyebilirsiniz.

        ![Dinamik içeriği kullanarak e-posta eylemi ekleme](./media/commercial-marketplace-lead-management-instructions-https/add-email-using-dynamic-content.png)

   - **Gövde** - Dinamik İçerik seçim listesinden, e-postanın gövdesine istediğiniz bilgileri ekleyin. Örneğin, Soyadı, İlkAdı, E-posta ve Şirket. <br> <br> E-posta bildirimini ayarlamayı bitirdiğinizde, aşağıdaki ekran yakalamaörneğinde örnek gibi görünür.


       ![E-posta eylemi ekleme](./media/commercial-marketplace-lead-management-instructions-https/send-an-email.png)

5. Akışınızı tamamlamak için **Kaydet'i** seçin. Bir HTTP POST URL'si oluşturulur ve *BIR HTTP isteği ne zaman alınır* penceresinde erişilebilir. HTTP POST URL'sinin sağında bulunan kopyalama denetimini kullanarak bu URL'yi kopyalayın - bu, yanlışlıkla URL'nin herhangi bir bölümünü kaçırmamanız için önemlidir. Yayın portalında müşteri adayı yönetimini yapılandırırken bu URL'yi ihtiyacınız olan şekilde kaydedin.

   ![HTTP SONRASı URL ](./media/commercial-marketplace-lead-management-instructions-https/http-post-url.png)

### <a name="testing"></a>Sınama

Her şeyin beklendiği gibi, [postman](https://app.getpostman.com/app/download/win64)adlı bir aracı kullanarak beklendiği gibi çalıştığını test edebilirsiniz, hangi online indirilebilir. Bu Windows için kullanılabilir. 

1. Postacı'yı başlatın ve test aracınızı ayarlamak için **Yeni** > **İstek'i** seçin. 

   ![Test aracınızı kurma isteği](./media/commercial-marketplace-lead-management-instructions-https/postman-request.png)

2. *İstek Kaydet* formunu doldurun ve ardından oluşturduğunuz klasöre **kaydedin.**

   ![İsteği kaydetme](./media/commercial-marketplace-lead-management-instructions-https/postman-save-to-test.png)

3. Açılan listeden **POST'u** seçin. 

   ![Akışımı test edin](./media/commercial-marketplace-lead-management-instructions-https/test-my-flow.png)

4. HTTP POST URL'sini Power Automate'de oluşturduğunuz akıştan, istek URL'sini girin yazan akıştan *yapıştırın.*

   ![HTTP POST URL'sini yapıştır](./media/commercial-marketplace-lead-management-instructions-https/paste-http-post-url.png)

5. Akış menüsü çubuğundan **Akışlarım'a** giderek müşteri adayları göndermek için oluşturduğunuz akışı bulun ve [Akış'a](https://flow.microsoft.com/) geri dön.  Akış adının yanındaki 3 noktayı seçin ve **Edit'i**seçin.

   ![Akışlarım - Edit](./media/commercial-marketplace-lead-management-instructions-https/my-flows-edit.png)

6. Sağ üst köşede **Test'i** seçin, "Tetikleyici eylemini gerçekleştireceğim" seçeneğini belirleyin ve **Test'i**seçin. Ekranın üst kısmında testin başladığını gösteren bir gösterge görürsünüz

   ![Test akışı - tetikleme](./media/commercial-marketplace-lead-management-instructions-https/test-flow-trigger-action.png)

7. Postacı uygulamanıza geri dön ve HTTPS URL'sini yapıştırdığınız yerin yanına **Gönder'i** seçin.

   ![Akışımı test edin - Gönder](./media/commercial-marketplace-lead-management-instructions-https/postman-send.png)

8. Akışınıza geri dön ve sonucu kontrol et. Her şey beklendiği gibi çalışırsa, başarılı olduğunu belirten bir ileti görürsünüz.

   ![Akış - Sonuçları kontrol et](./media/commercial-marketplace-lead-management-instructions-https/my-flow-check-results.png)

9. Ayrıca bir e-posta almış olmalıdır. E-posta gelen kutunuzu kontrol edin. 

    >[!Note] 
    >Testten bir e-posta görmüyorsanız, spam ve önemsiz klasörlerinizi kontrol edin. Aşağıda, e-posta bildirimini yapılandırırken eklediğiniz alan etiketlerini göreceksiniz. Bu, teklifinizden oluşturulan gerçek bir müşteri adayı ysa, gövdedeki ve Konu Satırındaki Müşteri Adayı Kişisi'nden gelen gerçek bilgileri de görürsünüz.

   ![Alınan e-posta](./media/commercial-marketplace-lead-management-instructions-https/email-received.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Müşteri adaylarını HTTPS bitiş noktasına gönderecek şekilde teklifinizi yapılandırın

Yayımlama portalında teklifiniz için müşteri adayı yönetimi bilgilerini yapılandırmaya hazır olduğunuzda aşağıdaki adımları izleyin:

1. **Teklifiniz** için Teklif kurulum sayfasına gidin.
2. Müşteri Adayı Yönetimi bölümü altında **Bağlan'ı** seçin.
3. Bağlantı ayrıntıları açılır penceresinde, **Müşteri Adayı Hedefi** için HTTPS Bitiş **Noktası'nı** seçin ve HTTPS bitiş **noktası URL** alanına daha önceki adımları izleyerek oluşturduğunuz akıştan HTTP POST URL'sine yapıştırın.
4. **Kişi e-postası** - Şirketinizde yeni bir müşteri adayı geldiğinde e-posta bildirimleri alması gereken kişiler için e-posta sağlayın. Bir semicolon ile ayırarak birden çok e-posta sağlayabilir.
5. **Tamam'ı**seçin.

Bir müşteri adayı hedefine başarıyla bağlandığınızdan emin olmak için doğrulama düğmesini tıklatın. Başarılı olursa, müşteri adayı hedefinde bir test müşteri adayınız olur.

>[!Note] 
>Teklifin geri kalanını yapılandırmayı bitirmeniz ve teklifiçin müşteri adayı alabilmek için yayımlamanız gerekir.

Müşteri adayları oluşturulduğunda, Microsoft, yapılandırılan CRM sistemine veya e-posta adresine yönlendirilen Akış'a müşteri adayları gönderir.

![Müşteri adayı yönetimi - bağlantı](./media/commercial-marketplace-lead-management-instructions-https/lead-management-connect.png)

![Bağlantı ayrıntıları](./media/commercial-marketplace-lead-management-instructions-https/connection-details.png)

![Bağlantı ayrıntıları](./media/commercial-marketplace-lead-management-instructions-https/https-connection-details.png)

