---
title: Azure Güvenlik duvarında IP grupları oluşturma
description: IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini Grupve yönetmenize olanak sağlar.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/23/2020
ms.author: victorh
ms.openlocfilehash: c3ae62bf5b4f0b4796cac2e7079c8a09116d4895
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85602542"
---
# <a name="create-ip-groups"></a>IP Grupları oluşturma

IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini Grupve yönetmenize olanak sağlar. Tek bir IP adresi, birden çok IP adresi veya bir veya daha fazla IP adresi aralığı olabilir.

## <a name="create-an-ip-group"></a>IP grubu oluştur

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. Arama metin kutusuna **IP grupları** yazın ve **IP grupları**' nı seçin.
3. **Oluştur**'u seçin.
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
6. **Oluştur**'u seçin.


## <a name="next-steps"></a>Sonraki adımlar

- [IP grupları hakkında daha fazla bilgi](ip-groups.md)