---
title: SQL Server VM'ler için depolama yapılandırması | Microsoft Dokümanlar
description: Bu konu, Azure'un sağlama sırasında SQL Server VM'ler için depolamaalanını nasıl yapılandırdığını açıklar (Kaynak Yöneticisi dağıtım modeli). Ayrıca, mevcut SQL Server VM'leriniz için depolama alanını nasıl yapılandırabileceğinizi de açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/26/2019
ms.author: mathoma
ms.openlocfilehash: 9d8fce0772f13c6e009b2441ecd85779a7622c5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243204"
---
# <a name="storage-configuration-for-sql-server-vms"></a>SQL Server VM’leri için depolama yapılandırması

Azure'da bir SQL Server sanal makine görseli yapılandırdığınızda, Portal depolama yapılandırmanızı otomatikleştirmenize yardımcı olur. Buna VM'ye depolama alanı eklemek, depolamayı SQL Server için erişilebilir hale getirmek ve belirli performans gereksinimleriniz için en iyi duruma getirmek üzere yapılandırmak da dahildir.

Bu konu, Azure'un SQL Server VM'leriniz için depolama alanını hem sağlama sırasında hem de varolan VM'ler için nasıl yapılandırdığını açıklar. Bu yapılandırma, SQL Server çalıştıran Azure VM'ler için [en iyi performans uygulamalarını](virtual-machines-windows-sql-performance.md) temel almaktadır.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Ön koşullar

Sanal makineniz otomatik depolama yapılandırma ayarlarını kullanmak için aşağıdaki özellikleri gerektirir:

* SQL Server [galeri görüntüsü](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo)ile birlikte .
* Kaynak [Yöneticisi dağıtım modelini](../../../azure-resource-manager/management/deployment-models.md)kullanır.
* [Premium SSD'ler](../disks-types.md)kullanır.

## <a name="new-vms"></a>Yeni VM'ler

Aşağıdaki bölümlerde, yeni SQL Server sanal makineleri için depolamanın nasıl yapılandırılabildiğini açıklanmıştır.

### <a name="azure-portal"></a>Azure portalında

SQL Server galeri görüntüsünü kullanarak Bir Azure VM sağlarken, Performans Optimize Edilmiş Depolama Yapılandırması sayfasını açmak için **SQL Server Ayarları** sekmesinde **yapılandırmayı değiştir'i** seçin. Değerleri varsayılan olarak bırakabilir veya iş yükünüze bağlı olarak gereksinimlerinize en uygun disk yapılandırmatürünü değiştirebilirsiniz. 

![SQL Server VM Depolama Yapılandırması Sağlama Sırasında](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

**Depolama optimizasyonu**altında SQL Server'ınızı dağıttığınız iş yükü türünü seçin. **Genel** optimizasyon seçeneği ile, varsayılan olarak 5000 max IOPS ile bir veri diskiniz olacak ve verileriniz, işlem günlüğünüz ve TempDB depolamanız için aynı sürücüyü kullanacaksınız. **İşlemişleme** (OLTP) veya **Veri depolama** sını seçmek veri için ayrı bir disk, işlem günlüğü için ayrı bir disk oluşturur ve TempDB için yerel SSD'yi kullanır. **İşlemişleme** ve **Veri depolama**arasında depolama farkı yoktur, ancak [şerit yapılandırmanızı](#workload-optimization-settings)değiştirir ve bayrakları izler. Premium depolama alanı seçmek, veri sürücüsü için *ReadOnly* önbelleğe alma ve [SQL Server VM performans en iyi uygulamalara](virtual-machines-windows-sql-performance.md)göre günlük sürücüsü için *Yok'u* ayarlar. 

![SQL Server VM Depolama Yapılandırması Sağlama Sırasında](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration.png)

Disk yapılandırması tamamen özelleştirilebilir, böylece SQL Server VM iş yükünüz için ihtiyacınız olan depolama topolojisini, disk türünü ve IOPs'yi yapılandırabilirsiniz. Ayrıca, SQL Server VM'niz desteklenen bölgelerden birinde (Doğu ABD 2, Güneydoğu Asya ve Kuzey Avrupa) bulunuyorsa ve [aboneliğiniz için ultra diskleri](/azure/virtual-machines/windows/disks-enable-ultra-ssd)etkinleştirmişseniz, **Disk türü** için bir seçenek olarak UltraSSD (önizleme) kullanabilirsiniz.  

Ayrıca, diskler için önbelleğe alma yeteneğine sahipsiniz. Azure [VM'ler, Premium Diskler](/azure/virtual-machines/windows/disks-types#premium-ssd)ile kullanıldığında [Blob Cache](/azure/virtual-machines/windows/premium-storage-performance#disk-caching) adı verilen çok katmanlı bir önbelleğe alma teknolojisine sahiptir. Blob Cache önbelleğe alma için Sanal Makine RAM ve yerel SSD bir arada kullanır. 

Premium SSD için disk önbelleğe alma *ReadOnly,* *ReadWrite* veya *None*olabilir. 

- *ReadOnly* önbelleğe alma, Premium Depolama'da depolanan SQL Server veri dosyaları için son derece yararlıdır. *ReadOnly* önbelleğe alma düşük okuma gecikmesi, yüksek okuma IOPS ve iş çıkış gibi, okumalar, VM bellek ve yerel SSD içinde os önbellek, gerçekleştirilir getiriyor. Bu okumalar, Azure blob depolama sından alınan veri diskinden okumalardan çok daha hızlıdır. Premium depolama, önbellekten disk IOPS'ye ve iş elde etme alanına doğru sunulan okumaları saymaz. Bu nedenle, uygulanabilir daha yüksek toplam IOPS karınca elde etmek mümkün. 
- Sql Server Log dosyasını barındıran diskler için *hiçbir* önbellek yapılandırması kullanılmamalıdır, çünkü günlük dosyası sırayla yazılır ve *ReadOnly* önbelleğe almadan yararlanamaz. 
- *READWrite* önbelleği, SQL Server dosyalarını barındırmak için kullanılmamalıdır, çünkü SQL Server *ReadWrite* önbelleğiyle veri tutarlılığını desteklemez. ReadOnly blob *ReadOnly* önbelleği ve gecikmelerin atık kapasitesini yazar, *readOnly* blob önbellek katmanlarından geçerse biraz artar. 


   > [!TIP]
   > Depolama yapılandırmanızın, seçili VM boyutunun uyguladığı sınırlamalarla eşleştiğinden emin olun. VM boyutunun performans sınırını aşan depolama parametrelerinin seçilmesi `The desired performance might not be reached due to the maximum virtual machine disk performance cap.`hataya neden olur: . Disk türünü değiştirerek IOP'leri azaltın veya VM boyutunu artırarak performans kapağı sınırlamasını artırın. 


Azure, seçimlerinize bağlı olarak, VM'yi oluşturduktan sonra aşağıdaki depolama yapılandırma görevlerini gerçekleştirir:

* Sanal makineye premium SSD'ler oluşturur ve bağlar.
* Veri disklerini SQL Server'a erişecek şekilde yapılandırır.
* Veri disklerini belirtilen boyut ve performans (IOPS ve iş ortası) gereksinimlerine göre bir depolama havuzuna dönüştürür.
* Depolama havuzunu sanal makinede yeni bir sürücüyle ilişkilendirin.
* Bu yeni sürücüyü, belirttiğiniz iş yükü türüne (Veri depolama, İşlemsel işleme veya Genel) göre optimize eder.

Azure'un depolama ayarlarını nasıl yapılandırdığını hakkında daha fazla bilgi için [Depolama yapılandırma bölümüne](#storage-configuration)bakın. Azure portalında sql server vm nasıl oluşturulacak hakkında tam bir gözden geçirme [için, sağlama öğreticisine](virtual-machines-windows-portal-sql-server-provision.md)bakın.

### <a name="resource-manage-templates"></a>Kaynak Yönet şablonları

Aşağıdaki Kaynak Yöneticisi şablonlarını kullanırsanız, depolama havuzu yapılandırması olmadan varsayılan olarak iki premium veri diski eklenir. Ancak, sanal makineye bağlı olan premium veri disklerinin sayısını değiştirmek için bu şablonları özelleştirebilirsiniz.

* [Otomatik Yedekleme ile VM Oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Otomatik Yama ile VM Oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [AKV Entegrasyonu ile VM Oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

### <a name="quickstart-template"></a>Hızlı Başlangıç şablonu

Depolama optimizasyonu kullanarak bir SQL Server VM dağıtmak için aşağıdaki quickstart şablonunu kullanabilirsiniz. 

* [Depolama optimizasyonu ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-new-storage/)
* [UltraSSD kullanarak VM oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-new-storage-ultrassd)

## <a name="existing-vms"></a>Varolan VM'ler

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Varolan SQL Server VM'leri için Azure portalındaki bazı depolama ayarlarını değiştirebilirsiniz. SQL [sanal makineler kaynağınızı](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource)açın ve **Genel Bakış'ı**seçin. SQL Server Genel Bakış sayfası, VM'nizin geçerli depolama kullanımını gösterir. VM'nizde bulunan tüm sürücüler bu grafikte görüntülenir. Her sürücü için depolama alanı dört bölümde görüntülenir:

* SQL verileri
* SQL günlüğü
* Diğer (SQL dışı depolama)
* Kullanılabilir

Depolama ayarlarını değiştirmek için **Ayarlar**altında **Yapılandır'ı** seçin. 

![Mevcut SQL Server VM için Depolamayı Yapılandırma](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

SQL Server VM oluşturma işlemi sırasında yapılandırılan sürücülerin disk ayarlarını değiştirebilirsiniz. Sürücüyü **Genişlet'i** seçmek, disk türünü değiştirmenize ve ek diskler eklemenize olanak tanıyan sürücü değişiklik sayfasını açar. 

![Mevcut SQL Server VM için Depolamayı Yapılandırma](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-drive.png)


## <a name="storage-configuration"></a>Depolama yapılandırması

Bu bölüm, Azure portalında SQL VM sağlama veya yapılandırma sırasında Azure'un otomatik olarak gerçekleştirdiği depolama yapılandırma değişiklikleri için bir başvuru sağlar.

* Azure, VM'nizden seçilen depolama alanının depolama havuzuna yapılandırır. Bu konunun sonraki bölümünde depolama havuzu yapılandırması hakkında ayrıntılı bilgi sağlar.
* Otomatik depolama yapılandırması her zaman [premium SSD'ler](../disks-types.md) P30 veri diskleri kullanır. Sonuç olarak, seçtiğiniz Terabayt sayısı ile VM'nize iliştirilen veri diski sayısı arasında 1:1 eşleme vardır.

Fiyatlandırma bilgileri için **Disk Depolama** sekmesindeki [Depolama fiyatlandırma](https://azure.microsoft.com/pricing/details/storage) sayfasına bakın.

### <a name="creation-of-the-storage-pool"></a>Depolama havuzunun oluşturulması

Azure, SQL Server VM'lerde depolama havuzu oluşturmak için aşağıdaki ayarları kullanır.

| Ayar | Değer |
| --- | --- |
| Şerit boyutu |256 KB (Veri depolama); 64 KB (İşlemsel) |
| Disk boyutları |Her biri 1 TB |
| Önbellek |Okuma |
| Tahsisboyutu |64 KB NTFS ayırma birimi boyutu |
| Kurtarma | Basit kurtarma (esneklik yok) |
| Sütun sayısı |8<sup>1'e</sup> kadar veri diski sayısı |


<sup>1</sup> Depolama havuzu oluşturulduktan sonra, depolama havuzundaki sütun sayısını değiştiremezsiniz.


## <a name="workload-optimization-settings"></a>İş yükü optimizasyonu ayarları

Aşağıdaki tabloda, kullanılabilir üç iş yükü türü seçeneği ve bunların karşılık gelen optimizasyonları açıklanmaktadır:

| İş yükü türü | Açıklama | İyileştirmeler |
| --- | --- | --- |
| **Genel** |Çoğu iş yükünü destekleyen varsayılan ayar |None |
| **İşlemsel işlem** |Geleneksel veritabanı OLTP iş yükleri için depolama optimize eder |İz Bayrağı 1117<br/>İz Bayrağı 1118 |
| **Veri ambarlama** |Analitik ve raporlama iş yükleri için depolamayı optimize eder |İz Bayrağı 610<br/>İz Bayrağı 1117 |

> [!NOTE]
> Yalnızca depolama yapılandırma adımında seçerek bir SQL sanal makine sağlarken iş yükü türünü belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'lerde SQL Server'ı çalıştırmakla ilgili diğer konular için [Azure Sanal Makineler'deki SQL Server'a](virtual-machines-windows-sql-server-iaas-overview.md)bakın.
