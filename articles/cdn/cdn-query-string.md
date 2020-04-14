---
title: Sorgu dizeleri ile Azure CDN önbelleğe alma davranışını denetleme - standart katman
description: Azure CDN sorgu dizesi önbelleğe alma, bir web isteği sorgu dizesi içerdiğinde dosyaların önbelleğe nasıl önbelleğe alınır denetler. Bu makalede, Azure CDN standart ürünlerinde sorgu dize önbelleğe açıklanır.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: allensu
ms.openlocfilehash: a0df9cecc4ccd09db3f6b07fa6fd4c5283753aa2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260216"
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---standard-tier"></a>Sorgu dizeleri ile Azure CDN önbelleğe alma davranışını denetleme - standart katman
> [!div class="op_single_selector"]
> * [Standart katman](cdn-query-string.md)
> * [Premium katman](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Genel Bakış
Azure İçerik Teslim Ağı (CDN) ile, sorgu dizesi içeren bir web isteği için dosyaların önbelleğe nasıl önbelleğe alınış edildiğini denetleyebilirsiniz. Sorgu dizesi olan bir web isteğinde, sorgu dizesi, soru işaretinden (?) sonra oluşan isteğin bu bölümüdür. Sorgu dizesi, alan adı ve değerinin eşit ler işaretiyle (=) ayrıldığı bir veya daha fazla anahtar değer çifti içerebilir. Her anahtar değeri çifti bir ampersand (&) ile ayrılır. Örneğin, http:\//www.contoso.com/content.mov?field1=value1&field2=value2. Bir isteğin sorgu dizesinde birden fazla anahtar değeri çifti varsa, siparişleri önemli değildir. 

> [!IMPORTANT]
> Azure CDN standardı ve premium ürünler aynı sorgu dizesini önbelleğe alma işlevini sağlar, ancak kullanıcı arabirimi farklıdır. Bu makalede, **Microsoft'tan Azure CDN Standard,** **Akamai'den Azure CDN Standardı** ve **Verizon'dan Azure CDN Standard**arabirimi açıklanmaktadır. **Verizon'dan Azure CDN Premium**ile sorgu dize önbelleğe alma için [bkz.](cdn-query-string-premium.md)

Üç sorgu dize modları mevcuttur:

- **Sorgu dizelerini yoksay**: Varsayılan mod. Bu modda, CDN durum noktası (POP) düğümü, sorgu dizelerini ilk istekte istekçiden başlangıç sunucusuna geçirir ve varlığı önbelleğe alır. POP'tan sunulan varlık için sonraki tüm istekler önbelleğe alınan kıymetin süresi dolana kadar sorgu dizelerini yoksa.

- **Sorgu dizeleri için atlama önbelleğe alma**: Bu modda, sorgu dizeleri ile istekleri CDN POP düğümünde önbelleğe alınmaz. POP düğümü varlığı doğrudan kaynak sunucudan alır ve her istekle birlikte istekçiye iletin.

- **Her benzersiz URL'yi önbelleğe alma**: Bu modda, sorgu dizesi de dahil olmak üzere benzersiz bir URL'ye sahip her istek, kendi önbelleğiyle benzersiz bir varlık olarak kabul edilir. Örneğin, örneğin.ashx?q=test1 için bir istek için başlangıç sunucusundan gelen yanıt POP düğümünde önbelleğe alınır ve aynı sorgu dizesiyle sonraki önbellekler için döndürülür. Örneğin.ashx?q=test2 için bir istek, kendi zaman-to-live ayarı ile ayrı bir varlık olarak önbelleğe alınr.
   
    >[!IMPORTANT] 
    > Sorgu dizesi, oturum kimliği veya kullanıcı adı gibi her istekte değişecek parametreler içeriyorsa, bu modu kullanmayın, çünkü bu modu düşük önbellek isabet oranıyla sonuçlanır.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Standart CDN profilleri için sorgu dize önbelleğe alma ayarlarını değiştirme
1. CdN profilini açın ve yönetmek istediğiniz CDN bitiş noktasını seçin.
   
   ![CDN profil uç noktaları](./media/cdn-query-string/cdn-endpoints.png)
   
2. Ayarlar altındaki sol bölmede **Önbelleğe Alma kurallarını**tıklatın.
   
    ![CDN Önbelleğe alma kuralları düğmesi](./media/cdn-query-string/cdn-caching-rules-btn.png)
   
3. Sorgu **dizesi önbelleğe alma davranış** listesinde, sorgu dize modunu seçin ve sonra **Kaydet'i**tıklatın.
   
   ![CDN sorgu dize önbelleğe alma seçenekleri](./media/cdn-query-string/cdn-query-string.png)

> [!IMPORTANT]
> Kaydın Azure CDN'de yayılması zaman aldığından, önbellek dize ayarları değişiklikleri hemen görünmeyebilir:
> - **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle 10 dakikada tamamlanır. 
> - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium** profilleri için yayma işlemi genellikle 10 dakika içinde tamamlanır. 



