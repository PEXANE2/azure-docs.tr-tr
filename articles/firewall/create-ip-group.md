---
title: Azure Güvenlik Duvarı'nda IP Grupları Oluşturma
description: IP Grupları, Azure Güvenlik Duvarı kuralları için IP adreslerini gruplandırmanıza ve yönetmenize olanak tanır.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 7e8b2350b9e85d07ce1c399ce4536703ec998cbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444543"
---
# <a name="create-ip-groups-preview"></a>IP Grupları Oluşturma (önizleme)

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IP Grupları, Azure Güvenlik Duvarı kuralları için IP adreslerini gruplandırmanıza ve yönetmenize olanak tanır. Tek bir IP adresine, birden çok IP adresine veya bir veya daha fazla IP adresi aralığına sahip olabilirler.

## <a name="create-an-ip-group"></a>IP Grubu Oluşturma

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. Arama metin kutusuna **IP Grupları** yazın, ardından **IP Grupları'nı**seçin.
3. **Oluştur'u**seçin.
4. Aboneliğinizi seçin.
5. Bir kaynak grubu seçin veya yeni bir tane oluşturun.
6. Sizin için benzersiz bir ad yazın IP Grubu ve ardından bir bölge seçin.

6. **Sonraki'ni seçin: IP adresleri.**
7. Bir IP adresi, birden çok IP adresi veya IP adresi aralığı yazın.

   IP adreslerini girmenin iki yolu vardır:
   - Bunları el ile girebilirsiniz
   - Bunları bir dosyadan içe aktarabilirsiniz

   Bir dosyadan almak için **dosyadan içeri aktar'ı**seçin. Dosyanızı kutuya sürükleyebilir veya **dosyalar için Gözat'ı**seçebilirsiniz. Gerekirse, yüklenen IP adreslerinizi gözden geçirebilir ve güncelleyebilirsiniz.

   Bir IP adresi yazdığınızda, portal örtüşme, yineleme ve biçimlendirme sorunlarını denetlemek için bu adresi doğrular.

5. Tamamlandığında Gözden **Geçir + Oluştur'u**seçin.
6. **Oluştur'u**seçin.


## <a name="next-steps"></a>Sonraki adımlar

- [IP Grupları hakkında daha fazla bilgi edinin](ip-groups.md)