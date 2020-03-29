---
title: 'Öğretici: Kullanılabilirlik grubu için ön koşullar'
description: Bu öğretici, Azure VM'lerde her zaman kullanılabilirlik grubunda bir SQL Server Her Zaman oluşturmak için ön koşullarınasıl yapılandırılabildiğini gösterir.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: c492db4c-3faa-4645-849f-5a1a663be55a
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/29/2018
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: c80a4c07e0649612b4c024cac79833c4b730f55e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060163"
---
# <a name="prerequisites-for-creating-always-on-availability-groups-on-sql-server-on-azure-virtual-machines"></a>Azure sanal makinelerde SQL Server'da Her Zaman Açık kullanılabilirlik gruplarını oluşturmak için ön koşullar

Bu öğretici, [Azure sanal makinelerinde (VM) kullanılabilirlik grubunda her zaman bir SQL Server Her Zaman](virtual-machines-windows-portal-sql-availability-group-tutorial.md)oluşturmak için ön koşulların nasıl tamamlanış edilebildiğini gösterir. Ön koşulları tamamladığınızda, bir etki alanı denetleyiciniz, iki SQL Server VM'niz ve tek bir kaynak grubunda bir tanık sunucunuz vardır.

**Zaman tahmini**: Ön koşulların tamamlanması birkaç saat sürebilir. Bu sürenin büyük bir kısmı sanal makineler oluşturmak için harcanıyor.

Aşağıdaki diyagram, öğreticide ne oluşturduğunuzu göstermektedir.

![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="review-availability-group-documentation"></a>Kullanılabilirlik grubu belgelerini gözden geçirme

Bu öğretici, sql server her zaman kullanılabilirlik grupları üzerinde temel bir anlayışa sahip olduğunu varsayar. Bu teknolojiye aşina değilseniz, [Her Zaman Kullanılabilirlik Gruplarına (SQL Server) Genel Bakış'a](https://msdn.microsoft.com/library/ff877884.aspx)bakın.


## <a name="create-an-azure-account"></a>Azure hesabı oluşturma
Bir Azure hesabınız olmalıdır. Ücretsiz [bir Azure hesabı açabilir](https://signup.azure.com/signup?offer=ms-azr-0044p&appId=102&ref=azureplat-generic&redirectURL=https:%2F%2Fazure.microsoft.com%2Fget-started%2Fwelcome-to-azure%2F&correlationId=24f9d452-1909-40d7-b609-2245aa7351a6&l=en-US) veya [Visual Studio abone avantajlarını etkinleştirebilirsiniz.](https://docs.microsoft.com/visualstudio/subscriptions/subscriber-benefits)

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Portalda yeni bir nesne oluşturmak için tıklatın. **+**

   ![Yeni nesne](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-portalplus.png)

3. **Market** arama penceresinde **kaynak grubu** yazın.

   ![Kaynak grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroupsymbol.png)
4. **Kaynak grubunu**tıklatın.
5. **Oluştur'u**tıklatın.
6. **Kaynak grubu adı**altında, kaynak grubu için bir ad yazın. Örneğin, **sql-ha-rg**yazın.
7. Birden çok Azure aboneliğiniz varsa, aboneliğin kullanılabilirlik grubunu oluşturmak istediğiniz Azure aboneliği olduğunu doğrulayın.
8. Bir konum seçin. Konum, kullanılabilirlik grubunu oluşturmak istediğiniz Azure bölgesidir. Bu makalede, tüm kaynakları tek bir Azure konumunda oluşturur.
9. **Panoya Pin'in** işaretli olduğunu doğrulayın. Bu isteğe bağlı ayar, Azure portal panosundaki kaynak grubu için bir kısayol yerleştirir.

   ![Kaynak grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/01-resourcegroup.png)

10. Kaynak grubunu oluşturmak için **Oluştur**'a tıklayın.

Azure kaynak grubu oluşturur ve portaldaki kaynak grubuna bir kısayol sabitler.

## <a name="create-the-network-and-subnets"></a>Ağı ve alt ağları oluşturma
Bir sonraki adım, Azure kaynak grubunda ağları ve alt ağları oluşturmaktır.

Çözüm, iki alt ağ içeren bir sanal ağ kullanır. [Sanal ağa genel bakış,](../../../virtual-network/virtual-networks-overview.md) Azure'daki ağlar hakkında daha fazla bilgi sağlar.

Sanal ağ oluşturmak için:

1. Azure portalında, kaynak grubunuzda **+ Ekle'yi**tıklatın. 

   ![Yeni öğe](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/02-newiteminrg.png)
2. Sanal **ağ**arayın.

     ![Sanal ağda arama](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/04-findvirtualnetwork.png)
3. **Sanal ağa**tıklayın.
4. Sanal **ağda,** **Kaynak Yöneticisi** dağıtım modelini tıklatın ve sonra **Oluştur'u**tıklatın.

    Aşağıdaki tablosanal ağ ayarlarını gösterir:

   | **Alan** | Değer |
   | --- | --- |
   | **Adı** |autoHAVNET |
   | **Adres alanı** |10.33.0.0/24 |
   | **Alt ağ adı** |Yönetici |
   | **Alt ağ adres aralığı** |10.33.0.0/29 |
   | **Abonelik** |Kullanmak istediğiniz aboneliği belirtin. **Yalnızca** bir aboneliğiniz varsa abonelik boştur. |
   | **Kaynak grubu** |**Varolan'ı kullan'ı** seçin ve kaynak grubunun adını seçin. |
   | **Konum** |Azure konumunu belirtin. |

   Adres alanınız ve alt net adres aralığınız tablodan farklı olabilir. Aboneliğinize bağlı olarak, portal kullanılabilir bir adres alanı ve ilgili alt net adres aralığı önerir. Yeterli adres alanı yoksa, farklı bir abonelik kullanın.

   Örnek, Alt ağ adı **Yönetici**kullanır. Bu alt ağ etki alanı denetleyicileri içindir.

5. **Oluştur'u**tıklatın.

   ![Sanal ağı yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/06-configurevirtualnetwork.png)

Azure sizi portal panosuna döndürür ve yeni ağ oluşturulduğunda sizi haber verir.

### <a name="create-a-second-subnet"></a>İkinci bir alt ağ oluşturma
Yeni sanal **ağ, Admin**adlı bir alt ağ vardır. Etki alanı denetleyicileri bu alt ağı kullanır. SQL Server VM'ler **SQL**adlı ikinci bir alt ağ kullanır. Bu alt ağı yapılandırmak için:

1. Panonuzda, oluşturduğunuz kaynak grubu **SQL-HA-RG'yi**tıklatın. **Kaynaklar**altında kaynak grubunda ağı bulun.

    **SQL-HA-RG** görünmüyorsa, **Kaynak Grupları'nı** tıklatarak ve kaynak grubu adına göre filtreuygulayarak onu bulun.
2. Kaynak listesinde **autoHAVNET'i** tıklatın. 
3. **AutoHAVNET** sanal ağında, **Ayarlar** altında **Alt ağları**seçin.

    Zaten oluşturduğunuz alt ağı not edin.

   ![Sanal ağı yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/07-addsubnet.png)
5. İkinci bir alt ağ oluşturun. **+ Subnet'i**tıklatın.
6. **Ekle alt netinde,** **Ad**altında **sqlsubnet** yazarak subnet'i yapılandır. Azure otomatik olarak geçerli bir **Adres aralığı**belirtir. Bu adres aralığının içinde en az 10 adres olduğunu doğrulayın. Üretim ortamında daha fazla adres gerekebilir.
7. **Tamam**'a tıklayın.

    ![Sanal ağı yapılandırma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/08-configuresubnet.png)

Aşağıdaki tabloda ağ yapılandırma ayarları özetlenir:

| **Alan** | Değer |
| --- | --- |
| **Adı** |**autoHAVNET** |
| **Adres alanı** |Bu değer, aboneliğinizdeki kullanılabilir adres alanlarına bağlıdır. Tipik bir değer 10.0.0.0/16'dır. |
| **Alt ağ adı** |**yönetici** |
| **Alt ağ adres aralığı** |Bu değer, aboneliğinizdeki kullanılabilir adres aralıklarına bağlıdır. Tipik bir değer 10.0.0.0/24'dür. |
| **Alt ağ adı** |**sqlsubnet** |
| **Alt ağ adres aralığı** |Bu değer, aboneliğinizdeki kullanılabilir adres aralıklarına bağlıdır. Tipik bir değer 10.0.1.0/24'dür. |
| **Abonelik** |Kullanmak istediğiniz aboneliği belirtin. |
| **Kaynak Grubu** |**SQL-HA-RG** |
| **Konum** |Kaynak grubu için seçtiğiniz konumu belirtin. |

## <a name="create-availability-sets"></a>Kullanılabilirlik kümeleri oluşturma

Sanal makineler oluşturmadan önce kullanılabilirlik kümeleri oluşturmanız gerekir. Kullanılabilirlik kümeleri, planlı veya planlanmamış bakım olaylarının kapalı kalma süresini azaltır. Azure kullanılabilirlik kümesi, Azure'un fiziksel hata etki alanlarında yerleştirir ve etki alanlarını güncelleştiren mantıksal bir kaynak grubudur. Hata etki alanı, kullanılabilirlik kümesinin üyelerinin ayrı güç ve ağ kaynaklarına sahip olmasını sağlar. Güncelleştirme etki alanı, kullanılabilirlik kümesinin üyelerinin aynı anda bakım için aşağı indirilmesini sağlar. Daha fazla bilgi için bkz: [Sanal makinelerin kullanılabilirliğini yönetin.](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

İki kullanılabilirlik setine ihtiyacınız var. Bunlardan biri etki alanı denetleyicileri içindir. İkincisi SQL Server VM'ler içindir.

Kullanılabilirlik kümesi oluşturmak için kaynak grubuna gidin ve **Ekle'yi**tıklatın. **Kullanılabilirlik kümesini**yazarak sonuçları filtreleyin. Sonuçlarda **Kullanılabilirlik Kümesi'ni** tıklatın ve sonra **Oluştur'u**tıklatın.

İki kullanılabilirlik kümesini aşağıdaki tablodaki parametrelere göre yapılandırın:

| **Alan** | Etki alanı denetleyicikullanılabilirlik kümesi | SQL Server kullanılabilirlik kümesi |
| --- | --- | --- |
| **Adı** |adavailabilityset |sqlavailabilityset |
| **Kaynak grubu** |SQL-HA-RG |SQL-HA-RG |
| **Hata etki alanları** |3 |3 |
| **Güncelleme etki alanları** |5 |3 |

Kullanılabilirlik kümelerini oluşturduktan sonra Azure portalındaki kaynak grubuna dönün.

## <a name="create-domain-controllers"></a>Etki alanı denetleyicileri oluşturma
Ağı, alt ağları ve kullanılabilirlik kümelerini oluşturduktan sonra etki alanı denetleyicileri için sanal makineler oluşturmaya hazırsınız.

### <a name="create-virtual-machines-for-the-domain-controllers"></a>Etki alanı denetleyicileri için sanal makineler oluşturma
Etki alanı denetleyicilerini oluşturmak ve yapılandırmak için **SQL-HA-RG** kaynak grubuna dönün.

1. **Ekle**’ye tıklayın. 
2. **Windows Server 2016 Datacenter**yazın.
3. **Windows Server 2016 Datacenter'ı**tıklatın. **Windows Server 2016 Datacenter'da**dağıtım modelinin **Kaynak Yöneticisi**olduğunu doğrulayın ve ardından **Oluştur'u**tıklatın. 

İki sanal makine oluşturmak için önceki adımları yineleyin. İki sanal makinenin adını ver:

* reklam-birincil-dc
* reklam-ikincil-dc

  > [!NOTE]
  > **Reklam-ikincil-dc** sanal makine, Active Directory Domain Services için yüksek kullanılabilirlik sağlamak için isteğe bağlıdır.
  >
  >

Aşağıdaki tablo, bu iki makinenin ayarlarını gösterir:

| **Alan** | Değer |
| --- | --- |
| **Adı** |İlk etki alanı denetleyicisi: *ad-primary-dc*.</br>İkinci etki alanı *denetleyicisi reklam-ikincil-dc*. |
| **VM disk türü** |SSD |
| **Kullanıcı adı** |DomainAdmin |
| **Parola** |Contoso!0000 |
| **Abonelik** |*Aboneliğiniz* |
| **Kaynak grubu** |SQL-HA-RG |
| **Konum** |*Konumunuz* |
| **Boyut** |DS1_V2 |
| **Depolama** | **Yönetilen diskleri** - kullanın**Evet** |
| **Sanal ağ** |autoHAVNET |
| **Alt ağ** |yönetici |
| **Genel IP adresi** |*VM ile aynı ad* |
| **Ağ güvenlik grubu** |*VM ile aynı ad* |
| **Kullanılabilirlik kümesi** |adavailabilityset </br>**Hata etki alanları**:2 </br>**Etki alanlarını güncelleştir**:2|
| **Tanılama** |Etkin |
| **Tanılama depolama hesabı** |*Otomatik olarak oluşturuldu* |

   >[!IMPORTANT]
   >Bir VM'yi yalnızca oluşturduğunuzda kullanılabilirlik kümesine yerleştirebilirsiniz. VM oluşturulduktan sonra kullanılabilirlik kümesini değiştiremezsiniz. Bkz. [Sanal makinelerin kullanılabilirliğini yönetin.](../manage-availability.md)

Azure sanal makineleri oluşturur.

Sanal makineler oluşturulduktan sonra etki alanı denetleyicisini yapılandırın.

### <a name="configure-the-domain-controller"></a>Etki alanı denetleyicisini yapılandırma
Aşağıdaki adımlarda, **reklam-birincil dc** makinesini corp.contoso.com için etki alanı denetleyicisi olarak yapılandırın.

1. **Portalda, SQL-HA-RG** kaynak grubunu açın ve **ad-primary-dc** makinesini seçin. **ad-primary-dc'de,** uzak masaüstü erişimi için bir RDP dosyasını açmak için **Bağlan'ı** tıklatın.

    ![Sanal makineye bağlanma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/20-connectrdp.png)
2. Yapılandırılmış yönetici hesabınızla oturum açın (**\DomainAdmin**) ve parola **(Contoso!0000).**
3. Varsayılan olarak, **Sunucu Yöneticisi** panosu görüntülenmelidir.
4. Panodaki **Rolleri ve Özellikleri Ekle** bağlantısını tıklatın.

    ![Server Manager - Rol ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
5. **Sunucu Rolleri** bölümüne gelene kadar **İleri'yi** seçin.
6. Active **Directory Etki Alanı Hizmetleri** ve **DNS Server** rollerini seçin. Sizden istendiğinde, bu rollerin gerektirdiği ek özellikleri ekleyin.

   > [!NOTE]
   > Windows statik IP adresi olmadığı konusunda sizi uyarır. Yapılandırmayı sınandırıyorsanız, **Devam et'i**tıklatın. Üretim senaryoları için IP adresini Azure portalında statik olarak ayarlayın veya [etki alanı denetleyici makinesinin statik IP adresini ayarlamak için PowerShell'i kullanın.](../../../virtual-network/virtual-networks-reserved-private-ip.md)
   >
   >

    ![Roller ekle iletişim kutusu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/23-addroles.png)
7. **Onay** bölümüne ulaşana kadar **İleri'yi** tıklatın. Gerekirse **hedef sunucuyu otomatik olarak yeniden başlat'ı** seçin.
8. **Yükle'yi**tıklatın.
9. Özellikler yüklemeyi bitirdikten sonra **Server Manager** panosuna dönün.
10. Sol bölmedeki yeni **AD DS** seçeneğini seçin.
11. Sarı uyarı çubuğundaki **Diğer** bağlantıyı tıklatın.

    ![DNS Server VM üzerinde AD DS iletişim kutusu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/24-addsmore.png)
12. Tüm Sunucu Görev **Ayrıntıları** iletişim kutusunun **Eylem** sütununda, **bu sunucuyu etki alanı denetleyicisine yükselt'i**tıklatın.
13. Active **Directory Etki Alanı Hizmetleri Yapılandırma Sihirbazı'nda**aşağıdaki değerleri kullanın:

    | **Sayfası** | Ayar |
    | --- | --- |
    | **Dağıtım Yapılandırması** |**Yeni bir orman ekleme**<br/> **Kök etki alanı adı** = corp.contoso.com |
    | **Etki Alanı Denetleyicisi Seçenekleri** |**DSRM Şifresi** = Contoso!0000<br/>**Şifreyi Onayla** = Contoso!0000 |
14. Sihirbazdaki diğer sayfaları gözden geçirmek için **İleri'yi** tıklatın. **Önkoşullar Denetimi** sayfasında, aşağıdaki iletiyi gördüğünüzden doğrulayın: **Tüm ön koşul denetimleri başarıyla geçti.** Geçerli uyarı iletilerini gözden geçirebilirsiniz, ancak yüklemeye devam etmek mümkündür.
15. **Yükle'yi**tıklatın. **Ad-primary-dc** sanal makine otomatik olarak yeniden başlatılır.

### <a name="note-the-ip-address-of-the-primary-domain-controller"></a>Birincil etki alanı denetleyicisinin IP adresini not edin

DNS için birincil etki alanı denetleyicisini kullanın. Birincil etki alanı denetleyicisi IP adresini not edin.

Birincil etki alanı denetleyicisi IP adresini almanın bir yolu Azure portalından geçer.

1. Azure portalında kaynak grubunu açın.

2. Birincil etki alanı denetleyicisini tıklatın.

3. Birincil etki alanı denetleyicisi üzerinde **Ağ arabirimleri'ni**tıklatın.

![Ağ arabirimleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/25-primarydcip.png)

Bu sunucunun özel IP adresini not edin.

### <a name="configure-the-virtual-network-dns"></a>Sanal ağ DNS yapılandırma
İlk etki alanı denetleyicisini oluşturduktan ve ilk sunucuda DNS'yi etkinleştirdikten sonra, sanal ağı dns için bu sunucuyu kullanacak şekilde yapılandırın.

1. Azure portalında sanal ağa tıklayın.

2. **Ayarlar** **altında, DNS Server'ı**tıklatın.

3. **Özel'i**tıklatın ve birincil etki alanı denetleyicisinin özel IP adresini yazın.

4. **Kaydet**'e tıklayın.

### <a name="configure-the-second-domain-controller"></a>İkinci etki alanı denetleyicisini yapılandırma
Birincil etki alanı denetleyicisi yeniden başlattıktan sonra, ikinci etki alanı denetleyicisini yapılandırabilirsiniz. Bu isteğe bağlı adım yüksek kullanılabilirlik içindir. İkinci etki alanı denetleyicisini yapılandırmak için aşağıdaki adımları izleyin:

1. **Portalda, SQL-HA-RG** kaynak grubunu açın ve **reklam-ikincil-dc** makinesini seçin. **Reklam-ikincil-dc'de,** uzak masaüstü erişimi için bir RDP dosyasını açmak için **Bağlan'ı** tıklatın.
2. Yapılandırılmış yönetici hesabınızı **(BUILTIN\DomainAdmin)** ve şifrenizi **(Contoso!0000)** kullanarak VM'de oturum açın.
3. Tercih edilen DNS sunucu adresini etki alanı denetleyicisinin adresiyle değiştirin.
4. **Ağ ve Paylaşım Merkezi'nde**ağ arabirimini tıklatın.
   ![Ağ arabirimi](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/26-networkinterface.png)

5. **Özellikler**'e tıklayın.
6. **Internet Protokolü Sürüm 4'ü (TCP/IPv4)** seçin ve **Özellikler'i**tıklatın.
7. **Aşağıdaki DNS sunucu adreslerini kullan'ı** seçin ve Tercih Edilen **DNS sunucusunda**birincil etki alanı denetleyicisinin adresini belirtin.
8. Değişiklikleri işlemek için **Tamam'ı**ve ardından **Kapat'ı** tıklatın. Artık **corp.contoso.com**için VM'ye katılabilirsiniz.

   >[!IMPORTANT]
   >DNS ayarını değiştirdikten sonra uzak masaüstünüze olan bağlantıyı kaybederseniz, Azure portalına gidin ve sanal makineyi yeniden başlatın.

9. Uzak masaüstünden ikincil etki alanı denetleyicisine, **Server Manager Panosu'nu**açın.
10. Panodaki **Rolleri ve Özellikleri Ekle** bağlantısını tıklatın.

    ![Server Manager - Rol ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
11. **Sunucu Rolleri** bölümüne gelene kadar **İleri'yi** seçin.
12. Active **Directory Etki Alanı Hizmetleri** ve **DNS Server** rollerini seçin. Sizden istendiğinde, bu rollerin gerektirdiği ek özellikleri ekleyin.
13. Özellikler yüklemeyi bitirdikten sonra **Server Manager** panosuna dönün.
14. Sol bölmedeki yeni **AD DS** seçeneğini seçin.
15. Sarı uyarı çubuğundaki **Diğer** bağlantıyı tıklatın.
16. Tüm Sunucu Görev **Ayrıntıları** iletişim kutusunun **Eylem** sütununda, **bu sunucuyu etki alanı denetleyicisine yükselt'i**tıklatın.
17. **Dağıtım Yapılandırması**altında, **varolan bir etki alanına etki alanı denetleyicisi ekle'yi**seçin.
    ![Dağıtım yapılandırması](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/28-deploymentconfig.png)
18. **Seç'i**tıklatın.
19. Yönetici hesabını kullanarak bağlan (**CORP. CONTOSO.COM\domainadmin**) ve şifre (**Contoso!0000**).
20. **Ormandan etki alanı seçin,** etki alanınızı tıklatın ve sonra **Tamam'ı**tıklatın.
21. **Etki Alanı Denetleyici seçeneklerinde**varsayılan değerleri kullanın ve Bir DSRM parolası ayarlayın.

    >[!NOTE]
    >**DNS Seçenekleri** sayfası, bu DNS sunucusu için bir temsilci oluşturulamayacağı konusunda sizi uyarabilir. Bu uyarıyı üretim dışı ortamlarda yoksayabilirsiniz.
22. İletişim **Önkoşullar** denetimine ulaşana kadar **İleri'yi** tıklatın. Ardından **Yükle**'ye tıklayın.

Sunucu yapılandırma değişikliklerini tamamladıktan sonra, sunucuyu yeniden başlatın.

### <a name="add-the-private-ip-address-to-the-second-domain-controller-to-the-vpn-dns-server"></a>VPN DNS Sunucusuna ikinci etki alanı denetleyicisine Özel IP Adresi ekleme

Azure portalında, sanal ağ altında, DNS Server'ı ikincil etki alanı denetleyicisinin IP adresini içerecek şekilde değiştirin. Bu ayar, DNS hizmet artıklığı sağlar.

### <a name="configure-the-domain-accounts"></a><a name="DomainAccounts"></a>Etki alanı hesaplarını yapılandırma

Sonraki adımlarda, Etkin Dizin hesaplarını yapılandırAbilirsiniz. Aşağıdaki tablo hesapları gösterir:

| |Yükleme hesabı<br/> |sqlserver-0 <br/>SQL Server ve SQL Agent Service hesabı |sqlserver-1<br/>SQL Server ve SQL Agent Service hesabı
| --- | --- | --- | ---
|**Ad** |Yükleme |SQLSvc1 | SQLSvc2
|**Kullanıcı SamAccountName** |Yükleme |SQLSvc1 | SQLSvc2

Her hesabı oluşturmak için aşağıdaki adımları kullanın.

1. **Reklam-birincil-dc** makinesinde oturum açın.
2. **Server Manager'da** **Araçlar'ı**seçin ve ardından **Active Directory Administrative Center'ı**tıklatın.   
3. Sol bölmeden **corp (yerel)** seçin.
4. SağDaki **Görevler** bölmesine **Yeni'yi**seçin ve ardından **Kullanıcı'yı**tıklatın.
   ![Active Directory Yönetim Merkezi](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/29-addcnewuser.png)

   >[!TIP]
   >Her hesap için karmaşık bir parola ayarlayın.<br/> Üretim dışı ortamlar için, kullanıcı hesabını asla sona ermeyecek şekilde ayarlayın.

5. Kullanıcıyı oluşturmak için **Tamam'ı** tıklatın.
6. Üç hesabın her biri için önceki adımları yineleyin.

### <a name="grant-the-required-permissions-to-the-installation-account"></a>Yükleme hesabına gerekli izinleri verme
1. Active **Directory Administrative**Center'da, sol bölmedeki **corp (yerel)** seçeneğini belirleyin. Ardından sağdaki **Görevler** bölmesinde **Özellikler'i**tıklatın.

    ![CORP kullanıcı özellikleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/31-addcproperties.png)
2. **Uzantılar'ı**seçin ve ardından **Güvenlik** sekmesindeki **Gelişmiş** düğmesini tıklatın.
3. Corp iletişim kutusu **için Gelişmiş Güvenlik Ayarları'nda** **Ekle'yi**tıklatın.
4. **Bir asıl seçin,** **CORP\Install'ı**arayın ve ardından **Tamam'ı**tıklatın.
5. Tüm **özellikleri oku** onay kutusunu seçin.

6. Bilgisayar **nesneleri oluştur** onay kutusunu seçin.

     ![Corp kullanıcı izinleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/33-addpermissions.png)
7. **Tamam'ı**tıklatın ve sonra tekrar **Tamam'ı** tıklatın. **Corp** özellikleri penceresini kapatın.

Active Directory ve kullanıcı nesnelerini yapılandırmayı tamamladığınızda, iki SQL Server VM ve bir tanık sunucusu VM oluşturun. Ardından üçünü de etki alanına birleştirin.

## <a name="create-sql-server-vms"></a>SQL Server VMs oluşturma

Üç ek sanal makine oluşturun. Çözüm, SQL Server örneklerine sahip iki sanal makine gerektirir. Üçüncü bir sanal makine tanık olarak çalışacaktır. Windows Server 2016 bir [bulut tanık](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)kullanabilirsiniz , ancak önceki işletim sistemleri ile tutarlılık için bu belge bir tanık için sanal bir makine kullanır.  

Devam etmeden önce aşağıdaki tasarım kararlarını göz önünde bulundurun.

* **Depolama - Azure Yönetilen Diskler**

   Sanal makine depolama için Azure Yönetilen Diskler'i kullanın. Microsoft, SQL Server sanal makineleri için Yönetilen Diskler önerir. Yönetilen Diskler, depolama alanını arka planda yönetir. Ayrıca, Yönetilen Disklere sahip sanal makineler aynı kullanılabilirlik kümesinde olduğunda Azure uygun artıklık düzeyini sağlamak için depolama kaynaklarını dağıtır. Daha fazla bilgi için bkz. [Azure Yönetilen Disklere Genel Bakış](../managed-disks-overview.md). Kullanılabilirlik kümesindeki yönetilen diskler hakkındaki ayrıntılar için, [kullanılabilirlik kümesindeki VM'ler için Yönetilen Diskleri Kullan'a](../manage-availability.md#use-managed-disks-for-vms-in-an-availability-set)bakın.

* **Ağ - Üretimde özel IP adresleri**

   Sanal makineler için bu öğretici ortak IP adreslerini kullanır. Genel BIR IP adresi internet üzerinden sanal makineye doğrudan uzaktan bağlantı sağlar - yapılandırma adımlarını kolaylaştırır. Üretim ortamlarında Microsoft, SQL Server örneği VM kaynağının güvenlik açığı nı azaltmak için yalnızca özel IP adresleri önerir.

### <a name="create-and-configure-the-sql-server-vms"></a>SQL Server VM'leri oluşturma ve yapılandırma
Ardından, ek bir küme düğümü için üç VM-iki SQL Server VM ve bir VM oluşturun. VM'lerin her birini oluşturmak için **SQL-HA-RG** kaynak grubuna geri dön, **Ekle'yi**tıklatın, uygun galeri öğesini arayın, **Sanal Makine'yi**tıklatın ve ardından **Galeriden'i**tıklatın. VM'leri oluşturmanıza yardımcı olmak için aşağıdaki tablodaki bilgileri kullanın:


| Sayfa | VM1 | VM2 | VM3 |
| --- | --- | --- | --- |
| Uygun galeri öğesini seçin |**Windows Server 2016 Datacenter** |**Windows Server 2016'da SQL Server 2016 SP1 Enterprise** |**Windows Server 2016'da SQL Server 2016 SP1 Enterprise** |
| Sanal makine yapılandırma **Temelleri** |**Ad** = küme-fsw<br/>**Kullanıcı Adı** = DomainAdmin<br/>**Şifre** = Contoso!0000<br/>Abonelik = **Aboneliğiniz**<br/>**Kaynak grubu** = SQL-HA-RG<br/>**Konum** = Azure konumunuz |**Ad** = sqlserver-0<br/>**Kullanıcı Adı** = DomainAdmin<br/>**Şifre** = Contoso!0000<br/>Abonelik = **Aboneliğiniz**<br/>**Kaynak grubu** = SQL-HA-RG<br/>**Konum** = Azure konumunuz |**Ad** = sqlserver-1<br/>**Kullanıcı Adı** = DomainAdmin<br/>**Şifre** = Contoso!0000<br/>Abonelik = **Aboneliğiniz**<br/>**Kaynak grubu** = SQL-HA-RG<br/>**Konum** = Azure konumunuz |
| Sanal makine yapılandırma **Boyutu** |**BOYUT** = DS1\_V2 (1 vCPU, 3,5 GB) |**BOYUT** = DS2\_V2 (2 vCPUs, 7 GB)</br>Boyut, SSD depolamayı desteklemelidir (Premium disk desteği. )) |**BOYUT** = DS2\_V2 (2 vCPUs, 7 GB) |
| Sanal makine yapılandırma **Ayarları** |**Depolama**: Yönetilen diskleri kullanın.<br/>**Sanal ağ** = autoHAVNET<br/>**Subnet** = sqlsubnet(10.1.1.0/24)<br/>**Genel IP adresi** otomatik olarak oluşturulur.<br/>**Ağ güvenlik grubu** = Yok<br/>**İzleme Tanılama** = Etkin<br/>**Tanılama depolama hesabı** = Otomatik olarak oluşturulan bir depolama hesabı kullanın<br/>**Kullanılabilirlik seti** = sqlAvailabilitySet<br/> |**Depolama**: Yönetilen diskleri kullanın.<br/>**Sanal ağ** = autoHAVNET<br/>**Subnet** = sqlsubnet(10.1.1.0/24)<br/>**Genel IP adresi** otomatik olarak oluşturulur.<br/>**Ağ güvenlik grubu** = Yok<br/>**İzleme Tanılama** = Etkin<br/>**Tanılama depolama hesabı** = Otomatik olarak oluşturulan bir depolama hesabı kullanın<br/>**Kullanılabilirlik seti** = sqlAvailabilitySet<br/> |**Depolama**: Yönetilen diskleri kullanın.<br/>**Sanal ağ** = autoHAVNET<br/>**Subnet** = sqlsubnet(10.1.1.0/24)<br/>**Genel IP adresi** otomatik olarak oluşturulur.<br/>**Ağ güvenlik grubu** = Yok<br/>**İzleme Tanılama** = Etkin<br/>**Tanılama depolama hesabı** = Otomatik olarak oluşturulan bir depolama hesabı kullanın<br/>**Kullanılabilirlik seti** = sqlAvailabilitySet<br/> |
| Sanal makine **yapılandırması SQL Server ayarları** |Uygulanamaz |**SQL bağlantısı** = Özel (Sanal Ağ içinde)<br/>**Bağlantı Noktası** = 1433<br/>**SQL Kimlik Doğrulama** = Devre Dışı Bırakma<br/>**Depolama yapılandırması** = Genel<br/>**Otomatik yalama** = Pazar saat 02:00<br/>**Otomatik yedekleme** = Devre Dışı</br>**Azure Anahtar Kasası tümleştirmesi** = Devre Dışı |**SQL bağlantısı** = Özel (Sanal Ağ içinde)<br/>**Bağlantı Noktası** = 1433<br/>**SQL Kimlik Doğrulama** = Devre Dışı Bırakma<br/>**Depolama yapılandırması** = Genel<br/>**Otomatik yalama** = Pazar saat 02:00<br/>**Otomatik yedekleme** = Devre Dışı</br>**Azure Anahtar Kasası tümleştirmesi** = Devre Dışı |

<br/>

> [!NOTE]
> Burada önerilen makine boyutları, Azure VM'lerde kullanılabilirlik gruplarını sınamak içindir. Üretim iş yüklerinde en iyi performans için, Sql Server makine boyutları ve yapılandırma için önerilere bakın [Sql Server için Azure sanal makinelerde SQL Server için en iyi performans uygulamaları.](virtual-machines-windows-sql-performance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
>
>

Üç VM tam olarak sağlandıktan sonra, **corp.contoso.com** etki alanına katılmanız ve makinelere CORP\Install yönetim hakları vermeniz gerekir.

### <a name="join-the-servers-to-the-domain"></a><a name="joinDomain"></a>Sunucuları etki alanına katılma

Artık **corp.contoso.com**için VM'lere katılabilirsiniz. Hem SQL Server VM'leri hem de dosya paylaşımı tanık sunucusu için aşağıdaki adımları yapın:

1. **BUILTIN\DomainAdmin**ile sanal makineye uzaktan bağlanın.
2. **Sunucu Yöneticisi'nde**Yerel **Sunucu'ya**tıklayın.
3. **WORKGROUP** bağlantısını tıklatın.
4. Bilgisayar **Adı** bölümünde **Değiştir'i**tıklatın.
5. Etki **Alanı** onay kutusunu seçin ve metin kutusuna **corp.contoso.com** yazın. **Tamam**'a tıklayın.
6. Windows **Security** açılır pencere iletişim kutusunda, varsayılan etki alanı yöneticisi hesabı **(CORP\DomainAdmin)** ve parola **(Contoso!0000)** için kimlik bilgilerini belirtin.
7. "corp.contoso.com etki alanına hoş geldiniz" iletisini gördüğünüzde **Tamam'ı**tıklatın.
8. **Kapat'ı**tıklatın ve ardından açılan pencereiletişiminde **Şimdi Yeniden Başlat'ı** tıklatın.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-cluster-vm"></a>Her küme VM'de Corp\Install kullanıcısını yönetici olarak ekleme

Her sanal makine etki alanının bir üyesi olarak yeniden başlatıldıktan sonra, yerel yöneticiler grubunun bir üyesi olarak **CORP\Install'ı** ekleyin.

1. VM yeniden başlatılana kadar bekleyin, sonra **CORP\DomainAdmin** hesabını kullanarak **sqlserver-0** oturum açmak için birincil etki alanı denetleyicisinden RDP dosyasını yeniden başlatın.
   >[!TIP]
   >Etki alanı yöneticisi hesabıyla oturum açtınemin. Önceki adımlarda, BUILT IN yönetici hesabını kullanıyordunuz. Sunucu etki alanında olduğuna göre, etki alanı hesabını kullanın. RDP oturumunuzda *DOMAIN*\\*kullanıcı adını*belirtin.

2. **Server Manager'da** **Araçlar'ı**seçin ve ardından **Bilgisayar Yönetimi'ni**tıklatın.
3. Bilgisayar **Yönetimi** penceresinde, **Yerel Kullanıcılar ve Gruplar'ı**genişletin ve ardından **Gruplar'ı**seçin.
4. **Yöneticiler** grubunu çift tıklatın.
5. Yönetici **Özellikleri** iletişim kutusunda **Ekle** düğmesini tıklatın.
6. Kullanıcı **CORP\Install'ı**girin ve ardından **Tamam'ı**tıklatın.
7. **Yönetici Özellikleri** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.
8. **sqlserver-1** ve **cluster-fsw'de**önceki adımları tekrarlayın.

### <a name="set-the-sql-server-service-accounts"></a><a name="setServiceAccount"></a>SQL Server hizmet hesaplarını ayarlama

Her SQL Server VM'de SQL Server hizmet hesabını ayarlayın. Etki alanı hesaplarını yapılandırırken oluşturduğunuz hesapları kullanın.

1. **SQL Server Configuration Manager**’ı açın.
2. SQL Server hizmetini sağ tıklatın ve ardından **Özellikler'i**tıklatın.
3. Hesabı ve parolayı ayarlayın.
4. Diğer SQL Server VM'de bu adımları yineleyin.  

SQL Server kullanılabilirlik grupları için her SQL Server VM'nin bir etki alanı hesabı olarak çalışması gerekir.

### <a name="create-a-sign-in-on-each-sql-server-vm-for-the-installation-account"></a>Yükleme hesabı için her SQL Server VM'de oturum açma oluşturma

Kullanılabilirlik grubunu yapılandırmak için yükleme hesabını (CORP\install) kullanın. Bu hesabın her SQL Server VM'deki **sysadmin** sabit sunucu rolünün bir üyesi olması gerekir. Aşağıdaki adımlar yükleme hesabı için oturum açma oluşturur:

1. * \<MachineName\>\DomainAdmin* hesabını kullanarak Uzak Masaüstü Protokolü (RDP) aracılığıyla sunucuya bağlanın.

1. SQL Server Management Studio'yu açın ve SQL Server'ın yerel örneğine bağlanın.

1. **Nesne Gezgini'nde**Güvenlik'i **tıklatın.**

1. Sağ **tıkla Girişler**. **Yeni Giriş'e**tıklayın.

1. **Giriş Yap - Yeni**, **Arama'yı**tıklatın.

1. **Konumlar'ı**tıklatın.

1. Etki alanı yöneticisi ağ kimlik bilgilerini girin.

1. Yükleme hesabını kullanın.

1. Oturum açmayı **sysadmin** sabit sunucu rolünün bir üyesi olarak ayarlayın.

1. **Tamam**'a tıklayın.

Diğer SQL Server VM'de önceki adımları yineleyin.

## <a name="add-failover-clustering-features-to-both-sql-server-vms"></a>Her iki SQL Server VM'sine Failover Kümeleme özellikleri ekleme

Failover Clustering özelliklerini eklemek için, her iki SQL Server VM'de de aşağıdaki adımları yapın:

1. *CORP\install* hesabını kullanarak Uzak Masaüstü Protokolü (RDP) aracılığıyla SQL Server sanal makinesine bağlanın. **Sunucu Yöneticisi Panosu'ni**aç.
2. Panodaki **Rolleri ve Özellikleri Ekle** bağlantısını tıklatın.

    ![Server Manager - Rol ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/22-addfeatures.png)
3. **Sunucu Özellikleri** bölümüne gelene kadar **İleri'yi** seçin.
4. **Özellikler'de** **Failover Clustering'i**seçin.
5. Gerekli ek özellikleri ekleyin.
6. Özellikleri eklemek için **Yükle'yi** tıklatın.

Diğer SQL Server VM'deki adımları yineleyin.

  >[!NOTE]
  > Bu adım, SQL Server VM'leri aslında başarısız kümeye katılmanın yanı sıra, artık [Azure SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) ve [Azure Quickstart Şablonları](virtual-machines-windows-sql-availability-group-quickstart-template.md)ile otomatikleştirilebiliyor.


## <a name="a-nameendpoint-firewall-configure-the-firewall-on-each-sql-server-vm"></a><a name="endpoint-firewall">Her SQL Server VM'deki güvenlik duvarını yapılandırma

Çözüm, güvenlik duvarında aşağıdaki TCP bağlantı noktalarının açık olmasını gerektirir:

- **SQL Server VM**:<br/>
   SQL Server varsayılan örneği için Bağlantı Noktası 1433.
- **Azure yük dengeleyici sondası:**<br/>
   Mevcut herhangi bir bağlantı noktası. Örnekler sık sık 59999 kullanın.
- **Veritabanı bitiş noktasını yansıtan:** <br/>
   Mevcut herhangi bir bağlantı noktası. Örnekler sık sık 5022 kullanın.

Güvenlik duvarı bağlantı noktalarının her iki SQL Server VM'de de açık olması gerekir.

Bağlantı noktalarını açma yöntemi kullandığınız güvenlik duvarı çözümüne bağlıdır. Sonraki bölümde Windows Güvenlik Duvarı'ndaki bağlantı noktalarının nasıl açılacağını açıklanmaktadır. SQL Server VM'lerinizin her birinde gerekli bağlantı noktalarını açın.

### <a name="open-a-tcp-port-in-the-firewall"></a>Güvenlik duvarında bir TCP bağlantı noktası açma

1. İlk SQL Server **Start** ekranında Gelişmiş **Güvenlik ile Windows Güvenlik Duvarı'nı**başlatın.
2. Sol bölmede Gelen **Kurallar'ı**seçin. Sağ bölmede Yeni **Kural'ı**tıklatın.
3. **Kural Türü**için, **Bağlantı Noktası'nı**seçin.
4. Bağlantı noktası için **TCP** belirtin ve uygun bağlantı noktası numaralarını yazın. Aşağıdaki örneğe bakın:

   ![SQL güvenlik duvarı](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/35-tcpports.png)

5. **İleri**'ye tıklayın.
6. **Eylem** sayfasında, bağlantıyı seçili **olarak İzin** ver'i tutun ve sonra **İleri'yi**tıklatın.
7. **Profil** sayfasında varsayılan ayarları kabul edin ve sonra **İleri'yi**tıklatın.
8. **Ad** sayfasında, **Ad** metin kutusuna bir kural adı **(Azure LB Sondası**gibi) belirtin ve ardından **Bitir'i**tıklatın.

İkinci SQL Server VM'de bu adımları yineleyin.

## <a name="configure-system-account-permissions"></a>Sistem hesabı izinlerini yapılandırma

Sistem hesabı için bir hesap oluşturmak ve uygun izinler vermek için, her SQL Server örneğinde aşağıdaki adımları tamamlayın:

1. Her SQL `[NT AUTHORITY\SYSTEM]` Server örneğinde bir hesap oluşturun. Aşağıdaki komut dosyası bu hesabı oluşturur:

   ```sql
   USE [master]
   GO
   CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS WITH DEFAULT_DATABASE=[master]
   GO 
   ```

1. Her SQL Server `[NT AUTHORITY\SYSTEM]` örneğinde aşağıdaki izinleri ver:

   - `ALTER ANY AVAILABILITY GROUP`
   - `CONNECT SQL`
   - `VIEW SERVER STATE`

   Aşağıdaki komut dosyası bu izinleri verir:

   ```sql
   GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM]
   GO
   GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM]
   GO
   GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM]
   GO 
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal makinelerinde her zaman kullanılabilirlik grubunda bir SQL Server oluşturma](virtual-machines-windows-portal-sql-availability-group-tutorial.md)
