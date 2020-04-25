---
title: Salesforce için lider yönetimini yapılandırma | Azure Marketi
description: Azure Market müşterileri için Salesforce 'ta lider yönetimini yapılandırın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: e0fbb09370e198772b4fc485b3c0fe8a56da4226
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133627"
---
# <a name="configure-lead-management-for-salesforce"></a>Salesforce için lider yönetimini yapılandırma

Bu makalede, ticari Market teklifinizin satış fırsatlarını işlemek üzere Salesforce sisteminizin nasıl ayarlanacağı açıklanır.

> [!NOTE]
> Azure Marketi, **ülke** alanı için değerlerin bir listesi gibi önceden doldurulmuş listeleri desteklemez. Devam etmeden önce ayarlanmış hiçbir liste olmadığından emin olun. Alternatif olarak, bir [https uç noktasını](./commercial-marketplace-lead-management-instructions-https.md) veya bir [Azure tablosunu](./commercial-marketplace-lead-management-instructions-azure-table.md) , müşteri adaylarını alacak şekilde yapılandırabilirsiniz.

## <a name="set-up-your-salesforce-system"></a>Salesforce sisteminizi ayarlama

1. Salesforce 'ta oturum açın.
1. Salesforce aydınlatma deneyimini kullanıyorsanız:
    1. Salesforce giriş sayfasında **Kurulum** ' u seçin.

       ![Salesforce kurulumu](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-1.png)

    1. **Kurulum** sayfasında **platform araçları** > **özellik ayarları** > **Pazarlama** > **Web-müşteri adayı**' na gidin.

        ![Salesforce Web 'den sağlama](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-2.png)

1. Salesforce Classic deneyimini kullanıyorsanız:

    1. Salesforce giriş sayfasında **Kurulum** ' u seçin.

       ![Salesforce klasik kurulum](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-classic-setup.png)

    1. **Kurulum** sayfasında, **derleme** > **Özelleştir** > **müşteri adaylarını** > **Web 'den müşteri adayı**' nı seçin.

        ![Salesforce klasik Web 'den sağlama](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-classic-web-to-lead.png)

Kullandığınız Salesforce deneyimi ne olduğuna bakılmaksızın, yönergelerin geri kalanı aynıdır.

1. Web 'den **müşteri adayı kurulumu** sayfasında, **Web 'Den müşteri adayı oluştur form** düğmesini seçin.
1. **Web 'Den müşteri adayı kurulumunda**, Web 'Den yapılacak **Müşteri formu oluştur**' u seçin.

    ![Salesforce Web 'den müşteri adayına kurulum](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-3.png)

1. **Web 'Den müşteri adayı oluşturma formunda**, `Include reCAPTCHA in HTML` ayarın temizlenmiş olduğundan emin olun ve **Oluştur**' u seçin.

    ![Salesforce bir Web 'den müşteri adayı form bölmesi oluşturma](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-4.png)

1. Size bazı HTML metinleri sunulacaktır. "OID" metnini arayın ve **"OID" DEĞERINI** HTML metinden (yalnızca tırnak işaretleri arasındaki metin) kopyalayın ve kaydedin. Bu değeri, yayımlama portalındaki **kuruluş tanımlayıcı** alanına yapıştırabilirsiniz.

    ![Salesforce HTML "OID" değerini gösteren bir Web-müşteri adayı formu oluştur](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-5.png)

1. **Tamamlandı**' ı seçin.

## <a name="configure-your-offer-to-send-leads-to-salesforce"></a>Teklifinizi Salesforce 'a göndermek için teklifinizi yapılandırma

Yayımlama portalında teklifiniz için lider yönetimi bilgilerini yapılandırmaya hazırsanız, bu adımları izleyin.

1. Teklifiniz için **teklif kurulumu** sayfasına gidin.
1. **Müşteri adayı yönetimi** bölümünde **Bağlan** ' ı seçin.

    ![Lider yönetimi bölümü Bağlan düğmesi](./media/commercial-marketplace-lead-management-instructions-salesforce/lead-management-connect.png)

1. **Bağlantı ayrıntıları** açılır penceresinde, **müşteri adayı hedefi** için `oid` **Salesforce** ' ı seçin ve değeri **kuruluş tanımlayıcı** alanına oluşturduğunuz Web 'den müşteri adayı formundan yapıştırın.

1. **İletişim e-postası**altında, şirketinizdeki kişilerin yeni bir müşteri adayı alındığında e-posta bildirimleri alması gereken e-posta adreslerini girin. Birden çok e-postayı noktalı virgülle ayırarak sağlayabilirsiniz.

1. **Tamam**’ı seçin.

Bir müşteri adayı hedefine başarıyla bağlandığınızdan emin olmak için **Doğrula** düğmesini seçin. Başarılı olursa, müşteri adayı hedefinde bir test liderine sahip olacaksınız.

>[!NOTE]
>Teklif için müşteri adaylarını almadan önce teklifin geri kalanını yapılandırmayı ve bunu yayımlamanız gerekir.

![Bağlantı ayrıntıları açılır penceresi bir müşteri adayı hedefi kutusu seçin](./media/commercial-marketplace-lead-management-instructions-salesforce/choose-lead-destination.png)

![Bağlantı ayrıntıları açılır penceresi kişi e-posta kutusunu doğrula](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-connection-details.png)
