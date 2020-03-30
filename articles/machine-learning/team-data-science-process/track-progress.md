---
title: Ekip Veri Bilimi Süreci projelerinin ilerlemesini izleme
description: Veri bilimi grup yöneticilerinin, ekip yöneticilerinin ve proje yöneticilerinin bir veri bilimi projesinin ilerlemesini nasıl izleyebilirsiniz?
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 8cf1e5a4d97b882e7a8d0c81041bbcde709760d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864205"
---
# <a name="track-the-progress-of-data-science-projects"></a>Veri bilimi projelerinin ilerlemesini izleme

Veri bilimi grup yöneticileri, ekip adayları ve proje müşteri adayları projelerinin ilerlemesini izleyebilir.  Yöneticiler ne işin yapıldığını, kimin yaptığını ve hangi işin kaldığını bilmek isterler.   Beklentileri yönetmek başarının önemli bir unsurudur.

## <a name="azure-devops-dashboards"></a>Azure DevOps panoları

Azure DevOps kullanıyorsanız, belirli bir Çevik projeyle ilişkili etkinlikleri ve iş öğelerini izlemek için panolar oluşturabilirsiniz. Panolar hakkında daha fazla bilgi için [Panolar, raporlar ve widget'lara](/azure/devops/report/dashboards/)bakın.

Azure DevOps'lerde panolar ve widget'lar oluşturma ve özelleştirme hakkında talimatlar için aşağıdaki hızlı başlangıçlara bakın:

- [Pano ekleme ve yönetme](/azure/devops/report/dashboards/dashboards)
- [Panoya widget ekleme](/azure/devops/report/dashboards/add-widget-to-dashboard)

## <a name="example-dashboard"></a>Örnek pano

Aşağıda, ilişkili depolara taahhüt edilenlerin sayısı da dahil olmak üzere Çevik veri bilimi projesinin sprint etkinliklerini izleyen basit bir örnek pano verilmiştir. 

- **Geri sayım döşemesi,** geçerli sprintte kalan gün sayısını gösterir. 

- İki **kod** döşemesi, son yedi gün için iki proje deposundaki taahhüt sayısını gösterir. 

- **TDSP Müşteri Projesi'nin çalışma öğeleri,** tüm iş öğeleri ve durumları için bir sorgunun sonuçlarını gösterir. 

- **Kümülatif akış diyagramı** (CFD) Kapalı ve Etkin iş öğelerinin sayısını gösterir.

- **Burndown grafiği,** sprint'te kalan süreye karşı hala tamamlanması gereken çalışmayı gösterir.

- **Yazma grafiği,** sprintteki toplam çalışma miktarıyla karşılaştırıldığında tamamlanan çalışmayı gösterir.

![Pano](./media/track-progress/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

[Ekip Veri Bilimi İşlemi'ni yürüten walkthroughs,](walkthroughs.md) tüm işlem adımlarını gösteren gözden geçirmeleri listeler. Bağlantılı senaryolar, bulutun ve şirket içi kaynakların akıllı uygulamalara nasıl yönetilenini gösterir. 
