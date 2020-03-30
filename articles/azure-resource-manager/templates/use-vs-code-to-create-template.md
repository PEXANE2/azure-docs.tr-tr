---
title: Şablonoluşturmak için Visual Studio Kodunu kullanma
description: Visual Studio Kodu ve Azure Kaynak Yöneticisi Araçları uzantısını nasıl yükleyip kullanacağınızı öğrenin.
author: mumian
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: jgao
ms.openlocfilehash: 585e2773ea5d6af184f85e65b63d39b60d632146
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273637"
---
# <a name="use-visual-studio-code-to-create-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonları oluşturmak için Visual Studio Kodunu kullanın

Visual Studio Code hafif, çok platformlu, açık kaynak kodlu bir editördür. Azure Kaynak Yöneticisi Şablon Araçları uzantısı, Kaynak Yöneticisi şablon geliştirme eklentisidir. Uzantı, size intellisense, sözdizimi vurgulama, satır içi yardım ve diğer birçok dil işlevi vermek için şablonlar için dil desteği ekler. Birlikte, önerilen şablon geliştirme deneyimi sağlar.

## <a name="install-visual-studio-code"></a>Visual Studio Code'u yükleme

Visual studio Code MacOS, Windows ve Linux'u destekler.  [Visual Studio Code'dan](https://code.visualstudio.com/)yüklenebilir.

## <a name="install-resource-manager-tools-extension"></a>Kaynak Yöneticisi Araçları uzantısı nı yükleme

1. Visual Studio Code'u açın.
1. Sol menüden **Uzantılar'ı** seçin. Veya **Görünüm** menüsünden Uzanlar bölmesini açmak için **Uzanlar'ı** seçin.

    ![Visual Studio Code Resource Manager Tools eklentisini yükleyin](./media/use-vs-code-to-create-template/resource-manager-visual-studio-code-tools-extension.png)
1. Kaynak **Yöneticisi'ni**ara.
1. **Azure Kaynak Yöneticisi Araçları**altında **Yükle'yi** seçin.

## <a name="the-extension-features"></a>Uzantı özellikleri

### <a name="colorization-for-template-language-expressions"></a>Şablon Dil İfadeleri için Renklendirme

Parametreler, değişkenler, işlevler, adlar ve ifadeler aşağıdaki ekran görüntüsünde gösterildiği gibi renk kodludur:

![Visual Studio Code Resource Manager araçları uzantıları renklendirme](./media/use-vs-code-to-create-template/resource-manager-tools-extension-colorization.png)

Şablon anahat görünümü, büyük şablonlarda gezinmeyi kolaylaştırır.

### <a name="intellisense"></a>ıntellisense

Kaynak Yöneticisi şablon uzantısı işlev adları, parametreler, değişkenler ve başvurular için olası tamamlamaları bilir. Siz yazarken IntelliSense önerileri açılır. Karakter yazmaya devam ederseniz, üye listesi (değişkenler, yöntemler, vb.) yalnızca yazdığınız karakterleri içeren üyeleri içerecek şekilde filtrelenir. **Sekme** veya **Enter** tuşuna basArak seçili üyeyi ekler.

- Yerleşik işlev adları

    ![Visual Studio Code Resource Manager araçları uzantıları intellisense fonksiyonları](./media/use-vs-code-to-create-template/resource-manager-tools-extension-intellisense-functions.png)

- Parametre referansları

    ![Visual Studio Code Resource Manager araçları uzantıları intellisense parametreleri](./media/use-vs-code-to-create-template/resource-manager-tools-extension-intellisense-parameters.png)

- Değişken referanslar

    ![Visual Studio Code Resource Manager araçları uzantıları intellisense değişkenler](./media/use-vs-code-to-create-template/resource-manager-tools-extension-intellisense-variables.png)

- resourceGroup() özellikleri

    ![Visual Studio Code Resource Manager araçları uzantıları intellisense fonksiyonları](./media/use-vs-code-to-create-template/resource-manager-tools-extension-intellisense-resourcegroup.png)

Buna ek olarak, intellisense aynı zamanda abonelik() özellikleri ve nesne olan değişkenlere yapılan başvuruların özellikleriyle de çalışır.

### <a name="signature-help-for-function-parameters"></a>Fonksiyon parametreleri için imza yardımı

İşlev adlarının üzerinde gezinirken, uzantı işlev parametreleri için imza yardımını gösterir.

![Visual Studio Code Resource Manager araçları uzantıları imza fonksiyonu](./media/use-vs-code-to-create-template/resource-manager-tools-extension-signature-function.png)

### <a name="go-to-definition-for-variable-and-parameter-references"></a>Değişken ve parametre başvuruları için Tanıma Git

**Ctrl+Click**ile tanıma atlayabilirsiniz veya ekran görüntüsünde gösterildiği gibi ![bağlam menüsünü kullanarak: Visual Studio Code Resource Manager araçları uzantıları tanıma gider](./media/use-vs-code-to-create-template/resource-manager-tools-extension-context-menu.png)

Tanımı **Ctrl+Alt+Click**ile yan lara açabilirsiniz.

### <a name="peek-for-variable-and-parameter-definitions"></a>Değişken ve parametre tanımları için peek

Peeked düzenleyiciyi açmak için, önceki ekran görüntüsünde gösterildiği gibi bağlam menüsünü kullanın.

Aşağıdaki ekran görüntüsü peek düzenleyicisini gösterir:

![Visual Studio Code Resource Manager araçları uzantıları peek düzenleyici](./media/use-vs-code-to-create-template/resource-manager-tools-extension-peek-editor.png)

### <a name="find-all-references-for-variables-and-parameters"></a>Değişkenler ve parametreler için tüm başvuruları bulun

Tüm başvuruları bulmak için, önceki ekran görüntüsünde gösterildiği gibi bağlam menüsünü kullanın.

Aşağıdaki ekran görüntüsü, başvuruların nasıl vurgulandığıgösterilmektedir:

![Visual Studio Code Resource Manager araçları uzantıları tüm başvuruları bulabilirsiniz](./media/use-vs-code-to-create-template/resource-manager-tools-extension-find-all-references.png)

### <a name="rename-all-references-for-variables-and-parameters"></a>Değişkenler ve parametreler için tüm başvuruları yeniden adlandırma

Tüm başvuruları değişkenler ve parametreler için yeniden adlandırmak için, önceki ekran görüntüsünde gösterildiği gibi bağlam menüsünü kullanın.

### <a name="hover-for-parameter-description"></a>Parametre açıklaması için hover

![Visual Studio Code Resource Manager araçları uzantıları hover tanımı](./media/use-vs-code-to-create-template/resource-manager-tools-extension-hover-parameters.png)

### <a name="brace-matching"></a>Ayraç eşleştirme

Eşleşen köşeli ayraçlar imleç bunlardan birine yakın olduğu anda vurgulanır. Bir ayraç tıklattığınızda, eşleşen ayraç da aşağıdaki ekran görüntüsünde gösterildiği gibi vurgulanır:

![Visual Studio Code Resource Manager araçları uzantıları ayraç eşleştirme](./media/use-vs-code-to-create-template/resource-manager-tools-extension-brace-matching.png)

### <a name="show-errors-and-warnings"></a>Hataları ve uyarıları gösterme

Uzantı tarafından tanımlanan hatalar şunlardır:

- Tanımlanmamış parametre referansları
- Tanımlanmamış değişken referanslar
- Tanınmayan işlev adları
- referans() değişken tanımında fonksiyon kullanımı
- Işlevlerde yanlış bağımsız değişken sayısı

Uyarılar şunlardır:

- Kullanılmayan parametreler
- Kullanılmayan değişkenler

## <a name="next-steps"></a>Sonraki adımlar

- Azure Kaynak Yöneticisi şablonları oluşturma hakkında bilgi edinmek için [bkz.](template-tutorial-create-first-template.md)
- Visual Studio Code'u kullanarak hızlı bir başlangıç yapmak için [Bkz. Hızlı Başlangıç: Visual Studio Code kullanarak Azure Kaynak Yöneticisi şablonları oluşturma](quickstart-create-templates-use-visual-studio-code.md)
