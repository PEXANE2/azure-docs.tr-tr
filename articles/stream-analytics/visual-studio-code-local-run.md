---
title: Visual Studio Code'u kullanarak örnek verilerle azure akışı analizi işini yerel olarak test edin
description: Bu makalede, Visual Studio Code için Azure Akış Analizi Araçları'nı kullanarak örnek verilerle sorguların yerel olarak nasıl test edilecek olduğu açıklanmaktadır.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 11/10/2019
ms.topic: conceptual
ms.openlocfilehash: c29d0d9ecd856ee9611df21d23b1b2b763e24652
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75486475"
---
# <a name="test-stream-analytics-queries-locally-with-sample-data-using-visual-studio-code"></a>Visual Studio Code kullanarak örnek verilerle test Akışı Analizi sorguları

Visual Studio Code için Azure Akış Analizi araçlarını kullanarak Stream Analytics işlerinizi örnek verilerle yerel olarak test edebilirsiniz. Sorgu sonuçlarını JSON dosyalarında projenizin **LocalRunOutputs** klasöründe bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* [.NET çekirdek SDK'yı](https://dotnet.microsoft.com/download) yükleyin ve Visual Studio Code'u yeniden başlatın.

* Visual Studio Code'u kullanarak Bir Akış Analizi işi oluşturmayı öğrenmek için bu [hızlı başlangıcı](quick-create-vs-code.md) kullanın.

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

Önce örnek giriş veri dosyaları hazırlamanız gerekir. Makinenizde zaten bazı örnek veri dosyaları varsa, bu adımı atlayabilir ve bir sonrakine geçebilirsiniz.

1. Giriş yapılandırma dosyanızdaki verileri en üst satırdan **Önizleme'yi** tıklatın. Bazı giriş verileri IoT Hub'ından getirilir ve önizleme penceresinde gösterilir. Bunun biraz zaman alabileceğini unutmayın.

2. Veriler gösterildikten sonra, verileri yerel bir dosyaya kaydetmek için **Kaydet'i** tıklatın.

 ![Canlı girişi önizleme](./media/quick-create-vs-code/preview-live-input.png)

## <a name="define-a-local-input"></a>Yerel bir giriş tanımlama

1. Akış Analizi projenizde Girişler klasörü altında **input.json'ı** tıklatın. Ardından üst hattan **yerel giriş ekle'yi** seçin.

    ![Projeden yerel giriş ekleme](./media/quick-create-vs-code/add-input-from-project.png)

    Komut paletini açmak ve ASA girmek için **Ctrl+Shift+P'yi** de **kullanabilirsiniz: Giriş Ekle.**

   ![VS Koduna Akış Analizi girişi ekleme](./media/quick-create-vs-code/add-input.png)

2. **Yerel Giriş'i**seçin.

    ![Visual Studio koduna ASA yerel girişi ekleme](./media/vscode-local-run/add-local-input.png)

3. Seçin **+ Yeni Yerel Giriş**.

    ![Visual Studio koduna yeni bir ASA yerel girişi ekleme](./media/vscode-local-run/add-new-local-input.png)

4. Sorgunuzda kullandığınız giriş takma adını girin.

    ![Yeni bir ASA yerel giriş takma adı ekleme](./media/vscode-local-run/new-local-input-alias.png)

5. Yeni oluşturulan **LocalInput_Input.json** dosyasında, yerel veri dosyanızın bulunduğu dosya yolunu girin.

    ![Visual Studio'da yerel dosya yolunu girin](./media/vscode-local-run/local-file-path.png)

6. Giriş verilerini önizlemek için **Verileri Önizleme'yi** seçin. Verileriniz için serileştirme türü, JSON veya CSV'si varsa otomatik olarak algılanır. Verilerinizi **Tablo** veya **Ham** formatında görüntülemek için seçiciyi kullanın. Aşağıdaki tablo **Tablo biçiminde**veri bir örnektir:

     ![Yerel verileri tablo biçiminde önizleme](./media/vscode-local-run/local-file-preview-table.png)

    Aşağıdaki tablo **Raw biçiminde**veri bir örnektir:

    ![Yerel verileri ham formatta önizleme](./media/vscode-local-run/local-file-preview-raw.png)

## <a name="run-queries-locally"></a>Sorguları yerel olarak çalıştırma

Sorgu düzenleyicinize dönün ve **yerel olarak Çalıştır'ı**seçin. Ardından açılır listeden **yerel girişi kullan'ı** seçin.

![Sorgu düzenleyicisinde yerel olarak çalıştır'ı seçin](./media/vscode-local-run/run-locally.png)

![Yerel girişi kullanma](./media/vscode-local-run/run-locally-use-local-input.png)

Sonuç sağ pencerede gösterilir. Yeniden test etmek için **Çalıştır'ı** tıklatabilirsiniz. Ayrıca, sonuç dosyalarını dosya gezgininde görmek ve diğer araçlarla daha fazla açmak için **klasörde Aç'ı** da seçebilirsiniz. Sonuç dosyalarının yalnızca JSON biçiminde kullanılabildiğini unutmayın.

![Yerel çalışma sonucunu görüntüleme](./media/vscode-local-run/run-locally-result.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code kullanarak azure akış analizi işlerini canlı girişle yerel olarak test edin](visual-studio-code-local-run-live-input.md)

* [Visual Studio Code (Önizleme) ile Azure Akış Analizi işlerini keşfedin](visual-studio-code-explore-jobs.md)
