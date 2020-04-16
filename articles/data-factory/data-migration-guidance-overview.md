---
title: Verileri veri gölünden ve veri ambarından Azure'a geçirme
description: Verileriniz ve veri ambarınızdaki verileri Azure'a geçirmek için Azure Veri Fabrikası'nı kullanın.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 7/30/2019
ms.openlocfilehash: 4408546c892299e5bbbc22b00a4b334c36eda616
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416442"
---
# <a name="use-azure-data-factory-to-migrate-data-from-your-data-lake-or-data-warehouse-to-azure"></a>Verilerinizi veri gölünüzden veya veri ambarınızdan Azure'a geçirmek için Azure Veri Fabrikası'nı kullanın

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Veri gölünüzü veya kurumsal veri ambarınızı (EDW) Microsoft Azure'a geçirmek istiyorsanız, Azure Veri Fabrikası'nı kullanmayı düşünün. Azure Veri Fabrikası aşağıdaki senaryolara uygundur:

- Amazon Basit Depolama Hizmeti'nden (Amazon S3) veya şirket içi Hadoop Dağıtılmış Dosya Sisteminden (HDFS) Azure'a büyük veri iş yükü geçişi
- Oracle Exadata, Netezza, Teradata veya Amazon Redshift'ten Azure'a EDW geçişi

Azure Veri Fabrikası, veri gölü geçişi için petabayt (PB) verileri ve veri ambarı geçişi için onlarca terabayt (TB) veri taşıyabilir.

## <a name="why-azure-data-factory-can-be-used-for-data-migration"></a>Azure Veri Fabrikası neden veri geçişi için kullanılabilir?

- Azure Veri Fabrikası, yüksek performans, esneklik ve ölçeklenebilirlik ile verileri sunucusuz bir şekilde taşımak için işlem gücü miktarını kolayca ölçeklendirebilir. Ve sadece kullandığın paraiçin para ödüyorsun. Ayrıca aşağıdakileri unutmayın: 
  - Azure Veri Fabrikası'nın veri hacmi veya dosya sayısı yla ilgili bir sınırlaması yoktur.
  - Azure Veri Fabrikası, ortamınızdaki en yüksek veri hareketi hacmini elde etmek için ağınızı ve depolama bant genişliğini tam olarak kullanabilir.
  - Azure Veri Fabrikası, yalnızca azure'a veri geçişini çalıştırmak için kullandığınız süreyi ödemeniz için kullandıkça öde yöntemi kullanır.  
- Azure Veri Fabrikası hem tek seferlik geçmiş yükleme yi hem de zamanlanmış artımlı yükleri gerçekleştirebilir.
- Azure Veri Fabrikası, verileri genel olarak erişilebilir veri gölü ve ambar bitiş noktaları arasında taşımak için Azure tümleştirme çalışma zamanı (IR) kullanır. Azure Sanal Ağı (VNet) içinde veya güvenlik duvarının arkasındaki veri gölü ve ambar uç noktaları için veri taşımak için kendi kendine barındırılan IR'yi de kullanabilir.
- Azure Veri Fabrikası'nın kurumsal sınıf güvenliği vardır: Güvenli hizmet-hizmet tümleştirmesi için Windows Installer (MSI) veya Hizmet Kimliği'ni kullanabilir veya kimlik bilgisi yönetimi için Azure Anahtar Kasası'nı kullanabilirsiniz.
- Azure Veri Fabrikası, sorunsuz bir yazma deneyimi ve zengin, yerleşik bir izleme panosu sağlar.  

## <a name="online-vs-offline-data-migration"></a>Çevrimiçi ve çevrimdışı veri geçişi

Azure Veri Fabrikası, verileri ağ üzerinden (internet, ER veya VPN) aktarmak için standart bir çevrimiçi veri geçiş aracıdır. Çevrimdışı veri geçişinde ise, kullanıcılar fiziksel olarak veri aktarım aygıtlarını kuruluşlarından bir Azure Veri Merkezi'ne naklederler.  

Çevrimiçi ve çevrimdışı geçiş yaklaşımı arasında seçim yaptığınızda üç önemli nokta vardır:  

- Geçirilecek verilerin boyutu
- Ağ bant genişliği
- Geçiş penceresi

Örneğin, veri geçişinizi iki hafta içinde tamamlamak için Azure Veri Fabrikası'nı kullanmayı planladığınızı varsayalım *(geçiş pencereniz).* Aşağıdaki tablodaki pembe/mavi kesme çizgisine dikkat edin. Belirli bir sütun için en düşük pembe hücre, geçiş penceresi en yakın ancak iki haftadan kısa olan veri boyutunu/ağ bant genişliği eşleştirmesini gösterir. (Mavi hücredeki herhangi bir boyut/bant genişliği eşleştirmesinin iki haftadan uzun bir çevrimiçi geçiş penceresi vardır.) 

![çevrimiçi ve çevrimdışı](media/data-migration-guidance-overview/online-offline.png) Bu tablo, verilerinizin boyutuna ve kullanılabilir ağ bant genişliğinize bağlı olarak çevrimiçi geçiş (Azure Veri Fabrikası) aracılığıyla istediğiniz geçiş pencerenizi karşılayıp karşılayıp karşılamadığınızı belirlemenize yardımcı olur. Çevrimiçi geçiş penceresi iki haftadan uzunsa, çevrimdışı geçiş kullanmak isteyebilirsiniz.

> [!NOTE]
> Çevrimiçi geçiş kullanarak, hem geçmiş veri yüklemesini hem de artımlı akışlarını tek bir araç aracılığıyla uçtan uca elde edebilirsiniz.  Bu yaklaşım sayesinde, verileriniz tüm geçiş penceresi boyunca varolan depo ile yeni depo arasında eşitlenmiş olarak tutulabilir. Bu, yeni depodaki ETL mantığınızı yenilenmiş verilerle yeniden oluşturabileceğiniz anlamına gelir.


## <a name="next-steps"></a>Sonraki adımlar

- [AWS S3'ten Azure'a verileri geçirme](data-migration-guidance-s3-azure-storage.md)
- [Şirket içi hadoop kümesinden Azure'a veri geçirme](data-migration-guidance-hdfs-azure-storage.md)
- [Şirket içi Netezza sunucusundan Azure’a veri geçirme](data-migration-guidance-netezza-azure-sqldw.md)
