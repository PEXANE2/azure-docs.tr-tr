---
title: Azure Kaynak Yöneticisi VM'leri için yüksek kullanılabilirlik ayarlama | Microsoft Dokümanlar
description: Bu öğretici, Azure Kaynak Yöneticisi modunda Azure sanal makineleriyle her zaman kullanılabilirlik grubu oluşturmanızı gösterir.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 64e85527-d5c8-40d9-bbe2-13045d25fc68
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: d7c88e500886453fbfb53655748ccf7025ab7d3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75374272"
---
# <a name="configure-always-on-availability-groups-in-azure-virtual-machines-automatically-resource-manager"></a>Azure Sanal Makinelerdeki kullanılabilirlik gruplarında her zaman yapılandırma: Kaynak Yöneticisi

Bu öğretici, Azure Kaynak Yöneticisi sanal makineleri kullanan bir SQL Server kullanılabilirlik grubunu nasıl oluşturabileceğinizi gösterir. Öğretici, şablonu yapılandırmak için Azure bıçaklarını kullanır. Varsayılan ayarları gözden geçirebilir, gerekli ayarları yazabilir ve bu öğreticide yürürken portaldaki bıçakları güncelleyebilirsiniz.

Tam öğretici, Azure Sanal Makineler'de aşağıdaki öğeleri içeren bir SQL Server kullanılabilirlik grubu oluşturur:

* Ön uç ve arka uç alt ağı da dahil olmak üzere birden çok alt ağı olan sanal ağ
* Etkin Dizin etki alanı olan iki etki alanı denetleyicisi
* SQL Server'ı çalıştıran ve arka uç alt ağına dağıtılan ve Active Directory etki alanına katılan iki sanal makine
* Düğüm Çoğunluk çoğunluk modeline sahip üç düğümlü bir başarısız küme
* Kullanılabilirlik veritabanının iki eşzamanlı yinelemesi olan kullanılabilirlik grubu

Aşağıdaki resimde tam çözüm temsil eder.

![Azure'daki kullanılabilirlik grupları için laboratuvar mimarisini test edin](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Bu çözümdeki tüm kaynaklar tek bir kaynak grubuna aittir.

Bu öğreticiye başlamadan önce aşağıdakileri onaylayın:

* Zaten bir Azure hesabınız var. Eğer yoksa, [bir deneme hesabı için kaydolun.](https://azure.microsoft.com/pricing/free-trial/)
* Sanal makine galerisinden bir SQL Server sanal makine sağlamak için GUI'yi nasıl kullanacağınızı zaten biliyorsunuz. Daha fazla bilgi için [Azure'da sql server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md)konusuna bakın.
* Kullanılabilirlik grupları hakkında zaten sağlam bir anlayışa sahipsiniz. Daha fazla bilgi için her [zaman kullanılabilirlik gruplarına (SQL Server)](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server)bakın.

> [!NOTE]
> SharePoint ile kullanılabilirlik gruplarını kullanmak istiyorsanız, [SharePoint 2013 için SQL Server 2012 Always On kullanılabilirlik gruplarını Yapılandırma](/SharePoint/administration/configure-an-alwayson-availability-group)konusuna da bakın.
>
>

Bu eğitimde, Azure portalını şu şekilde kullanın:

* Portaldan Her Zaman Şablonu'nu seçin.
* Şablon ayarlarını gözden geçirin ve ortamınız için birkaç yapılandırma ayarını güncelleştirin.
* Azure'u tüm ortamı oluştururken izleyin.
* Bir etki alanı denetleyicisine ve ardından SQL Server çalıştıran bir sunucuya bağlanın.

[!INCLUDE [availability-group-template](../../../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]

## <a name="provision-the-cluster-from-the-gallery"></a>Kümeyi galeriden sağlama
Azure, tüm çözüm için bir galeri görüntüsü sağlar. Şablonu bulmak için:

1. Hesabınızı kullanarak Azure portalında oturum açın.
2. Azure portalında, **Yeni** bölmeyi açmak için **kaynak oluştur'u** tıklatın.
3. **Yeni** bölmede **AlwaysOn'ı**arayın.
   ![AlwaysOn şablonu bul](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
4. Arama sonuçlarında **SQL Server AlwaysOn Cluster'ı**bulun.
   ![AlwaysOn Şablonu](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
5. **Dağıtım modelini seçin'** de **Kaynak Yöneticisi'ni**seçin.

### <a name="basics"></a>Temel Bilgiler
**Temel Bilgileri** tıklatın ve aşağıdaki ayarları yapılandırır:

* **Yönetici kullanıcı adı,** etki alanı yöneticisi izinleri olan ve SQL Server'ın her iki örneğinde de SQL Server sysadmin sabit sunucu rolünün bir üyesi olan bir kullanıcı hesabıdır. Bu öğretici için **DomainAdmin'i**kullanın.
* **Parola,** etki alanı yöneticisi hesabının parolasI.'dır. Karmaşık bir parola kullanın. Parola’yı onaylayın.
* **Abonelik,** kullanılabilirlik grubu için dağıtılan tüm kaynakları çalıştırmak için Azure faturalandırdığı aboneliktir. Hesabınızda birden çok abonelik varsa, farklı bir abonelik belirtebilirsiniz.
* **Kaynak grubu,** bu şablon tarafından oluşturulan tüm Azure kaynaklarının ait olduğu grubun adıdır. Bu öğretici için **SQL-HA-RG'yi**kullanın. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../../azure-resource-manager/management/overview.md#resource-groups).
* **Konum,** öğreticinin kaynakları oluşturduğu Azure bölgesidir. Bir Azure bölgesi seçin.

Aşağıdaki ekran görüntüsü tamamlanmış bir **Basics** bıçaktır:

![Temel Bilgiler](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

**Tamam**'a tıklayın.

### <a name="domain-and-network-settings"></a>Etki alanı ve ağ ayarları
Bu Azure galeri şablonu bir etki alanı ve etki alanı denetleyicileri oluşturur. Ayrıca bir ağ ve iki alt ağ oluşturur. Şablon varolan bir etki alanında veya sanal ağda sunucu lar oluşturamaz. Sonraki adım etki alanı ve ağ ayarlarını yapılandırır.

Etki **Alanı ve ağ ayarları** nda, etki alanı ve ağ ayarları için önceden ayarlanmış değerleri gözden geçirin:

* **Orman kökü etki alanı adı,** kümeyi barındıran Etkin Dizin etki alanının etki alanı adıdır. Öğretici **için, contoso.com**kullanın.
* **Sanal Ağ adı,** Azure sanal ağının ağ adıdır. Öğretici için **autohaVNET**kullanın.
* **Etki Alanı Denetleyicisi alt ağ adı,** etki alanı denetleyicisini barındıran sanal ağın bir bölümünün adıdır. **Alt ağ-1'i**kullanın. Bu alt ağ adres öneki **10.0.0.0/24**kullanır.
* **SQL Server alt ağ adı,** SQL Server'ı çalıştıran sunucuları ve dosya paylaşımı tanığını barındıran sanal ağın bir bölümünün adıdır. **Alt ağ-2'yi**kullanın. Bu alt ağ adres öneki **10.0.1.0/26**kullanır.

Azure'daki sanal ağlar hakkında daha fazla bilgi edinmek için [Sanal ağa genel bakış](../../../virtual-network/virtual-networks-overview.md)'a bakın.  

**Etki Alanı ve ağ ayarları** aşağıdaki ekran görüntüsü gibi görünmelidir:

![Etki alanı ve ağ ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Gerekirse, bu değerleri değiştirebilirsiniz. Bu öğretici için önceden ayarlanmış değerleri kullanın.

Ayarları gözden geçirin ve ardından **Tamam'ı**tıklatın.

### <a name="availability-group-settings"></a>Kullanılabilirlik grubu ayarları
**Kullanılabilirlik grubu ayarlarında,** kullanılabilirlik grubu ve dinleyici için önceden ayarlanmış değerleri gözden geçirin.

* **Kullanılabilirlik grubu adı,** kullanılabilirlik grubu için kümelenmiş kaynak adıdır. Bu öğretici için **Contoso-ag'ı**kullanın.
* **Kullanılabilirlik grubu dinleyici adı** küme ve iç yük dengeleyici tarafından kullanılır. SQL Server'a bağlanan istemciler, veritabanının uygun yinelemesine bağlanmak için bu adı kullanabilir. Bu öğretici için **Contoso-dinleyicikullanın.**
* **Kullanılabilirlik grubu dinleyici sit noktası,** SQL Server dinleyicisinin TCP bağlantı noktasını belirtir. Bu öğretici için varsayılan bağlantı noktası **olan 1433'u**kullanın.

Gerekirse, bu değerleri değiştirebilirsiniz. Bu öğretici için önceden ayarlanmış değerleri kullanın.  

![kullanılabilirlik grubu ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

**Tamam**'a tıklayın.

### <a name="virtual-machine-size-storage-settings"></a>Sanal makine boyutu, depolama ayarları
**VM boyutunda, depolama ayarlarında,** bir SQL Server sanal makine boyutu seçin ve diğer ayarları gözden geçirin.

* **SQL Server sanal makine boyutu,** SQL Server'ı çalıştıran her iki sanal makine için boyuttur. İş yükünüz için uygun bir sanal makine boyutu seçin. Öğretici için bu ortamı oluşturuyorsanız, **DS2**kullanın. Üretim iş yükleri için, iş yükünü desteklenebilen sanal bir makine boyutu seçin. Birçok üretim iş yükü **DS4** veya daha büyük gerektirir. Şablon, bu boyutta iki sanal makine oluşturur ve her birine SQL Server yükler. Daha fazla bilgi [için sanal makineler için Boyutlar'a](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

> [!NOTE]
> Azure, SQL Server'ın Enterprise Edition'ını yükler. Maliyet baskıya ve sanal makine boyutuna bağlıdır. Geçerli maliyetler hakkında ayrıntılı bilgi için [sanal makinelerin fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql)na bakın.
>
>

* **Etki alanı denetleyicisanal makine boyutu** etki alanı denetleyicileri için sanal makine boyutudur. Bu öğretici için **D2**kullanın.
* **Dosya Paylaşım Tanık sanal makine boyutu** dosya paylaşım tanık için sanal makine boyutudur. Bu öğretici için **A1**kullanın.
* **SQL Depolama hesabı,** SQL Server verilerini ve işletim sistemi disklerini tutan depolama hesabının adıdır. Bu öğretici **için, alwaysonsql01**kullanın.
* **DC Depolama hesabı,** etki alanı denetleyicilerinin depolama hesabının adıdır. Bu öğretici **için, alwaysondc01**kullanın.
* **TB'deki SQL Server veri diskboyutu,** TB'deki SQL Server veri diskinin boyutudur. 1'den 4'e kadar bir sayı belirtin. Bu öğretici için **1**kullanın.
* **Depolama optimizasyonu,** SQL Server sanal makineleri için iş yükü türüne göre belirli depolama yapılandırma ayarları ayarlar. Bu senaryodaki tüm SQL Server sanal makineleri, salt okunur olarak ayarlanmış Azure disk ana bilgisayarı önbelleğiyle premium depolama kullanır. Ayrıca, şu üç ayardan birini seçerek iş yükü için SQL Server ayarlarını en iyi duruma getirebilirsiniz:

  * **Genel iş yükü** belirli bir yapılandırma ayarlarını ayarlar.
  * **İşlemsel işleme,** izleme bayrağı 1117 ve 1118'i ayarlar.
  * **Veri ambarı** 1117 ve 610 bayrak izleme ayarlar.

Bu öğretici için **Genel iş yükünü**kullanın.

![VM boyutunda depolama ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

Ayarları gözden geçirin ve ardından **Tamam'ı**tıklatın.

#### <a name="a-note-about-storage"></a>Depolama ile ilgili bir not
Ek optimizasyonlar SQL Server veri disklerinin boyutuna bağlıdır. Azure, her bir veri diski terabaytı için ek 1 TB premium depolama alanı ekler. Bir sunucu 2 TB veya daha fazla sınadığında, şablon her SQL Server sanal makinesinde bir depolama havuzu oluşturur. Depolama havuzu, birden çok diskin daha yüksek kapasite, esneklik ve performans sağlamak üzere yapılandırıldığı bir depolama sanallaştırma biçimidir.  Şablon daha sonra depolama havuzunda bir depolama alanı oluşturur ve işletim sistemine tek bir veri diski sunar. Şablon, bu diski SQL Server için veri diski olarak belirtir. Şablon, aşağıdaki ayarları kullanarak SQL Server depolama havuzunu ayarlar:

* Şerit boyutu, sanal diskin ara ayarını belirler. İşlemsel iş yükleri 64 KB kullanır. Veri depolama iş yükleri 256 KB kullanın.
* Esneklik basittir (esneklik yoktur).

> [!NOTE]
> Azure premium depolama yerel olarak gereksizdir ve verilerin üç kopyasını tek bir bölgede tutar, bu nedenle depolama havuzunda ek esneklik gerekmez.
>
>

* Sütun sayısı depolama havuzundaki disk sayısına eşittir.

Depolama alanı ve depolama havuzları hakkında daha fazla bilgi için bkz:

* [Depolama Alanlarına Genel Bakış](https://technet.microsoft.com/library/hh831739.aspx)
* [Windows Server Yedekleme ve Depolama Havuzları](https://technet.microsoft.com/library/dn390929.aspx)

SQL Server yapılandırmaen iyi uygulamalar hakkında daha fazla bilgi için, [Azure sanal makinelerde SQL Server için en iyi performans uygulamaları](virtual-machines-windows-sql-performance.md)bakın.

### <a name="sql-server-settings"></a>SQL Server ayarları
**SQL Server ayarlarında,** SQL Server sanal makine adı önekini, SQL Server sürümünü, SQL Server hizmet hesabını ve parolasını ve SQL otomatik yama bakım zamanlamasını gözden geçirin ve değiştirin.

* **SQL Server Ad Öneki,** her SQL Server sanal makinesi için bir ad oluşturmak için kullanılır. Bu öğretici için **sqlserver'ı**kullanın. Şablon, SQL Server sanal makineleri *sqlserver-0* ve *sqlserver-1*adlandırır.
* **SQL Server sürümü** SQL Server'ın sürümüdür. Bu eğitim için **SQL Server 2014**kullanın. AYRıCA SQL **Server 2012** veya **SQL Server 2016'yı**da seçebilirsiniz.
* **SQL Server hizmet hesabı kullanıcı adı,** SQL Server hizmetinin alan adıdır. Bu öğretici için **sqlservice'i**kullanın.
* **Parola,** SQL Server hizmet hesabının parolasıdır.  Karmaşık bir parola kullanın. Parola’yı onaylayın.
* **SQL Auto Patching bakım çizelgesi,** Azure'un SQL Sunucularını otomatik olarak yadığı haftanın gününü tanımlar. Bu öğretici için, **Pazar**yazın.
* **SQL Otomatik Yama bakım başlangıç saati,** otomatik düzeltme ekinin başladığı Azure bölgesi için günün saatidir.

> [!NOTE]
> Her sanal makine için yama penceresi bir saat ile sendelenir. Hizmetlerin aksamasını önlemek için aynı anda yalnızca bir sanal makine yamalı.
>
>

![SQL Server ayarları](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Ayarları gözden geçirin ve ardından **Tamam'ı**tıklatın.

### <a name="summary"></a>Özet
Özet sayfasında, Azure ayarları doğrular. Şablonu da indirebilirsiniz. Özeti gözden geçirin. **Tamam**'a tıklayın.

### <a name="buy"></a>Satın al
Bu son bıçak **kullanım koşulları**ve **gizlilik politikası**içerir. Bu bilgileri gözden geçirin. Azure'un sanal makineleri ve kullanılabilirlik grubu için gerekli diğer tüm kaynakları oluşturmaya başlaması na hazır olduğunuzda **Oluştur'u**tıklatın.

Azure portalı kaynak grubunu ve tüm kaynakları oluşturur.

## <a name="monitor-deployment"></a>Dağıtımı izleme
Azure portalından dağıtım ilerlemesini izleyin. Dağıtımı temsil eden bir simge otomatik olarak Azure portal panosuna sabitlenir.

![Azure Panosu](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

## <a name="connect-to-sql-server"></a>SQL Server’a bağlanma
SQL Server'ın yeni örnekleri, Internet'e bağlı IP adreslerine sahip sanal makinelerde çalışıyor. Masaüstünü (RDP) doğrudan her SQL Server sanal makinesine uzaklatabilirsiniz.

BIR SQL Server'a RDP için aşağıdaki adımları izleyin:

1. Azure portal panosundan dağıtımın başarılı olduğunu doğrulayın.
2. **Kaynaklar'ı**tıklatın.
3. **Kaynaklar** bıçağında, SQL Server'ı çalıştıran sanal makinelerden birinin bilgisayar adı olan **sqlserver-0'ı**tıklatın.
4. **sqlserver-0**için bıçak üzerinde, **Connect'i**tıklatın. Tarayıcınız uzak bağlantı nesnesini açmak veya kaydetmek isteyip istemediğinizi sorar. **Aç**'a tıklayın.
5. **Uzak masaüstü bağlantısı,** bu uzak bağlantının yayımcısının tanımlanamayacağı konusunda sizi uyarabilir. **Bağlan**'a tıklayın.
6. Windows security, birincil etki alanı denetleyicisinin IP adresine bağlanmak için kimlik bilgilerinizi girmenizi ister. **Başka bir hesap kullan'ı**tıklatın. **Kullanıcı adı için,** **contoso\DomainAdmin**yazın. Şablonda yönetici kullanıcı adını ayarladığınızda bu hesabı yapılandırırsınız. Şablonu yapılandırırken seçtiğiniz karmaşık parolayı kullanın.
7. **Uzak masaüstü,** uzak bilgisayarın güvenlik sertifikasındaki sorunlar nedeniyle kimliğinin doğrulanamadığını konusunda sizi uyarabilir. Güvenlik sertifikası adını gösterir. Öğreticiyi izlediyseniz, adı **sqlserver-0.contoso.com.** **Evet'i**tıklatın.

Şimdi RDP ile SQL Server sanal makineye bağlısınız. SQL Server Management Studio'yu açabilir, varsayılan SQL Server örneğine bağlanabilir ve kullanılabilirlik grubunun yapılandırıldığından doğrulayabilirsiniz.
