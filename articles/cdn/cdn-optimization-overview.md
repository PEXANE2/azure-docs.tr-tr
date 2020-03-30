---
title: İçerik teslim türü için Azure CDN'sini optimize edin
description: İçerik teslim türü için Azure CDN'sini optimize edin
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: magattus
ms.openlocfilehash: da8f17da9225da1d2b92bd8515d645bce9a1bbaa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252122"
---
# <a name="optimize-azure-cdn-for-the-type-of-content-delivery"></a>İçerik teslim türü için Azure CDN'sini optimize edin

İçeriği büyük bir küresel kitleye ulaştırdığınızda, içeriğinizin en iyi şekilde sunulmasını sağlamak çok önemlidir. [Azure İçerik Teslim Ağı (CDN),](cdn-overview.md) sahip olduğunuz içeriğin türüne bağlı olarak teslim deneyimini en iyi duruma getirebilir. İçerik bir web sitesi, canlı akış, video veya indirilecek büyük bir dosya olabilir. BIR CDN bitiş noktası oluşturduğunuzda, **En İyi Duruma Getirilmiş** seçeneğinde bir senaryo belirtirsiniz. Seçiminiz CDN bitiş noktasından teslim edilen içeriğe hangi optimizasyonun uygulanacağı belirlenir.

Optimizasyon seçenekleri, içerik teslim performansını ve daha iyi kaynak boşaltmayı artırmak için en iyi uygulama davranışlarını kullanmak üzere tasarlanmıştır. Senaryo seçimleriniz, kısmi önbelleğe alma, nesne ötme ve kaynak hatası yeniden deneme ilkesi için yapılandırmaları değiştirerek performansı etkiler. 

Bu makalede, çeşitli optimizasyon özellikleri genel bir bakış sağlar ve bunları kullanmanız gerekir. Özellikler ve sınırlamalar hakkında daha fazla bilgi için, her bir optimizasyon türüyle ilgili makalelere bakın.

> [!NOTE]
> Bir CDN bitiş noktası oluşturduğunuzda, seçenekler **için Optimize edilmiş** olan noktanın oluşturulduğu profil türüne bağlı olarak değişebilir. Azure CDN sağlayıcıları, senaryoya bağlı olarak geliştirmeyi farklı şekillerde uygular. 

## <a name="provider-options"></a>Sağlayıcı seçenekleri

**Microsoft profillerinden Azure CDN Standardı** aşağıdaki optimizasyonları destekler:

* [Genel web teslimat](#general-web-delivery). Bu optimizasyon, medya akışı ve büyük dosya indirme için de kullanılır.

> [!NOTE]
> Microsoft'tan dinamik site [ivmesi Azure Ön Kapı Hizmeti](https://docs.microsoft.com/azure/frontdoor/front-door-overview)ile sunulmaktadır.

**Verizon'dan Azure CDN Standardı** ve **Verizon profillerinden Azure CDN Premium** aşağıdaki optimizasyonları destekler:

* [Genel web teslimat](#general-web-delivery). Bu optimizasyon, medya akışı ve büyük dosya indirme için de kullanılır.

* [Dinamik alan ivmesi](#dynamic-site-acceleration) 


**Akamai profillerinden Azure CDN Standardı** aşağıdaki optimizasyonları destekler:

* [Genel web teslimatı](#general-web-delivery) 

* [Genel medya akışı](#general-media-streaming)

* [İsteğe bağlı video medya akışı](#video-on-demand-media-streaming)

* [Büyük dosya indirme](#large-file-download)

* [Dinamik alan ivmesi](#dynamic-site-acceleration) 

Microsoft, teslimatınız için en uygun sağlayıcıyı seçmek için farklı sağlayıcılar arasındaki performans varyasyonlarını test etmenizi önerir.

## <a name="select-and-configure-optimization-types"></a>Optimizasyon türlerini seçme ve yapılandırma

BIR CDN bitiş noktası oluşturduğunuzda, bitiş noktasının teslim etmesini istediğiniz senaryo ve içerik türüyle en iyi eşleşen bir optimizasyon türü seçin. **Genel web teslimvarsayılan** seçimdir. Yalnızca **Akamai** uç noktalarından gelen mevcut Azure CDN Standardı için, optimizasyon seçeneğini istediğiniz zaman güncelleştirebilirsiniz. Bu değişiklik Azure CDN'den yapılan teslimatı kesintiye uğratmaz. 

1. **Akamai profilinden bir Azure CDN Standardında** bir bitiş noktası seçin.

    ![Bitiş noktası seçimi](./media/cdn-optimization-overview/01_Akamai.png)

2. AYARLAR altında **Optimizasyon'u**seçin. Ardından, **açılan** listeden bir tür seçin.

    ![Optimizasyon ve tür seçimi](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Belirli senaryolar için optimizasyon

Bu senaryolardan biri için CDN bitiş noktasını en iyi duruma getirebilirsiniz. 

### <a name="general-web-delivery"></a>Genel web teslimatı

Genel web teslimatı en yaygın optimizasyon seçeneğidir. Web sayfaları ve web uygulamaları gibi genel web içeriği optimizasyonu için tasarlanmıştır. Bu optimizasyon dosya ve video indirme için de kullanılabilir.

Tipik bir web sitesi statik ve dinamik içerik içerir. Statik içerik, önbelleğe alınıp farklı kullanıcılara teslim edilebilen resimler, JavaScript kitaplıkları ve stil sayfaları içerir. Dinamik içerik, kullanıcı profiline uygun haber öğeleri gibi tek bir kullanıcı için kişiselleştirilmiştir. Alışveriş sepeti içeriği gibi dinamik içerik, her kullanıcıya özgü olduğundan önbelleğe verilmez. Genel web yayını tüm web sitenizi optimize edebilir. 

> [!NOTE]
> **Akamai profilinden** bir Azure CDN Standardı kullanıyorsanız, ortalama dosya boyutunuz 10 MB'dan küçükse bu optimizasyon türünü seçin. Aksi takdirde, ortalama dosya boyutunuz 10 MB'dan büyükse, açılan **listeden Büyük** **dosya karşıdan yükleme'yi** seçin.

### <a name="general-media-streaming"></a>Genel medya akışı

Canlı akış ve isteğe bağlı video akışı için bitiş noktasını kullanmanız gerekiyorsa, genel medya akışı optimizasyonu türünü seçin.

Medya akışı zamana duyarlıdır, çünkü istemciye video içeriğinin sık sık arabelleğe aldanması gibi geç gelen paketler, düşük bir görüntüleme deneyimine neden olabilir. Medya akışı optimizasyonu, ortam içeriği dağıtımının gecikmesini azaltır ve kullanıcılar için sorunsuz bir akış deneyimi sağlar. 

Bu senaryo, Azure medya hizmeti müşterileri için yaygındır. Azure medya hizmetlerini kullandığınızda, hem canlı hem de isteğe bağlı akış için kullanılabilecek tek bir akış bitiş noktası elde elabilirsiniz. Bu senaryoda, müşterilerin canlı akıştan isteğe bağlı akışa geçtiklerinde başka bir uç noktaya geçmeleri gerekmez. Genel medya akışı optimizasyonu bu tür senaryoları destekler.

**Microsoft'tan Azure CDN Standardı,** **Verizon'dan Azure CDN Standardı**ve **Verizon'dan Azure CDN Premium**için genel akışlı ortam içeriği sunmak için genel web teslim optimizasyonu türünü kullanın.

Medya akışı optimizasyonu hakkında daha fazla bilgi için [Bkz.](cdn-media-streaming-optimization.md)

### <a name="video-on-demand-media-streaming"></a>İsteğe bağlı video medya akışı

İsteğe bağlı video medya akışı optimizasyonu, isteğe bağlı video akış içeriğini geliştirir. İsteğe bağlı video akışı için bir bitiş noktası kullanıyorsanız, bu seçeneği kullanın.

**Microsoft'tan Azure CDN Standardı**, **Verizon'dan Azure CDN Standardı**ve **Verizon profillerinden Azure CDN Premium** için, isteğe bağlı video akışlı ortam içeriği sunmak için genel web teslim optimizasyonu türünü kullanın.

Medya akışı optimizasyonu hakkında daha fazla bilgi için [Bkz.](cdn-media-streaming-optimization.md)

> [!NOTE]
> CDN bitiş noktası öncelikle isteğe bağlı video içeriğine hizmet veriyorsa, bu optimizasyon türünü kullanın. Bu eniyim türü ile genel medya akışı optimizasyonu türü arasındaki en büyük fark, bağlantı yeniden deneme zaman ayırmadır. Zaman zaman zaman zaman akışı senaryoları ile çalışmak için çok daha kısadır.
>

### <a name="large-file-download"></a>Büyük dosya indirme

**Akamai profillerinden Azure CDN Standardı** için büyük dosya indirmeler 10 MB'dan büyük içerik için optimize edilebilmektedir. Ortalama dosya boyutunuz 10 MB'dan küçükse, genel web dağıtımını kullanın. Ortalama dosya boyutlarınız sürekli olarak 10 MB'dan büyükse, büyük dosyalar için ayrı bir bitiş noktası oluşturmak daha verimli olabilir. Örneğin, firmware veya yazılım güncelleştirmeleri genellikle büyük dosyalardır. 1,8 GB'dan büyük dosyaları teslim etmek için büyük dosya indirme optimizasyonu gereklidir.

**Microsoft'tan Azure CDN Standardı,** **Verizon'dan Azure CDN Standardı**ve **Verizon profillerinden Azure CDN Premium** için, büyük dosya indirme içeriği sunmak için genel web teslim optimizasyonu türünü kullanın. Dosya indirme boyutunda herhangi bir sınırlama yoktur.

Büyük dosya optimizasyonu hakkında daha fazla bilgi için [büyük dosya optimizasyonu'na](cdn-large-file-optimization.md)bakın.

### <a name="dynamic-site-acceleration"></a>Dinamik site hızlandırma

 Dinamik site hızlandırma (DSA), Azure CDN Standard için **Akamai'den,** **Azure CDN Standard from Verizon**ve Azure CDN Premium için Verizon **profillerinden** kullanılabilir. Bu optimizasyon kullanmak için ek bir ücret içerir; daha fazla bilgi için [İçerik Dağıtım Ağı fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/)için bkz.

> [!NOTE]
> Microsoft'un dinamik site hızlandırması, uygulamanızın iş yüklerini sunmak için Microsoft'un özel küresel ağından yararlanan [küresel](https://en.wikipedia.org/wiki/Anycast) bir hizmet olan Azure Ön [Kapı Hizmeti](https://docs.microsoft.com/azure/frontdoor/front-door-overview) aracılığıyla sunulur.

DSA, dinamik içeriğin gecikmesi ve performansından yararlanan çeşitli teknikler içerir. Teknikler rota ve ağ optimizasyonu, TCP optimizasyonu ve daha fazlasını içerir. 

Önbelleğe alamayan çok sayıda yanıt içeren bir web uygulamasını hızlandırmak için bu optimizasyonu kullanabilirsiniz. Örnek olarak arama sonuçları, ödeme işlemleri veya gerçek zamanlı veriler verilebilir. Statik veriler için temel Azure CDN önbelleğe alma özelliklerini kullanmaya devam edebilirsiniz. 

Dinamik site ivmesi hakkında daha fazla bilgi için [Dinamik site ivmesi'ne](cdn-dynamic-site-acceleration.md)bakın.



