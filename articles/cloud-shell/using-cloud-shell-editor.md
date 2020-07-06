---
title: Azure Cloud Shell düzenleyicisini kullanma | Microsoft Docs
description: Azure Cloud Shell düzenleyicisini kullanma konusuna genel bakış.
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
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: 7f597bb5cba1a12bdb93325fe2b877ffc644e3e4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "60199231"
---
# <a name="using-the-azure-cloud-shell-editor"></a>Azure Cloud Shell düzenleyicisini kullanma

Azure Cloud Shell, açık kaynak [Monako düzenleyiciden](https://github.com/Microsoft/monaco-editor)oluşturulan tümleşik bir dosya düzenleyicisini içerir. Cloud Shell düzenleyicisi dil vurgulama, komut paleti ve dosya gezgini gibi özellikleri destekler.

![Cloud Shell Düzenleyicisi](media/using-cloud-shell-editor/open-editor.png)

## <a name="opening-the-editor"></a>Düzenleyiciyi açma

Basit dosya oluşturma ve düzenleme işlemleri için Cloud Shell terminalinde `code .` komutunu çalıştırarak düzenleyiciyi başlatabilirsiniz. Bu eylem, terminalde etkin çalışma dizininiz ayarlanmış bir şekilde eylem düzenleyiciyi açar.

Hızlı düzenleme yapmak için dosyayı doğrudan açmak isterseniz, `code <filename>` komutunu çalıştırarak düzenleyiciyi dosya gezgini olmadan açın.

Düzenleyiciyi kullanıcı arabirimi düğmesiyle açmak için araç çubuğunda `{}` düzenleyici simgesine tıklayın. Düzenleyici açılır ve dosya gezgininde `/home/<user>` dizini görüntülenir.

## <a name="closing-the-editor"></a>Düzenleyiciyi kapatma

Düzenleyiciyi kapatmak için, `...` düzenleyicinin sağ üst köşesindeki eylem panelini açın ve öğesini seçin `Close editor` .

![Düzenleyiciyi Kapat](media/using-cloud-shell-editor/close-editor.png)

## <a name="command-palette"></a>Komut paleti

Komut paletini başlatmak için, `F1` düzenleyicide odak ayarlandığında anahtarı kullanın. Komut paletini açmak, eylem panelinden de yapılabilir.

![Cmd paleti](media/using-cloud-shell-editor/cmd-palette.png)

## <a name="contributing-to-the-monaco-editor"></a>Monako düzenleyicisine katkıda bulunma

Cloud Shell düzenleyicisinde dil vurgulama desteği, [Monako Düzenleyicisi](https://github.com/Microsoft/monaco-editor)'nin Monarch sözdizimi tanımlarının kullanımıyla olan yukarı akış işlevselliği aracılığıyla desteklenir. Katkı yapmayı öğrenmek için, [Monako katkıda bulunan kılavuzunu](https://github.com/Microsoft/monaco-editor/blob/master/CONTRIBUTING.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar
Cloud Shell Bash [için hızlı başlangıcı deneyin](quickstart.md) 
 [Tümleşik Cloud Shell araçlarının tam listesini görüntüleyin](features.md)