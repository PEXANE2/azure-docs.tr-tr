---
title: Azure Veri Gezgini veri görselleştirme
description: Azure Veri Gezgini verilerinizi görselleştirebileceğiniz farklı yollar hakkında bilgi edinin
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/30/2020
ms.openlocfilehash: 7b1c7825beb769b610d661cb9644fc3f3919d548
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79139071"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Azure Veri Gezgini ile veri görselleştirme 

Azure Veri Gezgini, büyük miktarda veri için karmaşık analiz çözümleri oluşturmak için kullanılan günlük ve telemetri verileri için hızlı ve yüksek ölçeklenebilir bir veri arama hizmetidir. Azure Veri Gezgini çeşitli görselleştirme araçlarıyla tümleşir, böylece verilerinizi görselleştirebilir ve sonuçları kuruluşunuz genelinde paylaşabilirsiniz. Bu veriler, işletmeniz üzerinde bir etki yaratmak için işlem yapılabilir öngörülere dönüştürülebilir.

Veri görselleştirme ve raporlama, veri analizi sürecinde kritik bir adımdır. Azure Veri Gezgini, senaryonuza ve bütçenize en uygun hizmeti kullanabilmeniz için birçok BI hizmeti destekler.

## <a name="kusto-query-language-visualizations"></a>Kusto sorgu dili görselleştirmeleri

Kusto sorgu [`render operator`](/azure/kusto/query/renderoperator) dili, sorgu sonuçlarını göstermek için tablolar, pasta grafikleri ve çubuk grafikler gibi çeşitli görselleştirmeler sunar. Sorgu görselleştirmeleri anomali algılama ve tahmin, makine öğrenimi ve daha fazla yardımcı olur.

## <a name="power-bi"></a>Power BI

Azure Veri Gezgini çeşitli yöntemler kullanarak [Power BI'ye](https://powerbi.microsoft.com) bağlanma olanağı sağlar: 

  * [Dahili yerel Güç BI konektörü](/azure/data-explorer/power-bi-connector)

  * [Azure Veri Gezgini'nden Power BI'ye alma sorgusu](/azure/data-explorer/power-bi-imported-query)
 
  * [SQL sorgusu](/azure/data-explorer/power-bi-sql-query)

## <a name="microsoft-excel"></a>Microsoft Excel

Azure Veri Gezgini, [yerleşik yerel Excel bağlayıcısını](excel-connector.md)kullanarak Microsoft [Excel'e](https://products.office.com/excel) bağlanma veya Azure Veri Gezgini'nden Excel'e [sorgu alma](excel-blank-query.md) olanağı sağlar.

## <a name="grafana"></a>Grafana

[Grafana,](https://grafana.com) Azure Veri Gezgini'nden gelen verileri görselleştirmenizi sağlayan bir Azure Veri Gezgini eklentisi sağlar. [Azure Veri Gezgini'ni Grafana için bir veri kaynağı olarak ayarlar sınız ve verileri görselleştirirsiniz.](/azure/data-explorer/grafana) 

## <a name="kibana"></a>Kibana

Azure Veri Gezgini, açık kaynak bağlayıcısı Olan K2Bridge'i kullanarak [Kibana'ya (Discover sayfası)](https://www.elastic.co/guide/en/kibana/6.8/discover.html) bağlanma olanağı sağlar. [Azure Veri Gezgini'ni Kibana için bir veri kaynağı olarak ayarlar sınız ve verileri görselleştirirsiniz.](/azure/data-explorer/k2bridge)

## <a name="odbc-connector"></a>ODBC bağlayıcısı

Azure Veri Gezgini, ODBC'yi destekleyen herhangi bir uygulamanın Azure Veri Gezgini'ne bağlanabilmesi için [Açık Veritabanı Bağlantısı (ODBC) bağlayıcısı](connect-odbc.md) sağlar.

## <a name="tableau"></a>Tableau

Azure Veri Gezgini, [ODBC bağlayıcısını](/azure/data-explorer/connect-odbc) kullanarak [Tableau'ya](https://www.tableau.com) bağlanma ve [ardından Tableau'daki verileri görselleştirme](tableau.md)olanağı sağlar.

## <a name="qlik"></a>Qlik

Azure Veri Gezgini, [ODBC bağlayıcısını](/azure/data-explorer/connect-odbc) kullanarak [Qlik'e](https://www.qlik.com) bağlanma ve ardından Qlik Sense panoları oluşturma ve verileri görselleştirme olanağı sağlar. Aşağıdaki videoyu kullanarak Azure Veri Gezgini verilerini Qlik ile görselleştirmeyi öğrenebilirsiniz. 

> [!VIDEO https://www.youtube.com/embed/nhWIiBwxjjU]  

## <a name="sisense"></a>Sisense

Azure Veri Gezgini, JDBC konektörünü kullanarak [Sisense'e](https://www.sisense.com) bağlanma olanağı sağlar. [Azure Veri Gezgini'ni Sisense için bir veri kaynağı olarak ayarlar sınız ve ardından verileri görselleştirirsiniz.](/azure/data-explorer/sisense)

## <a name="redash"></a>Redash

Panolar oluşturmak ve verileri görselleştirmek için [Redash'i](https://redash.io/) kullanabilirsiniz. [Azure Veri Gezgini'ni Redash için bir veri kaynağı olarak ayarlayın ve ardından verileri görselleştirin.](/azure/data-explorer/redash)