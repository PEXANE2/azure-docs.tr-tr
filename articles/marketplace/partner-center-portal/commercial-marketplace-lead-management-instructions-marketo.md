---
title: Marketo 'da lider yönetimini yapılandırma | Azure Marketi
description: Azure Market müşterileri için Marketo için lider yönetimini yapılandırın.
author: qianw211
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: b142e0ab1aaa242157e207ceecf958be51bb1721
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133647"
---
# <a name="configure-lead-management-in-marketo"></a>Marketo 'da lider yönetimini yapılandırma

Bu makalede, ticari Market teklifinizden satış fırsatlarını işlemek üzere Marketo CRM sisteminizin nasıl ayarlanacağı açıklanır.

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

    - Sunucu KIMLIĞI, Marketo örneğinizin URL 'sinde bulunur (örneğin, `serverID.marketo.com`).
    - Daha fazla **hesap kimliği** alanındaki **yönetici** > **kaldırma menünüzün** veya Marketo REST API ana bilgisayar alt etki alanınızın ilk kısmından, aboneliğinizin daha ucuz bir kimliğini alın: `https://{Munchkin ID}.mktorest.com`.
    - Form KIMLIĞI, Market 'ten gelen müşteri adaylarını yönlendirmek için 7. adımda oluşturduğunuz ekleme kodu formunun KIMLIĞIDIR.

## <a name="configure-your-offer-to-send-leads-to-marketo"></a>Teklifinizi müşteri adaylarını Marketo 'ya gönderecek şekilde yapılandırma

Yayımlama portalında teklifiniz için lider yönetimi bilgilerini yapılandırmaya hazırsanız, bu adımları izleyin. 

1. Teklifiniz için **teklif kurulumu** sayfasına gidin.

1. **Müşteri adayı yönetimi** bölümünde **Bağlan** ' ı seçin. 

    ![Lider yönetimi bölümü Bağlan düğmesi](./media/commercial-marketplace-lead-management-instructions-marketo/lead-management-connect.png)

1. **Bağlantı ayrıntıları** açılır penceresinde, **müşteri adayı hedefi**için **Marketo** ' yı seçin.

    ![Bir müşteri adayı hedefi seçin](./media/commercial-marketplace-lead-management-instructions-marketo/choose-lead-destination.png)

1. **Sunucu kimliği**, BIR **Hesap KIMLIĞI**ve **form kimliği**sağlayın.

    > [!NOTE]
    > Teklif için müşteri adaylarını almadan önce teklifin geri kalanını yapılandırmayı ve bunu yayımlamanız gerekir. 

1. **İletişim e-postası**altında, şirketinizdeki kişilerin yeni bir müşteri adayı alındığında e-posta bildirimleri alması gereken e-posta adreslerini girin. Birden çok e-posta adresini noktalı virgülle ayırarak sağlayabilirsiniz.

1. **Tamam**’ı seçin.

   Bir müşteri adayı hedefine başarıyla bağlandığınızdan emin olmak için **Doğrula** düğmesini seçin. Başarılı olursa, müşteri adayı hedefinde bir test liderine sahip olacaksınız.

   ![Bağlantı ayrıntıları açılır penceresi](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-connection-details.png)
