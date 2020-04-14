---
title: Sorgu dizeleri ile Azure CDN önbelleğe alma davranışını denetleme - premium katman
description: Azure CDN sorgu dizesi önbelleğe alma, bir web isteği sorgu dizesi içerdiğinde dosyaların önbelleğe nasıl önbelleğe alınır denetler. Bu makalede, Verizon ürününden Azure CDN Premium'da sorgu dizesi önbelleğe alma açıklanmaktadır.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: 99db4a85-4f5f-431f-ac3a-69e05518c997
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: allensu
ms.openlocfilehash: 94949a31db5321929a3440281cebd01712c79bb8
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260148"
---
# <a name="control-azure-cdn-caching-behavior-with-query-strings---premium-tier"></a>Sorgu dizeleri ile Azure CDN önbelleğe alma davranışını denetleme - premium katman
> [!div class="op_single_selector"]
> * [Standart katman](cdn-query-string.md)
> * [Premium katman](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Genel Bakış
Azure İçerik Teslim Ağı (CDN) ile, sorgu dizesi içeren bir web isteği için dosyaların önbelleğe nasıl önbelleğe alınış edildiğini denetleyebilirsiniz. Sorgu dizesi olan bir web isteğinde, sorgu dizesi, soru işaretinden (?) sonra oluşan isteğin bu bölümüdür. Sorgu dizesi, alan adı ve değerinin eşit ler işaretiyle (=) ayrıldığı bir veya daha fazla anahtar değer çifti içerebilir. Her anahtar değeri çifti bir ampersand (&) ile ayrılır. Örneğin, http:\//www.contoso.com/content.mov?field1=value1&field2=value2. Bir isteğin sorgu dizesinde birden fazla anahtar değeri çifti varsa, siparişleri önemli değildir. 

> [!IMPORTANT]
> Standart ve premium CDN ürünleri aynı sorgu dize önbelleğe alma işlevselliği sağlar, ancak kullanıcı arabirimi farklıdır. Bu **makalede, Verizon Azure CDN Premium**için arayüz açıklanmaktadır. Azure CDN standart ürünleriyle sorgu dize önbelleğe alma için sorgu [dizeleri ile Azure CDN önbelleğe alma davranışına](cdn-query-string.md)bakın - standart katman.
>


Üç sorgu dize modları mevcuttur:

- **standart önbellek**: Varsayılan mod. Bu modda, CDN durum noktası (POP) düğümü, sorgu dizelerini ilk istekte istekçiden başlangıç sunucusuna geçirir ve varlığı önbelleğe alır. POP sunucusundan sunulan varlık için sonraki tüm istekler önbelleğe alınan varlık süresi dolana kadar sorgu dizelerini yoksa.

    >[!IMPORTANT] 
    > Bu hesaptaki herhangi bir yol için belirteç yetkilendirmesi etkinleştirilirse, standart önbellek modu kullanılabilecek tek moddur. 

- **önbellek yok**: Bu modda, sorgu dizeleri olan istekler CDN POP düğümünde önbelleğe alınmaz. POP düğümü varlığı doğrudan kaynak sunucudan alır ve her istekle birlikte istekçiye iletin.

- **benzersiz önbellek**: Bu modda, sorgu dizesi de dahil olmak üzere benzersiz bir URL'ye sahip her istek, kendi önbelleğiyle benzersiz bir varlık olarak kabul edilir. Örneğin, örneğin.ashx?q=test1 için bir istek için başlangıç sunucusundan gelen yanıt POP düğümünde önbelleğe alınır ve aynı sorgu dizesiyle sonraki önbellekler için döndürülür. Örneğin.ashx?q=test2 için bir istek, kendi zaman-to-live ayarı ile ayrı bir varlık olarak önbelleğe alınr.
   
    >[!IMPORTANT] 
    > Sorgu dizesi, oturum kimliği veya kullanıcı adı gibi her istekte değişecek parametreler içeriyorsa, bu modu kullanmayın, çünkü bu modu düşük önbellek isabet oranıyla sonuçlanır.

## <a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Premium CDN profilleri için sorgu dize önbelleğe alma ayarlarını değiştirme
1. CDN profilini açın ve **ardından Yönet'i**tıklatın.
   
    ![CDN profili Yönet düğmesi](./media/cdn-query-string-premium/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
2. **HTTP Büyük** sekmesinin üzerine gidin, ardından **Önbellek Ayarları** uçuş menüsünün üzerinde kalın. **Sorgu-String Önbelleğe'yi**tıklatın.
   
    Sorgu dize önbelleğe alma seçenekleri görüntülenir.
   
    ![CDN sorgu dize önbelleğe alma seçenekleri](./media/cdn-query-string-premium/cdn-query-string.png)
3. Sorgu dize modunu seçin ve ardından **Güncelleştir'i**tıklatın.

> [!IMPORTANT]
> Kaydın CDN'de yayılması zaman aldığından, önbellek dize ayarları değişiklikleri hemen görünmeyebilir. Yayılma genellikle 10 dakika içinde tamamlar.
 

