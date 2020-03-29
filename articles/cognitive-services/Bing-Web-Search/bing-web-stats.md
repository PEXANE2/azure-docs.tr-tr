---
title: Bing Web Arama API'sine analitik ekleme
titleSuffix: Azure Cognitive Services
description: Bing İstatistikleri, Bing Resim Arama API'sine analitik sağlar. Analitik, çağrı hacmini, üst sorgu dizelerini, coğrafi dağılımı ve daha fazlasını içerir.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 5d1b0b19523eb37aa83aa59b24114be9f76ffa55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68882758"
---
# <a name="add-analytics-to-the-bing-search-apis"></a>Bing Arama API'lerine analitik ekleme

Bing İstatistikleri, Bing Arama API'leri için analiz sağlar. Bu analizler çağrı hacmini, üst sorgu dizelerini, coğrafi dağılımı ve daha fazlasını içerir. Azure kaynağınızda gezinerek ve Bing İstatistiklerini Etkinleştir'i tıklatarak [Azure portalında](https://ms.portal.azure.com) Bing **İstatistikleri'ni**etkinleştirebilirsiniz.

> [!IMPORTANT]
> * Bing İstatistikleri ücretsiz deneme abonelikleri veya ücretsiz `F0` fiyatlandırma katmanındaki kaynaklarla kullanılamaz.
> * Bing İstatistikleri panosu nda bulunan hiçbir veriyi üçüncü taraflara dağıtım uygulamaları oluşturmak için kullanamazsınız.
> * Bing İstatistiklerini etkinleştirmek abonelik oranınızı biraz artırır. Ayrıntılar için [fiyatlandırmaya](https://aka.ms/bingstatisticspricing) bakın.


Aşağıdaki resim, her Bing Arama API bitiş noktası için kullanılabilir analizi gösterir.

![Uç nokta destek matrisine göre dağılım](./media/bing-statistics/bing-statistics-matrix.png)

## <a name="access-your-analytics"></a>Analitiğinize erişin

Bing, analiz verilerini her 24 saatte bir günceller ve [analiz panosundan](https://bingapistatistics.com)erişebileceğiniz 13 aya kadar geçmişi korur. Bing İstatistikleri'ne kaydolmak için kullandığınız Microsoft hesabını (MSA) kullandığınızdan emin olun.

> [!NOTE]  
> * Ölçümlerin gösterge panelinde yüzeye çıkması 24 saat kadar sürebilir. Pano, verilerin en son güncelleştirilen tarih ve saati gösterir.  
> * Ölçümler, Bing İstatistikleri Eklentisini etkinleştirdiğiniz andan itibaren kullanılabilir.

## <a name="filter-the-data"></a>Verileri filtreleme

Varsayılan olarak, grafikler ve grafikler erişebildiğiniz tüm ölçümleri ve verileri görüntüler. İlgilendiğiniz kaynakları, pazarları, uç noktaları ve raporlama dönemini seçerek grafiklerde ve grafiklerde gösterilen verileri filtreleyebilirsiniz. Aşağıdaki filtreleri değiştirebilirsiniz:

- **Kaynak Kimliği**: Azure aboneliğinizi tanımlayan benzersiz kaynak kimliği. Birden fazla Bing Arama API katmanına abone yseniz, liste birden çok kişi içerir. Varsayılan olarak, tüm kaynaklar seçilir.  
  
- **Piyasalar**: Sonuçların geldiği piyasalardır. Örneğin, en-us (İngilizce, Amerika Birleşik Devletleri). Varsayılan olarak, tüm pazarlar seçilir. Aramada bir pazar belirtmiyorsa ve Bing kullanıcının pazarını belirleyemiyorsa Bing'in `en-WW` kullandığı pazardır.  
  
- **Uç Noktalar**: Bing Arama API uç noktaları. Liste, ücretli aboneliğiniz olan tüm uç noktaları içerir. Varsayılan olarak, tüm uç noktalar seçilir.  

- **Zaman Dilimi**: Raporlama dönemi. Şunları belirtebilirsiniz:
  - **Tümü**: 13 aya kadar veri içerir  
  - **Son 24 saat**: Son 24 saate ait analizleri içerir  
  - **Önceki hafta**: Önceki yedi güne ait analizleri içerir  
  - **Önceki ay**: Önceki 30 güne ait analizleri içerir  
  - **Özel tarih aralığı**: Varsa, belirtilen tarih aralığından analiziçerir  

## <a name="charts-and-graphs"></a>Grafikler ve grafikler

Pano, seçili bitiş noktası için kullanılabilir ölçümlerin grafiklerini ve grafiklerini gösterir. Tüm ölçümler tüm uç noktalar için kullanılamaz. Her uç noktaiçin grafikler ve grafikler statiktir (gösterilecek grafikleri ve grafikleri seçemeyebilirsiniz). Pano, yalnızca veri bulunan grafikleri ve grafikleri gösterir.

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

Aşağıda olası ölçümler ve bitiş noktası kısıtlamaları yer alıyor.

- **Çağrı Hacmi**: Raporlama döneminde yapılan arama ların sayısını gösterir. Raporlama dönemi bir gün içinse, grafik saat başına yapılan arama sayısını gösterir. Aksi takdirde, grafik raporlama döneminin günlük yapılan arama sayısını gösterir.  
  
  > [!NOTE]
  > Arama hacmi, genellikle yalnızca başarılı çağrıları içeren faturalama raporlarından farklı olabilir.

- **En İyi Sorgular**: Raporlama döneminde ki en üst sorguları ve her sorgunun oluşum sayısını gösterir. Gösterilen sorgu sayısını yapılandırabilirsiniz. Örneğin, en iyi 25, 50 veya 75 sorguyu gösterebilirsiniz. En İyi Sorgular aşağıdaki uç noktalar için kullanılamaz:  

  - /images/trending
  - /görüntüler/ayrıntılar
  - /images/visualsearch
  - /videolar/trendler
  - /videolar/ayrıntılar
  - /haberler
  - /haberler/trendkonular
  - /öneriler  
  
  > [!NOTE]  
  > E-postalar, telefon numaraları, SSN gibi gizli bilgileri kaldırmak için bazı sorgu terimleri bastırılabilir.

- **Coğrafi Dağılım**: Arama sonuçlarının kaynağı olan pazarlar. Örneğin, `en-us` (İngilizce, Amerika Birleşik Devletleri). Bing, `mkt` belirtilmişse piyasayı belirlemek için sorgu parametresini kullanır. Aksi takdirde Bing, piyasayı belirlemek için arayanın IP adresi gibi sinyalleri kullanır.

- **Yanıt Kodu Dağılımı**: Raporlama dönemindeki tüm çağrıların HTTP durum kodları.

- **Arama Kaynağı Dağılımı**: Kullanıcılar tarafından kullanılan tarayıcı türleri. Örneğin, Microsoft Edge, Chrome, Safari ve FireFox. Tarayıcı dışından yapılan aramalar (botlar, Postacı veya konsol uygulamasından kıvrılma gibi) Kitaplıklar altında gruplandırılır. Kaynağı, isteğin Kullanıcı Temsilcisi üstbilgi değeri kullanılarak belirlenir. İstek Kullanıcı Aracısı üstbilgisini içermiyorsa, Bing kaynağı diğer sinyallerden elde etmeye çalışır.  

- **Güvenli Arama Dağılımı**: Güvenli arama değerlerinin dağılımı. Örneğin, kapalı, orta veya katı. Sorgu `safeSearch` parametresi belirtilmişse değeri içerir. Aksi takdirde, Bing değeri orta ya da orta varsayılan olarak alır.  

- **İstenen DağıtımA Verilen Yanıtlar**: `responseFilter` Sorgu parametresinde istediğiniz Web Arama API yanıtları.  

- **İade Edilen Yanıtlar Dağıtım**: Web Arama API'sinin yanıtta döndürüldettiği yanıtlar.

- **Yanıt Sunucu Dağılımı**: API isteklerinize hizmet veren uygulama sunucusu. Olası değerler Bing.com (masaüstü ve dizüstü bilgisayar aygıtlarından sunulan trafik için) ve Bing.com mobil (mobil cihazlardan sunulan trafik için) vardır. Sunucu, isteğin Kullanıcı Temsilcisi üstbilgi değeri kullanılarak belirlenir. İstek Kullanıcı Aracısı üstbilgisini içermiyorsa, Bing sunucuyu diğer sinyallerden türetmeye çalışır.

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Arama API'leri nedir?](bing-api-comparison.md)
* [Bing Arama API’si kullanım ve görüntüleme gereksinimleri](use-display-requirements.md)
