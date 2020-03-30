---
title: 'Öğretici: Kullanılabilirlik grubunu yapılandırma'
description: Bu öğretici, Azure Sanal Makinelerde Kullanılabilirlik Grubunda Her Zaman Bir SQL Server'ın nasıl oluşturulacağımı gösterir.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/30/2018
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: 426ba4c0ac84799b4d0e6bf9330508f928437fd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060186"
---
# <a name="tutorial-configure-availability-group-on-azure-sql-server-vm-manually"></a>Öğretici: Azure SQL Server VM'deki kullanılabilirlik grubunu el ile yapılandırma

Bu öğretici, Azure Sanal Makinelerde Kullanılabilirlik Grubunda Her Zaman Bir SQL Server'ın nasıl oluşturulacağımı gösterir. Tam öğretici, iki SQL Sunucusunda veritabanı çoğaltma içeren bir Kullanılabilirlik Grubu oluşturur.

**Zaman tahmini**: Ön koşullar yerine getirildiğinde tamamlanması yaklaşık 30 dakika sürer.

Diyagram, öğreticide ne oluşturduğunuzu gösterir.

![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Ön koşullar

Öğretici, SQL Server Always On Availability Groups hakkında temel bir anlayışa sahip olduğunuzu varsayar. Daha fazla bilgiye ihtiyacınız varsa, [Her Zaman Kullanılabilirlik Gruplarına (SQL Server) Genel Bakış'a](https://msdn.microsoft.com/library/ff877884.aspx)bakın.

Aşağıdaki tablo, bu öğreticiyi başlatmadan önce tamamlamanız gereken ön koşulları listeler:

|  |Gereksinim |Açıklama |
|----- |----- |----- |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | İki SQL Sunucusu | - Azure kullanılabilirlik kümesinde <br/> - Tek bir etki alanında <br/> - Failover Clustering özelliği yüklü |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | Küme tanığı için dosya paylaşımı |  
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server hizmet hesabı | Etki alanı hesabı |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server Agent hizmet hesabı | Etki alanı hesabı |  
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Güvenlik duvarı bağlantı noktaları açık | - SQL Server: varsayılan örnek için **1433** <br/> - Veritabanı yansıtma bitiş noktası: **5022** veya herhangi bir kullanılabilir bağlantı noktası <br/> - Kullanılabilirlik grubu yük dengeleyici IP adresi sağlık prob: **59999** veya herhangi bir kullanılabilir bağlantı noktası <br/> - Küme çekirdek yük dengeleyici IP adresi sağlık prob: **58888** veya herhangi bir kullanılabilir bağlantı noktası |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Failover Kümeleme Özelliği Ekle | Her iki SQL Sunucusu da bu özelliğe ihtiyaç duyar |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Yükleme etki alanı hesabı | - Her SQL Sunucusunda yerel yönetici <br/> - SQL Server'ın her örneği için SQL Server sysadmin sabit sunucu rolü  |


Öğreticiye başlamadan önce, [Azure Sanal Makinelerde Her Zaman Kullanılabilirlik Grupları oluşturmak için ön koşulları tamamlamanız](virtual-machines-windows-portal-sql-availability-group-prereq.md)gerekir. Bu ön koşullar zaten tamamlanırsa, Küme [Oluştur'a](#CreateCluster)atlayabilirsiniz.

  >[!NOTE]
  > Bu öğreticide sağlanan adımların çoğu artık [Azure SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) ve [Azure Quickstart Şablonları](virtual-machines-windows-sql-availability-group-quickstart-template.md)ile otomatikleştirilebilebilir.


<!--**Procedure**: *This is the first "step". Make titles H2's and short and clear – H2's appear in the right pane on the web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## <a name="create-the-cluster"></a>Kümeyi oluşturma

Ön koşullar tamamlandıktan sonra, ilk adım iki SQL Sever ve bir tanık sunucusu içeren bir Windows Server Failover Cluster oluşturmaktır.

1. RDP ilk SQL Server hem SQL Sunucular ve tanık sunucu bir yönetici olan bir etki alanı hesabı kullanarak.

   >[!TIP]
   >[Önkoşullar belgesini](virtual-machines-windows-portal-sql-availability-group-prereq.md)takip ettiyseniz, **CORP\Install**adlı bir hesap oluşturdunuz. Bu hesabı kullanın.

2. Sunucu **Yöneticisi** panosunda **Araçlar'ı**seçin ve ardından **Failover Cluster Manager'ı**tıklatın.
3. Sol bölmede **Failover Cluster Manager'ı**sağ tıklatın ve ardından **Küme Oluştur'u**tıklatın.
   ![Küme Oluştur](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. Küme Oluştur Sihirbazı'nda, aşağıdaki tablodaki ayarlarla sayfalararasında adım atarak tek düğümlü küme oluşturun:

   | Sayfa | Ayarlar |
   | --- | --- |
   | Başlamadan Önce |Varsayılanları kullanma |
   | Sunucuları Seçin |**Sunucu adını girin** ve **Ekle'ye**tıklayın. |
   | Doğrulama Uyarısı |Bu küme için Microsoft'tan destek istemiyorum seçeneğini belirleyin **ve bu nedenle doğrulama testlerini çalıştırmak istemiyorum. İleri'yi tıklattığınızda, küme oluşturmaya devam edin.** |
   | Kümeyi Yönetmek için Erişim Noktası |Küme **Adı**örneğin **SQLAGCluster1'da** bir küme adı yazın.|
   | Onay |Depolama Alanları kullanmıyorsanız varsayılanları kullanın. Bu tabloyu izleyen nota bakın. |

### <a name="set-the-windows-server-failover-cluster-ip-address"></a>Windows sunucusu failover küme IP adresini ayarlama

  > [!NOTE]
  > Windows Server 2019'da küme, **Cluster Network Adı**yerine **Dağıtılmış Sunucu Adı** oluşturur. Windows Server 2019 kullanıyorsanız, bu öğreticide küme çekirdeği adına atıfta bulunan adımları atlayın. [PowerShell'i](virtual-machines-windows-portal-sql-create-failover-cluster.md#windows-server-2019)kullanarak bir küme ağ adı oluşturabilirsiniz. Daha fazla bilgi için [Failover Cluster: Cluster Network Object](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97) adlı blogu inceleyin. 

1. **Failover Cluster**Manager'da, **Cluster Core Resources'a** gidin ve küme ayrıntılarını genişletin. **Başarısız** durumda hem **Ad** hem de **IP Adresi** kaynaklarını görmeniz gerekir. Küme, makinenin kendisiyle aynı IP adresi atanır, bu nedenle yinelenen bir adres olduğundan, IP adresi kaynağı çevrimiçi olarak getirilemez.

2. Başarısız **IP Adresi** kaynağını sağ tıklatın ve ardından **Özellikler'i**tıklatın.

   ![Küme Özellikleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. **Statik IP Adresi'ni** seçin ve sanal makineleriniz ile aynı alt ağdan kullanılabilir bir adres belirtin.

4. Küme **Çekirdek Kaynakları** bölümünde, küme adı sağ tıklatın ve **Çevrimiçi Getir'i**tıklatın. Ardından, her iki kaynak çevrimiçi olana kadar bekleyin. Küme adı kaynağı çevrimiçi olduğunda, DC sunucusunu yeni bir AD bilgisayar hesabıyla güncelleştirir. Kullanılabilirlik Grubu kümelenmiş hizmeti daha sonra çalıştırmak için bu AD hesabını kullanın.

### <a name="add-the-other-sql-server-to-cluster"></a><a name="addNode"></a>Kümeye diğer SQL Server'ı ekleyin

Kümeye diğer SQL Server'ı ekleyin.

1. Tarayıcı ağacında, kümeyi sağ tıklatın ve **Düğüm Ekle'yi**tıklatın.

    ![Kümeye Düğüm Ekleme](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. Düğüm **Ekle**Sihirbazı'nda **İleri'yi**tıklatın. **Sunucuları Seç** sayfasında ikinci SQL Server'ı ekleyin. **Sunucu adını gir'e sunucu adını** yazın ve sonra **Ekle'yi**tıklatın. Bittiğinde, **İleri'yi**tıklatın.

1. Doğrulama **Uyarısı** sayfasında **Hayır'ı** tıklatın (bir üretim senaryosunda doğrulama testlerini gerçekleştirmeniz gerekir). Ardından **İleri'yi**tıklatın.

8. Depolama Alanları kullanıyorsanız **Onay** sayfasında, kümeye uygun **tüm depolama alanını ekle** etiketli onay kutusunu temizleyin.

   ![Düğüm Onayı Ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Depolama Alanları kullanıyorsanız ve **kümeye tüm uygun depolama alanını ekle'nin**işaretlerini geri almayacaksanız, Windows kümeleme işlemi sırasında sanal diskleri ayırır. Sonuç olarak, depolama alanları kümeden kaldırılıp PowerShell kullanılarak yeniden eklenene kadar Disk Yöneticisi veya Gezgin'de görünmezler. Depolama Alanları depolama havuzlarına birden çok disk gruplanır. Daha fazla bilgi için [Depolama Alanları'na](https://technet.microsoft.com/library/hh831739)bakın.

1. **İleri**'ye tıklayın.

1. **Son**'a tıklayın.

   Failover Cluster Manager kümenizin yeni bir düğümü olduğunu gösterir ve **düğümler** kapsayıcısında listeler.

10. Uzak masaüstü oturumundan çıkış yapın.

### <a name="add-a-cluster-quorum-file-share"></a>Küme çoğunluk dosya paylaşımı ekleme

Bu örnekte, Windows kümesi küme çoğunluğu oluşturmak için bir dosya paylaşımı kullanır. Bu öğretici, bir Düğüm ve Dosya Paylaşımı Çoğunluk çoğunluk kullanır. Daha fazla bilgi için bkz. [Yük Devretme Kümesindeki Çekirdek Yapılandırmalarını Anlama](https://technet.microsoft.com/library/cc731739.aspx).

1. Uzak bir masaüstü oturumu ile dosya paylaşım tanık üye sunucusuna bağlanın.

1. **Server Manager'da** **Araçlar'ı**tıklatın. Açık **Bilgisayar Yönetimi**.

1. **Paylaşılan Klasörler'i**tıklatın.

1. **Paylaşımlar'ı**sağ tıklatın ve **Yeni Paylaşım'a tıklayın...**.

   ![Yeni Paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Paylaşım oluşturmak için **Paylaşılan Klasör Sihirbazı** Oluştur'u kullanın.

1. **Klasör Yolu'nda** **Gözat'ı** tıklatın ve paylaşılan klasör için bir yol bulun veya bulun. **İleri**'ye tıklayın.

1. **Ad, Açıklama ve Ayarlar'da** paylaşım adını ve yolunu doğrulayın. **İleri**'ye tıklayın.

1. **Paylaşılan Klasör İzinleri'nde** **izinleri özelleştir'i**ayarlayın. **Özel...** seçeneğini tıklatın.

1. **İzinleri Özelleştir'de** **Ekle...'yi**tıklatın.

1. Küme oluşturmak için kullanılan hesabın tam denetime sahip olduğundan emin olun.

   ![Yeni Paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. **Tamam**'a tıklayın.

1. **Paylaşılan Klasör İzinleri'nde,** **Bitiş'i**tıklatın. Yeniden **Bitir'i** tıklatın.  

1. Sunucudan çıkış

### <a name="configure-cluster-quorum"></a>Küme çoğunluklarını yapılandırma

Ardından, küme çoğunluğunu ayarlayın.

1. Uzak masaüstü ile ilk küme düğümüne bağlanın.

1. **Failover Cluster**Manager'da, kümeye sağ tıklayın, **Daha Fazla Eylem'e**işaret edin ve Küme **Çoğunluk Ayarlarını Yapılandır'ı tıklatın...**.

   ![Yeni Paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. **Küme Çoğunluk Sihirbazı'nı Yapılandır'** da **İleri'yi**tıklatın.

1. **Quorum Yapılandırma Seçeneğini**Seç'te, **quorum tanığını seçin**ve **İleri'yi**tıklatın.

1. **Quorum Witness'ı seçin'** de, **dosya paylaşımı tanığını yapılandır'ı**tıklatın.

   >[!TIP]
   >Windows Server 2016 bir bulut tanığını destekler. Bu tür bir tanık seçerseniz, bir dosya paylaşımı tanığıgerekmez. Daha fazla bilgi için bkz: [Failover Kümesi için bir bulut tanığı dağıt.](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness) Bu öğretici, önceki işletim sistemleri tarafından desteklenen bir dosya paylaşım tanığı kullanır.

1. **Dosya Paylaşımı Tanığını Yapılandırın'** da oluşturduğunuz paylaşım için yolu yazın. **İleri**'ye tıklayın.

1. **Onay'daki**ayarları doğrulayın. **İleri**'ye tıklayın.

1. **Son**'a tıklayın.

Küme çekirdek kaynakları bir dosya paylaşım tanığı ile yapılandırılır.

## <a name="enable-availability-groups"></a>Kullanılabilirlik Gruplarını Etkinleştir

Ardından, **AlwaysOn Kullanılabilirlik Grupları** özelliğini etkinleştirin. Bu adımları her iki SQL Sunucusunda da yapın.

1. **Başlangıç** ekranından **SQL Server Configuration Manager'ı**başlatın.
2. Tarayıcı ağacında **SQL Server Services'ı**tıklatın, ardından SQL **Server (MSSQLSERVER)** hizmetini sağ tıklatın ve **Özellikler'i**tıklatın.
3. **AlwaysOn Yüksek Kullanılabilirlik** sekmesini tıklatın ve ardından Aşağıdaki gibi **Her Zaman Kullanılabilirlik Gruplarını Etkinleştir'i**seçin:

    ![AlwaysOn Kullanılabilirlik Gruplarını Etkinleştir](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. **Uygula**’ya tıklayın. Açılan iletişim kutusunda **Tamam'ı** tıklatın.

5. SQL Server hizmetini yeniden başlatın.

Bu adımları diğer SQL Server'da yineleyin.

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for the database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for the instance of SQL Server that is used to synchronize the database replicas in the Availability Groups on that instance.

On both SQL Servers, open the firewall for the TCP port for the database mirroring endpoint.

1. On the first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In the left pane, select **Inbound Rules**. On the right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For the port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In the **Action** page, keep **Allow the connection** selected and click **Next**.
6. In the **Profile** page, accept the default settings and click **Next**.
7. In the **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in the **Name** text box, then click **Finish**.

Repeat these steps on the second SQL Server.
-------------------------->

## <a name="create-a-database-on-the-first-sql-server"></a>İlk SQL Server'da veritabanı oluşturma

1. RDP dosyasını sysadmin sabit sunucu rolünün üyesi bir etki alanı hesabıyla ilk SQL Server'a başlatın.
1. SQL Server Management Studio'yu açın ve ilk SQL Server'a bağlanın.
7. **Object**Explorer'da, **Veritabanları'nı** sağ tıklatın ve **Yeni Veritabanı'nı**tıklatın.
8. **Veritabanı adına**, **MyDB1**yazın, sonra **Tamam'ı**tıklatın.

### <a name="create-a-backup-share"></a><a name="backupshare"></a>Yedekleme paylaşımı oluşturma

1. **Server Manager'daki**ilk SQL Server'da **Araçlar'ı**tıklatın. Açık **Bilgisayar Yönetimi**.

1. **Paylaşılan Klasörler'i**tıklatın.

1. **Paylaşımlar'ı**sağ tıklatın ve **Yeni Paylaşım'a tıklayın...**.

   ![Yeni Paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Paylaşım oluşturmak için **Paylaşılan Klasör Sihirbazı** Oluştur'u kullanın.

1. **Klasör Yolu'nda** **Gözat'ı** tıklatın ve veritabanı yedekleme paylaşılan klasörü için bir yol bulun veya oluşturun. **İleri**'ye tıklayın.

1. **Ad, Açıklama ve Ayarlar'da** paylaşım adını ve yolunu doğrulayın. **İleri**'ye tıklayın.

1. **Paylaşılan Klasör İzinleri'nde** **izinleri özelleştir'i**ayarlayın. **Özel...** seçeneğini tıklatın.

1. **İzinleri Özelleştir'de** **Ekle...'yi**tıklatın.

1. Her iki sunucu için SQL Server ve SQL Server Agent hizmet hesaplarının tam denetime sahip olduğundan emin olun.

   ![Yeni Paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. **Tamam**'a tıklayın.

1. **Paylaşılan Klasör İzinleri'nde,** **Bitiş'i**tıklatın. Yeniden **Bitir'i** tıklatın.  

### <a name="take-a-full-backup-of-the-database"></a>Veritabanının tam yedeğini alın

Günlük zincirini başlatmak için yeni veritabanını yedeklemeniz gerekir. Yeni veritabanının yedeğini almazsanız, kullanılabilirlik grubuna dahil edilemez.

1. **Object**Explorer'da, veritabanını sağ tıklatın, Görevler'e işaret **edin...**, **Yukarı'yı geri**tıklatın.

1. Varsayılan yedekleme konumuna tam yedekleme almak için **Tamam'ı** tıklatın.

## <a name="create-the-availability-group"></a>Kullanılabilirlik Grubu Oluşturma
Artık aşağıdaki adımları kullanarak bir Kullanılabilirlik Grubu yapılandırmaya hazırsınız:

* İlk SQL Server'da bir veritabanı oluşturun.
* Veritabanının hem tam yedeklemesini hem de işlem günlüğü yedeklemesini alın
* **NORECOVERY** seçeneği ile tam ve günlük yedeklemelerini ikinci SQL Server'a geri yükleme
* Eşzamanlı işleme, otomatik hata ve okunabilir ikincil yinelemelerle Kullanılabilirlik Grubu **(AG1)** oluşturun

### <a name="create-the-availability-group"></a>Kullanılabilirlik Grubu oluşturun:

1. İlk SQL Server için uzak masaüstü oturumunda. SSMS'teki **Object Explorer'da,** **AlwaysOn High Availability'ı** sağ tıklatın ve Yeni **Kullanılabilirlik Grubu Sihirbazı'nı**tıklatın.

    ![Yeni Kullanılabilirlik Grup Sihirbazı Başlat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. **Giriş** **sayfasında, İleri'yi**tıklatın. **Kullanılabilirlik Grubu Adı belirt** sayfasında, Kullanılabilirlik Grubu adı için bir ad yazın, örneğin **AG1**, **Kullanılabilirlik grubu adı**. **İleri**'ye tıklayın.

    ![Yeni AG Sihirbazı, AG Adını Belirt](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. **Veritabanlarını Seç** sayfasında veritabanınızı seçin ve **İleri'yi**tıklatın.

   >[!NOTE]
   >Veritabanı, amaçlanan birincil yinelemede en az bir tam yedekleme aldığınızdan, Kullanılabilirlik Grubu'nun ön koşullarıyla karşılar.

   ![Yeni AG Sihirbazı, Veritabanları seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. **Yinelemeler belirt** sayfasında, **Yineleme Ekle'yi**tıklatın.

   ![Yeni AG Sihirbazı, Yinelemeler Belirt](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. **Sunucuya Bağlan** iletişim kutusu açılır. **Sunucu adına**ikinci sunucunun adını yazın. **Bağlan**'a tıklayın.

   **Yinelemeleri Belirt** sayfasında, artık **Kullanılabilirlik Yinelemeleri'nde**listelenen ikinci sunucuyu görmeniz gerekir. Yinelemeleri aşağıdaki gibi yapılandırın.

   ![Yeni AG Sihirbazı, Yinelemeleri Belirt (Tamamlandı)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Bu Kullanılabilirlik Grubu için bitiş noktasını yansıtan veritabanını görmek için **Uç Noktaları'nı** tıklatın. [Veritabanı son noktalarını yansıtmak için güvenlik duvarı kuralını](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall)ayarlarken kullandığınız aynı bağlantı noktasını kullanın.

    ![Yeni AG Sihirbazı, İlk Veri Senkronizasyonunu Seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. İlk **Veri Eşitlemesi Seç** sayfasında **Tam'ı** seçin ve paylaşılan bir ağ konumu belirtin. Konum için [oluşturduğunuz yedekleme payını](#backupshare)kullanın. Örnekte, ** \\ \\ \<İlk SQL Server\>\\\Backup**oldu. **İleri**'ye tıklayın.

   >[!NOTE]
   >Tam eşitleme, SQL Server'ın ilk örneğinde veritabanının tam bir yedeklemesini alır ve ikinci örne geri yükler. Büyük veritabanları için, uzun zaman alabileceğinden tam eşitleme önerilmez. Veritabanının yedeğini el ile alarak ve `NO RECOVERY`'ile geri getirerek bu süreyi azaltabilirsiniz. Kullanılabilirlik Grubu'nu yapılandırmadan önce veritabanı zaten ikinci SQL `NO RECOVERY` Server'da geri yüklenmişse, yalnızca **Katıl'ı**seçin. Kullanılabilirlik Grubu'nu yapılandırdıktan sonra yedeklemeyi almak istiyorsanız, **ilk veri eşitlemeyi atla'yı**seçin.

    ![Yeni AG Sihirbazı, İlk Veri Senkronizasyonunu Seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. **Doğrulama** sayfasında **İleri'yi**tıklatın. Bu sayfa aşağıdaki resme benzer olmalıdır:

    ![Yeni AG Sihirbazı, Doğrulama](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Bir Kullanılabilirlik Grubu dinleyicisi yapılandırmadığınız için dinleyici yapılandırması için bir uyarı vardır. Bu uyarıyı yok sayabilirsiniz, çünkü Azure sanal makinelerde Azure yük bakiyesini oluşturduktan sonra dinleyiciyi oluşturursunuz.

10. **Özet** sayfasında, **Bitiş'i**tıklatın ve ardından sihirbaz yeni Kullanılabilirlik Grubunu yapılandırırken bekleyin. **İlerleme** sayfasında, ayrıntılı ilerlemeyi görüntülemek için **daha fazla ayrıntıyı** tıklatabilirsiniz. Sihirbaz tamamlandıktan sonra, Kullanılabilirlik Grubu'nun başarıyla oluşturulduğunu doğrulamak için **Sonuçlar** sayfasını inceleyin.

     ![Yeni AG Sihirbazı, Sonuçlar](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Sihirbazdan çıkmak için **Kapat** 'a tıklayın.

### <a name="check-the-availability-group"></a>Kullanılabilirlik Grubunu Kontrol Edin

1. **Object**Explorer'da, **AlwaysOn High Availability'ı genişletin,** ardından **Kullanılabilirlik Gruplarını**genişletin. Şimdi bu kapsayıcıda yeni Kullanılabilirlik Grubu görmeniz gerekir. Kullanılabilirlik Grubu'na sağ tıklayın ve **Panoyu Göster'i**tıklatın.

   ![AG Panosu'ni Göster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   **AlwaysOn Panosunuz** buna benzer olmalıdır.

   ![AG Panosu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   Yinelemeleri, her yinelemenin başarısız modunu ve eşitleme durumunu görebilirsiniz.

2. **Failover Cluster Manager'da**kümenizi tıklatın. **Roller'i**seçin. Kullandığınız Kullanılabilirlik Grubu adı kümedeki bir roldür. Bir dinleyiciyapılandırmadığınız için Bu Kullanılabilirlik Grubu'nun istemci bağlantıları için bir IP adresi yoktur. Bir Azure yük dengeleyicisi oluşturduktan sonra dinleyiciyi yapılandırabilirsiniz.

   ![Failover Cluster Manager'da AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Failover Cluster Manager'dan Kullanılabilirlik Grubu üzerinde başarısız olmaya çalışmayın. Tüm başarısız operasyonlar **SSMS'teki AlwaysOn Dashboard** içinden yapılmalıdır. Daha fazla bilgi için, [Kullanılabilirlik Gruplarıyla Failover Küme Yöneticisi'ni Kullanma Kısıtlamaları'na](https://msdn.microsoft.com/library/ff929171.aspx)bakın.
    >

Bu noktada, SQL Server'ın iki örneğinde yinelemeleri olan bir Kullanılabilirlik Grubuna sahip siniz. Kullanılabilirlik Grubu'nu örnekler arasında taşıyabilirsiniz. Dinleyiciniz olmadığı için henüz Kullanılabilirlik Grubuna bağlanamazsınız. Azure sanal makinelerde dinleyici bir yük dengeleyicisi gerektirir. Bir sonraki adım, Azure'da yük dengeleyicisini oluşturmaktır.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Azure yük dengeleyici oluşturma

Azure sanal makinelerde, BIR SQL Server Kullanılabilirlik Grubu bir yük dengeleyicisi gerektirir. Yük dengeleyicisi Kullanılabilirlik Grubu dinleyicileri ve Windows Server Failover Cluster için IP adreslerini tutar. Bu bölümde, Azure portalında yük dengeleyicisinin nasıl oluşturuluşu özetlenmiştir.

Azure Yük Dengeleyicisi Standart Yük Dengeleyicisi veya Temel Yük Dengeleyicisi olabilir. Standart Yük Dengeleyicisi, Temel Yük Dengeleyicisinden daha fazla özelliğe sahiptir. Bir kullanılabilirlik grubu için, Kullanılabilirlik Bölgesi (Kullanılabilirlik Kümesi yerine) kullanıyorsanız Standart Yük Dengeleyicisi gereklidir. Yük dengeleyici türleri arasındaki fark hakkında ayrıntılı bilgi için [Yük Dengeleyici SKU karşılaştırması](../../../load-balancer/concepts-limitations.md#skus)bakın.

1. Azure portalında, SQL Sunucularınızın bulunduğu kaynak grubuna gidin ve **+ Ekle'yi**tıklatın.
1. Yük **Dengeleyicisi'ni**arayın. Microsoft tarafından yayınlanan yük dengeleyicisini seçin.

   ![Failover Cluster Manager'da AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1. **Oluştur'u**tıklatın.
1. Yük dengeleyicisi için aşağıdaki parametreleri yapılandırın.

   | Ayar | Alan |
   | --- | --- |
   | **Adı** |Yük bakiyesi için bir metin adı kullanın, örneğin **sqlLB**. |
   | **Tür** |İç |
   | **Sanal ağ** |Azure sanal ağının adını kullanın. |
   | **Alt ağ** |Sanal makinenin içinde olduğu alt netin adını kullanın.  |
   | **IP adresi ataması** |Statik |
   | **IP adresi** |Alt ağdan kullanılabilir bir adres kullanın. Kullanılabilirlik grubu dinleyiciniz için bu adresi kullanın. Bunun küme IP adresinizden farklı olduğunu unutmayın.  |
   | **Abonelik** |Sanal makineyle aynı aboneliği kullanın. |
   | **Konum** |Sanal makineyle aynı konumu kullanın. |

   Azure portal bıçağı aşağıdaki gibi görünmelidir:

   ![Yük Dengeleyicisi Oluştur](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Yük dengeleyicisini oluşturmak için **Oluştur'u**tıklatın.

Yük dengeleyicisini yapılandırmak için bir arka uç havuzu, bir sonda oluşturmanız ve yük dengeleme kurallarını ayarlamanız gerekir. Bunları Azure portalında yapın.

### <a name="add-backend-pool-for-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi için arka uç havuzu ekleme

1. Azure portalında kullanılabilirlik grubunuzun durumuna bakın. Yeni oluşturulan yük dengeleyicisini görmek için görünümü yenilemeniz gerekebilir.

   ![Kaynak Grubunda Yük Dengeleyicisi Bul](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Yük bakiyesini tıklatın, **Arka uç havuzlarına**tıklayın ve **+Ekle'ye**tıklayın.

1. Arka uç havuzu için bir ad yazın.

1. Arka uç havuzunu VM'leri içeren kullanılabilirlik kümesiyle ilişkilendirin.

1. **Hedef ağ IP yapılandırmaları**altında, VIRTUAL **MACHINE'i** denetleyin ve kullanılabilirlik grubu yinelemelerini barındıracak sanal makinelerin her ikisini de seçin. Dosya paylaşımı tanık sunucusunu eklemeyin.

   >[!NOTE]
   >Her iki sanal makine belirtilmemişse, bağlantılar yalnızca birincil yinelemede başarılı olur.

1. Arka uç havuzunu oluşturmak için **Tamam'ı** tıklatın.

### <a name="set-the-probe"></a>Sondayı ayarlama

1. Yük bakiyesini tıklatın, **Sağlık sondalarını**tıklatın ve **+Ekle'ye**tıklayın.

1. Dinleyici sağlık sondasını aşağıdaki gibi ayarlayın:

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Adı** | Metin | SQLAlwaysonEndPointProbe |
   | **Protokolü** | TCP’yi seçin | TCP |
   | **Bağlantı noktası** | Kullanılmayan bağlantı noktası | 59999 |
   | **Interval**  | Sonda denemeleri arasındaki süre saniye cinsinden |5 |
   | **Sağlıksız durum eşiği** | Sanal bir makinenin sağlıksız olarak kabul edilememesi için oluşması gereken ardışık sonda hatalarının sayısı  | 2 |

1. Sistem durumu sondasını ayarlamak için **Tamam'ı** tıklatın.

### <a name="set-the-load-balancing-rules"></a>Yük dengeleme kurallarını ayarlama

1. Yük bakiyesini tıklatın, **Yük dengeleme kurallarını**tıklatın ve **+Ekle'ye**tıklayın.

1. Dinleyici yük dengeleme kurallarını aşağıdaki gibi ayarlayın.

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Adı** | Metin | SQLAlwaysOnEndPointListener |
   | **Ön uç IP adresi** | Adres seçin |Yük bakiyesini oluşturduğunuzda oluşturduğunuz adresi kullanın. |
   | **Protokolü** | TCP’yi seçin |TCP |
   | **Bağlantı noktası** | Kullanılabilirlik grubu dinleyicisi için bağlantı noktasını kullanma | 1433 |
   | **Arka Uç Bağlantı Noktası** | Kayan IP doğrudan sunucu iadesi için ayarlandığında bu alan kullanılmaz | 1433 |
   | **Sonda** |Sonda için belirttiğiniz ad | SQLAlwaysonEndPointProbe |
   | **Oturum Kalıcılığı** | Açılan liste | **Yok** |
   | **Boşta Kalma Süresi Zaman Aşımı** | TCP bağlantısını açık tutmak için dakikalar | 4 |
   | **Kayan IP (doğrudan sunucu iadesi)** | |Etkin |

   > [!WARNING]
   > Oluşturma sırasında doğrudan sunucu iadesi ayarlanır. Bu değer değiştirilemez.

1. Dinleyici yük dengeleme kurallarını ayarlamak için **Tamam'ı** tıklatın.

### <a name="add-the-cluster-core-ip-address-for-the-windows-server-failover-cluster-wsfc"></a>Windows Server Failover Cluster (WSFC) için küme çekirdeği IP adresini ekleyin

WSFC IP adreside de yük dengeleyicisi olması gerekir.

1. Portalda, aynı Azure yük bakiyesinde **Frontend IP yapılandırmasını** tıklatın ve **+Ekle'yi**tıklatın. Küme çekirdek kaynaklarında WSFC için yapılandırdığınız IP Adresini kullanın. IP adresini statik olarak ayarlayın.

1. Yük bakiyesi **üzerinde, Sistem Durumu sondalarını**tıklatın ve **+Ekle'yi**tıklatın.

1. WSFC küme çekirdeği IP adresi sistem durumu sondasını aşağıdaki gibi ayarlayın:

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Adı** | Metin | WSFCEndPointProbe |
   | **Protokolü** | TCP’yi seçin | TCP |
   | **Bağlantı noktası** | Kullanılmayan bağlantı noktası | 58888 |
   | **Interval**  | Sonda denemeleri arasındaki süre saniye cinsinden |5 |
   | **Sağlıksız durum eşiği** | Sanal bir makinenin sağlıksız olarak kabul edilememesi için oluşması gereken ardışık sonda hatalarının sayısı  | 2 |

1. Sistem durumu sondasını ayarlamak için **Tamam'ı** tıklatın.

1. Yük dengeleme kurallarını ayarlayın. **Yük dengeleme kurallarını**tıklatın ve **+Ekle'yi**tıklatın.

1. Küme çekirdeği IP adresi yük dengeleme kurallarını aşağıdaki gibi ayarlayın.

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Adı** | Metin | WSFCEndPoint |
   | **Ön uç IP adresi** | Adres seçin |WSFC IP adresini yapılandırırken oluşturduğunuz adresi kullanın. Bu, dinleyici IP adresinden farklıdır |
   | **Protokolü** | TCP’yi seçin |TCP |
   | **Bağlantı noktası** | Küme IP adresi için bağlantı noktasını kullanın. Bu, dinleyici sondası bağlantı noktası için kullanılmayan kullanılabilir bir bağlantı noktasıdır. | 58888 |
   | **Arka Uç Bağlantı Noktası** | Kayan IP doğrudan sunucu iadesi için ayarlandığında bu alan kullanılmaz | 58888 |
   | **Sonda** |Sonda için belirttiğiniz ad | WSFCEndPointProbe |
   | **Oturum Kalıcılığı** | Açılan liste | **Yok** |
   | **Boşta Kalma Süresi Zaman Aşımı** | TCP bağlantısını açık tutmak için dakikalar | 4 |
   | **Kayan IP (doğrudan sunucu iadesi)** | |Etkin |

   > [!WARNING]
   > Oluşturma sırasında doğrudan sunucu iadesi ayarlanır. Bu değer değiştirilemez.

1. Yük dengeleme kurallarını ayarlamak için **Tamam'ı** tıklatın.

## <a name="configure-the-listener"></a><a name="configure-listener"></a>Dinleyiciyi yapılandırma

Yapılacak bir sonraki şey, failover kümesiüzerinde bir Kullanılabilirlik Grubu dinleyici yapılandırmaktır.

> [!NOTE]
> Bu öğretici, tek bir ILB IP adresiyle tek bir dinleyicinin nasıl oluşturulacaklarını gösterir. Bir veya daha fazla IP adresini kullanarak bir veya daha fazla dinleyici oluşturmak için, [Bkz. Kullanılabilirlik Grubu dinleyicisi oluştur ve yük dengeleyicisi | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>Dinleyici bağlantı noktasını ayarla

SQL Server Management Studio'da dinleyici bağlantı noktasını ayarlayın.

1. SQL Server Management Studio'yu başlatın ve birincil yinelemeye bağlanın.

1. **AlwaysOn Yüksek Kullanılabilirlik Kullanılabilirlik** | **Grupları** | **Kullanılabilirlik Grubu Dinleyiciler**gidin.

1. Artık Failover Cluster Manager'da oluşturduğunuz dinleyici adını görmeniz gerekir. Dinleyici adını sağ tıklatın ve **Özellikler'i**tıklatın.

1. Bağlantı **Noktası** kutusunda, Kullanılabilirlik Grubu dinleyicisinin bağlantı noktası numarasını belirtin. 1433 varsayılandır, sonra **Tamam'ı**tıklatın.

Artık Kaynak Yöneticisi modunda çalışan Azure sanal makinelerde bir SQL Server Kullanılabilirlik Grubunuz var.

## <a name="test-connection-to-listener"></a>Dinleyiciye test bağlantısı

Bağlantıyı test etmek için:

1. RDP aynı sanal ağda olan bir SQL Server için, ancak çoğaltma sahibi değil. Kümedeki diğer SQL Server'ı kullanabilirsiniz.

1. Bağlantıyı test etmek için **sqlcmd** yardımcı programını kullanın. Örneğin, aşağıdaki komut dosyası, Windows kimlik doğrulaması ile dinleyici aracılığıyla birincil yinelemeiçin bir **sqlcmd** bağlantısı kurar:

   ```cmd
   sqlcmd -S <listenerName> -E
   ```

   Dinleyici varsayılan bağlantı noktası (1433) dışında bir bağlantı noktası kullanıyorsa, bağlantı dizesinde bağlantı noktasını belirtin. Örneğin, aşağıdaki sqlcmd komutu 1435 bağlantı noktasındaki bir dinleyiciye bağlanır:

   ```cmd
   sqlcmd -S <listenerName>,1435 -E
   ```

SQLCMD bağlantısı, birincil yinelemeyi barındıran SQL Server'ın hangi örneğine otomatik olarak bağlanır.

> [!TIP]
> Her iki SQL Server'ın güvenlik duvarında belirttiğiniz bağlantı noktasının açık olduğundan emin olun. Her iki sunucu da kullandığınız TCP bağlantı noktası için gelen bir kural gerektirir. Daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/cc753558.aspx)

## <a name="next-steps"></a>Sonraki adımlar

- [İkinci bir kullanılabilirlik grubu için yük dengeleyicisine IP adresi ekleyin.](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP)
