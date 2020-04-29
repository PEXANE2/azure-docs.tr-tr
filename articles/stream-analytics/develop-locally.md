---
title: Azure Stream Analytics işleri yerel olarak geliştirme ve hata ayıklama
description: Azure portal ' de çalıştırmadan önce yerel bilgisayarınızda Azure Stream Analytics işleri geliştirmeyi ve test yapmayı öğrenin.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 03/31/2020
ms.service: stream-analytics
ms.openlocfilehash: 736fce1d4b347e36ad5c10ca89ad0627104a0232
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80879849"
---
# <a name="develop-and-debug-azure-stream-analytics-jobs-locally"></a>Azure Stream Analytics işleri yerel olarak geliştirme ve hata ayıklama

Azure portal Azure Stream Analytics işleri oluşturup test edebiliyor olsa da birçok geliştirici, yerel bir geliştirme deneyimi tercih eder. Stream Analytics, tam olarak arızalı tek düğümlü yerel çalışma zamanı kullanarak Azure Olay Hub 'ı, IoT Hub ve BLOB depolamadan canlı olay akışlarıyla iş oluşturmak ve test etmek için en sevdiğiniz kod düzenleyicinizi ve geliştirme araçlarınızı kullanmayı kolaylaştırır. Ayrıca, işleri doğrudan yerel geliştirme ortamınızdan Azure 'a gönderebilirsiniz.

## <a name="local-development-environments"></a>Yerel geliştirme ortamları

Yerel bilgisayarınızda Stream Analytics işleri geliştirme yönteminiz, araç tercihleriniz ve özellik kullanılabilirliğine bağlıdır. Her geliştirme ortamı için desteklenen özellikleri görmek için bkz. [Azure Stream Analytics Özellik Karşılaştırması](feature-comparison.md) .

Aşağıdaki tablodaki ortamlar yerel geliştirmeyi destekler:

|Ortam                              |Açıklama    |
|-----------------------------------------|------------|
|[Visual Studio Code](visual-studio-code-explore-jobs.md)| Visual Studio Code için [Azure Stream Analytics araçları uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-bigdatatools.vscode-asa) , hem yerel olarak hem de bulutta zengin IntelliSense ve yerel kaynak denetimi ile Stream Analytics işinizi hem yerel olarak hem de bulutta yazmanıza, yönetmenize ve test etmenize olanak tanır. Linux, MacOS ve Windows üzerinde geliştirmeyi destekler. Daha fazla bilgi için bkz. [Visual Studio Code Azure Stream Analytics Iş oluşturma](quick-create-vs-code.md).|
|[Visual Studio 2019](stream-analytics-tools-for-visual-studio-install.md) |Stream Analytics araçları, Visual Studio 'da Azure geliştirme ve veri depolama ve işleme iş yüklerinin bir parçasıdır. Özel C# Kullanıcı tanımlı işlevleri ve seri hale getiriciler yazmak için Visual Studio 'Yu kullanabilirsiniz. Daha fazla bilgi edinmek için bkz. [Visual Studio kullanarak Azure Stream Analytics Işi oluşturma](stream-analytics-quick-create-vs.md).|
|[Komut istemi veya Terminal](stream-analytics-tools-for-visual-studio-cicd.md)|Azure Stream Analytics CI/CD NuGet paketi, rastgele bir makinede Visual Studio proje derlemesi, yerel test için araçlar sağlar. Azure Stream Analytics CI/CD NPM paketi, rastgele bir makinede Visual Studio Code proje yapıları (bir Azure Resource Manager şablonu oluşturur) için araçlar sağlar.|

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code kullanarak örnek verilerle yerel olarak Stream Analytics sorguları test edin](visual-studio-code-local-run.md)
* [Visual Studio Code kullanarak canlı akış girişine göre Stream Analytics sorguları yerel olarak test edin](visual-studio-code-local-run-live-input.md)
* [Visual Studio ile yerel olarak Stream Analytics sorguları test etme](stream-analytics-vs-tools-local-run.md)
* [Visual Studio için Azure Stream Analytics araçlarını kullanarak canlı verileri yerel olarak test etme](stream-analytics-live-data-local-testing.md)
