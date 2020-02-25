---
title: Azure Veri Gezgini kümesi oluşturma hatası sorunlarını giderme
description: Bu makalede, Azure Veri Gezgini 'da küme oluşturmaya yönelik sorun giderme adımları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 863d9ecedf095e0ab284a0d7fd86363b69ae5658
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77562420"
---
# <a name="troubleshoot-failed-cluster-creation-of-azure-data-explorer"></a>Sorun giderme: Azure Veri Gezgini küme oluşturma başarısız

Küme oluşturmanın Azure Veri Gezgini başarısız olması olası olmayan bir olayda, bu adımları izleyin.

1. Uygun izinlerinizin olduğundan emin olun. Bir küme oluşturmak için Azure aboneliği için *katkıda* bulunan veya *sahip* rolünün bir üyesi olmanız gerekir. Gerekirse, uygun role ekleyebilmeleri için abonelik yöneticinizle birlikte çalışın.

1. Azure portal **küme oluştur** altında girdiğiniz küme adıyla ilgili doğrulama hatası olmadığından emin olun.

1. [Azure hizmet durumu panosunu](https://azure.microsoft.com/status/) denetleyin. Kümeyi oluşturmaya çalıştığınız bölgedeki Azure Veri Gezgini durumunu arayın.

    Durum **iyi** değilse (yeşil onay işareti), durum artdıktan sonra kümeyi oluşturmayı deneyin.

1. Sorununuzu çözmeye hala yardım gerekiyorsa lütfen [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)bir destek isteği açın.
