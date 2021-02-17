---
title: Azure NetApp Files için bir SMB birimi oluşturun | Microsoft Docs
description: Bu makalede, Azure NetApp Files içinde nasıl SMB3 birimi oluşturacağınız gösterilmektedir. Active Directory bağlantıları ve etki alanı Hizmetleri için gereksinimler hakkında bilgi edinin.
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
ms.date: 02/16/2021
ms.author: b-juche
ms.openlocfilehash: 6bb787084d4396737d3002bb98fe50e283c5764a
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100582889"
---
# <a name="create-an-smb-volume-for-azure-netapp-files"></a>Azure NetApp Files için SMB birimi oluşturma

Azure NetApp Files, NFS (NFSv3 ve NFSv 4.1), SMB3 veya Dual Protocol (NFSv3 ve SMB) kullanarak birim oluşturmayı destekler. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır. Bu makalede, SMB3 birimi oluşturma konusu gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.   
[Kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md)   
Azure NetApp Files için bir alt ağ atanmış olmalıdır.  
[Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)

## <a name="configure-active-directory-connections"></a>Active Directory bağlantıları yapılandırma 

SMB birimi oluşturmadan önce bir Active Directory bağlantısı oluşturmanız gerekir. Azure NetApp dosyaları için Active Directory bağlantıları yapılandırmadıysanız, [Active Directory bağlantıları oluşturma ve yönetme](create-active-directory-connections.md)bölümünde açıklanan yönergeleri izleyin.

## <a name="add-an-smb-volume"></a>SMB birimi ekleme

1. Kapasite havuzları dikey penceresinden **birimler** dikey penceresine tıklayın. 

    ![Birimlere git](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Birim oluştur penceresi görüntülenir.

3. Birim Oluştur penceresinde **Oluştur** ' a tıklayın ve temel bilgiler sekmesinde aşağıdaki alanlar için bilgi sağlayın:   
    * **Birim adı**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Birim adı her bir kapasite havuzu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır. Herhangi bir alfasayısal karakter kullanabilirsiniz.   

        `default` `bin` Birim adı olarak veya kullanamazsınız.

    * **Kapasite havuzu**  
        Birimin oluşturulmasını istediğiniz kapasite havuzunu belirtin.

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Aktarım hızı (MIB/S)**   
        Birim el ile QoS kapasite havuzunda oluşturulduysa, birim için istediğiniz aktarım hızını belirtin.   

        Birim bir otomatik QoS kapasite havuzunda oluşturulduysa, bu alanda görünen değer (Kota x hizmet düzeyi aktarım hızı) olur.   

    * **Sanal ağ**  
        Birime erişmek istediğiniz Azure sanal ağını (VNet) belirtin.  

        Belirttiğiniz VNet Azure NetApp Files için bir alt ağa sahip olmalıdır. Azure NetApp Files hizmetine yalnızca aynı VNet 'ten veya VNet eşlemesi ile aynı bölgedeki bir VNet 'ten erişilebilir. Ayrıca, hızlı rota aracılığıyla şirket içi ağınızdan birime da erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Files için temsilci atanmış olmalıdır. 
        
        Bir alt ağ temsilcisi yoksa, birim oluştur sayfasında **Yeni oluştur** ' a tıklayabilirsiniz. Sonra alt ağ oluştur sayfasında alt ağ bilgilerini belirtin ve alt ağın Azure NetApp Files için temsilci olarak **Microsoft. NetApp/birimler** ' i seçin. Her VNet 'te Azure NetApp Files için yalnızca bir alt ağ atanabilir.   
 
        ![Birim oluşturun](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

    * Birime var olan bir anlık görüntü ilkesi uygulamak istiyorsanız, genişletmek için **Gelişmiş bölümü göster** ' e tıklayın, anlık görüntü yolunu gizlemek isteyip istemediğinizi belirtin ve açılır menüden bir anlık görüntü ilkesi seçin. 

        Anlık görüntü ilkesi oluşturma hakkında daha fazla bilgi için bkz. [anlık görüntü Ilkelerini yönetme](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies).

        ![Gelişmiş seçimi göster](../media/azure-netapp-files/volume-create-advanced-selection.png)

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
* [SMB hakkında SSS](./azure-netapp-files-faqs.md#smb-faqs)
* [SMB veya çift protokol birimlerinde sorun giderme](troubleshoot-dual-protocol-volumes.md)
* [Azure hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](../virtual-network/virtual-network-for-azure-services.md)
* [Azure CLı kullanarak yeni bir Active Directory ormanı yüklemesi](/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm)
