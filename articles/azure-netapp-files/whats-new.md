---
title: Azure NetApp Files yenilikleri | Microsoft Docs
description: Azure NetApp Files en son yeni özellikleri ve geliştirmeleri hakkında bir Özet sağlar.
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
ms.topic: overview
ms.date: 03/11/2021
ms.author: b-juche
ms.openlocfilehash: f4019a28e98e06ad4a115e57b2cc9d3fb6f0a807
ms.sourcegitcommit: ba3a4d58a17021a922f763095ddc3cf768b11336
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104802939"
---
# <a name="whats-new-in-azure-netapp-files"></a>Azure NetApp Files yenilikleri

Azure NetApp Files düzenli olarak güncelleştirilir. Bu makale, en son yeni özellikler ve geliştirmeler hakkında bir Özet sağlar. 

## <a name="march-2021"></a>Mart 2021

* SMB sürekli kullanılabilirlik (CA) paylaşımları (Önizleme)  

    SMB saydam yük devretme, SMB birimlerinde verileri depolayan ve bunlara erişen sunucu uygulamalarına bağlantıyı kesintiye uğramadan Azure NetApp Files hizmetinde bakım işlemlerine olanak sağlar. SMB saydam yük devretmeyi desteklemek için Azure NetApp Files artık Azure VM 'lerinde çalışan SMB üzerinde SQL Server uygulamalarla birlikte kullanılacak SMB sürekli kullanılabilirlik paylaşımları seçeneğini desteklemektedir. Bu özellik şu anda Windows SQL Server 'de desteklenmektedir. Linux SQL Server şu anda desteklenmiyor. Bu özelliği etkinleştirmek [, tek örnek, Always-On yük devretme kümesi örneği ve Always-On kullanılabilirlik grubu dağıtımları](azure-netapp-files-solution-architectures.md#sql-server)için önemli SQL Server performans iyileştirmeleri ve ölçek ve maliyet avantajları sağlar. [SQL Server dağıtımı için Azure NetApp Files kullanma avantajlarına](solutions-benefits-azure-netapp-files-sql-server.md)bakın.

* [Çapraz bölge çoğaltma hedef biriminin otomatik olarak yeniden boyutlandırılması](azure-netapp-files-resize-capacity-pools-or-volumes.md#resize-a-cross-region-replication-destination-volume)

    Bir çapraz bölge çoğaltma ilişkisinde, hedef birim, kaynak birimin boyutuna göre otomatik olarak yeniden boyutlandırılır. Bu nedenle, hedef birimi ayrı olarak yeniden boyutlandırmanıza gerek yoktur. Bu otomatik yeniden boyutlandırma davranışı, birimler etkin bir çoğaltma ilişkisinde olduğunda veya çoğaltma eşlemesi yeniden eşitleme işlemiyle kesildiğinde geçerlidir. Bu özelliğin çalışması için, hem kaynak hem de hedef birimler için kapasite havuzlarında yeterli sayıda alan olduğundan emin olmanız gerekir.

## <a name="december-2020"></a>Aralık 2020

* [Azure uygulamayla tutarlı anlık görüntü aracı](azacsnap-introduction.md) (Önizleme)    

    Azure uygulamayla tutarlı anlık görüntü aracı (AzAcSnap), Linux ortamlarında (örneğin, SUSE ve RHEL) üçüncü taraf veritabanları (SAP HANA) için veri korumasını basitleştirmenizi sağlayan bir komut satırı aracıdır.   

    AzAcSnap, Azure NetApp Files ve Azure büyük örneğindeki birim anlık görüntüsünü ve çoğaltma işlevlerini kullanır. Aşağıdaki avantajları sağlar:

    * Uygulamayla tutarlı veri koruma 
    * Veritabanı kataloğu yönetimi 
    * *Geçici birim koruması* 
    * Depolama birimlerinin kopyalanması 
    * Olağanüstü durum kurtarma desteği 

## <a name="november-2020"></a>Kasım 2020

* [Anlık görüntü al](azure-netapp-files-manage-snapshots.md#revert-a-volume-using-snapshot-revert)

    Anlık görüntü alma işlevselliği, bir birimi belirli bir anlık görüntü çekilirken bulunduğu duruma hızlıca döndürmenizi sağlar. Çoğu durumda, bir birimin geri döndürülmesi, tek tek dosyaları bir anlık görüntüden etkin dosya sistemine geri yüklemeden çok daha hızlıdır. Ayrıca, bir anlık görüntünün yeni bir birime geri yüklenmesi ile karşılaştırıldığında daha fazla alan da daha etkilidir.

## <a name="september-2020"></a>Eylül 2020

* [Çapraz bölge çoğaltmasını Azure NetApp Files](cross-region-replication-introduction.md) (Önizleme)

  Azure NetApp Files, bölgeler arası çoğaltmayı desteklemektedir. Bu yeni olağanüstü durum kurtarma özelliği sayesinde, Azure NetApp Files birimlerinizi bir Azure bölgesinden diğerine hızlı ve ekonomik bir şekilde çoğaltıp, verileri öngörülebilir bölgesel hatalardan koruyabilirsiniz. Azure NetApp Files çapraz bölge çoğaltma, NetApp anlık görüntü® teknolojisini kullanır; yalnızca değiştirilen bloklar ağ üzerinden sıkıştırılmış, etkin bir biçimde gönderilir. Bu özel teknoloji, bölgeler genelinde çoğaltmak için gereken veri miktarını en aza indirir, bu nedenle veri aktarımı maliyetleri kaydediliyor. Ayrıca çoğaltma süresini kısaltır, bu sayede daha küçük bir geri yükleme noktası hedefi (RPO) elde edebilirsiniz.

* [El Ile QoS kapasite havuzu](manual-qos-capacity-pool-introduction.md) (Önizleme)  

    El ile QoS kapasite havuzunda, bir birimin kapasitesini ve iş üretimini bağımsız olarak atayabilirsiniz. El ile QoS kapasite havuzu ile oluşturulan tüm birimlerin toplam verimlilik, havuzun toplam verimi ile sınırlıdır. Havuz boyutu ve hizmet düzeyi aktarım hızı birleşimine göre belirlenir. Alternatif olarak, bir kapasite havuzunun [QoS türü](azure-netapp-files-understand-storage-hierarchy.md#qos_types) otomatik (otomatik) olabilir ve varsayılan değer olan. Bir otomatik QoS kapasite havuzunda, işleme, havuzdaki birimlere göre otomatik olarak atanır ve birimlere atanan boyut kotasıyla orantılıdır.

* [LDAP imzalama](azure-netapp-files-create-volumes-smb.md) (Önizleme)   

    Azure NetApp Files artık Azure NetApp Files hizmeti ve Kullanıcı tarafından belirtilen Active Directory Domain Services etki alanı denetleyicileri arasında güvenli LDAP aramaları için LDAP imzalamayı desteklemektedir. Bu özellik şu anda önizleme sürümündedir.

* [Ad kimlik doğrulaması Için AES şifrelemesi](azure-netapp-files-create-volumes-smb.md) (Önizleme)

    Azure NetApp Files, bir SMB birimi için AES şifrelemesini etkinleştirmek üzere DC 'ye LDAP bağlantısı üzerinde AES şifrelemesini desteklemektedir. Bu özellik şu anda önizleme sürümündedir. 

* Yeni [ölçümler](azure-netapp-files-metrics.md):   

    * Yeni birim ölçümleri: 
        * *Ayrılan birim boyutu*: birimin sağlanan boyutu
    * Yeni havuz ölçümleri: 
        * *Havuza ayrılan boyut*: havuzun sağlanan boyutu 
        * *Havuz Için toplam anlık görüntü boyutu*: havuzdaki tüm birimlerden anlık görüntü boyutu toplamı

## <a name="july-2020"></a>Temmuz 2020

* [Çift protokol (NFSv3 ve SMB) birimi](create-volumes-dual-protocol.md)

    Artık, LDAP kullanıcı eşlemesi desteğiyle eşzamanlı çift protokol (NFS v3 ve SMB) erişimine izin veren bir Azure NetApp Files birimi oluşturabilirsiniz. Bu özellik, Azure NetApp Files bir birimde veri üreten ve depolayan Linux tabanlı iş yükünüz olabilecek kullanım örneklerini sunar. Aynı zamanda, çalışanlarınızın aynı Azure NetApp Files birimden yeni oluşturulan verileri çözümlemek için Windows tabanlı istemcileri ve yazılımları kullanması gerekir. Eşzamanlı çift protokol erişimi özelliği, iş yükü tarafından üretilen verileri, analiz sonrası, depolama maliyeti kaydetme ve çalışma süresi için farklı bir protokole sahip ayrı bir birime kopyalama gereksinimini ortadan kaldırır. Bu özellik ücretsizdir (normal [Azure NetApp Files depolama maliyeti](https://azure.microsoft.com/pricing/details/netapp/) hala geçerlidir) ve genel kullanıma sunulmuştur. [Eşzamanlı çift protokol erişimi belgelerinden](create-volumes-dual-protocol.MD)daha fazla bilgi edinin.

* [NFS v 4.1 aktarım sırasında Kerberos şifrelemesi](configure-kerberos-encryption.MD)

    Azure NetApp Files artık, Kerberos modlarında (krb5, krb5i ve krb5p) NFS istemci şifrelemesini, AES-256 şifrelemesi ile destekleyerek ek veri güvenliği sağlar. Bu özellik ücretsizdir (normal [Azure NetApp Files depolama maliyeti](https://azure.microsoft.com/pricing/details/netapp/) hala geçerlidir) ve genel kullanıma sunulmuştur. [NFS v 4.1 Kerberos şifrelemesi belgelerinden](configure-kerberos-encryption.MD)daha fazla bilgi edinin.

* [Dinamik birim hizmet düzeyi değişikliği](dynamic-change-volume-service-level.MD)

    Bulut, BT harcamadan esneklik sağlar. Artık birimi için istediğiniz hizmet düzeyini kullanan başka bir kapasite havuzuna taşıyarak mevcut bir Azure NetApp Files biriminin hizmet düzeyini değiştirebilirsiniz. Birime yönelik bu yerinde hizmet düzeyi değişikliği, veri geçişini gerektirmez. Ayrıca, birime veri düzlemi erişimini etkilemez. Daha iyi performans için, mevcut bir birimi daha yüksek bir hizmet düzeyi kullanacak şekilde değiştirebilir veya maliyet iyileştirmesi için daha düşük bir hizmet düzeyi kullanabilirsiniz. Bu özellik ücretsizdir (normal [Azure NetApp Files depolama maliyeti](https://azure.microsoft.com/pricing/details/netapp/) hala geçerlidir) ve şu anda genel önizleme aşamasındadır. [Dinamik birim hizmet düzeyi değişiklik belgelerini](dynamic-change-volume-service-level.md)izleyerek Özellik önizlemesine kaydolabilirsiniz.

* [Birim anlık görüntü ilkesi](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies) (Önizleme) 

    Azure NetApp Files, birimlerinizin nokta anlık görüntülerini oluşturmanıza olanak sağlar. Artık seçtiğiniz bir sıklıkta birim anlık görüntülerini otomatik olarak oluşturmak Azure NetApp Files için bir anlık görüntü ilkesi oluşturabilirsiniz. Anlık görüntüleri saatlik, günlük, haftalık veya aylık Döngülerde alınmak üzere zamanlayabilirsiniz. Anlık görüntü ilkesinin bir parçası olarak saklanacak en fazla anlık görüntü sayısını da belirtebilirsiniz. Bu özellik ücretsizdir (normal [Azure NetApp Files depolama maliyeti](https://azure.microsoft.com/pricing/details/netapp/) hala geçerlidir) ve şu anda önizleme aşamasındadır. [Toplu anlık görüntü ilkesi belgelerini](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies)izleyerek Özellik önizlemesine kaydolabilirsiniz.

* [NFS kök erişimi dışarı aktarma ilkesi](azure-netapp-files-configure-export-policy.md)

    Azure NetApp Files artık kök hesabın birime erişip erişemeyeceğini belirtmenize olanak tanır. 

* [Anlık görüntü yolunu gizle](azure-netapp-files-manage-snapshots.md#restore-a-file-from-a-snapshot-using-a-client)

    Azure NetApp Files, bir kullanıcının `.snapshot` bağlı bir birimde dizin (NFS istemcileri) veya `~snapshot` klasörü (SMB istemcileri) görüp göremeyeceğini ve erişebileceğini belirtmenize olanak tanır.

## <a name="may-2020"></a>Mayıs 2020

* [Yedekleme ilkesi kullanıcıları](create-active-directory-connections.md) (Önizleme)

    Azure NetApp Files, Azure NetApp Files birlikte kullanılmak üzere oluşturulan bilgisayar hesabına yükseltilmiş ayrıcalıklar gerektiren ek hesaplar eklemenizi sağlar. Belirtilen hesapların dosya veya klasör düzeyinde NTFS izinlerini değiştirmesine izin verilir. Örneğin, Azure NetApp Files bir SMB dosya paylaşımında veri geçirmek için kullanılan ayrıcalıklı olmayan bir hizmet hesabı belirtebilirsiniz. Yedekleme ilkesi kullanıcıları özelliği şu anda önizlemededir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure NetApp Files nedir?](azure-netapp-files-introduction.md)
* [Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md) 
