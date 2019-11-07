---
title: Grafana kullanarak Azure Veri Gezgini verileri görselleştirme
description: Bu nasıl yapılır, Grafana için bir veri kaynağı olarak Azure Veri Gezgini ayarlamayı ve sonra bir örnek kümeden verileri görselleştirmeyi öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 6/30/2019
ms.openlocfilehash: f1eb9fb0d81d1e9cdf3dd8628a6d7ad1f0ccce92
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73581909"
---
# <a name="visualize-data-from-azure-data-explorer-in-grafana"></a>Grafana 'de Azure Veri Gezgini verileri görselleştirme

Grafana, verileri sorgulamanıza ve görselleştirmenize, sonra görselleştirmelerinizle ilgili panoları oluşturmanıza ve paylaşmanıza olanak tanıyan bir analiz platformudur. Grafana, Azure Veri Gezgini 'a bağlanmanızı ve verileri görselleştirmenizi sağlayan bir Azure Veri Gezgini *eklentisi*sağlar. Bu makalede, Grafana için bir veri kaynağı olarak Azure Veri Gezgini ayarlamayı ve sonra bir örnek kümeden verileri görselleştirmeyi öğreneceksiniz.

Aşağıdaki videoyu kullanarak Grafana 'in Azure Veri Gezgini eklentisini kullanmayı, Azure Veri Gezgini Grafana için bir veri kaynağı olarak ayarlamayı ve sonra verileri görselleştirmeyi öğrenebilirsiniz. 

> [!VIDEO https://www.youtube.com/embed/fSR_qCIFZSA]

Alternatif olarak, aşağıdaki makalede açıklandığı gibi [veri kaynağını yapılandırabilir](#configure-the-data-source) ve [verileri görselleştirebilirsiniz](#visualize-data) .

## <a name="prerequisites"></a>Ön koşullar

Şunları yapmak için şunlar gerekir:

* İşletim sisteminiz için [Grafana sürüm 5.3.0 veya üzeri](https://docs.grafana.org/installation/)

* Grafana için [Azure Veri Gezgini eklentisi](https://grafana.com/plugins/grafana-azure-data-explorer-datasource/installation)

* StormEvents örnek verilerini içeren bir küme. Daha fazla bilgi için bkz. [hızlı başlangıç: azure Veri Gezgini kümesi ve veritabanı oluşturma](create-cluster-database-portal.md) ve [örnek verileri Azure Veri Gezgini](ingest-sample-data.md)ile alma.

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

[!INCLUDE [data-explorer-configure-data-source](../../includes/data-explorer-configure-data-source.md)]

### <a name="specify-properties-and-test-the-connection"></a>Özellikleri belirtin ve bağlantıyı test edin

*Görüntüleyiciler* rolüne atanan hizmet sorumlusu sayesinde artık Grafana örneğinizdeki özellikleri belirtirsiniz ve Azure Veri Gezgini bağlantısını test edersiniz.

1. Grafana ' de, sol taraftaki menüden dişli simgesini ve ardından **veri kaynaklarını**seçin.

    ![Veri kaynakları](media/grafana/data-sources.png)

1. **Veri kaynağı Ekle**' yi seçin.

1. **Veri kaynakları/yeni** sayfasında, veri kaynağı için bir ad girin ve **Azure Veri Gezgini veri kaynağı**türünü seçin.

    ![Bağlantı adı ve türü](media/grafana/connection-name-type.png)

1. Kümenizin adını https://{ClusterName} biçiminde girin. {Region}. kusto. Windows. net. Azure portal veya CLı 'den diğer değerleri girin. Eşleme için aşağıdaki görüntünün altındaki tabloya bakın.

    ![Bağlantı özellikleri](media/grafana/connection-properties.png)

    | Grafana Kullanıcı arabirimi | Azure portal | Azure CLI |
    | --- | --- | --- |
    | Abonelik kimliği | ABONELIK KIMLIĞI | SubscriptionId |
    | Kiracı kimliği | Dizin KIMLIĞI | Kiracı |
    | İstemci kimliği | Uygulama Kimliği | appId |
    | Gizli anahtar | Parola | password |
    | | | |

1. **& testini kaydet**' i seçin.

    Test başarılı olursa, sonraki bölüme gidin. Herhangi bir sorunla karşılaşırsanız, Grafana içinde belirttiğiniz değerleri kontrol edin ve önceki adımları gözden geçirin.

## <a name="visualize-data"></a>Verileri görselleştirme

Azure Veri Gezgini 'yi Grafana için bir veri kaynağı olarak yapılandırmayı tamamladınız, verileri görselleştirmeye zaman atalım. Burada temel bir örnek göstereceğiz, ancak yapabileceğiniz çok daha fazla şey var. Örnek veri kümesinde çalıştırılacak diğer sorguların örnekleri için [Azure Veri Gezgini yazma sorguları](write-queries.md) yapmanızı öneririz.

1. Grafana ' de, sol taraftaki menüden artı simgesini ve ardından **panoyu**seçin.

    ![Pano Oluştur](media/grafana/create-dashboard.png)

1. **Ekle** sekmesi altında **grafik**' i seçin.

    ![Grafik Ekle](media/grafana/add-graph.png)

1. Grafik panelinde **panel başlığı** ' nı ve ardından **Düzenle**' yi seçin.

    ![Düzenleme bölmesi](media/grafana/edit-panel.png)

1. Panelin alt kısmındaki **veri kaynağı** ' nı seçin ve ardından yapılandırdığınız veri kaynağını seçin.

    ![Veri kaynağı seçme](media/grafana/select-data-source.png)

1. Sorgu bölmesinde aşağıdaki sorguyu kopyalayın ve **Çalıştır**' ı seçin. Sorgu, örnek veri kümesi için güne göre olay sayısını demetler.

    ```kusto
    StormEvents
    | summarize event_count=count() by bin(StartTime, 1d)
    ```

    ![Sorgu çalıştırma](media/grafana/run-query.png)

1. Grafik, en son altı saatin verileri için varsayılan olarak kapsam yaptığı için herhangi bir sonuç göstermez. Üstteki menüde **son 6 saat**' i seçin.

    ![Son altı saat](media/grafana/last-six-hours.png)

1. StormEvents örnek veri kümesine dahil edilen yılı 2007 içeren bir özel Aralık belirtin. **Uygula**’yı seçin.

    ![Özel tarih aralığı](media/grafana/custom-date-range.png)

    Artık grafik, günlük 2007 ' deki verileri gösterir.

    ![Tamamlanmış grafik](media/grafana/finished-graph.png)

1. Üstteki menüden Kaydet simgesini seçin: ![Kaydet simgesi](media/grafana/save-icon.png).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için sorgu yazma](write-queries.md)

* [Öğretici: Power BI içindeki Azure Veri Gezgini verileri görselleştirme](visualize-power-bi.md)
