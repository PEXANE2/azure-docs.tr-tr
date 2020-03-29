---
title: Azure CDN bitiş noktasında varlıkları önceden yükleme | Microsoft Dokümanlar
description: Önbelleğe alınmış içeriği Azure CDN bitiş noktasında önceden yüklemeyi öğrenin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: magattus
ms.openlocfilehash: d91507ad2cb271b23b588ef7da88e6e6712915b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593573"
---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Azure CDN uç noktasında varlıkları önceden yükleme
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Varsayılan olarak, varlıklar yalnızca istendiğinde önbelleğe alınır. Kenar sunucuları içeriği henüz önbelleğe almadığından ve isteği kaynak sunucuya iletmesi gerektiğinden, her bölgeden gelen ilk istek sonraki isteklerden daha uzun sürebilir. Bu ilk isabet gecikmesini önlemek için varlıklarınızı önceden yükleyin. Daha iyi bir müşteri deneyimi sağlamanın yanı sıra, önbelleğe alınmış varlıklarınızın önceden yüklenmesi, kaynak sunucudaki ağ trafiğini azaltabilir.

> [!NOTE]
> Ön yükleme varlıkları, yeni bir film sürümü veya yazılım güncelleştirmesi gibi birçok kullanıcıtarafından aynı anda kullanılabilen büyük olaylar veya içerik için yararlıdır.
> 
> 

Bu öğretici, tüm Azure CDN kenar düğümlerinde önbelleğe alınmış önbelleğe alınmış içerikte size yol gösterin.

## <a name="to-pre-load-assets"></a>Varlıkları önceden yüklemek için
1. Azure [portalında,](https://portal.azure.com)önceden yüklemek istediğiniz bitiş noktasını içeren CDN profiline göz atın. Profil bölmesi açılır.
    
2. Listedeki bitiş noktasını tıklatın. Bitiş noktası bölmesi açılır.
3. CDN bitiş noktası bölmesinden **Yükle'yi**seçin.
   
    ![CDN bitiş noktası bölmesi](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    **Yük** bölmesi açılır.
   
    ![CDN yükleme bölmesi](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. **İçerik yolu**için, yüklemek istediğiniz her varlığın tam yolunu `/pictures/kitten.png`girin (örneğin, ).
   
   > [!TIP]
   > Metin girmeye başladıktan sonra, birden çok varlığın bir listesini oluşturmanıza olanak sağlamak için daha fazla **İçerik yolu** metin kutusu görüntülenir. Varlıkları listeden silmek için elips (...) düğmesini seçin ve ardından **Sil'i**seçin.
   > 
   > Her içerik yolu aşağıdaki [normal ifadelere](/dotnet/standard/base-types/regular-expression-language-quick-reference)uyan göreceli bir URL olmalıdır:  
   > - Tek bir dosya yolu yükleyin:`^(?:\/[a-zA-Z0-9-_.%=\u0020]+)+$`  
   > - Sorgu dizesiyle tek bir dosyayı yükleyin:`^(?:\?[-_a-zA-Z0-9\/%:;=!,.\+'&\u0020]*)?$` 
   > 
   > Her varlığın kendi yolu olması gerektiğinden, önceden yükleme li varlıklar için joker karakter işlevi yoktur.
   > 
   > 
   
    ![Yük düğmesi](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. İçerik yollarına girmeyi bitirdiğinizde **Yükle'yi**seçin.
   

> [!NOTE]
> CDN profili başına dakikada 10 yük isteği sınırı vardır ve aynı anda 50 eşzamanlı yol işlenebilir. Her yolun yol uzunluğu 1024 karakterden oluşan bir sınırı vardır.
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN bitiş noktasını temizleme](cdn-purge-endpoint.md)
* [Azure CDN REST API başvurusu: Bir bitiş noktasında içeriği ön yükleme](https://docs.microsoft.com/rest/api/cdn/endpoints/loadcontent)
* [Azure CDN REST API başvurusu: İçeriği bir uç noktadan temizleme](https://docs.microsoft.com/rest/api/cdn/endpoints/purgecontent)

