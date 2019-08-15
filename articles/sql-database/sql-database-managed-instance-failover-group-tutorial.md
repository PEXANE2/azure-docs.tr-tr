---
title: 'Öğretici: Yük devretme grubuna SQL veritabanı yönetilen örneği ekleme'
description: Azure SQL veritabanı yönetilen örneğiniz için bir yük devretme grubu yapılandırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sashan, carlrab
manager: jroth
ms.date: 06/27/2019
ms.openlocfilehash: 5bf2ee68ff2cfde2846dff2d94f7478920a18760
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69037118"
---
# <a name="tutorial-add-a-sql-database-managed-instance-to-a-failover-group"></a>Öğretici: Yük devretme grubuna SQL veritabanı yönetilen örneği ekleme

Bir yük devretme grubuna SQL veritabanı yönetilen örneği ekleyin. Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> - Birincil yönetilen örnek oluşturma
> - Bir [Yük devretme grubunun](sql-database-auto-failover-group.md)parçası olarak ikincil bir yönetilen örnek oluşturun. 
> - Test yük devretmesi

  > [!NOTE]
  > Yönetilen bir örnek oluşturmak, önemli miktarda zaman alabilir. Sonuç olarak, Bu öğreticinin tamamlanması birkaç saat sürebilir. Sağlama süreleri hakkında daha fazla bilgi için bkz. [yönetilen örnek yönetimi işlemleri](sql-database-managed-instance.md#managed-instance-management-operations). Yönetilen örneklerle yük devretme gruplarının kullanılması şu anda önizlemededir. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun: 

- Bir Azure aboneliği, henüz yoksa [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) . 


## <a name="1----create-resource-group-and-primary-managed-instance"></a>1-kaynak grubu ve birincil yönetilen örnek oluşturma
Bu adımda, Azure portal kullanarak yük devretme grubunuz için kaynak grubunu ve birincil yönetilen örneği oluşturacaksınız. 

1. [Azure portal](https://portal.azure.com) oturum açın. 
1. Azure portal sol üst köşesinde **kaynak oluşturmayı** seçin. 
1. Arama `managed instance` kutusuna yazın ve Azure SQL yönetilen örneği seçeneğini belirleyin. 
1. **SQL yönetilen örnek** oluşturma sayfasını başlatmak için **Oluştur** ' u seçin. 
1. **Azure SQL veritabanı yönetilen örneği oluştur** sayfasında, **temel bilgiler** sekmesinde
    1. **Proje ayrıntıları**' nın altında, açılır listeden **aboneliğinizi** seçin ve ardından yeni kaynak grubu **oluşturmayı** seçin. Kaynak grubunuz `myResourceGroup`için gibi bir ad yazın. 
    1. **Yönetilen örnek ayrıntıları**' nın altında, yönetilen örneğinizin adını ve yönetilen örneğinizi dağıtmak istediğiniz bölgeyi girin. [Eşleştirilmiş bölge](/azure/best-practices-availability-paired-regions)içeren bir bölge seçtiğinizden emin olun. **İşlem + depolama alanını** varsayılan değerlerle bırakın. 
    1. **Yönetici hesabı**altında, gibi bir yönetici oturum açma `azureuser`ve karmaşık yönetici parolası sağlayın. 

    ![Birincil mı oluştur](media/sql-database-managed-instance-failover-group-tutorial/primary-sql-mi-values.png)

1. Ayarları varsayılan değerlerinde bırakın ve yönetilen örnek ayarlarınızı gözden geçirmek için **gözden geçir + oluştur** ' u seçin. 
1. Birincil yönetilen örneğinizi oluşturmak için **Oluştur** ' u seçin. 


## <a name="2---create-a-virtual-network"></a>2-sanal ağ oluşturma
Bu adımda, ikincil yönetilen örnek için bir sanal ağ oluşturacaksınız. Birincil ve ikincil yönetilen örneklerin alt ağının çakışmayan adres aralıkları olduğundan, bu adım gereklidir. 

Birincil sanal ağınızın alt ağ aralığını doğrulamak için şu adımları izleyin:
1. [Azure Portal](https://portal.azure.com), kaynak grubunuza gidin ve birincil örneğiniz için sanal ağı seçin. 
1. **Ayarlar** altında **alt ağlar** ' ı seçin ve **adres aralığını**aklınızda edin. İkincil yönetilen örnek için sanal ağın alt ağ adres aralığı bu ile çakışamaz. 


   ![Birincil alt ağ](media/sql-database-managed-instance-failover-group-tutorial/verify-primary-subnet-range.png)

Bir sanal ağ oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com), **kaynak oluştur** ve *sanal ağ*ara ' yı seçin. 
1. Microsoft tarafından yayımlanan **sanal ağ** seçeneğini seçin ve ardından sonraki sayfada **Oluştur** ' u seçin. 
1. İkincil yönetilen örneğinizin sanal ağını yapılandırmak için gerekli alanları doldurun ve **Oluştur**' u seçin. 

   Aşağıdaki tabloda, ikincil sanal ağ için gereken değerler gösterilmektedir:

    | **Alan** | Value |
    | --- | --- |
    | **Name** |  İkincil yönetilen örnek tarafından kullanılacak sanal ağın adı (gibi `vnet-sql-mi-secondary`). |
    | **Adres alanı** | Sanal ağınız `10.128.0.0/16`için gibi adres alanı. | 
    | **Abonelik** | Birincil yönetilen örneğinizin ve kaynak grubunuzun bulunduğu abonelik. |
    | **Bölge** | İkincil yönetilen örneğinizi dağıtacağınız konum; Bu, birincil yönetilen örneğe [eşlenmiş bir bölgede](/azure/best-practices-availability-paired-regions) olmalıdır.  |
    | **Alt ağ** | Alt ağınızın adı. `default`Varsayılan olarak sizin için sağlanır. |
    | **Adres aralığı**| Alt ağınızın adres aralığı. Bunun gibi, `10.128.0.0/24`birincil yönetilen örneğinizin sanal ağı tarafından kullanılan alt ağ adres aralığından farklı olması gerekir.  |
    | &nbsp; | &nbsp; |

    ![İkincil sanal ağ değerleri](media/sql-database-managed-instance-failover-group-tutorial/secondary-virtual-network.png)


## <a name="3---create-a-secondary-managed-instance"></a>3-ikincil bir yönetilen örnek oluşturma
Bu adımda, Azure portal bir ikincil yönetilen örnek oluşturacaksınız ve bu, iki yönetilen örnek arasında ağ iletişimini de yapılandıracaksınız. 

İkinci yönetilen örneğiniz şunları sağlamalıdır:
- Boş olmalıdır. 
- Birincil yönetilen örneğine karşılık gelen bir [eşleştirilmiş bölge](/azure/best-practices-availability-paired-regions) içinde yer alır. 
- Birincil yönetilen örnekten farklı bir alt ağa ve IP aralığına sahip olmak. 

İkincil yönetilen örneğinizi oluşturmak için aşağıdaki adımları izleyin: 

1. [Azure Portal](http://portal.azure.com), **kaynak oluştur** ' u seçin ve *Azure SQL yönetilen örneği*' ni arayın. 
1. Microsoft tarafından yayımlanan **Azure SQL yönetilen örnek** seçeneğini seçin ve ardından sonraki sayfada **Oluştur** ' u seçin.
1. **Azure SQL veritabanı yönetilen örneği oluştur** sayfasının **temel bilgiler** sekmesinde, ikincil yönetilen örneğinizi yapılandırmak için gerekli alanları doldurun. 

   Aşağıdaki tabloda, ikincil yönetilen örnek için gereken değerler gösterilmektedir:
 
    | **Alan** | Value |
    | --- | --- |
    | **Abonelik** |  Birincil yönetilen örneğinizin bulunduğu abonelik. |
    | **Kaynak grubu**| Birincil yönetilen örneğinizin bulunduğu kaynak grubu. |
    | **Yönetilen örnek adı** | Yeni ikincil yönetilen örneğinizin adı, örneğin`sql-mi-secondary`  | 
    | **Bölge**| İkincil yönetilen örneğinizin [eşleştirilmiş bölge](/azure/best-practices-availability-paired-regions) konumu.  |
    | **Yönetilen örnek yöneticisi oturum açma** | Yeni ikincil yönetilen örneğiniz `azureuser`için kullanmak istediğiniz oturum açma gibi. |
    | **Parola** | Yeni ikincil yönetilen örnek için yönetici oturumu tarafından kullanılacak karmaşık bir parola.  |
    | &nbsp; | &nbsp; |

1. **Ağ** sekmesinde, **sanal ağ**için, açılan listeden ikincil yönetilen örnek için oluşturduğunuz sanal ağı seçin.

   ![İkincil mı ağı](media/sql-database-managed-instance-failover-group-tutorial/networking-settings-for-secondary-mi.png)

1. **Ek ayarlar** sekmesinde, **coğrafi çoğaltma**için, _Yük devretme Ikincili olarak kullanmak_için **Evet** ' i seçin. Açılan listeden birincil yönetilen örneği seçin. 
    1. Harmanlama ve saat diliminin, birincil yönetilen örnekle eşleştiğinden emin olun. Bu öğreticide oluşturulan birincil yönetilen örnek, `SQL_Latin1_General_CP1_CI_AS` harmanlama `(UTC) Coordinated Universal Time` ve saat dilimi varsayılanını kullanır. 

   ![İkincil mı ağı](media/sql-database-managed-instance-failover-group-tutorial/secondary-mi-failover.png)

1. İkincil yönetilen örneğinizin ayarlarını gözden geçirmek için **gözden geçir + oluştur** ' u seçin. 
1. İkincil yönetilen örneğinizi oluşturmak için **Oluştur** ' u seçin. 


## <a name="4---create-primary-virtual-network-gateway"></a>4-birincil sanal ağ geçidi oluşturma 

Yük devretme grubuna katılacak iki yönetilen örnek için, ağ iletişimine izin vermek üzere iki yönetilen örnek sanal ağları arasında yapılandırılmış bir ağ geçidi olmalıdır. Azure portal kullanarak, birincil yönetilen örnek için ağ geçidini oluşturabilirsiniz:

1. [Azure Portal](https://portal.azure.com), kaynak grubunuza gidin ve birincil yönetilen örneğiniz için **sanal ağ** kaynağını seçin. 
1. **Ayarlar** altında **alt ağlar** ' ı seçin ve ardından yeni bir **ağ geçidi alt ağı**eklemeyi seçin. Varsayılan değerleri bırakın. 

   ![Birincil yönetilen örnek için ağ geçidi ekle](media/sql-database-managed-instance-failover-group-tutorial/add-subnet-gateway-primary-vnet.png)

1. Alt ağ geçidi oluşturulduktan sonra sol gezinti bölmesinden **kaynak oluştur** ' u seçin ve arama kutusuna yazın `Virtual network gateway` . **Microsoft**tarafından yayınlanan **sanal ağ geçidi** kaynağını seçin. 

   ![Yeni bir sanal ağ geçidi oluştur](media/sql-database-managed-instance-failover-group-tutorial/create-virtual-network-gateway.png)

1. Birincil yönetilen örneğinizin ağ geçidini yapılandırmak için gerekli alanları doldurun. 

   Aşağıdaki tabloda, birincil yönetilen örnek için ağ geçidi için gereken değerler gösterilmektedir:
 
    | **Alan** | Value |
    | --- | --- |
    | **Abonelik** |  Birincil yönetilen örneğinizin bulunduğu abonelik. |
    | **Name** | Sanal ağ geçidinizin adı, `primary-mi-gateway`örneğin. | 
    | **Bölge** | İkincil yönetilen örneğinizin bulunduğu bölge. |
    | **Ağ Geçidi türü** | **VPN**' yi seçin. |
    | **VPN türü** | **Rota tabanlı** seçin |
    | **SKU**| Varsayılan `VpnGw1`bırakın. |
    | **Location**| İkincil yönetilen örneğinizin ve ikincil sanal ağınızın bulunduğu konum.   |
    | **Sanal ağ**| 2 `vnet-sql-mi-primary`. bölümde oluşturulan sanal ağı seçin. |
    | **Genel IP adresi**| **Yeni oluştur**’u seçin. |
    | **Genel IP adresi adı**| IP adresiniz `primary-gateway-IP`için gibi bir ad girin. |
    | &nbsp; | &nbsp; |
1. Diğer değerleri varsayılan olarak bırakın ve sonra sanal ağ geçidinizin ayarlarını gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

   ![Birincil ağ geçidi ayarları](media/sql-database-managed-instance-failover-group-tutorial/settings-for-primary-gateway.png)

1. Yeni sanal ağ geçidinizi oluşturmak için **Oluştur** ' u seçin. 

## <a name="5---configure-secondary-virtual-network-gateway"></a>5-ikincil sanal ağ geçidini yapılandırma 

İkinci yönetilen örnek için sanal ağ alt ağını ve ağ geçidini oluşturmak üzere önceki bölümdeki adımları yineleyin. İkincil yönetilen örneğiniz için ağ geçidini yapılandırmak üzere gerekli alanları doldurun. 

   Aşağıdaki tabloda, ikincil yönetilen örnek için ağ geçidi için gereken değerler gösterilmektedir:

   | **Alan** | Value |
   | --- | --- |
   | **Abonelik** |  İkincil yönetilen örneğinizin olduğu abonelik. |
   | **Name** | Sanal ağ geçidinizin adı, `secondary-mi-gateway`örneğin. | 
   | **Bölge** | İkincil yönetilen örneğinizin bulunduğu bölge. |
   | **Ağ Geçidi türü** | **VPN**' yi seçin. |
   | **VPN türü** | **Rota tabanlı** seçin |
   | **SKU**| Varsayılan `VpnGw1`bırakın. |
   | **Location**| İkincil yönetilen örneğinizin ve ikincil sanal ağınızın bulunduğu konum.   |
   | **Sanal ağ**| 2 `vnet-sql-mi-secondary`. bölümde oluşturulan sanal ağı seçin. |
   | **Genel IP adresi**| **Yeni oluştur**’u seçin. |
   | **Genel IP adresi adı**| IP adresiniz `secondary-gateway-IP`için gibi bir ad girin. |
   | &nbsp; | &nbsp; |

   ![İkincil ağ geçidi ayarları](media/sql-database-managed-instance-failover-group-tutorial/settings-for-secondary-gateway.png)

## <a name="6---connect-the-gateways"></a>6-ağ geçitlerini bağlama
Bu adımda, ağ geçitleri arasında bir bağlantı oluşturun. Birincil sunucudan ikincil ağ geçidine bir bağlantı kurulması ve sonra ikincil ile birincil ağ geçidi arasında ayrı bir bağlantı kurulması gerekir. Her iki ağ geçidi arasında bağlantı yapılandırırken aynı **paylaşılan anahtarı** kullandığınızdan emin olun. 

Bağlantıyı yapılandırmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) kaynak grubunuza gidin ve 4. adımda oluşturduğunuz birincil ağ geçidini seçin. 
1. **Ayarlar** altında **Bağlantılar** ' ı seçin ve ardından **Ekle** ' yi seçerek yeni bir bağlantı oluşturun. 

   ![Birincil ağ geçidine bağlantı ekle](media/sql-database-managed-instance-failover-group-tutorial/add-primary-gateway-connection.png)

1. Bağlantınız `Primary-connection`için bir ad girin ve gibi `mi1mi2psk` **paylaşılan anahtar** için bir değer yazın. 
1. **İkinci sanal ağ geçidi** ' ni seçin ve ardından ikincil yönetilen örnek `secondary-mi-gateway`için ağ geçidini (gibi) seçin. 

   ![Birincil-ikincil bağlantı oluştur](media/sql-database-managed-instance-failover-group-tutorial/create-primary-to-secondary-connection.png)

1. Yeni birincil-ikincil ağ geçidi bağlantınızı eklemek için **Tamam ' ı** seçin.
1. İkincil yönetilen örneğin ağ geçidinden, birincil yönetilen örneğin ağ geçidine bağlantı oluşturmak için bu adımları tekrarlayın. 

   ![İkincil-birincil bağlantı oluştur](media/sql-database-managed-instance-failover-group-tutorial/create-secondary-to-primary-connection.png)


## <a name="7---create-a-failover-group"></a>7-yük devretme grubu oluşturma
Bu adımda, yük devretme grubunu oluşturacak ve yönetilen örneklerin her ikisini de ekleyecek. 

1. [Azure Portal](https://portal.azure.com), **tüm hizmetler** ' e gidin `managed instance` ve arama kutusuna yazın. 
1. Seçim Sol taraftaki gezinti çubuğunuza kısayol olarak yönetilen örnekler eklemek için **SQL yönetilen örneklerinin** yanındaki yıldızı seçin. 
1. **SQL yönetilen örnekler** ' i `sql-mi-primary`seçin ve birincil yönetilen örneğinizi (gibi) seçin. 
1. **Ayarlar**altında, **örnek yük devretme grupları** ' na gidin ve sonra **örnek yük devretme grubu** sayfasını açmak için **Grup Ekle** ' yi seçin. 

   ![Yük devretme grubu ekleme](media/sql-database-managed-instance-failover-group-tutorial/add-failover-group.png)

1. **Örnek yük devretme grubu** sayfasında, yük devretme grubunuzun `failovergrouptutorial` `sql-mi-secondary` adını yazın (örneğin, açılan kutudan). Yük devretme grubunuzu oluşturmak için **Oluştur** ' u seçin. 

   ![Yük devretme grubu oluştur](media/sql-database-managed-instance-failover-group-tutorial/create-failover-group.png)

1. Yük devretme grubu dağıtımı tamamlandıktan sonra, **Yük devretme grubu** sayfasına geri yönlendirilirsiniz. 

## <a name="8---test-failover"></a>8-yük devretme testi
Bu adımda, yük devretme grubunuzu ikincil sunucuya devreder ve sonra Azure portal kullanarak yeniden başarısız olursunuz. 

1. [Azure Portal](https://portal.azure.com) içinde yönetilen örneğinize gidin ve ayarlar altında **örnek yük devretme grupları** ' nı seçin. 
1. Hangi yönetilen örnek birincil olduğunu ve hangi yönetilen örnek ikincil olduğunu gözden geçirin. 
1. **Yük devretme** ' yı seçin ve sonra kesilmekte olan tds oturumlarının uyarısında **Evet** ' i seçin. 

   ![Yük devretme grubu yükünü devreder](media/sql-database-managed-instance-failover-group-tutorial/failover-mi-failover-group.png)

1. Hangi manşlı örneğin birincil olduğunu ve hangi örneğin ikincil olduğunu gözden geçirin. Yük devretme başarılı olursa, iki örnek anahtarlamalı rollere sahip olmalıdır. 

   ![Yönetilen örnekler, yük devretmeden sonra rolleri değiştirdi](media/sql-database-managed-instance-failover-group-tutorial/mi-switched-after-failover.png)

1. Birincil örnek birincil role geri dönmek için **Yük devretmeyi** bir kez daha seçin. 


## <a name="clean-up-resources"></a>Kaynakları temizleme
Önce yönetilen örneği, ardından sanal kümeyi, ardından kalan kaynakları ve son olarak kaynak grubunu silerek kaynakları temizleyin. 

1. [Azure Portal](https://portal.azure.com)kaynak grubunuza gidin. 
1. Yönetilen örneği seçin ve **Sil**' i seçin. Kaynağı `yes` silmek istediğinizi onaylamak için metin kutusunu yazın ve ardından **Sil**' i seçin. Bu işlemin tamamlanması biraz zaman alabilir ve tamamlanana kadar *sanal kümeyi* ya da başka herhangi bir bağımlı kaynağı silemeyeceksiniz. Yönetilen örneğinizin silindiğini onaylamak için etkinlik sekmesindeki silmeyi izleyin. 
1. Yönetilen örnek silindikten sonra, *sanal kümeyi* kaynak grubunuzda seçip **Sil**' i seçerek silin. Kaynağı `yes` silmek istediğinizi onaylamak için metin kutusunu yazın ve ardından **Sil**' i seçin. 
1. Kalan kaynakları silin. Kaynağı `yes` silmek istediğinizi onaylamak için metin kutusunu yazın ve ardından **Sil**' i seçin. 
1. Kaynak grubunu Sil ' i seçerekkaynak grubunu Sil ' i seçin, kaynak grubunun `myResourceGroup`adını yazın ve **Sil**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, iki yönetilen örnek arasında bir yük devretme grubu yapılandırdınız. Şunları öğrendiniz:

> [!div class="checklist"]
> - Birincil yönetilen örnek oluşturma
> - Bir [Yük devretme grubunun](sql-database-auto-failover-group.md)parçası olarak ikincil bir yönetilen örnek oluşturun. 
> - Test yük devretmesi

Yönetilen örneğinizle bağlantı kurmak ve yönetilen örneğiniz için bir veritabanını geri yüklemek için bir sonraki hızlı başlangıca ilerleyin: 

> [!div class="nextstepaction"]
> [Yönetilen örneğinizle bağlantı](sql-database-managed-instance-configure-vm.md)[bir veritabanını yönetilen örneğe geri yükleme](sql-database-managed-instance-get-started-restore.md) 
> 


