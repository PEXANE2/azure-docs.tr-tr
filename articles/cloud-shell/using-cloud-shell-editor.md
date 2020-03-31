---
title: Azure Bulut Shell düzenleyicisini kullanma | Microsoft Dokümanlar
description: Azure Bulut Shell düzenleyicisinin nasıl kullanılacağına genel bakış.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60199231"
---
# <a name="using-the-azure-cloud-shell-editor"></a>Azure Bulut Shell düzenleyicisini kullanma

Azure Cloud Shell, açık kaynak kodlu [Monako Düzenleyicisi](https://github.com/Microsoft/monaco-editor)tarafından oluşturulmuş entegre bir dosya düzenleyicisi içerir. Cloud Shell düzenleyicisi dil vurgulama, komut paleti ve dosya gezgini gibi özellikleri destekler.

![Cloud Shell editörü](media/using-cloud-shell-editor/open-editor.png)

## <a name="opening-the-editor"></a>Editörü açma

Basit dosya oluşturma ve düzenleme işlemleri için Cloud Shell terminalinde `code .` komutunu çalıştırarak düzenleyiciyi başlatabilirsiniz. Bu eylem, terminalde etkin çalışma dizininiz ayarlanmış bir şekilde eylem düzenleyiciyi açar.

Hızlı düzenleme yapmak için dosyayı doğrudan açmak isterseniz, `code <filename>` komutunu çalıştırarak düzenleyiciyi dosya gezgini olmadan açın.

Düzenleyiciyi kullanıcı arabirimi düğmesiyle açmak için araç çubuğunda `{}` düzenleyici simgesine tıklayın. Düzenleyici açılır ve dosya gezgininde `/home/<user>` dizini görüntülenir.

## <a name="closing-the-editor"></a>Editörü kapatma

Düzenleyiciyi kapatmak için, `...` düzenleyicinin sağ üst kısmındaki `Close editor`eylem panelini açın ve seçin.

![Editörü kapat](media/using-cloud-shell-editor/close-editor.png)

## <a name="command-palette"></a>Komut paleti

Komut paletini başlatmak için, odak düzenleyiciye ayarlandığında `F1` anahtarı kullanın. Komut paletinin açılması da eylem paneli aracılığıyla yapılabilir.

![Cmd palet](media/using-cloud-shell-editor/cmd-palette.png)

## <a name="contributing-to-the-monaco-editor"></a>Monako Editörüne Katkıda Bulunmak

Cloud Shell düzenleyicisindeki dil vurgusu desteği, [Monako](https://github.com/Microsoft/monaco-editor)Düzenleyicisi'nin Monarch sözdizimi tanımlarını kullanmasında yukarı akış işlevi aracılığıyla desteklenir. Nasıl katkıda bulunup bulunup katkıda bulunabilirsiniz, [Monako katılımcıkılavuzunu](https://github.com/Microsoft/monaco-editor/blob/master/CONTRIBUTING.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar
[Bash in Cloud Shell](quickstart.md)
için hızlı başlangıcı deneyin[Entegre Cloud Shell araçlarının tam listesini görüntüleyin](features.md)