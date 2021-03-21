---
title: Uyarı ve akıllı grup durumlarını yönetme
description: Uyarı ve akıllı Grup örneklerinin durumlarını yönetme
ms.topic: conceptual
author: anantr
ms.date: 09/24/2018
ms.openlocfilehash: cac926c3c56c7f58e5237b820e0328999ffe2c52
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102033580"
---
# <a name="manage-alert-and-smart-group-states"></a>Uyarı ve akıllı grup durumlarını yönetme

Azure Izleyici 'deki uyarılar artık bir [uyarı durumuna ve bir izleyici koşuluna](./alerts-overview.md) sahiptir ve benzer şekilde akıllı gruplar [akıllı grup durumuna](./alerts-smartgroups-overview.md?toc=%2fazure%2fazure-monitor%2ftoc.json)sahiptir. Durum üzerinde yapılan değişiklikler artık ilgili uyarıyla veya akıllı grupla ilişkili geçmiş olarak yakalanır. Bu makalede, hem uyarı hem de akıllı grup için durumu değiştirme sürecinde size yol gösterilir.

## <a name="change-the-state-of-an-alert"></a>Bir uyarının durumunu değiştirme

1. Bir uyarının durumunu aşağıdaki farklı yollarla değiştirebilirsiniz: 
    * Tüm Uyarılar sayfasında, durumunu değiştirmek istediğiniz uyarıların yanındaki onay kutusuna tıklayın ve durumu Değiştir ' e tıklayın.   
    ![Ekran görüntüsü, değişiklik durumu seçiliyken tüm uyarılar sayfasını gösterir.](./media/alerts-managing-alert-states/state-all-alerts.jpg)
    * Belirli bir uyarı örneği için Uyarı ayrıntıları sayfasında, durumu Değiştir ' e tıklayabilirsiniz.   
    ![Ekran görüntüsü, değişiklik uyarı durumu seçiliyken Uyarı ayrıntıları sayfasını gösterir.](./media/alerts-managing-alert-states/state-alert-details.jpg)
    * Belirli bir uyarı örneğinin Uyarı ayrıntıları sayfasında, akıllı grup bölmesinde istediğiniz uyarıların yanındaki onay kutusuna tıklayabilirsiniz    
    ![Ekran görüntüsü, sinyal uyarısı için onay işaretleri bulunan bazı örneklerle Uyarı ayrıntıları sayfasını gösterir.](./media/alerts-managing-alert-states/state-alert-details-sg.jpg)

    * Akıllı Grup ayrıntıları sayfasında, üye uyarıları listesinde, durumunu değiştirmek istediğiniz uyarıların yanındaki onay kutusuna tıklayabilir ve durumu Değiştir ' e tıklayarak durumu Değiştir ' e tıklayabilirsiniz.   
    ![Ekran görüntüsü, durumu değiştirecek uyarıları seçebileceğiniz akıllı Grup ayrıntıları sayfasını gösterir.](./media/alerts-managing-alert-states/state-sg-details-alerts.jpg)
1. Değişiklik durumunu tıklarken, bir açılan pencere açılır ve durumu seçmenizi sağlar (yeni/onaylanmış/kapalı) ve gerekirse bir yorum girersiniz.   
![Ekran görüntüsü Ayrıntılar değişiklik Uyarısı iletişim kutusunu gösterir.](./media/alerts-managing-alert-states/state-alert-change.jpg)
1. Bu işlem tamamlandıktan sonra durum değişikliği ilgili uyarının geçmişine kaydedilir. Bu, ilgili ayrıntılar sayfası açılarak ve geçmiş bölümü denetlenirken görüntülenebilir.    
![Ekran görüntüsü durum değişikliklerinin geçmişini gösterir.](./media/alerts-managing-alert-states/state-alert-history.jpg)

## <a name="change-the-state-of-a-smart-group"></a>Bir akıllı grubun durumunu değiştirme
1. Akıllı bir grubun durumunu aşağıdaki farklı yollarla değiştirebilirsiniz:
    1. Akıllı Grup listesi sayfasında, durumunu değiştirmek istediğiniz akıllı grupların yanındaki onay kutusuna tıklayabilir ve durumu Değiştir ' e tıklayabilirsiniz.  
    ![Ekran görüntüsü, akıllı grupların durum değiştirme sayfasını gösterir.](./media/alerts-managing-alert-states/state-sg-list.jpg)
    1. Akıllı Grup ayrıntıları sayfasında, durumu Değiştir ' e tıklayabilirsiniz.        
    ![Ekran görüntüsü, akıllı grup durumunu değiştir seçili olan akıllı Grup ayrıntıları sayfasını gösterir.](./media/alerts-managing-alert-states/state-sg-details.jpg)
1. Değişiklik durumunu tıklarken, bir açılan pencere açılır ve durumu seçmenizi sağlar (yeni/onaylanmış/kapalı) ve gerekirse bir yorum girersiniz. 
![Ekran görüntüsü, akıllı grup için değişiklik durumu iletişim kutusunu gösterir.](./media/alerts-managing-alert-states/state-sg-change.jpg)
   > [!NOTE]
   >  Bir akıllı grubun durumunun değiştirilmesi, bağımsız üye uyarılarının durumunu değiştirmez.

1. Bu işlem tamamlandıktan sonra, durum değişikliği ilgili akıllı grubun geçmişine kaydedilir. Bu, ilgili ayrıntılar sayfası açılarak ve geçmiş bölümü denetlenirken görüntülenebilir.     
![Ekran görüntüsü, akıllı grup için değişiklik geçmişini gösterir.](./media/alerts-managing-alert-states/state-sg-history.jpg)