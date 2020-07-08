---
title: 'Sorun giderme: indirilen etkinlik günlüklerinde eksik veriler | Microsoft Docs'
description: İndirilen Azure Active Directory etkinlik günlüklerindeki eksik verilere yönelik bir çözüm sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5003d7b644a0c75401a17ed6a37f31acd8180aa9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85608084"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>İndirdiğim Azure Active Directory etkinlik günlüğündeki tüm verileri bulamıyorum

## <a name="symptoms"></a>Belirtiler

Etkinlik günlüklerini (denetim veya oturum açma) indirdim ve seçtiğim süre için tüm kayıtları göremiyorum. Neden mi? 

 ![Raporlama](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Nedeni

Etkinlik günlüklerini Azure portal indirdiğinizde, ölçeği 250.000 kayıt olarak sınırlarız ve en son ilk olarak sıralanır. 

## <a name="resolution"></a>Çözüm

Belirli bir noktadaki bir milyon kaydı getirmek için [Azure AD Raporlama API’lerini](concept-reporting-api.md) kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory raporları SSS](reports-faq.md)

