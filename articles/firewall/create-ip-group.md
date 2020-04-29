---
title: Azure Güvenlik duvarında IP grupları oluşturma
description: IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini Grupve yönetmenize olanak sağlar.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 7e8b2350b9e85d07ce1c399ce4536703ec998cbc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77444543"
---
# <a name="create-ip-groups-preview"></a>IP grupları oluşturma (Önizleme)

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini Grupve yönetmenize olanak sağlar. Tek bir IP adresi, birden çok IP adresi veya bir veya daha fazla IP adresi aralığı olabilir.

## <a name="create-an-ip-group"></a>IP grubu oluştur

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. Arama metin kutusuna **IP grupları** yazın ve **IP grupları**' nı seçin.
3. **Oluştur**’u seçin.
4. Aboneliğinizi seçin.
5. Bir kaynak grubu seçin veya yeni bir tane oluşturun.
6. IP grubunuz için benzersiz bir ad yazın ve ardından bir bölge seçin.

6. **İleri ' yi seçin: IP adresleri**.
7. Bir IP adresi, birden fazla IP adresi veya IP adresi aralığı yazın.

   IP adreslerini girmenin iki yolu vardır:
   - Bunları el ile girebilirsiniz
   - Bunları bir dosyadan içeri aktarabilirsiniz

   Bir dosyadan içeri aktarmak için **bir dosyadan Içeri aktar**' ı seçin. Dosyanızı kutuya sürükleyebilir veya **dosyalara gözatabiliriz**' ı seçebilirsiniz. Gerekirse, karşıya yüklenen IP adreslerinizi gözden geçirebilir ve düzenleyebilirsiniz.

   Bir IP adresi yazdığınızda, Portal, çakışan, yinelenen ve biçimlendirme sorunlarını kontrol etmek için onu doğrular.

5. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin.
6. **Oluştur**’u seçin.


## <a name="next-steps"></a>Sonraki adımlar

- [IP grupları hakkında daha fazla bilgi](ip-groups.md)