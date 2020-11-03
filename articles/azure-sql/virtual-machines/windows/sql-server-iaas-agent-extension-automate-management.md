---
title: SQL Server IaaS Aracısı uzantısı nedir?
description: Bu makalede SQL Server IaaS aracı uzantısının, Azure VM 'lerinde SQL Server yönetim özel yönetim görevlerinin nasıl otomatikleştirilmesine yardımcı olduğu açıklanır. Bunlar otomatik yedekleme, otomatik düzeltme eki uygulama, Azure Key Vault tümleştirme, lisanslama yönetimi, depolama yapılandırması ve tüm SQL Server VM örneklerinin merkezi yönetimi gibi özellikler içerir.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/30/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 070058eae36bf4f8546cfcf4beb85ac5023e9c79
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286176"
---
# <a name="what-is-the-sql-server-iaas-agent-extension"></a>SQL Server IaaS Aracısı uzantısı nedir?
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]


SQL Server IaaS Aracısı uzantısı (Sqliaasextenma), yönetim ve yönetim görevlerini otomatikleştirmek için Azure sanal makinelerinde (VM) SQL Server çalışır. 

Bu makale, uzantıya genel bir bakış sağlar. SQL Server IaaS uzantısını Azure VM 'lerine SQL Server yüklemek için, [Otomatik yükleme](sql-vm-resource-provider-automatic-registration.md), [tek VM](sql-vm-resource-provider-register.md)'ler veya [VM 'ler için toplu olarak](sql-vm-resource-provider-bulk-register.md)makalelere bakın. 

## <a name="overview"></a>Genel Bakış

SQL Server IaaS Aracısı uzantısı, Azure VM 'lerinde SQL Server için birkaç avantaj sağlar: 

- **Özellik avantajları** : uzantı, Portal yönetimi, lisans esnekliği, otomatik yedekleme, otomatik düzeltme eki uygulama ve daha fazlası gibi birçok Otomasyon özelliği avantajlarından yararlanır. Ayrıntılar için bu makalenin ilerleyen kısımlarında bulunan [özellik avantajları](#feature-benefits) bölümüne bakın. 

- **Uyumluluk** : uzantı, Microsoft 'a Azure hibrit avantajı, ürün koşullarında belirtilen şekilde etkinleştirildiğini bildirmek için gereksinimi karşılamakta olan basitleştirilmiş bir yöntem sunar. Bu işlem, her kaynak için lisanslama kayıt formlarını yönetmeye gerek duymasını geçersiz kılar.  

- **Ücretsiz** : tüm üç yönetilebilirlik modundaki uzantı tamamen ücretsizdir. Kaynak sağlayıcısıyla veya değişen yönetim modlarıyla ilişkili ek bir maliyet yoktur. 

- **Basitleştirilmiş lisans yönetimi** : uzantı SQL Server lisans yönetimini basitleştirir ve [Azure Portal](manage-sql-vm-portal.md), Azure CLI veya PowerShell kullanarak etkinleştirilen Azure hibrit avantajı SQL Server VM 'leri hızlıca tanımlamanızı sağlar: 

   # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

   ```azurecli-interactive
   $vms = az sql vm list | ConvertFrom-Json
   $vms | Where-Object {$_.sqlServerLicenseType -eq "AHUB"}
   ```

   # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell-interactive
   Get-AzSqlVM | Where-Object {$_.LicenseType -eq 'AHUB'}
   ```

   ---


> [!IMPORTANT]
> SQL IaaS Aracısı uzantısı, Azure sanal makineler 'de SQL Server kullanırken müşterilere isteğe bağlı avantajlar vermek için veri toplar. Microsoft, bu verileri müşterinin öncelikli onayı olmadan lisanslama denetimleri için kullanmaz. Daha fazla bilgi için [SQL Server Gizlilik Eki](/sql/sql-server/sql-server-privacy#non-personal-data) ' ne bakın.


## <a name="feature-benefits"></a>Özellik avantajları 

SQL Server IaaS Aracısı uzantısı, SQL Server VM yönetmek için bir dizi özellik avantajlarından yararlanır. 

Aşağıdaki tabloda bu avantajlar ayrıntılı olarak verilmiştir: 


| Özellik | Açıklama |
| --- | --- |
| **Portal yönetimi** | [Portaldaki yönetimin](manage-sql-vm-portal.md)kilidini açarak SQL Server sanal makinelerinizi tek bir yerde görüntüleyebilir ve böylece SQL 'e özgü özellikleri doğrudan portaldan etkinleştirebilir ve devre dışı bırakabilmenizi sağlayabilirsiniz. 
| **Otomatik yedekleme** |Tüm veritabanları için, varsayılan örnek veya VM üzerinde [doğru şekilde yüklenmiş](frequently-asked-questions-faq.md#administration) bir SQL Server örneği için yedeklemelerin zamanlamasını otomatikleştirir. Daha fazla bilgi için bkz. [Azure sanal makinelerinde SQL Server Için otomatik yedekleme (Kaynak Yöneticisi)](automated-backup-sql-2014.md). |
| **Otomatik düzeltme eki uygulama** |, Önemli Windows ve VM 'niz için SQL Server güvenlik güncelleştirmelerinin gerçekleşmesi sırasında bir bakım penceresi yapılandırır, bu sayede iş yükünüz için yoğun zamanlarda güncelleştirmelerden kaçınabilirsiniz. Daha fazla bilgi için bkz. [Azure sanal makinelerinde SQL Server Için otomatik düzeltme eki uygulama (Kaynak Yöneticisi)](automated-patching.md). |
| **Azure Anahtar Kasası tümleştirme** |SQL Server VM Azure Key Vault otomatik olarak yüklemenize ve yapılandırmanıza olanak sağlar. Daha fazla bilgi için bkz. [Azure sanal makinelerinde SQL Server için Azure Key Vault tümleştirmesini yapılandırma (Kaynak Yöneticisi)](azure-key-vault-integration-configure.md). |
| **Esnek lisanslama** | Kendi lisansını getir (Azure Hibrit Avantajı olarak da bilinir) ile Kullandıkça Öde lisanslama modeline [sorunsuz](licensing-model-azure-hybrid-benefit-ahb-change.md) bir şekilde geçiş yaparak maliyetten tasarruf edin. | 
| **Esnek sürüm/sürüm** | SQL Server [sürümünü](change-sql-server-version.md) [veya sürümünü](change-sql-server-edition.md) değiştirmeye karar verirseniz, tüm SQL Server VM yeniden dağıtmak zorunda kalmadan, Azure Portal içindeki meta verileri güncelleştirebilirsiniz.  | 


## <a name="management-modes"></a>Yönetim modları

SQL IaaS uzantısının üç yönetim modu vardır:

- **Hafif** mod SQL Server yeniden başlatılmasını gerektirmez, ancak yalnızca SQL Server lisans türünü ve sürümünü değiştirmeyi destekler. Birden çok örneğe sahip veya bir yük devretme kümesi örneğine (FCı) katılan SQL Server VM 'Ler için bu seçeneği kullanın. Bu yönetim modu makinede SQL IaaS Aracısı uzantısı ikililerini korur, ancak aracıyı yüklemez. Hafif mod, [otomatik kayıt](sql-vm-resource-provider-automatic-registration.md) özelliği kullanılırken veya el ile kayıt sırasında bir yönetim türü belirtilmediğinde varsayılan yönetim modudur. Hafif mod kullanılırken bellek veya CPU üzerinde hiçbir etkisi yoktur ve ilişkili bir maliyet yoktur. SQL Server VM önce basit modda kaydetmeniz önerilir, sonra zamanlanmış bakım penceresi sırasında tam moda yükseltilir.

- **Tam** mod tüm işlevleri sunar, ancak SQL Server ve Sistem Yöneticisi izinlerinin yeniden başlatılmasını gerektirir. Tek bir örnekle SQL Server VM yönetmek için kullanın. Tam mod belleği ve CPU üzerinde en az etkisi olan iki Windows hizmeti yüklüyor. Bunlar, Görev Yöneticisi aracılığıyla izlenebilir. Tüm yönetilebilirlik modunu kullanmayla ilişkili bir maliyet yoktur. 

- **Noagent** modu, Windows Server 2008 ' de yüklü SQL Server 2008 ve SQL Server 2008 R2 için ayrılmıştır. NoAgent modu kullanılırken bellek veya CPU üzerinde bir etkisi yoktur. NoAgent yönetilebilirlik modunu kullanmayla ilişkili bir maliyet yoktur. 

SQL Server IaaS aracınızın geçerli modunu Azure PowerShell kullanarak görüntüleyebilirsiniz: 

  ```powershell-interactive
  # Get the SqlVirtualMachine
  $sqlvm = Get-AzSqlVM -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName
  $sqlvm.SqlManagementType
  ```

## <a name="installation"></a>Yükleme

SQL Server IaaS Aracısı uzantısı, SQL VM kaynak sağlayıcısı ile SQL Server sanal makinelerinizi kaydettiğinizde yüklenir. Kaynak sağlayıcısına kaydolmak, sanal makine kaynağından _ayrı_ bir kaynak olan aboneliğinizdeki **SQL sanal makine** _kaynağını_ oluşturur. Kaynak sağlayıcıdan SQL Server VM kaydını silmek **SQL sanal makine** _kaynağını_ kaldıracak ancak gerçek sanal makineyi temizlemiyor.

Azure portal üzerinden SQL Server VM Azure Marketi görüntüsünün dağıtımı, SQL Server VM kaynak sağlayıcısıyla otomatik olarak kaydeder. Bununla birlikte, bir Azure sanal makinesine SQL Server kendi kendine yüklemeyi veya özel bir VHD 'den bir Azure sanal makinesini sağlamayı seçerseniz, SQL IaaS Aracısı uzantısını yüklemek için SQL Server VM SQL VM kaynak sağlayıcısı ile kaydetmeniz gerekir. 

Uzantıyı yüklemek için SQL Server VM kaynak sağlayıcısına kaydedin:
- [Bir abonelikteki tüm geçerli ve gelecekteki VM 'Ler için otomatik olarak](sql-vm-resource-provider-automatic-registration.md)
- [Tek bir VM için](sql-vm-resource-provider-register.md)
- [Toplu olarak birden çok VM için](sql-vm-resource-provider-bulk-register.md)

### <a name="named-instance-support"></a>Adlandırılmış örnek desteği

SQL Server IaaS uzantısı, sanal makinede kullanılabilir tek SQL Server örneğidir SQL Server adlandırılmış bir örnekle çalışır. Uzantı birden çok SQL Server örneğine sahip VM 'Lere yüklenemeyecektir. 

SQL Server adlı bir örneğini kullanmak için, bir Azure sanal makinesini dağıtın, bu örneğe tek bir adlandırılmış SQL Server örneği yükledikten sonra uzantıyı yüklemek için [SQL VM kaynak sağlayıcısı](sql-vm-resource-provider-register.md) 'na kaydedin.

Alternatif olarak, Azure Marketi SQL Server görüntüsü ile adlandırılmış bir örnek kullanmak için şu adımları izleyin: 

   1. Azure Marketi 'nden bir SQL Server VM dağıtın. 
   1. SQL VM kaynak sağlayıcısı 'ndan SQL Server VM [kaydını kaldırın](sql-vm-resource-provider-register.md#unregister-from-rp) . 
   1. SQL Server tamamen SQL Server VM içinde kaldırın.
   1. SQL Server VM içindeki adlandırılmış bir örnekle SQL Server yükler. 
   1. SQL [VM kaynak sağlayıcısı ile SQL Server VM KAYDEDEREK](sql-vm-resource-provider-register.md#register-with-rp)SQL IaaS Aracısı uzantısını yükler. 

## <a name="verify-status-of-extension"></a>Uzantının durumunu doğrulama

Uzantının durumunu denetlemek için Azure portal veya Azure PowerShell kullanın. 


### <a name="azure-portal"></a>Azure portal

Azure portal uzantının yüklü olduğunu doğrulayın. 

Sanal makine bölmesinde **Tüm ayarlar** ' ı seçin ve ardından **Uzantılar** ' ı seçin. **Sqliaasextenma** uzantısının listelendiğini görmeniz gerekir.

![Azure portal IaaS Aracısı uzantısının durumu SQL Server](./media/sql-server-iaas-agent-extension-automate-management/azure-rm-sql-server-iaas-agent-portal.png)


### <a name="azure-powershell"></a>Azure PowerShell

**Get-AzVMSqlServerExtension** Azure PowerShell cmdlet 'ini de kullanabilirsiniz:

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

Önceki komut, aracının yüklendiğini onaylar ve genel durum bilgilerini sağlar. Aşağıdaki komutları kullanarak otomatik yedekleme ve düzeltme eki uygulama hakkında belirli durum bilgilerini alabilirsiniz:

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```


## <a name="limitations"></a>Sınırlamalar

SQL IaaS Aracısı uzantısı yalnızca şunları destekler: 

- Azure Resource Manager aracılığıyla dağıtılan SQL Server VM 'Ler. Klasik model aracılığıyla dağıtılan SQL Server VM 'Ler desteklenmez. 
- Genel veya Azure Kamu bulutuna dağıtılan VM 'Ler SQL Server. Diğer özel veya kamu bulutlarına dağıtımlar desteklenmez. 


## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

**Azure Marketi 'ndeki bir SQL Server görüntüsünden sağlanan SQL Server VM kaydetmem gerekir mi?**

Hayır. Microsoft, Azure Marketi 'ndeki SQL Server görüntülerden sağlanan VM 'Leri otomatik olarak kaydeder. SQL VM kaynak sağlayıcısına kaydolmak yalnızca VM 'nin Azure Marketi 'ndeki SQL Server görüntülerden *sağlanmaması* ve SQL Server kendi kendine yüklenmesi durumunda gereklidir.

**SQL VM kaynak sağlayıcısını tüm müşteriler kullanabilir mi?** 

Evet. Müşteriler, Azure Marketi 'nden bir SQL Server görüntüsü kullanmayan ve bunun yerine kendi kendine yüklenmiş SQL Server veya özel VHD 'leri getirdiklerinde SQL Server sanal makinelerini SQL VM kaynak sağlayıcısına kaydetmelidir. Tüm abonelik türlerinin (doğrudan, Kurumsal Anlaşma ve bulut çözümü sağlayıcısı) sahip olduğu VM 'Ler SQL VM kaynak sağlayıcısına kaydoabilir.

**SQL VM kaynak sağlayıcısına kaydolurken varsayılan yönetim modu nedir?**

SQL VM kaynak sağlayıcısı ile kaydettiğinizde varsayılan yönetim modu *hafif* olur. SQL VM kaynak sağlayıcısına kaydoldığınızda SQL Server Management özelliği ayarlanmamışsa, mod hafif olarak ayarlanır ve SQL Server hizmetiniz yeniden başlatılır. Önce basit modda SQL VM kaynak sağlayıcısına kaydolmanız ve ardından bakım penceresi sırasında tam olarak yükseltmeniz önerilir. Benzer şekilde, [otomatik kayıt özelliği](sql-vm-resource-provider-automatic-registration.md)kullanılırken varsayılan Yönetim de hafif olur.

**SQL VM kaynak sağlayıcısına kaydolma önkoşulları nelerdir?**

VM 'de yüklü SQL Server sahip olmayan SQL VM kaynak sağlayıcısı 'na kaydolmak için herhangi bir önkoşul yoktur. SQL IaaS Aracısı uzantısı tam modda yüklüyse, SQL Server hizmeti yeniden başlatılır, bu nedenle bir bakım penceresi sırasında bunu yapmanız önerilir.

**SQL VM kaynak sağlayıcısı ile kaydolacaktır, VM 'imde bir aracı yükler mi?**

Evet, SQL VM kaynak sağlayıcısına tam yönetilebilirlik modunda kaydolmak VM 'ye bir aracı yüklenir. Basit veya NoAgent modunda kaydolma, değildir. 

SQL VM kaynak sağlayıcısına yalnızca hafif modda kaydolma, SQL IaaS Aracısı uzantısı *ikililerini* VM 'ye kopyalar, aracıyı yüklemez. Bu ikili dosyalar daha sonra yönetim modu tam olarak yükseltildiğinde aracıyı yüklemek için kullanılır.


**VM 'imde SQL VM kaynak sağlayıcısı yeniden başlatma SQL Server kaydedilecek mı?**

Kayıt sırasında belirtilen moda bağlıdır. Basit veya NoAgent modu belirtilmişse SQL Server hizmeti yeniden başlatmaz. Ancak, yönetim modunun tam olarak belirtilmesi SQL Server hizmetinin yeniden başlatılmasına neden olur. Otomatik kayıt özelliği, Windows Server sürümü 2008 değilse SQL Server sanal makinelerinizi hafif modda kaydeder ve bu durumda SQL Server VM NoAgent modunda kaydedilir. 

**SQL VM kaynak sağlayıcısına kaydolurken basit ve NoAgent yönetim modları arasındaki fark nedir?** 

NoAgent yönetim modu, Windows Server 2008 üzerinde SQL Server 2008 ve SQL Server 2008 R2 için kullanılabilen tek yönetim modudur. Windows Server 'ın tüm sonraki sürümlerinde, kullanılabilen iki yönetilebilirlik modu hafif ve tamamludur. 

NoAgent modu, SQL Server sürüm ve sürüm özelliklerinin müşteri tarafından ayarlanmasını gerektirir. Hafif mod, SQL Server örneğinin sürümünü ve sürümünü bulmak için VM 'yi sorgular.

**SQL Server lisans türünü belirtmeden SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Hayır. SQL Server lisans türü, SQL VM kaynak sağlayıcısına kaydolurken isteğe bağlı bir özellik değildir. SQL Server lisans türünü, tüm yönetilebilirlik modlarında (NoAgent, hafif ve tam) SQL VM kaynak sağlayıcısı 'na kaydolurken Kullandıkça Öde veya Azure Hibrit Avantajı olarak ayarlamanız gerekir. Geliştirici veya değerlendirme sürümü gibi SQL Server yüklü ücretsiz sürümlerden herhangi birine sahipseniz, Kullandıkça Öde lisanslamayla kaydolmanız gerekir. Azure Hibrit Avantajı yalnızca, kurumsal ve standart sürümler gibi ücretli SQL Server sürümler için kullanılabilir.

**SQL Server IaaS uzantısını NoAgent modundan tam moda yükseltebilir miyim?**

Hayır. Yönetilebilirlik modunu tam veya hafif olarak yükseltmek, NoAgent modu için kullanılamaz. Bu, Windows Server 2008 ' in teknik sınırlamasıdır. Önce işletim sistemini önce Windows Server 2008 R2 veya sonraki bir sürüme yükseltmeniz gerekir, sonra tam yönetim moduna yükseltebilirsiniz. 

**SQL Server IaaS uzantısını hafif moddan tam moda yükseltebilir miyim?**

Evet. Yönetilebilirlik modunu hafif ' den Full ' a yükseltmek Azure PowerShell veya Azure portal aracılığıyla desteklenir. Bu, SQL Server hizmetinin yeniden başlatılmasını tetikler.

**SQL Server IaaS uzantısının tam moddan NoAgent veya hafif yönetim moduna indirgenmesini sağlayabilir miyim?**

Hayır. SQL Server IaaS uzantısının yönetilebilirlik modunu eski sürüme düşürme desteklenmez. Yönetilebilirlik modu tam moddan hafif veya NoAgent moduna düşürülemez ve hafif moddan NoAgent moduna düşürülemez. 

Yönetilebilirlik modunu tam yönetilebilirlik olarak değiştirmek için SQL sanal makine kaynağını bırakıp SQL VM kaynak sağlayıcısı 'ndan SQL Server VM [kaydını kaldırın](sql-vm-resource-provider-register.md#unregister-from-rp) ve SQL _VM kaynak sağlayıcısı_ ile SQL Server VM farklı bir yönetim modunda yeniden kaydedin.

**Azure portal SQL VM kaynak sağlayıcısı ile kayıt yapabilir miyim?**

Hayır. SQL VM kaynak sağlayıcısına kaydolma Azure portal kullanılamıyor. SQL VM kaynak sağlayıcısına kayıt yalnızca Azure CLı veya Azure PowerShell desteklenir. 

**SQL Server yüklenmeden önce SQL VM kaynak sağlayıcısı ile bir VM 'yi kaydedebilir miyim?**

Hayır. VM, SQL VM kaynak sağlayıcısına başarıyla kaydolmak için en az bir SQL Server (veritabanı altyapısı) örneğine sahip olmalıdır. VM üzerinde SQL Server örneği yoksa, yeni Microsoft. SqlVirtualMachine kaynağı başarısız durumda olur.

**Birden çok SQL Server örneği varsa SQL VM kaynak sağlayıcısı ile bir VM 'yi kaydedebilir miyim?**

Evet. SQL VM kaynak sağlayıcısı yalnızca bir SQL Server (veritabanı altyapısı) örneğini kaydeder. SQL VM kaynak sağlayıcısı, birden çok örnek olması durumunda varsayılan SQL Server örneğini kaydeder. Varsayılan örnek yoksa, yalnızca hafif modda kaydolma desteklenir. Hafif ve tam yönetilebilirlik moduna yükseltmek için, varsayılan SQL Server örneği var olmalıdır veya VM yalnızca bir adlandırılmış SQL Server örneğine sahip olmalıdır.

**SQL Server yük devretme kümesi örneğini SQL VM kaynak sağlayıcısı ile kaydedebilir miyim?**

Evet. Azure VM 'de yük devretme kümesi örnekleri SQL Server, SQL VM kaynak sağlayıcısına hafif modda kaydedilebilir. Ancak, SQL Server yük devretme kümesi örnekleri tam yönetilebilirlik moduna yükseltilemez.

**Her zaman açık kullanılabilirlik grubu yapılandırılmışsa VM 'imi SQL VM kaynak sağlayıcısı ile kaydedebilir miyim?**

Evet. Her zaman açık kullanılabilirlik grubu yapılandırmasına katılıyorsanız, SQL VM kaynak sağlayıcısı ile bir Azure VM 'de SQL Server örneğini kaydetme kısıtlaması yoktur.

**SQL VM kaynak sağlayıcısına kaydolma veya tam yönetilebilirlik moduna yükseltme maliyeti nedir?**

Yok. SQL VM kaynak sağlayıcısı ile veya üç yönetilebilirlik modundan birini kullanarak kayıt ile ilişkili bir ücret alınmaz. SQL Server VM kaynak sağlayıcıyla yönetme tamamen ücretsizdir. 

**Farklı yönetilebilirlik modlarını kullanmanın performans etkisi nedir?**

*Noagent* ve *hafif* yönetilebilirlik modları kullanılırken hiçbir etkisi yoktur. İşletim sistemine yüklenen iki hizmetten *tam* yönetilebilirlik modunu kullanırken en az etkisi vardır. Bunlar, Görev Yöneticisi ile izlenebilir ve yerleşik Windows Hizmetleri konsolunda görülebilir. 

İki hizmet adı şunlardır:
- `SqlIaaSExtensionQuery` (Görünen ad- `Microsoft SQL Server IaaS Query Service` )
- `SQLIaaSExtension` (Görünen ad- `Microsoft SQL Server IaaS Agent` )

**Uzantıyı kaldırmak Nasıl yaparım? mı?**

SQL VM kaynak sağlayıcısı 'ndan SQL Server VM [kaydını](sql-vm-resource-provider-register.md#unregister-from-rp) kaldırarak uzantıyı kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar

SQL Server IaaS uzantısını Azure VM 'lerine SQL Server yüklemek için, [Otomatik yükleme](sql-vm-resource-provider-automatic-registration.md), [tek VM](sql-vm-resource-provider-register.md)'ler veya [VM 'ler için toplu olarak](sql-vm-resource-provider-bulk-register.md)makalelere bakın.

Azure sanal makinelerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [Azure sanal makinelerinde SQL Server nedir?](sql-server-on-azure-vm-iaas-what-is-overview.md).
