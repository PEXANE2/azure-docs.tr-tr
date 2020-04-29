---
title: Marketo 'da lider yönetimini yapılandırma | Azure Marketi
description: Azure Market müşterileri için Marketo için lider yönetimini yapılandırın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dsindona
ms.openlocfilehash: 9fa05eae2d297cbd6ae7243d191cae5a7a3f990e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80288538"
---
# <a name="configure-lead-management-in-marketo"></a>Marketo 'da lider yönetimini yapılandırma

Bu makalede, Microsoft satış fırsatlarını işlemek üzere Marketo 'nın nasıl ayarlanacağı açıklanır.

1. Marketo 'da oturum açın.
2. **Design Studio**' yı seçin.
    ![Marketo Design Studio](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo1.png)

3.  **Yeni form**' u seçin.
    ![Marketo yeni formu](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo2.png)

4.  Yeni formdaki gerekli alanları doldurup **Oluştur**' u seçin.
    ![Marketo yeni form oluştur](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo3.png)

4.  Alan ayrıntıları sayfasında **son**' u seçin.
    ![Marketo bitiş formu](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo4.png)

5.  Onaylayın ve kapatın.

6.  Marketplaceleaddesteklenen sekmesinde **kodu ekle**' yi seçin.
    ![Marketo ekleme kodu seçeneği](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo5.png)

7.  Marketo ekleme kodu, aşağıdaki örneğe benzer bir kod görüntüler.

`<script src="//app-ys12.marketo.com/js/forms2/js/forms2.min.js"></script>`

    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>

1. Ekleme kodunda gösterilen değerleri kopyalayın, bu sayede **sunucu kimliği**, bir **kimlik kimliği**ve **form kimliğini** bulut iş ortağı portalı Marketo alanlarında yapılandırabilirsiniz.

Bir sonraki örneği, Marketo ekleme kodu örneğinde ihtiyacınız olan kimlikleri alma kılavuzu olarak kullanın.

- Sunucu kimliği = **ys12**
- Munchkabağı No = **123-PQR-789**
- Form kimliği = **1179**
