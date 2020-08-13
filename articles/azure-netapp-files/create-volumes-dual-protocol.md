---
title: Azure NetApp Files için bir çift protokol (NFSv3 ve SMB) birimi oluşturun | Microsoft Docs
description: LDAP kullanıcı eşlemesi desteğiyle NFSv3 ve SMB 'nin ikili protokolünü kullanan bir birimin nasıl oluşturulduğunu açıklar.
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
ms.topic: how-to
ms.date: 8/11/2020
ms.author: b-juche
ms.openlocfilehash: f4cc253de0de9d099cfc4881f48182cf9b2a1616
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88134590"
---
# <a name="create-a-dual-protocol-nfsv3-and-smb-volume-for-azure-netapp-files"></a>Azure NetApp Files için bir çift protokol (NFSv3 ve SMB) birimi oluşturun

Azure NetApp Files, NFS (NFSv3 ve NFSv 4.1), SMBv3 veya Dual Protocol kullanarak birim oluşturmayı destekler. Bu makalede, LDAP kullanıcı eşlemesi desteğiyle NFSv3 ve SMB 'nin ikili protokolünü kullanan bir birimin nasıl oluşturulacağı gösterilmektedir.  


## <a name="before-you-begin"></a>Başlamadan önce 

* Zaten bir kapasite havuzu ayarlamış olmalısınız.  
    Bkz. [Kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md).   
* Azure NetApp Files için bir alt ağ atanmış olmalıdır.  
    [Azure NetApp Files için bir alt ağ temsilcisine](azure-netapp-files-delegate-subnet.md)bakın.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* [Active Directory bağlantıları Için gereksinimleri](azure-netapp-files-create-volumes-smb.md#requirements-for-active-directory-connections)karşıladığınızdan emin olun. 
* DNS sunucusunda bir geriye doğru arama bölgesi oluşturun ve ardından bu geriye doğru arama bölgesine AD ana makinesi için bir işaretçi (PTR) kaydı ekleyin. Aksi halde, çift protokol birimi oluşturma işlemi başarısız olur.
* NFS istemcisinin güncel olduğundan ve işletim sistemi için en son güncelleştirmeleri çalıştırdığından emin olun.

## <a name="create-a-dual-protocol-volume"></a>Çift protokollü birim oluşturma

1.  Kapasite havuzları dikey penceresinden **birimler** dikey penceresine tıklayın. Birim oluşturmak için **+ Birim ekle**'ye tıklayın. 

    ![Birimlere git](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png) 

2.  Birim Oluştur penceresinde **Oluştur**' a tıklayın ve temel bilgiler sekmesinde aşağıdaki alanlar için bilgi sağlayın:   
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

        Belirttiğiniz VNET Azure NetApp Files için bir alt ağa sahip olmalıdır. Azure NetApp Files hizmetine yalnızca aynı VNET 'ten veya VNET eşlemesi ile aynı bölgedeki bir VNET 'ten erişilebilir. Ayrıca, hızlı rota aracılığıyla şirket içi ağınızdan birime da erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Files için temsilci atanmış olmalıdır. 
        
        Bir alt ağ temsilcisi yoksa, birim oluştur sayfasında **Yeni oluştur** ' a tıklayabilirsiniz. Sonra alt ağ oluştur sayfasında alt ağ bilgilerini belirtin ve alt ağın Azure NetApp Files için temsilci olarak **Microsoft. NetApp/birimler** ' i seçin. Her VNET 'te Azure NetApp Files için yalnızca bir alt ağ atanabilir.   
 
        ![Birim oluşturun](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

    * Birime var olan bir anlık görüntü ilkesi uygulamak istiyorsanız, genişletmek için **Gelişmiş bölümünü göster** ' e tıklayın ve açılır menüden bir anlık görüntü ilkesi seçin. 

        Anlık görüntü ilkesi oluşturma hakkında daha fazla bilgi için bkz. [anlık görüntü Ilkelerini yönetme](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies).

        ![Gelişmiş seçimi göster](../media/azure-netapp-files/volume-create-advanced-selection.png)

3. **Protokol**' e tıklayın ve ardından aşağıdaki eylemleri tamamlamayı seçin:  
    * Birimin protokol türü olarak **çift protokol (NFSv3 ve SMB)** seçeneğini belirleyin.   

    * Açılan listeden **Active Directory** bağlantıyı seçin.  
    Kullandığınız Active Directory bir sunucu kök CA sertifikasına sahip olmalıdır. 

    * Birimin **birim yolunu** belirtin.   
    Bu birim yolu, Paylaşılan birimin adıdır. Ad alfabetik bir karakterle başlamalıdır ve her abonelik ve her bölge içinde benzersiz olmalıdır.  

    * Kullanılacak **güvenlik stilini** BELIRTIN: NTFS (varsayılan) veya UNIX.

    * İsteğe bağlı olarak, [birim için dışa aktarma ilkesi yapılandırın](azure-netapp-files-configure-export-policy.md).

    ![Çift protokol belirtin](../media/azure-netapp-files/create-volume-protocol-dual.png)

4. Birim ayrıntılarını gözden geçirmek için **gözden geçir + oluştur** ' a tıklayın. Birimi oluşturmak için **Oluştur** ' a tıklayın.

    Oluşturduğunuz birim birimler sayfasında görünür. 
 
    Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.

## <a name="upload-active-directory-certificate-authority-public-root-certificate"></a>Active Directory Sertifika yetkilisi ortak kök sertifikası yükleme  

1.  Sertifika yetkilisini yüklemek ve yapılandırmak için [sertifika yetkilisini yüklemeyi](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority) izleyin. 

2.  MMC ek bileşenini ve Sertifika Yöneticisi aracını kullanmak için [MMC ek bileşeni ile sertifikaları görüntüleyin](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in) ' i izleyin.  
    Yerel cihaz için kök veya veren sertifikayı bulmak için Sertifika Yöneticisi ek bileşenini kullanın. Sertifika Yönetimi ek bileşeni komutlarını aşağıdaki ayarlardan birinden çalıştırmalısınız:  
    * Etki alanına katılmış ve kök sertifikası yüklü olan Windows tabanlı bir istemci 
    * Kök sertifikayı içeren etki alanındaki başka bir makine  

3. Kök sertifikayı dışarı aktarın.  
    Sertifikanın Base-64 ile kodlanmış X. 509.440 (. CER) biçimi: 

    ![Sertifika Dışarı Aktarma Sihirbazı](../media/azure-netapp-files/certificate-export-wizard.png)

4. Çift protokol biriminin NetApp hesabına gidin, **Active Directory bağlantılar**' a tıklayın ve **Active Directory Birleştir** penceresini kullanarak kök CA sertifikasını karşıya yükleyin:  

    ![Sunucu kök CA sertifikası](../media/azure-netapp-files/server-root-ca-certificate.png)

    Sertifika yetkilisi adının DNS tarafından çözümlenebildiğinden emin olun. Bu ad, sertifikadaki "çıkarılan" veya "veren" alanıdır:  

    ![Sertifika bilgileri](../media/azure-netapp-files/certificate-information.png)

## <a name="manage-ldap-posix-attributes"></a>LDAP POSIX özniteliklerini yönetme

UID, Ana Dizin ve diğer değerler gibi POSIX özniteliklerini, Active Directory Kullanıcıları ve bilgisayarları MMC ek bileşenini kullanarak yönetebilirsiniz.  Aşağıdaki örnek Active Directory öznitelik düzenleyicisini gösterir:  

![Active Directory öznitelik Düzenleyicisi](../media/azure-netapp-files/active-directory-attribute-editor.png) 


## <a name="configure-the-nfs-client"></a>NFS istemcisini yapılandırma 

NFS istemcisini yapılandırmak için [Azure NetApp FILES NFS Istemcisi yapılandırma](configure-nfs-clients.md) konusundaki yönergeleri izleyin.  

## <a name="next-steps"></a>Sonraki adımlar  

* [Çift protokol SSS](azure-netapp-files-faqs.md#dual-protocol-faqs)
* [Azure NetApp Files için NFS istemcisini yapılandırma](configure-nfs-clients.md) 
