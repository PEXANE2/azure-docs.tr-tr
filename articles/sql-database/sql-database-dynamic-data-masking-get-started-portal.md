---
title: 'Azure portalı: Dinamik veri maskeleme'
description: Azure portalında SQL Veritabanı dinamik veri maskeleme ile nasıl başlarsınız?
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 03/04/2018
ms.openlocfilehash: a8098b31c6b389b640fc03e756da44c70d9f3a70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722127"
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>Azure portalı ile SQL Veritabanı dinamik veri maskeleme ile başlayın

Bu makalede, Azure portalı ile [dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md) nasıl uygulanacağı gösterilmektedir. [Ayrıca Azure SQL Veritabanı cmdlets](https://docs.microsoft.com/powershell/module/az.sql/) veya REST [API](https://docs.microsoft.com/rest/api/sql/)kullanarak dinamik veri maskeleme uygulayabilirsiniz.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Azure portalını kullanarak veritabanınız için dinamik veri maskeleme ayarlama

1. Azure portalını [https://portal.azure.com](https://portal.azure.com)' dan başlatın.
2. Maskelemek istediğiniz hassas verileri içeren veritabanının ayarlar sayfasına gidin.
3. Dinamik **Veri Maskeleme** yapılandırma sayfasını başlatan Dinamik **Veri** Maskeleme döşemesini tıklatın.

   * Alternatif olarak, **İşlemler** bölümüne ilerleyebilir ve **Dinamik Veri Maskelemesini tıklatabilirsiniz.**

     ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)

4. Dinamik **Veri Maskeleme** yapılandırma sayfasında, öneriler altyapısının maskeleme için işaretlediği bazı veritabanı sütunları görebilirsiniz. Önerileri kabul etmek için, bir veya daha fazla sütun için **Maske Ekle'yi** tıklatmanız yeterlidir ve bu sütunun varsayılan türüne göre bir maske oluşturulur. Maskeleme kuralını tıklatarak ve maskeleme alanı biçimini seçtiğiniz farklı bir biçimde düzenleyerek maskeleme işlevini değiştirebilirsiniz. Ayarlarınızı kaydetmek için **Kaydet'i** tıklattığınızdan emin olun.

    ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)

5. Veritabanınızdaki herhangi bir sütun için maske eklemek için, **Dinamik Veri Maskeleme** yapılandırma sayfasının üst kısmında maske ekle'yi **tıklatın** Maske **Kuralı** yapılandırma sayfasını açın.

    ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)

6. Maskeleme için belirlenen alanı tanımlamak için **Şema,** **Tablo** ve **Sütun'u** seçin.
7. Hassas veri maskeleme kategorileri listesinden **Maskeleme Alanı Biçimi'ni** seçin.

    ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)

8. Dinamik veri maskeleme ilkesindeki maskeleme kuralları kümesini güncelleştirmek için veri maskeleme kuralı sayfasında **Kaydet'i** tıklatın.
9. Maskelemenin dışında tutulabilmesi gereken ve maskesiz hassas verilere erişebilen SQL kullanıcılarını veya AAD kimliklerini yazın. Bu, yarı sütunlu ayrılmış bir kullanıcı listesi olmalıdır. Yönetici ayrıcalıklarına sahip kullanıcılar her zaman özgün maskesiz verilere erişebilir.

    ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    > [!TIP]
    > Uygulama katmanının uygulama ayrıcalıklı kullanıcılar için hassas verileri görüntüleyebilsin diye, uygulamanın veritabanını sorgulamak için kullandığı SQL kullanıcısını veya AAD kimliğini ekleyin. Bu listenin, hassas verilerin açığa çıkmasıen aza indirmek için en az sayıda ayrıcalıklı kullanıcı içermesi önerilir.

10. Yeni veya güncelleştirilmiş maskeleme ilkesini kaydetmek için veri maskeleme yapılandırma sayfasında **Kaydet'i** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Dinamik veri maskeleme genel bakış için, [dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md)bakın.
* [Ayrıca Azure SQL Veritabanı cmdlets](https://docs.microsoft.com/powershell/module/az.sql/) veya REST [API](https://docs.microsoft.com/rest/api/sql/)kullanarak dinamik veri maskeleme uygulayabilirsiniz.
