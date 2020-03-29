---
title: Azure Veri Gezgini küme bağlantı hatalarını giderme
description: Bu makalede, Azure Veri Gezgini'nde bir kümeye bağlanmak için sorun giderme adımlarını açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: c71af799f614e9cd28221d79634666cbc3b2c987
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60827045"
---
# <a name="troubleshoot-failure-to-connect-to-a-cluster-in-azure-data-explorer"></a>Sorun Giderme: Azure Veri Gezgini'nde kümeye bağlanılamaması

Azure Veri Gezgini'nde bir kümeye bağlanamıyorsanız bu adımları izleyin.

1. Bağlantı dizesinin doğru olduğundan emin olun. Bu formda olmalıdır: `https://<ClusterName>.<Region>.kusto.windows.net`, aşağıdaki örnek gibi: `https://docscluster.westus.kusto.windows.net`.

1. Uygun izinlerinizin olduğundan emin olun. Yoksa *yetkisiz* yanıtını alırsınız.

    İzinler hakkında daha fazla bilgi için bkz. [Veritabanı izinlerini yönetme](manage-database-permissions.md). Gerekirse size uygun rolü ekleyebilmesi için küme yöneticinizle çalışın.

1. Kümenin silinmediğini doğrulayın: aboneliğinizde etkinlik günlüğünü gözden geçirin.

1. [Azure hizmet durumu panosunu](https://azure.microsoft.com/status/) denetleyin. Kümeye bağlanmaya çalıştığınız bölgede Azure Veri Gezgini'nin durumuna bakın.

    Durum **İyi** değilse (yeşil onay işareti), durum iyileştikten sonra kümeye bağlanmayı deneyin.

1. Sorununuzu çözmek için hala yardıma ihtiyacınız varsa, lütfen [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)bir destek isteği açın.