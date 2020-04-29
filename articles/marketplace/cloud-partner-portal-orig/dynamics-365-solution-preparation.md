---
title: Dynamics 365 çözüm hazırlığı
description: Paketleme, bileşenleri yükleme ve kaldırma çerçevesi
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: ac1e4fa541e945f20904ced114a36b58d14585ba
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80278595"
---
# <a name="dynamics-365-solution-preparation"></a>Dynamics 365 çözüm hazırlığı

Dynamics 365 solul sistemi, belirli iş işlevlerini sağlayan bileşenleri paketleme, yükleme ve kaldırmaya yönelik bir çerçevedir. Çözümler, ISV 'Ler ve diğer Microsoft Dynamics 365 iş ortakları tarafından oluşturdukları uzantıları dağıtmak için kullanılır.

Mevcut bir Dynamics 365 (xRM) ISV iseniz, büyük olasılıkla zaten yönetilen bir çözüm oluşturmuş ve bir çözüm. zip dosyasına sahip olabilirsiniz. Çözümünüzde, lütfen "görünen ad" ve "Açıklama" alanlarının müşterilerin görmesini istediğiniz şeyi yansıttığından emin olun. Bunlar, CRM Online Yönetim merkezinde görüntülenir.

![CRMScreenShot1](media/CRMScreenShot1.png)

_**Note:** Aşağıdaki paket örneğinde çözüm adının "SampleSolution. zip" olduğunu varsayacağız_

Yeni bir ISV iseniz, burada çözüm oluşturma hakkında daha fazla bilgi edinebilirsiniz:[https://msdn.microsoft.com/library/gg334530.aspx](https://msdn.microsoft.com/library/gg334530.aspx)

Çözümünüz destekleme verileri gerektiriyorsa:

* Test ortamınızda örnek verileri oluşturma
* Verilerinize ilişkin karşılaştırma kurallarına sahip bir şema oluşturmak için yapılandırma geçiş aracını kullanın.
* Yapılandırma şemanızı proje dosyalarınıza kaydedin. Yapılandırma verilerinizi güncelleştirirseniz bu, daha sonra gerekecektir.
* Yapılandırma verilerinizi dışarı aktarın. Dışarı aktarma dosyasına dışarı aktarma için anlamlı olan bir ad vermeyi unutmayın.
