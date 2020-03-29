---
title: SSS - Azure için Avere vFXT
description: Azure için Avere vFXT hakkında sık sorulan sorular
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 12/19/2019
ms.author: rohogue
ms.openlocfilehash: 186b2c048a9de42318e4af287393d731a4eb16f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153471"
---
# <a name="avere-vfxt-for-azure-faq"></a>Azure için Avere vFXT hakkında SSS

Bu makale, Azure için Avere vFXT'nin gereksinimleriniz için uygun olup olmadığına karar vermenize yardımcı olabilecek soruları yanıtlar. Avere vFXT hakkında temel bilgiler verir ve diğer Azure bileşenleri yle ve dışarıdan satıcıların ürünleriyle nasıl çalıştığını açıklar.

## <a name="general"></a>Genel

### <a name="what-is-avere-vfxt-for-azure"></a>Azure için Avere vFXT nedir?

Azure için Avere vFXT, kritik iş yüklerinin verimli bir şekilde işlenmesi için Azure bilgi işlemindeki etkin verileri önbelleğe alan yüksek performanslı bir dosya sistemidir.

### <a name="is-avere-vfxt-a-storage-solution"></a>Avere vFXT bir depolama çözümü mü?

Hayır. Azure için Avere vFXT, EMC veya NetApp NAS'ınız veya Azure blob kapsayıcınız gibi depolama ortamlarına takılan bir dosya sistemi *önbelleğidir.* Avere vFXT istemcilerden gelen veri isteklerini kolaylaştırır ve ölçekte ve zaman içinde performansı artırmak için hizmet verdiği verileri önbelleğe alar. Avere vFXT kendisi veri depolamaz. Arkasında depolanan veri miktarı hakkında hiçbir bilgi yoktur.

### <a name="is-avere-vfxt-a-tiering-solution"></a>Avere vFXT bir katmanlama çözümü mü?

Azure için Avere vFXT, verileri sıcak ve havalı katmanlar arasında otomatik olarak katmanlanmaz.  

### <a name="how-do-i-know-if-an-environment-is-right-for-avere-vfxt"></a>Avere vFXT için bir ortamın doğru olup olmadığını nasıl anlarım?

Bu soruyu düşünmenin en iyi yolu, "İş yükü önbelleğe uygun mu?" diye sormaktır. Diğer bir şey, iş yükü yüksek okuma-yazma oranına sahip mi? Bir örnek 80/20 veya 70/30 okur /yazıyor.

Çok sayıda Azure sanal makinesinde çalışan dosya tabanlı bir analitik ardışık hattınız varsa ve aşağıdaki koşullardan birini veya birkaçını karşılıyorsa Azure için Avere vFXT'yi düşünün:

* Uzun dosya erişim süreleri (gereksinimlere bağlı olarak onlarca milisaniye veya saniye) nedeniyle genel performans yavaş veya tutarsızdır. Bu gecikme, müşteri için kabul edilemez.

* İşleme için gereken veriler WAN ortamının en uç noktasında bulunur ve bu verileri kalıcı olarak taşımak pratik değildir. Veriler farklı bir Azure bölgesinde veya bir müşteri veri merkezinde olabilir.

* Önemli sayıda istemci veri talep ediyor - örneğin, yüksek performanslı bir bilgi işlem (HPC) kümesinde. Çok sayıda eşzamanlı istek gecikmeyi artırabilir.

* Müşteri, geçerli ardışık hattını Azure sanal makinelerinde "olduğu gibi" çalıştırmak istiyor ve ölçeklenebilirlik için POSIX tabanlı paylaşılan depolama (veya önbelleğe alma) çözümüne ihtiyaç duyar. Azure için Avere vFXT'yi kullanarak, Azure Blob depolamasına yerel aramalar yapmak için iş ardışık hattını yeniden düzenlemeniz gerekmez.

* HPC uygulamanız NFSv3 istemcilerini temel adamaktadır. (Bazı durumlarda, SMB 2.1 istemcilerini kullanabilir, ancak performans sınırlıdır.)

Aşağıdaki diyagram bu soruya yanıt yardımcı olabilir. İş akışınız sağ üstte ne kadar yakınsa, Azure önbelleğe alma çözümü için Avere vFXT ortamınız için doğru olması o kadar olasıdır.

![Binlerce müşteriile okuma ağır yükleri daha iyi Avere vFXT için uygun olduğunu gösteren grafik diyagramı](media/avere-vfxt-fit-assessment.png)

### <a name="at-what-scale-of-clients-does-the-avere-vfxt-solution-make-the-most-sense"></a>Avere vFXT çözümü hangi ölçekte en mantıklı çözümdür?

Avere vFXT önbellek çözümü yüzlerce, binlerce veya on binlerce işlem çekirdeğini işlemek için oluşturulmuştür. Hafif çalışan birkaç makineniz varsa, Avere vFXT doğru çözüm değildir.

Tipik Avere vFXT müşterileri, yaklaşık 1.000 CPU çekirdeğinden başlayarak zorlu iş yükleri çalıştırın. Bu ortamlar 50.000 veya daha büyük olabilir. Avere vFXT ölçeklenebilir olduğundan, daha fazla iş elde etme veya daha fazla IOPS gerektirecek şekilde büyüdükçe bu iş yüklerini desteklemek için düğümler ekleyebilirsiniz.

### <a name="how-much-data-can-an-avere-vfxt-environment-store"></a>Bir Avere vFXT ortamı ne kadar veri saklayabilir?

Azure için Avere vFXT bir önbellektir. Özellikle veri depolamaz. Önbelleğe alınan verileri depolamak için RAM ve SSD'lerin bir birleşimini kullanır. Veriler kalıcı olarak bir arka uç depolama sisteminde (örneğin, bir NetApp NAS sistemi veya bir blob kapsayıcısı) depolanır. Avere vFXT sisteminin arkasında depolanan veri miktarı hakkında bilgi yoktur. Avere vFXT yalnızca istemcilerin istediği verilerin alt kümesini önbelleğe alar.  

### <a name="what-regions-are-supported"></a>Hangi bölgeler desteklenir?

Azure için Avere vFXT, egemen bölgeler (Çin, Almanya) dışındaki tüm bölgelerde desteklenir. Kullanmak istediğiniz bölgenin büyük miktarda hesaplama çekirdeğini ve Avere vFXT kümesini oluşturmak için gereken VM örneklerini desteklediğinden emin olun.

### <a name="how-do-i-get-help-with-avere-vfxt"></a>Avere vFXT ile nasıl yardım alabilirim?

Azure için Avere vFXT ile ilgili özel bir destek personeli grubu yardım sunar. Azure portalından destek bileti açmak için [sisteminizle ilgili yardım alın'daki](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) yönergeleri izleyin.

### <a name="is-avere-vfxt-highly-available"></a>Avere vFXT son derece kullanılabilir mi?

Evet, Avere vFXT sadece BIR HA çözümü olarak çalışır.

### <a name="does-avere-vfxt-for-azure-also-support-other-cloud-services"></a>Azure için Avere vFXT diğer bulut hizmetlerini de destekliyor mu?

Evet, müşteriler Avere vFXT kümesiile birden fazla bulut sağlayıcısı kullanabilir. AWS S3 standart kovalarını, Google Bulut Hizmetleri standart kovalarını ve Azure blob kapsayıcılarını destekler.

> [!NOTE]
> Avere vFXT'yi AWS veya Google Cloud depolama alanına kullanmak için yazılım ücreti uygulanır. Azure blob depolama alanını kullanmak için ek bir yazılım ücreti yoktur.

## <a name="technical-compute"></a>Teknik: İşlem

### <a name="can-you-describe-what-an-avere-vfxt-environment-looks-like"></a>Avere vFXT ortamının nasıl göründüğünü tarif edebilir misiniz?

Avere vFXT, birden çok Azure sanal makineden oluşan kümelenmiş bir cihazdır. Python kitaplığı küme oluşturma, silme ve değişiklik işler. Azure [için Avere vFXT nedir?](avere-vfxt-overview.md)

### <a name="what-kind-of-azure-virtual-machines-does-avere-vfxt-run-on"></a>Avere vFXT ne tür Azure sanal makineleriyle çalışır?  

Azure kümesi için Bir Avere vFXT, Sanal makineler E32s_v3 Microsoft Azure'u kullanır.

<!-- ### Can I mix and match virtual machine types for my cluster?

No, you must choose one virtual machine type or the other.
    
### Can I move between virtual machine types?

Yes, there is a migration path to move from one VM type to the other. [Open a support ticket](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) to learn how.
-->

### <a name="does-the-avere-vfxt-environment-scale"></a>Avere vFXT ortamı ölçeklendirin mi?

Avere vFXT kümesi üç sanal makine düğümü kadar küçük veya 24 düğüm kadar büyük olabilir. Dokuzdan fazla düğümkümesine ihtiyacınız olduğunu düşünüyorsanız planlama konusunda yardım için Azure teknik desteğine başvurun. Daha fazla sayıda düğüm daha büyük bir dağıtım mimarisi gerektirir.

### <a name="does-the-avere-vfxt-environment-autoscale"></a>Avere vFXT ortamı "otomatik ölçeklendirme" mi?

Hayır. Küme boyutunu yukarı ve aşağı ölçeklendirebilirsiniz, ancak küme düğümleri eklemek veya kaldırmak el ile bir adımdır.

### <a name="can-i-run-the-avere-vfxt-cluster-as-a-virtual-machine-scale-set"></a>Avere vFXT kümesini sanal makine ölçeği kümesi olarak çalıştırabilir miyim?

Avere vFXT sanal makine ölçeği kümesinin dağıtımını desteklemez. Birkaç yerleşik kullanılabilirlik destek mekanizması yalnızca bir kümeye katılan atomik VM'ler için tasarlanmıştır.  

### <a name="can-i-run-the-avere-vfxt-cluster-on-low-priority-vms"></a>Avere vFXT kümesini düşük öncelikli VM'lerde çalıştırabilir miyim?

Hayır, sistem altta yatan kararlı bir sanal makine kümesi gerektirir.

### <a name="can-i-run-the-avere-vfxt-cluster-in-containers"></a>Avere vFXT kümesini kaplarda çalıştırabilir miyim?

Hayır, Avere vFXT bağımsız bir uygulama olarak dağıtılmalıdır.

### <a name="do-the-avere-vfxt-vms-count-against-my-compute-quota"></a>Avere vFXT VM'ler işlem kotama göre mi sayılır?

Evet. Kümeyi desteklemek için bölgede yeterli kotaya sahip olduğundan emin olun.  

### <a name="can-i-run-the-avere-vfxt-cluster-machines-in-different-availability-zones"></a>Avere vFXT küme makinelerini farklı kullanılabilirlik bölgelerinde çalıştırabilir miyim?

Hayır. Avere vFXT'deki yüksek kullanılabilirlik modeli şu anda farklı kullanılabilirlik bölgelerinde bulunan tek tek Avere vFXT küme üyelerini desteklememektedir.

### <a name="can-i-clone-avere-vfxt-virtual-machines"></a>Avere vFXT sanal makinelerini klonlayabilir miyim?

Hayır, Avere vFXT kümesine düğüm eklemek veya kaldırmak için desteklenen Python komut dosyasını kullanmanız gerekir. Daha fazla bilgi için [Avere vFXT kümesini yönet'i](avere-vfxt-manage-cluster.md)okuyun.

### <a name="is-there-a-vm-version-of-the-software-i-can-run-in-my-own-local-environment"></a>Kendi yerel ortamımda çalıştırabileceğim yazılımın "VM" sürümü var mı?

Hayır, sistem kümelenmiş bir cihaz olarak sunulur ve belirli sanal makine türleri üzerinde test edilir. Bu kısıtlama, müşterilerin tipik bir Avere vFXT iş akışının yüksek performans gereksinimlerini destekleyemeyecek bir sistem oluşturmaktan kaçınmalarını sağlar.

## <a name="technical-disks"></a>Teknik: Diskler

### <a name="what-types-of-disks-are-supported-for-the-azure-vms"></a>Azure VM'ler için ne tür diskler desteklenir?

Azure için Avere vFXT 1-TB veya 4-TB premium SSD yapılandırmalarını kullanabilir. Premium SSD yapılandırması birden çok yönetilen disk olarak dağıtılabilir.

### <a name="does-the-cluster-support-unmanaged-disks"></a>Küme yönetilmeyen diskleri destekliyor mu?

Hayır, küme yönetilen diskler gerektirir.

### <a name="does-the-system-support-local-attached-ssds"></a>Sistem yerel (ekli) SSD'leri destekliyor mu?

Azure için Avere vFXT şu anda yerel SSD'leri desteklemiyor. Avere vFXT için kullanılan diskler kapatıp yeniden başlatabilmeli, ancak bu yapılandırmadaki yerel ekli SSD'ler yalnızca sonlandırılabilir.

### <a name="does-the-system-support-ultra-ssds"></a>Sistem ultra SSD'leri destekliyor mu?

Hayır, sistem yalnızca premium SSD yapılandırmalarını destekler.

### <a name="can-i-detach-my-premium-ssds-and-reattach-them-later-to-preserve-cache-contents-between-use"></a>Premium SSD'lerimi ayırıp, önbellek içeriğini kullanım arasında korumak için daha sonra yeniden ekleyebilir miyim?

SSD'leri ayırmak ve yeniden takmak desteklenmez. Kaynaktaki meta veriler veya dosya içeriği, kullanımlar arasında değişmiş olabilir ve bu da veri bütünlüğü sorunlarına neden olabilir.

### <a name="does-the-system-encrypt-the-cache"></a>Sistem önbelleği şifrelir mi?

Veriler diskler arasında şeritlenir, ancak şifrelenmez. Ancak, disklerin kendileri şifrelenebilir. Daha fazla bilgi için [Azure'daki sanal makinelerde Güvenli İlkelere](https://docs.microsoft.com/azure/virtual-machines/linux/security-policy#encryption)bakın ve ilkeleri kullanın.

## <a name="technical-networking"></a>Teknik: Ağ

### <a name="what-network-is-recommended"></a>Hangi ağ önerilir?

Avere vFXT ile şirket içi depolama kullanıyorsanız, depolama alanınız ile küme arasında 1 Gbps veya daha iyi bir ağ bağlantısı olmalıdır. Az miktarda veriniz varsa ve işleri çalıştırmadan önce verileri buluta kopyalamak istiyorsanız, VPN bağlantısı yeterli olabilir.

> [!TIP]
> Ağ bağlantısı ne kadar yavaşsa, ilk "soğuk" okursa o kadar yavaş olur. Yavaş okumalar iş ardışık lığının gecikmesini artırır.

### <a name="can-i-run-avere-vfxt-in-a-different-virtual-network-than-my-compute-cluster"></a>Avere vFXT'yi bilgi işlem kümemden farklı bir sanal ağda çalıştırabilir miyim?

Evet, Avere vFXT sisteminizi farklı bir sanal ağda oluşturabilirsiniz. Ayrıntılar için [Avere vFXT sisteminizi planlayın.](avere-vfxt-deploy-plan.md)

### <a name="does-avere-vfxt-require-its-own-subnet"></a>Avere vFXT kendi alt ağına ihtiyaç duyar mı?

Evet. Avere vFXT kesinlikle yüksek kullanılabilirlik (HA) kümesi olarak çalışır ve çalışması için birden çok IP adresi gerektirir. Küme kendi alt netindeyse, yükleme ve normal çalışma için sorunlara neden olabilecek IP adresi çakışmaları riskini önlersiniz. Kümenin alt ağı, IP adresi çakışma olmadığı sürece diğer kaynaklar tarafından kullanılan sanal ağ içinde olabilir.

### <a name="can-i-run-avere-vfxt-on-infiniband"></a>Avere vFXT'yi InfiniBand'da çalıştırabilir miyim?

Hayır, Avere vFXT yalnızca Ethernet/IP kullanır.

### <a name="how-do-i-access-my-on-premises-nas-environment-from-avere-vfxt"></a>Şirket içi NAS ortamıma Avere vFXT'den nasıl erişebilirim?

Avere vFXT ortamı, bir ağ ağ geçidi veya VPN üzerinden müşteri veri merkezine (ve geri) yönlendirilmiş erişim gerektirdiği diğer Azure VM'leri gibidir. Ortamınızda kullanılabilen Azure ExpressRoute bağlantısını kullanmayı düşünün.

### <a name="what-are-the-bandwidth-requirements-for-avere-vfxt"></a>Avere vFXT için bant genişliği gereksinimleri nelerdir?

Genel bant genişliği gereksinimi iki etkene bağlıdır:

* Kaynaktan istenen veri miktarı
* İstemci sisteminin ilk veri yüklemesi sırasında gecikmeye karşı toleransı  

Gecikmeye duyarlı ortamlar için minimum bağlantı hızı 1 Gbps olan bir fiber çözüm kullanmalısınız. Varsa ExpressRoute'u kullanın.  

### <a name="can-i-run-avere-vfxt-with-public-ip-addresses"></a>Avere vFXT'yi genel IP adresleriyle çalıştırabilir miyim?

Hayır, Avere vFXT en iyi uygulamalarla güvenli bir ağ ortamında işletilmesi içindir.

### <a name="can-i-restrict-internet-access-from-my-clusters-virtual-network"></a>Kümemin sanal ağındaki internet erişimini kısıtlayabilir miyim?

Genel olarak, gerektiğinde sanal ağınızda ek güvenlik yapılandırabilirsiniz, ancak bazı kısıtlamalar kümenin çalışmasını etkileyebilir.

Örneğin, AzureCloud'a erişime açıkça izin veren bir kural eklemediğiniz sürece, sanal ağınızdan giden Internet erişimini kısıtlamak küme için sorunlara neden olur. Bu durum [GitHub ek belgelerde](https://github.com/Azure/Avere/tree/master/src/vfxt/internet_access.md)açıklanmıştır.

Özelleştirilmiş güvenlikle ilgili yardım için, sisteminizle ilgili yardım alın'da açıklandığı gibi desteğe [başvurun.](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt)

## <a name="technical-back-end-storage-core-filers"></a>Teknik: Arka uç depolama (çekirdek filörleri)

### <a name="how-many-core-filers-does-a-single-avere-vfxt-environment-support"></a>Tek bir Avere vFXT ortamı kaç çekirdek filers destek yapar?

Bir Avere vFXT kümesi en fazla 20 çekirdek filerdestekler.

### <a name="how-does-the-avere-vfxt-environment-store-data"></a>Avere vFXT ortamı verileri nasıl saklar?

Avere vFXT depolama değildir. Çekirdek dosyalayıcıları adı verilen birden çok depolama hedefinden gelen verileri okuyan ve yazan bir önbellektir. Avere vFXT'deki premium SSD disklerde depolanan veriler geçicidir ve sonunda arka uç çekirdek filer depolamaalanına atılır.

### <a name="which-core-filers-does-avere-vfxt-support"></a>Avere vFXT hangi çekirdek dosyalayıcılarını destekler?

Genel anlamda, Azure için Avere vFXT aşağıdaki sistemleri çekirdek dosyalayıcılar olarak destekler:

* Dell EMC Isilon (OneFS 7.1, 7.2, 8.0 ve 8.1) 
* NetApp ONTAP (Kümelenmiş Mod 9.4, 9.3, 9.2, 9.1P1, 8.0-8.3) ve (7-Mode 7.*, 8.0-8.3)

* Azure blob kapsayıcıları (yalnızca yerel olarak yedekli depolama)
* AWS S3 kovalar
* Google Bulut kovaları

### <a name="why-doesnt-avere-vfxt-support-all-nfs-filers"></a>Avere vFXT neden tüm NFS dosyalayıcılarını desteklemiyor?

Tüm NFS platformları aynı IETF standartlarını karşılasa da, uygulamada her uygulamanın kendine has tuhaflıkları vardır. Bu ayrıntılar, Avere vFXT'nin depolama sistemiyle nasıl etkileşimde olduğunu etkiler. Desteklenen sistemler pazarda en yaygın olarak kullanılan platformlardır.

### <a name="does-avere-vfxt-support-private-object-storage-such-as-swiftstack"></a>Avere vFXT özel nesne depolama (SwiftStack gibi) destekliyor mu?

Avere vFXT özel nesne depolamayı desteklemez.

### <a name="how-can-i-get-a-specific-storage-product-under-support"></a>Belirli bir depolama ürünlerini destek altında nasıl alabilirim?

Destek, alandaki talep miktarına bağlıdır. Bir NAS çözümünü desteklemek için yeterli gelir eğitimi istek varsa, bunu dikkate alırız. Azure desteği aracılığıyla istekte bulunun.

### <a name="can-i-use-azure-blob-storage-as-a-core-filer"></a>Azure Blob depolama alanını çekirdek filer olarak kullanabilir miyim?

Evet, Azure için Avere vFXT, bulut çekirdek filer olarak bir blok blob kapsayıcı kullanabilirsiniz.

### <a name="what-are-the-storage-account-requirements-for-a-blob-core-filer"></a>Bir blob çekirdek filer için depolama hesabı gereksinimleri nelerdir?

Depolama hesabınız genel amaçlı bir v2 (GPv2) hesabı olmalı ve yalnızca yerel olarak yedekli depolama için yapılandırıldı. Coğrafi yedekli depolama ve bölge yedekli depolama desteklenmez.

Depolama hesabı gereksinimleri hakkında daha fazla bilgi için [Azure Blob Depolama bulut çekirdek filer'ını](avere-vfxt-add-storage.md#azure-blob-storage-cloud-core-filer) okuyun.

### <a name="can-i-use-archive-blob-storage"></a>Arşiv blob depolama kullanabilir miyim?

Hayır. Arşiv depolama için hizmet düzeyi sözleşmesi (SLA), Avere vFXT sisteminin gerçek zamanlı dizin ve dosya erişim gereksinimleriyle uyumlu değildir.

### <a name="can-i-use-cool-blob-storage"></a>Serin blob depolama kullanabilir miyim?

Cool katman blob depolama genellikle Azure çekirdek filer için bir Avere vFXT için tavsiye edilmez. Cool tier daha düşük depolama maliyetleri ancak daha yüksek işlem maliyetleri sunar. (Daha fazla ayrıntı için [blob fiyatlandırması](<https://azure.microsoft.com/pricing/details/storage/blobs/>) engelle'ye bakın.) Verilere sık sık erişilecek ve değiştirilecekveya silinecekse, lütfen Sıcak katmanı kullanmayı düşünün.

[Erişim katmanları,](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers#cool-access-tier) Cool tier depolamasını vFXT çekirdek filer olarak kullanmanın ne zaman mantıklı olabileceği hakkında daha fazla bilgi verir.

### <a name="how-do-i-encrypt-the-blob-container"></a>Blob kabını nasıl şifrelerim?

Blob şifrelemesini Azure 'da (tercih edilen) veya Avere vFXT çekirdek filer düzeyinde yapılandırabilirsiniz.  

### <a name="can-i-use-my-own-encryption-key-for-a-blob-core-filer"></a>Blob core filer için kendi şifreleme anahtarımı kullanabilir miyim?

Varsayılan olarak, veriler Azure Blob, Tablo ve Sıra depolama alanı nın yanı sıra Azure Dosyaları için Microsoft tarafından yönetilen anahtarlarla şifrelenir. Blob depolama ve Azure Dosyaları için şifreleme için kendi anahtarınızı getirebilirsiniz. Avere vFXT şifrelemesini kullanmayı seçerseniz, Avere tarafından oluşturulan anahtarı kullanmalı ve yerel olarak depolamanız gerekir.

## <a name="purchasing"></a>Satın alma

### <a name="how-do-i-get-avere-vfxt-for-azure-licensing"></a>Azure lisanslama için Avere vFXT'yi nasıl edinebilirim?

Azure için Avere vFXT lisansı almak Azure Marketi üzerinden kolaydır. Bir Azure hesabına kaydolun ve ardından [Avere vFXT kümesi oluşturmak için Avere vFXT kümesini dağıt'taki](avere-vfxt-deploy.md) yönergeleri izleyin.

### <a name="how-much-does-avere-vfxt-cost"></a>Avere vFXT'nin maliyeti nedir?

Azure'da, Avere vFXT kümelerini kullanmak için ek bir lisans ücreti yoktur. Müşteriler depolama ve diğer Azure tüketim ücretlerini karşılar.

### <a name="can-avere-vfxt-vms-be-run-as-low-priority"></a>Avere vFXT VM'ler düşük öncelikli olarak çalıştırılabilir mi?

Hayır, Avere vFXT kümeleri "her zaman açık" hizmet gerektirir. Kümeler gerekli olmadığında kapatılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure için Avere vFXT ile başlamak için, kendi sisteminizi nasıl planlayıp dağıtılacağınızı öğrenmek için şu makaleleri okuyun:

* [Avere vFXT sisteminizi planlama](avere-vfxt-deploy-plan.md)
* [Dağıtıma genel bakış](avere-vfxt-deploy-overview.md)
* [Avere vFXT kümesi oluşturmaya hazırlanın](avere-vfxt-prereqs.md)
* [Avere vFXT kümesini dağıtma](avere-vfxt-deploy.md)

Avere vFXT için özellikler ve kullanım örnekleri hakkında daha fazla bilgi edinmek [için Azure için Avere vFXT](https://azure.microsoft.com/services/storage/avere-vfxt/)adresini ziyaret edin.
