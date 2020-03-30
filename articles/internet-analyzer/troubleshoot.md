---
title: Azure Internet Çözümleyici sorun giderme | Microsoft Dokümanlar
description: Azure Internet Analyzer için sorun giderme başvurusu.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: a265278652c16b4682707470d183a02a55b9a0ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77069226"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Azure Internet Çözümleyici sorun giderme

Bu makalede, sık karşılaşılan Internet Çözümleyici sorunları için sorun giderme adımları içerir.

## <a name="things-to-keep-in-mind"></a>Akılda tutulması gereken noktalar
- İstemci komut dosyası bir **HTTPS** web sitesine katışdırılmış olmalıdır. Komut dosyası düz metin **(http://)** veya yerel **(file://)** web sitesinde çalışırsa ölçümler toplanmaz.
- Ölçüm verileri yalnızca Internet Analyzer profilinin istemci komut dosyası gerçek kullanıcı trafiği alan bir uygulamaya gömülüyse toplanır. Sentetik trafik (örneğin, Azure WebApp Performans Testleri) genellikle gömülü Javascript kodunu yürütmez, bu nedenle bu tür bir trafik tarafından ölçüm oluşturulmaz.

## <a name="azure-portal"></a>Azure portalında
**Karneler bölümünde "Seçili filtre kombinasyonu için bir karne oluşturulmadı"**
- Karneler günlük olarak oluşturulur (her günün sonunda, UTC saati).
- Karneler yalnızca seçilen filtre kombinasyonu (Test, Zaman Dilimi, Ülke, vb.) için 100'den fazla ölçüm toplanmışsa oluşturulur.

**"Toplam Ölçüm Sayısı" bir testteki bir veya her iki uç nokta için sıfırdır**
- Zaman serileri ve ölçüm sayıları saatte bir kez hesaplanır, bu nedenle yeni ölçüm verilerinin gösterilmesi için en az bu kadar süre beklemeniz gerekir.
- Internet Analyzer, analizi için yalnızca başarılı ölçümleri (örneğin, HTTP 200 yanıtları) sayar. Bir testteki bir veya her iki uç nokta erişilemez veya 200 olmayan bir HTTP kodu döndürebilirse, toplam sıfır ölçümle gösterilir.

## <a name="next-steps"></a>Sonraki adımlar
Internet [Analyzer SSS'yi](internet-analyzer-faq.md) okuyun
