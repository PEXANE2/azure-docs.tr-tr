---
title: Linux Azure Sanal Makinelerde SQL Server SSS | Microsoft Dokümanlar
description: Bu makalede, Linux Azure VM'lerde SQL Server'ı çalıştırma hakkında sık sorulan soruların yanıtları verilmektedir.
services: virtual-machines-linux
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 12/13/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 1e729c608a2cad28c810f8d5236360c909a496b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70082025"
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Linux Azure Sanal Makinelerde SQL Server için sık sorulan sorular

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

Bu makalede, [Linux Azure Sanal Makinelerde SQL Server'ı](sql-server-linux-virtual-machines-overview.md)çalıştırma yla ilgili en sık sorulan sorulardan bazılarına yanıt verilmektedir.

> [!NOTE]
> Bu makalede, Linux VM'lerde SQL Server'a özgü konular ele alduruluyor. Windows VM'lerde SQL Server çalıştırıyorsanız, [Windows SSS](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)bölümüne bakın.

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="images"></a><a id="images"></a>Görüntü

1. **Hangi SQL Server sanal makine galerisi görüntüleri mevcuttur?**

   Azure, hem Linux hem de Windows için tüm sürümlerde desteklenen tüm büyük SQL Server sürümleri için sanal makine görüntüleri tutar. Daha fazla bilgi için [Linux VM görüntülerinin](sql-server-linux-virtual-machines-overview.md#create) ve [Windows VM görüntülerinin](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md#payasyougo)tam listesine bakın.

1. **Mevcut SQL Server sanal makine galerisi görüntüleri güncelleştirildi mi?**

   Sanal makine galerisindeki SQL Server görüntüleri her iki ayda bir en son Linux ve Windows güncellemeleriyle güncellenir. Linux görüntüleri için bu, en son sistem güncelleştirmelerini içerir. Windows görüntüleri için bu, önemli SQL Server güvenlik güncelleştirmeleri ve hizmet paketleri de dahil olmak üzere Windows Update'te önemli olarak işaretlenmiş güncelleştirmeleri içerir. SQL Server kümülatif güncelleştirmeleri Linux ve Windows için farklı şekilde işlenir. Linux için, SQL Server kümülatif güncelleştirmeleri de yenilemeye dahildir. Ancak şu anda, Windows VM'ler SQL Server veya Windows Server kümülatif güncelleştirmeleri ile güncelleştirilmeyecektir.

1. **İlgili SQL Server paketleri de yüklenir?**

   Varsayılan olarak SQL Server Linux VM'lerinde yüklenen SQL Server paketlerini görmek için [Yüklü paketlere](sql-server-linux-virtual-machines-overview.md#packages)bakın.

1. **SQL Server sanal makine görüntüleri galeriden kaldırılabilir mi?**

   Evet. Azure, ana sürüm ve sürüm başına yalnızca bir görüntü tutar. Örneğin, yeni bir SQL Server hizmet paketi yayımlandığında, Azure bu hizmet paketi için galeriye yeni bir resim ekler. Önceki hizmet paketinin SQL Server görüntüsü Azure portalından hemen kaldırılır. Ancak, önümüzdeki üç ay boyunca PowerShell'den tedarik için hala kullanılabilir. Üç ay sonra, önceki hizmet paketi görüntüsü artık kullanılamaz. Bu kaldırma ilkesi, bir SQL Server sürümü yaşam döngüsünün sonuna ulaştığında desteklenmediğinde de geçerli olur.

## <a name="creation"></a>Oluşturma

1. **SQL Server ile Nasıl Linux Azure sanal makine oluştururum?**

   En kolay çözüm, SQL Server içeren bir Linux Sanal Makine oluşturmaktır. Azure'a kaydolma ve portaldan bir SQL VM oluşturma hakkında bir öğretici için Azure [portalında Linux SQL Server sanal makine sağlama](provision-sql-server-linux-virtual-machine.md)konusuna bakın. Ayrıca, sql server'ı serbest lisanslı bir sürümle (Geliştirici veya Ekspres) veya şirket içi lisansı yeniden kullanarak vm'ye el ile yükleme seçeneğiniz de vardır. Kendi lisansınızı getiriyorsanız, [Azure'da Yazılım Güvencesi ile Lisans Mobilite'ine](https://azure.microsoft.com/pricing/license-mobility)sahip olmalısınız.

1. **Harcama limiti olan bir Azure aboneliğiyle neden bir RHEL veya SLES SQL Server VM'si sağlamıyorum?**

   RHEL ve SLES sanal makineleri, harcama limiti olmayan bir abonelik ve abonelikle ilişkili doğrulanmış bir ödeme yöntemi (genellikle kredi kartı) gerektirir. Harcama sınırını kaldırmadan bir RHEL veya SLES VM sağlamanız durumunda aboneliğiniz devre dışı bırakılır ve tüm VM'ler/hizmetler durdurulur. Bu duruma rastlarsanız, aboneliği yeniden etkinleştirmek için [harcama sınırını kaldırın.](https://account.windowsazure.com/subscriptions) Kalan kredileriniz geçerli faturalandırma döngüsü için geri yüklenir, ancak yeniden başlatmayı ve çalıştırmaya devam etmeyi seçerseniz RHEL veya SLES VM görüntü ek ücreti kredi kartınıza ters gider.

## <a name="licensing"></a>Lisanslama

1. **Lisanslı SQL Server kopyamı bir Azure sanal makinesine nasıl yükleyebilirim?**

   İlk olarak, bir Linux işletim sistemi sanal makine oluşturun. Ardından Linux dağıtımınız için [SQL Server yükleme adımlarını](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) çalıştırın. SQL Server'ın serbestçe lisanslı sürümlerinden birini yüklemdiğiniz sürece, Azure'da Yazılım Güvencesi aracılığıyla bir SQL Server lisansınız ve [Lisans Hareketliliğiniz](https://azure.microsoft.com/pricing/license-mobility/)de olmalıdır.

1. **SQL Server için Bring-Your-Own-License (BYOL) Linux sanal makine görüntüleri var mı?**

   Şu anda, SQL Server için BYOL Linux sanal makine görüntüleri yoktur. Ancak, önceki sorularda anlatıldığı gibi SQL Server'ı yalnızca Linux'a özel bir VM'ye el ile yükleyebilirsiniz.

1. **Kullandıkça ödeme galeri görüntülerinden biri kullanılarak oluşturulan sanal makineyi kendi SQL Server lisansımı kullanmak için değiştirebilir miyim?**

   Hayır. Saniye başına ödemeli lisanslamadan kendi lisansınızı kullanmaya geçiş yapamazsınız. Yeni bir Linux VM oluşturmalı, SQL Server'ı yüklemeli ve verilerinizi aktarmalısınız. Kendi lisansınızı getirme hakkında daha fazla bilgi için önceki soruya bakın.

## <a name="administration"></a>Yönetim

1. **SQL Server Management Studio (SSMS) ile linux SQL Server sanal makinesini yönetebilir miyim?**

   Evet, ancak SSMS şu anda yalnızca Windows'a özel bir araçtır. Linux SQL Server VMs ile SSMS kullanmak için bir Windows makinesinden uzaktan bağlanmanız gerekir. Linux'ta yerel olarak, yeni [mssql-conf](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf) aracı birçok yönetim görevi gerçekleştirebilir. Platformlar arası veritabanı yönetim aracı için [Azure Veri Stüdyosu'na](https://docs.microsoft.com/sql/azure-data-studio/what-is)bakın.

1. **SQL Server'ı SQL VM'sinden tamamen kaldırabilir miyim?**

   Evet, ancak [SQL Server Azure VM'lerin Fiyatlandırma kılavuzunda](../../windows/sql/virtual-machines-windows-sql-server-pricing-guidance.md?toc=%2fazure%2fvirtual-machines%2flinux%2fsql%2ftoc.json)açıklandığı gibi SQL VM'niz için ücretlendirilmeye devam edersiniz. Artık SQL Server'a ihtiyacınız kalmadıysa yeni bir sanal makine dağıtabilir ve verilerle uygulamaları yeni sanal makineye geçirebilirsiniz. Ardından SQL Server sanal makinesini kaldırabilirsiniz.

## <a name="updating-and-patching"></a>Güncelleme ve Yama

1. **Azure VM'de SQL Server'ın yeni sürümüne/sürümüne nasıl yükseltebilirim?**

   Şu anda bir Azure sanal makinesinde çalışan SQL Server için yerinde yükseltme mevcut değildir. İstediğiniz SQL Server sürümü ile yeni bir Azure sanal makinesi oluşturun ve ardından [standart veri geçirme tekniklerini](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview) kullanarak veritabanlarınızı yeni sunucuya geçirin.

## <a name="general"></a>Genel

1. **SQL Server Yüksek Kullanılabilirlik çözümleri Azure VM'lerde desteklenir mi?**

   Şu anda değil. Her zaman Kullanılabilirlik Grupları ve Failover Kümeleme hem Pacemaker gibi Linux'ta bir kümeleme çözümü gerektirir. SQL Server için desteklenen Linux dağıtımları, Bulut'taki yüksek kullanılabilirlik eklentilerini desteklemez.

## <a name="resources"></a>Kaynaklar

**Linux VMs**:

* [Linux VM'de SQL Server'a Genel Bakış](sql-server-linux-virtual-machines-overview.md)
* [SQL Server Linux VM sağlama](provision-sql-server-linux-virtual-machine.md)
* [Linux üzerinde SQL Server belgeleri](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**Windows VMs**:

* [Windows VM'de SQL Server'a Genel Bakış](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server Windows VM sağlama](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [SSS (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)