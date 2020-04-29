---
title: Bir HTTPS uç noktası kullanarak müşteri adayı yönetimini yapılandırma | Azure Marketi
description: Microsoft AppSource ve Azure Market müşteri adaylarını işlemek için bir HTTP uç noktası kullanmayı öğrenin.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: dsindona
ms.openlocfilehash: f56cc5aaad7d77ff8dc753115ef1becb08ddde73
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81770160"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>HTTPS uç noktası kullanarak müşteri adayı yönetimini yapılandırma

Microsoft AppSource ve Azure Market müşteri adaylarını işlemek için bir HTTPS uç noktası kullanabilirsiniz. Bu müşteri adayları, bir müşteri Ilişki yönetimi (CRM) sistemine yazılabilir veya e-posta bildirimi olarak gönderilebilir. Bu makalede, [Microsoft Power otomatikleştiren](https://powerapps.microsoft.com/automate-processes/) Otomasyon Hizmeti 'nin müşteri adayı yönetimini yapılandırmak için nasıl kullanılacağı açıklanır.

## <a name="create-a-flow-using-microsoft-power-automate"></a>Microsoft Power otomatikleştir kullanarak akış oluşturma

1. [Power otomatikleştirir](https://flow.microsoft.com/) Web sayfasını açın. **Oturum aç '** ı seçin veya ücretsiz **kaydolun** ' ı seçerek ücretsiz bir akış hesabı oluşturun.

1. Oturum açın ve menü çubuğunda **Akışlarım** ' ı seçin.
    > [!div class="mx-imgBorder"]
    > ![Akışlarım](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows.png)

1. **+ Yeni**altında, **boş 'tan + anında**' yı seçin.
    > [!div class="mx-imgBorder"]
    > ![Sıfırdan oluştur](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank.png)

1. Akışınızı adlandırın ve ardından **bu akışın nasıl tetikleneceğini seçin**altında **bir http isteği alındığında**öğesini seçin.

    > [!div class="mx-imgBorder"]
    > ![Alınan HTTP isteği tetikleyicisini seçin](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-pick-request-trigger.png)

1. Açmak için Flow adımına tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Flow adımını Genişlet](./media/cloud-partner-portal-lead-management-instructions-https/expand-flow-step.png)

1. **Istek GÖVDESI JSON şemasını**yapılandırmak için aşağıdaki yöntemlerden birini kullanın:

   - Bu makalenin sonundaki [JSON şemasını](#json-schema) , **Istek gövdesi JSON şeması** metin kutusuna kopyalayın.
   - **Şema oluşturmak için örnek yük kullanma** öğesini seçin. **Örnek JSON yükü girin veya yapıştırın** metin kutusunda [JSON örneğine](#json-example)yapıştırın. Şemayı oluşturmak için **bitti** ' yi seçin.

   >[!Note]
   >Akışta bu noktada, bir CRM sistemine bağlanabilir veya bir e-posta bildirimi yapılandırabilirsiniz.

### <a name="to-connect-to-a-crm-system"></a>Bir CRM sistemine bağlanmak için

1. **+ Yeni adım**’ı seçin.
2. Yeni bir kayıt oluşturma eylemiyle tercih ettiğiniz CRM sistemini seçin. Aşağıdaki ekran yakalama, Dynamics 365 ' i gösterir. örnek olarak **Yeni bir kayıt oluşturur** .

    ![Yeni kayıt oluşturma](./media/cloud-partner-portal-lead-management-instructions-https/https-image009.png)

3. Bağlayıcının bağlantı girişleri olan **kuruluş adını** belirtin. **Varlık adı** açılır listesinden **müşteri adayları** ' nı seçin.

    ![Müşteri adaylarını seçin](./media/cloud-partner-portal-lead-management-instructions-https/https-image011.png)

4. Flow, müşteri adayı bilgilerini sağlamak için bir form gösterir. Dinamik içerik eklemeyi seçerek giriş isteğinden öğeleri eşleyebilirsiniz. Aşağıdaki ekran yakalama, bir örnek olarak **Offertitle** gösterir.

    ![Dinamik içerik ekle](./media/cloud-partner-portal-lead-management-instructions-https/https-image013.png)

5. İstediğiniz alanları eşleyin ve sonra akışınızı kaydetmek için **Kaydet** ' i seçin.

6. İstekte bir HTTP POST URL 'SI oluşturulur. Bu URL 'YI kopyalayın ve HTTPS uç noktası olarak kullanın.

    ![HTTP Post URL 'SI](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

### <a name="to-set-up-email-notification"></a>E-posta bildirimi ayarlamak için

1. **+ Yeni adım**’ı seçin.
2. **Eylem seçin**altında **Eylemler**' i seçin.
3. **Eylemler** altında **E-posta gönder**’i seçin.

    ![E-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-add-email-action.png)

4. **E-posta gönder**' de, aşağıdaki gerekli alanları yapılandırın:

   - En az bir geçerli **e-posta** adresi girin.
   - **Konu** akışı, aşağıdaki ekran yakalamadaki **Leadsource** gibi dinamik içerik ekleme seçeneği sunar.

     ![Dinamik içerik kullanarak e-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-dynamic-content.png)

   - **Gövde** -dinamik içerik listesinden, e-postanın gövdesinde istediğiniz bilgileri ekleyin. Örneğin, soyadı, ad, e-posta ve şirket.

   E-posta bildirimini ayarlamayı tamamladığınızda, aşağıdaki ekran yakalamadaki örneğe benzer şekilde görünür.

   ![E-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-action.png)

5. Akışınızı son vermek için **Kaydet** ' i seçin.

6. İstekte bir HTTP POST URL 'SI oluşturulur. Bu URL 'YI kopyalayın ve HTTPS uç noktası olarak kullanın.

    ![HTTP Post URL 'SI](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Teklifinizi, HTTPS uç noktasına müşteri adayları gönderecek şekilde yapılandırma

Teklifiniz için lider yönetimi bilgilerini yapılandırırken, **müşteri adayı hedefi** Için **https uç noktası** ' nı seçin ve ÖNCEKI adımda kopyaladığınız http post URL 'sine yapıştırın.  

![Dinamik içerik ekle](./media/cloud-partner-portal-lead-management-instructions-https/https-image017.png)

Müşteri adayları oluşturulduğunda, Microsoft, sizin yapılandırdığınız CRM sistemine veya e-posta adresine yönlendirilmeye yönelik olarak, Power otomatikleştir akışınıza müşteri adayları gönderir.

## <a name="json-schema-and-example"></a>JSON şeması ve örneği

JSON test örneği aşağıdaki şemayı kullanır:

### <a name="json-schema"></a>JSON şeması

``` json
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
    "Description": {
      "id": "/properties/Description",
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

Akışta test olarak kullanmak için aşağıdaki JSON örneğini kopyalayabilir ve düzenleyebilirsiniz.

### <a name="json-example"></a>JSON örneği

```json
{
  "UserDetails": {
    "FirstName": "Some",
    "LastName": "One",
    "Email": "someone@contoso.com",
    "Phone": "16175555555",
    "Country": "USA",
    "Company": "Contoso",
    "Title": "Esquire"
 },
  "LeadSource": "AzureMarketplace",
  "ActionCode": "INS",
  "OfferTitle": "Test Microsoft",
  "Description": "Test run through Power Automate"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Daha önce yapmadıysanız, Bulut İş Ortağı Portalı müşteri [adaylarını](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) yapılandırın.
