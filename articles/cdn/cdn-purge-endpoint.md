---
title: Azure CDN bitiş noktasını temizleme | Microsoft Dokümanlar
description: Önbelleğe alınan tüm içeriği Azure CDN bitiş noktasından nasıl temizleyebilirsiniz öğrenin.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: sohamnchatterjee
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2019
ms.author: allensu
ms.openlocfilehash: ebbb0dd059ce2bcf4a3bc260ed6d426d5be09dfe
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260267"
---
# <a name="purge-an-azure-cdn-endpoint"></a>Azure CDN bitiş noktasını temizleme
## <a name="overview"></a>Genel Bakış
Azure CDN kenar düğümleri, varlığın süresi (TTL) sona erene kadar varlıkları önbelleğe alacak.  Kıymetin TTL'si sona erdikten sonra, istemci kıymeti kenar düğümünden istediğinde, kenar düğümü istemci isteğine hizmet etmek ve önbelleği yenilemek için kıymetin yeni güncelleştirilmiş bir kopyasını alır.

Kullanıcılarınızın varlıklarınızın her zaman en son kopyasını aldığından emin olmak için en iyi yöntem, varlıklarınızı her güncelleştirme için sürüm altına almak ve bunları yeni URL'ler olarak yayımlamaktır.  CDN, sonraki istemci istekleri için yeni varlıkları hemen alır.  Bazen önbelleğe alınan içeriği tüm kenar düğümlerinden temizlemek ve hepsini yeni güncelleştirilmiş varlıkları almaya zorlamak isteyebilirsiniz.  Bunun nedeni, web uygulamanızdaki güncelleştirmeler veya yanlış bilgiler içeren varlıkları hızla güncelleştirmek olabilir.

> [!TIP]
> Temizlemenin yalnızca CDN kenar sunucularında önbelleğe alınmış içeriği temizlediğini unutmayın.  Proxy sunucuları ve yerel tarayıcı önbellekleri gibi alt akış önbellekleri, dosyanın önbelleğe alınmış bir kopyasını saklamaya devam edebilir.  Bir dosyanın yayınlanacak zamanlarını ayarlarken bunu hatırlamak önemlidir.  Bir alt istemciyi, dosyanızı her güncellediğiniz de benzersiz bir ad vererek veya [sorgu dize önbelleğe](cdn-query-string.md)alınmasından yararlanarak dosyanızın en son sürümünü istemeye zorlayabilirsiniz.  
> 
> 

Bu öğretici, bir bitiş noktasının tüm kenar düğümlerinden varlıkları temizleme de size yol eder.

## <a name="walkthrough"></a>Kılavuz
1. Azure [Portalı'nda,](https://portal.azure.com)temizlemek istediğiniz bitiş noktasını içeren CDN profiline göz atın.
2. CDN profil bıçağından temizleme düğmesini tıklatın.
   
    ![CDN profil bıçağı](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    Temizleme bıçağı açılır.
   
    ![CDN temizleme bıçağı](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. Temizleme bıçağında, URL açılır tarafından temizlemek istediğiniz servis adresini seçin.
   
    ![Temizleme formu](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > CdN uç nokta bıçağındaki **Temizleme** düğmesine tıklayarak Temizleme bıçağına da ulaşabilirsiniz.  Bu durumda, **URL** alanı söz konusu bitiş noktasının hizmet adresiyle önceden doldurulur.
   > 
   > 
4. Kenar düğümlerinden temizlemek istediğiniz varlıkları seçin.  Tüm varlıkları temizlemek istiyorsanız, Tüm Onay Kutusunu **Temizleme'yi** tıklatın.  Aksi takdirde, temizlemek istediğiniz her varlığın yolunu **Yol** metin kutusuna yazın. Aşağıdaki biçimler yolda desteklenir.
    1. **Tek URL temizleme**: Dosya uzantısı olan veya olmayan tam URL'yi belirterek tek`/pictures/strasbourg.png`tek varlığı temizleme;`/pictures/strasbourg`
    2. **Joker karakter temizleme**: Yıldız\*işareti ( ) joker karakter olarak kullanılabilir. Tüm klasörleri, alt klasörleri ve dosyaları bir `/*` bitiş noktası altında, yoldaki yle temizleme veya belirli bir klasörün altındaki `/*`tüm alt klasörleri ve dosyaları, örneğin,`/pictures/*`.  Joker karakter temizlemenin şu anda Akamai'den Azure CDN tarafından desteklenmediğini unutmayın. 
    3. **Kök etki alanı temizleme**: Yoldaki "/" ile bitiş noktasının kökünü temizle.
   
   > [!TIP]
   > Yollar temizleme için belirtilmeli ve aşağıdaki [normal ifadeye](/dotnet/standard/base-types/regular-expression-language-quick-reference)uyan göreli bir URL olmalıdır. Şu anda **Akamai'den Azure CDN** tarafından desteklenmeyen tüm ve **Joker karakter temizleme temizleme** yi **temizleyin.**
   > > Tek URL temizleme`@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`  
   > > Sorgu dizesi`@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`  
   > > Joker karakter `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`tasfiyesi. 
   > 
   > Birden çok varlığın bir listesini oluşturmanıza olanak sağlamak için metin girdikten sonra daha fazla **Yol** metin kutusu görüntülenir.  Elips (...) düğmesini tıklayarak varlıkları listeden silebilirsiniz.
   > 
5. Temizleme **düğmesini** tıklatın.
   
    ![Temizleme düğmesi](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Temizleme isteklerinin **Microsoft'tan Azure CDN**ile işleme sayılması yaklaşık 10 dakika, **Verizon'dan Azure CDN** ile yaklaşık 2 dakika (standart ve premium) ve **Akamai'den Azure CDN**ile yaklaşık 10 saniye sürer.  Azure CDN'nin profil düzeyinde herhangi bir zamanda 50 eşzamanlı temizleme isteği sınırı vardır. 
> 
> 

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN uç noktasında varlıkları önceden yükleme](cdn-preload-endpoint.md)
* [Azure CDN REST API başvurusu - Bir Bitiş Noktasını Temizleme veya Ön Yükleme](/rest/api/cdn/endpoints)

