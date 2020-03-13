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
ms.sourcegitcommit: d322d0a9d9479dbd473eae239c43707ac2c77a77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79139071"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Azure Veri Gezgini ile veri görselleştirme 

Azure Veri Gezgini, büyük miktarlarda veri için karmaşık analiz çözümleri oluşturmak üzere kullanılan günlük ve telemetri verilerine yönelik hızlı ve yüksek düzeyde ölçeklenebilir bir veri araştırma hizmetidir. Azure Veri Gezgini, çeşitli görselleştirme araçlarıyla tümleşir, böylece verilerinizi görselleştirebilir ve sonuçları kuruluşunuz genelinde paylaşabilirsiniz. Bu veriler, işletmenize bir etkisi olması için eyleme dönüştürülebilir içgörüler halinde dönüştürülebilir.

Veri görselleştirme ve raporlama, veri analizi işleminde önemli bir adımdır. Azure Veri Gezgini, senaryonuza ve bütçenize en uygun olanı kullanabilmeniz için birçok bı hizmetini destekler.

## <a name="kusto-query-language-visualizations"></a>Kusto sorgu dili görselleştirmeleri

Kusto sorgu dili [`render operator`](/azure/kusto/query/renderoperator) sorgu sonuçlarını belirleyen tablolar, pasta grafikleri ve çubuk grafikler gibi çeşitli görselleştirmeler sunar. Sorgu Görselleştirmeleri, anomali algılama ve tahmin, makine öğrenimi ve daha birçok konuda yardımcı olur.

## <a name="power-bi"></a>Power BI

Azure Veri Gezgini, çeşitli yöntemler kullanarak [Power BI](https://powerbi.microsoft.com) bağlanma özelliği sağlar: 

  * [Yerleşik yerel Power BI Bağlayıcısı](/azure/data-explorer/power-bi-connector)

  * [Azure Veri Gezgini 'de sorgu içeri aktarma Power BI](/azure/data-explorer/power-bi-imported-query)
 
  * [SQL sorgusu](/azure/data-explorer/power-bi-sql-query)

## <a name="microsoft-excel"></a>Microsoft Excel

Azure Veri Gezgini, [yerleşik yerel Excel bağlayıcısını](excel-connector.md)kullanarak [Microsoft Excel](https://products.office.com/excel) 'e bağlanma yeteneği sağlar veya Azure Veri Gezgini 'den Excel 'e [bir sorgu içeri aktarabilir](excel-blank-query.md) .

## <a name="grafana"></a>Grafana

[Grafana](https://grafana.com) , Azure Veri Gezgini verileri görselleştirmenizi sağlayan bir Azure Veri Gezgini eklentisi sağlar. [Azure Veri Gezgini 'Yi Grafana için bir veri kaynağı olarak ayarlayın ve ardından verileri görselleştirin](/azure/data-explorer/grafana). 

## <a name="kibana"></a>Kibana

Azure Veri Gezgini, açık kaynak bağlayıcı olan K2Bridge kullanarak [kibana (bul sayfasına)](https://www.elastic.co/guide/en/kibana/6.8/discover.html) bağlanma yeteneği sağlar. [Azure Veri Gezgini 'Yi kibana için bir veri kaynağı olarak ayarlayın ve ardından verileri görselleştirin](/azure/data-explorer/k2bridge).

## <a name="odbc-connector"></a>ODBC bağlayıcısı

Azure Veri Gezgini, [açık bir veritabanı bağlantısı (ODBC) Bağlayıcısı](connect-odbc.md) sağlar, böylece ODBC 'yi destekleyen herhangi bir uygulama Azure Veri Gezgini bağlanabilir.

## <a name="tableau"></a>Tableau

Azure Veri Gezgini, [ODBC bağlayıcısını](/azure/data-explorer/connect-odbc) kullanarak [Tableau](https://www.tableau.com) 'e bağlanma yeteneği sağlar ve ardından [Tableau içindeki verileri görselleştirin](tableau.md).

## <a name="qlik"></a>Qlik

Azure Veri Gezgini, [ODBC bağlayıcısını](/azure/data-explorer/connect-odbc) kullanarak [Qlik](https://www.qlik.com) 'e bağlanma yeteneği sağlar ve ardından Qlik Sense panoları oluşturup verileri görselleştirin. Aşağıdaki videoyu kullanarak Azure Veri Gezgini verilerini Qlik ile görselleştirmeyi öğrenebilirsiniz. 

> [!VIDEO https://www.youtube.com/embed/nhWIiBwxjjU]  

## <a name="sisense"></a>Sisense

Azure Veri Gezgini, JDBC bağlayıcısını kullanarak [Sisense](https://www.sisense.com) 'e bağlanma yeteneği sağlar. [Azure Veri Gezgini 'Yi Sisense için bir veri kaynağı olarak ayarlayın ve ardından verileri görselleştirin](/azure/data-explorer/sisense).

## <a name="redash"></a>Redash

Panoları derlemek ve verileri görselleştirmek için [yeniden Dash](https://redash.io/) kullanabilirsiniz. [Yeniden Dash için bir veri kaynağı olarak Azure Veri Gezgini ayarlayın ve ardından verileri görselleştirin](/azure/data-explorer/redash).