---
title: Öğretici-Azure VMware çözümünün özel bulutunu silme
description: Artık ihtiyacınız olmayan bir Azure VMware çözümü özel bulutunu silmeyi öğrenin.
ms.topic: tutorial
ms.date: 02/09/2021
ms.openlocfilehash: b11b8f902691db4bd71fd3f52aaa67d46efea643
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100101741"
---
# <a name="tutorial-delete-an-azure-vmware-solution-private-cloud"></a>Öğretici: Azure VMware çözümünü özel bulutu silme

Artık ihtiyacınız olmayan bir Azure VMware çözümü özel bulutunuz varsa, bunu silebilirsiniz. Özel bulut yalıtılmış bir ağ etki alanı, adanmış sunucu konaklarında bir veya daha fazla sağlanan vSphere kümesi ve çeşitli sanal makineler (VM 'Ler) içerir. Özel bir bulutu sildiğinizde, tüm VM 'Ler, verileri ve kümeleri silinir. Adanmış konaklar güvenli bir şekilde temizlenir ve ücretsiz havuza döndürülür. Müşteri için sağlanan ağ etki alanı da silinir.  

> [!CAUTION]
> Özel bulutun silinmesi geri alınamaz bir işlemdir. Özel bulut silindikten sonra, tüm çalışan iş yüklerini ve bileşenlerini sonlandırdığı ve genel IP adresleri dahil tüm özel bulut verilerini ve yapılandırma ayarlarını yok etmek için veriler kurtarılamaz.

## <a name="prerequisites"></a>Önkoşullar

VM 'Ler ve verileri daha sonra gerekliyse, özel bulutu silmeden önce verileri yedeklediğinizden emin olun.  VM 'Leri ve bunların verilerini kurtarmanın bir yolu yoktur.


## <a name="delete-the-private-cloud"></a>Özel bulutu silme

1. [Azure Portal](https://portal.azure.com)Azure VMware Çözüm konsoluna erişin.

2. Silinecek özel bulutu seçin.
 
3. Özel bulutun adını girip **Evet**' i seçin. 

>[!NOTE]
>Silme işleminin tamamlanabilmesi birkaç saat sürer.  
