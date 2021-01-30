---
title: Panoyu kullanarak hataları araştırın
description: Bu belge, ITSMC panosundaki hatalar hakkında bilgi içerir.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/15/2021
ms.openlocfilehash: ebd59e637e498b8088fe9a302b1bb12efdf2c173
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99089284"
---
# <a name="investigate-errors-by-using-the-itsmc-dashboard"></a>ISMC panosunu kullanarak hataları araştırın

Bu makale BT Hizmet Yönetimi Bağlayıcısı (ıSMC) panosu hakkında bilgiler içerir. Pano, ıSMC 'ın durumunu araştırmanıza yardımcı olur.

## <a name="view-the-dashboard"></a>Panoyu görüntüleme

Panoyu açmak için aşağıdaki adımları izleyin.

1. **Tüm kaynaklar**' ı seçin ve ardından **ServiceDesk (*çalışma alanınızın adı*)** bulun.

   ![Azure Hizmetlerindeki kaynakları gösteren ekran görüntüsü.](media/itsmc-definition/create-new-connection-from-resource.png)

1. Sol bölmede, **çalışma alanı veri kaynakları**' nı seçin ve **ITSM bağlantıları**' nı seçin.

   ![Çalışma alanı veri kaynakları altında ıTSM bağlantıları seçmeyi gösteren ekran görüntüsü.](media/itsmc-overview/add-new-itsm-connection.png)

1. **Özet bölümünde Özet** grafiğini görüntülemek Için özeti **görüntüle** ' yi seçin.

    ![Özet bölümündeki Özet Görünüm seçeneğini gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/dashboard-view-summary.png)

1. Panoyu açmak için **Özet** bölümündeki grafiği seçin.

    ![Özet grafiğinin seçilip seçildiğinin gösterildiği ekran görüntüsü.](media/itsmc-resync-servicenow/dashboard-graph-click.png)

1. Panodaki durum ve hatalar için panoyu gözden geçirin.
    ![Panoyu gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/connector-dashboard.png)

## <a name="understand-dashboard-elements"></a>Pano öğelerini anlayın

Pano, bu bağlayıcı kullanılarak ıTSM aracına gönderilen uyarılarla ilgili bilgiler içerir.

Pano dört bölüme ayrılır:

- **Oluşturulan Iş öğeleri**: grafik ve tablo, türe göre iş öğelerinin sayısını gösterir. Çalışma öğeleriniz hakkında daha fazla bilgi edinmek için grafiği veya tabloyu seçin.
      ![İş öğeleri oluşturma bölümünü gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-workitems.png)
- **ETKILENEN bilgisayarlar**: tablo, iş öğelerini oluşturan yapılandırma öğeleriyle ilgili ayrıntıları içerir.
      Yapılandırma öğeleri hakkında daha fazla ayrıntı için tablolardaki satırları seçin.
      Tablo, sınırlı sayıda satır içerir. Listenin tamamını görmek için **Tümünü görüntüle**' yi seçin.
      ![Etkilenen bilgisayarlar bölümünü gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-impacted-comp.png)
- **BAĞLAYıCı durumu**: grafik ve tablo, bağlayıcının durumu hakkında bilgi gösterir. Daha fazla ayrıntı için grafik veya tablodaki iletileri seçin. Tabloda sınırlı sayıda satır gösterilmektedir. Listenin tamamını görmek için **Tümünü görüntüle**' yi seçin.
      ![Bağlayıcı durumu bölümünü gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-connector-status.png)
- **Uyarı kuralları**: Bu bölümde algılanan uyarı kuralları sayısıyla ilgili bilgiler gösterilmektedir. Algılanan kurallar hakkında daha fazla ayrıntı için tablolardaki satırları seçin. Tabloda sınırlı sayıda satır vardır. Listenin tamamını görmek için **Tümünü görüntüle**' yi seçin.
      ![Uyarı kuralları bölümünü gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-alert-rules.png)

## <a name="next-steps"></a>Sonraki adımlar

[Ortak bağlayıcı durum hatalarını](itsmc-dashboard-errors.md)inceleyin.
