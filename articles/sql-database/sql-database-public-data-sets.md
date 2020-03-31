---
title: Azure analitiği için genel veri kümeleri
description: Azure analitik hizmetlerini ve çözümlerini prototiplemek ve test etmek için kullanabileceğiniz genel veri kümeleri hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: reference
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 10/01/2018
ms.openlocfilehash: 4b1c1a963b065411f1a0ab84141bdf1835930ebb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74973552"
---
# <a name="public-data-sets-for-testing-and-prototyping"></a>Test ve prototipleme için genel veri kümeleri

Depolama ve analiz hizmetlerini ve çözümlerini prototiplemek ve test etmek için kullanabileceğiniz veriler için genel veri kümeleri listesine göz atın.

## <a name="us-government-and-agency-data"></a>ABD Hükümeti ve ajans verileri

| Veri kaynağı | Veriler hakkında | Dosyalar hakkında |
|---|---|---|
| [ABD Hükümeti verileri](https://catalog.data.gov/dataset) | Tarım, iklim, tüketici, ekosistemler, eğitim, enerji, finans, sağlık, yerel yönetim, üretim, denizcilik, okyanus, kamu güvenliği ve ABD'de bilim ve araştırma kapsayan 250.000'den fazla veri seti. | HTML, XML, CSV, JSON, Excel ve diğerleri dahil olmak üzere çeşitli biçimlerde çeşitli boyutlarda dosyalar. Kullanılabilir veri kümelerini dosya biçimine göre filtreleyebilirsiniz. |
| [ABD Nüfus Sayımı verileri](https://www.census.gov/data.html) | ABD nüfusu hakkında istatistiksel veriler | Veri kümeleri çeşitli biçimlerdedir. |
| [NASA'dan dünya bilimi verileri](https://earthdata.nasa.gov/) | Tarım, atmosfer, biyosfer, iklim, kriyosfer, insan boyutları, hidrosfer, kara yüzeyi, okyanuslar, güneş-dünya etkileşimleri ve daha fazlasını kapsayan 32.000'den fazla veri koleksiyonu. | Veri kümeleri çeşitli biçimlerdedir. |
| [Havayolu uçuş gecikmeleri ve diğer ulaşım verileri](https://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp) | "ABD Ulaştırma Bakanlığı'nın (DOT) Ulaştırma İstatistikleri Bürosu (BTS), büyük havayolu şirketleri tarafından gerçekleştirilen iç hat uçuşlarının zamanında performansını takip ediyor. Zamanında, gecikmeli, iptal edilmiş ve yönlendirilmiş uçuş sayısına ilişkin özet bilgiler görünür... özet tablolar bu web sitesinde yayınlanmıştır." | Dosyalar CSV formatındadır. |
| [Trafik ölümleri - ABD Fatality Analysis Reporting System (FARS)](https://www.nhtsa.gov/FARS) | "FARS NHTSA, Kongre ve motorlu araç trafik kazalarında uğradığı ölümcül yaralanmalar ile ilgili Amerikan kamu yıllık verileri sağlayan ülke çapında bir nüfus sayımı olduğunu." | "FARS Sorgu Sistemi'ni kullanarak çevrimiçi olarak çalışan kendi ölüm verilerinizi oluşturun. Veya 1975'ten FTP Sitesinden günümüze kadar tüm FARS verilerini indirin." |
| [Toksik kimyasal veriler - EPA Toksisite ForeCaster (ToxCast™) verileri](https://www.epa.gov/chemical-research/toxicity-forecaster-toxcasttm-data) | "EPA en güncel, kamuya açık yüksek iş ittoksisite verileri kimyasalların binlerce. Bu veriler EPA'nın ToxCast araştırma çabası ile oluşturulur." | Veri kümeleri elektronik tablolar, R paketleri ve MySQL veritabanı dosyaları dahil olmak üzere çeşitli biçimlerde kullanılabilir. |
| [Toksik kimyasal veriler - NIH Tox21 Data Challenge 2014](https://tripod.nih.gov/tox21/challenge/) | "2014 Tox21 veri mücadelesi, bilim insanlarının 21. | Veri kümeleri SMILES ve SDF formatlarında mevcuttur. Veriler "~ 10.000 bileşikler (Tox21 10K) Tox21 toplama üzerinde araştırma faaliyet verileri ve kimyasal yapılar sağlar." |
| [NCBI'den biyoteknoloji ve genom verileri](https://www.ncbi.nlm.nih.gov/guide/data-software/) | Genleri, genomları ve proteinleri kapsayan çoklu veri kümeleri. | Veri kümeleri metin, XML, BLAST ve diğer biçimlerdedir. Bir BLAST uygulaması mevcuttur. |

## <a name="other-statistical-and-scientific-data"></a>Diğer istatistiksel ve bilimsel veriler

| Veri kaynağı | Veriler hakkında | Dosyalar hakkında |
|---|---|---|
| [New York City taksi verileri](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml) | "Taksi seyahati kayıtları, teslim alma ve bırakma tarihlerini/saatlerini, teslim alma ve bırakma yerlerini, seyahat mesafelerini, belirlenen ücretleri, fiyat türlerini, ödeme türlerini ve sürücü tarafından bildirilen yolcu sayılarını yakalayan alanları içerir." | Veri kümeleri her ay CSV dosyalarında bulunur. |
| [Microsoft Research veri setleri - "Araştırma için Veri Bilimi"](https://www.microsoft.com/research/academic-program/data-science-microsoft-research/) | İnsan-bilgisayar etkileşimi, ses/video, veri madenciliği/bilgi alma, jeouzamsal/konum, doğal dil işleme ve robotik/bilgisayar vizyonunu kapsayan çoklu veri kümeleri. | Veri kümeleri, karşıdan yüklenebilir şekilde sıkıştırılmış çeşitli biçimlerdedir. |
| [Genel genom verileri](https://www.completegenomics.com/public-data/) | "Tüm insan genomlarının çeşitli veri seti serbestçe herhangi bir genomik çalışma geliştirmek için kamu kullanımı için kullanılabilir ..." Sağlayıcı, Complete Genomics, kar amacı gütmeyen özel bir şirkettir. | Veri kümeleri, ayıklamadan sonra UNIX metin biçimindedir. Analiz araçları da mevcuttur. |
| [Açık Bilim Veri Bulutu verileri](https://www.opensciencedatacloud.org/projects/) | "Açık Bilim Veri Bulutu, bilim camiasına terabayt ve petabayt ölçekli bilimsel veri kümelerini depolama, paylaşma ve analiz etme için kaynaklar sağlar."| Veri kümeleri çeşitli biçimlerdedir. |
| [Küresel iklim verileri - WorldClim](https://worldclim.org/) | "WorldClim yaklaşık 1 km2 bir mekansal çözünürlüğe sahip küresel iklim katmanları (ızgaralı iklim verileri) kümesidir. Bu veriler haritalama ve mekansal modelleme için kullanılabilir." | Bu dosyalar jeouzamsal veriler içerir. Daha fazla bilgi için [Bkz. Veri biçimi.](https://worldclim.org/formats1) |
| [İnsan toplumu hakkında veriler - GDELT Projesi](https://www.gdeltproject.org/data.html) | "GDELT Projesi, insan toplumunun şimdiye kadar yaratılmış en büyük, en kapsamlı ve en yüksek çözünürlüklü açık veritabanıdır." | Ham veri dosyaları CSV formatındadır. |
| [Criteo makine öğrenimi için reklam tıklama tahmin verileri](https://labs.criteo.com/2013/12/download-terabyte-click-logs/) | "Şimdiye kadar halka açık en büyük ML veri seti." Daha fazla bilgi için [Criteo'nun 1 TB Tıklama Tahmini Veri Seti'ne](https://blogs.technet.microsoft.com/machinelearning/20../../now-available-on-azure-ml-criteos-1tb-click-prediction-dataset/)bakın. | |
| [Lemur Projesi'nden ClueWeb09 metin madenciliği veri seti](https://www.lemurproject.org/clueweb09.php/) | "ClueWeb09 veri seti bilgi alma ve ilgili insan dili teknolojileri araştırma desteklemek için oluşturuldu. Ocak ve Şubat 2009'da toplanan 10 dilde yaklaşık 1 milyar web sayfasından oluşmaktadır." | Bkz. [Dataset Bilgileri](https://www.lemurproject.org/clueweb09/datasetInformation.php).|

## <a name="online-service-data"></a>Çevrimiçi hizmet verileri

| Veri kaynağı | Veriler hakkında | Dosyalar hakkında |
|---|---|---|
| [GitHub arşivi](https://www.githubarchive.org/) | "GitHub Arşivi, halka açık GitHub zaman çizelgesini [olayların] kaydetmek, arşivlemek ve daha fazla analiz için kolayca erişilebilir hale getirmek için bir projedir." | JSON kodlu olay arşivlerini bir web istemcisinden .gz (Gzip) biçiminde indirin. |
| [GHTorrent projesinden GitHub etkinlik verileri](http://ghtorrent.org/) | "GHTorrent projesi GitHub REST API aracılığıyla sunulan verilerin ölçeklenebilir, sorgulanabilir, çevrimdışı aynaoluşturmak için bir çabadır. GHTorrent GitHub genel olay zaman çizgisini izler. Her olay için, içeriğini ve bağımlılıklarını ayrıntılı bir şekilde alır." | MySQL veritabanı dökümleri CSV formatındadır. |
| [Yığın Taşma veri dökümü](https://archive.org/details/stackexchange) | "Bu, Stack Exchange ağında [Stack Overflow dahil] kullanıcı tarafından katkıda bulunulan tüm içeriğin anonim bir dökümüdür." | "Stack Overflow gibi her site, bzip2 sıkıştırma kullanılarak 7 zip üzerinden sıkıştırılan XML dosyalarından oluşan ayrı bir arşiv olarak biçimlendirilir. Her site arşivi Mesajlar, Kullanıcılar, Oylar, Yorumlar, PostHistory ve PostLinks içerir." |
