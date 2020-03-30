---
title: SQL Server sürümünün yerinde değişimi
description: Azure'daki SQL Server sanal makinenizin sürümünü nasıl değiştireceğinizi öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/14/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: bd9622d3e34198402bc45e5d855d05b0c8e35eb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605450"
---
# <a name="in-place-change-of-sql-server-edition-on-azure-vm"></a>Azure VM'de SQL Server sürümünün yerinde değişimi

Bu makalede, Azure'daki bir Windows sanal makinesinde SQL Server sürümünün nasıl değiştirilen açıklanmaktadır. 

SQL Server sürümü ürün anahtarı tarafından belirlenir ve yükleme ortamı kullanılarak yükleme işlemi sırasında belirtilir. Sürüm, SQL Server ürününde hangi [özelliklerin](/sql/sql-server/editions-and-components-of-sql-server-2017) mevcut olduğunu belirler. Sql Server sürümünü yükleme ortamıyla değiştirebilir ve daha fazla özellik etkinleştirmek için maliyeti azaltmak veya yükseltme yapmak için düşürmeyi azaltabilirsiniz.

SQL Server sürümü dahili olarak SQL Server VM olarak değiştirildikten sonra, faturalandırma amacıyla Azure portalında SQL Server'ın sürüm özelliğini güncelleştirmeniz gerekir. 

## <a name="prerequisites"></a>Ön koşullar

SQL Server sürümünü yerinde değiştirmek için aşağıdakilere ihtiyacınız var: 

- [Azure aboneliği.](https://azure.microsoft.com/free/)
- [Windows'da](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) SQL [VM kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md)kayıtlı bir SQL Server VM.
- SQL Server **istenilen sürümü** ile kurulum ortamı. [Yazılım Güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) olan müşteriler, yükleme ortamlarını [Toplu LisansLama Merkezi'nden](https://www.microsoft.com/Licensing/servicecenter/default.aspx)edinebilirler. Yazılım Güvencesi olmayan müşteriler, istedikleri sürümü içeren (genellikle burada `C:\SQLServerFull`bulunan) bir Azure Marketplace SQL Server VM görüntüsündeki kurulum ortamını kullanabilir. 


## <a name="upgrade-an-edition"></a>Sürümü yükseltme

> [!WARNING]
> SQL Server sürümünün yükseltilmesi, Çözümhizmetleri ve R Hizmetleri gibi ilişkili hizmetlerle birlikte SQL Server için hizmeti yeniden başlatacaktır. 

SQL Server sürümünü yükseltmek için, SQL Server'ın istenilen sürümü için SQL Server kurulum ortamını edinin ve ardından aşağıdakileri yapın:

1. SQL Server yükleme medyasından Setup.exe'yi açın. 
1. **Bakım'a** gidin ve **Edition Yükseltme** seçeneğini seçin. 

   ![SQL Server sürümünü yükseltmek için seçim](media/virtual-machines-windows-sql-change-edition/edition-upgrade.png)

1. **Yükseltmeye Hazır sürümü** sayfasına ulaşana kadar **İleri'yi** seçin ve ardından **Yükseltme'yi**seçin. Değişiklik etkinolurken kurulum penceresi birkaç dakika yanıt vermeyi durdurabilir. **Tam** sayfa, sürüm yükseltmenizin tamamladığını doğrular. 

SQL Server sürümü yükseltildikten sonra, Azure portalındaki SQL Server sanal makinesinin sürüm özelliğini değiştirin. Bu, bu VM ile ilişkili meta verileri ve faturalandırmayı güncelleştirecektir.

## <a name="downgrade-an-edition"></a>Bir sürümü düşürme

SQL Server sürümünü düşürmek için, SQL Server'ı tamamen kaldırmanız ve istediğiniz sürüm kurulum ortamıyla yeniden yüklemeniz gerekir. 

> [!WARNING]
> SQL Server'ın yüklenmesi ek kapalı kalma süresine neden olabilir. 

Aşağıdaki adımları izleyerek SQL Server sürümünü düşürebilirsiniz:

1. Sistem veritabanları da dahil olmak üzere tüm veritabanlarını yedekleyin. 
1. Sistem veritabanlarını (ana, model ve msdb) yeni bir konuma taşıyın. 
1. SQL Server'ı ve ilişkili tüm hizmetleri tamamen kaldırın. 
1. Sanal makineyi yeniden başlatın. 
1. SQL Server istenilen sürümü ile medya kullanarak SQL Server yükleyin.
1. En son hizmet paketlerini ve kümülatif güncelleştirmeleri yükleyin.  
1. Yükleme sırasında oluşturulan yeni sistem veritabanlarını, daha önce farklı bir konuma taşıdığınız sistem veritabanlarıyla değiştirin. 

SQL Server sürümü düşürüldükten sonra, Azure portalındaki SQL Server sanal makinesinin sürüm özelliğini değiştirin. Bu, bu VM ile ilişkili meta verileri ve faturalandırmayı güncelleştirecektir.

## <a name="change-edition-in-portal"></a>Portalda sürümü değiştir 

Yükleme ortamını kullanarak SQL Server sürümünü değiştirdikten ve SQL Server [VM'inizi SQL VM kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md)kaydettikten sonra, faturalandırma amacıyla SQL Server VM'nin Edition özelliğini değiştirmek için Azure portalını kullanabilirsiniz. Bunu yapmak için şu adımları uygulayın: 

1. [Azure portalında](https://portal.azure.com)oturum açın. 
1. SQL Server sanal makine kaynağınıza gidin. 
1. **Ayarlar**altında, **Yapıla'yı**seçin. Ardından, **Edition**altındaki açılır listeden istediğiniz SQL Server sürümünü seçin. 

   ![Sürüm meta verilerini değiştirme](media/virtual-machines-windows-sql-change-edition/edition-change-in-portal.png)

1. Önce SQL Server sürümünü değiştirmeniz ve sürüm özelliğinin SQL Server sürümüyle eşleşmesi gerektiğini söyleyen uyarıyı gözden geçirin. 
1. Baskı meta veri değişikliklerinizi uygulamak için **Uygula'yı** seçin. 


## <a name="remarks"></a>Açıklamalar

- SQL Server VM'nin sürüm özelliği, hem gider olarak öde hem de kendi lisans türlerini getir dahil olmak üzere tüm SQL Server sanal makineleri için yüklenen SQL Server örneğinin sürümüyle eşleşmelidir.
- SQL Server VM kaynağınızı düşürürseniz, görüntünün sabit kodlanmış sürüm ayarına geri dönersiniz.
- Sürümü değiştirme özelliği, SQL VM kaynak sağlayıcısının bir özelliğidir. Azure portalı üzerinden Azure Marketi görüntüsünü dağıtmak, kaynak sağlayıcısına otomatik olarak bir SQL Server VM kaydeder. Ancak, sql server'ı kendi kendine yükleyen müşterilerin [SQL Server VM'lerini](virtual-machines-windows-sql-register-with-resource-provider.md)el ile kaydetmeleri gerekir.
- Kullanılabilirlik kümesine SQL Server VM eklemek, VM'nin yeniden oluşturulmasını gerektirir. Kullanılabilirlik kümesine eklenen tüm VM'ler varsayılan baskıya geri döner ve sürümün yeniden değiştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM'de SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM'de SQL Server için SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM'de SQL Server için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM'de SQL Server için sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)


