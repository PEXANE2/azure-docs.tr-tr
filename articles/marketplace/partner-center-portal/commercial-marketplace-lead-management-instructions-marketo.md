---
title: Marketo 'da lider Yönetimi-Microsoft ticari marketi
description: Microsoft AppSource ve Azure Marketi 'nden müşteri adaylarını yönetmek için bir Marketo CRM sistemini nasıl kullanacağınızı öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 03/30/2020
ms.openlocfilehash: be1f21f927b01d66e19dc5e97b38e5c35e6664cb
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86120271"
---
# <a name="use-marketo-to-manage-commercial-marketplace-leads"></a>Ticari Market müşteri adaylarını yönetmek için Marketo kullanın

Bu makalede, Microsoft AppSource ve Azure Marketi 'ndeki tekliflerinizin satış fırsatlarını işlemek üzere Marketo CRM sisteminizin nasıl ayarlanacağı açıklanır.

## <a name="set-up-your-marketo-crm-system"></a>Marketo CRM sisteminizi ayarlama

1. Marketo 'da oturum açın.

1. **Design Studio**' yı seçin.

    ![Marketo Design Studio](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-1.png)

1.  **Yeni form**' u seçin.

    ![Marketo yeni formu](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-2.png)

1.  **Yeni form** iletişim kutusunda gerekli alanları doldurup **Oluştur**' u seçin.

    ![Marketo yeni form oluştur](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-3.png)

1.  **Alan ayrıntıları** sayfasında **son**' u seçin.

    ![Marketo bitiş formu](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-4.png)

1.  Onaylayın ve kapatın.

1. **Marketplaceleadarka uç** sekmesinde **kodu ekle**' yi seçin. 

    ![Marketo ekleme kodu](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-6.png)

1. Marketo ekleme kodu, aşağıdaki örneğe benzer bir kod görüntüler.

    ```html
    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>
    ```

1. Ekleme kodu formunda gösterilen aşağıdaki alanlar için değerleri kopyalayın. Bu değerleri, bir sonraki adımda müşteri adaylarını almak üzere teklifinizi yapılandırmak için kullanacaksınız. Bir sonraki örneği, Marketo ekleme kodu örneğinde ihtiyacınız olan kimlikleri alma kılavuzu olarak kullanın.

    - Sunucu KIMLIĞI = **ys12**
    - Munchkabağı No = **123-PQR-789**
    - Form KIMLIĞI = **1179**

    Bu değerleri anlamak için başka bir yol:

    - Sunucu KIMLIĞI, Marketo örneğinizin URL 'sinde bulunur (örneğin,) `serverID.marketo.com` .
    - **Admin**  >  Daha fazla **hesap kimliği** alanındaki yönetici kaldırma menünüzün veya Marketo REST API ana bilgisayar alt etki alanınızın ilk kısmından, aboneliğinizin daha**ucuz bir** kimliğini alın: `https://{Munchkin ID}.mktorest.com` .
    - Form KIMLIĞI, Market 'ten gelen müşteri adaylarını yönlendirmek için 7. adımda oluşturduğunuz ekleme kodu formunun KIMLIĞIDIR.

## <a name="configure-your-offer-to-send-leads-to-marketo"></a>Teklifinizi müşteri adaylarını Marketo 'ya gönderecek şekilde yapılandırma

Yayımlama portalında teklifiniz için lider yönetimi bilgilerini yapılandırmaya hazırsanız, bu adımları izleyin. 

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.

1. Teklifinizi seçin ve **teklif kurulumu** sekmesine gidin.

1. **Müşteri müşteri adayları** bölümünde **Bağlan**' ı seçin.

    :::image type="content" source="./media/commercial-marketplace-lead-management-instructions-marketo/customer-leads.png" alt-text="Müşteri liderleri":::

1. **Bağlantı ayrıntıları** açılır penceresinde, **müşteri adayı hedefi**için **Marketo** ' yı seçin.

    ![Bir müşteri adayı hedefi seçin](./media/commercial-marketplace-lead-management-instructions-marketo/choose-lead-destination.png)

1. **Sunucu kimliği**, BIR **Hesap KIMLIĞI**ve **form kimliği**sağlayın.

    > [!NOTE]
    > Teklif için müşteri adaylarını almadan önce teklifin geri kalanını yapılandırmayı ve bunu yayımlamanız gerekir. 

1. **İletişim e-postası**altında, şirketinizdeki kişilerin yeni bir müşteri adayı alındığında e-posta bildirimleri alması gereken e-posta adreslerini girin. Birden çok e-posta adresini noktalı virgülle ayırarak sağlayabilirsiniz.

1. **Tamam**’ı seçin.

   Bir müşteri adayı hedefine başarıyla bağlandığınızdan emin olmak için **Doğrula**' yı seçin. Başarılı olursa, müşteri adayı hedefinde bir test liderine sahip olacaksınız.

   ![Bağlantı ayrıntıları açılır penceresi](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-connection-details.png)
