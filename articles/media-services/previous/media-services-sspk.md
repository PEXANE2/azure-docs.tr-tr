---
title: Microsoft® Sorunsuz Akış İstemci Taşıma Kitilisanslama
description: Microsoft® Sorunsuz Akış İstemci Taşıma Kiti'ni lisanslama hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: xpouyat
manager: femila
editor: ''
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2019
ms.author: xpouyat
ms.openlocfilehash: 59ac3ab2e70bfa00f19bae7e551780f2b2bb8ccb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79096717"
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Microsoft® Sorunsuz Akış İstemci Taşıma Kitilisanslama 
## <a name="overview"></a>Genel Bakış
Microsoft Smooth Streaming İstemci Taşıma Kiti (kısaca**SSPK),** gömülü aygıt üreticilerine, kablo ve mobil operatörlere, içerik servis sağlayıcılarına, cep telefonu üreticilerine, bağımsız yazılım satıcılarına (ISV) ve çözüm sağlayıcılarına yardımcı olmak için, Uyumlu İçeriği Düzgün Akış biçiminde akışsağlamak için ürün ve hizmetler oluşturmaya yardımcı olmak için optimize edilmiş bir Sorunsuz Akış istemcisi uygulamasıdır. SSPK, lisans sahibi tarafından herhangi bir cihaza ve platforma taşınabilir Düzgün Akış istemcisinin aygıt ve platformdan bağımsız bir uygulamasıdır. 

Aşağıda üst düzey bir mimari ve IIS Smooth Streaming Porting Kit kutusu Microsoft tarafından sağlanan Smooth Streaming İstemci uygulamasıdır ve Düzgün Akış içeriğinin oynatılmı için tüm temel mantığı içerir. Bu içerik daha sonra uygun arabirimler uygulanarak belirli bir aygıt veya platform için ortaklar tarafından taşınır. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a>Açıklama
SSPK, mükemmel iş değeri sunan terimlerle lisanslanır. SSPK lisansı sektöre şunları sağlar:

* C++'da Düzgün Akış Taşıma Kiti kaynağı 
  * Sorunsuz Akış İstemci işlevselliğini uygular
  * format ayrıştırma, buluşsal, tamponlama mantığı, vb ekler
* Oyuncu uygulama API'leri 
  * bir medya oynatıcı uygulaması ile etkileşim için programlama arayüzleri
* Platform Soyutlama Katmanı (PAL) Arabirimi 
  * işletim sistemi (iş parçacığı, soketler) ile etkileşim için programlama arayüzleri
* Donanım Soyutlama Katmanı (HAL) Arabirimi 
  * donanım A/V kod çözücülerle etkileşim için programlama arabirimleri (kod çözme, işleme)
* Dijital Haklar Yönetimi (DRM) Arabirimi 
  * DRM Soyutlama Katmanı (DAL) aracılığıyla DRM'yi işlemek için programlama arayüzleri
  * Microsoft PlayReady Porting Kit ayrı ayrı gemi ama bu arayüz üzerinden entegre. Microsoft PlayReady Device lisanslama hakkında daha fazla bilgi için [buraya](https://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl)tıklayın.
* Uygulama örnekleri 
  * Linux için örnek PAL uygulaması
  * GStreamer için örnek HAL uygulaması

## <a name="licensing-options"></a>Lisans Seçenekleri
Microsoft Smooth Streaming İstemci Taşıma Kiti, iki farklı lisans anlaşması altında lisans sahiplerinin kullanımına sunulmuştur: biri Düzgün Akışlı İstemci Geçici Ürünleri geliştirmek ve diğeri de Düzgün Akışlı İstemci Son Ürünlerini son kullanıcılara dağıtmak için.

* Geçici Ürünler geliştirmek için kaynak kodu taşıma kiti gerektiren yonga seti üreticileri, sistem entegratörleri veya bağımsız yazılım satıcıları (ISV) için bir Microsoft Smooth Streaming İstemci Taşıma Kiti **Geçici Ürün Lisansı** çalıştırılmalıdır.
* Düzgün Akış Lı İstemci Son Ürünleri için son kullanıcılara dağıtım hakları gerektiren aygıt üreticileri veya ISV'ler için Microsoft Düzgün Akış İstemci Taşıma Kiti **Son Ürün Lisansı** yürütülmelidir.

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft Smooth Streaming İstemci Taşıma Kiti Geçici Ürün Lisansı
Bu lisans altında, Microsoft, Sorunsuz Akış İstemci Taşıma Kiti ve diğer Sorunsuz Akış İstemci Taşıma Kiti cihaz lisans sahiplerine Düzgün Akış İstemci Geçici Ürünler geliştirmek ve dağıtmak için gerekli fikri mülkiyet hakları sunar Smooth Streaming İstemci Son Ürünleri dağıtın.

#### <a name="fee-structure"></a>Ücret yapısı
50.000 ABD doları tek seferlik lisans ücreti, Smooth Streaming İstemci Taşıma Kiti'ne erişim sağlar. 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft Smooth Streaming İstemci Taşıma Kiti Son Ürün Lisansı
Bu lisans altında, Microsoft diğer Smooth Streaming İstemci Taşıma Kiti lisans sahiplerinden Düzgün Akış İstemci Geçici Ürünler almak ve şirket markalı Smooth Streaming Client Final dağıtmak için gerekli tüm fikri mülkiyet haklarını sunar Son kullanıcılara ürünler.

#### <a name="fee-structure"></a>Ücret yapısı
Smooth Streaming Client Final Product aşağıdaki gibi bir telif modeli altında sunulmaktadır:

* Sevk edilen cihaz uygulaması başına $0.10
* Kraliyet ailesi her yıl 50.000 dolar alacaktır.
* Her yıl ilk 10.000 cihaz uygulaması için telif hakkı yok 

## <a name="licensing-procedure-and-sspk-access"></a>Lisans Prosedürü ve SSPK erişimi
Tüm [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) lisans sorguları için e-posta.

[SSPK Dağıtım portalı,](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) kayıtlı Geçici lisans sahipleri tarafından erişilebilir.

Ara ve Son SSPK lisans sahipleri [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com)teknik soruları .

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft Smooth Streaming İstemci Geçici Ürün Sözleşmesi Lisans Sahipleri

* Adroit İş Çözümleri A.Ş.
* Gelişmiş Dijital Yayın SA
* AirTies Kablosuz Iletism Sanayive Dis Ticaret A.Ş.
* Albis Teknolojileri Ltd
* Alticast Şirketi
* Amazon Dijital Hizmetler, Inc
* Arion Teknoloji A.Ş.
* AVC Multimedya Yazılım A.Ş.
* Cavium, Inc.
* EchoStar Satın Alma Şirketi
* Enseo, Inc.
* Fluendo S.A.
* Guangzhou Shikun Elektronik., Ltd
* HANDAN BroadInfoCom Co, Ltd
* Infomir GMBH
* Irdeto ABD A.Ş.
* Liberty Global Hizmetler BV
* MediaTek A.Ş.
* MStar Co, Ltd
* Nintendo Co, Ltd
* OpenTV, Inc.
* Safran Dijital Limited
* Sichuan Changhong Elektrik Co, Ltd
* SoftatHome
* Sony Corporation
* Tatung Teknoloji A.Ş.
* Top Victory Investments, Ltd.
* Vestel Elektronik Sanayi ve Ticaret A.Ş.
* VisualOn, Inc.
* ZTE Şirketi

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft Smooth Streaming İstemci Nihai Ürün Sözleşmesi Lisans Sahipleri
* Gelişmiş Dijital Yayın SA
* AirTies Kablosuz Iletism Sanayive Dis Ticaret A.Ş.
* Albis Teknolojileri Ltd
* Amazon Dijital Hizmetler, Inc
* AmTRAN Teknoloji A.Ş.
* Arcadyan Teknoloji A.Ş.
* Arion Teknoloji A.Ş.
* ATMACA ELEKTRONİK SAN. VE TİC. A.ş
* İngiliz Sky Broadcasting Limited
* CastPal Teknoloji A.Ş., Shenzhen
* Compal Elektronik A.Ş.
* Dongguan Dijital AV Technology Corp, Ltd
* EchoStar Satın Alma Şirketi
* Enseo, Inc.
* FilmFlex Filmler Limited
* Fluendo S.A.
* FUNAI ELEKTRİk A.Ş., LTD
* Gibson Yenilikler Limited
* Haier Bilgi Başvuru S.R.L
* HANDAN BroadInfoCom Co, Ltd
* Hisense International Co, Ltd 
* Homecast Co, Ltd
* Hon Hai Hassas Sanayi A.Ş.
* Infomir GMBH
* Kaonmedia Co, Ltd
* KDDI Şirketi
* Nintendo Co, Ltd
* Turuncu SA
* Safran Dijital Limited
* Sagemcom Genişbant SAS
* Shenzhen Chuangwei-RGB Elektronik Co, Ltd
* Shenzhen Coship Elektronik CO., LTD
* Shenzhen Jiuzhou Elektrik Co, Ltd
* Shenzhen Skyworth Dijital Teknoloji A.Ş., Ltd
* Sichuan Changhong Elektrik Co, Ltd
* Skardin Sanayi A.Ş.
* Gökyüzü Deutschland Fernsehen GmbH & Co KG
* SmarDTV S.A.
* SoftatHome
* Sony Corporation
* Technicolor Dağıtım Teknolojileri, SAS
* Tongfang Global Ltd
* Top Victory Investments, Ltd.
* Toshiba Lifestyle Ürünler & Hizmetleri Corporation
* Universal Media Corporation /Slovakya/ s.r.o.
* VIZIO, Inc.
* Wistron Şirketi
* ZTE Şirketi

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

