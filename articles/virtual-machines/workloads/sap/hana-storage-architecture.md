---
title: SAP HANA'nın Azure'da depolama mimarisi (Büyük Örnekler) | Microsoft Dokümanlar
description: SAP HANA'nın Azure'da (Büyük Örnekler) nasıl dağıtılanabildiğini depolama mimarisi.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/20/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a12c454906d6c6ff702b7f635a91361bbe3994c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616883"
---
# <a name="sap-hana-large-instances-storage-architecture"></a>SAP HANA (Büyük Örnekler) depolama mimarisi

Azure'daki SAP HANA'nın depolama düzeni (Büyük Örnekler), SAP önerilen yönergelere göre klasik dağıtım modelinde SAP HANA tarafından yapılandırılır. Yönergeler [SAP HANA depolama gereksinimleri](https://go.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) teknik incelemede belgelenmiştir.

Tip I sınıfının HANA Büyük Örneği, depolama birimi olarak bellek hacminin dört katı ile birlikte gelir. HANA Büyük Örnek birimlerinin Type II sınıfı için depolama dört kat daha fazla değildir. Birimler HANA hareket günlüğü yedeklemelerini depolamak için tasarlanmış bir hacimle birlikte gelir. Daha fazla bilgi için [Azure'da SAP HANA'yı (Büyük Örnekler) yükle ve yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)ya da yapılandırma 'ya bakın.

Depolama ayırma açısından aşağıdaki tabloya bakın. Tablo, farklı HANA Büyük Örnek birimleriyle sağlanan farklı birimlerin kaba kapasitesini listeler.

| HANA Büyük Örnek SKU | hana/veri | hana/kütük | hana/paylaşılan | hana/logbackups |
| --- | --- | --- | --- | --- |
| S72 | 1.280 GB | 512 GB | 768 GB | 512 GB |
| S72m | 3.328 GB | 768 GB |1.280 GB | 768 GB |
| S96 | 1.280 GB | 512 GB | 768 GB | 512 GB |
| S192 | 4.608 GB | 1.024 GB | 1.536 GB | 1.024 GB |
| S192m | 11.520 GB | 1.536 GB | 1.792 GB | 1.536 GB |
| S192xm |  11.520 GB |  1.536 GB |  1.792 GB |  1.536 GB |
| S224 |  4.224 GB |  512 GB |  1.024 GB |  512 GB |
| S224m |  8.448 GB |  512 GB |  1.024 GB |  512 GB |
| S384 | 11.520 GB | 1.536 GB | 1.792 GB | 1.536 GB |
| S384m | 12.000 GB | 2.050 GB | 2.050 GB | 2.040 GB |
| S384xm | 16.000 GB | 2.050 GB | 2.050 GB | 2.040 GB |
| S384xxm |  20.000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S576m | 20.000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S576xm | 31.744 GB | 4.096 GB | 2.048 GB | 4.096 GB |
| S768m | 28.000 GB | 3.100 GB | 2.050 GB | 3.100 GB |
| S768xm | 40.960 GB | 6.144 GB | 4.096 GB | 6.144 GB |
| S960m | 36.000 GB | 4.100 GB | 2.050 GB | 4.100 GB |


Dağıtılan gerçek birimler, dağıtıma ve birim boyutlarını göstermek için kullanılan araca bağlı olarak değişebilir.

Bir HANA Büyük Örnek SKU'ya bölerseniz, olası bölme parçalarının birkaç örneği aşağıdaki gibi görünebilir:

| GB'de bellek bölümü | hana/veri | hana/kütük | hana/paylaşılan | hana/log/yedekleme |
| --- | --- | --- | --- | --- |
| 256 | 400 GB | 160gb | 304 GB | 160gb |
| 512 | 768 GB | 384 GB | 512 GB | 384 GB |
| 768 | 1.280 GB | 512 GB | 768 GB | 512 GB |
| 1,024 | 1.792 GB | 640 GB | 1.024 GB | 640 GB |
| 1536 | 3.328 GB | 768 GB | 1.280 GB | 768 GB |


Bu boyutlar, dağıtıma ve birimlere bakmak için kullanılan araçlara göre biraz değişiklik yapabilen kaba hacim numaralarıdır. 2,5 TB gibi başka bölüm boyutları da vardır. Bu depolama boyutları, önceki bölümler için kullanılana benzer bir formülle hesaplanır. "Bölümler" terimi, işletim sisteminin, belleğin veya CPU kaynaklarının herhangi bir şekilde bölümlenmiş olduğu anlamına gelmez. Tek bir HANA Büyük Örnek biriminde dağıtmak isteyebileceğin farklı HANA örneklerinin depolama bölümlerini gösterir. 

Daha fazla depolama alanına ihtiyacınız olabilir. 1-TB birimlerinde ek depolama alanı satın alarak depolama alanı ekleyebilirsiniz. Bu ek depolama ek birim olarak eklenebilir. Ayrıca, varolan birimlerden bir veya daha fazlasını genişletmek için de kullanılabilir. Özgün olarak dağıtılan ve çoğunlukla önceki tablolar tarafından belgelenen birimlerin boyutlarını azaltmak mümkün değildir. Ayrıca, birimlerin adlarını veya montaj adlarını değiştirmek de mümkün değildir. Daha önce açıklanan depolama birimleri HANA Büyük Örnek birimlerine NFS4 birimleri olarak eklenir.

Yedekleme ve geri yükleme ve olağanüstü durum kurtarma amacıyla depolama anlık görüntülerini kullanabilirsiniz. Daha fazla bilgi için, [Azure'da SAP HANA (Büyük Örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bilgisine bakın.

Senaryonuz için depolama düzeni ayrıntıları için [HLI destekli senaryolara](hana-supported-scenario.md) bakın.

## <a name="run-multiple-sap-hana-instances-on-one-hana-large-instance-unit"></a>Bir HANA Büyük Örnek biriminde birden çok SAP HANA örneğini çalıştırma

HANA Büyük Örnek birimlerinde birden fazla etkin SAP HANA örneğini barındırmak mümkündür. Depolama anlık görüntüleri ve olağanüstü durum kurtarma yeteneklerini sağlamak için, böyle bir yapılandırma örnek başına bir birim kümesi gerektirir. Şu anda, HANA Büyük Örnek birimleri aşağıdaki gibi alt bölümlere ayrılabilir:

- **S72, S72m, S96, S144, S192**: 256 GB'lık artışlarla, 256 GB ile en küçük başlangıç ünitesidir. 256 GB ve 512 GB gibi farklı artışlar ünitenin bellek maksimum birleştirilebilir.
- **S144m ve S192m**: 256 GB'lık artışlarla, en küçük birim 512 GB'dır. 512 GB ve 768 GB gibi farklı artışlar ünitenin bellek maksimum kombine edilebilir.
- **Tip II sınıfı**: 512 GB'lık artışlarla, en küçük başlangıç ünitesi 2 TB'dir. 512 GB, 1 TB ve 1,5 TB gibi farklı artışlar ünitenin belleği maksimumuna kadar birleştirilebilir.

Birden çok SAP HANA örneğini çalıştırmaya birkaç örnek aşağıdaki gibi görünebilir.

| SKU | Bellek boyutu | Depolama boyutu | Birden çok veritabanına sahip boyutlar |
| --- | --- | --- | --- |
| S72 | 768 GB | 3 TB | 1x768-GB HANA örneği<br /> veya 1x512-GB örneği + 1x256-GB örneği<br /> veya 3x256-GB örnekleri | 
| S72m | 1,5 TB | 6 Veste | 3x512GB HANA örnekleri<br />veya 1x512-GB örneği + 1x1-TB örneği<br />veya 6x256-GB örnekleri<br />veya 1x1.5-TB örneği | 
| S192m | 4 TB | 16 TB | 8x512-GB örnekleri<br />veya 4x1-TB örnekleri<br />veya 4x512-GB örnekleri + 2x1-TB örnekleri<br />veya 4x768-GB örnekleri + 2x512-GB örnekleri<br />veya 1x4-TB örneği |
| S384xm | 8 TB | 22 TB | 4x2-TB örnekleri<br />veya 2x4-TB örnekleri<br />veya 2x3-TB örnekleri + 1x2-TB örnekleri<br />veya 2x2.5-TB örnekleri + 1x3-TB örnekleri<br />veya 1x8-TB örneği |


Başka varyasyonlar da vardır. 

## <a name="encryption-of-data-at-rest"></a>Verilerin istirahatte şifrelemi
HANA Large Instance için kullanılan depolama, 2018 yılı sonundan itibaren disklerde depolanan veriler için saydam şifreleme kullanır. Önceki dağıtımlarda, birimleri nşifreetmesini seçebilirsiniz. Bu seçeneğin karşı olduğuna karar verdiyseniz, birimleri çevrimiçi olarak şifrelemek için talepte bulunabilirsiniz. Şifrelenmemiş birimlerden şifrelenmiş birimlere geçiş saydamdır ve kapalı kalma süresi gerektirmez. 

SNU'ların Tip I sınıfıile, önyükleme LUN'un depolanan hacmi şifrelenir. Revizyon 3 HANA Büyük Örnek pullarında, HANA Büyük Örnek'in SK'lerinin Type II sınıfını kullanarak, önyükleme LUN'u işletim sistemi yöntemleriyle şifrelemeniz gerekir. Revizyon 4 HANA Büyük Örnek pullarında, Tip II birimleri kullanılarak boparan LUN depolanır ve varsayılan olarak istirahatte de şifrelenir. 

## <a name="required-settings-for-larger-hana-instances-on-hana-large-instances"></a>HANA Büyük Örnekleri'nde daha büyük HANA örnekleri için gerekli ayarlar
HANA Büyük Örnekleri'nde kullanılan depolama alanının dosya boyutu sınırlaması vardır. Boyut sınırlaması dosya başına [16 TB'dir.](https://docs.netapp.com/ontap-9/index.jsp?topic=%2Fcom.netapp.doc.dot-cm-vsmg%2FGUID-AA1419CF-50AB-41FF-A73C-C401741C847C.html) EXT3 dosya sistemlerindeki dosya boyutu sınırlamalarının aksine, HANA, HANA Büyük Örnekler depolama sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağısı yoktur. Sonuç olarak HANA, 16TB dosya boyutu sınırına ulaşıldığında otomatik olarak yeni bir veri dosyası oluşturmaz. HANA dosyayı 16 TB'nin ötesine büyütmeye çalışırken, HANA hataları bildirir ve dizin sunucusu sonunda çöker.

> [!IMPORTANT]
> HANA Büyük Örnek depolamanın 16 TB dosya boyutu sınırının ötesinde veri dosyalarını büyütmeye çalışan HANA'yı önlemek için, HANA'nın global.ini yapılandırma dosyasında aşağıdaki parametreleri ayarlamanız gerekir
> 
> - datavolume_striping=doğru
> - datavolume_striping_size_gb = 15000
> - Ayrıca bakınız SAP not [#2400005](https://launchpad.support.sap.com/#/notes/2400005)
> - SAP [not](https://launchpad.support.sap.com/#/notes/2631285) #2631285




**Sonraki adımlar**
- [HANA Büyük Örnekleri için Desteklenen senaryolara](hana-supported-scenario.md) bakın