---
title: Hızlı Başlangıç Azure portalı kullanarak denetim raporu indirme | Microsoft Docs
description: Azure portalı kullanarak bir denetim raporunun nasıl indirileceğini öğrenme
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6cbea49fe39c92c8a2fc50e501cb4ef5cff74b1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "68989686"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalı kullanarak denetim raporu indirme

Bu hızlı başlangıçta, son 24 saat içinde kiracınız için denetim günlüklerinin CSV dosyasını nasıl indirdiğinizi öğrenirsiniz. Azure portalından en fazla 250.000 kayıt indirebilirsiniz. Kayıtlar en son olarak sıralanır, bu nedenle varsayılan olarak en son 250.000 kaydı alırsınız. 

## <a name="prerequisites"></a>Ön koşullar

Gerekenler:

* Bir Azure Active Directory kiracısı. 
* **Güvenlik Yöneticisi, Güvenlik** **Okuyucu**veya Kiracı için **Global Administrator** rolünde olan bir kullanıcı. Ayrıca, kiracıdaki tüm kullanıcılar kendi denetim günlüklerine erişebilir.

## <a name="quickstart-download-an-audit-report"></a>Hızlı Başlangıç: Denetim raporunu indirme

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sol gezinti bölmesinden **Azure Active Directory**’yi seçin ve **Dizini değiştir** düğmesini kullanarak etkin dizininizi seçin.
3. Panodan **Azure Active Directory**’yi ve sonra **Denetim günlükleri**’ni seçin. 
4. **Tarih aralığı** filtre açılır listesinden **son 24 saat**’i seçin ve sonra **Uygula**’yı seçerek son 24 saatteki denetim günlüklerini görüntüleyin. 
5. **İndir** düğmesini seçin, dosya biçimi olarak **CSV'yi** seçin ve filtre uygulanmış kayıtları içeren bir CSV dosyasını indirmek için bir dosya adı belirtin. 

![Raporlama](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory portaldaki oturum açma etkinlik raporları](concept-sign-ins.md)
* [Azure Active Directory raporlama elde tutma](reference-reports-data-retention.md)
* [Azure Etkin Dizin raporlama gecikmeleri](reference-reports-latencies.md)
