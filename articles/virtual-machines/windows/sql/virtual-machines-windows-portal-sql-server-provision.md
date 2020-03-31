---
title: Azure portalı ile sanal makine sağlama
description: Bu nasıl yapılır kılavuzu, Azure portalında Windows SQL Server 2017 sanal makineleri oluşturma seçeneklerinizi açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 11/07/2019
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 1fdf776570b6f10a363fb98dfe343387d86219d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249795"
---
# <a name="how-to-provision-a-windows-sql-server-virtual-machine-in-the-azure-portal"></a>Azure portalında Windows SQL Server sanal makine nasıl sağlanmaktadır?

Bu kılavuz, Azure portalında bir Windows SQL Server sanal makinesi oluşturduğunuzda kullanılabilen farklı seçenekler hakkında ayrıntılı bilgi sağlar. Bu makalede, bir olası sağlama görevinden daha fazla geçen [SQL Server VM quickstart'tan](quickstart-sql-vm-create-portal.md)daha fazla yapılandırma seçeneği yer almaktadır. 

Kendi SQL Server VM oluşturmak için bu kılavuzu kullanın. Veya Azure portalındaki kullanılabilir seçenekler için başvuru olarak kullanın.

> [!TIP]
> SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sql-server-virtual-machine-gallery-images"></a><a id="select"></a>SQL Server sanal makine galerisi görüntüleri

Bir SQL Server sanal makine oluşturduğunuzda, sanal makine galerisinden önceden yapılandırılmış birkaç resimden birini seçebilirsiniz. Aşağıdaki adımlar, SQL Server 2017 görüntülerinden birinin nasıl seçilebildiğini gösterir.

1. Azure portalının sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna Azure SQL yazın. (İsteğe bağlı) En beğenilebilmek için **Azure SQL'in** yanındaki yıldızı seçin ve sol gezintiye öğe olarak ekleyin. 
1. SELECT SQL dağıtım **seçeneği** sayfasını açmak için **+ Ekle'yi** seçin. **Ayrıntıları Göster'i**seçerek ek bilgileri görüntüleyebilirsiniz. 
1. SQL `2017` **sanal makineler** kutucuğuna SQL Server resim arama kutusunu yazın ve ardından açılan pencereden **Windows Server 2016'da Ücretsiz SQL Server Lisansı: SQL Server 2017 Developer'ı** seçin. 


   ![SQL VM görüntüsünü seçin](media/virtual-machines-windows-portal-sql-server-provision/select-sql-vm-image-portal.png)

   > [!TIP]
   > Geliştirme sınama için SQL Server'ın tam özellikli, ücretsiz bir sürümü olduğundan Geliştirici sürümü bu gözden geçirme de kullanılır. Yalnızca çalışan VM'ler için ücret ödersiniz. Ancak, bu izlenme de kullanmak için görüntülerden herhangi birini seçmekte özgürüz. Kullanılabilir görüntülerin açıklaması için [SQL Server Windows Sanal Makinelere genel bakış](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo)bölümüne bakın.

   > [!TIP]
   > SQL Server'ın lisans maliyetleri, oluşturduğunuz VM'nin saniye başına fiyatlandırmasına dahil edilir ve baskıya ve çekime göre değişir. Ancak, SQL Server Developer sürümü geliştirme/test için ücretsizdir (üretim değildir) ve SQL Express hafif iş yükleri için ücretsizdir (1 GB'dan az bellek, 10 GB'dan az depolama alanı). Ayrıca kendi lisansınızı (BYOL) getirebilir ve yalnızca VM için ödeme yapabilirsiniz. Bu görüntü adlarının başına {BYOL} ön eki getirilir. 
   >
   > Bu seçeneklerle ilgili daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).


1. **Oluştur'u**seçin.


## <a name="1-configure-basic-settings"></a>1. Temel ayarları yapılandırın


Temel **Bilgiler** sekmesinde aşağıdaki bilgileri sağlayın:

* **Proje Ayrıntıları**altında, doğru aboneliğin seçildiğinden emin olun. 
*  Kaynak **grubu** bölümünde, listeden varolan bir kaynak grubu seçin veya yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin. Bir kaynak grubu, Azure’daki ilgili kaynakların bir koleksiyonudur (sanal makineler, depolama hesapları, sanal ağlar, vb.). 

    ![Abonelik](media/quickstart-sql-vm-create-portal/basics-project-details.png)

  > [!NOTE]
  > Yalnızca Azure’daki SQL Server dağıtımlarını test ediyor veya öğreniyorsanız, yeni bir kaynak grubu kullanmak faydalıdır. Test işleminizi tamamladıktan sonra, sanal makineyi ve bu kaynak grubu ile ilişkili tüm kaynakları otomatik olarak silmek için kaynak grubunu silin. Kaynak grupları hakkında daha fazla bilgi için Azure [Kaynak Yöneticisine Genel Bakış'a](../../../azure-resource-manager/management/overview.md)bakın.


* **Örnek altında ayrıntılar**:
    1. Benzersiz bir **Sanal makine adı**girin.  
    1. **Bölgeniz**için bir konum seçin. 
    1. Bu kılavuzun amacı için, **Kullanılabilirlik seçeneklerini** _altyapı artıklığı gerektirmeyecek_şekilde ayarlanmış olarak bırakın. Kullanılabilirlik seçenekleri hakkında daha fazla bilgi edinmek için [Kullanılabilirlik'e](../../windows/availability.md)bakın. 
    1. **Resim** _listesinde, Ücretsiz SQL Server Lisansı: SQL Server 2017 Developer on Windows Server 2016'yı_seçin.  
    1. Sanal makinenin **boyutu** için **boyutu değiştirmeyi** seçin ve **A2 Basic** teklifini seçin. Beklenmeyen ücretleri önlemek için kaynaklarınızı temizledikten sonra bu kaynaklarla işiniz bittiğinden emin olun. Üretim iş yükleri için [Azure Virtual Machines'de SQL Server için en iyi performans uygulamaları](virtual-machines-windows-sql-performance.md)’nda önerilen makine boyutlarına ve yapılandırmalara bakın.

    ![Örnek ayrıntıları](media/quickstart-sql-vm-create-portal/basics-instance-details.png)

> [!IMPORTANT]
> **Boyut seçin** penceresinde gösterilen tahmini aylık maliyet, SQL Server lisans maliyetlerini içermez. Bu tahmin sadece VM maliyetidir. SQL Server'ın Express ve Developer sürümleri için bu tahmin toplam tahmini maliyettir. Diğer sürümler için [Windows Sanal Makineler fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) bakın ve hedef SQL Server sürümünüzü seçin. Ayrıca, sanal makineler [için SQL Server Azure VM'leri](virtual-machines-windows-sql-server-pricing-guidance.md) ve Boyutlar için Fiyatlandırma [kılavuzuna](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

* **Yönetici hesabı**altında, bir kullanıcı adı ve parola sağlayın. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../../windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.

   ![Yönetici hesabı](media/quickstart-sql-vm-create-portal/basics-administrator-account.png)

* **Gelen bağlantı noktası kuralları altında,** **seçili bağlantı noktalarına izin ver'i** seçin ve ardından açılır bağlantı noktasından **RDP'yi (3389)** seçin. 

   ![Gelen bağlantı noktası kuralları](media/quickstart-sql-vm-create-portal/basics-inbound-port-rules.png)


## <a name="2-configure-optional-features"></a>2. İsteğe bağlı özellikleri yapılandırma

### <a name="disks"></a>Diskler

**Diskler** sekmesinde, disk seçeneklerinizi yapılandırın. 

* **OS disk türü**altında, açılır işletim sistemi için istediğiniz disk türünü seçin. Premium üretim sistemleri için önerilir, ancak Temel VM için kullanılamaz. Premium SSD'yi kullanmak için sanal makine boyutunu değiştirin. 
* **Gelişmiş**altında, kullanım altında **Evet'i** seçin **Yönetilen Diskler.**

   > [!NOTE]
   > Microsoft, SQL Server için Yönetilen Diskleri önerir. Yönetilen Diskler, depolama alanını arka planda yönetir. Ayrıca, Yönetilen Disklere sahip sanal makineler aynı kullanılabilirlik kümesinde olduğunda Azure uygun artıklık düzeyini sağlamak için depolama kaynaklarını dağıtır. Daha fazla bilgi için Azure [Yönetilen Disklere Genel Bakış'a](../managed-disks-overview.md)bakın. Bir kullanılabilirlik kümesindeki yönetilen diskler hakkında daha fazla ayrıntı için bkz. [Kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma](../manage-availability.md).

![SQL VM Disk ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-disks.png)
  
  
### <a name="networking"></a>Ağ Oluşturma

**Ağ** sekmesinde ağ seçeneklerinizi yapılandırın. 

* Yeni bir **sanal ağ**oluşturun veya SQL Server VM'iniz için varolan bir vNet kullanın. Bir **Subnet** de belirleyin. 

* **NIC ağ güvenlik grubu**altında, temel bir güvenlik grubu veya gelişmiş güvenlik grubu seçin. Temel seçeneği seçmek, SQL Server VM için gelen bağlantı noktalarını **(Temel** sekmesinde yapılandırılan aynı değerler) seçmenize olanak tanır. Gelişmiş seçeneği seçmek, varolan bir ağ güvenlik grubu seçmenize veya yeni bir ağ oluşturmanıza olanak tanır. 

* Ağ ayarlarında başka değişiklikler yapabilir veya varsayılan değerleri koruyabilirsiniz.

![SQL VM Ağ ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-networking.png)

#### <a name="monitoring"></a>İzleme

**İzleme** sekmesinde, izlemeyi ve otomatik kapatmayı yapılandırın. 

* Azure, VM için belirlenen aynı depolama hesabıyla varsayılan olarak **Önyükleme tanılamasını** sağlar. Bu ayarları burada değiştirebilir ve işletim **sistemi konuk tanılamayı**etkinleştirebilirsiniz. 
* **Sistem atanmış yönetilen kimlik** ve otomatik **kapatma** yı bu sekmede de etkinleştirebilirsiniz. 

![SQL VM yönetim ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-management.png)


## <a name="3-configure-sql-server-settings"></a>3. SQL Server ayarlarını yapılandırın

SQL **Server ayarları** sekmesinde, SQL Server için belirli ayarları ve optimizasyonları yapılandırın. SQL Server için yapılandırabileceğiniz ayarlar şunlardır:

| Ayar |
| --- |
| [Bağlantı](#connectivity) |
| [Kimlik doğrulaması](#authentication) |
| [Azure Anahtar Kasası Tümleştirme](#azure-key-vault-integration) |
| [Depolama yapılandırması](#storage-configuration) |
| [Otomatik Yama](#automated-patching) |
| [Otomatik Yedekleme](#automated-backup) |
| [Machine Learning Services](#machine-learning-services) |


### <a name="connectivity"></a>Bağlantı

**SQL bağlantısı** altında, bu VM’de SQL Server örneğini istediğiniz erişim türünü belirtin. Bu iznin amaçları doğrultusunda, internet üzerindeki makinelerden veya hizmetlerden SQL Server'a bağlantı sağlamak için **Genel (internet)** seçeneğini belirleyin. Bu seçenek seçildiğinde, Azure güvenlik duvarını ve ağ güvenlik grubunu otomatik olarak seçilen bağlantı noktasındaki trafiğe izin verecek şekilde yapılandırır.

> [!TIP]
> Varsayılan olarak, SQL Server **1433** gibi iyi bilinen bir bağlantı noktasını dinler. Daha yüksek güvenlik için önceki iletişim kutusunda dinleme bağlantı noktasını 1401 gibi varsayılan olmayan bir bağlantı noktasıyla değiştirin. Bağlantı noktasını değiştirirseniz, bu bağlantı noktasını kullanarak SSMS gibi istemci araçlarından bağlanmanız gerekir.

![SQL VM Güvenlik](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-security.png)

İnternet üzerinden SQL Server'a bağlanmak için, sonraki bölümde açıklanan, SQL Server Kimlik Doğrulamasını etkinleştirmeniz gerekir.

İnternet üzerinden Veritabanı Altyapısı’na bağlantıları etkinleştirmek istemiyorsanız, aşağıdaki seçeneklerden birini seçin:

* SQL Server’a Yalnızca VM içinden gelen bağlantılara izin vermek için **Yerel (yalnızca VM dahilinde)**
* SQL Server’a aynı sanal ağdaki makineler ve hizmetlerden gelen bağlantılara izin vermek için **Özel (Sanal Ağ dahilinde)**

Genel olarak, senaryonuzun izin verdiği en kısıtlayıcı bağlantıyı seçerek güvenliği geliştirin. Ancak tüm seçenekler Ağ Güvenlik Grubu kuralları ve SQL/Windows Kimlik Doğrulaması üzerinden korumaya alınabilir. VM oluşturulduktan sonra Ağ Güvenlik Grubu’nu düzenleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler](virtual-machines-windows-sql-security.md).



### <a name="authentication"></a>Kimlik doğrulaması

SQL Server Kimlik Doğrulaması gerekiyorsa, SQL Server **ayarları** sekmesinde **SQL kimlik doğrulaması** altında **Etkinleştir'i** tıklatın.

![SQL Server Kimlik Doğrulaması](./media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-authentication.png)

> [!NOTE]
> SQL Server'a internet üzerinden (Genel bağlantı seçeneği) erişmeye planlıyorsanız, burada SQL kimlik doğrulamasını etkinleştirmeniz gerekir. SQL Server'a genel erişim SQL Kimlik Doğrulaması kullanılması gerektirir.

SQL Server Kimlik Doğrulamasını etkinleştirirseniz, bir **Oturum açma adı** ve **parola** belirtin. Bu giriş adı BIR SQL Server Authentication giriş ve **sysadmin** sabit sunucu rolü üyesi olarak yapılandırılır. Kimlik Doğrulama Modları hakkında daha fazla bilgi için bkz. [Kimlik Doğrulama Modu Seçme](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

SQL Server Kimlik Doğrulamasını etkinleştirmezseniz, SQL Server örneğine bağlanmak için VM’deki yerel Yönetici hesabını kullanabilirsiniz.


### <a name="azure-key-vault-integration"></a>Azure Anahtar Kasası tümleştirme

Şifreleme için güvenlik sırlarını Azure'da depolamak için **SQL Server ayarlarını**seçin ve **Azure anahtar kasası tümleştirmesine**gidin. **Etkinleştir'i** seçin ve istenen bilgileri doldurun. 

![Azure Anahtar Kasası tümleştirme](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-akv.png)

Aşağıdaki tabloda Azure Anahtar Kasası Tümleştirmeyi yapılandırmak için gereken parametreler listelenmektedir.

| PARAMETRE | AÇIKLAMA | ÖRNEK |
| --- | --- | --- |
| **Anahtar Kasası URL'si** |Anahtar kasası konumu. |https:\//contosokeyvault.vault.azure.net/ |
| **Asıl ad** |Azure Active Directory hizmet asıl adı. Bu ad İstemci Kimliği olarak da bilinir. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Asıl gizli anahtar** |Azure Active Directory hizmet asıl gizli anahtarı. Bu gizli anahtar İstemci Gizli Anahtarı olarak da bilinir. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Kimlik bilgisi adı** |**Kimlik bilgisi adı**: AKV Tümleştirme, VM’nin anahtar kasasına erişim sağlamasına izin vererek, SQL Server’da bir kimlik bilgisi oluşturur Bu kimlik bilgisi için bir ad seçin. |mycred1 |

Daha fazla bilgi için bkz. [Azure VM’lerde SQL Server için Azure Anahtar Kasası Tümleştirmeyi Yapılandırma](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="storage-configuration"></a>Depolama yapılandırması

SQL **Server ayarları** sekmesinde, **Depolama yapılandırması**altında, Performans Optimize Edilmiş Depolama Yapılandırması sayfasını açmak ve depolama gereksinimlerini belirtmek için **yapılandırmayı değiştir'i** seçin.

![SQL VM Depolama yapılandırması](media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

**Depolama iyileştirme seçimi** altında aşağıdaki seçeneklerden birini seçin:

* **Genel**, varsayılan ayardır ve çoğu iş yükünü destekler.
* **İşlemsel işleme,** geleneksel veritabanı OLTP iş yükleri için depolama optimize eder.
* **Veri depolama**, depolamayı çözümleme ve raporlama iş yükleri için iyileştirir.

![SQL VM Depolama yapılandırması](media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration.png)

Değerleri varsayılan olarak bırakmayı seçebilir veya depolama topolojisini IOPS gereksinimlerinize uyacak şekilde el ile değiştirebilirsiniz. Daha fazla bilgi için [depolama yapılandırmasına](virtual-machines-windows-sql-server-storage-configuration.md)bakın. 

### <a name="sql-server-license"></a>SQL Sunucu Lisansı
Yazılım Güvencesi müşterisiyseniz, kendi SQL Server lisansınızı getirmek ve kaynaklardan tasarruf etmek için [Azure Karma Avantajı'ndan](https://azure.microsoft.com/pricing/hybrid-benefit/) yararlanabilirsiniz. 

![SQL VM Lisansı](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-license.png)

### <a name="automated-patching"></a>Otomatik düzeltme eki uygulama

**Otomatik düzeltme eki uygulama** varsayılan olarak etkindir. Otomatik düzeltme eki uygulama Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını sağlar. Bakım penceresi için haftanın gününü, saati ve süreyi belirtin. Azure düzeltme eki uygulamayı bu bakım penceresinde gerçekleştirir. Bakım penceresi zamanlaması saat için VM yerel saatini kullanır. Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını istemiyorsanız tıklayın **Devre dışı**’na tıklayın.  

![SQL VM otomatik yamalar](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-automated-patching.png)

Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Düzeltme Eki Uygulama](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Otomatik yedekleme

**Otomatik yedekleme** altında, tüm veritabanları için otomatik veritabanı yedeklemeyi etkinleştirin. Otomatik yedekleme varsayılan olarak devre dışıdır.

SQL otomatik yedeklemeyi etkinleştirdiğinizde aşağıdaki ayarları yapılandırabilirsiniz:

* Yedeklemeler için elde tutma süresi (gün)
* Yedeklemeler için kullanılacak depolama hesabı
* Yedeklemeler için şifreleme seçeneği ve parola
* Backup sistem veritabanları
* Yedekleme zamanlamasını yapılandırma

Yedeklemeyi şifrelemek için **Etkinleştir**’e tıklayın. Ardından **Parola**’yı belirtin. Azure yedeklemeleri şifrelemek için bir sertifika oluşturur ve bu sertifikayı korumak için belirtilen parolayı kullanır. Varsayılan olarak zamanlama otomatik olarak ayarlanır, ancak **El Kitabı'nı**seçerek manuel zamanlama oluşturabilirsiniz. 

![SQL VM otomatik yedekleme](media/virtual-machines-windows-portal-sql-server-provision/automated-backup.png)

Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Yedekleme](virtual-machines-windows-sql-automated-backup.md).


### <a name="machine-learning-services"></a>Machine Learning Services

[Makine Öğrenimi Hizmetlerini](/sql/advanced-analytics/)etkinleştirme seçeneğiniz var. Bu seçenek, SQL Server 2017'de Python ve R ile makine öğrenimini kullanmanızı sağlar. SQL Server **Ayarları** penceresinde **Etkinleştir'i** seçin.


## <a name="4-review--create"></a>4. İnceleme + oluşturma

Gözden **Geçir + oluştur** sekmesinde, özeti gözden geçirin ve bu VM için belirtilen SQL Server, kaynak grubu ve kaynakları oluşturmak için **Oluştur'u** seçin.

Azure portalından dağıtımı izleyebilirsiniz. Ekranın üst kısmındaki **Bildirimler** düğmesi dağıtımın temel durumunu gösterir.

> [!NOTE]
> Size dağıtım zamanları hakkında bir fikir vermek için,Doğu ABD bölgesinde, varsayılan ayarlarla bir SQL VM dağıttım. Bu test dağıtımının tamamlanması yaklaşık 12 dakika sürdü. Ancak bölgeniz ve seçili ayarlarınıza göre, daha hızlı veya daha yavaş dağıtım süresiyle karşılaşabilirsiniz.

## <a name="open-the-vm-with-remote-desktop"></a><a id="remotedesktop"></a>Uzak Masaüstü ile VM'yi açın

Uzak Masaüstü kullanarak SQL Server sanal makinesine bağlanmak için aşağıdaki adımları kullanın:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

SQL Server sanal makineye bağlandıktan sonra, SQL Server Management Studio'yu başlatabilir ve yerel yönetici kimlik bilgilerinizi kullanarak Windows Kimlik Doğrulamasına bağlanabilirsiniz. SQL Server Kimlik Doğrulamasını etkinleştirdiyseniz, sağlama işlemi sırasında yapılandırdığınız SQL oturum açma adı ve parolasını kullanarak da SQL Kimlik Doğrulamasına bağlanabilirsiniz.

Makineye erişim, gereksinimlerinize göre makineyi ve SQL Server ayarlarını doğrudan değiştirmenize olanak tanır. Örneğin, güvenlik duvarı ayarlarını yapılandırabilir veya SQL Server yapılandırma ayarlarını değiştirebilirsiniz.

## <a name="connect-to-sql-server-remotely"></a><a id="connect"></a>SQL Server'a uzaktan bağlanın

Bu gözden geçirme de, sanal makine ve **SQL Server Kimlik Doğrulaması**için **Genel** erişim seçtiniz. Bu ayarlar, İnternet üzerinden tüm istemcilerden gelen (doğru SQL oturum açma bilgilerine sahip oldukları varsayılarak) SQL Server bağlantılarına izin verecek şekilde sanal makineyi yapılandırdı.

> [!NOTE]
> Sağlama sırasında Genel’i seçmediyseniz, sağlamadan sonra SQL bağlantı ayarlarınızı portal üzerinden değiştirebilirsiniz. Daha fazla bilgi edinmek için bkz. [SQL bağlantı ayarlarınızı değiştirme](virtual-machines-windows-sql-connect.md#change).

Aşağıdaki bölümlerde, internet üzerinden SQL Server VM örneğinize nasıl bağlanılalışiz gösterin.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

  > [!NOTE]
  > Bu örnek, ortak bağlantı noktası 1433 kullanır. Ancak, SQL Server VM dağıtımı sırasında farklı bir bağlantı noktası (1401 gibi) belirtilmişse, bu değerin değiştirilmesi gerekir. 


## <a name="next-steps"></a>Sonraki adımlar

Azure’da SQL Server'ı kullanma hakkında diğer bilgiler için bkz. [Azure Virtual Machines’de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) ve [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md).
