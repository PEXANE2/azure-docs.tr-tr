---
title: Azure Bulut Kabuğu penceresini kullanma | Microsoft Dokümanlar
description: Azure Bulut Bulutu penceresinin nasıl kullanılacağına genel bakış.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/15/2019
ms.author: damaerte
ms.openlocfilehash: 3435d958049012750e775e2a17f43f463a0cc358
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70860311"
---
# <a name="using-the-azure-cloud-shell-window"></a>Azure Bulut BulutU penceresini kullanma

Bu belge, Bulut Bulut penceresinin nasıl kullanılacağını açıklar.

## <a name="swap-between-bash-and-powershell-environments"></a>Bash ve PowerShell ortamları arasında takas

Bash ve PowerShell ortamları arasında değiş tokuş yapmak için Cloud Shell araç çubuğundaki ortam seçicisini kullanın.  
![Ortam seçme](media/using-the-shell-window/env-selector.png)

## <a name="restart-cloud-shell"></a>Bulut Kabuğunu Yeniden Başlat
Makine durumunu sıfırlamak için Bulut Kabuğu araç çubuğundaki yeniden başlat simgesini tıklatın.  
![Bulut Kabuğunu Yeniden Başlat](media/using-the-shell-window/restart.png)
> [!WARNING]
> Cloud Shell'i yeniden başlatmak makine durumunu sıfırlar ve Azure dosya paylaşımınız tarafından kalıcı olmayan dosyalar kaybolur.

## <a name="change-the-text-size"></a>Metin boyutunu değiştirme
Pencerenin sol üst kısmındaki ayarlar simgesine tıklayın, ardından "Metin boyutu" seçeneğinin üzerine gidin ve istediğiniz metin boyutunu seçin. Seçiminiz oturumlar arasında devam edecektir.
![Metin boyutu](media/using-the-shell-window/text-size.png)

## <a name="change-the-font"></a>Yazı tipini değiştirme
Pencerenin sol üst kısmındaki ayarlar simgesine tıklayın, ardından "Font" seçeneğinin üzerine gidin ve istediğiniz yazı tipini seçin.  Seçiminiz oturumlar arasında devam edecektir.
![Yazı tipi](media/using-the-shell-window/text-font.png)

## <a name="upload-and-download-files"></a>Dosyaları yükleme ve indirme
Pencerenin sol üst kısmındaki dosya yükle/indir simgesine tıklayın ve ardından yükleme veya indirme seçeneğini belirleyin.  
![Dosyaları yükleme/indirme](media/using-the-shell-window/uploaddownload.png)
* Dosya yüklemek için açılan pencereyi kullanarak yerel bilgisayarınızdaki dosyaya göz atın, istediğiniz dosyayı seçin ve "Aç" düğmesini tıklayın.  Dosya dizine `/home/user` yüklenir.
* Dosyayı indirmek için, açılır pencereye tam nitelikli dosya yolunu girin (yani, temelde `/home/user` varsayılan olarak dizinin altında görünen bir yol) ve "İndir" düğmesini seçin.  
> [!NOTE] 
> Dosyalar ve dosya yolları Cloud Shell'de büyük/küçük harf duyarlıdır. Dosya yolunızdaki kılıfınızı iki kez kontrol edin.

## <a name="open-another-cloud-shell-window"></a>Başka bir Bulut Kabuğu penceresi açma
Bulut Kabuğu, her oturumun ayrı bir işlem olarak var olmasına izin vererek tarayıcı sekmeleri arasında birden çok eşzamanlı oturum sağlar.
Oturumdan çıkıyorsanız, her işlem aynı makinede çalışmasına rağmen bağımsız olarak çalıştığından her oturum penceresinden çıktığınızdan emin olun.  
Pencerenin sol üst kısmındaki yeni oturumu aç simgesini tıklatın. Varolan kapsayıcıya bağlı başka bir oturumla yeni bir sekme açılır.
![Yeni oturumu aç](media/using-the-shell-window/newsession.png)

## <a name="cloud-shell-editor"></a>Cloud Shell editörü
* [Azure Bulut BulutU Düzenleyicisini Kullanma](using-cloud-shell-editor.md) sayfasına bakın.

## <a name="web-preview"></a>Web önizleme
Pencerenin sol üst kısmındaki web önizleme simgesine tıklayın, "Yapılandır" seçeneğini belirleyin, açmak için istediğiniz bağlantı noktasını belirtin.  Yalnızca bağlantı noktasını açmak için "Bağlantı noktasını aç"ı veya bağlantı noktasını açmak ve yeni bir sekmede bağlantı noktasını önizlemek için "Aç ve göz at" seçeneğini belirleyin.  
![Web önizleme](media/using-the-shell-window/preview.png)  
<br>
![Bağlantı noktasını yapılandırma](media/using-the-shell-window/preview-configure.png)  
Pencerenin sol üst kısmındaki web önizleme simgesine tıklayın, "Önizleme bağlantı noktasını..." seçeneğini belirleyin. yeni bir sekmede açık bir bağlantı noktasını önizlemek için. Pencerenin sol üst kısmındaki web önizleme simgesini tıklatın, "Bağlantı noktasını kapat..." seçeneğini belirleyin. açık bağlantı noktasını kapatmak için.  
![Önizleme/kapatma bağlantı noktası](media/using-the-shell-window/preview-options.png)

## <a name="minimize--maximize-cloud-shell-window"></a>Bulut Kabuğu pencereyi en üst düzeye & en üst düzeye çıkarın
Gizlemek için pencerenin sağ üst kısmındaki simgeyi tıklatın. Gizlemek için Bulut Kabuğu simgesini yeniden tıklatın.
Pencereyi maksimum yüksekliğe ayarlamak için en üst düzeye çıkarma simgesini tıklatın. Pencereyi önceki boyuta geri yüklemek için geri yükleme'yi tıklatın.  
![Pencereyi en aza indirin veya en üst düzeye çıkarın](media/using-the-shell-window/minmax.png)

## <a name="copy-and-paste"></a>Kopyala ve yapıştır
[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Bulut Kabuğunu Yeniden Boyutlandırma penceresi
Bulut Kabuğu penceresini yeniden boyutlandırmak için araç çubuğunun üst kenarını yukarı veya aşağı tıklatın.

## <a name="scrolling-text-display"></a>Metin ekranında kaydırma
Terminal metnini taşımak için fareniz veya dokunmatik yüzeyinizle kaydırın.

## <a name="exit-command"></a>Çıkış komutu
Çalışan `exit` etkin oturumu sona erdirir. Bu davranış, etkileşim olmadan 20 dakika sonra varsayılan olarak oluşur.

## <a name="next-steps"></a>Sonraki adımlar

[Bulut Kabuk Quickstart Bash](quickstart.md) <br>
[PowerShell Bulut Shell Quickstart](quickstart-powershell.md)
