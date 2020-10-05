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
ms.date: 09/30/2020
ms.author: b-juche
ms.openlocfilehash: beb1cc9957f7b43bc6ad9b9b6ee6c7707eb0c2d6
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91571570"
---
# <a name="whats-new-in-azure-netapp-files"></a>Azure NetApp Files yenilikleri

Azure NetApp Files düzenli olarak güncelleştirilir. Bu makale, en son yeni özellikler ve geliştirmeler hakkında bir Özet sağlar. 

## <a name="september-2020"></a>Eylül 2020

* [Çapraz bölge çoğaltmasını Azure NetApp Files](cross-region-replication-introduction.md) (Genel Önizleme)

  Azure NetApp Files, bölgeler arası çoğaltmayı desteklemektedir. Bu yeni olağanüstü durum kurtarma özelliği sayesinde, Azure NetApp Files birimlerinizi bir Azure bölgesinden diğerine hızlı ve ekonomik bir şekilde çoğaltıp, verileri öngörülebilir bölgesel hatalardan koruyabilirsiniz. Azure NetApp Files çapraz bölge çoğaltma, NetApp anlık görüntü® teknolojisini kullanır; yalnızca değiştirilen bloklar ağ üzerinden sıkıştırılmış, etkin bir biçimde gönderilir. Bu özel teknoloji, bölgeler genelinde çoğaltmak için gereken veri miktarını en aza indirir, bu nedenle veri aktarımı maliyetleri kaydediliyor. Ayrıca çoğaltma süresini kısaltır, bu sayede daha küçük bir geri yükleme noktası hedefi (RPO) elde edebilirsiniz.

* [El Ile QoS kapasite havuzu](manage-manual-qos-capacity-pool.md) (Önizleme)  

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

* [Yedekleme ilkesi kullanıcıları](azure-netapp-files-create-volumes-smb.md#create-an-active-directory-connection) (Önizleme)

    Azure NetApp Files, Azure NetApp Files birlikte kullanılmak üzere oluşturulan bilgisayar hesabına yükseltilmiş ayrıcalıklar gerektiren ek hesaplar eklemenizi sağlar. Belirtilen hesapların dosya veya klasör düzeyinde NTFS izinlerini değiştirmesine izin verilir. Örneğin, Azure NetApp Files bir SMB dosya paylaşımında veri geçirmek için kullanılan ayrıcalıklı olmayan bir hizmet hesabı belirtebilirsiniz. Yedekleme ilkesi kullanıcıları özelliği şu anda önizlemededir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure NetApp Files nedir?](azure-netapp-files-introduction.md)
* [Azure NetApp Files’ın depolama hiyerarşisini anlama](azure-netapp-files-understand-storage-hierarchy.md) 
