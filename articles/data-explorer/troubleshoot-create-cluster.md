---
title: Azure Veri Gezgini küme oluşturma hatalarını giderme
description: Bu makalede, Azure Veri Gezgini'nde küme oluşturmak için sorun giderme adımlarını açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 863d9ecedf095e0ab284a0d7fd86363b69ae5658
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562420"
---
# <a name="troubleshoot-failed-cluster-creation-of-azure-data-explorer"></a>Sorun Giderme: Azure Veri Gezgini'nin başarısız küme oluşturma

Azure Veri Gezgini'nde küme oluşturmanın başarısız olması olası durumlarda aşağıdaki adımları izleyin.

1. Uygun izinlerinizin olduğundan emin olun. Bir küme oluşturmak için Azure aboneliği için *Katılımcı* veya *Sahip* rolünün bir üyesi olmalısınız. Gerekirse, sizi uygun role ekleyebilmeleri için abonelik yöneticinizle birlikte çalışın.

1. Azure portalında **Oluştur kümesi** altında girdiğiniz küme adı ile ilgili doğrulama hatası olmadığından emin olun.

1. [Azure hizmet durumu panosunu](https://azure.microsoft.com/status/) denetleyin. Küme oluşturmaya çalıştığınız bölgedeki Azure Veri Gezgini'nin durumunu arayın.

    Durum **İyi** değilse (yeşil onay işareti), durum iyileştikten sonra küme oluşturmayı deneyin.

1. Sorununuzu çözmek için hala yardıma ihtiyacınız varsa, lütfen [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)bir destek isteği açın.
