---
title: Azure CDN'deki dosyaları sıkıştırarak performansı artırın | Microsoft Dokümanlar
description: Dosyalarınızı Azure CDN'de sıkıştırarak dosya aktarım hızını nasıl artıracağınızı ve sayfa yükleme performansını nasıl artıracağınızı öğrenin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: magattus
ms.openlocfilehash: 9f185f58e1d33a3985777cb22bc7578f9f2c4541
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593794"
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Azure CDN’de dosyaları sıkıştırarak performansı geliştirme
Dosya sıkıştırma, dosyanın sunucudan gönderilmeden önce boyutunu azaltarak dosya aktarım hızını artırmak ve sayfa yükleme performansını artırmak için basit ve etkili bir yöntemdir. Dosya sıkıştırma bant genişliği maliyetlerini azaltabilir ve kullanıcılarınız için daha duyarlı bir deneyim sağlar.

Dosya sıkıştırmayı etkinleştirmenin iki yolu vardır:

- Başlangıç sunucunuzda sıkıştırmayı etkinleştirin. Bu durumda, Azure CDN sıkıştırılmış dosyaları boyunca geçirir ve bunları isteyen istemcilere sunar.
- CdN POP sunucularında doğrudan sıkıştırmayı*etkinleştirin (anında sıkıştırma).* Bu durumda, CDN dosyaları sıkıştırır ve kaynak sunucusu tarafından sıkıştırılmamış olsalar bile son kullanıcılara hizmet eder.

> [!IMPORTANT]
> Azure CDN yapılandırma değişikliklerinin ağ da yayılması biraz zaman alabilir: 
> - **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle 10 dakikada tamamlanır. 
> - **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
> - **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium** profilleri için yayma işlemi genellikle 10 dakika içinde tamamlanır. 
> 
> CDN bitiş noktanız için sıkıştırmayı ilk kez ayarlıyorsanız, sıkıştırma ayarlarının PS'lere yaydığından emin olmak için sorun giderme işleminden önce 1-2 saat beklemeyi düşünün.

## <a name="enabling-compression"></a>Sıkıştırmayı etkinleştirme
Standart ve premium CDN katmanları aynı sıkıştırma işlevini sağlar, ancak kullanıcı arabirimi farklıdır. Standart ve premium CDN katmanları arasındaki farklar hakkında daha fazla bilgi için [Azure CDN Genel Bakış'a](cdn-overview.md)bakın.

### <a name="standard-cdn-profiles"></a>Standart CDN profilleri 
> [!NOTE]
> Bu bölüm **Microsoft'tan Azure CDN Standardı, Verizon'dan** **Azure CDN Standardı**ve **Akamai profillerinden Azure CDN Standardı** için geçerlidir.
> 
> 

1. CDN profil sayfasından yönetmek istediğiniz CDN bitiş noktasını seçin.

    ![CDN profil uç noktaları](./media/cdn-file-compression/cdn-endpoints.png)

    CDN bitiş noktası sayfası açılır.
2. **Sıkıştırma'yı**seçin.

    ![CDN sıkıştırma seçimi](./media/cdn-file-compression/cdn-compress-select-std.png)

    Sıkıştırma sayfası açılır.
3. Sıkıştırmayı açmak için **Aç'ı** seçin.

    ![CDN dosya sıkıştırma seçenekleri](./media/cdn-file-compression/cdn-compress-standard.png)
4. Varsayılan MIME türlerini kullanın veya MIME türlerini ekleyerek veya kaldırarak listeyi değiştirin.

   > [!TIP]
   > Mümkün olsa da, sıkıştırılmış biçimlere sıkıştırma uygulanması önerilmez. Örneğin, ZIP, MP3, MP4 veya JPG.
   > 

   > [!NOTE]
   > VARSAYıLAN MIME türleri listesini değiştirmek şu anda Microsoft'tan Azure CDN Standardı'nda desteklenmez.
   > 

5. Değişikliklerinizi yaptıktan sonra **Kaydet'i**seçin.

### <a name="premium-cdn-profiles"></a>Premium CDN profilleri
> [!NOTE]
> Bu bölüm yalnızca **Verizon profillerinden Azure CDN Premium** için geçerlidir.
> 

1. CDN profil sayfasından **Yönet'i**seçin.

    ![CDN Seç'i yönet](./media/cdn-file-compression/cdn-manage-btn.png)

    CDN yönetim portalı açılır.
2. **HTTP Büyük** sekmesinin üzerine gidin, ardından **Önbellek Ayarları'nın** üzerinde gezinin. **Sıkıştırma'yı**seçin.

    ![CDN sıkıştırma seçimi](./media/cdn-file-compression/cdn-compress-select.png)

    Sıkıştırma seçenekleri görüntülenir.

    ![CDN dosya sıkıştırma seçenekleri](./media/cdn-file-compression/cdn-compress-files.png)
3. **Sıkıştırma Etkin'i**seçerek sıkıştırmayı etkinleştirin. **Dosya Türleri** kutusuna virgülle sınırlandırılmış liste (boşluk yok) olarak sıkıştırmak istediğiniz MIME türlerini girin.

   > [!TIP]
   > Mümkün olsa da, sıkıştırılmış biçimlere sıkıştırma uygulanması önerilmez. Örneğin, ZIP, MP3, MP4 veya JPG.
   > 

4. Değişikliklerinizi yaptıktan sonra **Güncelleştir'i**seçin.

## <a name="compression-rules"></a>Sıkıştırma kuralları

### <a name="azure-cdn-standard-from-microsoft-profiles"></a>Microsoft profillerinden Azure CDN Standardı

**Microsoft profillerinden Azure CDN Standardı** için yalnızca uygun dosyalar sıkıştırılır. Sıkıştırma için uygun olmak için bir dosya gerekir:
- [Sıkıştırma için yapılandırılan](#enabling-compression)bir MIME türünden olun.
- 1 KB'den büyük olması
- 8 MB'dan küçük olması

Bu profiller aşağıdaki sıkıştırma kodlamalarını destekler:
- gzip (GNU zip)
- brotli 

İstek birden fazla sıkıştırma türünü destekliyorsa, brotli sıkıştırma önceliklidir.

Bir varlık için bir istek gzip sıkıştırma belirtir ve bir önbellek özledim istek sonuçları, Azure CDN doğrudan POP sunucusunda varlığın gzip sıkıştırma gerçekleştirir. Daha sonra sıkıştırılmış dosya önbellekten servis edilir.

### <a name="azure-cdn-from-verizon-profiles"></a>Verizon profillerinden Azure CDN

**Verizon profillerinden Azure CDN Standardı** ve Verizon **profillerinden Azure CDN Premium** için yalnızca uygun dosyalar sıkıştırılır. Sıkıştırma için uygun olmak için bir dosya gerekir:
- 128 bayttan daha büyük olmalı
- 3 MB'dan küçük olması

Bu profiller aşağıdaki sıkıştırma kodlamalarını destekler:
- gzip (GNU zip)
- Deflate
- bzip2
- brotli 

İstek birden fazla sıkıştırma türünü destekliyorsa, bu sıkıştırma türleri brotli sıkıştırmadan önceliklidir.

Bir varlık için istek brotli sıkıştırma (HTTP üstbilgi) `Accept-Encoding: br`belirtir ve istek bir önbellek özledim sonuçlanır, Azure CDN doğrudan POP sunucusunda varlığın brotli sıkıştırma gerçekleştirir. Daha sonra sıkıştırılmış dosya önbellekten servis edilir.

### <a name="azure-cdn-standard-from-akamai-profiles"></a>Akamai profillerinden Azure CDN Standardı

**Akamai profillerinden Azure CDN Standardı** için tüm dosyalar sıkıştırma için uygundur. Ancak, bir dosya [sıkıştırma için yapılandırılan](#enabling-compression)bir MIME türünde olmalıdır.

Bu profiller yalnızca gzip sıkıştırma kodlamayı destekler. Bir profil bitiş noktası gzip kodlanmış bir dosya istediğinde, istemci isteğine bakılmaksızın her zaman kaynaktan istenir. 

## <a name="compression-behavior-tables"></a>Sıkıştırma davranış tabloları
Aşağıdaki tablolar her senaryo için Azure CDN sıkıştırma davranışını açıklar:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Sıkıştırma devre dışı bırakılır veya dosya sıkıştırma için uygun değildir
| İstemci tarafından istenen biçim (Kabul Etme-Kodlama üstbilgisi aracılığıyla) | Önbelleğe alınmış dosya biçimi | İstemciye CDN yanıtı | &nbsp; &nbsp; Notlar&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
| --- | --- | --- | --- |
| Sıkıştırılmış |Sıkıştırılmış |Sıkıştırılmış | |
| Sıkıştırılmış |Sıkıştırılmamış |Sıkıştırılmamış | |
| Sıkıştırılmış |Önbelleğe alınmamış |Sıkıştırılmış veya Sıkıştırılmamış |Başlangıç yanıtı, CDN'nin sıkıştırma yapıp yapmayacağını belirler. |
| Sıkıştırılmamış |Sıkıştırılmış |Sıkıştırılmamış | |
| Sıkıştırılmamış |Sıkıştırılmamış |Sıkıştırılmamış | |
| Sıkıştırılmamış |Önbelleğe alınmamış |Sıkıştırılmamış | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Sıkıştırma etkindir ve dosya sıkıştırma için uygundur
| İstemci tarafından istenen biçim (Kabul Etme-Kodlama üstbilgisi aracılığıyla) | Önbelleğe alınmış dosya biçimi | İstemciye CDN yanıtı | Notlar |
| --- | --- | --- | --- |
| Sıkıştırılmış |Sıkıştırılmış |Sıkıştırılmış |Desteklenen biçimler arasındaki CDN transkodları. |
| Sıkıştırılmış |Sıkıştırılmamış |Sıkıştırılmış |CDN bir sıkıştırma gerçekleştirir. |
| Sıkıştırılmış |Önbelleğe alınmamış |Sıkıştırılmış |Kaynak sıkıştırılmamış bir dosyayı döndürürse CDN sıkıştırma gerçekleştirir. <br/>**Verizon'daki Azure CDN,** sıkıştırılmamış dosyayı ilk istekte geçirir ve ardından sonraki istekler için dosyayı sıkıştırır ve önbelleğe alır. <br/>Üstbilgiiçeren `Cache-Control: no-cache` dosyalar asla sıkıştırılmadı. |
| Sıkıştırılmamış |Sıkıştırılmış |Sıkıştırılmamış |CDN bir dekompresyon gerçekleştirir. |
| Sıkıştırılmamış |Sıkıştırılmamış |Sıkıştırılmamış | |
| Sıkıştırılmamış |Önbelleğe alınmamış |Sıkıştırılmamış | |

## <a name="media-services-cdn-compression"></a>Medya Hizmetleri CDN Sıkıştırma
Medya Hizmetleri CDN akışı için etkin olan uç noktalar için sıkıştırma varsayılan olarak aşağıdaki MIME türleri için etkinleştirilir: 
- uygulama/vnd.ms-sstr+xml 
- uygulama/dash+xml
- uygulama/vnd.apple.mpegurl
- uygulama/f4m+xml 

## <a name="see-also"></a>Ayrıca bkz.
* [CDN dosya sıkıştırma sorunlarını giderme](cdn-troubleshoot-compression.md)    

