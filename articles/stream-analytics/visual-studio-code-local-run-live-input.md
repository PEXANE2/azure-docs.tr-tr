---
title: Visual Studio Code kullanarak Azure Akış Analizi sorgularını canlı akış girişine karşı yerel olarak test edin
description: Bu makalede, Visual Studio Code için Azure Akış Analizi Araçlarını kullanarak sorguların canlı akış girişine karşı yerel olarak nasıl test edilecek olduğu açıklanmaktadır.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 11/14/2019
ms.topic: conceptual
ms.openlocfilehash: 34ce91a1385f951847abeedd3a6b526d3a07af35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75660860"
---
# <a name="test-stream-analytics-queries-locally-against-live-stream-input-by-using-visual-studio-code"></a>Test Stream Analytics, Visual Studio Code'u kullanarak canlı akış girişine karşı yerel olarak sorgular

Akış Analizi işlerinizi canlı akış girişine karşı yerel olarak test etmek için Visual Studio Code için Azure Akış Analizi Araçlarını kullanabilirsiniz. Giriş, Azure Etkinlik Hub'ları veya Azure IoT Hub gibi bir kaynaktan gelebilir. Çıktı sonuçları, JSON dosyaları olarak projenizdeki **LocalRunOutputs**adlı bir klasöre gönderilir.

## <a name="prerequisites"></a>Ön koşullar

* [.NET Core SDK'yı](https://dotnet.microsoft.com/download) yükleyin ve Visual Studio Kodunu yeniden başlatın.

* Visual Studio Code'u kullanarak Bir Akış Analizi işi oluşturmayı öğrenmek için [bu hızlı başlangıcı](quick-create-vs-code.md) kullanın.

## <a name="define-a-live-stream-input"></a>Canlı akış girişi tanımlama

1. Akış Analizi projenizdeki **Girişler** klasörüne sağ tıklayın. Ardından **ASA:** Bağlam menüsünden Giriş Ekle'yi seçin.

   ![Girişler klasöründen giriş ekleme](./media/quick-create-vs-code/add-input-from-inputs-folder.png)

   Komut paletini açmak için **Ctrl+Shift+P'yi** de seçebilir ve **ASA'ya girebilirsiniz: Giriş Ekle**.

   ![Visual Studio Koduna Akış Analizi girişi ekleme](./media/quick-create-vs-code/add-input.png)

2. Açılan listeden bir giriş kaynağı türü seçin.

   ![Giriş seçeneği olarak IoT hub'ını seçin](./media/quick-create-vs-code/iot-hub.png)

3. Komut paletinden girişi eklediyseniz, girişi kullanacak Stream Analytics sorgu komut dosyasını seçin. Otomatik olarak **myASAproj.asaql**için dosya yolu ile doldurulmalıdır.

   ![Visual Studio Code'da bir Akış Analizi komut dosyası seçin](./media/quick-create-vs-code/asa-script.png)

4. Açılan menüden **Azure Aboneliklerinizden Seç'i** seçin.

    ![Aboneliklerden seçim](./media/quick-create-vs-code/add-input-select-subscription.png)

5. Yeni oluşturulan JSON dosyasını yapılandırın. CodeLens özelliğini kullanarak bir dize girmenize, açılır listeden seçim yapmaya veya doğrudan dosyadaki metni değiştirmenize yardımcı olabilirsiniz. Aşağıdaki ekran görüntüsü, **Aboneliklerinizden Select'i** örnek olarak gösterir.

   ![Visual Studio Kodu'nda girişi yapılandırma](./media/quick-create-vs-code/configure-input.png)

## <a name="preview-input"></a>Girişi önizleme

Giriş verilerinin geldiğinden emin olmak için, canlı giriş yapılandırma dosyanızdaki Verileri En üst hattan **Önizleme'yi** seçin. Bazı giriş verileri bir IoT hub'ından gelir ve önizleme penceresinde gösterilir. Önizlemenin görünmesi birkaç saniye sürebilir.

 ![Canlı girişi önizleme](./media/quick-create-vs-code/preview-live-input.png)

## <a name="run-queries-locally"></a>Sorguları yerel olarak çalıştırma

Sorgu düzenleyicinize dönün ve **yerel olarak Çalıştır'ı**seçin. Ardından açılan listeden **Canlı Girişi Kullan'ı** seçin.

![Sorgu düzenleyicisinde "Yerel olarak çalıştır" seçeneğini belirleyin](./media/vscode-local-run/run-locally.png)

!["Canlı Giriş Kullan" seçeneğini belirleyin](./media/vscode-local-run-live-input/run-locally-use-live-input.png)

Sonuç sağ pencerede gösterilir ve her 3 saniyede bir yenilenir. Yeniden test etmek için **Çalıştır'ı** seçebilirsiniz. Ayrıca Dosya **Gezgini'ndeki** sonuç dosyalarını görmek için klasörde Aç'ı seçebilir ve Visual Studio Code veya Excel gibi bir araçla açabilirsiniz. Sonuç dosyalarının yalnızca JSON biçiminde kullanılabildiğini unutmayın.

İşin çıktı oluşturmaya başlaması için varsayılan süre **Şimdi**olarak ayarlanır. Sonuç penceresinde **Çıktı başlangıç saati** düğmesini seçerek zamanı özelleştirebilirsiniz.

![Yerel çalışma sonucunu görüntüleme](./media/vscode-local-run-live-input/vscode-livetesting.gif)

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code (önizleme) ile Azure Akış Analizi işlerini keşfedin](visual-studio-code-explore-jobs.md)

* [npm paketini kullanarak CI/CD ardışık hatlarını ayarlama](setup-cicd-vs-code.md)
