---
title: Avere vFXT için desteği etkinleştirin - Azure
description: Azure için Avere vFXT'den destek yüklemelerini etkinleştirme
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/14/2019
ms.author: rohogue
ms.openlocfilehash: d12bbd1708ceb948aea982f9ed1ab36879e3751c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75415362"
---
# <a name="enable-support-uploads"></a>Destek karşıya yüklemelerini etkinleştirme

Azure için Avere vFXT, kümeniz hakkındaki destek verilerini otomatik olarak yükleyebilir. Bu yüklemeler, destek personelinin mümkün olan en iyi müşteri hizmetini sağlamasına olanak sağlar.

## <a name="steps-to-enable-uploads"></a>Yüklemeleri etkinleştirme adımları

Desteği etkinleştirmek için Avere Kontrol Paneli'nden şu adımları izleyin. (Kontrol panelinin nasıl açılacağını öğrenmek için [vFXT kümesine eriş'i](avere-vfxt-cluster-gui.md) okuyun.)

1. Üstteki **Ayarlar** sekmesine gidin.
1. Soldaki **Destek** bağlantısını tıklayın ve gizlilik ilkesini kabul edin.

   ![Gizlilik politikasını kabul etmek için Onay düğmesiyle Avere Kontrol Paneli ve açılır pencereyi gösteren ekran görüntüsü](media/avere-vfxt-privacy-policy.png)

1. Destek yapılandırma sayfasında, soldaki üçgene tıklayarak **Müşteri Bilgileri** bölümünü açın.
1. Yükleme **bilgilerini yeniden ertele** düğmesini tıklatın.
1. Kümenin destek adını **Benzersiz Küme Adı'nda**ayarlayın. Bu adın, personeli desteklemek için kümenizi benzersiz olarak tanımladığından emin olun.
1. **İstatistik İzleme,** **Genel Bilgi Yükleme**ve **Crash Information Upload**için kutuları işaretleyin.
1. **Gönder'i**tıklatın.

   ![Destek ayarları sayfasının tamamlanmış müşteri bilgileri bölümünü içeren ekran görüntüsü](media/avere-vfxt-support-info.png)

1. Bölümü genişletmek için **Güvenli Proaktif Destek'in (SPS)** solundaki üçgeni tıklatın.
1. **SPS Bağlantısını Etkinleştir**için kutuyu işaretleyin.
1. **Gönder'i**tıklatın.

   ![Destek ayarları sayfasında tamamlanmış Güvenli Proaktif Destek bölümünü içeren ekran görüntüsü](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Sonraki adımlar

Kümeye şirket içi veya varolan bir bulut depolama sistemi eklemeniz gerekiyorsa, [Depolamayı Yapılandır'daki](avere-vfxt-add-storage.md)yönergeleri izleyin.

İstemcileri kümeye iliştirmeye başlamaya hazırsanız, [Avere vFXT kümesini dağı'nı](avere-vfxt-mount-clients.md)okuyun.
