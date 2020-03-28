---
title: Azure'daki RHEL sanal makinelerde SQL Server için kullanılabilirlik grubu dinleyicisini yapılandırma - Linux Sanal Makineler | Microsoft Dokümanlar
description: Azure'da RHEL sanal makinelerde SQL Server'da kullanılabilirlik grubu dinleyicisi kurma hakkında bilgi edinin
ms.service: virtual-machines-linux
ms.subservice: ''
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: jroth
ms.date: 03/11/2020
ms.openlocfilehash: 80557eb3776ba17a4922d1fc384b87419ffbd67e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79096581"
---
# <a name="tutorial-configure-availability-group-listener-for-sql-server-on-rhel-virtual-machines-in-azure"></a>Öğretici: Azure'daki RHEL sanal makinelerde SQL Server için kullanılabilirlik grubu dinleyicisini yapılandırma

> [!NOTE]
> Sunulan öğretici **genel önizlemededir.** 
>
> Bu eğitimde RHEL 7.6 ile SQL Server 2017'yi kullanıyoruz, ancak HA'yı yapılandırmak için RHEL 7 veya RHEL 8'de SQL Server 2019'u kullanmak mümkündür. Kullanılabilirlik grubu kaynaklarını yapılandırma komutları RHEL 8'de değişti ve makaleye bakmak, [kullanılabilirlik grubu kaynağı oluşturma](/sql/linux/sql-server-linux-availability-group-cluster-rhel#create-availability-group-resource) ve doğru komutlar hakkında daha fazla bilgi için RHEL 8 kaynakları oluşturmak isteyeceksiniz.

Bu öğretici, Azure'daki RHEL sanal makinelerde SQL Sunucularınız için kullanılabilirlik grubu dinleyicisi oluşturma adımlarının üzerinden geçecektir. Şunları öğrenirsiniz:

> [!div class="checklist"]
> - Azure portalında yük dengeleyicisi oluşturma
> - Yük dengeleyicisi için arka uç havuzunu yapılandırın
> - Yük dengeleyicisi için bir sonda oluşturma
> - Yük dengeleme kurallarını ayarlama
> - Kümedeki yük dengeleyici kaynağını oluşturma
> - Kullanılabilirlik grubu dinleyicisini oluşturma
> - Dinleyiciye bağlanmayı test etme
> - Bir başarısızı test etme

## <a name="prerequisite"></a>Önkoşul

Tamamlanmış [ **Öğretici: Azure'daki RHEL sanal makinelerde SQL Server için kullanılabilirlik gruplarını yapılandırma**](sql-server-linux-rhel-ha-stonith-tutorial.md)

## <a name="create-the-load-balancer-in-the-azure-portal"></a>Azure portalında yük bakiyesini oluşturma

Aşağıdaki yönergeler, Oluştur'dan 1'den 4'e kadar olan adımlara kadar götürür ve [Yük bakiyesi - Azure portalı](../../../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md) [makalesinin Azure portalı bölümündeki yük dengeleyicisini yapılandırın.](../../../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md#create-and-configure-the-load-balancer-in-the-azure-portal)

### <a name="create-the-load-balancer"></a>Yük dengeleyiciyi oluşturma

1. Azure portalında, SQL Server sanal makinelerini içeren kaynak grubunu açın. 

2. Kaynak grubunda **Ekle'yi**tıklatın.

3. Yük **dengeleyicisini** arayın ve ardından arama sonuçlarında **Microsoft**tarafından yayınlanan **Yük Dengeleyicisini**seçin.

4. Yük **Dengeleyici** silik üzerinde **Oluştur'u**tıklatın.

5. Yük **bakiyesi oluştur** iletişim kutusunda, yük bakiyesini aşağıdaki gibi yapılandırın:

   | Ayar | Değer |
   | --- | --- |
   | **Adı** |Yük dengeleyicisini temsil eden bir metin adı. Örneğin, **sqlLB**. |
   | **Tür** |**Iç** |
   | **Sanal ağ** |Oluşturulan varsayılan VNet **VM1VNET**olarak adlandırılmalıdır. |
   | **Alt ağ** |SQL Server örneklerinin içinde olduğu alt ağı seçin. Varsayılan **VM1Subnet**olmalıdır.|
   | **IP adresi ataması** |**Statik** |
   | **Özel IP adresi** |Kümede `virtualip` oluşturulan IP adresini kullanın. |
   | **Abonelik** |Kaynak grubunuz için kullanılan aboneliği kullanın. |
   | **Kaynak grubu** |SQL Server örneklerinin içinde olduğu kaynak grubunu seçin. |
   | **Konum** |SQL Server örneklerinin bulunduğu Azure konumunu seçin. |

### <a name="configure-the-back-end-pool"></a>Arka uç havuzunu yapılandırma
Azure arka uç adresi havuzu *arka uç havuzunu*çağırır. Bu durumda, arka uç havuzu kullanılabilirlik grubunuzdaki üç SQL Server örneğinin adresleridir. 

1. Kaynak grubunuzda, oluşturduğunuz yük bakiyesini tıklatın. 

2. **Ayarlar'da** **Arka Uç havuzlarına**tıklayın.

3. **Arka uç havuzlarında,** arka uç adresi havuzu oluşturmak için **Ekle'yi** tıklatın. 

4. **Ad**altında **arka uç ekle havuzunda,** arka uç havuzu için bir ad yazın.

5. **Associated altında**, **Sanal makine**seçin. 

6. Ortamdaki her sanal makineyi seçin ve her seçimle uygun IP adresini ilişkilendirin.

    :::image type="content" source="media/sql-server-linux-rhel-ha-listener-tutorial/add-backend-pool.png" alt-text="Arka uç havuzu ekleme":::

7. **Ekle**’ye tıklayın. 

### <a name="create-a-probe"></a>Sonda oluşturma

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

5. Tüm sanal makinelerinize giriş yapın ve aşağıdaki komutları kullanarak sonda bağlantı noktasını açın:

    ```bash
    sudo firewall-cmd --zone=public --add-port=59999/tcp --permanent
    sudo firewall-cmd --reload
    ```

Azure sondayı oluşturur ve ardından kullanılabilirlik grubu için hangi SQL Server örneğinin dinleyiciye sahip olduğunu test etmek için kullanır.

### <a name="set-the-load-balancing-rules"></a>Yük dengeleme kurallarını ayarlama

Yük dengeleme kuralları, yük bakiyesi trafiğini SQL Server örneklerine nasıl yönlendirir. Bu yük dengeleyicisi için, üç SQL Server örneğinden yalnızca biri aynı anda kullanılabilirlik grubu dinleyici kaynağına sahip olduğundan, doğrudan sunucu iadesini etkinleştirin.

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

   :::image type="content" source="media/sql-server-linux-rhel-ha-listener-tutorial/add-load-balancing-rule.png" alt-text="Yük dengeleme kuralı ekleme":::

4. **Tamam**'a tıklayın. 
5. Azure yük dengeleme kuralını yapılandırır. Şimdi yük bakiyesi, trafiği kullanılabilirlik grubu için dinleyiciyi barındıran SQL Server örneğine yönlendirecek şekilde yapılandırıldı. 

Bu noktada, kaynak grubunun tüm SQL Server makinelerine bağlanan bir yük dengeleyicisi vardır. Yük dengeleyicisi ayrıca, herhangi bir makinenin kullanılabilirlik grupları isteklerine yanıt verebilmeleri için SQL Server Always On availability group dinleyicisinin IP adresini de içerir.

## <a name="create-the-load-balancer-resource-in-the-cluster"></a>Kümedeki yük dengeleyici kaynağını oluşturma

1. Birincil sanal makineye giriş yapın. Azure yük dengeleyici sondası bağlantı noktasını etkinleştirmek için kaynağı oluşturmamız gerekir (örneğimizde 59999 kullanılır). Şu komutu çalıştırın:

    ```bash
    sudo pcs resource create azure_load_balancer azure-lb port=59999
    ```

1. Kaynağı `virtualip` ve `azure_load_balancer` kaynağı içeren bir grup oluşturun:

    ```bash
    sudo pcs resource group add virtualip_group azure_load_balancer virtualip
    ```

### <a name="add-constraints"></a>Kısıtlama ekleme

1. Azure yük bakiyesi IP adresi ve AG kaynağının aynı düğümüzerinde çalıştığından emin olmak için bir birlikte konum kısıtlaması yapılandırılmalıdır. Şu komutu çalıştırın:

    ```bash
    sudo pcs constraint colocation add azure_load_balancer ag_cluster-master INFINITY with-rsc-role=Master
    ```
1. Azure yük bakiyesi IP adresinden önce AG kaynağının çalışır durumda olduğundan emin olmak için bir sıralama kısıtlaması oluşturun. Birlikte konum kısıtlaması bir sıralama kısıtlaması anlamına gelirken, bu onu zorlar.

    ```bash
    sudo pcs constraint order promote ag_cluster-master then start azure_load_balancer
    ```

1. Kısıtlamaları doğrulamak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo pcs constraint list --full
    ```

    Aşağıdaki çıktıyı görmeniz gerekir:

    ```output
    Location Constraints:
    Ordering Constraints:
      promote ag_cluster-master then start virtualip (kind:Mandatory) (id:order-ag_cluster-master-virtualip-mandatory)
      promote ag_cluster-master then start azure_load_balancer (kind:Mandatory) (id:order-ag_cluster-master-azure_load_balancer-mandatory)
    Colocation Constraints:
      virtualip with ag_cluster-master (score:INFINITY) (with-rsc-role:Master) (id:colocation-virtualip-ag_cluster-master-INFINITY)
      azure_load_balancer with ag_cluster-master (score:INFINITY) (with-rsc-role:Master) (id:colocation-azure_load_balancer-ag_cluster-master-INFINITY)
    Ticket Constraints:
    ```

## <a name="create-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisini oluşturma

1. Birincil düğümde, SQLCMD veya SSMS'te aşağıdaki komutu çalıştırın:

    - Aşağıda kullanılan IP adresini `virtualip` IP adresiyle değiştirin.

    ```sql
    ALTER AVAILABILITY
    GROUP [ag1] ADD LISTENER 'ag1-listener' (
            WITH IP(('10.0.0.7'    ,'255.255.255.0'))
                ,PORT = 1433
            );
    GO
    ```

1. Her VM düğümüne giriş yapın. Ana bilgisayar dosyasını açmak ve her makinedeki ana `ag1-listener` bilgisayar ad çözünürlüğünü ayarlamak için aşağıdaki komutu kullanın.

    ```
    sudo vi /etc/hosts
    ```

    **vi** düzenleyicisinde, `i` metin eklemek için girin ve boş bir satıra `ag1-listener`IP'yi ekleyin. Sonra `ag1-listener` IP yanında bir boşluk sonra ekleyin.

    ```output
    <IP of ag1-listener> ag1-listener
    ```

    **Vi** düzenleyiciden çıkmak için önce **Esc** tuşuna basın `:wq` ve sonra dosyayı yazmak için komutu girin ve çıkın. Bunu her düğümde yapın.

## <a name="test-the-listener-and-a-failover"></a>Dinleyiciyi ve başarısızı test edin

### <a name="test-logging-into-sql-server-using-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisini kullanarak SQL Server'a giriş testi

1. Kullanılabilirlik grubu dinleyici adını kullanarak SQL Server'ın birincil düğümüne giriş yapmak için SQLCMD'yi kullanın:

    - Daha önce oluşturulmuş bir giriş kullanın `<YourPassword>` ve doğru parolayla değiştirin. Aşağıdaki örnekte `sa` SQL Server ile oluşturulan giriş kullanır.

    ```bash
    sqlcmd -S ag1-listener -U sa -P <YourPassword>
    ```

1. Bağlı olduğunuz sunucunun adını denetleyin. SQLCMD'de aşağıdaki komutu çalıştırın:

    ```sql
    SELECT @@SERVERNAME
    ```

    Çıktınız geçerli birincil düğümü göstermelidir. Eğer bir `VM1` failover test hiç bu olmalıdır.

    Komutu yazarak SQL `exit` oturumundan çıkın.

### <a name="test-a-failover"></a>Bir başarısızı test edin

1. Birincil yinelemeveya başka bir yineleme üzerinde `<VM2>` el ile başarısız olmak için aşağıdaki komutu çalıştırın. Sunucu `<VM2>` adınızın değeriyle değiştirin.

    ```bash
    sudo pcs resource move ag_cluster-master <VM2> --master
    ```

1. Kısıtlamalarınızı denetlerseniz, el ile başarısız olması nedeniyle başka bir kısıtlamanın eklandığını görürsünüz:

    ```bash
    sudo pcs constraint list --full
    ```

    Kimlikle `cli-prefer-ag_cluster-master` ilgili bir kısıtlama eklandığını göreceksiniz.

1. Aşağıdaki komutu `cli-prefer-ag_cluster-master` kullanarak KIMLIĞI ile kısıtlamayı kaldırın:

    ```bash
    sudo pcs constraint remove cli-prefer-ag_cluster-master
    ```

1. Küme kaynaklarınızı komutu `sudo pcs resource`kullanarak denetleyin ve birincil örneğin `<VM2>`şimdi olduğunu görmeniz gerekir.

    ```output
    [<username>@<VM1> ~]$ sudo pcs resource
    Master/Slave Set: ag_cluster-master [ag_cluster]
        Masters: [ <VM2> ]
        Slaves: [ <VM1> <VM3> ]
    Resource Group: virtualip_group
        azure_load_balancer        (ocf::heartbeat:azure-lb):      Started <VM2>
        virtualip  (ocf::heartbeat:IPaddr2):       Started <VM2>
    ```

1. Dinleyici adını kullanarak birincil yinelemeoturum adabınıza giriş yapmak için SQLCMD'yi kullanın:

    - Daha önce oluşturulmuş bir giriş kullanın `<YourPassword>` ve doğru parolayla değiştirin. Aşağıdaki örnekte `sa` SQL Server ile oluşturulan giriş kullanır.

    ```bash
    sqlcmd -S ag1-listener -U sa -P <YourPassword>
    ```

1. Bağlı olduğunuz sunucuyu denetleyin. SQLCMD'de aşağıdaki komutu çalıştırın:

    ```sql
    SELECT @@SERVERNAME
    ```

    Artık başarısız olduğunuz VM'ye bağlı olduğunuzu görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure'daki Yük bakiyeleri hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Azure SQL Server VM'lerde kullanılabilirlik grubu için yük dengeleyicisini yapılandırma](../../../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md)
