---
title: Dynamics 365 Çözüm Hazırlama
description: Bileşenlerin paketlenmesi, yüklenmesi ve boşaltılamı için çerçeve
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: ac1e4fa541e945f20904ced114a36b58d14585ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278595"
---
# <a name="dynamics-365-solution-preparation"></a>Dynamics 365 Çözüm Hazırlama

Dynamics 365 çözümleme sistemi, belirli iş işlevselliği sağlayan bileşenlerin paketlenmesi, yüklenmesi ve yüklenmesi için bir çerçevedir. Çözümler, ISV'ler ve diğer Microsoft Dynamics 365 iş ortakları tarafından oluşturdukları uzantıları dağıtmak için kullanılır.

Varolan bir Dynamics 365 (xRM) ISV iseniz, büyük olasılıkla zaten yönetilen bir çözüm oluşturdunuz ve bir solution.zip dosyası var. Çözümünüzde, lütfen "Görüntü Adı" ve "Açıklama" alanlarının müşterilerin görmesini istediğiniz ibareleri yansıttığından emin olun. Bunlar CRM Çevrimiçi Yönetim Merkezi'nde görüntülenir.

![CRMScreenShot1](media/CRMScreenShot1.png)

_**Not:** Aşağıdaki paket örneğinde çözüm adının "SampleSolution.zip" olduğunu varsayacağız._

Yeni bir ISV'yseniz, burada bir çözüm oluşturma hakkında daha fazla bilgi edinebilirsiniz:[https://msdn.microsoft.com/library/gg334530.aspx](https://msdn.microsoft.com/library/gg334530.aspx)

Çözümünüz veri desteği gerektiriyorsa:

* Test ortamınızda örnek verileri oluşturma
* Verileriniz için karşılaştırma kuralları içeren bir Şema oluşturmak için Yapılandırma Geçiş Aracı'nı kullanın.
* Yapılandırma şemanızı proje dosyalarınızla kaydedin. Yapılandırma verilerinizi güncellerseniz, daha sonra buna ihtiyacınız olacaktır.
* Yapılandırma verilerinizi dışa aktarın. Dışa aktarma dosyasına, dışa aktarmanız için anlamlı bir ad verdiğinizden emin olun.
