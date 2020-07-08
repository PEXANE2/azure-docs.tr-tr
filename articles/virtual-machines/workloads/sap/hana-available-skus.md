---
title: Azure 'da SAP HANA için SKU 'Lar (büyük örnekler) | Microsoft Docs
description: Azure 'da SAP HANA için SKU 'Lar (büyük örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/10/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7a487b105c8edc34d9427de3b8ca6738da1855a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84672111"
---
# <a name="available-skus-for-hli"></a>HLI için kullanılabilir SKU'lar

Azure 'da (büyük örnekler) yalnızca düzeltme 3 damgalarına dayalı olarak SAP HANA, Azure bölgelerindeki çeşitli yapılandırmalarda kullanılabilir:

- Doğu Avustralya
- Güneydoğu Avustralya
- Doğu Japonya
- Batı Japonya

Düzeltme 4 damgalarına dayanan Azure (büyük örnekler) hizmetinde SAP HANA, Azure bölgelerindeki çeşitli yapılandırmalarda mevcuttur:

- Batı ABD 2
- Doğu ABD
- Doğu ABD 2
- Orta Güney ABD
- Batı Avrupa
- Kuzey Avrupa



[LISANSLı Hana büyük örnek listesi SAP HANA,](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) örneğin:

| SAP çözümü | Model | Bellek | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- |
| [OLAP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2185), [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2265) | Azure S96 üzerinde SAP HANA<br /> – 2 x Intel® Xeon® Işlemci E7-8890 v4 <br /> 48 CPU çekirdeği ve 96 CPU iş parçacığı |  768 GB |  3,0 TB | Kullanılabilir |
| [OLAP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2186), [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2269) | Azure S224 üzerinde SAP HANA<br /> – 4 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 112 CPU çekirdeği ve 224 CPU iş parçacığı |  3,0 TB |  6,3 TB | Kullanılabilir |
| [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2297) | Azure S224m üzerinde SAP HANA<br /> – 4 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 112 CPU çekirdeği ve 224 CPU iş parçacığı |  6,0 TB |  10,5 TB | Kullanılabilir |
| [OLAP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=1983), [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2268) | Azure S384 üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Işlemci E7-8890 v4<br /> 192 CPU çekirdeği ve 384 CPU iş parçacığı |  4,0 TB |  16 TB | Kullanılabilir |
|[OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2080)| Azure S384xm üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Işlemci E7-8890 v4<br /> 192 CPU çekirdeği ve 384 CPU iş parçacığı |  6,0 TB |  18 TB | Kullanılabilir (yalnızca 4. Rev) |
| TDIv5 | Azure S384m üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Işlemci E7-8890 v4<br /> 192 CPU çekirdeği ve 384 CPU iş parçacığı |  12,0 TB |  28 TB | Kullanılabilir |
|[OLAP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=1984), [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2267) | Azure S384xm üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Işlemci E7-8890 v4<br /> 192 CPU çekirdeği ve 384 CPU iş parçacığı |  8,0 TB |  22 TB |  Kullanılabilir  |
| [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2049) | Azure S576m üzerinde SAP HANA<br /> – 12 x Intel® Xeon® Işlemci E7-8890 v4<br /> 288 CPU çekirdeği ve 576 CPU iş parçacığı |  12,0 TB |  28 TB | Kullanılabilir (yalnızca 4. Rev) |
| TDIv5 | Azure S576xm üzerinde SAP HANA<br /> – 12 x Intel® Xeon® Işlemci E7-8890 v4<br /> 288 CPU çekirdeği ve 576 CPU iş parçacığı |  18,0 TB |  41 TB | Kullanılabilir |
| [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=1985) | Azure S768m üzerinde SAP HANA<br /> – 16 x Intel® Xeon® Işlemci E7-8890 v4<br /> 384 CPU çekirdeği ve 768 CPU iş parçacığı |  16,0 TB |  36 TB | Kullanılabilir (yalnızca 4. Rev) |
| TDIv5 | Azure S768xm üzerinde SAP HANA<br /> – 16 x Intel® Xeon® Işlemci E7-8890 v4<br /> 384 CPU çekirdeği ve 768 CPU iş parçacığı |  24,0 TB |  56 TB | Kullanılabilir |
| [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=1986) | Azure S960m üzerinde SAP HANA<br /> – 20 x Intel® Xeon® Işlemci E7-8890 v4<br /> 480 CPU çekirdeği ve 960 CPU iş parçacığı |  20,0 TB |  46 TB | Kullanılabilir (yalnızca 4. Rev) |
| [OLTP](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure&recordid=2328) | Azure S896m üzerinde SAP HANA<br /> – 16 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 448 CPU çekirdeği ve 896 CPU iş parçacığı | 24,0 TB | 35,8 TB | Kullanılabilir (yalnızca 4. Rev) |

- CPU çekirdekleri = sunucu biriminin işlemcilerin toplamının hiper iş parçacıklı olmayan CPU çekirdekleri toplamı.
- CPU iş parçacıkları = sunucu biriminin işlemcilerin toplamının hiper iş parçacıklı CPU çekirdekleri tarafından sunulan işlem iş parçacıklarının toplamı. Çoğu birim, varsayılan olarak hiper Iş parçacığı teknolojisini kullanacak şekilde yapılandırılır.
- Üretici S768m, S768xm ve S960m temel alınarak SAP HANA çalıştırmak için hiper Iş parçacığı kullanacak şekilde yapılandırılmamıştır.


SAP HANA TDIv5 ' nin altında SAP, müşteriye özgü boyutlandırma ve müşteriye özgü projelere izin verir ve bu, içinde sertifikalı olarak listelenmeyen sunucu yapılandırmalarına yol açabilir:

- [Sertifikalı gereçler SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/appliances.html)
- [Sertifikalı IaaS platformlarını SAP HANA](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)

Çoğu durumda, bu müşteriye özgü sunucu yapılandırmalarının SAP ile sertifikalı sunucu birimlerinden daha fazla bellek taşıması gerekir. SAP ile birlikte çalışarak müşteriler, müşterilere özgü boyutlardaki sunucu yapılandırmalarına yönelik SAP desteği ve onaylama olanağı sağlar. 

Ayrıca, SAP ile sertifikalı olmayan aşağıdaki büyük örnek standart SKU 'Ları mevcuttur ve satın alma için Microsoft fiyat listesinde:

 | Model | Toplam bellek | Bellek DRAM | Bellek Optane | Depolama | Kullanılabilirlik |
| --- | --- | --- | --- | --- | --- |
| Azure S224oo üzerinde SAP HANA<br /> – 4 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 112 CPU çekirdeği ve 224 CPU iş parçacığı | 4,5 TB |  1,5 TB |  3,0 TB | 8,4 TB | Kullanılabilir |
| Azure S224om üzerinde SAP HANA<br /> – 4 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 112 CPU çekirdeği ve 224 CPU iş parçacığı | 6,0 TB |  3,0 TB |  3,0 TB | 10,5 TB | Kullanılabilir |
| Azure S224ooo üzerinde SAP HANA<br /> – 4 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 112 CPU çekirdeği ve 224 CPU iş parçacığı | 7,5 TB |  1,5 TB |  6,0 TB | 12,7 TB | Kullanılabilir |
| Azure S224oom üzerinde SAP HANA<br /> – 4 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 112 CPU çekirdeği ve 224 CPU iş parçacığı | 9,0 TB |  3,0 TB |  6,0 TB | 14,8 TB | Kullanılabilir |
| Azure S448 üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 224 CPU çekirdeği ve 448 CPU iş parçacığı | 6,0 TB |  6,0 TB |  --- | 10,5 TB | Kullanılabilir (yalnızca 4. Rev) |
| Azure S448m üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 224 CPU çekirdeği ve 448 CPU iş parçacığı | 12,0 TB |  12,0 TB |  --- | 18,9 TB | Kullanılabilir (yalnızca 4. Rev) |
| Azure S448oo üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 224 CPU çekirdeği ve 448 CPU iş parçacığı | 9,0 TB |  3,0 TB |  6,0 TB | 14,8 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S448om üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 224 CPU çekirdeği ve 448 CPU iş parçacığı | 12,0 TB |  6,0 TB |  6,0 TB | 18,9 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S448ooo üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 224 CPU çekirdeği ve 448 CPU iş parçacığı | 15,0 TB |  3,0 TB |  12,0 TB | 23,2 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S448oom üzerinde SAP HANA<br /> – 8 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 224 CPU çekirdeği ve 448 CPU iş parçacığı | 18,0 TB |  6,0 TB |  12,0 TB | 27,4 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S672 üzerinde SAP HANA<br /> – 12 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 336 CPU çekirdeği ve 672 CPU iş parçacığı | 9,0 TB |  9,0 TB |  --- | 14,7 TB | Kullanılabilir (yalnızca 4. Rev) |
| Azure S672m üzerinde SAP HANA<br /> – 12 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 336 CPU çekirdeği ve 672 CPU iş parçacığı | 18,0 TB |  18,0 TB |  --- | 27,4 TB | Kullanılabilir (yalnızca 4. Rev) |
| Azure S672oo üzerinde SAP HANA<br /> – 12 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 336 CPU çekirdeği ve 672 CPU iş parçacığı | 13,5 TB |  4,5 TB |  9,0 TB | 21,1 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S672om üzerinde SAP HANA<br /> – 12 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 336 CPU çekirdeği ve 672 CPU iş parçacığı | 18,0 TB |  9,0 TB |  9,0 TB | 27,4 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S672ooo üzerinde SAP HANA<br /> – 12 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 336 CPU çekirdeği ve 672 CPU iş parçacığı | 22,5 TB |  4,5 TB |  18,0 TB | 33,7 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S672oom üzerinde SAP HANA<br /> – 12 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 336 CPU çekirdeği ve 672 CPU iş parçacığı | 27,0 TB |  9,0 TB |  18,0 TB | 40,0 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S896 üzerinde SAP HANA<br /> – 16 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 448 CPU çekirdeği ve 896 CPU iş parçacığı | 12,0 TB |  12,0 TB |  --- | 18,9 TB | Kullanılabilir (yalnızca 4. Rev) |
| Azure S896oo üzerinde SAP HANA<br /> – 16 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 448 CPU çekirdeği ve 896 CPU iş parçacığı | 18,0 TB |  6,0 TB |  12,0 TB | 27,4 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S896om üzerinde SAP HANA<br /> – 16 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 448 CPU çekirdeği ve 896 CPU iş parçacığı | 24,0 TB |  12,0 TB |  12,0 TB | 35,8 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S896ooo üzerinde SAP HANA<br /> – 16 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 448 CPU çekirdeği ve 896 CPU iş parçacığı | 30,0 TB |  6,0 TB |  24,0 TB | 44,3 TB  | Kullanılabilir (yalnızca 4. Rev) |
| Azure S896oom üzerinde SAP HANA<br /> – 16 x Intel® Xeon® Platinum 8276 işlemcisi <br /> 448 CPU çekirdeği ve 896 CPU iş parçacığı | 36,0 TB |  12,0 TB |  24,0 TB | 52,7 TB  | Kullanılabilir (yalnızca 4. Rev) |


> [!IMPORTANT]
> Yine de desteklenmekle birlikte, şu SKU 'Lar artık satın alınamaz: s72, S72m, S144, S144m, S192 ve S192m 

Seçilen belirli yapılandırma iş yüküne, CPU kaynaklarına ve istenen belleğe bağımlıdır. OLTP iş yükünün OLAP iş yükü için en iyi duruma getirilmiş SKU 'Ları kullanabilmesi mümkündür. 

Müşterilere özgü boyutlandırma projelerine yönelik birimler hariç, tekliflere yönelik donanım tabanı, TDı sertifikalı SAP HANA. İki farklı donanım sınıfı, SKU 'Ları içine böler:

- S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224 ve S224m, S224oo, S224om, S224ooo, S224oom SKU 'ların "Type ı Class" olarak adlandırılır.
- Diğer tüm SKU 'lar SKU 'ların "tür II sınıfı" olarak adlandırılır.
- Henüz SAP donanım dizininde listelenmeyen SKU 'Lar ile ilgileniyorsanız daha fazla bilgi edinmek için Microsoft hesabı ekibinize başvurun. 


Tüm HANA büyük örnek damgası, tek bir müşteri&#39;kullanımı için özel olarak ayrılmaz. Bu olgu, Azure 'da dağıtılan bir ağ dokusuna göre bağlanmış işlem ve depolama kaynaklarının raflarına yöneliktir. Azure gibi HANA büyük örnek altyapısı, &quot; &quot; aşağıdaki üç düzeyde bir diğerinden yalıtılmış farklı müşteri kiracılar dağıtır:

- **Ağ**: Hana büyük örnek damgasında sanal ağlar aracılığıyla yalıtım.
- **Depolama**: depolama birimleri atanmış depolama sanal makineler aracılığıyla yalıtım yapın ve depolama birimlerini kiracılar arasında yalıtın.
- **İşlem**: sunucu birimlerinin tek bir kiracıya adanmış atanması. Sunucu birimlerinin sabit veya geçici bölümlendirme yok. Kiracılar arasında tek bir sunucu veya konak birimi paylaşımı yoktur. 

Farklı kiracılar arasında HANA büyük örnek birimlerinin dağıtımları birbirleriyle görülemez. Farklı kiracılarda dağıtılan HANA büyük örnek birimleri, HANA büyük örnek damgası düzeyinde birbirleriyle doğrudan iletişim kuramaz. Yalnızca bir kiracının içindeki HANA büyük örnek birimleri, HANA büyük örnek damgası düzeyinde birbirleriyle iletişim kurabilir.

Büyük örnek damgasındaki dağıtılan bir kiracı, Faturalama amacıyla bir Azure aboneliğine atanır. Bir ağ için aynı Azure kaydı içindeki diğer Azure aboneliklerinin sanal ağlarından erişilebilir. Aynı Azure bölgesindeki başka bir Azure aboneliğiyle dağıtım yaparsanız, ayrılmış bir HANA büyük örnek kiracısı isteyip istemediğinizi de tercih edebilirsiniz.

HANA büyük örnekteki SAP HANA çalıştırma ve Azure 'da dağıtılan VM 'lerde çalışan SAP HANA arasında önemli farklılıklar vardır:

- Azure 'da SAP HANA için sanallaştırma katmanı yoktur (büyük örnekler). Temel alınan çıplak donanımın performansını alırsınız.
- Azure 'dan farklı olarak, Azure 'daki SAP HANA (büyük örnekler) sunucu belirli bir müşteriye ayrılmıştır. Sunucu biriminin veya konağın sabit veya geçici olarak bölümlenmiş olması mümkün değildir. Sonuç olarak, bir kiracıya ve sizin için bir bütün olarak atanan bir HANA büyük örnek birimi kullanılır. Sunucunun yeniden başlatılması veya kapatılmasını, işletim sistemine otomatik olarak ve SAP HANA başka bir sunucuya dağıtılmayacak. (Tür ı sınıfı SKU 'Ları için tek istisna, bir sunucunun sorunlarla karşılaştığında ve yeniden dağıtımı başka bir sunucuda gerçekleştirilmesi gerekir.)
- En iyi fiyat/performans oranı için ana bilgisayar işlemcisi türlerinin seçildiği Azure 'dan farklı olarak, Azure 'da SAP HANA için seçilen işlemci türleri (büyük örnekler), Intel E7v3 ve E7v4 işlemci hattının en yüksek gerçekleştirmesinden oluşur.

**Sonraki adımlar**
- [HLi boyutlandırmayı](hana-sizing.md) inceleyin
