---
title: Azure Depolama Gezgini erişilebilirliği | Microsoft Docs
description: Azure Depolama Gezgini erişilebilirliği anlayın. Hangi ekran okuyucularının kullanılabildiğini, yakınlaştırma özelliğini, yüksek karşıtlık temalarını ve kısayol tuşlarını inceleyin.
services: storage
documentationcenter: na
author: MrayermannMSFT
manager: jinglouMSFT
editor: ''
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2018
ms.author: marayerm
ms.openlocfilehash: ca4a8d719277eaa1d853d53d282649f839256be9
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88035494"
---
# <a name="storage-explorer-accessibility"></a>Depolama Gezgini Erişilebilirliği

## <a name="screen-readers"></a>Ekran okuyucular

Depolama Gezgini, Windows ve Mac üzerinde ekran okuyucu kullanımını destekler. Her platform için aşağıdaki ekran okuyucular önerilir:

Platform | Ekran okuyucu
---------|--------------
Windows  | NVDA
Mac      | Seslendirme
Linux    | (ekran okuyucular Linux üzerinde desteklenmez)

Depolama Gezgini kullanırken bir erişilebilirlik sorunuyla karşılaşırsanız lütfen [GitHub 'da bir sorun açın](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="zoom"></a>Zoom

Yakınlaştırma aracılığıyla Depolama Gezgini metnin daha büyük olmasını sağlayabilirsiniz. Yakınlaştırmak için Yardım menüsünde **Yakınlaştır** ' a tıklayın. Ayrıca, yakınlaştırma düzeyini uzaklaştırmak ve varsayılan düzeye döndürmek için Yardım menüsünü de kullanabilirsiniz.

![Yardım menüsündeki yakınlaştırma seçenekleri][0]

Yakınlaştırma ayarı, çoğu UI öğesinin boyutunu artırır. Tüm Kullanıcı arabirimi öğelerinin doğru ölçeklenmesini sağlamak için işletim sistemi için büyük metin ve yakınlaştırma ayarlarını da etkinleştirmeniz önerilir.

## <a name="high-contrast-themes"></a>Yüksek Karşıtlık Temaları

Depolama Gezgini iki yüksek karşıtlık teması, **yüksek karşıtlık ışığı** ve **koyu yüksek karşıtlık**. Yardım > Temalar menüsünden ' i seçerek temanızı değiştirebilirsiniz.

![Temalar alt menüsü][1]

Tema ayarı, çoğu UI öğesinin rengini değiştirir. Tüm Kullanıcı arabirimi öğelerinin düzgün şekilde renklendirildiğinden emin olmak için işletim sisteminin eşleşen Yüksek Karşıtlık temasını da etkinleştirmeniz önerilir.

## <a name="shortcut-keys"></a>Kısayol Tuşları

### <a name="window-commands"></a>Pencere komutları

Komut       | Klavye kısayolu
--------------|--------------------
Yeni pencere    | **Denetim + SHIFT + N**
Düzenleyiciyi Kapat  | **Denetim + F4**
Çık          | **Denetim + SHIFT + W**

### <a name="navigation-commands"></a>Gezinti komutları

Komut                | Klavye kısayolu
-----------------------|----------------------
Sonraki panele odaklan       | **F6**
Odağı önceki panel   | **Shift+F6**
Gezgin               | **Denetim + SHIFT + E**
Hesap Yönetimi     | **Denetim + SHIFT + A**
Yan çubuğu aç        | **Denetim + B**
Etkinlik Günlüğü           | **Denetim + SHIFT + L**
Eylemler ve Özellikler | **Denetim + SHIFT + P**
Geçerli düzenleyici         | **Denetim + giriş**
Sonraki düzenleyici            | **Denetim + sayfa aşağı**
Önceki düzenleyici        | **Denetim + sayfa yukarı**

### <a name="zoom-commands"></a>Yakınlaştırma komutları

Komut  | Klavye kısayolu
---------|------------------
Yakınlaştır  | **Denetim + =**
Uzaklaştır | **Denetim +-**

### <a name="blob-and-file-share-editor-commands"></a>Blob ve dosya paylaşma Düzenleyicisi komutları

Komut | Klavye kısayolu
--------|--------------------
Geri    | **Alt+Sol Ok**
İleri | **Alt+Sağ Ok**
Yukarı      | **Alt+Yukarı Ok**

### <a name="editor-commands"></a>Düzenleyici komutları

Komut | Klavye kısayolu
--------|------------------
Kopyala    | **Denetim + C**
Kes     | **Denetim + X**
Yapıştır   | **Denetim + V**
Yenile  | **Denetim + R**

### <a name="other-commands"></a>Diğer komutlar

Komut                | Klavye kısayolu
-----------------------|------------------
Geliştirici Araçları değiştirme | **F12**
Yeniden yükle                 | **Alt + Control + R**

[0]: ./media/vs-azure-tools-storage-explorer-accessibility/Zoom.png
[1]: ./media/vs-azure-tools-storage-explorer-accessibility/HighContrast.png
