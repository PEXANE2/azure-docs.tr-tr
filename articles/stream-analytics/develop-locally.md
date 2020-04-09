---
title: Azure Akış Analizi işlerini yerel olarak geliştirme ve hata ayıklama
description: Azure portalında çalıştırmadan önce Azure Akış Analizi işlerini yerel bilgisayarınızda nasıl geliştirip test edebilirsiniz öğrenin.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 03/31/2020
ms.service: stream-analytics
ms.openlocfilehash: 736fce1d4b347e36ad5c10ca89ad0627104a0232
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879849"
---
# <a name="develop-and-debug-azure-stream-analytics-jobs-locally"></a>Azure Akış Analizi işlerini yerel olarak geliştirme ve hata ayıklama

Azure portalında Azure Akış Analizi işlerini oluşturup test edebilseniz de, birçok geliştirici yerel bir geliştirme deneyimini tercih eder. Akış Analizi, azure Event Hub, IoT Hub ve Blob Depolama'dan canlı etkinlik akışlarıyla tam işlevli tek bir tek düğüm yerel çalışma zamanı kullanarak iş oluşturmak ve test etmek için favori kod düzenleyicinizi ve geliştirme araçlarınızı kullanmayı kolaylaştırır. İşlerinizi doğrudan yerel geliştirme ortamınızdan Azure'a da gönderebilirsiniz.

## <a name="local-development-environments"></a>Yerel gelişim ortamları

Yerel bilgisayarınızda Stream Analytics işlerini geliştirme şekliniz, araç tercihlerinize ve özellik kullanılabilirliğinize bağlıdır. Her geliştirme ortamı için hangi özelliklerin desteklenedildiğini görmek için [Azure Akış Analizi özelliğine](feature-comparison.md) bakın.

Aşağıdaki tablodaki ortamlar yerel gelişimi destekler:

|Ortam                              |Açıklama    |
|-----------------------------------------|------------|
|[Visual Studio Code](visual-studio-code-explore-jobs.md)| Visual Studio Code için [Azure Akış Analiz Araçları uzantısı,](https://marketplace.visualstudio.com/items?itemName=ms-bigdatatools.vscode-asa) zengin IntelliSense ve yerel kaynak denetimi yle Akış analizi işinizi hem yerel hem de bulutta yazmanızı, yönetmenize ve test etmenizi sağlar. Linux, MacOS ve Windows'da geliştirmeyi destekler. Daha fazla bilgi için visual [studio kodunda Azure Akışı Analizi işi oluşturun](quick-create-vs-code.md)bölümüne bakın.|
|[Visual Studio 2019](stream-analytics-tools-for-visual-studio-install.md) |Akış Analizi Araçları, Visual Studio'daki Azure geliştirme ve Veri depolama ve işleme iş yüklerinin bir parçasıdır. Özel C# kullanıcı tanımlı işlevleri ve deserializers yazmak için Visual Studio kullanabilirsiniz. Daha fazla bilgi için [Visual Studio'u kullanarak Azure Akışı Analizi İşi oluştur](stream-analytics-quick-create-vs.md)bölümüne bakın.|
|[Komut istemi veya terminal](stream-analytics-tools-for-visual-studio-cicd.md)|Azure Akış Analytics CI/CD NuGet paketi, Visual studio proje oluşturma araçları, rasgele bir makinede yerel test sağlar. Azure Akışı Analytics CI/CD npm paketi, rasgele bir makinede Visual Studio Code proje oluşturma (Azure Kaynak Yöneticisi şablonu oluşturur) için araçlar sağlar.|

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code kullanarak örnek verilerle test Akışı Analizi sorguları](visual-studio-code-local-run.md)
* [Test Stream Analytics, Visual Studio Code'u kullanarak canlı akış girişine karşı yerel olarak sorgular](visual-studio-code-local-run-live-input.md)
* [Visual Studio ile Test Stream Analytics sorgularını yerel olarak sorgulayın](stream-analytics-vs-tools-local-run.md)
* [Visual Studio için Azure Akış Analizi araçlarını kullanarak canlı verileri yerel olarak test edin](stream-analytics-live-data-local-testing.md)
