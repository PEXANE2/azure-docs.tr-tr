---
title: Azure NetApp Dosyaları için Bir Kobİ hacmi oluşturma | Microsoft Dokümanlar
description: Azure NetApp Dosyaları için Nasıl Kobİ hacmi oluşturulacak açıklanır.
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
ms.date: 03/13/2020
ms.author: b-juche
ms.openlocfilehash: b2000c3fd3d64793f797e997d8f3c10eaed5d7aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409616"
---
# <a name="create-an-smb-volume-for-azure-netapp-files"></a>Azure NetApp Files için SMB birimi oluşturma

Azure NetApp Files, NFS ve SMBv3 birimlerini destekler. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır. Bu makalede, nasıl bir SMBv3 birim oluşturmak için gösterir. Bir NFS birimi oluşturmak istiyorsanız, Azure [NetApp Dosyaları için NFS birimi oluşturma'ya](azure-netapp-files-create-volumes.md)bakın. 

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.   
[Kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md)   
Bir alt ağ Azure NetApp Dosyalarına devredilmelidir.  
[Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)

## <a name="requirements-for-active-directory-connections"></a>Etkin Dizin bağlantıları için gereksinimler

 Bir Kobİ birimi oluşturmadan önce Active Directory bağlantıları oluşturmanız gerekir. Active Directory bağlantıları için gereksinimler şunlardır: 

* Kullandığınız yönetici hesabı, belirteceğiniz kuruluş birimi (OU) yolunda makine hesapları oluşturma yeteneğine sahip olmalıdır.  

* İlgili Windows Active Directory (AD) sunucusunda uygun bağlantı noktaları açık olmalıdır.  
    Gerekli bağlantı noktaları aşağıdaki gibidir: 

    |     Hizmet           |     Bağlantı noktası     |     Protokol     |
    |-----------------------|--------------|------------------|
    |    AD Web Hizmetleri    |    9389      |    TCP           |
    |    DNS                |    53        |    TCP           |
    |    DNS                |    53        |    UDP           |
    |    ICMPv4             |    Yok       |    Yankı Yanıtı    |
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
    |    w32time            |    123       |    UDP           |

* Hedeflenen Active Directory Etki Alanı Hizmetleri için site topolojisi, özellikle Azure NetApp Dosyalarının dağıtıldığı Azure VNet başta olmak üzere en iyi uygulamalara uymalıdır.  

    Azure NetApp Dosyalarının dağıtıldığı sanal ağın adres alanı yeni veya varolan Active Directory sitesine (Azure NetApp Files tarafından erişilebilen bir etki alanı denetleyicisinin bulunduğu) eklenmelidir. 

* Belirtilen DNS sunucularına Azure NetApp Dosyaları'nın [devredilen alt tarafından](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet) erişilebilmelidir.  

    Desteklenen ağ topolojileri için [Azure NetApp Dosyaları ağ planlaması yönergelerine](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) bakın.

    Ağ Güvenlik Grupları (NSGs) ve güvenlik duvarları, Etkin Dizin ve DNS trafik isteklerine izin verecek şekilde uygun şekilde yapılandırılmış kurallara sahip olmalıdır. 

* Azure NetApp Files devredilen alt ağı, tüm yerel ve uzak etki alanı denetleyicileri de dahil olmak üzere etki alanında bulunan tüm Active Directory Domain Services (ADDS) etki alanı denetleyicilerine erişebilmeli. Aksi takdirde, hizmet kesintisi oluşabilir.  

    Azure NetApp Files tarafından erişilemeyen etki alanı denetleyicileriniz varsa, Active Directory bağlantısının oluşturulması sırasında Bir Active Directory sitesi belirtebilirsiniz.  Azure NetApp Dosyaları'nın yalnızca Azure NetApp Dosyaları'nın alt ağ adres alanının bulunduğu sitedeki etki alanı denetleyicileriyle iletişim kurması gerekir.

    Bkz. AD siteleri ve hizmetleri hakkında [site topolojisinin tasarlanması.](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology) 
    
Ek AD bilgileri hakkında Azure NetApp Dosyaları [Kobİ SSS'lerine](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs) bakın. 

## <a name="decide-which-domain-services-to-use"></a>Hangi Etki Alanı Hizmetlerini kullanacağına karar verin 

Azure NetApp Files, AD bağlantıları için hem [Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology) (ADDS) hem de Azure Active Directory Domain Services 'i (AADDS) destekler.  Bir AD bağlantısı oluşturmadan önce ADDS veya AADDS'ı kullanıp kullanmayacağınıza karar vermeniz gerekir.  

Daha fazla bilgi için bkz: [Kendi kendini yöneten Active Directory Etki Alanı Hizmetlerini, Azure Etkin Dizini ve yönetilen Azure Etkin Dizin Etki Alanı Hizmetlerini Karşılaştır.](https://docs.microsoft.com/azure/active-directory-domain-services/compare-identity-solutions) 

### <a name="active-directory-domain-services"></a>Active Directory Domain Services

Azure NetApp Dosyaları için tercih ettiğiniz [Active Directory Sites and Services](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology) kapsamını kullanabilirsiniz. Bu seçenek, [Azure NetApp Dosyaları tarafından erişilebilen](azure-netapp-files-network-topologies.md)Active Directory Domain Services (ADDS) etki alanı denetleyicilerine okuma ve yazma olanağı sağlar. Ayrıca, hizmetin belirtilen Active Directory Sites and Services sitesinde olmayan etki alanı denetleyicileriyle iletişim kurmasını da engeller. 

ADDS'ı kullandığınızda sitenizin adını bulmak için, kuruluşunuzdaki Active Directory Domain Services'dan sorumlu yönetim grubuyla iletişim kurabilirsiniz. Aşağıdaki örnekte, site adının görüntülendiği Active Directory Sites and Services eklentisi gösterilmektedir: 

![Active Directory Siteleri ve Hizmetleri](../media/azure-netapp-files/azure-netapp-files-active-directory-sites-and-services.png)

Azure NetApp Dosyaları için bir AD bağlantısı yapılandırdığınızda, **AD Site Adı** alanının kapsamının site adını belirtirsiniz.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services 

Azure Etkin Dizin Etki Alanı Hizmetleri (AADDS) yapılandırması ve kılavuzu için [Azure AD Etki Alanı Hizmetleri belgelerine](https://docs.microsoft.com/azure/active-directory-domain-services/)bakın.

Azure NetApp Dosyaları için ek AADDS hususları geçerlidir: 

* AADDS'ın dağıtıldığı VNet veya alt netin Azure NetApp Dosyaları dağıtımıyla aynı Azure bölgesinde olduğundan emin olun.
* Azure NetApp Dosyalarının dağıtıldığı bölgede başka bir VNet kullanıyorsanız, iki VNet arasında bir eşleme oluşturmanız gerekir.
* Azure NetApp `user` Dosyaları `resource forest` destekler ve türleri.
* Eşitleme türü için, `All` seçebilirsiniz `Scoped`veya .   
    Seçerseniz, `Scoped`Kobİ paylaşımlarına erişmek için doğru Azure REKLAM grubunun seçildiğinden emin olun.  Emin değilseniz, `All` eşitleme türünü kullanabilirsiniz.
* Kurumsal veya Premium SKU'nun kullanımı gereklidir. Standart SKU desteklenmez.

Etkin Dizin bağlantısı oluşturduğunuzda, AADDS için aşağıdaki özellikleri not edin:

* AADDS menüsünde **Birincil DNS,** **İkincil DNS**ve **AD DNS Alan Adı** hakkında bilgi bulabilirsiniz.  
DNS sunucuları için Active Directory bağlantısını yapılandırmak için iki IP adresi kullanılır. 
* **Organizasyonel birim** `OU=AADDC Computers`yolu.  
Bu **ayar, NetApp Hesabı**altındaki **Active Directory Connections'da** yapılandırılmıştır:

  ![Örgütsel birim yolu](../media/azure-netapp-files/azure-netapp-files-org-unit-path.png)

* **Kullanıcı adı** kimlik bilgileri, Azure AD grubu **Azure AD DC Yöneticilerinin**üyesi olan tüm kullanıcı olabilir.


## <a name="create-an-active-directory-connection"></a>Etkin Dizin bağlantısı oluşturma

1. NetApp hesabınızdan **Active Directory bağlantılarını**tıklatın ve ardından **Katıl'ı**tıklatın.  

    ![Etkin Dizin Bağlantıları](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. Etkin Dizine Katıl penceresinde, kullanmak istediğiniz Etki Alanı Hizmetlerini temel alan aşağıdaki bilgileri sağlayın:  

    Kullandığınız Etki Alanı Hizmetlerine özel bilgiler için [bkz.](#decide-which-domain-services-to-use) 

    * **Birincil DNS**  
        Bu, Etkin Dizin etki alanı birleştirme ve Kobİ kimlik doğrulama işlemleri için gerekli olan DNS'dir. 
    * **İkincil DNS**   
        Bu, gereksiz ad hizmetlerini sağlamak için ikincil DNS sunucusudur. 
    * **AD DNS Etki Alanı Adı**  
        Bu, katılmak istediğiniz Active Directory Etki Alanı Hizmetlerinizin etki alanı adıdır.
    * **AD Site Adı**  
        Bu, Etki Alanı Denetleyicisi keşfinin sınırlandırılacak site adıdır.
    * **SMB sunucusu (bilgisayar hesabı) öneki**  
        Bu, Azure NetApp Dosyaları'nın yeni hesaplar oluşturmak için kullanacağı Active Directory'deki makine hesabının adlandırma önekidir.

        Örneğin, kuruluşunuzun dosya sunucuları için kullandığı adlandırma standardı NAS-01, NAS-02..., NAS-045 ise, önek için "NAS" girersiniz. 

        Hizmet, gerektiğinde Active Directory'de ek makine hesapları oluşturur.

    * **Örgütsel birim yolu**  
        Bu, Kobİ sunucu makine hesaplarının oluşturulacağı kuruluş birimi (OU) için LDAP yoludur. Yani, OU=ikinci seviye, OU=birinci seviye. 

        Azure Active Directory Etki Alanı Hizmetleri ile Azure NetApp Dosyalarını kullanıyorsanız, kuruluş birimi yolu NetApp hesabınız için Active Directory'yi yapılandırdığınızda dır. `OU=AADDC Computers`
        
    * **Kullanıcı adınız** ve **şifreniz** de dahil olmak üzere kimlik bilgileri

    ![Aktif Dizine Katılın](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

3. **Katıl**’a tıklayın.  

    Oluşturduğunuz Etkin Dizin bağlantısı görüntülenir.

    ![Etkin Dizin Bağlantıları](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

> [!NOTE] 
> Active Directory bağlantısını kurtardıktan sonra kullanıcı adı ve parola alanlarını değiştirebilirsiniz. Bağlantı kaydedildikten sonra başka hiçbir değer düzenlenemez. Başka değerler değiştirmeniz gerekiyorsa, önce dağıtılan Kobİ birimlerini silmeniz, ardından Active Directory bağlantısını silmeniz ve yeniden oluşturmanız gerekir.

## <a name="add-an-smb-volume"></a>Kobİ birimi ekleme

1. Kapasite Havuzları bıçağından **Birimler** bıçağını tıklatın. 

    ![Birimlere Git](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Birim Oluştur penceresi görüntülenir.

3. Birim Oluştur penceresinde, **Oluştur'u** tıklatın ve aşağıdaki alanlar için bilgi sağlayın:   
    * **Birim adı**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Bir birim adı her kapasite havuzu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalı. Herhangi bir alfasayısal karakter kullanabilirsiniz.   

        Birim adı olarak `default` kullanamazsınız.

    * **Kapasite havuzu**  
        Birimin oluşturulmasını istediğiniz kapasite havuzunu belirtin.

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Sanal ağ**  
        Ses düzeyine erişmek istediğiniz Azure sanal ağını (VNet) belirtin.  

        Belirttiğiniz VNet'in Azure NetApp Dosyalarına bir alt ağı olmalıdır. Azure NetApp Files hizmetine yalnızca aynı VNet'ten veya VNet eşleme yoluyla ses seviyesiyle aynı bölgede bulunan bir VNet'ten erişilebilir. Ayrıca, Express Route üzerinden şirket içi ağınızdan ses birimine de erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Dosyalarına devredilmelidir. 
        
        Bir alt ağ görevlendirmediyseniz, Birim Oluştur sayfasında **yeni oluştur'u** tıklatabilirsiniz. Ardından Alt Net Oluştur sayfasında alt net bilgilerini belirtin ve Azure NetApp Dosyaları'nın alt netini devretmek için **Microsoft.NetApp/volumes'u** seçin. Her VNet'te Azure NetApp Dosyalarına yalnızca bir alt ağ devredilebilir.   
 
        ![Birim oluşturun](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. **Protokol'e** tıklayın ve aşağıdaki bilgileri doldurun:  
    * Birim için protokol türü olarak **SMB'yi** seçin. 
    * Açılan listeden **Active Directory** bağlantınızı seçin.
    * Paylaşılan birimin adını Share **adına**belirtin.

    ![SMB protokolünü belirtin](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Birim ayrıntılarını gözden geçirmek için **Gözden Geçir + Oluştur'u** tıklatın.  Ardından Kobİ birimini oluşturmak için **Oluştur'u** tıklatın.

    Oluşturduğunuz birim Birimler sayfasında görünür. 
 
    Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar  

* [Windows veya Linux sanal makineleri için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
* [Kobİ Sıkça Sorulan Soru](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs)
* [Azure hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
* [Azure CLI'yi kullanarak yeni bir Active Directory ormanı yükleme](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm)
