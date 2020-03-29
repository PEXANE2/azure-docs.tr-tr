---
title: Ana bilgisayar bilgiişlemini Azure Sanal Makineleri'ne taşıyın
description: Azure bilgi işlem kaynakları, IBM z14 uygulamalarını geçirebilmeniz ve modernize edebilmeniz için ana bilgisayar kapasitesiyle olumlu bir karşılaştırma sağlar.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 97f354d0a313d58c671366dd0e5f485504823e13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76288940"
---
# <a name="move-mainframe-compute-to-azure"></a>Ana bilgisayar işlem işlem'ini Azure'a taşıma

Ana bilgisayarlar yüksek güvenilirlik ve kullanılabilirlik için bir üne sahiptir ve birçok işletmenin güvenilir omurgası olmaya devam etmektedir. Genellikle neredeyse sınırsız ölçeklenebilirlik ve bilgi işlem gücüne sahip oldukları düşünülmektedir. Ancak, bazı işletmeler mevcut en büyük ana bilgisayarların kapasitesini aştı. Bu sizin gibi yse, Azure çeviklik, erişim ve altyapı tasarrufu sunar.

Microsoft Azure'da ana bilgisayar iş yüklerini çalıştırmak için ana bilgisayarınızın işlem yeteneklerinin Azure ile nasıl karşılaştırılacağını bilmeniz gerekir. IBM z14 ana bilgisayarına (bu yazının en güncel modeli) dayanan bu makale, Azure'da nasıl karşılaştırılabilir sonuçlar elde abileceğinizi anlatır.

Başlamak için ortamları yan yana düşünün. Aşağıdaki şekilde, uygulamaları çalıştırmak için bir ana bilgisayar ortamını Azure barındırma ortamıyla karşılaştırın.

![Azure hizmetleri ve öykünme ortamları karşılaştırılabilir destek sunar ve geçişi kolaylaştırır](media/mainframe-compute-azure.png)

Ana bilgisayarların gücü genellikle binlerce kullanıcı için milyonlarca güncelleştirmeyi işleyen çevrimiçi işlem işleme (OLTP) sistemleri için kullanılır. Bu uygulamalar genellikle işlem işleme, ekran işleme ve form girişi için yazılım kullanır. Müşteri Bilgi Kontrol Sistemi (CICS), Bilgi Yönetim Sistemi (IMS) veya İşlem Arabirimi Paketi (TIP) kullanabilirler.

Şekildeki de görüldüğü gibi, Azure'daki bir TPM emülatörü CICS ve IMS iş yüklerini işleyebilir. Azure'daki bir toplu iş sistemi emülatörü İş Denetimi Dili (JCL) rolünü gerçekleştirir. Ana bilgisayar verileri Azure SQL Veritabanı gibi Azure veritabanlarına taşınır. Azure hizmetleri veya Azure Sanal Makineleri'nde barındırılan diğer yazılımlar sistem yönetimi için kullanılabilir.

## <a name="mainframe-compute-at-a-glance"></a>Bir bakışta ana bilgisayar hesaplama

Z14 ana bilgisayarında işlemciler en fazla dört *çekmecehalinde*düzenlenir. *Çekmece* sadece işlemci ler ve yonga setleri kümesidir. Her çekmecede altı aktif merkezi işlemci (CP) yongası olabilir ve her CP'de 10 sistem denetleyici (SC) yongası bulunur. Intel x86 terminolojisinde çekmece başına altı yuva, soket başına 10 çekirdek ve dört çekmece vardır. Bu mimari, bir z14 için en fazla 24 yuva ve 240 çekirdek li kaba eşdeğeri sağlar.

Hızlı z14 CP 5.2 GHz saat hızına sahiptir. Genellikle, bir z14 kutusunda tüm CP'ler ile teslim edilir. Gerektiği gibi aktif hale getirilmişler. Bir müşteri, gerçek kullanıma rağmen ayda en az dört saatlik işlem süresi için ücretlendirilir.

Bir ana bilgisayar işlemcisi aşağıdaki türlerden biri olarak yapılandırılabilir:

- Genel Amaçlı (GP) işlemci
- Sistem z Entegre Bilgi İşlemci (zIIP)
- Linux (IFL) işlemci için Entegre Tesis
- Sistem Yardımcı İşlemci (SAP)
- Entegre Kaplin Tesisi (ICF) işlemci

## <a name="scaling-mainframe-compute-up-and-out"></a>Ana bilgisayar hesaplamasını ölçekleme

IBM ana bilgisayarları 240 çekiye kadar ölçeklendirme olanağı sunar (tek bir sistem için geçerli z14 boyutu). Ayrıca, IBM ana bilgisayarları Kaplin Tesisi (CF) adı verilen bir özellik üzerinden ölçeklenebilir. CF, birden fazla ana bilgisayar sisteminin aynı anda aynı verilere erişmesine izin verir. Ana bilgisayar Paralel Sysplex teknolojisi, CF'yi kullanarak ana bilgisayar işlemcilerini kümeler halinde gruplabir araya getirir. Bu kılavuz yazıldığında, Paralel Sysplex özelliği her biri 64 işlemciden oluşan 32 gruplandırmayı destekleydi. İşlem kapasitesini ölçeklendirmek için bu şekilde en fazla 2.048 işlemci gruplandırılabilir.

CF, bilgi işlem kümelerinin verileri doğrudan erişimle paylaşmasına olanak tanır. Bilgileri kilitlemek, önbellek bilgileri ve paylaşılan veri kaynakları nın listesini kilitlemek için kullanılır. Bir veya daha fazla C'yi kullanan Paralel Sysplex, "paylaşılan her şeyi" ölçeklendirme işlem kümesi olarak düşünülebilir. Bu özellikler hakkında daha fazla bilgi için IBM [Z'deki Paralel Sysplex'e](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources) ibm web sitesinden bakın.

Uygulamalar, hem ölçeklendirme performansı hem de yüksek kullanılabilirlik sağlamak için bu özellikleri kullanabilir. CICS'nin PARALEL Sysplex'i CF ile nasıl kullanabileceği hakkında bilgi için [IBM CICS ve Kaplin Tesisi: Beyond the Basics](https://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf) redbook'u indirin.

## <a name="azure-compute-at-a-glance"></a>Bir bakışta Azure hesaplama

Bazı insanlar yanlışlıkla Intel tabanlı sunucuların ana bilgisayarlar kadar güçlü olmadığını düşünüyor. Ancak, yeni çekirdek yoğun, Intel tabanlı sistemler ana bilgisayarlar kadar bilgi işlem kapasitesine sahiptir. Bu bölümde, bilgi işlem ve depolama için hizmet olarak Azure altyapısı (IaaS) seçenekleri açıklanmaktadır. Azure hizmet olarak platform (PaaS) seçenekleri de sağlar, ancak bu makale, karşılaştırılabilir ana bilgisayar kapasitesi sağlayan IaaS seçeneklerine odaklanır.

Azure Sanal Makineler, çeşitli boyutlarda ve türlerde bilgi işlem gücü sağlar. Azure'da, sanal cpu (vCPU) kabaca bir ana bilgisayardaki bir çeki eşittir.

Şu anda, Azure Sanal Makine boyutları aralığı 1 ile 128 vCPUs arasında dır. Sanal makine (VM) türleri belirli iş yükleri için optimize edilebiyi amaçlar. Örneğin, aşağıdaki liste VM türlerini (bu yazıitibariyle geçerli) ve önerilen kullanımlarını gösterir:

| Boyut     | Türü ve açıklaması                                                                 |
|----------|--------------------------------------------------------------------------------------|
| D Serisi | 64 vCPU ve 3,5 GHz'e kadar saat hızına sahip genel amaç                           |
| E-Serisi | 64 vCPUs'a kadar en iyi şekilde optimize edilmiş bellek                                                 |
| F Serisi | 64 vCPUs'a ve 3..7 GHz saat hızına kadar optimize edilmiş işlem                       |
| H Serisi | Yüksek performanslı bilgi işlem (HPC) uygulamaları için optimize edile                          |
| L Serisi | NoSQL gibi veritabanları tarafından desteklenen yüksek iş elde uygulamaları için optimize edilmiş depolama |
| M Serisi | 128 vCPUs'a kadar en büyük bilgi işlem ve bellek optimize edilmiş VM'ler                        |

Kullanılabilir Sanal Makineler hakkında ayrıntılı bilgi için [Virtual Machine serisine](https://azure.microsoft.com/pricing/details/virtual-machines/series/)bakın.

Bir z14 ana bilgisayarında 240 çekimetreye kadar sahip olabilir. Ancak, z14 ana bilgisayarları hemen hemen hiçbir zaman tek bir uygulama veya iş yükü için tüm çekirdekleri kullanmaz. Bunun yerine, bir ana bilgisayar iş yüklerini mantıksal bölümlere (LVR) ayırır ve LVR'lerin derecelendirmeleri vardır—MIPS (Milyonlarca Saniyeyle Talimat) veya MSU (Milyon Hizmet Birimi). Azure'da bir ana bilgisayar iş yükünü çalıştırmak için gereken karşılaştırılabilir VM boyutunu belirlerken, MIPS (veya MSU) derecelendirmesini faktöre edin.

Genel tahminler şunlardır:

-   vCPU başına 150 MIPS

-   İşlemci başına 1.000 MIPS

Bir LPAR'da belirli bir iş yükü için doğru VM boyutunu belirlemek için, önce vm'yi iş yükü için optimize edin. Daha sonra gerekli vCPUs sayısını belirleyin. Konservatif bir tahmin vCPU başına 150 MIPS'dir. Bu tahmine dayanarak, örneğin, 16 vCPUs'lu bir F serisi VM, 2.400 MIPS'li bir LPAR'dan gelen IBM Db2 iş yükünü kolayca destekleyebilir.

## <a name="azure-compute-scale-up"></a>Azure işlem ölçeği

M serisi VM'ler 128 vCPUs'a kadar ölçeklenebilir (bu makalenin yazıldığı tarihte). VCPU başına 150 MIPS konservatif tahmin kullanarak, M-serisi VM yaklaşık 19.000 MIPS eşittir. Bir ana bilgisayar için MIPS tahmin etmek için genel kural işlemci başına 1.000 MIPS olduğunu. Bir z14 ana bilgisayar 24 işlemciye kadar olabilir ve tek bir ana bilgisayar sistemi için yaklaşık 24.000 MIPS sağlayabilir.

En büyük tek z14 ana bilgisayarında Azure'daki en büyük VM'den yaklaşık 5.000 MIPS daha fazladır. Ancak iş yüklerinin nasıl dağıtılanolduğunu karşılaştırmak önemlidir. Bir ana bilgisayar sisteminin hem bir uygulaması hem de ilişkisel veritabanı varsa, genellikle her biri kendi LPAR'ında aynı fiziksel ana bilgisayarda dağıtılır. Azure'da aynı çözüm genellikle uygulama için bir VM ve veritabanı için ayrı, uygun boyutta bir VM kullanılarak dağıtılır.

Örneğin, bir M64 vCPU sistemi uygulamayı destekliyorsa ve veritabanı için bir M96 vCPU kullanılıyorsa, aşağıdaki şekilde yaklaşık 150 vCP'ye veya yaklaşık 24.000 MIPS'ye ihtiyaç duyulur.

![24.000 MIPS'nin iş yükü dağıtımlarını karşılaştırma](media/mainframe-compute-mips.png)

Yaklaşım, LGT'leri tek tek VM'lere geçirmektir. Ardından Azure, tek bir ana bilgisayar sisteminde dağıtılan çoğu uygulama için gereken boyuta kolayca ölçeklenir.

## <a name="azure-compute-scale-out"></a>Azure işlem ölçeği

Azure tabanlı bir çözümün avantajlarından biri ölçeklendirme yeteneğidir. Ölçekleme, neredeyse sınırsız bilgi işlem kapasitesini bir uygulama için kullanılabilir hale getirir. Azure, bilgi işlem gücünü ölçeklendirmek için birden çok yöntemi destekler:

- **Küme üzerinde yük dengelemesi.** Bu senaryoda, bir uygulama bir kümedeki birden çok VM arasında iş yükünü yaymak için bir [yük dengeleyici](/azure/load-balancer/load-balancer-overview) si veya kaynak yöneticisi kullanabilir. Daha fazla işlem kapasitesi gerekiyorsa, kümeye ek VM'ler eklenir.

- **Sanal makine ölçek setleri.** Bu seri çekim senaryosunda, bir uygulama VM kullanımına dayalı ek [bilgi işlem kaynaklarına](/azure/virtual-machine-scale-sets/overview) ölçeklenebilir. Talep düştüğünde, bir ölçek kümesindeki VM sayısı da düşerek bilgi işlem gücünün verimli kullanılmasını sağlar.

- **PaaS ölçekleme.** Azure PaaS teklifleri hesaplama kaynaklarını ölçeklendirin. Örneğin, [Azure Hizmet Kumaşı,](/azure/service-fabric/service-fabric-overview) istek hacmindeki artışları karşılamak için bilgi işlem kaynaklarını ayırır.

- **Kubernetes kümeleri.** Azure'daki uygulamalar, belirtilen kaynaklar için bilgi işlem hizmetleri için [Kubernetes kümelerini](/azure/aks/concepts-clusters-workloads) kullanabilir. Azure Kubernetes Service (AKS), Azure'da Kubernetes düğümlerini, havuzlarını ve kümelerini düzenleyen yönetilen bir hizmettir.

Bilgi işlem kaynaklarını ölçeklemek için doğru yöntemi seçmek için Azure ve ana bilgisayarların nasıl farklılık gösterir olduğunu anlamak önemlidir. Önemli olan, verilerin bilgi işlem kaynakları tarafından nasıl veya nasıl paylaşılabilen olduğudur. Azure'da veriler (varsayılan olarak) genellikle birden çok Sanal Bilgisayar tarafından paylaşılmaz. Veri paylaşımı, ölçeklendirilen bir bilgi işlem kümesindeki birden çok VM tarafından gerekliyse, paylaşılan verilerin bu işlevselliği destekleyen bir kaynakta olması gerekir. Azure'da, aşağıdaki bölümde belirtildiği gibi veri paylaşımı depolamayı içerir.

## <a name="azure-compute-optimization"></a>Azure hesaplama optimizasyonu

Azure mimarisinde her işlem katmanını en iyi duruma getirebilirsiniz. Her ortam için en uygun VM türünü ve özelliklerini kullanın. Aşağıdaki şekilde, Db2 kullanan bir CICS uygulamasını desteklemek için Azure'da VM'leri dağıtmak için bir potansiyel deseni gösterilmektedir. Birincil sitede, üretim, üretim öncesi ve test VM'leri yüksek kullanılabilirlik ile dağıtılır. İkincil site yedekleme ve olağanüstü durum kurtarma içindir.

Her katman uygun olağanüstü durum kurtarma hizmetleri de sağlayabilir. Örneğin, üretim ve veritabanı VM'leri sıcak veya sıcak bir kurtarma gerektirirken, geliştirme ve test VM'leri soğuk bir kurtarmayı destekler.

![Olağanüstü durum kurtarmayı destekleyen yüksek kullanılabilir dağıtım](media/mainframe-compute-dr.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ana bilgisayar geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Azure Sanal Makinelerde Ana Bilgisayar yeniden barındırma](/azure/virtual-machines/workloads/mainframe-rehosting/overview)
- [Ana bilgisayar depolama alanını Azure'a taşıma](mainframe-storage-Azure.md)

### <a name="ibm-resources"></a>IBM kaynakları

- [IBM Z'de Paralel Sysplex](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources)
- [IBM CICS ve Kaplin Tesisi: Temellerin Ötesinde](https://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf)
- [Db2 pureScale Özellik yüklemesi için gerekli kullanıcı oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)
- [Db2icrt - Örnek komutu oluşturma](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)
- [Db2 pureScale Kümelenmiş Veritabanı Çözümü](https://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)
- [IBM Veri Stüdyosu](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

### <a name="azure-government"></a>Azure Kamu

- [Ana bilgisayar uygulamaları için Microsoft Azure Genel bulutu](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/)
- [Microsoft ve FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/FedRAMP)

### <a name="more-migration-resources"></a>Daha fazla geçiş kaynağı

- [Azure Sanal Veri Merkezi Kaldırma ve Kaydırma Kılavuzu](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
- [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)
