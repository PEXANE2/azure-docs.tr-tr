---
title: Azure Site Kurtarma ile diskleri çoğaltmadan çıkarma
description: Azure Site Kurtarma ile diskleri çoğaltmadan Azure'a dışlama.
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 57bf06f0fde85714530c06cbd008db08de7460d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281853"
---
# <a name="exclude-disks-from-disaster-recovery"></a>Diskleri olağanüstü durum kurtarma dan hariç tutma

Bu makalede, olağanüstü durum kurtarma sırasında disklerin şirket içi azure [site kurtarma](site-recovery-overview.md)ile Azure'a nasıl dışlanabildiğini açıklamaktadır. Diskleri birkaç nedenden dolayı çoğaltmadan çıkarabilirsiniz:

- Böylece dışlanan diskte çalkalanan önemsiz veriler çoğaltılmaz.
- Tüketilen çoğaltma bant genişliğini veya hedef tarafındaki kaynakları en iyi duruma getirmek için.
- İhtiyacınız olmayan verileri çoğaltmayarak depolama ve ağ kaynaklarını kaydetmek için.
- Azure VM'ler Site Kurtarma çoğaltma sınırlarına ulaştı.


## <a name="supported-scenarios"></a>Desteklenen senaryolar

Diskleri tabloda özetlendiği gibi çoğaltmadan çıkarabilirsiniz.

**Azure-Azure arası** | **Vmware’den Azure’a** | **Hyper-V'den Azure'a** 
--- | --- | ---
Evet (PowerShell kullanarak) | Evet | Evet 

## <a name="exclude-limitations"></a>Sınırlamaları hariç tutma

**Sınırlama** | **Azure VM'ler** | **VMware Sanal Makineleri** | **Hyper-V Sanal Makineleri**
--- | --- | ---
**Disk türleri** | Temel diskleri çoğaltmadan hariç tutabilirsiniz.<br/><br/> İşletim sistemi disklerini veya dinamik diskleri hariç tutamazsınız. Geçici diskler varsayılan olarak dışlanır. | Temel diskleri çoğaltmadan hariç tutabilirsiniz.<br/><br/> İşletim sistemi disklerini veya dinamik diskleri hariç tutamazsınız. | Temel diskleri çoğaltmadan hariç tutabilirsiniz.<br/><br/> İşletim sistemi diskleri dışarıda tutulamaz. Dinamik diskleri dışarıda tutmamanızı öneririz. Site Kurtarma, konuk VM'de hangi VHS'nin temel veya dinamik olduğunu belirleyemez. Tüm bağımlı dinamik birim diskler dışlanmazsa, korumalı dinamik disk başarısız bir VM üzerinde başarısız bir disk olur ve bu diskteki verilere erişilemez.
**Diski çoğaltma** | Çoğalan bir diski dışlayamazsınız.<br/><br/> VM için devre dışı ve yeniden etkinleştirilebilir çoğaltma. |  Çoğalan bir diski dışlayamazsınız. |  Çoğalan bir diski dışlayamazsınız.
**Mobilite hizmeti (VMware)** | İlgili değil | Diskleri yalnızca Mobilite hizmeti yüklü olan VM'lerde hariç tutabilirsiniz.<br/><br/> Bu, diskleri hariç tutmak istediğiniz VM'lere Mobilite hizmetini el ile yüklemeniz gerektiği anlamına gelir. Mobilite hizmetini yalnızca çoğaltma etkinleştirildikten sonra yüklediği için itme yükleme mekanizmasını kullanamazsınız. | Konuyla ilgisi yok.
**Ekle/Kaldır** | Yönetilen disklerle Azure VM'lerine disk ekleyebilir ve kaldırabilirsiniz. | Çoğaltma etkinleştirildikten sonra disk ekleyip kaldıramam. Devre dışı ve sonra bir disk eklemek için çoğaltma yeniden etkinleştirin. | Çoğaltma etkinleştirildikten sonra disk ekleyip kaldıramam. Devre dışı ve sonra çoğaltma yı etkinleştirin.
**Başarısızlık** | Bir uygulamanın dışladığınız bir diske ihtiyacı varsa, başarısız olduktan sonra, çoğaltılan uygulamanın çalıştırabilmesi için diski el ile oluşturmanız gerekir.<br/><br/> Alternatif olarak, Azure otomasyonu bir kurtarma planına entegre ederek VM başarısız olduğu sırada diski oluşturabilirsiniz. | Bir uygulamanın ihtiyaç duyduğu bir diski dışlarsanız, başarısız olduktan sonra diski Azure'da el ile oluşturun. | Bir uygulamanın ihtiyaç duyduğu bir diski dışlarsanız, başarısız olduktan sonra diski Azure'da el ile oluşturun.
**El ile oluşturulan şirket içi geri dönüş diskleri** | İlgili değil | **Windows VM'ler**: Azure'da el ile oluşturulan diskler geri başarısız olmaz. Örneğin, üç disk üzerinde başarısız olur ve doğrudan bir Azure VM üzerinde iki disk oluşturursanız, yalnızca üzerinde başarısız olan üç disk geri başarısız olur.<br/><br/> **Linux VM'leri**: Azure'da el ile oluşturulan diskler geri başarısız oldu. Örneğin, üç disküzerinde başarısız olur ve bir Azure VM'de iki disk oluşturursanız, beşi de başarısız olur. El ile oluşturulmuş diskleri, yeniden çalışmanın dışında tutamazsınız. | Azure'da el ile oluşturulan diskler geri başarısız olmaz. Örneğin, üç disk üzerinde başarısız olur ve doğrudan bir Azure VM üzerinde iki disk oluşturursanız, yalnızca üzerinde başarısız olan üç disk geri başarısız olur.
**Şirket içi failback-Dışlanmış diskler** | İlgili değil | Orijinal makineye geri dönmezseniz, başarısız VM disk yapılandırması dışlanan diskleri içermez. VMware'den Azure çoğaltmasına hariç tutulmuş diskler, geri dönüşlü VM'de kullanılamaz. | Failback orijinal Hyper-V konumuna geldiğinde, failback VM disk yapılandırması orijinal kaynak VM diskile aynı kalır. Hyper-V sitesinden Azure çoğaltmasına kadar dışlanan diskler geri dönüş VM'de kullanılabilir.



## <a name="typical-scenarios"></a>Tipik senaryolar

Dışlama için harika adaylar olan veri karmaşasına örnek olarak bir sayfalama dosyasına (pagefile.sys) yazar ve Microsoft SQL Server'ın tempdb dosyasına yazar. İş yüküne ve depolama alt sistemine bağlı olarak, sayfalama ve tempdb dosyaları önemli miktarda karmaşa kaydedebilir. Bu tür verileri Azure'a çoğaltmak kaynak yoğundur.

- Hem işletim sistemini hem de sayfalama dosyasını içeren tek bir sanal diske sahip bir VM için çoğaltmayı en iyi duruma getirmek için şunları
    1. Tek sanal diski iki sanal diske bölün. Bir sanal diskte işletim sistemi ve diğerinde disk belleği dosyası vardır.
    2. Disk belleği dosyasını çoğaltmanın dışında tutun.

- Hem Microsoft SQL Server tempdb dosyasını hem de sistem veritabanı dosyasını içeren bir disk için çoğaltmayı en iyi duruma getirmek için şunları
    1. Sistem veritabanını ve tempdb dosyasını iki farklı diskte tutabilirsiniz.
    2. Tempdb diskini çoğaltmanın dışında tutabilirsiniz.

## <a name="example-1-exclude-the-sql-server-tempdb-disk"></a>Örnek 1: SQL Server tempdb diskini dışlama

Tempdb'yi hariç tutmak istediğimiz bir kaynak SQL Server Windows VM - **SalesDB***, disk dışlama, başarısızlık ve başarısızlığı nasıl ele alabileceğimize bakalım. 

### <a name="exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma

Biz kaynak Windows VM SalesDB bu diskler var.

**Disk adı** | **Konuk Işletim Sistemi diski** | **Sürücü harfi** | **Disk veri türü**
--- | --- | --- | ---
DB-Disk0-OS | Disk0 | C:\ | İşletim sistemi diski.
DB-Disk1| Disk1 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1.
DB-Disk2 (Disk, korumanın dışında tutuldu) | Disk2 | E:\ | Geçici dosyalar.
DB-Disk3 (Disk, korumanın dışında tutuldu) | Disk3 | F:\ | SQL tempdb veritabanı.<br/><br/> Klasör yolu - F:\MSSQL\Data\. Başarısız olmadan önce klasör yolunu not edin.
DB-Disk4 | Disk4 |G:\ | Kullanıcı Veritabanı2

1. SalesDB VM için çoğaltmayı etkinleştiriyoruz.
2. Bu disklerde veri karmaşası geçici olduğundan Disk2 ve Disk3'ün çoğaltmadan hariç tutmaz. 


### <a name="handle-disks-during-failover"></a>Arıza sırasında diskleri işleme

Diskler çoğaltılmdığından, Azure'da başarısız olduğunuzda bu diskler, başarısız olduktan sonra oluşturulan Azure VM'de bulunmaz. Azure VM bu tabloda özetlenmiş disklere sahiptir.

**Konuk Işletim Sistemi diski** | **Sürücü harfi** | **Disk veri türü**
--- | --- | ---
Disk0 | C:\ | İşletim sistemi diski.
Disk1 | E:\ | Geçici depolama<br/><br/>Azure bu diski ekler. Disk2 ve Disk3 çoğaltma dışında bırakıldı, E: kullanılabilir listeden ilk sürücü harfidir. Azure, geçici depolama birimine E: harfini atar. Çoğaltılan diskler için diğer sürücü harfleri aynı kalır.
Disk2 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1
Disk3 | G:\ | Kullanıcı Veritabanı2

Örneğimizde, SQL tempdb diski Disk3 çoğaltma dışında bırakıldı ve Azure VM'de kullanılamadığıiçin, SQL hizmeti durmuş durumda ve F:\MSSQL\Data yoluna ihtiyaç duyuyor. Bu yolu birkaç şekilde oluşturabilirsiniz: 

- Başarısız olduktan sonra yeni bir disk ekleyin ve tempdb klasör yolunu atayın.
- tempdb klasör yolu için var olan geçici bir depolama diski kullanın.

#### <a name="add-a-new-disk-after-failover"></a>Başarısız olduktan sonra yeni bir disk ekleme

1. Yük devretme öncesinde SQL tempdb.mdf ve tempdb.ldf yollarını yazın.
2. Azure portalından, azure vm üzerinde başarısız yeni bir disk ekleyin. Disk, kaynak SQL tempdb disk (Disk3) ile aynı boyutta (veya daha büyük) olmalıdır.
3. Azure VM'de oturum açın.
4. Disk yönetimi (diskmgmt.msc) konsolundan yeni eklenen diski başlatıp biçimlendirin.
5. SQL tempdb disk (F:) tarafından kullanılan aynı sürücü harfiatama
6. F: biriminde bir tempdb klasörü (F:\MSSQL\Data) oluşturun.
7. Hizmet konsolundan SQL hizmetini başlatın.

#### <a name="use-an-existing-temporary-storage-disk"></a>Varolan geçici depolama diskini kullanma 

1. Bir komut istemi açın.
2. Komut isteminden kurtarma modunda SQL Server çalıştırın.

        Net start MSSQLSERVER /f / T3608

3. tempdb yolunu yeni yola değiştirmek için aşağıdaki sqlcmd’yi çalıştırın.

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;     
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');       
        GO


4. Microsoft SQL Server hizmetini durdurun.

        Net stop MSSQLSERVER
5. Microsoft SQL Server hizmetini başlatın.

        Net start MSSQLSERVER



### <a name="vmware-vms-disks-during-failback-to-original-location"></a>VMware VM'ler: Orijinal konuma geri dönüş sırasında diskler

Şimdi, orijinal şirket içi konumunuza geri döndüğünüzde VMware VM'lerde disklerin nasıl işleyeceğinize bakalım.

- **Azure'da oluşturulan diskler**: Örneğimizde Windows VM kullandığından, Azure'da el ile oluşturduğunuz diskler, geri döndüğünüzde veya bir VM'yi yeniden korusanız sitenize çoğaltılmaz.
- **Azure'daki geçici depolama diski**: Geçici depolama diski şirket içi ana bilgisayarlara çoğaltılamaz.
- **Dışlanan diskler**: VMware'den Azure çoğaltmasına hariç tutulmuş diskler, başarısız olduktan sonra şirket içi VM'de kullanılamaz.

VMware VM'leri orijinal konuma geri getiremeden önce Azure VM disk ayarları aşağıdaki gibidir.

**Konuk Işletim Sistemi diski** | **Sürücü harfi** | **Disk veri türü**
--- | --- | ---
Disk0 | C:\ | İşletim sistemi diski.
Disk1 | E:\ | Geçici depolama.
Disk2 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1.
Disk3 | G:\ | Kullanıcı Veritabanı2.

Başarısız olduktan sonra, orijinal konumdaki VMware VM'de tabloda özetlenen diskler bulunur.

**Konuk Işletim Sistemi diski** | **Sürücü harfi** | **Disk veri türü**
--- | --- | ---
Disk0 | C:\ | İşletim sistemi diski.
Disk1 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1.
Disk2 | G:\ | Kullanıcı Veritabanı2.


### <a name="hyper-v-vms-disks-during-failback-to-original-location"></a>Hyper-V VM'ler: Orijinal konuma geri dönüş sırasında diskler

Şimdi, orijinal şirket içi konumunuza geri döndüğünüzde Hyper-V VM'lerde disklerin nasıl işleyeceğinizüzerinde görelim.

- **Azure'da oluşturulan diskler**: Azure'da el ile oluşturduğunuz diskler, geri döndüğünüzde veya bir VM'yi yeniden korusanız sitenize çoğaltılmaz.
- **Azure'daki geçici depolama diski**: Geçici depolama diski şirket içi ana bilgisayarlara çoğaltılamaz.
- **Dışlanan diskler**: Başarısız olduktan sonra VM disk yapılandırması orijinal VM disk yapılandırması ile aynıdır. Hyper-V'den Azure'a çoğaltma dışında tutulmuş diskler geri dönüş VM'de kullanılabilir.

Hyper-V VM'leri orijinal konuma geri getiremeden önce Azure VM disk ayarları aşağıdaki gibidir.

**Konuk Işletim Sistemi diski** | **Sürücü harfi** | **Disk veri türü**
--- | --- | ---
Disk0 | C:\ | İşletim sistemi diski.
Disk1 | E:\ | Geçici depolama.
Disk2 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1.
Disk3 | G:\ | Kullanıcı Veritabanı2.

Azure'dan şirket içi Hyper-V'ye planlanan başarısız (failback) sonrasında, orijinal konumdaki Hyper-V VM'de tabloda özetlenen diskler bulunur.

**Disk Adı** | **Konuk işletim sistemi disk#** | **Sürücü harfi** | **Disk veri türü**
 --- | --- | --- | ---
DB-Disk0-OS | Disk0 |   C:\ | İşletim sistemi diski.
DB-Disk1 | Disk1 | D:\ | SQL sistem veritabanı ve Kullanıcı Veritabanı1.
DB-Disk2 (Dışlanan disk) | Disk2 | E:\ | Geçici dosyalar.
DB-Disk3 (Dışlanan disk) | Disk3 | F:\ | SQL tempdb veritabanı<br/><br/> Klasör yolu (F:\MSSQL\Data\).
DB-Disk4 | Disk4 | G:\ | Kullanıcı Veritabanı2


## <a name="example-2-exclude-the-paging-file-disk"></a>Örnek 2: Sayfalama dosyası diskini dışla

Hem D sürücüsünde hem de alternatif bir sürücüde pagefile.sys dosya diskini hariç tutmak istediğimiz bir kaynak Windows VM için disk dışlama, başarısızlık ve başarısızlık nasıl işleyeceğinize bakalım.


### <a name="paging-file-on-the-d-drive"></a>D sürücüsünde sayfalama dosyası

Kaynak VM'de bu diskler var.

**Disk adı** | **Konuk Işletim Sistemi diski** | **Sürücü harfi** | **Disk veri türü**
--- | --- | --- | ---
DB-Disk0-OS | Disk0 | C:\ | İşletim sistemi diski
DB-Disk1 (Çoğaltma hariç) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Kullanıcı verileri 1
DB-Disk3 | Disk3 | F:\ | Kullanıcı verileri 2

Kaynak VM'deki sayfalama dosya ayarlarımız aşağıdaki gibidir:

![Kaynak sanal makinedeki disk belleği dosyası ayarları](./media/exclude-disks-replication/pagefile-d-drive-source-vm.png)

1. VM için çoğaltmayı etkinleştiriyoruz.
2. DB-Disk1'i çoğaltmadan hariç tutmalıyız.

#### <a name="disks-after-failover"></a>Başarısız olduktan sonra diskler

Azure VM'de başarısız olduktan sonra diskler tabloda özetlenmiştir.

**Disk adı** | **Konuk işletim sistemi disk no.** | **Sürücü harfi** | **Diskteki veri türü**
--- | --- | --- | ---
DB-Disk0-OS | Disk0 | C:\ | İşletim sistemi diski
DB-Disk1 | Disk1 | D:\ | Geçici depolama/pagefile.sys <br/><br/> Çünkü DB-Disk1 (D:) hariç tutuldu, D: kullanılabilir listeden ilk sürücü harfidir.<br/><br/> Azure, geçici depolama birimine D: harfini atar.<br/><br/> D: kullanılabilir olduğundan, VM sayfalama dosya ayarı aynı kalır).
DB-Disk2 | Disk2 | E:\ | Kullanıcı verileri 1
DB-Disk3 | Disk3 | F:\ | Kullanıcı verileri 2

Azure VM'deki sayfalama dosya ayarlarımız aşağıdaki gibidir:

![Azure sanal makinesindeki disk belleği dosyası ayarları](./media/exclude-disks-replication/pagefile-azure-vm-after-failover.png)

### <a name="paging-file-on-another-drive-not-d"></a>Başka bir sürücüde sayfalama dosyası (D değil:)

Sayfalama dosyasının D sürücüsünde olmadığı örneğe bakalım.  

Kaynak VM'de bu diskler var.

**Disk adı** | **Konuk Işletim Sistemi diski** | **Sürücü harfi** | **Disk veri türü**
--- | --- | --- | ---
DB-Disk0-OS | Disk0 | C:\ | İşletim sistemi diski
DB-Disk1 (Çoğaltma hariç) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Kullanıcı verileri 1
DB-Disk3 | Disk3 | F:\ | Kullanıcı verileri 2

Şirket içi VM'deki sayfalama dosya ayarlarımız aşağıdaki gibidir:

![Şirket içi sanal makinedeki disk belleği dosyası ayarları](./media/exclude-disks-replication/pagefile-g-drive-source-vm.png)

1. VM için çoğaltmayı etkinleştiriyoruz.
2. DB-Disk1'i çoğaltmadan hariç tutmalıyız.

#### <a name="disks-after-failover"></a>Başarısız olduktan sonra diskler

Azure VM'de başarısız olduktan sonra diskler tabloda özetlenmiştir.

**Disk adı** | **Konuk işletim sistemi disk#** | **Sürücü harfi** | **Disk veri türü**
--- | --- | --- | ---
DB-Disk0-OS | Disk0  |C:\ | İşletim sistemi diski
DB-Disk1 | Disk1 | D:\ | Geçici depolama<br/><br/> D: kullanılabilir sürücü harfleri listesindeki ilk harf olduğundan, Azure geçici depolama birimine D: harfini atar.<br/><br/> Tüm çoğaltılan diskler için, sürücü harfi aynı kalır.<br/><br/> G: disk kullanılamadığından, sistem C: sürücü çağrı dosyası için kullanır.
DB-Disk2 | Disk2 | E:\ | Kullanıcı verileri 1
DB-Disk3 | Disk3 | F:\ | Kullanıcı verileri 2

Azure VM'deki sayfalama dosya ayarlarımız aşağıdaki gibidir:

![Azure sanal makinesindeki disk belleği dosyası ayarları](./media/exclude-disks-replication/pagefile-azure-vm-after-failover-2.png)


## <a name="next-steps"></a>Sonraki adımlar

- Geçici depolama diski için yönergeler hakkında daha fazla bilgi edinin:
    - SQL Server TempDB ve Arabellek Havuzu Uzantılarını depolamak için Azure VM'lerinde SSD kullanma [hakkında bilgi edinin](https://blogs.technet.microsoft.com/dataplatforminsider/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)
    - Azure VM'lerde SQL Server için performans en iyi uygulamaları [gözden geçirin.](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)
- Dağıtımınız ayarlandıktan ve çalışmaya başladıktan sonra farklı türdeki yük devretmeler hakkında [daha fazla bilgi edinebilirsiniz](failover-failback-overview.md).

