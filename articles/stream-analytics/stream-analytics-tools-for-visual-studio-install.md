---
title: Visual Studio için Azure Stream Analytics araçları ayarlama
description: Bu makalede, Yükleme gereksinimleri ve Visual Studio için Azure Stream Analytics araçlarının nasıl ayarlanacağı açıklanır.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: fa3990d2239da71fb27f4c9a06699f758fcfe7a0
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354360"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Visual Studio için Azure Stream Analytics araçları 'nı yükler

Visual Studio 2019 ve Visual Studio 2017 Azure Data Lake ve Stream Analytics araçları destekler. Bu makalede araçların nasıl yükleneceği ve kaldırılacağı açıklanır.

Araçları kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Visual Studio 'yu kullanarak Azure Stream Analytics Işi oluşturma](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Yükleme

Visual Studio Enterprise (Ultimate/Premium), Professional ve Community sürümleri, araçları destekler. Express Edition ve Mac için Visual Studio bunları desteklemez.

Visual Studio 2019 önerilir.

### Visual Studio 2019 ve 2017 için Install<a name="recommended-visual-studio-2019-and-2017"></a>

Azure Data Lake ve Stream Analytics araçları, **Azure geliştirme** ve **veri depolama ve işleme** iş yüklerinin bir parçasıdır. Yükleme sırasında bu iki iş yükünün birini etkinleştirin. Visual Studio zaten yüklüyse, araçları **ve özellikleri al** > iş yüklerini eklemek için **Araçlar** ' ı seçin.

[Visual studio 2019 (Preview 2 veya üzeri) ya da Visual studio 2017 (15,3 veya üzeri)](https://www.visualstudio.com/) sürümünü indirin ve yükleme yönergelerini izleyin.

**Veri depolama ve işleme** iş yükünü gösterildiği gibi seçin:

![Veri depolama ve işleme iş yükü seçildi](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2019-install-01.png)

Gösterildiği gibi **Azure geliştirme** iş yükünü seçin:

![Azure geliştirme iş yükü seçildi](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2019-install-02.png)

İş yükünü ekledikten sonra, araçları güncelleştirin. Bu yordam, Visual Studio 2019 'e başvurur:

1. Uzantıları **yönet** > **Uzantılar** ' ı seçin.

1. **Uzantıları Yönet**' de **güncelleştirmeler** ' i seçin ve **Azure Data Lake ve Stream Analytics araçları**' nı seçin.

1. En son uzantıyı yüklemek için **Güncelleştir** ' i seçin.

![Visual Studio uzantıları ve güncelleştirmeleri](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-vs2019-extensions-updates.png)

### Visual Studio 2015 ve 2013 için Install<a name="visual-studio-2015-2013"></a>

Visual Studio Enterprise (Ultimate/Premium), Professional ve Community sürümleri, araçları destekler. Express Edition bunları desteklemez.

* Visual Studio 2015 veya Visual Studio 2013 Güncelleştirme 4 'i yükler.
* [Web platformu yükleyicisini](https://www.microsoft.com/web/downloads/platform.aspx)kullanarak .NET için Microsoft Azure SDK sürüm 2.7.1 veya üstünü yükleme.
* [Visual Studio için Microsoft Azure Data Lake ve Stream Analytics araçları](https://www.microsoft.com/en-us/download/details.aspx?id=49504)'nı yükler.

## Update<a name="visual-studio-2019-and-2017"></a><a name="visual-studio-2015-and-2013"></a>

Visual Studio 2019 ve Visual Studio 2017 için yeni bir sürüm anımsatıcısı, Visual Studio bildirimi olarak gösterilir.

Visual Studio 2015 ve Visual Studio 2013 için araçlar, yeni sürümleri otomatik olarak denetler. En son sürümü yüklemek için yönergeleri izleyin.

## <a name="uninstall"></a>Kaldırma

Azure Data Lake ve Stream Analytics araçlarını kaldırabilirsiniz. Visual Studio 2019 veya Visual Studio 2017 için Araçlar **ve Özellikler al** > **Araçlar** ' ı seçin. **Değiştirme**bölümünde **Azure Data Lake ve Stream Analytics araçlarının**seçimini kaldırın. **Veri depolama ve işleme** iş yükü ya da **Azure geliştirme** iş yükü altında görünür.

Visual Studio 2015 veya Visual Studio 2013 kaldırmak için, **Denetim masası** > **Programlar ve Özellikler**' e gidin. **Visual Studio için Microsoft Azure Data Lake ve Stream Analytics araçları**'nı kaldırın.
