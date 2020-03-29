---
title: Visual Studio için Azure Akış Analizi araçlarını ayarlama
description: Bu makalede, yükleme gereksinimleri ve Visual Studio için Azure Akış Analizi araçlarının nasıl ayarılanıaçıklanmaktadır.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: fa3990d2239da71fb27f4c9a06699f758fcfe7a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75354360"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Visual Studio için Azure Akışı Analytics araçlarını yükleyin

Visual Studio 2019 ve Visual Studio 2017, Azure Veri Gölü ve Akış Analizi Araçlarını destekler. Bu makalede, araçların nasıl yüklenir ve kaldırılanın.

Araçları kullanma hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: Visual Studio'yu kullanarak Bir Azure Akışı Analizi işi oluşturun.](stream-analytics-quick-create-vs.md)

## <a name="install"></a>Yükleme

Visual Studio Enterprise (Ultimate/Premium), Professional ve Community sürümleri araçları destekler. Express sürümü ve Mac için Visual Studio bunları desteklemez.

Visual Studio 2019'u öneriyoruz.

### <a name="install-for-visual-studio-2019-and-2017"></a>Visual Studio 2019 ve 2017 için yükleyin<a name="recommended-visual-studio-2019-and-2017"></a>

Azure Veri Gölü ve Akış Analizi Araçları, **Azure geliştirme** ve Veri depolama **ve işleme** iş yüklerinin bir parçasıdır. Yükleme sırasında bu iki iş yükünden birini etkinleştirin. Visual Studio zaten yüklüyse, iş yükleri eklemek için **Araçlar** > **Araçları ve Özellikleri Al'ı** seçin.

[Visual Studio 2019 (Önizleme 2 veya üzeri) veya Visual Studio 2017 (15.3 veya üzeri)](https://www.visualstudio.com/) indirin ve yükleme yönergelerini izleyin.

Gösterildiği gibi **Veri depolama ve işleme** iş yükünü seçin:

![Veri depolama ve işleme iş yükü seçilir](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2019-install-01.png)

Gösterildiği gibi **Azure geliştirme** iş yükünü seçin:

![Azure geliştirme iş yükü seçilir](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2019-install-02.png)

İş yükünü ekledikten sonra araçları güncelleştirin. Bu yordam Visual Studio 2019 anlamına gelir:

1. **Uzantıları** > **Yönet Uzantıları'nı**seçin.

1. Uzantıları Yönet'te **Güncelleştirmeler'i** seçin ve Azure Veri Gölü ve Akış Analizi **Manage Extensions** **Araçlarını**seçin.

1. En son uzantıyı yüklemek için **Güncelleştir'i** seçin.

![Visual Studio uzantıları ve güncellemeleri](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-vs2019-extensions-updates.png)

### <a name="install-for-visual-studio-2015-and-2013"></a>Visual Studio 2015 ve 2013 için yükleyin<a name="visual-studio-2015-2013"></a>

Visual Studio Enterprise (Ultimate/Premium), Professional ve Community sürümleri araçları destekler. Express edition onları desteklemiyor.

* Visual Studio 2015 veya Visual Studio 2013 Güncelleme 4'e yükleyin.
* [Web platformu yükleyicisini](https://www.microsoft.com/web/downloads/platform.aspx)kullanarak .NET sürüm 2.7.1 veya üzeri için Microsoft Azure SDK'yı yükleyin.
* [Visual Studio için Microsoft Azure Veri Gölü ve Akış Analizi Araçlarını](https://www.microsoft.com/en-us/download/details.aspx?id=49504)yükleyin.

## <a name="update"></a>Güncelleştirme<a name="visual-studio-2019-and-2017"></a><a name="visual-studio-2015-and-2013"></a>

Visual Studio 2019 ve Visual Studio 2017 için yeni bir sürüm hatırlatıcısı Visual Studio bildirimi olarak ortaya çıkar.

Visual Studio 2015 ve Visual Studio 2013 için araçlar yeni sürümleri otomatik olarak denetler. En son sürümü yüklemek için yönergeleri izleyin.

## <a name="uninstall"></a>Kaldırma

Azure Veri Gölü ve Akış Analizi Araçlarını kaldırabilirsiniz. Visual Studio 2019 veya Visual Studio 2017 için **Araçlar** > **Alın Araçlarını ve Özellikleri'ni**seçin. **Değiştirmede,** Azure **Veri Gölü ve Akış Analizi Araçlarını**seçin. Veri depolama ve **işleme** iş yükü veya **Azure geliştirme** iş yükü altında görünür.

Visual Studio 2015 veya Visual Studio 2013'ten kaldırmak için **Denetim Masası** > **Programları ve Özellikleri'ne**gidin. Visual **Studio için Microsoft Azure Veri Gölü ve Akış Analizi Araçlarını**kaldırın.
