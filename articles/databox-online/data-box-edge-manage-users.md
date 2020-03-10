---
title: Azure Data Box Edge kullanıcıları yönetme | Microsoft Docs
description: Azure Data Box Edge kullanıcıları yönetmek için Azure portal nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/11/2019
ms.author: alkohli
ms.openlocfilehash: 443ed983a0eec5dfd8f7a917fbc1440cd66c3db3
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78946134"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-edge"></a>Azure Data Box Edge kullanıcıları yönetmek için Azure portal kullanın

Bu makalede, Azure Data Box Edge kullanıcıların nasıl yönetileceği açıklanmaktadır. Azure Data Box Edge Azure portal veya yerel Web Kullanıcı arabirimi aracılığıyla yönetebilirsiniz. Kullanıcı ekleme, değiştirme ve silme işlemleri için Azure portalı kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kullanıcı ekleme
> * Kullanıcıyı değiştirme
> * Kullanıcı silme

## <a name="about-users"></a>Kullanıcılar hakkında

Kullanıcılara salt okunur erişim veya tam ayrıcalık verilebilir. Adından anlaşılacağı üzere salt okunur kullanıcılar paylaşım verilerini yalnızca görüntüleyebilir. Tam ayrıcalık sahibi kullanıcılar paylaşım verilerini okuyabilir, bu paylaşımlara veri yazabilir, paylaşım verilerini değiştirebilir veya silebilir.

 - **Tam ayrıcalıklı kullanıcı**: Tam erişime sahip yerel kullanıcıdır.
 - **Salt okunur kullanıcı**: Salt okunur erişime sahip yerel kullanıcıdır. Bu kullanıcılar yalnızca salt okunur işlemlere izin veren paylaşımlarla ilişkilendirilir.

Kullanıcı izinleri, paylaşım oluşturma sırasında kullanıcı oluşturulurken tanımlanır. Paylaşma düzeyi izinlerinin değiştirilmesi Şu anda desteklenmiyor.

## <a name="add-a-user"></a>Kullanıcı ekleme

Kullanıcı eklemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Azure portal, Data Box Edge kaynağınız ' ne gidin ve ardından **genel bakış > kullanıcılar**' a gidin. Komut çubuğunda **+ Kullanıcı Ekle** ' yi seçin.

    ![Kullanıcı Ekle ' yi seçin](media/data-box-edge-manage-users/add-user-1.png)

2. Eklemek istediğiniz kullanıcının kullanıcı adını ve parolasını belirtin. Parolayı onaylayın ve **Ekle**' yi seçin.

    ![Kullanıcı adı ve parola belirtin](media/data-box-edge-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Bu kullanıcılar sistem tarafından ayrılmıştır ve kullanılmamalıdır: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Kullanıcı oluşturma başladığında ve tamamlandığında bir bildirim gösterilir. Kullanıcı oluşturulduktan sonra, komut çubuğundan, güncelleştirilmiş Kullanıcı listesini görüntülemek için **Yenile** ' yi seçin.


## <a name="modify-user"></a>Kullanıcıyı değiştirme

Kullanıcı oluşturulduktan sonra parolasını değiştirebilirsiniz. Kullanıcı listesinden seçin. Yeni parolayı girin ve onaylayın. Değişiklikleri kaydedin.
 
![Kullanıcıyı değiştirme](media/data-box-edge-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Kullanıcı silme

Kullanıcı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.


1. Azure portal, Data Box Edge kaynağınız ' ne gidin ve ardından **genel bakış > kullanıcılar**' a gidin.

    ![Silinecek kullanıcıyı seçin](media/data-box-edge-manage-users/delete-user-1.png)

2. Kullanıcılar listesinden bir kullanıcı seçin ve **Sil**' i seçin.  

   ![Sil ' i seçin](media/data-box-edge-manage-users/delete-user-2.png)

3. Sorulduğunda silme işlemini onaylayın. 

   ![Silmeyi onayla](media/data-box-edge-manage-users/delete-user-3.png)

Kullanıcı listesi silinen kullanıcıya göre güncelleştirilir.

![Güncelleştirilmiş kullanıcı listesi](media/data-box-edge-manage-users/delete-user-4.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](data-box-edge-manage-bandwidth-schedules.md) öğrenin.
