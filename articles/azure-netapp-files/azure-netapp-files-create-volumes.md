---
title: Azure NetApp Dosyaları için NFS birimi oluşturma | Microsoft Dokümanlar
description: Azure NetApp Dosyaları için nfs hacminin nasıl oluşturulacak olduğunu açıklar.
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
ms.date: 12/01/2019
ms.author: b-juche
ms.openlocfilehash: 9e8817f802ca1d73ca0f6bfa2b32b1b14b37d7da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274092"
---
# <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Azure NetApp Files için NFS birimi oluşturma

Azure NetApp Files, NFS (NFSv3 ve NFSv4.1) ve SMBv3 birimlerini destekler. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır. Bu makalede, nasıl bir NFS hacmi oluşturmak için gösterir. Bir Kobİ birimi oluşturmak istiyorsanız, Azure [NetApp Dosyaları için Bir Kobİ birimi oluşturma'ya](azure-netapp-files-create-volumes-smb.md)bakın. 

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.   
[Kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md)   
Bir alt ağ Azure NetApp Dosyalarına devredilmelidir.  
[Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)

## <a name="considerations"></a>Dikkat edilmesi gerekenler 

* Hangi NFS sürümünün kullanılacağına karar verme  
  NFSv3 çok çeşitli kullanım servis taleplerini işleyebilir ve çoğu kurumsal uygulamada yaygın olarak dağıtılır. Uygulamanızın hangi sürümü gerektirdiğini (NFSv3 veya NFSv4.1) doğrulamalı ve uygun sürümü kullanarak ses inizi oluşturmalısınız. Örneğin, [Apache ActiveMQ](https://activemq.apache.org/shared-file-system-master-slave)kullanıyorsanız, NFSv4.1 ile dosya kilitleme NFSv3 üzerinde önerilir. 

* Güvenlik  
  NFSv3 ve NFSv4.1 için UNIX mod bitleri (okuma, yazma ve yürütme) desteği mevcuttur. NFS birimlerini monte etmek için NFS istemcisine kök düzeyinde erişim gerekir.

* NFSv4.1 için yerel kullanıcı/grup ve LDAP desteği  
  Şu anda, NFSv4.1 yalnızca birimlere kök erişimi destekler. [Azure NetApp Dosyaları için NFSv4.1 varsayılan etki alanını yapılandırma ya](azure-netapp-files-configure-nfsv41-domain.md)bakın. 

## <a name="best-practice"></a>En iyi yöntem

* Ses seviyesi için uygun montaj yönergelerini kullandığınızdan emin olmalısınız.  [Bkz. Windows veya Linux sanal makineleri için bir ses düzeyini mount veya sökme.](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)

* NFS istemcisi, Azure NetApp Dosyaları hacmiyle aynı VNet'te veya eşlenen VNet'te olmalıdır. VNet dışından bağlanma desteklenir; ancak, ek gecikme sayılacağına ve genel performansı düşürecektir.

* NFS istemcisinin güncel olduğundan ve işletim sistemi için en son güncelleştirmeleri çalıştırdığından emin olmalısınız.

## <a name="create-an-nfs-volume"></a>NFS birimi oluşturma

1.  Kapasite Havuzları bıçağından **Birimler** bıçağını tıklatın. 

    ![Birimlere Git](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Birim Oluştur penceresi görüntülenir.

3.  Birim Oluştur penceresinde, **Oluştur'u** tıklatın ve aşağıdaki alanlar için bilgi sağlayın:   
    * **Birim adı**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Bir birim adı her kapasite havuzu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalı. Herhangi bir alfasayısal karakter kullanabilirsiniz.   

        Birim adı `default` olarak kullanamazsınız.

    * **Kapasite havuzu**  
        Birimin oluşturulmasını istediğiniz kapasite havuzunu belirtin.

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin.  

        Belirttiğiniz Vnet'in Azure NetApp Dosyalarına bir alt ağı olmalıdır. Azure NetApp Files hizmetine yalnızca aynı Vnet'ten veya Vnet'ten ses seviyesiyle aynı bölgede bulunan bir Vnet'ten erişilebilir. Ayrıca, Express Route üzerinden şirket içi ağınızdan ses birimine de erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Dosyalarına devredilmelidir. 
        
        Bir alt ağ atamadıysanız, Birim Oluştur sayfasında **yeni oluştur'u** tıklatabilirsiniz. Ardından Alt Net Oluştur sayfasında alt net bilgilerini belirtin ve Azure NetApp Dosyaları'nın alt netini devretmek için **Microsoft.NetApp/volumes'u** seçin. Her Vnet'te Azure NetApp Dosyalarına yalnızca bir alt ağ devredilebilir.   
 
        ![Birim oluşturun](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. **Protokol'e**tıklayın ve ardından aşağıdaki eylemleri tamamlayın:  
    * Birim için protokol türü olarak **NFS'yi** seçin.   
    * Yeni birim için dışa aktarma yolunu oluşturmak için kullanılacak **dosya yolunu** belirtin. Dışarı aktarma yolu, birimi bağlamak ve birime erişmek için kullanılır.

        Dosya yolu adında yalnızca harfler, sayılar ve kısa çizgiler ("-") bulunabilir. 16 ile 40 karakter arası uzunlukta olmalıdır. 

        Dosya yolu her abonelik ve her bölge içinde benzersiz olmalıdır. 

    * Ses düzeyi için NFSv3 veya **NFSv4.1**sürümünü seçin.**NFSv3**  
    * İsteğe bağlı olarak, [NFS hacmi için dışa aktarma ilkesini yapılandırın.](azure-netapp-files-configure-export-policy.md)

    ![NFS protokolünü belirtin](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. Birim ayrıntılarını gözden geçirmek için **Gözden Geçir + Oluştur'u** tıklatın.  Ardından NFS birimini oluşturmak için **Oluştur'u** tıklatın.

    Oluşturduğunuz birim Birimler sayfasında görünür. 
 
    Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files için NFSv 4.1 varsayılan etki alanını yapılandırma](azure-netapp-files-configure-nfsv41-domain.md)
* [Windows veya Linux sanal makineleri için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [NFS birimine yönelik dışarı aktarma ilkesini yapılandırma](azure-netapp-files-configure-export-policy.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
* [Azure hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
