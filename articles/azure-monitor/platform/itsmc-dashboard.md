---
title: Pano kullanarak hata araştırma
description: Bu belge Panoyu kullanarak hata araştırma hakkında bilgiler içerir
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/15/2021
ms.openlocfilehash: 9a43318ed935dd1e71b6da75c9f078fbebbd29eb
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98599012"
---
# <a name="error-investigation-using-the-dashboard"></a>Panoyu kullanarak araştırma hatası

Bu sayfa ıTSM bağlayıcı panosu hakkında bilgiler içerir. Bu Pano, ıTSM bağlayıcısının durumunu araştırmanıza yardımcı olur.

## <a name="how-to-view-the-dashboard"></a>Panoyu görüntüleme

Panodaki hataları görüntülemek için sonraki adımları izlemeniz gerekir:

1. **Tüm kaynaklarda**, **ServiceDesk 'yi arayın (*çalışma alanınızın adı*)**:

   ![Azure portal son kaynakları gösteren ekran görüntüsü.](media/itsmc-definition/create-new-connection-from-resource.png)

2. Sol bölmedeki **çalışma alanı veri kaynakları** altında **ITSM bağlantıları**' nı seçin:

   ![ITSM bağlantıları menü öğesini gösteren ekran görüntüsü.](media/itsmc-overview/add-new-itsm-connection.png)

3. Sol taraftaki kutuda **BT hizmet yönetimi Bağlayıcısı** **Özet altında,** **Özeti görüntüle**' yi seçin:

    ![Görünüm özetini gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/dashboard-view-summary.png)

4. Sol taraftaki kutudaki **BT hizmet yönetimi Bağlayıcısı** **Özet** altında, grafiğe tıklayın:

    ![Graph tıklamesinin gösterildiği ekran görüntüsü.](media/itsmc-resync-servicenow/dashboard-graph-click.png)

5. Bu Panoyu kullanarak, bağlayıcıdaki durum ve hataları inceleyebilirsiniz.
    ![Bağlayıcı durumunu gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/connector-dashboard.png)

## <a name="dashboard-elements"></a>Pano öğeleri

Pano, bu bağlayıcıyı kullanarak ıTSM aracına gönderilen uyarılarla ilgili bilgiler içerir.
Pano dört parçaya ayrılır:

1. Oluşturulan iş öğesi: grafik ve aşağıdaki tablo, tür başına iş öğesi sayısını içerir. Grafiğe veya tabloya tıkladığınızda, iş öğeleri hakkında daha fazla ayrıntı görebilirsiniz.
    ![Oluşturulan iş öğesini gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-workitems.png)
2. Etkilenen bilgisayarlar: tablolar, yapılandırma öğelerini oluşturan yapılandırma öğeleriyle ilgili ayrıntıları içerir.
    Tablolardaki satırlara tıklayarak yapılandırma öğeleri hakkında daha fazla bilgi edinebilirsiniz.
    Tablo sınırlı sayıda satır içerir. tüm listeyi görmek isterseniz "tümünü gör" düğmesine tıklayabilirsiniz.
    ![Etkilenen bilgisayarları gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-impacted-comp.png)
3. Bağlayıcı durumu: aşağıdaki grafik ve tablo, bağlayıcının durumu hakkındaki iletileri içerir. Tablodaki satırlardaki grafiğe tıklayarak bağlayıcı durumunun iletileri hakkında daha fazla bilgi edinebilirsiniz.
    Tablo sınırlı sayıda satır içerir. tüm listeyi görmek isterseniz "tümünü gör" düğmesine tıklayabilirsiniz.

    Tablodaki iletilerle ilgili ayrıntıları [burada](itsmc-dashboard-errors.md)görebilirsiniz.

    ![Bağlayıcı durumunu gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-connector-status.png)
4. Uyarı kuralları: tablolar algılanan uyarı kuralları sayısı hakkındaki bilgileri içerir.
    Tablolardaki satırlara tıklayarak, algılanan kurallar hakkında daha fazla ayrıntı edinebilirsiniz.
    Tablo sınırlı sayıda satır içerir. tüm listeyi görmek isterseniz "tümünü gör" düğmesine tıklayabilirsiniz.
    ![Uyarı kurallarını gösteren ekran görüntüsü.](media/itsmc-resync-servicenow/itsm-dashboard-alert-rules.png)