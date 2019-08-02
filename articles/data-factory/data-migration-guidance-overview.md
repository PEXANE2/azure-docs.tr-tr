---
title: Data Lake ve veri ambarınızdan Azure 'a veri geçirmek için Azure Data Factory kullanın | Microsoft Docs
description: Data Lake ve veri ambarınızdan verileri Azure 'a geçirmek için Azure Data Factory kullanın.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 7/30/2019
ms.openlocfilehash: 780b9ae6e4664af86fa655c9136193bed58526d9
ms.sourcegitcommit: 6ad03fa28a0f60cb6dce6144f728c2ceb56ff6e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68708504"
---
# <a name="use-azure-data-factory-to-migrate-data-from-your-data-lake-or-data-warehouse-to-azure"></a>Data Lake veya veri ambarınızdan verileri Azure 'a geçirmek için Azure Data Factory kullanın 

Azure Data Factory Data Lake veya kurumsal veri Ambarınızı (EDW) Azure 'a geçirmek istediğinizde veri geçişini yapmak için araç olabilir. Data Lake geçişi ve veri ambarı geçişi, aşağıdaki senaryolarla ilgilidir: 

- AWS S3, şirket içi Hadoop dosya sisteminden Azure 'a büyük veri iş yükü geçişi. 
- Oracle sınavlarından (Netezza, Teradata, AWS Redshift 'den Azure 'a) EDW geçişi. 

Azure Data Factory, Data Lake geçişi için PBs ' veri düzeyini ve veri ambarı geçişi için on TB veri taşıyabilir. 

## <a name="why-azure-data-factory-can-be-used-for-data-migration"></a>Veri geçişi için neden Azure Data Factory kullanılabilir? 

- Azure Data Factory, yüksek performanslı, esnekliği ve ölçeklenebilirlik sayesinde verileri sunucusuz şekilde taşımak için horsegüç miktarını kolayca ölçeklendirebilir ve yalnızca kullandığınız kadar ödeyin.  
  - Azure Data Factory veri hacminde ve dosya sayısıyla ilgili bir sınırlama yoktur.
  - Azure Data Factory, ortamınızda en yüksek veri taşıma aktarım hızını elde etmek için ağınızı ve depolama bant genişliğinizi 100.   
  - Azure Data Factory Kullandıkça Öde stratejisinden sonra, yalnızca Azure 'a veri geçişi yapmak için Azure Data Factory kullandığınız zaman ödeme yapmanız gerekir.  
- Azure Data Factory, tek seferlik bir geçmiş yükü ve zamanlanmış artımlı yükü gerçekleştirebilme olanağı vardır. 
- Azure Data Factory, verileri genel olarak erişilebilen Veri Gölü/ambar uç noktaları arasında taşımak için Azure IR kullanır veya alternatif olarak, sanal ağ veya güvenlik duvarının arkasındaki Data Lake/Warehouse uç noktaları için verileri taşımak üzere şirket içinde barındırılan IR kullanır 
- Azure Data Factory kurumsal düzeyde güvenliğe sahiptir: güvenli hizmetten hizmete tümleştirme için MSI veya hizmet kimliğini kullanın veya alternatif olarak kimlik bilgileri yönetimi için Azure Key Vault yararlanın. 
- Azure Data Factory, kod ücretsiz yazma deneyimi ve zengin yerleşik izleme panosu sağlar.  

## <a name="online-vs-offline-data-migration"></a>Çevrimiçi ve çevrimdışı veri geçişi

Azure Data Factory, verileri ağ üzerinden (Internet, ER veya VPN) aktarmaya yönelik tipik bir çevrimiçi veri geçiş aracıdır. Bu, çevrimdışı veri geçişinin, kullanıcıların kuruluşunuzdaki veri aktarımı cihazlarını Azure veri merkezine fiziksel olarak göndermesine olanak sağlar.  

Çevrimiçi ve çevrimdışı geçiş yaklaşımı seçerken dikkat etmeniz gereken üç önemli noktalar vardır:  

- Geçirilecek verilerin boyutu. 
- Ağ bant genişliği. 
- Geçiş penceresi.   

Veri geçişini iki hafta içinde (geçiş penceresi) gerçekleştirmek istiyorsanız, farklı veri boyutu ve ağ bant genişliğine sahip çevrimiçi geçiş aracı (Azure Data Factory) kullanmanın ne zaman iyi olduğunu göstermek için aşağıdaki resimde kesme satırı görebilirsiniz.   

![çevrimiçi ve çevrimdışı](media/data-migration-guidance-overview/online-offline.png)

> [!NOTE]
> Çevrimiçi geçiş yaklaşımının avantajı, geçmiş veri yükleme ve artımlı akışların her ikisinin de bir araç ile sona erdirmek için bir arada elde etmenizi sağlar.  Bunu yaptığınızda veriler, tüm geçiş penceresi sırasında mevcut ve yeni mağaza arasında eşitlenmiş durumda tutularak, ETL mantığınızı yenilenen verilerle yeni depoda yeniden oluşturabilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory ile birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md)