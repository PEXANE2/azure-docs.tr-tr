---
title: Şirket içi Her Zaman Kullanılabilirlik Gruplarını Azure'a genişletin | Microsoft Dokümanlar
description: Bu öğretici, klasik dağıtım modeliyle oluşturulan kaynakları kullanır ve Azure'da Her Zaman Kullanılabilirlik Grubu yinelemesi eklemek için SQL Server Management Studio'da (SSMS) Çoğaltma Ekle sihirbazını nasıl kullanacağımı açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 7ca7c423-8342-4175-a70b-d5101dfb7f23
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 4521c2c112c93e83144cfc84d600208817b2ccac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978041"
---
# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Şirket içi AlwaysOn Kullanılabilirlik Grupları’nı Azure’a Genişletme
Her Zaman Kullanılabilirlik Grupları ikincil yinelemeler ekleyerek veritabanı grupları için yüksek kullanılabilirlik sağlar. Bu yinelemeler, bir hata durumunda veritabanları üzerinde başarısız lığa izin verir. Ayrıca, okuma iş yüklerini veya yedekleme görevlerini boşaltmak için de kullanılabilirler.

Bir veya daha fazla Azure VM'sini SQL Server'a vererek ve bunları şirket içi Kullanılabilirlik Gruplarınıza yineleme olarak ekleyerek şirket içi Kullanılabilirlik Gruplarını Microsoft Azure'a genişletebilirsiniz.

Bu öğretici, aşağıdakilere sahip olduğunuzu varsayar:

* Etkin bir Azure aboneliği. Ücretsiz [deneme sürümü için kaydolabilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)
* Varolan Her Zaman Kullanılabilirlik Grubu şirket içinde. Kullanılabilirlik Grupları hakkında daha fazla bilgi için her [zaman kullanılabilirlik gruplarına](https://msdn.microsoft.com/library/hh510230.aspx)bakın.
* Şirket içi ağ ile Azure sanal ağınız arasındaki bağlantı. Bu sanal ağı oluşturma hakkında daha fazla bilgi için Azure [portalını (klasik) kullanarak Siteden Siteye bağlantı oluşturma](../../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md)'ya bakın.

> [!IMPORTANT] 
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

## <a name="add-azure-replica-wizard"></a>Azure Çoğaltma Sihirbazı Ekle
Bu bölümde, Her Zaman Kullanılabilirlik Grubu çözümünüzü Azure yinelemelerini içerecek şekilde genişletmek için **Azure Çoğaltma Ekle Sihirbazı'nı** nasıl kullanacağınızı gösterir.

> [!IMPORTANT]
> **Azure Çoğaltma Ekle Sihirbazı** yalnızca Klasik dağıtım modeliyle oluşturulan sanal makineleri destekler. Yeni VM dağıtımları yeni Kaynak Yöneticisi modelini kullanmalıdır. Kaynak Yöneticisi ile VM kullanıyorsanız, Transact-SQL komutlarını kullanarak ikincil Azure yinelemesini el ile eklemeniz gerekir (burada gösterilmez). Bu sihirbaz Kaynak Yöneticisi senaryosunda çalışmaz.

1. SQL Server Management Studio içinden, Her Zaman Yüksek**Kullanılabilirlik Kullanılabilirlik Grupları** > [Kullanılabilirlik **Always On High Availability** > **Grubunuzun Adı] genişletin.**
2. Sağ tıklatın **Kullanılabilirlik Yinelemeler,** sonra **Çoğaltma Ekle'yi**tıklatın.
3. Varsayılan olarak, **Kullanılabilirlik Grubu Sihirbazı'na Yineleme Ekle** görüntülenir. **İleri**'ye tıklayın.  Bu sihirbazın önceki lansmanı sırasında sayfanın alt kısmında **bu sayfayı tekrar gösterme** seçeneğini seçtiyseniz, bu ekran görüntülenmez.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. Varolan tüm ikincil yinelemelere bağlanmanız gerekir. Bağlan'a **tıklayabilirsiniz...** her çoğaltma yanında veya **Tümünü Bağla'yı tıklatabilirsiniz...** ekranın alt kısmında. Kimlik doğrulamadan sonra, bir sonraki ekrana ilerlemek için **İleri'yi** tıklatın.
5. **Yinelemeleri Belirt** sayfasında, üstte birden çok sekme listelenir: **Yinelemeler,** **Uç Noktalar,** **Yedekleme Tercihleri**ve **Dinleyici.** **Yinelemeler** sekmesinden **Azure Yineleme Ekle'yi tıklatın...** Azure Çoğaltma Sihirbazı Ekle'yi başlatmak için.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Daha önce yüklediyseniz, yerel Windows sertifika mağazasından varolan bir Azure Yönetim Sertifikası'nı seçin. Daha önce kullandıysanız Azure aboneliğinin kimliğini seçin veya girin. Azure Yönetim Sertifikası'nı indirmek ve yüklemek için İndir'i tıklatabilir ve bir Azure hesabı kullanarak abonelik listesini indirebilirsiniz.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. Sayfadaki her alanı, yinelemeyi barındıracak Azure Sanal Makinesini (VM) oluşturmak için kullanılacak değerlerle doldurursunuz.
   
   | Ayar | Açıklama |
   | --- | --- |
   | **Görüntü** |OS ve SQL Server'ın istenilen birleşimini seçin |
   | **VM Boyutu** |İş gereksinimlerinize en uygun VM boyutunu seçin |
   | **VM Adı** |Yeni VM için benzersiz bir ad belirtin. Ad 3 ile 15 karakter arasında olmalıdır, yalnızca harfler, sayılar ve tireler içermeli ve bir harfle başlayıp bir harf le veya sayıyla bitmelidir. |
   | **VM Kullanıcı Adı** |VM'de yönetici hesabı olacak bir kullanıcı adı belirtin |
   | **VM Yönetici Şifresi** |Yeni hesap için parola belirtin |
   | **Parolayı Onayla** |Yeni hesabın parolasını onaylama |
   | **Sanal Ağ** |Yeni VM'nin kullanması gereken Azure sanal ağını belirtin. Sanal ağlar hakkında daha fazla bilgi için [Sanal Ağa Genel Bakış'a](../../../virtual-network/virtual-networks-overview.md)bakın. |
   | **Sanal Ağ Subnet** |Yeni VM'nin kullanması gereken sanal ağ alt netini belirtin |
   | **Domain** |Etki alanı için önceden doldurulmuş değerin doğru olduğunu doğrulayın |
   | **Etki Alanı Kullanıcı Adı** |Yerel küme düğümlerinde yerel yöneticiler grubunda bulunan bir hesap belirtin |
   | **Parola** |Etki alanı kullanıcı adının parolasını belirtin |
8. Dağıtım ayarlarını doğrulamak için **Tamam'ı** tıklatın.
9. Sonraki yasal terimler görüntülenir. Bu koşulları kabul ederseniz **Tamam'ı** okuyun ve tıklatın.
10. **Yinelemeleri Belirt** sayfası yeniden görüntülenir. **Yinelemeler,** **Uç Noktalar**ve **Yedekleme Tercihleri** sekmelerinde yeni Azure yinelemesinin ayarlarını doğrulayın. İş gereksinimlerinizi karşılayacak şekilde ayarları değiştirin.  Bu sekmelerde yer alan parametreler hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/hh213088.aspx) Dinleyicilerin Azure yinelemeleri içeren Kullanılabilirlik Grupları için Dinleyici sekmesini kullanarak oluşturulamayacağını unutmayın. Ayrıca, Sihirbazı başlatmadan önce bir dinleyici zaten oluşturulduysa, Azure'da desteklenmediğini belirten bir ileti alırsınız. **Kullanılabilirlik Grubu Dinleyicisi Oluştur** bölümünde dinleyicilerin nasıl oluşturulacağını inceleyeceğiz.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. **İleri**'ye tıklayın.
12. **İlk Veri Eşitlemesi** seç sayfasında kullanmak istediğiniz veri eşitleme yöntemini seçin ve **İleri'yi**tıklatın. Çoğu senaryoiçin **Tam Veri Eşitleme'yi**seçin. Veri eşitleme yöntemleri hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/hh231021.aspx)
13. Sonuçları **Doğrulama** sayfasında inceleyin. Bekleyen sorunları düzeltin ve gerekirse doğrulamayı yeniden çalıştırın. **İleri**'ye tıklayın.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. **Özet** sayfasındaki ayarları gözden geçirin ve **ardından Bitir'i**tıklatın.
15. Sağlama süreci başlar. Sihirbaz başarıyla tamamlandığında, sihirbazdan çıkmak için **Kapat'ı** tıklatın.

> [!NOTE]
> Azure Çoğaltma Ekle Sihirbazı, Kullanıcılar\Kullanıcı Adı\AppData\Local\SQL Server\AddReplicaWizard'da bir günlük dosyası oluşturur. Bu günlük dosyası, başarısız Azure çoğaltma dağıtımlarını gidermek için kullanılabilir. Sihirbaz herhangi bir eylemi yürütmezse, sağlanan VM'yi silmesi de dahil olmak üzere önceki tüm işlemler geri alınır.
> 
> 

## <a name="create-an-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi oluşturma
Kullanılabilirlik grubu oluşturulduktan sonra, istemcilerin yinelemelere bağlanması için bir dinleyici oluşturmanız gerekir. Dinleyiciler gelen bağlantıları birincil veya salt okunur ikincil yinelemeye yönlendirir. Dinleyiciler hakkında daha fazla bilgi için, [Azure'daki Her Zaman Kullanılabilirlik Grupları için Bir ILB dinleyicisini Yapılandır' a](../classic/ps-sql-int-listener.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Her Zaman Kullanılabilirlik Grubunuzu Azure'a genişletmek için **Azure Çoğaltma Ekle Sihirbazı'nı** kullanmanın yanı sıra, bazı SQL Server iş yüklerini tamamen Azure'a taşıyabilirsiniz. Başlamak için [Azure'da SQL Server Virtual Machine Sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md)konusuna bakın.

Azure VM'lerde SQL Server'ı çalıştırmakla ilgili diğer konular için [Azure Sanal Makineler'deki SQL Server'a](../sql/virtual-machines-windows-sql-server-iaas-overview.md)bakın.

