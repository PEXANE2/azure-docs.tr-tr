---
title: "Azure'da SAP: Azure'da hangi SAP yazılımı desteklenir"
description: Azure'da dağıtılmak üzere hangi SAP yazılımının desteklenir
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/16/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0aaa13ff4d3331378cc17cd0cde29be43822397
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460798"
---
# <a name="what-sap-software-is-supported-for-azure-deployments"></a>Azure dağıtımları için hangi SAP yazılımı desteklenir
Bu makalede, Azure dağıtımları için hangi SAP yazılımının desteklendirilip desteklenebileceği ve gerekli işletim sistemi sürümlerinin veya DBMS sürümlerinin ne olduğu açıklanmaktadır.

Geçerli SAP yazılımınızın desteklenip desteklenmediğini ve Azure'daki SAP yazılımınızla hangi işletim sistemi ve DBMS sürümlerinin desteklenip desteklenmediğini değerlendirirken, aşağıdakilere erişmeniz gerekecektir:

- SAP destek notları
- SAP Ürün kullanılabilirliği Matrisi



## <a name="general-restrictions-for-sap-workload"></a>SAP iş yükü için genel kısıtlamalar
SAP iş yükü için kullanılabilecek Azure IaaS hizmetleri x86-64 veya x64 donanımı ile sınırlıdır. SAP iş yükü için geçerli olan Sparc veya Power CPU tabanlı teklifler yoktur. IBM ana bilgisayarı veya AS400 gibi donanım mimarilerine özel işletim sistemlerinde veya HP-UX, Solaris veya AIX işletim sistemlerinin kullanıldığı işletim sistemlerinde çalışan müşteriler, DBMS dahil sap uygulamalarını aşağıdaki işletim sistemleri:

- X86-64 platformu için Windows sunucusu 64bit
- X86-64 platformu için SUSE linux 64bit
- X86-64 platformu için Red hat Linux 64Bit
- X86-64 platformu için Oracle Linux 64bit

SAP yazılımıyla birlikte, başka işletim sistemi sürümleri veya Linux dağıtımları desteklenmez. Belirli sürümler ve servis talepleri yle ilgili kesin ayrıntılar daha sonra belgede belgelenmiştir.


## <a name="you-start-here"></a>Buradan başla.
Sizin için başlangıç noktası [SAP destek notu #1928533.](https://launchpad.support.sap.com/#/notes/1928533) Bu SAP notunu yukarıdan aşağıya doğru ilerlerken, desteklenen yazılımın ve VM'lerin çeşitli alanları gösterilir

İlk bölümde, genel olarak Azure VM'lerde SAP yazılımıyla desteklenen işletim sürümleri için minimum gereksinimler listelenir. Bu minimum gereksinimlere ulaşamıyor sanız ve bu işletim sistemlerinin eski sürümlerini çalıştıramıyorsanız, işletim sistemi sürümünüzü böyle bir minimum sürümveya daha yeni sürümlere yükseltmeniz gerekir. Azure'un genel olarak bu işletim sistemlerinden bazılarının eski sürümlerini destekleyeceği doğrudur. Ancak listelenen kısıtlamalar veya minimum sürümler, yürütülen testlere ve niteliklere dayanır ve daha geriye doğru uzatılmayacak. 


> [!NOTE]
>Daha yeni işletim sistemi sürümleri gerektirecek bazı özel VM türleri, HANA Büyük Örnekleri veya SAP iş yükleri vardır. Bu gibi durumlarda belge boyunca belirtilecektir. Bu gibi durumlar SAP notlarında veya diğer SAP yayınlarında açıkça belgelenmiştir.

Aşağıdaki bölümde, desteklenen sürümlerle desteklenen genel SAP platformları ve desteklenen SAP çekirdekleri daha önemlidir. Desteklenen Ve, minimum çekirdek sürümleri gereken NetWeaver / ABAP veya Java yığınları listeler. Daha yeni ABAP yığınları Azure'da desteklenir, ancak Azure için değişiklikler daha yeni yığınların geliştirilmesinin başlangıcından itibaren uygulandığından minimum çekirdek yayımlanmasına gerek yoktur

Kontrol et:

- Çalıştırdığınız SAP uygulamalarının belirtilen minimum sürümler kapsamında olup olmadığı. Değilse, yeni bir hedef sürümü tanımlamanız, SAP Ürün Kullanılabilirlik Matrisi'ni, hangi işletim sisteminin oluşturduğunu ve DBMS kombinasyonlarının yeni hedef sürümüyle desteklenmelerini kontrol etebilirsiniz. Böylece, doğru işletim sistemi sürümü ve DBMS sürümü seçebilirsiniz
- SAP çekirdeklerinizi Azure'a taşırken güncelleştirmeniz gerekip gerekmediği
- SAP Destek Paketlerini güncelleştirmeniz gerekip gerekmediği. Özellikle daha yeni bir DBMS sürümüne geçmeniz gereken durumlar için gerekli olabilecek Temel Destek Paketleri


Bir sonraki bölümde, Windows ve Linux için Azure'da SAP tarafından desteklenen diğer SAP ürünleri ve DBMS sürümleri hakkında daha fazla ayrıntı yer almaktadır. 

> [!NOTE]
> Farklı DBMS'nin minimum sürümleri dikkatle seçilir ve dbms satıcılarının genel olarak Azure'daki farklı DBMS satıcılarının desteğini her zaman tüm spektrumunu yansıtmayabilir. Bu minimum sürümleri tanımlamak için sap iş yükü ile ilgili birçok hususlar dikkate alınmıştır. Eski DBMS sürümlerini test etmek ve nitelemek için hiçbir çaba yoktur. 

> [!NOTE]
> Listelenen minimum sürümler işletim sistemlerinin ve veritabanı sürümlerinin eski sürümünü temsil eder. Biz son derece en son işletim sistemi sürümleri ve veritabanı sürümleri kullanmayı teşvik ediyoruz. Birçok durumda, daha yeni işletim sistemi ve veritabanı sürümleri, genel bulutta veya daha özel Azure'da çalıştırmak için optimize edilecek şekilde genel bulutta çalıştırma nın kullanım durumunu ve uyarlanmış kodu dikkate aldı

## <a name="oracle-dbms-support"></a>Oracle DBMS desteği
İşletim sistemi, Oracle DBMS sürümleri ve Azure'da desteklenen Oracle işlevleri özellikle [SAP destek notunda](https://launchpad.support.sap.com/#/notes/2039619)#2039619 listelenmiştir. Bu notun özü şöyle özetlenebilir:

- NetWeaver için sertifikalı Azure VM'lerde desteklenen minimum Oracle sürümü Oracle 11g Release 2 Patchset 3 (11.2.0.4)
- Konuk işletim sistemleri olarak sadece Windows ve Oracle Linux hak kazanır. İşletim sistemi ve ilgili minimum DBMS sürümlerinin tam sürümleri notta listelenmiştir
- Oracle Linux desteği Oracle DBMS istemcisine de uzanır. Bu, ABAP veya Java Stack'in iletişim örnekleri gibi tüm SAP bileşenlerinin Oracle Linux'ta da çalışması gerektiği anlamına gelir. Oracle DBMS'ye bağlanmayan böyle bir SAP sistemi içindeki yalnızca SAP bileşenlerinin farklı bir Linux işletim sistemi çalıştırmasına izin verilir
- Oracle RAC desteklenmiyor 
- Oracle ASM bazı servis talepleri için desteklenir. Ayrıntılar notta listelenir
- Unicode olmayan SAP sistemleri yalnızca Windows konuk işletim sistemiyle çalışan uygulama sunucularıyla desteklenir. DBMS'nin konuk işletim sistemi Oracle Linux veya Windows olabilir. SAP Ürün Kullanılabilirlik Matrisi (PAM) denetlenirken bu kısıtlamanın nedeni belirgindir. Oracle Linux için SAP, Unicode olmayan SAP çekirdeklerini hiç yayımlamadı

Hedeflenen Azure altyapısıyla desteklenen DBMS sürümlerini bilmek, işletim sistemi sürümlerinin ve gerekli DBMS'nin çalıştırmak istediğiniz SAP ürün sürümleriyle desteklenip desteklenmediğini kontrol etmek için SAP Ürün Kullanılabilirliği Matrisi'ni kontrol etebilirsiniz. 


## <a name="sap-hana-support"></a>SAP HANA desteği
Azure'da HANA veritabanını çalıştırmak için kullanılabilecek iki hizmet vardır:

- Azure Sanal Makineler
- [HANA Büyük Örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)

SAP HANA'yı çalıştırmak için SAP, NetWeaver veya diğer SAP uygulamalarını ve DBMS'yi çalıştırmak için altyapının karşılaması gereken daha fazla ve daha güçlü koşullara sahiptir. Sonuç olarak, daha az sayıda Azure VM SAP HANA DBMS'yi çalıştırahak kazanır. SAP HANA için desteklenen desteklenen Azure altyapısının listesi SAP [HANA donanım dizininde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)bulunabilir. 

> [!NOTE]
> 'S' harfi ile başlayan birimler [HANA Büyük Örnekler](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) birimleridir. 

> [!NOTE]
> SAP' nin SAP HANA ana sürümlerine bağlı olarak belirli bir sertifikası yoktur. Ortak görüşün aksine, [HANA sertifikalı IaaS platformlarda](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)sütun **Sertifikasyon senaryo** , sütun **HANA büyük veya küçük sürüm sertifikalı hakkında hiçbir açıklama**yapar . Belirli birimler için sertifikalı işletim sistemi sürümleri HANA 1.0 sürümleri tarafından da desteklenir sürece HANA 1.0 ve HANA 2.0 için kullanılabilecek listelenen tüm birimleri varsaymak gerekir. 

SAP HANA kullanımı için, genel NetWeaver durumlarına göre farklı minimum işletim sistemi sürümleri uygulanabilir. Her birim için desteklenen işletim sistemlerini ayrı ayrı kontrol etmeniz gerekir, çünkü bunlar değişebilir. Bunu her üniteye tıklayarak yaparsınız. Daha fazla ayrıntı görüntülenir. Listelenen ayrıntılardan biri, bu özel birim için desteklenen farklı işletim sistemleridir.

> [!NOTE]
> Azure HANA Büyük Örnek birimleri, desteklenen işletim sistemleriyle Azure VM'lere göre daha kısıtlayıcıdır. Öte yandan Azure VM'ler en az sürüm olarak daha yeni işletim sürümleri uygulayabilir. Bu özellikle Linux çekirdeklerinde değişiklik gerektiren daha büyük VM birimlerinin bazıları için geçerlidir

Azure altyapısı için desteklenen işletim sistemi bilmek, sap [destek notu #2235581](https://launchpad.support.sap.com/#/notes/2235581) tam SAP HANA sürümleri ve hedeflediğiniz Azure birimleri yle desteklenen yama düzeyleri için kontrol etniz gerekir. 

> [!IMPORTANT]
> Tam SAP HANA sürümleri ve desteklenen yama düzeyleri kontrol adımı çok önemlidir. Birçok durumda, belirli bir işletim sistemi sürümü desteği SAP HANA yürütülebilir belirli bir yama düzeyine bağlıdır.

Hedeflenen Azure altyapısında çalıştırabileceğiniz belirli HANA sürümlerini bildiğiniz den sonra, filtrelediğiniz HANA sürümlerini destekleyen SAP ürün sürümlerinde kısıtlamalar olup olmadığını öğrenmek için SAP Ürün Kullanılabilirliği Matrisi'ni kontrol etmeniz gerekir


## <a name="certified-azure-vms-and-hana-large-instance-units-and-business-transaction-throughput"></a>Sertifikalı Azure VM'ler ve HANA Büyük Örnek birimleri ve iş hareketi çıktısı
Desteklenen işletim sistemi sürümlerini, DBMS sürümlerini ve Azure altyapı birimleri için bağımlı destek SAP yazılım sürümlerini değerlendirmenin yanı sıra, bu birimleri 'SAP' biriminde ifade edilen iş işlem çıktısı ile niteleme niz gerekir Sap. Tüm SAP boyutlandırması SAPS hesaplamalarına bağlıdır. Varolan SAP sistemlerini değerlendirirken, genellikle altyapı sağlayıcınızın yardımıyla birimlerin SAPS'sini hesaplayabilirsiniz. DBMS katmanı için olduğu kadar uygulama katmanı için de. Yeni işlevselliğin oluşturulduğu diğer durumlarda, SAP ile boyutlandırma alıştırması uygulama katmanı ve DBMS katmanı için gerekli SAPS numaralarını ortaya çıkarabilir. Altyapı sağlayıcısı olarak Microsoft, NetWeaver ve/veya HANA sertifikalı farklı birimlerin SAP iş birimi karakterizasyonunu sağlamakla yükümlüdür.

Azure VM'ler için bu SAPS iş ortası numaraları [SAP destek notu #1928533](https://launchpad.support.sap.com/#/notes/1928533)belgelenmiştir. Azure HANA Büyük Örnek birimleri için SAPS iş çıktısı numaraları [SAP destek notunda](https://launchpad.support.sap.com/#/notes/2316233) #2316233

SAP [destek notu #1928533](https://launchpad.support.sap.com/#/notes/1928533)baktığımızda aşağıdaki açıklamalar geçerlidir:

- **M-Serisi Azure VM'ler ve Mv2 Serisi Azure VM'ler için, diğer Azure VM türlerine göre farklı minimum işletim sistemi sürümleri geçerlidir.** Daha yeni işletim sistemi sürümleri gereksinimi, farklı işletim sistemi satıcılarının işletim sistemi sürümlerinde belirli Azure VM türlerinde çalışan işletim sistemlerini etkinleştirmek veya performansı optimize etmek için sağlamak zorunda olduğu değişikliklere bağlıdır ve bu VM türlerinde SAP iş yükünün iş bilgili
- Farklı VM türlerini belirten iki tablo vardır. İkinci tablo, yalnızca Azure standart Depolama'yı destekleyen Azure VM türleri için SAPS iş ortasını belirtir. Notun ikinci tablosunda belirtilen birimler üzerinde DBMS dağıtımı desteklenmiyor


## <a name="other-sap-products-supported-on-azure"></a>Azure'da desteklenen diğer SAP ürünleri
Genel olarak varsayım, Azure gibi hiper ölçekli bulutların durumuyla, SAP yazılımlarının çoğunun Azure'da işlevsel sorunlar olmadan çalışması gerektiğidir. Yine de ve özel bulut görselleştirmesinin tam tersi olan SAP, farklı hyerpscale bulut sağlayıcıları için farklı SAP ürünlerine yönelik desteği açıkça ifade etmektedir. Sonuç olarak, farklı SAP ürünleri için Azure desteğini gösteren farklı SAP destek notları vardır. 

İş Nesneleri BI platformu için [SAP destek notu #2145537](https://launchpad.support.sap.com/#/notes/2145537) Azure'da desteklenen SAP İş Nesneleri ürünlerinin listesini verir. Listelenen veya desteklenmemiş gibi görünen ve listelenen minimum sürümlerden daha yeni olan yazılım sürümleri ve işletim sistemi sürümlerinin bileşenleri veya kombinasyonları hakkında sorular varsa, sorguladığınız bileşene karşı bir SAP destek isteği açmanız gerekir için destek.

İş Nesneleri Veri Hizmetleri için [SAP destek notu #22288344,](https://launchpad.support.sap.com/#/notes/2288344) Azure'da çalışan SAP Veri Hizmetleri'nin minimum desteğini açıklar. 

> [!NOTE]
> SAP destek notunda belirtildiği gibi, Azure'da desteklenecek doğru destek paketi düzeyini belirlemek için SAP PAM'i kontrol etmeniz gerekir

Azure Kubernetes Hizmetleri'ndeki (AKS) SAP Datahub/Vora desteği [SAP destek notunda](https://launchpad.support.sap.com/#/notes/2464722) #2464722

SAP BPC 10.1 SP08 [desteği SAP destek notunda](https://launchpad.support.sap.com/#/notes/2451795) #2451795

Azure'da SAP Hybris Commerce Platform 5.x ve 6.x desteği [Hybris Wiki'de](https://wiki.hybris.com/display/SUP/Using+the+hybris+Platform+with+the+Cloud) ayrıntılı olarak açıklanmaktadır


## <a name="next-steps"></a>Sonraki Adımlar
[SAP NetWeaver için Azure Sanal Makineler planlaması ve uygulamasında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) sonraki adımları okuyun

