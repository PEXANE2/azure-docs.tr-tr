---
title: Azure Veri Kutusu Kenarı kullanıcıları yönetir | Microsoft Dokümanlar
description: Azure Veri Kutusu Kenarı'ndaki kullanıcıları yönetmek için Azure portalını nasıl kullanacağınızı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/11/2019
ms.author: alkohli
ms.openlocfilehash: 443ed983a0eec5dfd8f7a917fbc1440cd66c3db3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78946134"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-edge"></a>Azure Veri Kutusu Kenarı'nızdaki kullanıcıları yönetmek için Azure portalını kullanın

Bu makalede, Azure Veri Kutusu Kenarı'nızdaki kullanıcıların nasıl yönetilenbir şekilde yönetilen. You can manage the Azure Data Box Edge via the Azure portal or via the local web UI. Kullanıcı ekleme, değiştirme ve silme işlemleri için Azure portalı kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kullanıcı ekleme
> * Kullanıcıyı değiştirme
> * Kullanıcı silme

## <a name="about-users"></a>Kullanıcılar hakkında

Kullanıcılara salt okunur erişim veya tam ayrıcalık verilebilir. Adından anlaşılacağı üzere salt okunur kullanıcılar paylaşım verilerini yalnızca görüntüleyebilir. Tam ayrıcalık sahibi kullanıcılar paylaşım verilerini okuyabilir, bu paylaşımlara veri yazabilir, paylaşım verilerini değiştirebilir veya silebilir.

 - **Tam ayrıcalıklı kullanıcı**: Tam erişime sahip yerel kullanıcıdır.
 - **Salt okunur kullanıcı**: Salt okunur erişime sahip yerel kullanıcıdır. Bu kullanıcılar yalnızca salt okunur işlemlere izin veren paylaşımlarla ilişkilendirilir.

Kullanıcı izinleri, paylaşım oluşturma sırasında kullanıcı oluşturulurken tanımlanır. Hisse düzeyi izinlerinin değiştirilmesi şu anda desteklenmez.

## <a name="add-a-user"></a>Kullanıcı ekleme

Kullanıcı eklemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Kullanıcılara Genel Bakış >** gidin. Komut çubuğunda **+ Kullanıcı ekle'yi** seçin.

    ![Kullanıcı ekle'yi seçin](media/data-box-edge-manage-users/add-user-1.png)

2. Eklemek istediğiniz kullanıcının kullanıcı adını ve parolasını belirtin. Parolayı onaylayın ve **Ekle'yi**seçin.

    ![Kullanıcı adı ve parola belirtin](media/data-box-edge-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Bu kullanıcılar sistem tarafından ayrılmıştır ve kullanılmamalıdır: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Kullanıcı oluşturma başlatıldığında ve tamamlandığında bir bildirim gösterilir. Kullanıcı oluşturulduktan sonra, komut çubuğundan güncelleştirilmiş kullanıcı listesini görüntülemek için **Yenile'yi** seçin.


## <a name="modify-user"></a>Kullanıcıyı değiştirme

Kullanıcı oluşturulduktan sonra parolasını değiştirebilirsiniz. Kullanıcı listesinden seçim belirleyin. Yeni parolayı girin ve onaylayın. Değişiklikleri kaydedin.
 
![Kullanıcıyı değiştirme](media/data-box-edge-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Kullanıcı silme

Kullanıcı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.


1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Kullanıcılara Genel Bakış >** gidin.

    ![Silmek için kullanıcıyı seçin](media/data-box-edge-manage-users/delete-user-1.png)

2. Kullanıcı listesinden bir kullanıcı seçin ve sonra **Sil'i**seçin.  

   ![Sil'i Seçin](media/data-box-edge-manage-users/delete-user-2.png)

3. Sorulduğunda silme işlemini onaylayın. 

   ![Silmeyi onayla](media/data-box-edge-manage-users/delete-user-3.png)

Kullanıcı listesi silinen kullanıcıya göre güncelleştirilir.

![Güncelleştirilmiş kullanıcı listesi](media/data-box-edge-manage-users/delete-user-4.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](data-box-edge-manage-bandwidth-schedules.md) öğrenin.
