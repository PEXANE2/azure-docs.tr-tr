---
title: Oracle Veritabanı ile Azure NetApp Dosyalarını Kullanmanın Avantajları | Microsoft Dokümanlar
description: Teknolojiyi açıklar ve Oracle Direct NFS (dNFS) ile geleneksel NFS istemcisi arasında bir performans karşılaştırması sağlar. Azure NetApp Dosyaları ile dNFS kullanmanın avantajlarını gösterir.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: b-juche
ms.openlocfilehash: a73da39dafcc8be78fbe1c023693ffa4a19aa1d3
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085017"
---
# <a name="benefits-of-using-azure-netapp-files-with-oracle-database"></a>Oracle Database ile Azure NetApp Files kullanmanın avantajları

Oracle Direct NFS (dNFS), işletim sisteminin kendi NFS sürücüsünden daha yüksek performans alabilmek için mümkün kılar. Bu makalede, teknoloji açıklar ve dNFS ve geleneksel NFS istemcisi (Çekirdek NFS) arasında bir performans karşılaştırması sağlar. Ayrıca Azure NetApp Dosyaları ile dNFS kullanmanın avantajlarını ve kolaylığını gösterir.  

## <a name="how-oracle-direct-nfs-works"></a>Oracle Direct NFS nasıl çalışır?

Oracle Direct NFS (dNFS), işletim sisteminin arabellek önbelleğini atlar. Veriler, kullanıcı alanında yalnızca bir kez önbelleğe çıkarve bellek kopyalarının ek yükü ortadan kaldırır.  

Geleneksel NFS istemcisi, aşağıdaki örnekte görüldüğü gibi tek bir ağ akışı kullanır: 

![Tek bir ağ akışı kullanarak geleneksel NFS istemcisi](../media/azure-netapp-files/solutions-traditional-nfs-client-using-single-network-flow.png)

Buna karşılık, Oracle dNFS birden çok ağ akışı nda ağ trafiğini yük dengeleme yaparak performansı artırır. Bu özellik, Oracle veritabanının aşağıdaki örnekte gösterildiği gibi, önemli sayıda 650 farklı ağ bağlantısı oluşturmasına olanak tanır:  

![Oracle Direct NFS performansı artırıyor](../media/azure-netapp-files/solutions-oracle-direct-nfs-performance-load-balancing.png)

[Doğrudan NFS için Oracle SSS,](http://www.orafaq.com/wiki/Direct_NFS) Oracle dNFS'nin optimize edilmiş bir NFS istemcisi olduğunu gösterir. NAS depolama aygıtlarında bulunan NFS depolama alanına (TCP/IP üzerinden erişilebilir) hızlı ve ölçeklenebilir erişim sağlar. dNFS, asm gibi veritabanı çekirdeğinde yerleşiktir ve bu çekirdek öncelikle DAS veya SAN depolama ile kullanılır. Bu nedenle, *kılavuz NAS depolama uygularken dNFS kullanmak ve SAN depolama uygularken ASM kullanmaktır.*

dNFS, Oracle 18c'deki varsayılan seçenektir.

dNFS Oracle Database 11g ile başlayarak kullanılabilir. Aşağıdaki diyagram, dNFS ile yerli NFS karşılaştırılır. DNFS kullandığınızda, Azure sanal makinesinde çalışan bir Oracle veritabanı, yerel NFS istemcisinden daha fazla G/Ç kullanabilir.

![Oracle ve Azure NetApp Dosyaları dNFS ile yerli NFS karşılaştırması](../media/azure-netapp-files/solutions-oracle-azure-netapp-files-comparing-dnfs-native-nfs.png)

İki komut çalıştırıp veritabanını yeniden başlatarak dNFS'yi etkinleştirebilir veya devre dışı kullanabilirsiniz.

Etkinleştirmek için:  
`cd $ORACLE_HOME/rdbms/lib ; make -f ins_rdbms.mk dnfs_on`

Devre dışı kalmak için:  
`cd $ORACLE_HOME/rdbms/lib ; make -f ins_rdbms.mk dnfs_off`

## <a name="azure-netapp-files-combined-with-oracle-direct-nfs"></a>Azure NetApp Dosyaları Oracle Direct NFS ile birleştirildi

Azure NetApp Files hizmeti ile Oracle dNFS'nin performansını artırabilirsiniz. Hizmet, uygulama performansınız üzerinde tam denetim sağlar. Son derece zorlu uygulamaları karşılayabilir. Oracle dNFS ile Azure NetApp Dosyaları'nın birleşimi iş yükleri için büyük avantaj sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files kullanan çözüm mimarileri](azure-netapp-files-solution-architectures.md)
- [Azure'da Oracle Uygulamalarına ve çözümlerine genel bakış](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-overview)
