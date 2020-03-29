---
title: include dosyası
description: include dosyası
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 10/15/2019
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: e971af5d415ea788f97ab11b58541e629c7eb0ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76694386"
---
## <a name="lifecycle"></a>Yaşam döngüsü

Yaşam **Döngüsü** sekmesinde, bir kullanıcının erişim paketine atamasının süresinin ne zaman dolduğunu belirtirsiniz. Ayrıca, kullanıcıların atamalarını genişletip genişletemeyeceğini de belirtebilirsiniz.

1. Sona **Erme** bölümünde, Access paketi atamaları ayarlanan **tarih**, **Gün Sayısı**veya **Asla** **sona erer.**

    **Tarih için,** gelecekte bir son kullanma tarihi seçin.

    **Gün sayısı**için, 0 ile 3660 gün arasında bir sayı belirtin.

    Seçiminizi temel alan, bir kullanıcının erişim paketine atamasının süresi belirli bir tarihte, onaylandıktan sonraki belirli bir sayıda veya hiç sona erer.

1. Ek ayarları göstermek için **gelişmiş son kullanma ayarlarını göster'i** tıklatın.

    ![Erişim paketi - Yaşam Döngüsü Sona Erme ayarları](./media/active-directory-entitlement-management-lifecycle-policy/expiration.png)

1. Kullanıcının atamalarını genişletmesine izin vermek için, **kullanıcıların Evet'e erişimi genişletmesine izin ver'i** ayarlayın. **Yes**

    İlkede uzantılara izin verilirse, kullanıcı 14 gün ve ayrıca erişim paketi atamasının süresi dolmadan 1 gün önce bir e-posta alır ve atamayı uzatmalarını sağlar. Kullanıcı erişimi genişletmek için bir istek gönderirse, uzatma tarihi, kullanıcıya erişim paketine erişim izni vermek için kullanılan ilkede tanımlandığı şekilde atamaların süresinin dolması durumunda olmalıdır. Örneğin, ilke atamaların 30 Haziran'da sona erecek şekilde ayarlı olduğunu gösteriyorsa, bir kullanıcının isteyebileceği maksimum uzantı 30 Haziran'dır.

    Bir kullanıcının erişimi genişletilirse, belirtilen uzatma tarihinden (ilkeyi oluşturan kullanıcının saat diliminde ayarlanan tarih) ardından erişim paketini isteyemezler.

1. Bir uzantı vermek için onay gerektirmek için, **Evet**uzantısı **vermek için onay ayarlamayı** ayarlayın.

    **İstekler** sekmesinde belirtilen onay ayarları kullanılır.

1. İleri veya **Güncelleştir'i** tıklatın. **Update**
