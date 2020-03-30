---
title: Hızlı Başlangıç Azure portalı kullanarak bir oturum açma raporunu indirme | Microsoft Docs
description: Azure portalı kullanarak bir oturum açma raporunun nasıl indirileceğini öğrenme
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 9131f208-1f90-4cc1-9c29-085cacd69317
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
ms.openlocfilehash: 86bc72f69903134afa3750ad6b72486a713b6cc0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "68989667"
---
# <a name="quickstart-download-a-sign-in-report-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portalı kullanarak bir oturum açma raporunu indirme

Bu hızlı başlangıçta, son 24 saat boyunca kiracınız için oturum açma verilerinin nasıl indirileceğini öğreneceksiniz. Azure portalından en fazla 250.000 kayıt indirebilirsiniz. Kayıtlar en son olarak sıralanır, bu nedenle varsayılan olarak en son 250.000 kaydı alırsınız. 

## <a name="prerequisites"></a>Ön koşullar

Gerekenler:

* Oturum açma etkinlik raporunu görüntülemek için Premium lisansına sahip Azure Active Directory kiracısı. Azure Active Directory baskınızı yükseltmek için [Azure Active Directory Premium ile başlarken](../fundamentals/active-directory-get-started-premium.md) bakın. Yükseltmeden önce herhangi bir etkinlik veriniz yoksa, bir premium lisansa yükselttikten sonra verilerin raporlarda gösterilmesinin birkaç gün süreceğini unutmayın.
* **Güvenlik Yöneticisi, Güvenlik** **Okuyucu,** **Rapor Okuyucu** veya **Kiracı** için Genel Yönetici rolü olan bir kullanıcı. Ayrıca, kiracıdaki tüm kullanıcılar kendi oturum açma işlemlerine erişebilir.

## <a name="quickstart-download-a-sign-in-report"></a>Hızlı Başlangıç: Oturum açma raporunu indirme

1. [Azure portalına](https://portal.azure.com) gidin.
2. Sol gezinti bölmesinden **Azure Active Directory**’yi seçin ve **Dizini değiştir** düğmesini kullanarak etkin dizininizi seçin.
3. Panodan **Azure Active Directory**’yi ve sonra **Oturum açma işlemleri**’ni seçin. 
4. **Tarih** filtre açılır listesinden **son 24 saat**’i seçin ve sonra **Uygula**’yı seçerek son 24 saatteki oturum açma işlemlerini görüntüleyin. 
5. **İndir** düğmesini seçin, dosya biçimi olarak **CSV'yi** seçin ve filtre uygulanmış kayıtları içeren bir CSV dosyasını indirmek için bir dosya adı belirtin. 

![Raporlama](./media/quickstart-download-sign-in-report/download-sign-ins.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory portaldaki oturum açma etkinlik raporları](concept-sign-ins.md)
* [Azure Active Directory raporlama elde tutma](reference-reports-data-retention.md)
* [Azure Etkin Dizin raporlama gecikmeleri](reference-reports-latencies.md)
