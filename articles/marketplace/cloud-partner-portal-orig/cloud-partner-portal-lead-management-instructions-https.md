---
title: HTTPS Bitiş Noktası | Azure Marketi
description: Bir HTTPS bitiş noktası için müşteri adayı yönetimini yapılandırın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/24/2018
ms.author: dsindona
ms.openlocfilehash: cb6ef173e97a7c2bbd7d7cad5e5074b1f2d0f066
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288606"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>HTTPS bitiş noktasını kullanarak müşteri adayı yönetimini yapılandırma

Azure Marketi ve AppSource müşteri adaylarını işlemek için bir HTTPS bitiş noktası kullanabilirsiniz. Bu müşteri adayları, Müşteri İlişkileri Yönetimi (CRM) sistemine yazılabilir veya e-posta bildirimi olarak gönderilebilir. Bu makalede, [Microsoft Flow](https://powerapps.microsoft.com/automate-processes/) otomasyon hizmetini kullanarak müşteri adayı yönetiminin nasıl yapılandırılacayacağı anlatılıyor.

## <a name="create-a-flow-using-microsoft-flow"></a>Microsoft Flow'u kullanarak akış oluşturma

1. [Akış](https://flow.microsoft.com/) web sayfasını açın. Ücretsiz Akış hesabı oluşturmak için **Kaydol** veya **Ücretsiz Kaydol'u** seçin.

2. Oturum açın ve menü çubuğunda **akışlarım'ı** seçin.

    ![Akışlarım](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows.png)

3. Boştan **+ Oluştur'u**seçin.

    ![Sıfırdan oluştur](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank.png)

4. **Boştan Oluştur'u**seçin.

    ![Sıfırdan oluştur](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank2.png)

5. Arama **bağlayıcıları ve tetikleyiciler** alanında, İstek bağlayıcısını bulmak için "istek" yazın.
6. **Tetikleyiciler**altında, **bir HTTP isteği aldığında**seçin. 

    ![Alınan HTTP isteğini seçin](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-pick-request-trigger.png)

7. **İstek Gövdesi JSON Schema'yı**yapılandırmak için aşağıdaki adımlardan birini kullanın:

   - Bu makalenin sonundaki [JSON şeasını](#json-schema) **İstek Gövdesi JSON Schema** metin kutusuna kopyalayın.
   - **Şema oluşturmak için örnek yük kullanma** öğesini seçin. Örnek **bir JSON yük metin kutusunu girin veya yapıştırın,** [JSON örneğine](#json-example)yapıştırın. Şemayı oluşturmak için **Bitti'yi** seçin.

   >[!Note]
   >Akış bu noktada ya bir CRM sistemine bağlanmak veya bir e-posta bildirimi yapılandırmak.

### <a name="to-connect-to-a-crm-system"></a>CRM sistemine bağlanmak için

1. **+ Yeni adım**’ı seçin.
2. Yeni bir kayıt oluşturmak için eylemle seçtiğiniz CRM sistemini seçin. Aşağıdaki ekran yakalama **Dinamikleri 365** gösterir - Örnek olarak yeni bir kayıt oluşturun.

    ![Yeni kayıt oluşturma](./media/cloud-partner-portal-lead-management-instructions-https/https-image009.png)

3. Bağlayıcınızın bağlantı girişleri olan **Kuruluş Adı'nı** sağlayın. **Varlık Adı** açılır listesinden **Müşteri Adayları'nı** seçin.

    ![Müşteri adaylarını seçme](./media/cloud-partner-portal-lead-management-instructions-https/https-image011.png)

4. Akış, müşteri adayı bilgileri sağlamak için bir form gösterir. Dinamik içerik eklemeyi seçerek giriş isteğindeki öğeleri eşleyebilirsiniz. Aşağıdaki ekran yakalama, **OfferTitle'ı** örnek olarak gösterir.

    ![Dinamik içerik ekle](./media/cloud-partner-portal-lead-management-instructions-https/https-image013.png)

5. İstediğiniz alanları eşle ve ardından akışınızı kaydetmek için **Kaydet'i** seçin.

6. İstekte bir HTTP POST URL'si oluşturulur. Bu URL'yi kopyalayın ve HTTPS bitiş noktası olarak kullanın.

    ![HTTP Mesaj URL](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

### <a name="to-set-up-email-notification"></a>E-posta bildirimini ayarlamak için

1. **+ Yeni adım**’ı seçin.
2. Bir **eylem seçin** **altında, Eylemler'i**seçin.
3. **Eylemler** altında **E-posta gönder**’i seçin.

    ![E-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-add-email-action.png)

4. **E-posta**gönder'de, aşağıdaki gerekli alanları yapılandırın:

   - **Için** - En az bir geçerli e-posta adresi girin.
   - **Konu** - Akış, aşağıdaki ekran yakalamada **LeadSource** gibi Dinamik içerik ekleme seçeneği sunar.

     ![Dinamik içeriği kullanarak e-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-dynamic-content.png)

   - **Gövde** - Dinamik içerik listesinden, e-postanın gövdesine istediğiniz bilgileri ekleyin. Örneğin, Soyadı, İlkAdı, E-posta ve Şirket.

   E-posta bildirimini ayarlamayı bitirdiğinizde, aşağıdaki ekran yakalamaörneğinde örnek gibi görünür.

   ![E-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-action.png)

5. Akışınızı tamamlamak için **Kaydet'i** seçin.
6. İstekte bir HTTP POST URL'si oluşturulur. Bu URL'yi kopyalayın ve HTTPS bitiş noktası olarak kullanın.

    ![HTTP Mesaj URL](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Müşteri adaylarını HTTPS bitiş noktasına gönderecek şekilde teklifinizi yapılandırın

Teklifiniz için müşteri adayı yönetimi bilgilerini yapılandırdığınızda, **Müşteri Adayı Hedefi** için HTTPS Bitiş **Noktası'nı** seçin ve önceki adımda kopyaladığınız HTTP POST URL'ye yapıştırın.  

![Dinamik içerik ekle](./media/cloud-partner-portal-lead-management-instructions-https/https-image017.png)

Müşteri adayları oluşturulduğunda, Microsoft, yapılandırılan CRM sistemine veya e-posta adresine yönlendirilen Akış'a müşteri adayları gönderir.

## <a name="json-schema-and-example"></a>JSON şema ve örnek

JSON test örneği aşağıdaki şema kullanır:

### <a name="json-schema"></a>JSON şema

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

MS Akışınızda test olarak kullanmak üzere aşağıdaki JSON örneğini kopyalayabilir ve edinebilirsiniz.

### <a name="json-example"></a>JSON örneği

```json
{
"OfferTitle": "Test Microsoft",
"LeadSource": "Test run through MS Flow",
"UserDetails": {
"Company": "Contoso",
"Country": "USA",
"Email": "someone@contoso.com",
"FirstName": "Some",
"LastName": "One",
"Phone": "16175555555",
"Title": "Esquire"
}
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bunu daha önce yapmadıysanız, Bulut İş Ortağı Portalı'ndaki müşteri [müşteri adaylarını](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) yapılandırın.
