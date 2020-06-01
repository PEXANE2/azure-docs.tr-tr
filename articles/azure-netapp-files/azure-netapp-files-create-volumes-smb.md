---
title: Azure NetApp Files için bir SMB birimi oluşturun | Microsoft Docs
description: Azure NetApp Files için SMB birimi oluşturmayı açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2020
ms.author: b-juche
ms.openlocfilehash: ef54eddd52d67f595fe78cd8bba9f308a4f19a94
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235234"
---
# <a name="create-an-smb-volume-for-azure-netapp-files"></a>Azure NetApp Files için SMB birimi oluşturma

Azure NetApp Files NFS ve SMBv3 birimlerini destekler. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır. Bu makalede, SMBv3 birimi oluşturma konusu gösterilmektedir. NFS birimi oluşturmak istiyorsanız, bkz. [Azure NetApp Files IÇIN NFS birimi oluşturma](azure-netapp-files-create-volumes.md). 

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.   
[Kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md)   
Azure NetApp Files için bir alt ağ atanmış olmalıdır.  
[Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)

## <a name="requirements-for-active-directory-connections"></a>Active Directory bağlantıları için gereksinimler

 SMB birimi oluşturmadan önce Active Directory bağlantı oluşturmanız gerekir. Active Directory bağlantıları için gereksinimler şunlardır: 

* Kullandığınız yönetici hesabının, belirttiğiniz kuruluş birimi (OU) yolunda makine hesapları oluşturma yeteneği olmalıdır.  

* Uygun bağlantı noktaları geçerli Windows Active Directory (AD) sunucusunda açık olmalıdır.  
    Gerekli bağlantı noktaları aşağıdaki gibidir: 

    |     Hizmet           |     Bağlantı noktası     |     Protokol     |
    |-----------------------|--------------|------------------|
    |    AD Web Hizmetleri    |    9389      |    TCP           |
    |    DNS                |    53        |    TCP           |
    |    DNS                |    53        |    UDP           |
    |    Icmpv4             |    Yok       |    Yankı yanıtı    |
    |    Kerberos           |    464       |    TCP           |
    |    Kerberos           |    464       |    UDP           |
    |    Kerberos           |    88        |    TCP           |
    |    Kerberos           |    88        |    UDP           |
    |    LDAP               |    389       |    TCP           |
    |    LDAP               |    389       |    UDP           |
    |    LDAP               |    3268      |    TCP           |
    |    NetBIOS adı       |    138       |    UDP           |
    |    SAM/LSA            |    445       |    TCP           |
    |    SAM/LSA            |    445       |    UDP           |
    |    W32Time            |    123       |    UDP           |

* Hedeflenen Active Directory Domain Services yönelik site topolojisi, özel olarak Azure NetApp Files dağıtıldığı Azure VNet 'te yönergelere uymalıdır.  

    Azure NetApp Files dağıtıldığı sanal ağın adres alanı yeni veya var olan bir Active Directory sitesine eklenmelidir (Azure NetApp Files tarafından erişilebilen bir etki alanı denetleyicisi). 

* Belirtilen DNS sunucularına, Azure NetApp Files için [temsilci alt ağından](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet) ulaşılabilir olmalıdır.  

    Desteklenen ağ topolojileri için [Azure NetApp Files ağ planlama yönergelerine](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) bakın.

    Ağ güvenlik grupları (NSG 'ler) ve güvenlik duvarları, Active Directory ve DNS trafiği isteklerine izin vermek için uygun şekilde yapılandırılmış kurallara sahip olmalıdır. 

* Azure NetApp Files atanmış alt ağ, tüm yerel ve uzak etki alanı denetleyicileri dahil olmak üzere etki alanındaki tüm Active Directory Domain Services (ekleme) etki alanı denetleyicilerine erişebilmelidir. Aksi takdirde, hizmet kesintisi meydana gelebilir.  

    Azure NetApp Files atanmış alt ağ tarafından erişilemeyen etki alanı denetleyicileriniz varsa, Active Directory bağlantısını oluştururken bir Active Directory sitesi belirtebilirsiniz.  Azure NetApp Files, yalnızca Azure NetApp Files atanmış alt ağ adres alanının olduğu sitedeki etki alanı denetleyicileriyle iletişim kurması gerekir.

    Bkz. AD siteleri ve hizmetleriyle ilgili [site topolojisini tasarlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology) . 
    
<!--
* Azure NetApp Files supports DES, Kerberos AES 128, and Kerberos AES 256 encryption types (from the least secure to the most secure). The user credentials used to join Active Directory must have the highest corresponding account option enabled that matches the capabilities enabled for your Active Directory.   

    For example, if your Active Directory has only the AES-128 capability, you must enable the AES-128 account option for the user credentials. If your Active Directory has the AES-256 capability, you must enable the AES-256 account option (which also supports AES-128). If your Active Directory does not have any Kerberos encryption capability, Azure NetApp Files uses DES by default.  

    You can enable the account options in the properties of the Active Directory Users and Computers Microsoft Management Console (MMC):   

    ![Active Directory Users and Computers MMC](../media/azure-netapp-files/ad-users-computers-mmc.png)
-->

Ek AD bilgileri hakkında Azure NetApp Files [SMB SSS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs) bölümüne bakın. 

## <a name="decide-which-domain-services-to-use"></a>Hangi etki alanı hizmetlerinin kullanılacağını belirleyin 

Azure NetApp Files, AD bağlantıları için hem [Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology) (eklemeleri) hem Azure Active Directory Domain Services (aeklemeleri) destekler.  Bir AD bağlantısı oluşturmadan önce, ekleme veya AEKLEMESINE göre ' yi kullanıp kullanmayacağınızı belirlemeniz gerekir.  

Daha fazla bilgi için bkz. [otomatik olarak yönetilen Active Directory Domain Services, Azure Active Directory ve yönetilen Azure Active Directory Domain Services karşılaştırma](https://docs.microsoft.com/azure/active-directory-domain-services/compare-identity-solutions). 

### <a name="active-directory-domain-services"></a>Active Directory Domain Services

Azure NetApp Files için tercih edilen [Active Directory Siteleri ve Hizmetleri](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology) kapsamını kullanabilirsiniz. Bu seçenek, [Azure NetApp Files tarafından erişilebilen](azure-netapp-files-network-topologies.md)etki alanı denetleyicilerini ACTIVE DIRECTORY DOMAIN SERVICES (ekler) okuma ve yazma işlemleri sağlar. Ayrıca, hizmetin belirtilen Active Directory Siteleri ve Hizmetleri sitesinde olmayan etki alanı denetleyicileriyle iletişim kurmasını da engeller. 

EKLEME kullandığınızda sitenizin adını bulmak için, kuruluşunuzda Active Directory Domain Services sorumlu yönetim grubuyla iletişim sağlayabilirsiniz. Aşağıdaki örnekte, site adının görüntülendiği Active Directory Siteleri ve Hizmetleri eklentisi gösterilmektedir: 

![Active Directory Siteleri ve Hizmetleri](../media/azure-netapp-files/azure-netapp-files-active-directory-sites-services.png)

Azure NetApp Files için bir AD bağlantısı yapılandırdığınızda, **ad site adı** alanı için kapsam içinde site adını belirtirsiniz.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services 

Azure Active Directory Domain Services (AEKLEYEN) yapılandırma ve kılavuz için bkz. [Azure AD Domain Services belgeleri](https://docs.microsoft.com/azure/active-directory-domain-services/).

Azure NetApp Files için ek AEKLEYEN konular geçerlidir: 

* AEKLEMELERI 'nin dağıtıldığı VNet veya alt ağın Azure NetApp Files dağıtımıyla aynı Azure bölgesinde olduğundan emin olun.
* Azure NetApp Files dağıtıldığı bölgede başka bir VNet kullanırsanız, iki sanal ağ arasında bir eşleme oluşturmanız gerekir.
* Azure NetApp Files `user` ve `resource forest` türlerini destekler.
* Eşitleme türü için, `All` veya seçebilirsiniz `Scoped` .   
    Seçeneğini belirlerseniz `Scoped` , SMB paylaşımlarına erişmek için doğru Azure AD grubunun seçildiğinden emin olun.  Emin değilseniz, `All` eşitleme türünü kullanabilirsiniz.
* Enterprise veya Premium SKU kullanımı gereklidir. Standart SKU desteklenmiyor.

Bir Active Directory bağlantısı oluşturduğunuzda, AEKLEMELERI için aşağıdaki özellikleri aklınızda bulabilirsiniz:

* **BIRINCIL DNS**, **Ikincil DNS**ve **ad DNS etki alanı ADıNA** ilişkin bilgileri, aeklemeleri menüsünde bulabilirsiniz.  
DNS sunucuları için, Active Directory bağlantısını yapılandırmak için iki IP adresi kullanılacaktır. 
* **Kuruluş birimi yolu** `OU=AADDC Computers` .  
Bu ayar, **NetApp hesabı**altındaki **Active Directory bağlantılarında** yapılandırılır:

  ![Kuruluş birimi yolu](../media/azure-netapp-files/azure-netapp-files-org-unit-path.png)

* **Kullanıcı adı** kimlik bilgileri, Azure AD grubunun **Azure AD DC yöneticileri**üyesi olan herhangi bir kullanıcı olabilir.


## <a name="create-an-active-directory-connection"></a>Active Directory bağlantısı oluşturma

1. NetApp hesabınızdan **Active Directory bağlantılar**' a ve ardından **Birleştir**' e tıklayın.  

    ![Active Directory bağlantıları](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. Active Directory Birleştir penceresinde, kullanmak istediğiniz etki alanı hizmetlerine bağlı olarak aşağıdaki bilgileri sağlayın:  

    Kullandığınız etki alanı hizmetlerine özgü bilgiler için bkz. [hangi etki alanı hizmetlerinin kullanılacağını belirleyin](#decide-which-domain-services-to-use). 

    * **Birincil DNS**  
        Bu, Active Directory etki alanına ekleme ve SMB kimlik doğrulama işlemleri için gereken DNS 'dir. 
    * **İkincil DNS**   
        Bu, gereksiz ad hizmetleri sağlamak için ikincil DNS sunucusudur. 
    * **AD DNS etki alanı adı**  
        Bu, birleştirmek istediğiniz Active Directory Domain Services etki alanı adıdır.
    * **AD site adı**  
        Bu, etki alanı denetleyicisi bulmanın sınırlandırılacak site adıdır.
    * **SMB sunucusu (bilgisayar hesabı) ön eki**  
        Bu, Azure NetApp Files yeni hesapların oluşturulması için kullanacağı Active Directory makine hesabının adlandırma ön ekidir.

        Örneğin, kuruluşunuzun dosya sunucuları için kullandığı adlandırma standardı, NAS-01, NAS-02..., NAS-045 ise, ön ek için "NAS" girersiniz. 

        Hizmet, gerektiğinde Active Directory daha fazla makine hesabı oluşturur.

        > [!IMPORTANT] 
        > Active Directory bağlantısını oluşturduktan sonra SMB sunucusu ön ekinin yeniden adlandırılması kesintiye uğramasıdır. SMB sunucu önekini yeniden adlandırdıktan sonra mevcut SMB paylaşımlarını yeniden bağlamanız gerekecektir.

    * **Kuruluş birimi yolu**  
        Bu, SMB sunucu makinesi hesaplarının oluşturulacağı kuruluş birimi (OU) için LDAP yoludur. Diğer bir deyişle, OU = ikinci düzey, OU = ilk düzey. 

        Azure Active Directory Domain Services ile Azure NetApp Files kullanıyorsanız, kuruluş birimi yolu, `OU=AADDC Computers` NetApp hesabınız için Active Directory yapılandırdığınızda olur.

     * **Yedekleme ilkesi kullanıcıları**  
        Azure NetApp Files ile kullanım için oluşturulan bilgisayar hesabına yükseltilmiş ayrıcalıklar gerektiren ek hesaplar ekleyebilirsiniz. Belirtilen hesapların dosya veya klasör düzeyinde NTFS izinlerini değiştirmesine izin verilir. Örneğin, Azure NetApp Files bir SMB dosya paylaşımında veri geçirmek için kullanılan ayrıcalıklı olmayan bir hizmet hesabı belirtebilirsiniz.  

        > [!IMPORTANT] 
        > Yedekleme İlkesi Kullanıcı özelliğinin kullanılması için beyaz liste gerekir. anffeedback@microsoft.comBu özelliği istemek için ABONELIK Kimliğiniz ile e-posta gönderin. 

    * **Kullanıcı adınız** ve **Parolanız** dahil kimlik bilgileri

    ![Active Directory Birleştir](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

3. **Katıl**’a tıklayın.  

    Oluşturduğunuz Active Directory bağlantı görüntülenir.

    ![Active Directory bağlantıları](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

## <a name="add-an-smb-volume"></a>SMB birimi ekleme

1. Kapasite havuzları dikey penceresinden **birimler** dikey penceresine tıklayın. 

    ![Birimlere git](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Birim oluştur penceresi görüntülenir.

3. Birim Oluştur penceresinde **Oluştur** ' a tıklayın ve aşağıdaki alanlar için bilgi sağlayın:   
    * **Birim adı**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Birim adı her bir kapasite havuzu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır. Herhangi bir alfasayısal karakter kullanabilirsiniz.   

        `default`Birim adı olarak kullanamazsınız.

    * **Kapasite havuzu**  
        Birimin oluşturulmasını istediğiniz kapasite havuzunu belirtin.

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Sanal ağ**  
        Birime erişmek istediğiniz Azure sanal ağını (VNet) belirtin.  

        Belirttiğiniz VNet Azure NetApp Files için bir alt ağa sahip olmalıdır. Azure NetApp Files hizmetine yalnızca aynı VNet 'ten veya VNet eşlemesi ile aynı bölgedeki bir VNet 'ten erişilebilir. Ayrıca, hızlı rota aracılığıyla şirket içi ağınızdan birime da erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Files için temsilci atanmış olmalıdır. 
        
        Bir alt ağ temsilcisi yoksa, birim oluştur sayfasında **Yeni oluştur** ' a tıklayabilirsiniz. Sonra alt ağ oluştur sayfasında alt ağ bilgilerini belirtin ve alt ağın Azure NetApp Files için temsilci olarak **Microsoft. NetApp/birimler** ' i seçin. Her VNet 'te Azure NetApp Files için yalnızca bir alt ağ atanabilir.   
 
        ![Birim oluşturun](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. **Protokol** ' e tıklayın ve aşağıdaki bilgileri doldurun:  
    * Birimin protokol türü olarak **SMB** ' yi seçin. 
    * Açılır listeden **Active Directory** bağlantınızı seçin.
    * **Paylaşım adı**' nda paylaşılan birimin adını belirtin.

    ![SMB protokolünü belirtin](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Birim ayrıntılarını gözden geçirmek için **gözden geçir + oluştur** ' a tıklayın.  Ardından, SMB birimini oluşturmak için **Oluştur** ' a tıklayın.

    Oluşturduğunuz birim birimler sayfasında görünür. 
 
    Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.

## <a name="control-access-to-an-smb-volume"></a>SMB birimine erişimi denetleme  

SMB birimine erişim izinler aracılığıyla yönetilir.  

### <a name="share-permissions"></a>İzinleri paylaşma  

Varsayılan olarak, yeni bir birimde **Everyone/Full denetim** Share izinleri vardır. Domain Admins grubunun üyeleri, Azure NetApp Files birimi için kullanılan bilgisayar hesabında Bilgisayar Yönetimi ' ni kullanarak paylaşma izinlerini değiştirebilir.

![SMB bağlama yolu ](../media/azure-netapp-files/smb-mount-path.png) 
 ![ kümesi paylaşma izinleri](../media/azure-netapp-files/set-share-permissions.png) 

### <a name="ntfs-file-and-folder-permissions"></a>NTFS dosya ve klasör izinleri  

Windows SMB istemcisinde nesnenin özelliklerinin **güvenlik** sekmesini kullanarak bir dosya veya klasör için izinleri ayarlayabilirsiniz.
 
![Dosya ve klasör izinlerini ayarlama](../media/azure-netapp-files/set-file-folder-permissions.png) 

## <a name="next-steps"></a>Sonraki adımlar  

* [Windows veya Linux sanal makineleri için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
* [SMB hakkında SSS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs)
* [Azure hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
* [Azure CLı kullanarak yeni bir Active Directory ormanı yüklemesi](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm)
