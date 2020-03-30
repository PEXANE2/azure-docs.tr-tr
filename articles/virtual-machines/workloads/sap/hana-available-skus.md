---
title: Azure'da SAP HANA için SK'lar (Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA için SUK'lar (Büyük Örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/21/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aad4e3ff3df8b4aeecbbbee7883ba383b9fd0d9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617256"
---
# <a name="available-skus-for-hli"></a>HLI için kullanılabilir SKU'lar

Düzeltme 3 damgalarına dayalı Azure'da SAP HANA (Büyük Örnekler) hizmeti, Azure bölgelerinde çeşitli yapılandırmalarda kullanılabilir:

- Batı ABD
- Doğu ABD
- Doğu Avustralya
- Güneydoğu Avustralya
- Batı Avrupa
- Kuzey Avrupa
- Doğu Japonya
- Batı Japonya

Düzeltme 4 pullarına dayalı Azure'da SAP HANA (Büyük Örnekler) hizmeti, Azure bölgelerinde çeşitli yapılandırmalarda kullanılabilir:

- Batı ABD 2
- Doğu ABD
- Orta Güney ABD
- Batı Avrupa
- Kuzey Avrupa



[SAP HANA sertifikalı SUS HANA büyük Örnekleri](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) listesi gibi:

| SAP çözümü | CPU | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| OLAP için optimize edin: SAP BW, BW/4HANA<br /> veya genel OLAP iş yükü için SAP HANA | AZURE S72'de SAP HANA<br /> – 2 x Intel® Xeon® Işlemci E7-8890 v3<br /> 36 CPU çekirdeği ve 72 CPU iş parçacığı |  768 GB |  3 TB | Artık teklif edilmedi |
| --- | AZURE S144'te SAP HANA<br /> – 4 x Intel® Xeon® Işlemci E7-8890 v3<br /> 72 CPU çekirdeği ve 144 CPU iş parçacığı |  1,5 TB |  6 Veste | Artık teklif edilmedi |
| --- | AZURE S192'de SAP HANA<br /> – 4 x Intel® Xeon® Işlemci E7-8890 v4<br /> 96 CPU çekirdeği ve 192 CPU iş parçacığı |  2.0 TB |  8 TB | Artık teklif edilmedi |
| --- | AZURE S224'te SAP HANA<br /> – 4 x Intel® Xeon® Platinum 8276 işlemci (Cascade gölü olarak da bilinir)<br /> 112 CPU çekirdeği ve 224 CPU iş parçacığı |  3.0 TB |  6.3 TB | Revision3 ve Revision4 pullarında mevcuttur  |
| --- | AZURE S384'te SAP HANA<br /> – 8 x Intel® Xeon® Işlemci E7-8890 v4<br /> 192 CPU çekirdeği ve 384 CPU iş parçacığı |  4.0 TB |  16 TB | Revision4 pullar mevcuttur |
| OLTP için optimize edin: SAP Business Suite<br /> SAP HANA veya S/4HANA (OLTP),<br /> jenerik OLTP | SAP HANA Üzerinde Azure S72m<br /> – 2 x Intel® Xeon® Işlemci E7-8890 v3<br /> 36 CPU çekirdeği ve 72 CPU iş parçacığı |  1,5 TB |  6 Veste | Artık teklif edilmedi |
|---| SAP HANA Üzerinde Azure S144m<br /> – 4 x Intel® Xeon® Işlemci E7-8890 v3<br /> 72 CPU çekirdeği ve 144 CPU iş parçacığı |  3.0 TB |  12 TB | Artık teklif edilmedi |
|---| SAP HANA Üzerinde Azure S192m<br /> – 4 x Intel® Xeon® Işlemci E7-8890 v4<br /> 96 CPU çekirdeği ve 192 CPU iş parçacığı  |  4.0 TB |  16 TB | Artık teklif edilmedi |
| --- | SAP HANA Üzerinde Azure S224m<br /> – 4 x Intel® Xeon® Platinum 8276 işlemci (Cascade gölü olarak da bilinir)<br /> 112 CPU çekirdeği ve 224 CPU iş parçacığı |  6.0 TB |  10,5 TB | Revision3 ve Revision4 pullarında mevcuttur  |
|---| AZURE S384m üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Işlemci E7-8890 v4<br /> 192 CPU çekirdeği ve 384 CPU iş parçacığı |  6.0 TB |  18 TB | Revision4 pullar mevcuttur|
|---| SAP HANA Üzerinde Azure S384xm<br /> – 8 x Intel® Xeon® Işlemci E7-8890 v4<br /> 192 CPU çekirdeği ve 384 CPU iş parçacığı |  8.0 TB |  22 TB |  Revision4 pullar mevcuttur |
|---| SAP HANA Üzerinde Azure S576m<br /> – 12 x Intel® Xeon® Işlemci E7-8890 v4<br /> 288 CPU çekirdeği ve 576 CPU iş parçacığı |  12.0 TB |  28 TB | Revision4 pullar mevcuttur|
|---| SAP HANA Üzerinde Azure S768m<br /> – 16 x Intel® Xeon® Işlemci E7-8890 v4<br /> 384 CPU çekirdekleri ve 768 CPU iş parçacığı |  16.0 TB |  36 TB | Revision4 pullar mevcuttur|
|---| AZURE S960m'da SAP HANA<br /> – 20 x Intel® Xeon® Işlemci E7-8890 v4<br /> 480 CPU çekirdek leri ve 960 CPU iş parçacığı |  20.0 TB |  46 TB | Revision4 pullar mevcuttur|


SAP HANA TDIv5 altında SAP, müşteriye özel boyutlandırmaya ve müşteriye özel projelere izin verir ve bu projeler sunucu yapılandırmalarına yol açabilir ve bunlar sertifikalı olarak listelenmemiştir:

- [SAP HANA Sertifikalı Cihazlar](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/appliances.html)
- [SAP HANA sertifikalı IaaS platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

Çoğu durumda, bu müşteriye özel sunucu yapılandırmaları SAP sertifikalı sunucu birimlerinden daha fazla bellek taşır. SAP ile çalışırken, müşteriler SAP desteği alma ve müşteriye özel boyutlu sunucu yapılandırmaları için sertifika alma imkanına sahiptir. Azure'da, aşağıdaki HANA Büyük Örnek standart SUK'ları kullanılabilir ve bu TDIv5 müşteriye özel boyutlandırma projeleri için Microsoft fiyat listesinde kullanılabilir.

| SKU|CPU | Bellek | Depolama | Kullanılabilirlik |
| ---| --- | --- | --- | --- |
| S96 | Azure S96'da SAP HANA<br /> – 2 x Intel® Xeon® Işlemci E7-8890 v4<br /> 48 CPU çekirdeği ve 96 CPU iş parçacığı |  768 GB |  3 TB | Revision3 ve Revision4 pullarında mevcuttur|


| Olabilir Orijinal SKU <br /> bellekte genişletilmiş | CPU | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| S192m uzatılabilir | SAP HANA Üzerinde Azure S192xm<br /> – 4 x Intel® Xeon® Işlemci E7-8890 v4<br /> 96 CPU çekirdeği ve 192 CPU iş parçacığı |  6.0 TB |  16 TB | Artık teklif edilmedi |
| S384xm uzatılabilir | AZURE S384xxm üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Işlemci E7-8890 v4<br /> 192 CPU çekirdeği ve 384 CPU iş parçacığı |  12.0 TB |  28 TB | Revision4 pullar mevcuttur |
| S576m uzatılabilir | SAP HANA Üzerinde Azure S576xm<br /> – 12 x Intel® Xeon® Işlemci E7-8890 v4<br /> 288 CPU çekirdeği ve 576 CPU iş parçacığı |  18.0 TB |  41 TB | Revision4 pullar mevcuttur|
| S768m uzatılabilir | SAP HANA Üzerinde Azure S768xm<br /> – 16 x Intel® Xeon® Işlemci E7-8890 v4<br /> 384 CPU çekirdekleri ve 768 CPU iş parçacığı |  24.0 TB |  56 TB | Revision4 pullar mevcuttur |

- CPU çekirdekleri = sunucu biriminin işlemcilerinin toplamının hiper iş parçacığı olmayan CPU çekirdeklerinin toplamı.
- CPU iş parçacıkları = sunucu biriminin işlemcilerinin toplamının hiper dişli CPU çekirdekleri tarafından sağlanan bilgi işlem ipliklerinin toplamı. Çoğu birim, Hyper-Threading Teknolojisini kullanmak üzere varsayılan olarak yapılandırılır.
- Tedarikçi önerilerise S768m, S768xm ve S960m, SAP HANA'yı çalıştırmak için Hyper-Threading'i kullanacak şekilde yapılandırılmamıştır.


Seçilen belirli yapılandırmalar iş yüküne, CPU kaynaklarına ve istenen belleğe bağlıdır. OLTP iş yükünün OLAP iş yükü için optimize edilmiş SKU'ları kullanması mümkündür. 

Müşteriye özel boyutlandırma projelerinin birimleri hariç tekliflerin donanım tabanı SAP HANA TDI sertifikalıdır. İki farklı donanım sınıfı SNU'ları aşağıdakilere böler:

- S72, S72m, S96, S144, S144m, S192, S192xm, S224 ve S224m, SNU'ların "Tip I sınıfı" olarak adlandırılır.
- S384, S384m, S384xm, S384xxm, S576m, S576xm S768m, S768xm ve S960m, Hangi SUS "Tip II sınıfı" olarak adlandırılır.
- Optane ile 4,5 TB'dan 9TB'ye kadar sunan diğer S224 SK'larla ilgileniyorsanız, daha fazla bilgi almak için Microsoft hesap ekibinize başvurun. 


Tam bir HANA Büyük Örnek damgası yalnızca tek bir müşteri&#39;kullanımı için tahsis edilmemiştir. Bu gerçek, Azure'da dağıtılan bir ağ dokusu üzerinden bağlanan bilgi işlem ve depolama kaynaklarının rafları için de geçerlidir. Azure gibi HANA Büyük Örnek altyapısı, &quot;aşağıdaki&quot; üç düzeyde birbirinden yalıtılmış farklı müşteri kiracıları dağıtır:

- **Ağ**: HANA Büyük Örnek damgası içindeki sanal ağlar üzerinden izolasyon.
- **Depolama**: Depolama hacimleri atanmış ve kiracılar arasında depolama hacimlerini yalıtmak depolama sanal makineleri aracılığıyla yalıtım.
- **İşlem**: Sunucu birimlerinin tek bir kiracıya özel ataması. Sunucu birimlerinin sert veya yumuşak bölümleme yok. Kiracılar arasında tek bir sunucu veya ana bilgisayar biriminin paylaşıcısı yok. 

HANA Büyük Örnek birimlerinin farklı kiracılar arasındaki dağıtımları birbirleri tarafından görülemez. Farklı kiracılarda dağıtılan HANA Büyük Örnek birimleri, HANA Büyük Örnek damga düzeyinde birbirleriyle doğrudan iletişim kuramıyor. Sadece bir kiracı içindeki HANA Büyük Örnek birimleri HANA Büyük Örnek damga düzeyinde birbirleriyle iletişim kurabilir.

Büyük Örnek damgasında dağıtılan bir kiracı, faturalandırma amacıyla bir Azure aboneliğine atanır. Bir ağ için, aynı Azure kaydındaki diğer Azure aboneliklerinin sanal ağlarından erişilebilir. Aynı Azure bölgesinde başka bir Azure aboneliğiyle dağıtım yaptığınızda, ayrılmış bir HANA Büyük Örnek kiracısını da isteyebilirsiniz.

HANA Büyük Örneği'nde SAP HANA'yı çalıştırmakla Azure'da dağıtılan VM'lerde çalışan SAP HANA arasında önemli farklar vardır:

- Azure'da SAP HANA için sanallaştırma katmanı yoktur (Büyük Örnekler). Altta yatan çıplak metal donanımın performansını elde elabilirsiniz.
- Azure'un aksine, Azure'daki SAP HANA (Büyük Örnekler) sunucusu belirli bir müşteriye adanmıştır. Sunucu biriminin veya ana bilgisayarın sert veya yumuşak bölümlenmiş olması mümkün değildir. Sonuç olarak, bir HANA Büyük Örnek birimi bir kiracıya bir bütün olarak atanmış olarak ve bu da size göre kullanılır. Sunucunun yeniden başlatılması veya kapatılması otomatik olarak işletim sistemine ve SAP HANA'nın başka bir sunucuda dağıtılmasına yol açmaz. (Tip I sınıfı SK'ler için tek istisna, bir sunucunun sorunlarla karşılaşması ve yeniden dağıtımın başka bir sunucuda gerçekleştirilmesi gerektiğidir.)
- Ana bilgisayar işlemci türlerinin en iyi fiyat/performans oranı için seçildiği Azure'un aksine, Azure'da SAP HANA (Büyük Örnekler) için seçilen işlemci türleri Intel E7v3 ve E7v4 işlemci serisinin en yüksek performansıdır.

**Sonraki adımlar**
- Bkz. [HLI Boyutlandırma](hana-sizing.md)
