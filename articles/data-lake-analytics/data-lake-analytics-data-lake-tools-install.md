---
title: Visual Studio için Azure Data Lake Araçları’nı yükleme
description: Bu makalede, Visual Studio için Azure Veri Gölü Araçları nasıl yüklenir.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 08f0456b6a8b663f0af68fbb179de14b9e3acf79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70914065"
---
# <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake Araçları’nı yükleme.

Azure Veri Gölü Analizi hesapları oluşturmak için Visual Studio'yı nasıl kullanacağınızı öğrenin. [U-SQL'deki](data-lake-analytics-u-sql-get-started.md) işleri tanımlayabilir ve işleri Data Lake Analytics hizmetine gönderebilirsiniz. Veri Gölü Analizi hakkında daha fazla bilgi için [Azure Veri Gölü Analizi'ne genel bakış](data-lake-analytics-overview.md)bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

* **Visual Studio**: Express dışında tüm sürümler desteklenir.

  * Visual Studio 2019
  * Visual Studio 2017
  * Visual Studio 2015
  * Visual Studio 2013

* **.NET için Microsoft Azure SDK** 2.7.1 sürümü veya sonraki sürümleri. [Web platformu yükleyicisini](https://www.microsoft.com/web/downloads/platform.aspx) kullanarak yükleyin.
* **Bir Data Lake Analytics** hesabı. Hesap oluşturmak için bkz. [Azure portalı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio-2017-or-visual-studio-2019"></a>Visual Studio 2017 veya Visual Studio 2019 için Azure Veri Gölü Araçlarını Yükleyin

Visual Studio için Azure Veri Gölü Araçları Visual Studio 2017 15.3 veya sonraki yıllarda desteklenir. Araç, **Veri depolama ve işleme ve** Azure **geliştirme** iş yüklerinin bir parçasıdır. Visual Studio yüklemenizin parçası olarak bu iki iş yükünden birini etkinleştirin.

Gösterildiği gibi **Veri depolama ve işleme** iş yükünü etkinleştirin:

![Veri depolama ve işleme iş yükünü etkinleştirme](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2019-install-01.png)

Gösterildiği gibi **Azure geliştirme** iş yükünü etkinleştirin:

![Azure geliştirme iş yükünü seçin](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2019-install-02.png)

## <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Visual Studio 2013 ve 2015 için Azure Data Lake Araçları’nı yükleme

[Visual Studio için Microsoft Azure Veri Gölü ve Akış Analizi Araçlarını ](https://aka.ms/adltoolsvs)indirin ve yükleyin. Kurulumdan sonra Visual Studio'da aşağıdaki değişiklikler vardır:

* **Sunucu Gezgini** > **Azure** düğümü bir Veri **Gölü Analizi** düğümü içerir.
* **Araçlar** menüsünde **Data Lake** öğesi vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Tanılama bilgilerini günlüğe kaydetmek için Bkz. [Azure Veri Gölü Analizi için tanılama günlüklerine erişme.](data-lake-analytics-diagnostic-logs.md)
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.
* Vertex yürütme görünümünü kullanmak [için](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)bkz.
