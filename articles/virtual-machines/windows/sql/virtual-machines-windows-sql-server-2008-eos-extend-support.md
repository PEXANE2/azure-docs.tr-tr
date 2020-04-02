---
title: SQL Server 2008 & 2008 R2 desteğini genişletin
description: SQL Server örneğini Azure'a geçirerek veya örnekleri şirket içinde tutmak için genişletilmiş destek satın alarak SQL Server 2008 ve SQL Server 2008 R2 desteğini genişletin.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/08/2019
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: faaff9e9ad5982efac6409f2284158a3d0711331
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548401"
---
# <a name="extend-support-for-sql-server-2008-and-sql-server-2008-r2-with-azure"></a>Azure ile SQL Server 2008 ve SQL Server 2008 R2 için destek genişletme

SQL Server 2008 ve SQL Server 2008 R2 her ikisi de [destek (EOS) yaşam döngüsünün sonuna](https://www.microsoft.com/sql-server/sql-server-2008)ulaşmıştır. Birçok müşteri hala her iki sürümü de kullandığından, destek almaya devam etmek için çeşitli seçenekler salıyoruz. Şirket içi SQL Server örneklerinizi Azure sanal makinelerine (VM) geçirebilir, Azure SQL Veritabanı'na geçiş yapabilir veya şirket içinde kalıp genişletilmiş güvenlik güncelleştirmeleri satın alabilirsiniz.

Yönetilen bir örneğin aksine, bir Azure VM'ye geçiş yapmak, uygulamalarınızı yeniden onaylamayı gerektirmez. Şirket içinde kalmanın aksine, bir Azure VM'ye geçiş yaparak ücretsiz genişletilmiş güvenlik yamaları alırsınız.

Bu makalenin geri kalanı, SQL Server örneğini Azure VM'ye geçirmek için göz önünde bulundurulması gereken hususları sağlar.

Destek sonu seçenekleri hakkında daha fazla bilgi için [bkz.](/sql/sql-server/end-of-support/sql-server-end-of-life-overview)

## <a name="provisioning"></a>Sağlama

Azure Marketplace'te Windows Server **2008 R2** görüntüde olabildiğince çok öde SQL Server 2008 R2 vardır.

SQL Server 2008'de olan müşterilerin sql server 2008 R2'ye kendi kendine yüklemeleri veya yükseltmeleri gerekir. Aynı şekilde, Windows Server 2008'deki müşterilerin vm'lerini özel bir VHD'den dağıtmaları veya Windows Server 2008 R2'ye yükseltmeleri gerekir.

Azure Marketi üzerinden dağıtılan görüntüler, önceden yüklenmiş SQL IaaS uzantısı ile birlikte gelir. SQL IaaS uzantısı esnek lisanslama ve otomatik düzeltme için bir gerekliliktir. Kendi kendine yüklenen VM'leri dağıtan müşterilerin SQL IaaS uzantısını el ile yüklemeleri gerekir. SQL IaaS uzantısı Windows Server 2008'de desteklenmez.

> [!NOTE]
> SQL Server **Create** and **Manage** bıçakları Azure portalındaki SQL Server 2008 R2 görüntüsüyle birlikte çalışsa da, aşağıdaki özellikler _desteklenmez:_ Otomatik yedeklemeler, Azure Key Vault tümleştirmesi, R Hizmetleri ve depolama yapılandırması.

## <a name="licensing"></a>Lisanslama
Gittiğiniz kadar öde SQL Server 2008 R2 dağıtımları [Azure Karma Avantajı'na](https://azure.microsoft.com/pricing/hybrid-benefit/)dönüştürülebilir.

Yazılım Güvencesi (SA) tabanlı bir lisansı istediğiniz kadar ödeye dönüştürmek için müşterilerin SQL VM [kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md)kaydolması gerekir. Bu kayıttan sonra, SQL lisans türü Azure Karma Avantajı ile istediğiniz kadar öde arasında değiştirilebilir.

Azure VM'de kendi kendine yüklenen SQL Server 2008 veya SQL Server 2008 R2 örnekleri, SQL VM kaynak sağlayıcısına kaydolabilir ve lisans tiplerini istediğiniz kadar öde'ye dönüştürebilir.

## <a name="migration"></a>Geçiş
EOS SQL Server örneklerini el ile yedekleme/geri yükleme yöntemleriyle Azure VM'ye geçirebilirsiniz. Bu, şirket içi ve Azure VM'ye en yaygın geçiş yöntemidir.

### <a name="azure-site-recovery"></a>Azure Site Recovery

Toplu geçişler için [Azure Site Kurtarma](/azure/site-recovery/site-recovery-overview) hizmetini öneririz. Azure Site Kurtarma ile müşteriler, SQL Server dahil olmak üzere tüm VM'yi şirket içi nden Azure VM'ye çoğaltabilir.

SQL Server, kurtarmayı garanti etmek için uygulama tutarlı Azure Site Kurtarma anlık görüntüleri gerektirir. Azure Site Kurtarma, en az 1 saatlik aralıklarla uygulama tutarlı anlık görüntüleri destekler. Azure Site Kurtarma geçişleri ile SQL Server için mümkün olan minimum kurtarma noktası hedefi (RPO) 1 saattir. Kurtarma süresi hedefi (RTO) 2 saat artı SQL Server kurtarma süresidir.

### <a name="database-migration-service"></a>Veritabanı Geçiş Hizmeti

[Veritabanı Geçiş Hizmeti,](/azure/dms/dms-overview) SQL Server'ı 2012 sürümüne veya daha sonrasına yükselterek şirket içi olarak Azure VM'ye geçiş yapıyorlarsa müşteriler için bir seçenektir.

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Azure VM'deki EOS SQL Server için olağanüstü durum kurtarma çözümleri aşağıdaki gibidir:

- **SQL Server yedeklemeleri**: EOS SQL Server 2008 ve 2008 R2'nizi fidye yazılımlarına, kazara silmeişlemine ve 15 dakika rpo ve zaman içinde kurtarma yla bozulmaya karşı korumaya yardımcı olmak için Azure Yedekleme'yi kullanın. Daha fazla bilgi için [bu makaleye](https://docs.microsoft.com/azure/backup/sql-support-matrix#scenario-support)bakın.
- **Günlük gönderim**: RTO'yu azaltmak için sürekli geri yüklemelerle başka bir bölgede veya Azure bölgesinde günlük gönderi çoğaltma oluşturabilirsiniz. Günlük gönderisini el ile yapılandırmanız gerekir.
- **Azure Site Kurtarma**: Azure Site Kurtarma çoğaltma yoluyla VM'nizi bölgeler ve bölgeler arasında çoğaltabilirsiniz. SQL Server, bir felaket durumunda kurtarmayı garanti etmek için uygulama tutarlı anlık görüntüler gerektirir. Azure Site Kurtarma, EOS SQL Server olağanüstü kurtarma kurtarma için en az 1 saatlik RPO ve 2 saatlik (artı SQL Server kurtarma süresi) RTO sunar.

## <a name="security-patching"></a>Güvenlik düzeltme eki
SQL Server VM'ler için genişletilmiş güvenlik güncelleştirmeleri, SQL Server VM'nin SQL VM [kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md)kaydedilmesinden sonra Microsoft Update kanalları aracılığıyla teslim edilir. Yamalar el ile veya otomatik olarak indirilebilir.

*Otomatik düzeltme eki uygulama* varsayılan olarak etkindir. Otomatik düzeltme eki uygulama Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını sağlar. SQL Server IaaS uzantısı yüklüyse, bakım penceresi için haftanın gününü, süresini ve süresini belirtebilirsiniz. Azure düzeltme eki uygulamayı bu bakım penceresinde gerçekleştirir. Bakım penceresi zamanlaması saat için VM yerel saatini kullanır.  Daha fazla bilgi [için Azure Sanal Makinelerde SQL Server için Otomatik düzeltme ekine](virtual-machines-windows-sql-automated-patching.md)bakın.


## <a name="next-steps"></a>Sonraki adımlar

SQL Server VM'nizi Azure'a geçirin:

* [Bir SQL Server veritabanını Azure VM’deki SQL Server’a geçirme](virtual-machines-windows-migrate-sql.md)

Azure Sanal Makinelerde SQL Server ile başlayın:

* [Azure portalında SQL Server VM’i oluşturma](quickstart-sql-vm-create-portal.md)

SQL Server VM'ler hakkında sık sorulan soruların yanıtlarını alın:

* [Azure Sanal Makinelerde SQL Server için SSS](virtual-machines-windows-sql-server-iaas-faq.md)

Destek sonu seçenekleri ve genişletilmiş güvenlik güncelleştirmeleri hakkında daha fazla bilgi edinin:

* [Desteğin Sonu](/sql/sql-server/end-of-support/sql-server-end-of-life-overview) & [Genişletilmiş Güvenlik Güncelleştirmeleri](/sql/sql-server/end-of-support/sql-server-extended-security-updates)