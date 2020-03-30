---
title: Azure HPC Önbelleği ve Azure NetApp Dosyalarını Kullanma
description: Azure NetApp Dosyaları ile depolanan verilere erişimi geliştirmek için Azure HPC Önbelleği nasıl kullanılır?
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 38f9d0338ce4c47024d670e6d3ee89a97faecc91
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80238689"
---
# <a name="use-azure-hpc-cache-with-azure-netapp-files"></a>Azure NetApp Dosyalarıyla Azure HPC Önbelleğini Kullanma

Azure HPC Önbelleğiniz için [Azure NetApp Dosyalarını](https://azure.microsoft.com/services/netapp/) depolama hedefi olarak kullanabilirsiniz. Bu makalede, iki hizmetin birlikte nasıl çalışabileceği açıklanmaktadır ve bunları ayarlamak için ipuçları verir.

Azure NetApp Files, ONTAP işletim sistemini Microsoft Azure'un ölçeklenebilirliği ve hızıyla birleştirir. Bu kombinasyon, kullanıcıların konumlanan iş akışlarını kodu yeniden yazmadan buluta kaydırmalarına olanak tanır.

Azure HPC Önbellek bileşeni eklemek, birden çok Azure NetApp Dosyası cildini tek bir toplu ad alanında sunarak dosya erişimini artırabilir. Farklı bir hizmet bölgesinde bulunan birimler için kenar önbelleğe alma sağlayabilir. Ayrıca, maliyet tasarrufu sağlamak için daha düşük katman hizmet düzeylerinde oluşturulan birimler için talep üzerine performansı artırabilir.

## <a name="overview"></a>Genel Bakış

Azure HPC Önbelleği ile bir Azure NetApp Files sistemini arka uç depolama alanı olarak kullanmak için bu işlemi izleyin.

1. Azure NetApp Dosyaları sistemini ve birimlerini [sisteminizi](#plan-your-azure-netapp-files-system)planla'daki kılavuza göre oluşturun, aşağıda .
1. Dosya erişimine ihtiyaç duyduğunuz bölgede Azure HPC Önbelleği oluşturun. (Azure [HPC Önbelleği Oluştur'da](hpc-cache-create.md)yönergeleri kullanın.)
1. Azure NetApp Dosyaları birimlerini gösteren önbellekte [depolama hedeflerini tanımlayın.](#create-storage-targets-in-the-cache) Birimlere erişmek için kullanılan her benzersiz IP adresi için bir önbellek depolama hedefi oluşturun.
1. Müşterilerin Azure NetApp Dosyaları birimlerini doğrudan monte etmek yerine [Azure HPC Önbelleğini takmalarını](#mount-storage-targets) sağlar.

## <a name="plan-your-azure-netapp-files-system"></a>Azure NetApp Dosyaları sisteminizi planlayın

Azure NetApp Dosyaları sisteminizi planlarken, Azure HPC Önbelleği ile sorunsuz bir şekilde entegre edebileceğinizden emin olmak için bu bölümdeki öğelere dikkat edin.

Azure HPC Önbelleği ile kullanılmak üzere hacimler oluşturmadan önce [Azure NetApp Dosyaları belgelerini](../azure-netapp-files/index.yml) de okuyun.

### <a name="nfs-client-access-only"></a>Yalnızca NFS istemci erişimi

Azure HPC Önbelleği şu anda yalnızca NFS erişimini destekler. SMB ACL veya POSIX mod bit hacimleri ile kullanılamaz.

### <a name="exclusive-subnet-for-azure-netapp-files"></a>Azure NetApp Dosyaları için özel alt ağ

Azure NetApp Files, birimleri için tek bir temsilci alt net kullanır. Başka hiçbir kaynak bu alt ağı kullanamaz. Ayrıca, Azure NetApp Dosyaları için sanal ağda yalnızca bir alt ağ kullanılabilir. [Azure NetApp Dosyaları ağ planlaması için Yönergeler](../azure-netapp-files/azure-netapp-files-network-topologies.md)hakkında daha fazla bilgi edinin.

### <a name="delegated-subnet-size"></a>Devredilen alt ağ boyutu

Azure HPC Önbelleği ile kullanılmak üzere bir Azure NetApp Files sistemi oluştururken devredilen alt ağ için minimum boyutu kullanın.

Netmask /28 ile belirtilen minimum boyut 16 IP adresi sağlar. Uygulamada, Azure NetApp Files, toplu erişim için kullanılabilir IP adreslerinden yalnızca üçünü kullanır. Bu, tüm birimleri kapsayacak şekilde Azure HPC Önbelleğinizde yalnızca üç depolama hedefi oluşturmanız gerektiği anlamına gelir.

Devredilen alt ağ çok büyükse, Azure NetApp Dosyaları birimlerinin tek bir Azure HPC Önbelleği örneğinin işleyebilirebileceğinden daha fazla IP adresi kullanması mümkündür. Tek bir önbelleğe en fazla on depolama hedefi olabilir.

Azure NetApp Files belgelerinde hızlı başlangıç örneği, devralınan alt ağ için 10.7.0.0/16 kullanır ve bu da çok büyük bir alt net verir.

### <a name="capacity-pool-service-level"></a>Kapasite havuzu hizmet düzeyi

Kapasite havuzunuz için hizmet düzeyini seçerken iş akışınızı göz önünde bulundurun. Verileri sık sık Azure NetApp Files birimine geri yazarsanız, geri yazma süresi yavaşsa önbelleğin performansı kısıtlanabilir. Sık yazacak birimler için yüksek bir hizmet düzeyi seçin.

Düşük hizmet düzeylerine sahip birimler, önbellek içeriği önceden doldururken görevin başlangıcında bazı gecikmeler de gösterebilir. Önbellek iyi bir çalışma dosyaları kümesi yle çalışır duruma geldikten sonra, gecikme fark edilmez hale gelmelidir.

Kapasite havuzu hizmet düzeyini önceden planlamak önemlidir, çünkü oluşturulduktan sonra değiştirilemez. Farklı bir kapasite havuzunda yeni bir birim oluşturulması ve verilerin kopyaedilmesi gerekir.

Bir birimin depolama kotasını ve kapasite havuzunun boyutunu erişimi kesintiye uğratmadan değiştirebileceğinizi unutmayın.

## <a name="create-storage-targets-in-the-cache"></a>Önbellekte depolama hedefleri oluşturma

Azure NetApp Dosyaları sisteminiz ayarlandıktan ve Azure HPC Önbelleği oluşturulduktan sonra, önbellekte dosya sistemi birimlerini işaret eden depolama hedeflerini tanımlayın.

Azure NetApp Dosyaları birimleriniz tarafından kullanılan her IP adresi için bir depolama hedefi oluşturun. IP adresi, birimin montaj talimatları sayfasında listelenir.

Birden çok birim aynı IP adresini paylaşıyorsa, hepsi için tek bir depolama hedefi kullanabilirsiniz.  

Kullanılacak IP adreslerini bulmak için [Azure NetApp Dosyaları belgelerindeki montaj yönergelerini](../azure-netapp-files/azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) izleyin.

Azure CLI ile IP adreslerini de bulabilirsiniz:

```azurecli
az netappfiles volume list -g ${RESOURCE_GROUP} --account-name ${ANF_ACCOUNT} --pool-name ${POOL} --query "[].mountTargets[].ipAddress" | grep -Ee '[0-9]+[.][0-9]+[.][0-9]+[.][0-9]+' | tr -d '"' | tr -d , | sort | uniq
```

Azure NetApp Files sistemindeki dışa aktarma adlarının tek bir yol bileşeni vardır. Azure NetApp Dosyaları'ndaki kök dışa aktarım ``/`` için bir depolama hedefi oluşturmaya çalışmayın, çünkü bu dışa aktarım dosya erişimi sağlamaz.

Bu depolama hedefleri için sanal ad alanı yollarında özel bir kısıtlama yoktur.

## <a name="mount-storage-targets"></a>Montaj depolama hedefleri

İstemci makineleri, Azure NetApp Dosyaları birimlerini doğrudan monte etmek yerine önbelleği monte etmelidir. [Azure HPC Önbelleği Dağı'ndaki](hpc-cache-mount.md)yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure NetApp Dosyalarını](../azure-netapp-files/index.yml) ayarlama ve kullanma hakkında daha fazla bilgi edinin
* Azure NetApp Dosyalarını kullanmak için Azure HPC Önbellek sisteminizi planlama ve ayarlama da yardım için [desteğe başvurun.](hpc-cache-support-ticket.md)
