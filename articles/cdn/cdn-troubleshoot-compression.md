---
title: Azure CDN'de sorun giderme dosya sıkıştırma | Microsoft Dokümanlar
description: Azure CDN dosya sıkıştırma ile ilgili sorunları giderin.
services: cdn
documentationcenter: ''
author: sohamnc
manager: danielgi
editor: ''
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: aff2dadee365fcdc7e14070714aa1d2cbba901ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476432"
---
# <a name="troubleshooting-cdn-file-compression"></a>CDN dosya sıkıştırma sorunlarını giderme
Bu makale, [CDN dosya sıkıştırma](cdn-improve-performance.md)ile sorunları gidermek yardımcı olur.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı da dosyalayabilirsiniz. [Azure Destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**tıklatın.

## <a name="symptom"></a>Belirti
Bitiş noktanız için sıkıştırma etkindir, ancak dosyalar sıkıştırılmamış olarak döndürülür.

> [!TIP]
> Dosyalarınızın sıkıştırılmış olarak döndürülüp döndürülmediğini kontrol etmek için [Fiddler](https://www.telerik.com/fiddler) veya tarayıcınızın [geliştirici araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)gibi bir araç kullanmanız gerekir.  Önbelleğe alınmış CDN içeriğinizle döndürülen HTTP yanıt üstbilgilerini kontrol edin.  `Content-Encoding` **Gzip**, **bzip2**veya **söndürme**değeri ile adlı bir üstbilgi varsa, içeriğiniz sıkıştırılır.
> 
> ![İçerik Kodlama üstbilgi](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a>Nedeni
Çeşitli olası nedenleri vardır, dahil:

* İstenen içerik sıkıştırma için uygun değildir.
* İstenen dosya türü için sıkıştırma etkinleştirildi.
* HTTP isteği, geçerli bir sıkıştırma türü isteyen bir üstbilgi içermiş.
* Origin yığınlı içerik gönderiyor.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
> [!TIP]
> Yeni uç noktaları dağıtmada olduğu gibi, CDN yapılandırma değişikliklerinin ağ da yayılması biraz zaman alır.  Genellikle, değişiklikler 90 dakika içinde uygulanır.  CDN bitiş noktanız için sıkıştırmayı ilk kez ayarladıysanız, sıkıştırma ayarlarının PS'lere yayDığından emin olmak için 1-2 saat beklemeyi düşünmelisiniz. 
> 
> 

### <a name="verify-the-request"></a>İsteği doğrulama
İlk olarak, istek üzerine hızlı bir akıl sağlığı kontrolü yapmalıyız.  Yapılan istekleri görüntülemek için tarayıcınızın [geliştirici araçlarını](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) kullanabilirsiniz.

* İsteğin kaynağınıza değil, `<endpointname>.azureedge.net`bitiş noktası URL'nize gönderildiğini doğrulayın.
* İsteğin bir **Kabul-Kodlama** üstbilgisini içerdiğini ve bu üstbilginin değeri **gzip,** **deflate**veya **bzip2**içerir.

> [!NOTE]
> **Akamai profillerinden Azure CDN** yalnızca **gzip** kodlamayı destekler.
> 
> 

![CDN istek üstbilgi](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profiles"></a>Sıkıştırma ayarlarını doğrula (standart CDN profilleri)
> [!NOTE]
> Bu adım yalnızca CDN profiliniz **Microsoft'tan bir Azure CDN Standardı,** **Verizon'dan Azure CDN Standardı**veya **Akamai profilinden Azure CDN Standardı** ysa geçerlidir. 
> 
> 

[Azure portalındaki](https://portal.azure.com) bitiş noktanıza gidin ve **Yapıl'ı** tıklatın.

* Sıkıştırmanın etkin olduğunu doğrulayın.
* Sıkıştırılacak içeriğin sıkıştırılmış biçimler listesine dahil edildiğine göre MIME türünü doğrulayın.

![CDN sıkıştırma ayarları](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profiles"></a>Sıkıştırma ayarlarını doğrulayın (Premium CDN profilleri)
> [!NOTE]
> Bu adım yalnızca CDN profiliniz **Verizon profilinden** bir Azure CDN Premium'sa geçerlidir.
> 
> 

[Azure portalında](https://portal.azure.com) bitiş noktanıza gidin ve **Yönet** düğmesini tıklatın.  Ek geçit açılacak.  **HTTP Büyük** sekmesinin üzerine gidin, ardından **Önbellek Ayarları'nın** üzerinde gezinin.  **Sıkıştırma'yı**tıklatın. 

* Sıkıştırmanın etkin olduğunu doğrulayın.
* Dosya **Türleri** listesini doğrulayın, MIME türlerinin virgülle ayrılmış bir listesini (boşluk yok) içerir.
* Sıkıştırılacak içeriğin sıkıştırılmış biçimler listesine dahil edildiğine göre MIME türünü doğrulayın.

![CDN premium sıkıştırma ayarları](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached-verizon-cdn-profiles"></a>İçeriğin önbelleğe alınmış olduğunu doğrulayın (Verizon CDN profilleri)
> [!NOTE]
> Bu adım yalnızca CDN profiliniz **Verizon'dan** bir Azure CDN Standardı veya Verizon **profilinden Azure CDN Premium** ise geçerlidir.
> 
> 

Tarayıcınızın geliştirici araçlarını kullanarak, dosyanın istendiği bölgede önbelleğe alolduğundan emin olmak için yanıt üstaylarını denetleyin.

* **Sunucu** yanıt üstbilgisini kontrol edin.  Üstbilgi, aşağıdaki örnekte görüldüğü gibi **Platform (POP/Server ID)** biçimine sahip olmalıdır.
* **X-Önbellek** yanıt üstbilgisini kontrol edin.  Üstbilgi **HIT**okumalısınız.  

![CDN yanıt üstbilgi](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements-verizon-cdn-profiles"></a>Dosyanın boyut gereksinimlerini karşıladığını doğrulayın (Verizon CDN profilleri)
> [!NOTE]
> Bu adım yalnızca CDN profiliniz **Verizon'dan** bir Azure CDN Standardı veya Verizon **profilinden Azure CDN Premium** ise geçerlidir.
> 
> 

Sıkıştırma için uygun olmak için bir dosya aşağıdaki boyut gereksinimlerini karşılaması gerekir:

* 128 bayttan daha büyük.
* 1 MB'dan küçük.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>**Via** üstbilgiiçin başlangıç sunucusundaki isteği denetleme
**Via** HTTP üstbilgisi web sunucusuna isteğin bir proxy sunucusu tarafından geçirildiğini gösterir.  İstek bir **Via** üstbilgisi içerdiğinde, Microsoft IIS web sunucuları varsayılan olarak yanıtları sıkıştırmaz.  Bu davranışı geçersiz kılmak için aşağıdakileri gerçekleştirin:

* **IIS 6**: [IIS Metabase özelliklerinde HcNoCompressionForxies="FALSE" kümesi](/previous-versions/iis/6.0-sdk/ms525390(v=vs.90))
* **IIS 7 ve up**: [Sunucu yapılandırmasında hem **noCompressionForHttp10** hem de **noCompressionForProxies'i** False'a ayarlayın](https://www.iis.net/configreference/system.webserver/httpcompression)

