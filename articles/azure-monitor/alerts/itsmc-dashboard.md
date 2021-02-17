---
title: ISMC panosunu kullanarak hataları araştırın
description: Hataları araştırmak için BT Hizmet Yönetimi Bağlayıcısı panosunu nasıl kullanacağınızı öğrenin.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/15/2021
ms.openlocfilehash: 5cbd501b9ccb408ee23cb5c8ed9cde2689ef79fb
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100623960"
---
# <a name="investigate-errors-by-using-the-itsmc-dashboard"></a>ISMC panosunu kullanarak hataları araştırın

Bu makale BT Hizmet Yönetimi Bağlayıcısı (ıSMC) panosu hakkında bilgiler içerir. Pano, bağlayıcının durumunu araştırmanıza yardımcı olur.

## <a name="view-errors"></a>Hataları görüntüle

Panodaki hataları görüntülemek için:

1. **Tüm kaynaklar**' ı seçin ve ardından **ServiceDesk (*çalışma alanınızın adı*)** bulun.

   ![Azure Hizmetlerindeki kaynakları gösteren ekran görüntüsü.](media/itsmc-definition/create-new-connection-from-resource.png)

2. Sol bölmedeki **çalışma alanı veri kaynakları** altında **ITSM bağlantıları**' nı seçin:

   ![Çalışma alanı veri kaynakları altında ıTSM bağlantıları seçmeyi gösteren ekran görüntüsü.](media/itsmc-overview/add-new-itsm-connection.png)

3. **Özet** altında, **BT hizmet yönetimi Bağlayıcısı** alanında, **Özeti görüntüle**' yi seçin:

   ![Görünüm Özeti düğmesini gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/dashboard-view-summary.png)

4. **BT hizmet yönetimi Bağlayıcısı** alanında bir grafik göründüğünde seçin:

   ![Grafiğin seçimini gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/dashboard-graph-click.png)

5. Pano görüntülenir. Bu durumu, bağlayıcıdaki hataları ve hataları gözden geçirmek için kullanın.
   
   ![Panodaki bağlayıcı durumunu gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/connector-dashboard.png)

## <a name="understand-dashboard-elements"></a>Pano öğelerini anlayın

Pano, bu bağlayıcı aracılığıyla ıTSM aracına gönderilen uyarılarla ilgili bilgiler içerir. Pano dört parçaya bölünür.

### <a name="created-work-items"></a>Oluşturulan iş öğeleri 

**Iş ÖĞELERI oluşturulan** alanında, grafik ve altındaki tablo, tür başına iş öğelerinin sayısını içerir. Grafiği veya tabloyu seçerseniz, iş öğeleri hakkında daha fazla ayrıntı görebilirsiniz.

![Oluşturulan bir iş öğesini gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-workitems.png)

### <a name="affected-computers"></a>Etkilenen bilgisayarlar 

**ETKILENEN bilgisayarlar** alanında, tablo bilgisayarları ve bunlarla ilişkili iş öğelerini listeler. Tablolardaki satırları seçerek bilgisayarlar hakkında daha fazla bilgi edinebilirsiniz.

Tablo, sınırlı sayıda satır içerir. Tüm satırları görmek istiyorsanız **Tümünü göster**' i seçin.

![Etkilenen bilgisayarları gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-impacted-comp.png)

### <a name="connector-status"></a>Bağlayıcı durumu 

**BAĞLAYıCı durumu** alanında, grafik ve altındaki tablo, bağlayıcının durumu hakkında iletiler içerir. Tablodaki grafik veya satırları seçerek iletiler hakkında daha fazla bilgi edinebilirsiniz.

Tablo, sınırlı sayıda satır içerir. Tüm satırları görmek istiyorsanız **Tümünü göster**' i seçin.

Tablodaki iletiler hakkında daha fazla bilgi edinmek için [Bu makaleye](itsmc-dashboard-errors.md)bakın.

![Bağlayıcı durumunu gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-connector-status.png)

### <a name="alert-rules"></a>Uyarı kuralları 

**Uyarı kuralları** bölümünde, tablo algılanan uyarı kuralları sayısıyla ilgili bilgiler içerir. Tablodaki satırları seçerek, algılanan kurallar hakkında daha fazla bilgi edinebilirsiniz.
    
Tablo, sınırlı sayıda satır içerir. Tüm satırları görmek istiyorsanız **Tümünü göster**' i seçin.

![Uyarı kurallarını gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-alert-rules.png)
