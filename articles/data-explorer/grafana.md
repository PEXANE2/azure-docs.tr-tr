---
title: Grafana'yı kullanarak Azure Veri Gezgini'nden gelen verileri görselleştirin
description: Bu makalede, Azure Veri Gezgini'ni Grafana için bir veri kaynağı olarak ayarlamayı ve ardından örnek kümedeki verileri görselleştirmeyi öğreniyorsunuz.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: a1c52007ea86ca0812c4a73a92ce81db6ddadc7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74037995"
---
# <a name="visualize-data-from-azure-data-explorer-in-grafana"></a>Grafana'daki Azure Veri Gezgini'nden verileri görselleştirin

Grafana, verileri sorgulayıp görselleştirmenizi, ardından görselleştirmelerinize dayalı panolar oluşturmanızı ve paylaşmanızı sağlayan bir analiz platformudur. Grafana, Azure Veri *Gezgini'nden*gelen verilere bağlanmanızı ve görselleştirmenizi sağlayan bir Azure Veri Gezgini eklentisi sağlar. Bu makalede, Azure Veri Gezgini'ni Grafana için bir veri kaynağı olarak ayarlamayı ve ardından örnek kümedeki verileri görselleştirmeyi öğreniyorsunuz.

Grafana'nın Azure Veri Gezgini eklentisini nasıl kullanacağınızı öğrenmek, Azure Veri Gezgini'ni Grafana için veri kaynağı olarak ayarlamak ve ardından verileri görselleştirmek için aşağıdaki videoyu kullanın. 

> [!VIDEO https://www.youtube.com/embed/fSR_qCIFZSA]

Alternatif [olarak, veri kaynağını yapılandırabilir](#configure-the-data-source) ve aşağıdaki makalede ayrıntılı olarak [verileri görselleştirebilirsiniz.](#visualize-data)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdakilere ihtiyacınız var:

* [Grafana sürüm 5.3.0 veya sonraki](https://docs.grafana.org/installation/) işletim sisteminiz için

* Grafana için [Azure Veri Gezgini eklentisi](https://grafana.com/plugins/grafana-azure-data-explorer-datasource/installation)

* StormEvents örnek verilerini içeren bir küme. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure Veri Gezgini kümesi ve veritabanı oluşturun](create-cluster-database-portal.md) ve örnek verileri Azure Veri [Gezgini'ne alın.](ingest-sample-data.md)

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

[!INCLUDE [data-explorer-configure-data-source](../../includes/data-explorer-configure-data-source.md)]

### <a name="specify-properties-and-test-the-connection"></a>Özellikleri belirtin ve bağlantıyı test edin

*Görüntüleyenler* rolüne atanan hizmet ilkesiyle, artık Grafana örneğinizdeki özellikleri belirtin ve Azure Veri Gezgini'ne bağlantıyı sın.

1. Grafana'da, sol menüde, dişli simgesini ve **Veri Kaynaklarını**seçin.

    ![Veri kaynakları](media/grafana/data-sources.png)

1. **Veri kaynağı ekle'yi**seçin.

1. Veri **Kaynakları / Yeni** sayfada, veri kaynağı için bir ad girin ve ardından **Azure Veri Gezgini Veri Kaynağı**türünü seçin.

    ![Bağlantı adı ve türü](media/grafana/connection-name-type.png)

1. Kümenizin adını https://{ClusterName} formuna girin. {Region}.kusto.windows.net adresinden alındı. Azure portalından veya CLI'den diğer değerleri girin. Eşleme için aşağıdaki resmin altındaki tabloya bakın.

    ![Bağlantı özellikleri](media/grafana/connection-properties.png)

    | Grafana UI | Azure portalında | Azure CLI |
    | --- | --- | --- |
    | Abonelik Kimliği | ABONELİK KİMLİĞİ | SubscriptionId |
    | Kiracı Kimliği | Dizin Kimliği | Kiracı |
    | İstemci Kimliği | Uygulama Kimliği | appId |
    | Gizli anahtar | Parola | password |
    | | | |

1. **& Testi Kaydet'i**seçin.

    Test başarılı olursa, bir sonraki bölüme gidin. Herhangi bir sorunla karşılaşırsanız, Grafana'da belirttiğiniz değerleri denetleyin ve önceki adımları gözden geçirin.

## <a name="visualize-data"></a>Verileri görselleştirme

Azure Veri Gezgini'ni Grafana için bir veri kaynağı olarak yapılandırmayı bitirdiniz, verileri görselleştirme zamanı geldi. Burada temel bir örnek göstereceğiz, ama yapabileceğiniz çok şey var. Örnek veri kümesine karşı çalışacak diğer sorguörnekleri [için Azure Veri Gezgini için Yaz sorgularına](write-queries.md) bakmanızı öneririz.

1. Grafana'da, sol menüde artı simgesini ve **Pano'yu**seçin.

    ![Pano oluşturma](media/grafana/create-dashboard.png)

1. **Ekle** sekmesinin altında **Grafik'i**seçin.

    ![Grafik ekleme](media/grafana/add-graph.png)

1. Grafik panelinde Panel **Başlığı'nı** seçin ve **ardından Edit'i**seçin.

    ![Paneli edin](media/grafana/edit-panel.png)

1. Panelin alt kısmında, **Veri Kaynağı'nı** seçin ve ardından yapılandırdığınız veri kaynağını seçin.

    ![Veri kaynağı seçme](media/grafana/select-data-source.png)

1. Sorgu bölmesinde, aşağıdaki sorguda kopyala sonra **Çalıştır'ı**seçin. Sorgu, örnek veri kümesi için olayların sayısını gün sayısına göre kovalar.

    ```kusto
    StormEvents
    | summarize event_count=count() by bin(StartTime, 1d)
    ```

    ![Sorgu çalıştırma](media/grafana/run-query.png)

1. Grafik, varsayılan olarak son altı saatteki verilere göre kapsamlı olduğundan herhangi bir sonuç göstermez. Üst menüde **Son 6 saat'i**seçin.

    ![Son altı saat](media/grafana/last-six-hours.png)

1. StormEvents örnek veri setimize dahil edilen 2007 yılını kapsayan özel bir aralık belirtin. **Uygula**’yı seçin.

    ![Özel tarih aralığı](media/grafana/custom-date-range.png)

    Şimdi grafik 2007 verileri gösterir, gün kovalı.

    ![Bitmiş grafik](media/grafana/finished-graph.png)

1. Üst menüde kaydet simgesini seçin: ![Simgeyi kaydet](media/grafana/save-icon.png).

## <a name="create-alerts"></a>Uyarılar Oluştur

1. Ana Sayfa Panosu'nda, yeni bir bildirim kanalı oluşturmak için **Bildirim Eki'ni** > **Notification channels** seçin

    ![bildirim kanalı oluşturma](media/grafana/create-notification-channel.png)

1. Yeni bir **Bildirim kanalı**oluşturun, ardından **kaydet.**

    ![Yeni bildirim kanalı oluşturma](media/grafana/new-notification-channel-adx.png)

1. **Pano'da**açılan dosyadan **Edit'i** seçin.

    ![panoda edit'i seçin](media/grafana/edit-panel-4-alert.png)

1. **Uyarı** bölmesini açmak için uyarı zili simgesini seçin. **Uyarı Oluştur'u**seçin. **Uyarı** bölmesinde aşağıdaki özellikleri tamamlayın.

    ![uyarı özellikleri](media/grafana/alert-properties.png)

1. Değişikliklerinizi kaydetmek için **panoyu kaydet** simgesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için sorgu yazma](write-queries.md)
