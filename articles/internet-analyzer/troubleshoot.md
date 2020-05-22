---
title: Azure Internet Analyzer sorunlarını giderme | Microsoft Docs
description: Azure Internet Çözümleyicisi için sorun giderme başvurusu.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 12/04/2019
ms.author: dibotero
ms.openlocfilehash: 0c9e203d7e8be7b24c711f73e2152a7745a57dac
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745480"
---
# <a name="azure-internet-analyzer-troubleshooting"></a>Azure Internet Çözümleyicisi sorunlarını giderme

Bu makale, yaygın Internet çözümleyici sorunları için sorun giderme adımlarını içerir.

## <a name="things-to-keep-in-mind"></a>Akılda tutulması gereken noktalar
- İstemci betiği bir **https** Web sitesine katıştırmalıdır. Betik bir düz metin (**http://**) veya yerel (**File://**) Web sitesinde çalışıyorsa ölçümler toplanmaz.
- Ölçüm verileri yalnızca Internet Çözümleyicisi profilinin istemci betiği gerçek Kullanıcı trafiği alan bir uygulamaya katıştırılmışsa toplanacaktır. Yapay trafik (örneğin, Azure WebApp performans testleri) genellikle katıştırılmış JavaScript kodunu yürütmez, bu nedenle bu tür trafik tarafından hiçbir ölçüm oluşturulmaz.

## <a name="azure-portal"></a>Azure portal
**"Karneler bölümünde seçili filtre birleşimi için bir karne üretilmemiştir"**
- Karneler günlük olarak (her günün sonunda UTC saati) oluşturulur.
- Karneler yalnızca seçili filtre birleşimi (test, zaman aralığı, ülke/bölge vb.) için 100 ' den fazla ölçüm toplanmışsa üretilir.

**Testteki bir veya her iki uç nokta için "Toplam ölçüm sayısı" sıfır**
- Zaman serisi ve ölçüm sayıları bir saat olarak hesaplanır, bu nedenle yeni ölçüm verilerinin görünmesi için en az bu süreyi beklemeniz gerekir.
- Internet çözümleyici, analizi için yalnızca başarılı ölçümleri (yani, HTTP 200 yanıtlarını) sayar. Testteki bir veya her iki uç noktanın ulaşılamaz veya 200 olmayan bir HTTP Kodu döndürmesi durumunda, sıfır toplam ölçümle gösterilir.

## <a name="next-steps"></a>Sonraki adımlar
[Internet ÇÖZÜMLEYICISI SSS](internet-analyzer-faq.md) makalesini okuyun
