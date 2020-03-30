---
title: Marketo'da müşteri adayı yönetimini yapılandırma | Azure Marketi
description: Azure pazar yeri müşterileri için Marketo için müşteri adayı yönetimini yapılandırın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dsindona
ms.openlocfilehash: 9fa05eae2d297cbd6ae7243d191cae5a7a3f990e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288538"
---
# <a name="configure-lead-management-in-marketo"></a>Marketo'da müşteri adayı yönetimini yapılandırma

Bu makalede, Marketo'nun Microsoft satış müşteri adaylarını işlemek üzere nasıl ayarlanılacağı açıklanmaktadır.

1. Marketo'da oturum açın.
2. **Tasarım Stüdyosu'ni**seçin.
    ![Marketo Tasarım Stüdyosu](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo1.png)

3.  **Yeni Formu**Seçin.
    ![Marketo yeni formu](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo2.png)

4.  Yeni Form'da gerekli alanları doldurun ve sonra **Oluştur'u**seçin.
    ![Marketo yeni form oluşturmak](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo3.png)

4.  Alan Ayrıntıları'nda **Finish'i**seçin.
    ![Marketo bitiş formu](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo4.png)

5.  Onayla ve Kapat.

6.  MarketplaceLeadBacked sekmesinde, **Kodu Girin'i**seçin.
    ![Marketo gömme kodu seçeneği](./media/cloud-partner-portal-lead-management-instructions-marketo/marketo5.png)

7.  Marketo Embed Code aşağıdaki örneğe benzer kod görüntüler.

`<script src="//app-ys12.marketo.com/js/forms2/js/forms2.min.js"></script>`

    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>

1. Bulut İş Ortağı Portalı'ndaki Marketo alanlarında **Sunucu Kimliğini,** **Munchkin Kimliğini**ve Form **Id'yi** yapılandırabilmeniz için Gömme Kodu'nda gösterilen değerleri kopyalayın.

Marketo Gömme Kodu örneğinden ihtiyacınız olan kimlikleri almak için bir sonraki örneği kılavuz olarak kullanın.

- Sunucu Kimliği = **ys12**
- Munchkin Id = **123-PQR-789**
- Form Kimliği = **1179**
