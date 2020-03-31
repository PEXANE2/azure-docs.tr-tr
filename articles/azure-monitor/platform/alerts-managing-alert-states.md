---
title: Uyarı ve akıllı grup durumlarını yönetme
description: Uyarı ve akıllı grup örneklerinin durumlarını yönetme
ms.subservice: alerts
ms.topic: conceptual
author: anantr
ms.date: 09/24/2018
ms.openlocfilehash: a22d7b7b962b36f93ee804c64b3bc6b08ecceb0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667525"
---
# <a name="manage-alert-and-smart-group-states"></a>Uyarı ve akıllı grup durumlarını yönetme

Azure Monitor'daki uyarılar artık bir [uyarı durumuna ve bir monitör koşuluna](https://aka.ms/azure-alerts-overview) sahiptir ve benzer şekilde Akıllı Grupların akıllı grup [durumu](https://aka.ms/smart-groups)vardır. Durum değişiklikleri artık ilgili uyarı veya akıllı grup ile ilişkili geçmişte yakalanır. Bu makalede, hem bir uyarı hem de akıllı bir grup için, durumu değiştirme sürecinde size yol kat edin.

## <a name="change-the-state-of-an-alert"></a>Uyarının durumunu değiştirme

1. Bir uyarının durumunu aşağıdaki farklı şekillerde değiştirebilirsiniz: 
    * Tüm Uyarılar sayfasında, durumunu değiştirmek istediğiniz uyarıların yanındaki onay kutusunu tıklatın ve durum değiştir'i tıklatın.   
    ![İzleme](./media/alerts-managing-alert-states/state-all-alerts.jpg)
    * Belirli bir uyarı örneği için Uyarı Ayrıntıları sayfasında, değişiklik durumunu tıklatabilirsiniz   
    ![İzleme](./media/alerts-managing-alert-states/state-alert-details.jpg)
    * Belirli bir uyarı örneği için Uyarı Ayrıntıları sayfasında, Akıllı Grup bölmesinde istediğiniz uyarıların yanındaki onay kutusunu tıklatabilirsiniz    
    ![İzleme](./media/alerts-managing-alert-states/state-alert-details-sg.jpg)

    * Akıllı Grup Ayrıntıları sayfasında, üye uyarıları listesinde durumunu değiştirmek istediğiniz uyarıların yanındaki onay kutusunu tıklatabilir ve durumu değiştirmek için Durumu Değiştir'i ve Durumu Değiştir'i tıklatabilirsiniz.   
    ![İzleme](./media/alerts-managing-alert-states/state-sg-details-alerts.jpg)
1. Durum Değiştir'i tıklattığınızda, durumu (Yeni/Kabul Edilen/Kapalı) seçmenize ve gerekirse yorum girmenize olanak tanıyan bir açılır pencere açılır.   
![İzleme](./media/alerts-managing-alert-states/state-alert-change.jpg)
1. Bu yapıldıktan sonra, durum değişikliği ilgili uyarının geçmişine kaydedilir. Bu, ilgili Ayrıntılar sayfasını açarak ve geçmiş bölümünü denetleyerek görüntülenebilir.    
![İzleme](./media/alerts-managing-alert-states/state-alert-history.jpg)

## <a name="change-the-state-of-a-smart-group"></a>Akıllı bir grubun durumunu değiştirme
1. Akıllı bir grubun durumunu aşağıdaki farklı şekillerde değiştirebilirsiniz:
    1. Akıllı Grup listesi sayfasında, durumunu değiştirmek istediğiniz akıllı grupların yanındaki onay kutusunu tıklatabilir ve Durumu Değiştir'i tıklatabilirsiniz  
    ![İzleme](./media/alerts-managing-alert-states/state-sg-list.jpg)
    1. Akıllı Grup Ayrıntıları sayfasında, durum değiştir'i tıklatabilirsiniz        
    ![İzleme](./media/alerts-managing-alert-states/state-sg-details.jpg)
1. Durum Değiştir'i tıklattığınızda, durumu (Yeni/Kabul Edilen/Kapalı) seçmenize ve gerekirse yorum girmenize olanak tanıyan bir açılır pencere açılır. 
![İzleme](./media/alerts-managing-alert-states/state-sg-change.jpg)
   > [!NOTE]
   >  Akıllı bir grubun durumunu değiştirmek, tek tek üye uyarılarının durumunu değiştirmez.

1. Bu yapıldıktan sonra, durum değişikliği ilgili akıllı grubun geçmişine kaydedilir. Bu, ilgili Ayrıntılar sayfasını açarak ve geçmiş bölümünü denetleyerek görüntülenebilir.     
![İzleme](./media/alerts-managing-alert-states/state-sg-history.jpg)

