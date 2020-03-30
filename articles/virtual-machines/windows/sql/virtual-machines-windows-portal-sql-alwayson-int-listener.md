---
title: Kullanılabilirlik grubu dinleyicilerini & yük bakiyesini yapılandırın (Azure portalı)
description: Azure sanal makinelerde SQL Server için Her Zaman Kullanılabilirlik Grubu için dinleyici oluşturmak için adım adım yönergeler
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/16/2017
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: aefd7a55090da7f55404d6f551ab61268582ff5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79096332"
---
# <a name="configure-a-load-balancer-for-an-availability-group-on-azure-sql-server-vms"></a>Azure SQL Server VM'lerde kullanılabilirlik grubu için yük dengeleyicisini yapılandırma
Bu makalede, Azure Kaynak Yöneticisi ile çalışan Azure sanal makinelerde sql server her zaman kullanılabilirlik grubunda bir yük dengeleyicisi nasıl oluşturulacak açıklanmaktadır. Kullanılabilirlik grubu, SQL Server örnekleri Azure sanal makinelerdeyken bir yük dengeleyicisi gerektirir. Yük dengeleyicisi kullanılabilirlik grubu dinleyicisinin IP adresini depolar. Kullanılabilirlik grubu birden çok bölgeye yayılıyorsa, her bölgenin bir yük dengeleyicisi gerekir.

Bu görevi tamamlamak için, Kaynak Yöneticisi ile çalışan Azure sanal makinelerinde dağıtılan bir SQL Server kullanılabilirlik grubuna sahip olmanız gerekir. Her iki SQL Server sanal makinesi de aynı kullanılabilirlik kümesine ait olmalıdır. Kaynak Yöneticisi'nde kullanılabilirlik grubunu otomatik olarak oluşturmak için [Microsoft şablonunu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) kullanabilirsiniz. Bu şablon otomatik olarak sizin için bir dahili yük dengeleyici oluşturur. 

İsterseniz, [kullanılabilirlik grubunu el ile](virtual-machines-windows-portal-sql-availability-group-tutorial.md)yapılandırabilirsiniz.

Bu makalede, kullanılabilirlik gruplarınızın zaten yapılandırılmış olması gerekmektedir.  

İlgili konular şunlardır:

* [Azure VM'deki (GUI) kullanılabilirlik gruplarında her zaman yapılandır](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Azure Resource Manager ve PowerShell kullanarak bir Sanal Ağdan Sanal Ağa bağlantısı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Bu makalede gezinerek, Azure portalında bir yük dengeleyicisi oluşturur ve yapılandırırsınız. İşlem tamamlandıktan sonra, kullanılabilirlik grubu dinleyicisi için yük dengeleyicisinden IP adresini kullanacak şekilde kümeyi yapılandırAbilirsiniz.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Azure portalında yük bakiyesini oluşturma ve yapılandırma
Görevin bu bölümünde aşağıdakileri yapın:

1. Azure portalında yük dengeleyicisini oluşturun ve IP adresini yapılandırın.
2. Arka uç havuzunu yapılandırın.
3. Sondayı oluşturun. 
4. Yük dengeleme kurallarını ayarlayın.

> [!NOTE]
> SQL Server örnekleri birden çok kaynak grubunda ve bölgedeyse, her adımı her kaynak grubuna bir kez olmak üzere iki kez gerçekleştirin.
> 
> 

### <a name="step-1-create-the-load-balancer-and-configure-the-ip-address"></a>Adım 1: Yük dengeleyicisini oluşturun ve IP adresini yapılandırın
İlk olarak, yük dengeleyicioluşturun. 

1. Azure portalında, SQL Server sanal makinelerini içeren kaynak grubunu açın. 

2. Kaynak grubunda **Ekle'yi**tıklatın.

3. Yük **dengeleyicisini** arayın ve ardından arama sonuçlarında **Microsoft**tarafından yayınlanan **Yük Dengeleyicisini**seçin.

4. Yük **Dengeleyici** silik üzerinde **Oluştur'u**tıklatın.

5. Yük **bakiyesi oluştur** iletişim kutusunda, yük bakiyesini aşağıdaki gibi yapılandırın:

   | Ayar | Değer |
   | --- | --- |
   | **Adı** |Yük dengeleyicisini temsil eden bir metin adı. Örneğin, **sqlLB**. |
   | **Tür** |**Dahili**: Çoğu uygulama, aynı sanal ağ daki uygulamaların kullanılabilirlik grubuna bağlanmasını sağlayan bir dahili yük dengeleyicisi kullanır.  </br> **Harici**: Uygulamaların genel Internet bağlantısı üzerinden kullanılabilirlik grubuna bağlanmasını sağlar. |
   | **Sanal ağ** |SQL Server örneklerinin içinde olduğu sanal ağı seçin. |
   | **Alt ağ** |SQL Server örneklerinin içinde olduğu alt ağı seçin. |
   | **IP adresi ataması** |**Statik** |
   | **Özel IP adresi** |Alt ağdan kullanılabilir bir IP adresi belirtin. Kümede bir dinleyici oluştururken bu IP adresini kullanın. PowerShell komut dosyasında, bu makalenin ilerleyen `$ILBIP` saatlerinde, değişken için bu adresi kullanın. |
   | **Abonelik** |Birden çok aboneliğiniz varsa, bu alan görünebilir. Bu kaynakla ilişkilendirmek istediğiniz aboneliği seçin. Normalde kullanılabilirlik grubu için tüm kaynaklarla aynı aboneliktir. |
   | **Kaynak grubu** |SQL Server örneklerinin içinde olduğu kaynak grubunu seçin. |
   | **Konum** |SQL Server örneklerinin bulunduğu Azure konumunu seçin. |

6. **Oluştur'u**tıklatın. 

Azure yük dengeleyicisini oluşturur. Yük dengeleyicisi belirli bir ağa, alt ağa, kaynak grubuna ve konuma aittir. Azure görevi tamamladıktan sonra, Azure'daki yük dengeleyici ayarlarını doğrulayın. 

### <a name="step-2-configure-the-back-end-pool"></a>Adım 2: Arka uç havuzunu yapılandırın
Azure arka uç adresi havuzu *arka uç havuzunu*çağırır. Bu durumda, arka uç havuzu kullanılabilirlik grubunuzdaki iki SQL Server örneğinin adresleridir. 

1. Kaynak grubunuzda, oluşturduğunuz yük bakiyesini tıklatın. 

2. **Ayarlar'da** **Arka Uç havuzlarına**tıklayın.

3. **Arka uç havuzlarında,** arka uç adresi havuzu oluşturmak için **Ekle'yi** tıklatın. 

4. **Ad**altında **arka uç ekle havuzunda,** arka uç havuzu için bir ad yazın.

5. **Sanal makineler**altında, sanal **makine ekle'yi**tıklatın. 

6. **Sanal makineleri seç'in**altında, **kullanılabilirlik kümesini seçin'i**tıklatın ve ardından SQL Server sanal makinelerinin ait olduğu kullanılabilirlik kümesini belirtin.

7. Kullanılabilirlik kümesini seçtikten sonra, **sanal makineleri seçin'** i tıklatın, kullanılabilirlik grubunda SQL Server örneklerini barındıran iki sanal makineyi seçin ve sonra **Seç'i**tıklatın. 

8. **Sanal makineleri seç**ve arka uç havuzu **ekle**bıçakları kapatmak için **Tamam'ı** tıklatın. 

Azure, arka uç adres havuzunun ayarlarını güncelleştirir. Artık kullanılabilirlik kümenizde iki SQL Server örneği havuzu bulunuyor.

### <a name="step-3-create-a-probe"></a>Adım 3: Sonda oluşturma
Sonda, Azure'un hangi SQL Server örneğinin şu anda kullanılabilirlik grubu dinleyicisine sahip olduğunu nasıl doğruladığına bağlıdır. Azure, sondayı oluştururken tanımladığınız bir bağlantı noktasındaki IP adresini temel alan hizmeti inceler.

1. Yük dengeleyici **Ayarları** bıçağında, **Sağlık sondalarını**tıklatın. 

2. Sağlık **sondaları** bıçağında **Ekle'yi**tıklatın.

3. **Sonda** ekle bıçağındaki sondayı yapılandırın. Sondayı yapılandırmak için aşağıdaki değerleri kullanın:

   | Ayar | Değer |
   | --- | --- |
   | **Adı** |Sondayı temsil eden bir metin adı. Örneğin, **SQLAlwaysOnEndPointProbe**. |
   | **Protokolü** |**TCP** |
   | **Bağlantı noktası** |Kullanılabilir herhangi bir bağlantı noktasını kullanabilirsiniz. Örneğin, *59999*. |
   | **Interval** |*5* |
   | **Sağlıksız durum eşiği** |*2* |

4.  **Tamam**'a tıklayın. 

> [!NOTE]
> Belirttiğiniz bağlantı noktasının her iki SQL Server örneğinin güvenlik duvarında açık olduğundan emin olun. Her iki örnek de kullandığınız TCP bağlantı noktası için gelen bir kural gerektirir. Daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/cc753558.aspx) 
> 
> 

Azure sondayı oluşturur ve ardından kullanılabilirlik grubu için hangi SQL Server örneğinin dinleyiciye sahip olduğunu test etmek için kullanır.

### <a name="step-4-set-the-load-balancing-rules"></a>Adım 4: Yük dengeleme kurallarını ayarlama
Yük dengeleme kuralları, yük bakiyesi trafiğini SQL Server örneklerine nasıl yönlendirir. Bu yük bakiyesi için, iki SQL Server örneğinden yalnızca biri aynı anda kullanılabilirlik grubu dinleyici kaynağına sahip olduğundan, doğrudan sunucu iadesini etkinleştirin.

1. Yük dengeleyici **Ayarları** **bıçağında, Yük dengeleme kurallarını**tıklatın. 

2. Yük **dengeleme kuralları** bıçağında **Ekle'yi**tıklatın.

3. Yük **dengeleme kuralı ekle** kuralında, yük dengeleme kuralını yapılandırın. Aşağıdaki ayarları kullanın: 

   | Ayar | Değer |
   | --- | --- |
   | **Adı** |Yük dengeleme kurallarını temsil eden bir metin adı. Örneğin, **SQLAlwaysOnEndPointListener**. |
   | **Protokolü** |**TCP** |
   | **Bağlantı noktası** |*1433* |
   | **Arka Uç Bağlantı Noktası** |*1433*. Bu kural **Kayan IP (doğrudan sunucu iadesi)** kullandığından bu değer yoksayılır. |
   | **Sonda** |Bu yük dengeleyicisi için oluşturduğunuz sondanın adını kullanın. |
   | **Oturum kalıcılığı** |**Yok** |
   | **Boşta zaman/zaman ası (dakika)** |*4* |
   | **Kayan IP (doğrudan sunucu iadesi)** |**Etkin** |

   > [!NOTE]
   > Tüm ayarları görüntülemek için bıçağı aşağı kaydırmanız gerekebilir.
   > 

4. **Tamam**'a tıklayın. 
5. Azure yük dengeleme kuralını yapılandırır. Şimdi yük bakiyesi, trafiği kullanılabilirlik grubu için dinleyiciyi barındıran SQL Server örneğine yönlendirecek şekilde yapılandırıldı. 

Bu noktada, kaynak grubunun her iki SQL Server makinesine de bağlanan bir yük dengeleyicisi vardır. Yük bakiyeleyicisi ayrıca SQL Server Always On availability group dinleyicisi için bir IP adresi de içerir, böylece her iki makine de kullanılabilirlik gruplarına yönelik isteklere yanıt verebilir.

> [!NOTE]
> SQL Server örnekleriniz iki ayrı bölgedeyse, diğer bölgedeki adımları yineleyin. Her bölge bir yük dengeleyici gerektirir. 
> 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Kümeyi yük dengeleyicisi IP adresini kullanacak şekilde yapılandırın
Bir sonraki adım, dinleyiciyi kümeüzerinde yapılandırmak ve dinleyiciyi çevrimiçi duruma getirmektir. Şunları yapın: 

1. Failover kümesinde kullanılabilirlik grubu dinleyicisini oluşturun. 

2. Dinleyiciyi çevrimiçi duruma getirin.

### <a name="step-5-create-the-availability-group-listener-on-the-failover-cluster"></a>Adım 5: failover kümesinde kullanılabilirlik grubu dinleyicisini oluşturma
Bu adımda, Failover Cluster Manager ve SQL Server Management Studio'da kullanılabilirlik grubu dinleyicisini el ile oluşturursunuz.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-the-configuration-of-the-listener"></a>Dinleyicinin yapılandırmasını doğrulama

Küme kaynakları ve bağımlılıkları doğru yapılandırılmışsa, dinleyiciyi SQL Server Management Studio'da görüntüleyebilmelisiniz. Dinleyici bağlantı noktasını ayarlamak için aşağıdakileri yapın:

1. SQL Server Management Studio'yu başlatın ve ardından birincil yinelemeye bağlanın.

2. **AlwaysOn Yüksek Kullanılabilirlik** > **Grupları** > **Kullanılabilirlik Grubu Dinleyiciler**gidin.  
    Artık Failover Cluster Manager'da oluşturduğunuz dinleyici adını görmeniz gerekir. 

3. Dinleyici adını sağ tıklatın ve ardından **Özellikler'i**tıklatın.

4. Bağlantı **Noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarasını belirtin (varsayılan 1433'tü) ve ardından **Tamam'ı**tıklatın.

Artık Kaynak Yöneticisi modunda çalışan Azure sanal makinelerinde kullanılabilirlik grubunuz var. 

## <a name="test-the-connection-to-the-listener"></a>Dinleyiciye olan bağlantıyı test etme
Aşağıdakileri yaparak bağlantıyı test edin:

1. RDP aynı sanal ağda olan bir SQL Server örneğine, ancak çoğaltma sahibi değil. Bu sunucu kümedeki diğer SQL Server örneği olabilir.

2. Bağlantıyı test etmek için **sqlcmd** yardımcı programını kullanın. Örneğin, aşağıdaki komut dosyası, Windows kimlik doğrulaması ile dinleyici aracılığıyla birincil yinelemeiçin bir **sqlcmd** bağlantısı kurar:
   
        sqlcmd -S <listenerName> -E

SQLCMD bağlantısı otomatik olarak birincil yinelemeyi barındıran SQL Server örneğine bağlanır. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Ek bir kullanılabilirlik grubu için IP adresi oluşturma

Her kullanılabilirlik grubu ayrı bir dinleyici kullanır. Her dinleyicinin kendi IP adresi vardır. Ek dinleyiciler için IP adresini tutmak için aynı yük dengeleyicisini kullanın. Bir kullanılabilirlik grubu oluşturduktan sonra, IP adresini yük bakiyesine ekleyin ve ardından dinleyiciyi yapılandırın.

Azure portalı ile bir yük dengeleyicisine IP adresi eklemek için aşağıdakileri yapın:

1. Azure portalında, yük bakiyesini içeren kaynak grubunu açın ve ardından yük bakiyesini tıklatın. 

2. **AYARLAR'ın**altında **Frontend IP havuzuna**tıklayın ve sonra **Ekle'yi**tıklatın. 

3. **Frontend IP adresi ekle**altında, ön uç için bir ad atayın. 

4. **Sanal ağ** ve **Alt ağ** ın SQL Server örnekleriyle aynı olduğunu doğrulayın.

5. Dinleyicinin IP adresini ayarlayın. 
   
   >[!TIP]
   >IP adresini statik olarak ayarlayabilir ve alt ağda şu anda kullanılmayan bir adres yazabilirsiniz. Alternatif olarak, IP adresini dinamik olarak ayarlayabilir ve yeni ön uç IP havuzunu kaydedebilirsiniz. Bunu yaptığınızda, Azure portalı havuza otomatik olarak kullanılabilir bir IP adresi atar. Daha sonra ön uç IP havuzunu yeniden açabilir ve atamayı statik olarak değiştirebilirsiniz. 

6. IP adresini dinleyiciye kaydedin. 

7. Aşağıdaki ayarları kullanarak bir sistem durumu sondası ekleyin:

   |Ayar |Değer
   |:-----|:----
   |**Adı** |Sondayı tanımlayabilmek için bir isim.
   |**Protokolü** |TCP
   |**Bağlantı noktası** |Tüm sanal makinelerde bulunması gereken kullanılmayan bir TCP bağlantı noktası. Başka bir amaç için kullanılamaz. Hiçbir dinleyici aynı sonda bağlantı noktasını kullanamaz. 
   |**Interval** |Sonda girişimleri arasındaki süre. Varsayılanı kullanın (5).
   |**Sağlıksız durum eşiği** |Sanal bir makineden önce başarısız olması gereken ardışık eşik sayısı sağlıksız olarak kabul edilir.

8. Sondayı kaydetmek için **Tamam'ı** tıklatın. 

9. Yük dengeleme kuralı oluşturun. **Yük dengeleme kurallarını**tıklatın ve sonra **Ekle'yi**tıklatın.

10. Yeni yük dengeleme kuralını aşağıdaki ayarları kullanarak yapılandırın:

    |Ayar |Değer
    |:-----|:----
    |**Adı** |Yük dengeleme kuralını tanımlamak için bir ad. 
    |**Ön uç IP adresi** |Oluşturduğunuz IP adresini seçin. 
    |**Protokolü** |TCP
    |**Bağlantı noktası** |SQL Server örneklerinin kullandığı bağlantı noktasını kullanın. Varsayılan bir örnek, siz değiştirmediğiniz sürece 1433 bağlantı noktasını kullanır. 
    |**Arka uç bağlantı noktası** |**Bağlantı Noktası**ile aynı değeri kullanın.
    |**Arka uç havuzu** |SQL Server örnekleri ile sanal makineleri içeren havuz. 
    |**Sağlık sondası** |Oluşturduğunuz sondayı seçin.
    |**Oturum kalıcılığı** |None
    |**Boşta zaman/zaman ası (dakika)** |Varsayılan (4)
    |**Kayan IP (doğrudan sunucu iadesi)** | Etkin

### <a name="configure-the-availability-group-to-use-the-new-ip-address"></a>Kullanılabilirlik grubunu yeni IP adresini kullanacak şekilde yapılandırın

Kümeyapılandırmayı bitirmek için, ilk kullanılabilirlik grubunu yaptığınızda izlediğiniz adımları yineleyin. Diğer bir diğer olarak, [kümeyi yeni IP adresini kullanacak şekilde yapılandırın.](#configure-the-cluster-to-use-the-load-balancer-ip-address) 

Dinleyici için bir IP adresi ekledikten sonra, aşağıdakileri yaparak ek kullanılabilirlik grubunu yapılandırın: 

1. Yeni IP adresiiçin sonda bağlantı noktasının her iki SQL Server sanal makinede de açık olduğunu doğrulayın. 

2. [Cluster Manager'da istemci erişim noktasını ekleyin.](#addcap)

3. [Kullanılabilirlik grubu için IP kaynağını yapılandırın.](#congroup)

   >[!IMPORTANT]
   >IP adresini oluştururken, yük dengeleyicisine eklediğiniz IP adresini kullanın.  

4. [SQL Server kullanılabilirlik grubu kaynağını istemci erişim noktasına bağımlı hale getirin.](#dependencyGroup)

5. [İstemci erişim noktası kaynağını IP adresine bağımlı hale getirin.](#listname)
 
6. [PowerShell'de küme parametrelerini ayarlayın.](#setparam)

Kullanılabilirlik grubunu yeni IP adresini kullanacak şekilde yapılandırdıktan sonra, bağlantıyı dinleyiciye yapılandırın. 

## <a name="add-load-balancing-rule-for-distributed-availability-group"></a>Dağıtılmış kullanılabilirlik grubu için yük dengeleme kuralı ekleme

Kullanılabilirlik grubu dağıtılmış bir kullanılabilirlik grubuna katılırsa, yük dengeleyicisinin ek bir kurala ihtiyacı vardır. Bu kural, dağıtılmış kullanılabilirlik grubu dinleyicisi tarafından kullanılan bağlantı noktasını depolar.

>[!IMPORTANT]
>Bu adım yalnızca kullanılabilirlik grubu [dağıtılmış kullanılabilirlik grubuna](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups)katıldığında geçerlidir. 

1. Dağıtılmış kullanılabilirlik grubuna katılan her sunucuda, dağıtılmış kullanılabilirlik grubu dinleyicisi TCP bağlantı noktası üzerinde gelen bir kural oluşturun. Birçok örnekte, dokümantasyon 5022 kullanır. 

1. Azure portalında, yük bakiyesini tıklatın ve **Yük dengeleme kurallarını**tıklatın ve sonra **+Ekle'yi**tıklatın. 

1. Aşağıdaki ayarlarla yük dengeleme kuralını oluşturun:

   |Ayar |Değer
   |:-----|:----
   |**Adı** |Dağıtılmış kullanılabilirlik grubu için yük dengeleme kuralını tanımlamak için bir ad. 
   |**Ön uç IP adresi** |Kullanılabilirlik grubuyla aynı frontend IP adresini kullanın.
   |**Protokolü** |TCP
   |**Bağlantı noktası** |5022 - [Dağıtılmış kullanılabilirlik grubu uç nokta dinleyicisi](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups)için bağlantı noktası.</br> Herhangi bir kullanılabilir bağlantı noktası olabilir.  
   |**Arka uç bağlantı noktası** | 5022 - **Port**ile aynı değeri kullanın.
   |**Arka uç havuzu** |SQL Server örnekleri ile sanal makineleri içeren havuz. 
   |**Sağlık sondası** |Oluşturduğunuz sondayı seçin.
   |**Oturum kalıcılığı** |None
   |**Boşta zaman/zaman ası (dakika)** |Varsayılan (4)
   |**Kayan IP (doğrudan sunucu iadesi)** | Etkin

Dağıtılmış kullanılabilirlik gruplarına katılan diğer kullanılabilirlik gruplarındaki yük dengeleyicisi için bu adımları yineleyin.

Bir Azure Ağ Güvenlik Grubu ile erişimi kısıtlıyorsanız, izin kurallarının arka uç SQL Server VM IP adreslerini ve varsa AG dinleyicisi ve küme çekirdeği IP adresi için yük bakiyesi kayan IP adreslerini içerdiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [Farklı bölgelerdeki Azure sanal makinelerinde sql server'ı her zaman kullanılabilirlik grubunda yapılandırma](virtual-machines-windows-portal-sql-availability-group-dr.md)
