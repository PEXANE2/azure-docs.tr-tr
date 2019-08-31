---
title: Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma
description: Visual Studio Code ve Azure Resource Manager araçları uzantısını yüklemeyi ve kullanmayı öğrenin.
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: jgao
ms.openlocfilehash: 8d2b818846eeae44ffe05b4546a333510c638621
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196159"
---
# <a name="use-visual-studio-code-to-create-azure-resource-manager-templates"></a>Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma

Visual Studio Code basit, çok platformlu, açık kaynaklı bir düzenleyicidir. Azure Resource Manager şablon araçları uzantısı, Kaynak Yöneticisi Şablon geliştirmesi için bir eklentidir. Uzantı, IntelliSense, sözdizimi vurgulama, çevrimiçi Yardım ve diğer birçok dil işlevi sağlamak için şablonlar için dil desteği ekler. Birlikte, önerilen şablon geliştirme deneyimi sağlar.

## <a name="install-visual-studio-code"></a>Visual Studio Kodu'nu yükle

Visual Studio Code MacOS, Windows ve Linux 'u destekler.  [Visual Studio Code](https://code.visualstudio.com/)yüklenebilir.

## <a name="install-resource-manager-tools-extension"></a>Kaynak Yöneticisi Araçları uzantısını yükler

1. Visual Studio Code'u açın.
1. Sol menüden **Uzantılar** ' ı seçin. Ya da **Görünüm** menüsünde Uzantılar ' ı seçerek uzantılar bölmesini açın.

    ![Visual Studio Code Kaynak Yöneticisi Araçları uzantısı 'nı yükler](./media/resource-manager-tools-vs-code/resource-manager-visual-studio-code-tools-extension.png)
1. **Kaynak Yöneticisi**arayın.
1. **Azure Resource Manager araçları**altında **yüklemeyi** seçin.

## <a name="the-extension-features"></a>Uzantı özellikleri

### <a name="colorization-for-template-language-expressions"></a>Şablon dili Ifadeleri için renklendirme

Parametreler, değişkenler, işlevler, adlar ve ifadeler, aşağıdaki ekran görüntüsünde gösterildiği gibi renk kodludur:

![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları renklendirme](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-colorization.png)

Şablon ana hat görünümü büyük şablonlar arasında gezinmeyi kolaylaştırır.

### <a name="intellisense"></a>IntelliSense

Kaynak Yöneticisi şablon uzantısı, işlev adları, parametreler, değişkenler ve başvurular için olası tamamların olduğunu bilir. IntelliSense önerileri yazarken açılır. Karakter yazmaya devam ederseniz, üye listesi (değişkenler, yöntemler vb.) yalnızca yazılan karakterleri içeren üyeleri içerecek şekilde filtrelenir. **Sekme** veya **ENTER** tuşlarına basmak seçili üyeyi ekler.

- Yerleşik işlev adları

    ![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları IntelliSense işlevleri](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-intellisense-functions.png)

- Parametre başvuruları

    ![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları IntelliSense parametreleri](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-intellisense-parameters.png)

- Değişken başvuruları

    ![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları IntelliSense değişkenleri](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-intellisense-variables.png)

- resourceGroup () özellikleri

    ![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları IntelliSense işlevleri](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-intellisense-resourcegroup.png)

IntelliSense Ayrıca, abonelik () özellikleriyle ve nesneler olan değişkenlere yapılan başvuruların özellikleriyle da kullanılabilir.

### <a name="signature-help-for-function-parameters"></a>İşlev parametreleri için imza yardımı

İşlev adlarının üzerine gelindiğinde, uzantı işlev parametreleri için imza yardımını gösterir.

![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları imza işlevi](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-signature-function.png)

### <a name="go-to-definition-for-variable-and-parameter-references"></a>Değişken ve parametre başvuruları için tanıma git

**CTRL + tıklama**ile tanıma geçebilirsiniz veya ekran görüntüsünde gösterildiği gibi bağlam menüsünü kullanarak: ![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları tanıma git](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-context-menu.png)

Tanımı, **Ctrl + Alt + tıklama**ile yan yana açabilirsiniz.

### <a name="peek-for-variable-and-parameter-definitions"></a>Değişken ve parametre tanımlarına göz atın

Atılamıyor düzenleyicisini açmak için, önceki ekran görüntüsünde gösterildiği gibi bağlam menüsünü kullanın.

Aşağıdaki ekran görüntüsünde, Peek Düzenleyicisi gösterilmektedir:

![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları araç Özeti Düzenleyicisi](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-peek-editor.png)

### <a name="find-all-references-for-variables-and-parameters"></a>Değişkenler ve parametreler için tüm başvuruları bul

Tüm başvuruları bulmak için, önceki ekran görüntüsünde gösterildiği gibi bağlam menüsünü kullanın.

Aşağıdaki ekran görüntüsünde başvuruların nasıl vurgulandığı gösterilmektedir:

![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları tüm başvuruları bul](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-find-all-references.png)

### <a name="rename-all-references-for-variables-and-parameters"></a>Değişkenler ve parametreler için tüm başvuruları yeniden adlandır

Değişkenler ve parametrelerin tüm başvurularını yeniden adlandırmak için, önceki ekran görüntüsünde gösterildiği gibi bağlam menüsünü kullanın.

### <a name="hover-for-parameter-description"></a>Parametre açıklaması için Vurgula

![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları üzerine gelme tanımı](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-hover-parameters.png)

### <a name="brace-matching"></a>Ayraç eşleştirme

Eşleşen köşeli ayraç, imleç bunlardan birinin yakınında olduğu anda vurgulanır. Bir küme ayracına tıkladığınızda, eşleşen küme ayracı de aşağıdaki ekran görüntüsünde gösterildiği gibi vurgulanır:

![Visual Studio Code Kaynak Yöneticisi Araçları uzantıları ayraç eşleştirme](./media/resource-manager-tools-vs-code/resource-manager-tools-extension-brace-matching.png)

### <a name="show-errors-and-warnings"></a>Hataları ve uyarıları göster

Uzantı tarafından tanımlanmakta olan hatalar şunlardır:

- Tanımsız parametre başvuruları
- Tanımsız değişken başvuruları
- Tanınmayan işlev adları
- değişken tanımında başvuru () işlev kullanımı
- İşlevlerde yanlış sayıda bağımsız değişken

Uyarılar şunları içerir:

- Kullanılmayan parametreler
- Kullanılmayan değişkenler

## <a name="next-steps"></a>Sonraki adımlar

- Kaynak Yöneticisi ve Kaynak Yöneticisi şablonları hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager genel bakış](./resource-group-overview.md).
- Visual Studio Code kullanarak hızlı başlangıç üzerinden gezinmek için bkz [. hızlı başlangıç: Visual Studio Code kullanarak Azure Resource Manager şablonları oluşturma](./resource-manager-quickstart-create-templates-use-visual-studio-code.md)
