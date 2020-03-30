---
title: Azure sanal makinelerde ana bilgisayar yeniden barındırma
description: Microsoft Azure'da sanal makineleri (VM' ler) kullanarak IBM Z tabanlı sistemler gibi ana bilgisayar iş yüklerinizi yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: ccdeefabeedfca4959239696361ccce0bc6c1c78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76289807"
---
# <a name="mainframe-rehosting-on-azure-virtual-machines"></a>Azure sanal makinelerde ana bilgisayar yeniden barındırma

İş yüklerini ana bilgisayar ortamlarından buluta geçirmek, altyapınızı modernize etmenizi ve genellikle maliyetlerden tasarruf etmenizi sağlar. Birçok iş yükü, yalnızca küçük kod değişiklikleriyle (veritabanlarının adlarını güncelleştirme gibi) Azure’a aktarılabilir.

Genel olarak konuşursak, *ana bilgisayar* terimi büyük bir bilgisayar sistemi anlamına gelir. Özellikle, şu anda kullanılmakta olan büyük çoğunluğu, MVS, DOS, VSE, OS/390 veya z/OS çalıştıran IBM System Z sunucuları veya IBM plug-uyumlu sistemlerdir.

Azure sanal makinesi (VM), belirli bir uygulamanın kaynaklarını tek bir örnekte yalıtmak ve yönetmek için kullanılır. IBM z/OS gibi ana bilgisayarlar bu amaçla Mantıksal Bölümler (LPARS) kullanır. Bir ana bilgisayar, ilişkili COBOL programlarına sahip bir CICS bölgesi için bir LPAR ve IBM Db2 veritabanı için ayrı bir LPAR kullanabilir. [Azure'daki](/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) tipik bir n katmanlı uygulama, Azure VM'lerini her katman için alt ağlara bölilebilen sanal bir ağa dağır.

Azure VM'ler, kaldırma ve kaydırma senaryolarını destekleyen ana bilgisayar öykünme ortamlarını ve derleyicileri çalıştırabilir. Geliştirme ve sınama genellikle bir ana bilgisayardan Azure geliştirme/test ortamına geçiş yapmak için ilk iş yükleri arasındadır. Öykünebileceğiniz ortak sunucu bileşenleri arasında çevrimiçi işlem işlemi (OLTP), toplu iş ve aşağıdaki şekilde veri alma sistemleri yer alır.

![Azure'daki öykünme ortamları z/OS tabanlı sistemleri çalıştırmanızı sağlar.](media/01-overview.png)

Bazı ana bilgisayar iş yükleri göreceli kolaylıkla Azure'a geçirilebilirken, diğerleri iş ortağı çözümü kullanılarak Azure'da yeniden barındırılabilir. Azure [Ana Bilgisayar Geçiş merkezi,](https://azure.microsoft.com/migration/mainframe/) iş ortağı çözümü seçme hakkında ayrıntılı rehberlik için yardımcı olabilir.

## <a name="mainframe-migration"></a>Ana bilgisayar geçişi

Yeniden ev sahipliği mi, yeniden inşa etmek, değiştirmek veya emekli olmak mı? IaaS mı PaaS mı? Ana bilgisayar uygulamanız için doğru geçiş stratejisini belirlemek için Azure Mimari Merkezi'ndeki [Ana Bilgisayar geçiş](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview) kılavuzuna bakın.

## <a name="micro-focus-rehosting-platform"></a>Micro Focus yeniden barındırma platformu

Micro Focus Enterprise Server mevcut en büyük ana bilgisayar barındırma platformlarından biridir. Azure'da daha ucuz bir x86 platformunda z/OS iş yüklerinizi çalıştırabilirsiniz.

Başlamak için:

- [Azure'da Kurumsal Sunucu ve Kurumsal Geliştirici Yükleme](./microfocus/set-up-micro-focus-azure.md)
- [Azure'da Kurumsal Geliştirici için CICS BankDemo'u ayarlama](./microfocus/demo.md)
- [Azure'da Docker Konteynerinde Enterprise Server'ı Çalıştırın](./microfocus/run-enterprise-server-container.md)


## <a name="tmaxsoft-openframe-on-azure"></a>Azure'da TmaxSoft OpenFrame

TmaxSoft OpenFrame, kaldırma ve kaydırma senaryolarında kullanılan popüler bir ana bilgisayar barındırma çözümüdür. Azure'daki OpenFrame ortamı geliştirme, demolar, testler veya üretim iş yükleri için uygundur.

Başlamak için:

- [TmaxSoft OpenFrame ile başlayın](./tmaxsoft/get-started.md)
- [E-kitabı indirin](https://azure.microsoft.com/resources/install-tmaxsoft-openframe-on-azure/)

## <a name="ibm-zdt-120"></a>IBM zD&T 12.0

IBM Z Geliştirme ve Test Ortamı (IBM zD&T), Azure'da z/OS tabanlı uygulamaların geliştirilmesi, test edilmesi ve demoları için kullanabileceğiniz üretim dışı bir ortam ayarlar.

Azure'daki öykünme ortamı, Uygulama Geliştiricileri Kontrollü Dağıtımlar (ADCD'ler) aracılığıyla farklı türde Z örnekleribarındırabilir. Azure ve Azure Yığını'nda ZD&T Personal Edition, zD&T Parallel Sysplex ve zD&T Enterprise Edition çalıştırabilirsiniz.

Başlamak için:

- [Azure'da IBM zD&T 12.0'ı ayarlama](./ibm/install-ibm-z-environment.md)
- [ZD&T'de ADCD'yi ayarlama](./ibm/demo.md)

## <a name="ibm-db2-purescale-on-azure"></a>Azure'da IBM DB2 pureScale

IBM DB2 pureScale ortamı Azure için bir veritabanı kümesi sağlar. Orijinal ortamla aynı değildir, ancak Paralel Sysplex kurulumunda çalışan z/OS için IBM DB2 ile benzer kullanılabilirlik ve ölçek sağlar.

Başlamak için [Azure'da IBM DB2 pureScale'e](/azure/virtual-machines/linux/ibm-db2-purescale-azure)bakın.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Ana bilgisayar iş yüklerini hizmet olarak Azure altyapısına (IaaS) geçirdiğinizde, Azure VM'leri de dahil olmak üzere isteğe bağlı, ölçeklenebilir bilgi işlem kaynakları nın çeşitli türleri arasından seçim yapabilirsiniz. Azure, çeşitli [Linux](/azure/virtual-machines/linux/overview) ve [Windows](/azure/virtual-machines/windows/overview) VM'leri sunar.

### <a name="compute"></a>İşlem

Azure işlem gücü, bir ana bilgisayarın kapasitesiyle olumlu karşılaştırır. Bir ana bilgisayar iş yükünü Azure'a taşımayı düşünüyorsanız, saniyede bir milyon talimatın (MIPS) ana bilgisayar metrik ölçüsünün sanal CPU'larla karşılaştırın. 

[Ana bilgisayar bilgiişlemini Azure'a](./concepts/mainframe-compute-azure.md)nasıl taşıyabilirsiniz öğrenin.

### <a name="high-availability-and-failover"></a>Yüksek kullanılabilirlik ve başarısız

Azure, taahhüt tabanlı hizmet düzeyi anlaşmaları (SLA'lar) sunar. Çoklu dokuzlu kullanılabilirlik varsayılandır ve SLA'lar hizmetlerin yerel veya coğrafi tabanlı çoğaltılmasıyla optimize edilebilir. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

VM gibi Azure IaaS ile, belirli sistem işlevleri başarısız destek sağlar (örneğin, başarısız kümeleme örnekleri ve kullanılabilirlik kümeleri). Azure platform'u hizmet (PaaS) kaynakları olarak kullandığınızda, platform otomatik olarak başarısız olur. Bunlara örnek olarak [Azure SQL Veritabanı](/azure/sql-database/sql-database-technical-overview) ve Azure [Cosmos DB](/azure/cosmos-db/introduction)verilebilir.

### <a name="scalability"></a>Ölçeklenebilirlik

Ana bilgisayarlar genellikle ölçeklendirilirken, bulut ortamları ölçeklendirilir. Azure, ihtiyaçlarınızı karşılamak için çeşitli [Linux](/azure/virtual-machines/linux/sizes) ve [Windows](/azure/virtual-machines/windows/sizes) boyutları sunar. Bulut ayrıca, tam kullanıcı belirtimlerini eşleşecek şekilde yukarı veya aşağı ölçeklendirilir. Kullanım tabanlı faturalandırma modeli [scale](/azure/architecture/best-practices/auto-scaling) altında talep üzerine güç, depolama ve hizmetleri ölçeklendirin.

### <a name="storage"></a>Depolama

Bulutta, esnek, ölçeklenebilir depolama seçenekleri sunar ve yalnızca ihtiyacınız olan şey için ödeme yapmışsınızdır. [Azure Depolama,](/azure/storage/common/storage-introduction) veri nesneleri için büyük ölçüde ölçeklenebilir bir nesne deposu, bulut için bir dosya sistemi hizmeti, güvenilir bir mesajlaşma mağazası ve bir NoSQL mağazası sunar. VM'ler için yönetilen ve yönetilmeyen diskler kalıcı, güvenli disk depolama sağlar.

[Ana bilgisayar depolama alanını Azure'a](./concepts/mainframe-storage-azure.md)nasıl taşıyacağım öğrenin.

### <a name="backup-and-recovery"></a>Yedekleme ve kurtarma

Kendi olağanüstü durum kurtarma sitesi bakımı pahalı bir teklif olabilir. Azure, yerel veya bölgesel düzeylerde veya coğrafi artıklık yoluyla [yedekleme,](/azure/backup/backup-introduction-to-azure-backup) [kurtarma](/azure/site-recovery/site-recovery-overview)ve [artıklık](/azure/storage/common/storage-redundancy) için uygulanması kolay ve uygun maliyetli seçeneklere sahiptir.

## <a name="azure-government-for-mainframe-migrations"></a>Ana bilgisayar geçişleri için Azure Genel

Birçok kamu sektörü kuruluşu ana bilgisayar uygulamalarını daha modern ve esnek bir platforma taşımak ister. Microsoft Azure Kamu, federal, eyalet ve yerel yönetim sistemleri için paketlenmiş olan genel Microsoft Azure platformunun fiziksel olarak ayrılmış bir örneğidir. Özellikle Amerika Birleşik Devletleri devlet kurumları ve ortakları için birinci sınıf güvenlik, koruma ve uyumluluk hizmetleri sağlar.

Azure Hükümeti, bu tür ortama ihtiyaç duyan sistemler için FedRAMP High Impact için Geçici Çalışma Yetkisi (P-ATO) kazanmıştır.

Başlamak [için ana bilgisayar uygulamaları için Microsoft Azure Devlet bulutu indirin.](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/en-us/)

## <a name="next-steps"></a>Sonraki adımlar

[Ortaklarımızdan](partner-workloads.md) ana bilgisayar uygulamalarınızı geçirmenize veya yeniden barındırmanıza yardımcı olmasını isteyin. 

Ayrıca bkz:

- [Ana bilgisayar konuları hakkında teknik incelemeler](mainframe-white-papers.md)
- [Ana bilgisayar geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Sorun giderme](/azure/virtual-machines/troubleshooting/)
- [Azure geçişiiçin ana bilgisayarın mymystifying](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: /ibm/install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md
