---
title: Azure portal Windows SQL Server VM 'Leri için sağlama kılavuzu | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda, Azure portal Windows SQL Server 2017 sanal makineleri oluşturma seçeneklerinizi açıklanmaktadır.
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
ms.date: 05/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 1e0bc4647476cd5c6aa0f38456ef8890b4ddcaa5
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71828773"
---
# <a name="how-to-provision-a-windows-sql-server-virtual-machine-in-the-azure-portal"></a>Azure portal bir Windows SQL Server sanal makinesi sağlama

Bu kılavuzda, Azure portal bir Windows SQL Server sanal makinesi oluşturduğunuzda kullanılabilir farklı seçenekler hakkında ayrıntılı bilgi sağlanır. Bu makalede, olası bir sağlama göreviyle daha fazla geçen [SQL Server VM hızlı başlangıç](quickstart-sql-vm-create-portal.md)ile daha fazla yapılandırma seçeneği ele alınmaktadır. 

Kendi SQL Server VM oluşturmak için bu kılavuzu kullanın. Ya da bunu, Azure portal kullanılabilen seçeneklere yönelik bir başvuru olarak kullanın.

> [!TIP]
> SQL Server sanal makineler hakkında sorularınız varsa, [sık sorulan sorular](virtual-machines-windows-sql-server-iaas-faq.md)bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a id="select"></a>SQL Server sanal makine galeri görüntüleri

SQL Server bir sanal makine oluşturduğunuzda, sanal makine galerisindeki önceden yapılandırılmış birkaç görüntüden birini seçebilirsiniz. Aşağıdaki adımlarda SQL Server 2017 görüntülerinden birini seçme gösterilmektedir.

1. Azure portal sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna Azure SQL yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin. 
1. **+ Ekle** ' yı seçerek **SQL dağıtım seçeneğini seçin** sayfasını açın. **Ayrıntıları göster**' i seçerek ek bilgileri görüntüleyebilirsiniz. 
1. **SQL sanal makineler** kutucuğunda SQL Server resim arama kutusuna `2017` yazın ve ardından, açılan listeden **Ücretsiz SQL Server Lisansı: Windows Server 2016 üzerinde SQL Server 2017 Developer** ' ı seçin. 


   ![SQL VM görüntüsünü seçin](media/virtual-machines-windows-portal-sql-server-provision/select-sql-vm-image-portal.png)

   > [!TIP]
   > Geliştirici sürümü, geliştirme testi için SQL Server tam özellikli, ücretsiz bir sürümü olduğundan Bu izlenecek yolda kullanılır. Yalnızca VM 'yi çalıştırmanın maliyeti için ödeme yaparsınız. Ancak, bu kılavuzda kullanılacak görüntülerden herhangi birini seçebilirsiniz. Kullanılabilir görüntülerin açıklaması için [SQL Server Windows sanal makineleri genel bakış](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo)bölümüne bakın.

   > [!TIP]
   > SQL Server lisans maliyetleri, oluşturduğunuz sanal makinenin ikinci başına fiyatlandırmasına dahil edilir ve sürüm ve çekirdekler için farklılık gösterir. Ancak, SQL Server Developer Edition geliştirme/test için (üretim değil) ücretsizdir ve SQL Express hafif iş yükleri (1 GB 'tan az depolama alanı 10 GB 'tan az) için ücretsizdir. Ayrıca, kendi lisansını de (KLG) getirebilir ve yalnızca VM için ödeme yapabilirsiniz. Bu görüntü adlarına {KLG} ön eki eklenir. 
   >
   > Bu seçenekler hakkında daha fazla bilgi için bkz. [Azure VM 'leri SQL Server Için fiyatlandırma Kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).


1. **Oluştur**' u seçin.


## <a name="1-configure-basic-settings"></a>1. temel ayarları Yapılandır


**Temel bilgiler** sekmesinde, aşağıdaki bilgileri sağlayın:

* **Proje ayrıntıları**' nın altında, doğru aboneliğin seçildiğinden emin olun. 
*  **Kaynak grubu** bölümünde, listeden var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturmak Için **Yeni oluştur** ' u seçin. Kaynak grubu, Azure 'daki ilgili kaynakların koleksiyonudur (sanal makineler, depolama hesapları, sanal ağlar, vb.). 

    ![Aboneliğiniz](media/quickstart-sql-vm-create-portal/basics-project-details.png)

  > [!NOTE]
  > Azure 'da SQL Server dağıtımlarını yalnızca test ediyorsanız veya öğrenmeniz durumunda yeni bir kaynak grubu kullanmak faydalıdır. Testinizi tamamladıktan sonra, sanal makineyi ve kaynak grubuyla ilişkili tüm kaynakları otomatik olarak silmek için kaynak grubunu silin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](../../../azure-resource-manager/resource-group-overview.md).


* **Örnek ayrıntıları**:
    1. Benzersiz bir **sanal makine adı**girin.  
    1. **Bölgeniz**için bir konum seçin. 
    1. Bu kılavuzun amacı doğrultusunda, **kullanılabilirlik seçeneklerini** _altyapı yedekliliği olmadan_ayarlayın. Kullanılabilirlik seçenekleri hakkında daha fazla bilgi edinmek için bkz. [kullanılabilirlik](../../windows/availability.md). 
    1. **Görüntü** listesinde _Ücretsiz SQL Server Lisansı: Windows Server 2016 üzerinde SQL Server 2017 Developer_' ı seçin.  
    1. Sanal makine **boyutunun** **boyutunu değiştirmeyi** seçin ve **a2 temel** teklifini seçin. Beklenmedik ücretleri engellemek için, bu kaynaklarla işiniz bittiğinde kaynaklarınızı temizlediğinizden emin olun. Üretim iş yükleri için, [Azure sanal makinelerinde SQL Server yönelik performans en iyi uygulamalarında](virtual-machines-windows-sql-performance.md)önerilen makine boyutlarına ve yapılandırmaya bakın.

    ![Örnek ayrıntıları](media/quickstart-sql-vm-create-portal/basics-instance-details.png)

> [!IMPORTANT]
> **Boyut seç** penceresinde görünen tahmini aylık maliyet SQL Server lisanslama maliyetlerini içermez. Bu tahmin VM 'nin tek başına maliyetidir. SQL Server Express ve Developer sürümleri için bu tahmin toplam tahmini maliyettir. Diğer sürümler için [Windows sanal makineleri fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) bakın ve SQL Server hedef sürümünüzü seçin. Ayrıca bkz. [Azure VM 'leri Için fiyatlandırma Kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md) ve [sanal makineler için Boyutlar](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)SQL Server.

* **Yönetici hesabı**altında, bir Kullanıcı adı ve parola sağlayın. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../../windows/faq.md#what-are-the-password-requirements-when-creating-a-vm)karşılamalıdır.

   ![Yönetici hesabı](media/quickstart-sql-vm-create-portal/basics-administrator-account.png)

* **Gelen bağlantı noktası kuralları**altında **Seçili bağlantı noktalarına izin ver** ' i seçin ve ardından açılır listeden **RDP (3389)** öğesini seçin. 

   ![Gelen bağlantı noktası kuralları](media/quickstart-sql-vm-create-portal/basics-inbound-port-rules.png)


## <a name="2-configure-optional-features"></a>2. isteğe bağlı özellikleri yapılandırma

### <a name="disks"></a>Disklerinden

**Diskler** sekmesinde disk seçeneklerinizi yapılandırın. 

* **Işletim sistemi disk türü**altında, işletim sistemi için istediğiniz disk türünü seçin. Premium, üretim sistemleri için önerilir, ancak temel bir VM için kullanılamaz. Premium SSD kullanmak için sanal makine boyutunu değiştirin. 
* **Gelişmiş**' in altında, **yönetilen diskleri**kullan altında **Evet** ' i seçin.

   > [!NOTE]
   > Microsoft SQL Server için yönetilen diskler önerir. Yönetilen diskler, arka planda depolamayı işler. Ayrıca, yönetilen diskleri olan sanal makineler aynı Kullanılabilirlik kümesinde olduğunda, Azure uygun artıklık sağlamak üzere depolama kaynaklarını dağıtır. Daha fazla bilgi için bkz. [Azure yönetilen disklere genel bakış](../managed-disks-overview.md). Bir kullanılabilirlik kümesindeki yönetilen diskler hakkında daha fazla bilgi için bkz. [kullanılabilirlik kümesindeki VM 'ler için yönetilen diskleri kullanma](../manage-availability.md).

![SQL VM disk ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-disks.png)
  
  
### <a name="networking"></a>Ağ Oluşturma

**Ağ** sekmesinde, ağ seçeneklerinizi yapılandırın. 

* Yeni bir **sanal ağ**oluşturun veya SQL Server VM mevcut bir vNet 'i kullanın. Bir **alt ağ** da belirleyin. 

* **NIC ağ güvenlik grubu**altında, temel bir güvenlik grubu ya da Gelişmiş güvenlik grubu ' nu seçin. Temel seçeneği belirlemek SQL Server VM için gelen bağlantı noktalarını seçmenizi sağlar ( **temel** sekmede yapılandırılmış olan değerler). Gelişmiş seçeneğinin belirlenmesi, var olan bir ağ güvenlik grubunu seçmenizi sağlar veya yeni bir tane oluşturabilirsiniz. 

* Ağ ayarlarında başka değişiklikler yapabilir veya varsayılan değerleri tutabilirsiniz.

![SQL VM ağ ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-networking.png)

#### <a name="monitoring"></a>İzleme

**İzleme** sekmesinde, izleme ve oto kapatma yapılandırın. 

* Azure, varsayılan olarak, sanal makine için ayrılmış depolama hesabıyla **önyükleme tanılamayı** mümkün bir şekilde sunar. Bu ayarları burada değiştirebilir ve **Işletim sistemi Konuk tanılamayı**etkinleştirebilirsiniz. 
* Bu sekmede, **sistem tarafından atanan yönetilen kimliği** ve **oto de kapatma** özelliğini etkinleştirebilirsiniz. 

![SQL VM yönetimi ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-management.png)


## <a name="3-configure-sql-server-settings"></a>3. SQL Server ayarlarını yapılandırın

**SQL Server ayarları** sekmesinde, SQL Server için belirli ayarları ve iyileştirmeleri yapılandırın. SQL Server için yapılandırabilmeniz gereken ayarlar şunlardır:

| Ayar |
| --- |
| [Bağlanabilirlik](#connectivity) |
| [Kimlik Doğrulaması](#authentication) |
| [Azure Key Vault tümleştirme](#azure-key-vault-integration) |
| [Depolama yapılandırması](#storage-configuration) |
| [Otomatik düzeltme eki uygulama](#automated-patching) |
| [Otomatik yedekleme](#automated-backup) |
| [R Services (gelişmiş analiz)](#r-services-advanced-analytics) |


### <a name="connectivity"></a>Bağlanabilirlik

**SQL bağlantısı**altında, bu VM 'de SQL Server örneğine istediğiniz erişim türünü belirtin. Bu izlenecek yolun amaçları doğrultusunda, internet 'teki makinelerden veya hizmetlerden SQL Server bağlantılara izin vermek için **Genel (Internet)** seçeneğini belirleyin. Bu seçenek belirlendiğinde Azure, güvenlik duvarını ve ağ güvenlik grubunu seçilen bağlantı noktasındaki trafiğe izin verecek şekilde otomatik olarak yapılandırır.

> [!TIP]
> Varsayılan olarak, SQL Server iyi bilinen bir bağlantı noktasını dinler, **1433**. Daha yüksek güvenlik için, önceki iletişim kutusundaki bağlantı noktasını, varsayılan olmayan bir bağlantı noktasında dinlemek üzere değiştirin (örneğin, 1401). Bağlantı noktasını değiştirirseniz SSMS gibi herhangi bir istemci araçlarından Bu bağlantı noktasını kullanarak bağlanmanız gerekir.

![SQL VM güvenliği](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-security.png)

Internet üzerinden SQL Server bağlanmak için, sonraki bölümde açıklanan SQL Server kimlik doğrulamasını da etkinleştirmeniz gerekir.

Internet üzerinden veritabanı altyapısına bağlantıları etkinleştirmeyi tercih ediyorsanız, aşağıdaki seçeneklerden birini seçin:

* Yalnızca VM içinden SQL Server bağlantılara izin vermek için **Yerel (yalnızca VM içinde)** .
* Aynı sanal ağdaki makinelerden veya hizmetlerden SQL Server bağlantılara izin vermek için **özel (sanal ağ içinde)** .

Genel olarak, senaryonuzun izin verdiği en kısıtlayıcı bağlantıyı seçerek güvenliği geliştirebilirsiniz. Ancak tüm seçenekler ağ güvenlik grubu kuralları ve SQL/Windows kimlik doğrulaması aracılığıyla güvenli kılınabilir. VM oluşturulduktan sonra ağ güvenlik grubunu düzenleyebilirsiniz. Daha fazla bilgi için bkz. [Azure sanal makinelerinde SQL Server Için güvenlik konuları](virtual-machines-windows-sql-security.md).



### <a name="authentication"></a>Kimlik doğrulaması

SQL Server kimlik doğrulaması gerekliyse, **SQL Server ayarları** sekmesinde **SQL kimlik doğrulaması** altında **Etkinleştir** ' e tıklayın.

![SQL Server kimlik doğrulaması](./media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-authentication.png)

> [!NOTE]
> Internet üzerinden SQL Server erişmeyi planlıyorsanız (genel bağlantı seçeneği), burada SQL kimlik doğrulamasını etkinleştirmeniz gerekir. SQL Server genel erişimi, SQL kimlik doğrulamasının kullanılmasını gerektirir.

SQL Server kimlik doğrulamasını etkinleştirirseniz, bir **oturum açma adı** ve **parola**belirtin. Bu oturum açma adı, SQL Server kimlik doğrulaması oturumu ve **sysadmin** sabit sunucu rolünün üyesi olarak yapılandırılır. Kimlik doğrulama modları hakkında daha fazla bilgi için bkz. [kimlik doğrulama modu seçme](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

SQL Server kimlik doğrulaması etkinleştirmezseniz, sanal makinede yerel yönetici hesabını kullanarak SQL Server örneğine bağlanabilirsiniz.


### <a name="azure-key-vault-integration"></a>Azure Key Vault tümleştirme

Şifreleme için Azure 'da güvenlik gizli dizileri depolamak için **SQL Server ayarları**' nı seçin ve **Azure Anahtar Kasası tümleştirmesi**' ne gidin. **Etkinleştir** ' i seçin ve istenen bilgileri girin. 

![Azure Key Vault tümleştirme](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-akv.png)

Aşağıdaki tabloda Azure Key Vault tümleştirmesini yapılandırmak için gereken parametreler listelenmektedir.

| PARAMETRESININ | AÇıKLAMASı | ÖRNEĞINDE |
| --- | --- | --- |
| **Key Vault URL 'SI** |Anahtar kasasının konumu. |https: \//contosokeykasa. kasa. Azure. net/ |
| **Asıl ad** |Azure Active Directory hizmet sorumlusu adı. Bu ad, Istemci KIMLIĞI olarak da adlandırılır. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Asıl gizli dizi** |Hizmet sorumlusu gizliliğini Azure Active Directory. Bu gizli anahtar, Istemci parolası olarak da adlandırılır. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM = |
| **Kimlik bilgisi adı** |**Kimlik bilgisi adı**: Akv tümleştirmesi SQL Server içinde bir kimlik bilgisi oluşturur ve bu da VM 'nin anahtar kasasına erişmesine izin verir. Bu kimlik bilgisi için bir ad seçin. |mycred1 |

Daha fazla bilgi için bkz. [Azure VM 'lerinde SQL Server için Azure Key Vault tümleştirmeyi yapılandırma](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="storage-configuration"></a>Depolama yapılandırması

**SQL Server ayarları** sekmesinde, **depolama yapılandırması**altında, **yapılandırma Değiştir** ' i seçerek performans için iyileştirilmiş depolama yapılandırması sayfasını açın ve depolama gereksinimlerini belirtin.

![SQL VM depolama yapılandırması](media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

**İçin en iyi duruma getirilmiş depolama**altında, aşağıdaki seçeneklerden birini seçin:

* **Genel** ayar varsayılan ayardır ve çoğu iş yükünü destekler.
* **İşlemsel işleme** , geleneksel veritabanı OLTP iş yükleri için depolamayı iyileştirir.
* **Veri** ambarı, analitik ve raporlama iş yükleri için depolamayı iyileştirir.

![SQL VM depolama yapılandırması](media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration.png)

Değerleri varsayılan olarak bırakmayı tercih edebilir veya depolama topolojisini ıOPS gereksinimlerinize uyacak şekilde el ile değiştirebilirsiniz. Daha fazla bilgi için bkz. [depolama yapılandırması](virtual-machines-windows-sql-server-storage-configuration.md). 

### <a name="sql-server-license"></a>SQL Server Lisansı
Yazılım Güvencesi müşterisi iseniz, kendi SQL Server lisansınızı getirmek ve kaynaklara kaydetmek için [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) kullanabilirsiniz. 

![SQL VM lisansı](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-license.png)

### <a name="automated-patching"></a>Otomatik düzeltme eki uygulama

**Otomatik düzeltme eki uygulama** varsayılan olarak etkindir. Otomatik düzeltme eki uygulama SQL Server ve işletim sistemine otomatik olarak düzeltme eki uygulamanıza olanak tanır. Bakım penceresi için haftanın gününü, saati ve süreyi belirtin. Azure bu bakım penceresinde düzeltme eki uygular. Bakım penceresi zamanlaması zaman için VM yerel ayarını kullanır. Azure 'un SQL Server ve işletim sistemine otomatik olarak düzeltme eki uygulamasına istemiyorsanız, **devre dışı bırak**' a tıklayın.  

![SQL VM otomatik düzeltme eki uygulama](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-automated-patching.png)

Daha fazla bilgi için bkz. [Azure sanal makinelerinde SQL Server Için otomatik düzeltme eki uygulama](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Otomatik yedekleme

Otomatik **yedekleme**altındaki tüm veritabanları için otomatik veritabanı yedeklerini etkinleştirin. Otomatik yedekleme varsayılan olarak devre dışıdır.

SQL Otomatik yedeklemeyi etkinleştirdiğinizde, aşağıdaki ayarları yapılandırabilirsiniz:

* Yedeklemeler için bekletme süresi (gün)
* Yedeklemeler için kullanılacak depolama hesabı
* Yedeklemeler için şifreleme seçeneği ve parola
* Yedekleme sistemi veritabanları
* Yedekleme Zamanlamasını Yapılandır

Yedeklemeyi şifrelemek için **Etkinleştir**' e tıklayın. Ardından **parolayı**belirtin. Azure, yedeklemeleri şifrelemek için bir sertifika oluşturur ve bu sertifikayı korumak için belirtilen parolayı kullanır. Zamanlama varsayılan olarak otomatik olarak ayarlanır, ancak **el ile**' yi seçerek el ile zamanlama oluşturabilirsiniz. 

![SQL VM otomatik yedeklemeleri](media/virtual-machines-windows-portal-sql-server-provision/automated-backup.png)

Daha fazla bilgi için bkz. [Azure sanal makineler 'de SQL Server Için otomatik yedekleme](virtual-machines-windows-sql-automated-backup.md).


### <a name="r-services-advanced-analytics"></a>R Services (gelişmiş analiz)

[SQL Server R Services (gelişmiş analiz)](/sql/advanced-analytics/r/sql-server-r-services/)seçeneğini etkinleştirme seçeneğiniz vardır. Bu seçenek SQL Server 2017 ile gelişmiş analiz kullanmanıza olanak sağlar. **SQL Server ayarları** penceresinde **Etkinleştir** ' i seçin.


## <a name="4-review--create"></a>4. gözden geçir + oluştur

**Gözden geçir + oluştur** sekmesinde, Özeti gözden geçirin ve **Oluştur** ' u seçerek bu VM için belirtilen SQL Server, kaynak grubu ve kaynakları oluşturun.

Azure portal dağıtımı izleyebilirsiniz. Ekranın üst kısmındaki **Bildirimler** düğmesi dağıtımın temel durumunu gösterir.

> [!NOTE]
> Dağıtım saatlerinde fikir sağlamak için, varsayılan ayarlarla Doğu ABD bölgesine bir SQL VM dağıttım. Bu test dağıtımının tamamlanması yaklaşık 12 dakika sürdü. Ancak bölgeniz ve seçtiğiniz ayarlara göre daha hızlı veya daha yavaş bir dağıtım süresi yaşayabilirsiniz.

## <a id="remotedesktop"></a>VM 'yi uzak masaüstü ile açma

Uzak Masaüstü ile SQL Server sanal makinesine bağlanmak için aşağıdaki adımları kullanın:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

SQL Server sanal makinesine bağlandıktan sonra, SQL Server Management Studio başlatabilir ve yerel yönetici kimlik bilgilerinizi kullanarak Windows kimlik doğrulamasıyla bağlantı sağlayabilirsiniz. SQL Server kimlik doğrulamasını etkinleştirdiyseniz, sağlama sırasında yapılandırdığınız SQL oturum açma ve parolasını kullanarak SQL kimlik doğrulamasına de bağlanabilirsiniz.

Makineye erişim, gereksinimlerinize bağlı olarak makine ve SQL Server ayarlarını doğrudan değiştirmenize olanak sağlar. Örneğin, güvenlik duvarı ayarlarını yapılandırabilir veya SQL Server yapılandırma ayarlarını değiştirebilirsiniz.

## <a id="connect"></a>SQL Server uzaktan bağlanma

Bu kılavuzda, sanal makine ve **SQL Server kimlik doğrulaması**için **genel** erişim ' i seçmiş olursunuz. Bu ayarlar, sanal makineyi Internet üzerinden herhangi bir istemciden SQL Server bağlantılara izin verecek şekilde otomatik olarak yapılandırırlar (doğru SQL oturum açmaları kabul edildiğinde).

> [!NOTE]
> Sağlama sırasında genel ' i seçmediyseniz, sağlamaktan sonra SQL bağlantı ayarlarınızı Portal aracılığıyla değiştirebilirsiniz. Daha fazla bilgi için bkz. [SQL bağlantı ayarlarınızı değiştirme](virtual-machines-windows-sql-connect.md#change).

Aşağıdaki bölümlerde, SQL Server VM örneğinize internet üzerinden nasıl bağlanalecek gösterilmektedir.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

  > [!NOTE]
  > Bu örnek, 1433 ortak bağlantı noktasını kullanır. Ancak, SQL Server VM dağıtımı sırasında farklı bir bağlantı noktası (1401 gibi) belirtilmişse bu değerin değiştirilmesi gerekir. 


## <a name="next-steps"></a>Sonraki adımlar

Azure 'da SQL Server kullanma hakkında diğer bilgiler için bkz. [Azure sanal makinelerinde SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) ve [sık sorulan sorular](virtual-machines-windows-sql-server-iaas-faq.md).
