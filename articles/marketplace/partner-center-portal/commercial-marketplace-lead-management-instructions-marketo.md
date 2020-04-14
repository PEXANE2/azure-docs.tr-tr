---
title: Marketo'da müşteri adayı yönetimini yapılandırma | Azure Marketi
description: Azure pazar yeri müşterileri için Marketo için müşteri adayı yönetimini yapılandırın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 04eae529efcf9509d8cd43a7629bb0b76b593cd0
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81252508"
---
# <a name="configure-lead-management-in-marketo"></a>Marketo'da müşteri adayı yönetimini yapılandırma

Bu makalede, market teklifinizden satış müşteri adaylarını işlemek için Marketo CRM sisteminizin nasıl ayarlanır.

## <a name="set-up-your-marketo-crm-system"></a>Marketo CRM sisteminizi ayarlayın

1. Marketo'da oturum açın.
2. **Tasarım Stüdyosu'ni**seçin.
    ![Marketo Tasarım Stüdyosu](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-1.png)

3.  **Yeni Formu**Seçin.
    ![Marketo yeni formu](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-2.png)

4.  Yeni Form'da gerekli alanları doldurun ve sonra **Oluştur'u**seçin.
    ![Marketo yeni form oluşturmak](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-3.png)

5.  Alan Ayrıntıları'nda **Finish'i**seçin.
    ![Marketo bitiş formu](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-4.png)

6.  Onayla ve Kapat.

7. *MarketplaceLeadBacked* sekmesinde, **Kodu Girin'i**seçin. 

    ![Ekleme kodu](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-6.png)

8. Marketo Embed Code aşağıdaki örneğe benzer kod görüntüler.

    ```
    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>
    ```

9. Katıştırılmış Kod formunda gösterilen aşağıdaki alanların değerlerini kopyalayın. Bu değerleri, teklifinizi bir sonraki adımda müşteri adaylarını almak üzere yapılandırmak için kullanırsınız. Marketo Gömme Kodu örneğinden ihtiyacınız olan kimlikleri almak için bir sonraki örneği kılavuz olarak kullanın.

    - Sunucu Kimliği = **ys12**
    - Munchkin ID = **123-PQR-789**
    - Form Kimliği = **1179**

    **Bu değerleri çözmenin başka bir yolu**

    - Sunucu kimliği, Marketo örneğinizin URL'sinde bulunur,`serverID.marketo.com`örneğin , " ".
    - "Munchkin Hesap Kimliği" alanında admin>Munchkin menüsüne giderek veya Marketo REST API ana etki alanının ilk bölümünden aboneliğinizin Munching kimliğini alın: `https://{Munchkin ID}.mktorest.com`.
    - Form Kimliği, müşteri adaylarını pazardan yönlendirmek için adım 7'de oluşturduğunuz Gömme Kodu formunun kimliğidir.

## <a name="configure-your-offer-to-send-leads-to-marketo"></a>Teklifinizi Marketo'ya müşteri adayları göndermek için yapılandırın

Yayımlama portalında teklifiniz için müşteri adayı yönetimi bilgilerini yapılandırmaya hazır olduğunuzda aşağıdaki adımları izleyin: 

1. **Teklifiniz** için Teklif kurulum sayfasına gidin.
1. Müşteri Adayı Yönetimi bölümü altında **Bağlan'ı** seçin. 

    ![Müşteri adayı yönetimi - Bağlan](./media/commercial-marketplace-lead-management-instructions-marketo/lead-management-connect.png)

1. Bağlantı ayrıntıları açılır penceresinde, Müşteri Adayı Hedef için **Marketo'yu** seçin.

    ![Müşteri adayı hedefi seçin](./media/commercial-marketplace-lead-management-instructions-marketo/choose-lead-destination.png)

4. Sunucu **Kimliği,** **Munching hesap kimliği**ve **Form Kimliği**sağlayın.

    >[!Note]
    >Teklifin geri kalanını yapılandırmayı bitirmeniz ve teklifiçin müşteri adayı alabilmek için yayımlamanız gerekir. 

5. **Kişi e-postası** - Şirketinizde yeni bir müşteri adayı geldiğinde e-posta bildirimleri alması gereken kişiler için e-posta sağlayın. Bir semicolon ile ayırarak birden çok e-posta sağlayabilir.
6. **Tamam'ı**seçin.

Bir müşteri adayı hedefine başarıyla bağlandığınızdan emin olmak için doğrulama düğmesini tıklatın. Başarılı olursa, müşteri adayı hedefinde bir test müşteri adayınız olur.

![Bağlantı ayrıntıları](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-connection-details.png)