---
title: Azure NetApp Files için bir NFS birimi oluşturun | Microsoft Docs
description: Azure NetApp Files için bir NFS birimi oluşturmayı açıklar.
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
ms.date: 7/9/2019
ms.author: b-juche
ms.openlocfilehash: 45164acd89fc9634d6929bafb35e64a5dc9f2b86
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2019
ms.locfileid: "71178228"
---
# <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Azure NetApp Files için NFS birimi oluşturma

Azure NetApp Files NFS ve SMBv3 birimlerini destekler. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır. Bu makalede, bir NFS biriminin nasıl oluşturulacağı gösterilmektedir. SMB birimi oluşturmak istiyorsanız, bkz. [Azure NetApp Files IÇIN SMB birimi oluşturma](azure-netapp-files-create-volumes-smb.md). 

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.   
[Kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md)   
Azure NetApp Files için bir alt ağ atanmış olmalıdır.  
[Azure NetApp Files için bir alt ağ temsilcisi seçme](azure-netapp-files-delegate-subnet.md)

## <a name="create-an-nfs-volume"></a>NFS birimi oluşturma

1.  Kapasite havuzları dikey penceresinden **birimler** dikey penceresine tıklayın. 

    ![Birimlere git](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Birim oluştur penceresi görüntülenir.

3.  Birim Oluştur penceresinde **Oluştur** ' a tıklayın ve aşağıdaki alanlar için bilgi sağlayın:   
    * **Birim adı**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Birim adı her bir kapasite havuzu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır. Herhangi bir alfasayısal karakter kullanabilirsiniz.   

        Birim adı olarak `default` kullanamazsınız.

    * **Kapasite havuzu**  
        Birimin oluşturulmasını istediğiniz kapasite havuzunu belirtin.

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Sanal ağ**  
        Birime hangi Azure sanal ağından (Vnet) erişmek istediğinizi belirtin.  

        Belirttiğiniz VNET Azure NetApp Files için bir alt ağa sahip olmalıdır. Azure NetApp Files hizmetine yalnızca aynı VNET 'ten veya VNET eşlemesi ile aynı bölgedeki bir VNET 'ten erişilebilir. Ayrıca, hızlı rota aracılığıyla şirket içi ağınızdan birime da erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Files için temsilci atanmış olmalıdır. 
        
        Bir alt ağ temsilcisi yoksa, birim oluştur sayfasında **Yeni oluştur** ' a tıklayabilirsiniz. Sonra alt ağ oluştur sayfasında alt ağ bilgilerini belirtin ve alt ağın Azure NetApp Files için temsilci olarak **Microsoft. NetApp/birimler** ' i seçin. Her VNET 'te Azure NetApp Files için yalnızca bir alt ağ atanabilir.   
 
        ![Birim oluştur](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. **Protokol**' e tıklayın ve ardından birimin protokol türü olarak **NFS** ' yi seçin.   
    * Yeni birim için dışarı aktarma yolunu oluşturmak üzere kullanılacak **dosya yolunu** belirtin. Dışarı aktarma yolu, birimi bağlamak ve birime erişmek için kullanılır.

        Dosya yolu adında yalnızca harfler, sayılar ve kısa çizgiler ("-") bulunabilir. 16 ile 40 karakter arası uzunlukta olmalıdır. 

        Dosya yolu her abonelik ve her bölge içinde benzersiz olmalıdır. 

    * İsteğe bağlı olarak, [NFS birimi için dışarı aktarma ilkesini yapılandırın](azure-netapp-files-configure-export-policy.md)

    ![NFS protokolünü belirtin](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. Birim ayrıntılarını gözden geçirmek için **gözden geçir + oluştur** ' a tıklayın.  Ardından, NFS birimini oluşturmak için **Oluştur** ' a tıklayın.

    Oluşturduğunuz birim birimler sayfasında görünür. 
 
    Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar  

* [Windows veya Linux sanal makineleri için bir birimi bağlama veya çıkarma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [NFS Birimi için dışarı aktarma ilkesinin sorunlarını giderme ve çözme](azure-netapp-files-configure-export-policy.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
* [Azure hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
