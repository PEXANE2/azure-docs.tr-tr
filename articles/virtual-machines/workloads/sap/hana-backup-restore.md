---
title: HANA yedekleme ve Geri yükleme SAP HANA Azure (Büyük Örnekler) | Microsoft Dokümanlar
description: AZURE'da HANA yedeklemesi ve geri yükleme sayılma (Büyük Örnekler)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/16/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4384d29811d29f06422802abba5d3eb1ea5737e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72430084"
---
# <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

>[!IMPORTANT]
>Bu makale, SAP HANA yönetim belgeleri veya SAP Notları yerine geçmez. Özellikle yedekleme, geri yükleme, yüksek kullanılabilirlik ve olağanüstü durum kurtarma için SAP HANA yönetimi ve operasyonlarında sağlam bir anlayışa ve uzmanlığa sahip olduğunuzu bekliyoruz. Bu makalede, SAP HANA Studio ekran görüntüleri gösterilir. SAP yönetim araçlarının ve araçların ekranlarının içeriği, yapısı ve doğası SAP HANA sürümünden piyasaya sürülebilir.

Ortamınızda ve HANA sürümleriniz ve sürümleriniz ile atılan adımları ve süreçleri uygulamanız önemlidir. Bu makalede açıklanan bazı işlemler daha iyi bir genel anlayış için basitleştirilmiştir. Bunlar nihai operasyon el kitapları için ayrıntılı adımlar olarak kullanılmak üzere anlamına gelmez. Yapılandırmalarınız için işlem el kitapları oluşturmak istiyorsanız, işlemlerinizi test edin ve çalıştırın ve belirli yapılandırmalarınızla ilgili işlemleri belgelein. 

Veritabanlarını çalıştırmanın en önemli yönlerinden biri, onları felaket olaylardan korumaktır. Bu olayların nedeni basit kullanıcı hataları doğal afetler bir şey olabilir.

Bir veritabanını yedeklemek, örneğin birisi kritik verileri silmeden önce olduğu gibi, zaman içinde herhangi bir noktaya geri yükleme, bozulmadan önceki duruma mümkün olduğunca yakın bir duruma geri yükleme sağlar.

Geri yükleme özelliğini elde etmek için iki tür yedekleme yapılması gerekir:

- Veritabanı yedekleri: Tam, artımlı veya diferansiyel yedeklemeler
- İşlem günlüğü yedeklemeleri

Uygulama düzeyinde gerçekleştirilen tam veritabanı yedeklemelerine ek olarak, depolama anlık görüntüleriyle yedeklemeler gerçekleştirebilirsiniz. Depolama anlık görüntüleri hareket günlüğü yedeklemeleri değiştirmez. İşlem günlüğü yedeklemeleri, veritabanını belirli bir noktaya geri yüklemek veya günlükleri zaten işlenmiş işlemlerden boşaltmak için önemli olmaya devam eder. Depolama anlık görüntüleri, veritabanının hızlı bir şekilde ileri sarma görüntüsünü sağlayarak kurtarmayı hızlandırabilir. 

Azure'daki SAP HANA (Büyük Örnekler) iki yedekleme ve geri yükleme seçeneği sunar:

- **Kendiniz yapın (DIY).** Yeterli disk alanı olduğundan emin olduktan sonra, aşağıdaki disk yedekleme yöntemlerinden birini kullanarak tam veritabanı ve günlük yedeklemeleri gerçekleştirin. Doğrudan HANA Büyük Örnek birimlerine bağlı birimlere veya bir Azure sanal makinesinde (VM) ayarlanan NFS paylaşımlarına yedekleyebilirsiniz. İkinci durumda, müşteriler Azure'da bir Linux VM ayarlar, Azure Depolama'yı VM'ye bağlar ve depolama alanını bu VM'de yapılandırılmış bir NFS sunucusu aracılığıyla paylaşır. Yedeklemeyi HANA Büyük Örnek birimlerine doğrudan iliştirilen birimlere karşı gerçekleştirirseniz, yedeklemeleri bir Azure depolama hesabına kopyalayın. Bunu, Azure Depolama'yı temel alan NFS paylaşımları dışa aktatan bir Azure VM'si ayarladıktan sonra yapın. Azure Yedekleme kasası veya Azure soğuk depolama alanı da kullanabilirsiniz. 

   Başka bir seçenek, yedeklemeleri bir Azure depolama hesabına kopyalandıktan sonra depolamak için bir üçüncü taraf veri koruma aracı kullanmaktır. Diy yedekleme seçeneği, uyumluluk ve denetim amacıyla daha uzun süre saklamanız gereken veriler için de gerekli olabilir. Her durumda, yedeklemeler VM ve Azure Depolama aracılığıyla temsil edilen NFS paylaşımlarına kopyalanır.

- **Altyapı yedekleme ve işlevselliği geri yükleme.** Ayrıca, Azure'daki SAP HANA'nın (Büyük Örnekler) temel altyapısının sağladığı yedekleme ve geri yükleme işlevlerini de kullanabilirsiniz. Bu seçenek, yedekleme ve hızlı geri yükleme gereksinimini karşılar. Bu bölümün geri kalanı, HANA Büyük Örnekleri ile sunulan yedekleme ve geri yükleme işlevlerini ele almaktadır. Bu bölüm ayrıca, HANA Büyük Örnekleri tarafından sunulan olağanüstü durum kurtarma işlevine yedekleme ve geri yükleme ilişkisi de kapsar.

> [!NOTE]
>   HANA Büyük Örnekleri'nin temel altyapısı tarafından kullanılan anlık görüntü teknolojisi SAP HANA anlık görüntülerine bağımlıdır. Bu noktada, SAP HANA anlık görüntüleri SAP HANA çok kiracı veritabanı kapsayıcılarının birden çok kiracı ile birlikte çalışmaz. Yalnızca bir kiracı dağıtılırsa, SAP HANA anlık görüntüleri çalışır ve bu yöntemi kullanabilirsiniz.

## <a name="use-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Azure'da SAP HANA'nın depolama anlık görüntülerini kullanma (Büyük Örnekler)

Azure'da SAP HANA'nın (Büyük Örnekler) altında yatan depolama altyapısı, birimlerin depolama anlık görüntülerini destekler. Birimlerin hem yedekleme hem de geri yükleme si aşağıdaki hususlarla desteklenir:

- Tam veritabanı yedeklemeleri yerine, depolama birimi anlık görüntüleri sık sık alınır.
- /hana/data ve /hana/shared,/usr/sap, hacimleri içeren bir anlık görüntü tetiklendiğinde, anlık görüntü depolama anlık görüntüsünü çalıştırmadan önce bir SAP HANA anlık görüntüsünü başlatır. Bu SAP HANA anlık görüntüsü, depolama anlık görüntüsünün kurtarılmasından sonra nihai günlük geri yüklemeleri için kurulum noktasıdır. Bir HANA anlık görüntüsünün başarılı olması için etkin bir HANA örneğine ihtiyacınız vardır. Bir HSR senaryosunda, hana anlık görüntüsünün gerçekleştirilmediği geçerli bir ikincil düğümde depolama anlık görüntüsü desteklenmez.
- Depolama anlık görüntüsü başarıyla çalıştırıldıktan sonra SAP HANA anlık görüntüsü silinir.
- İşlem günlüğü yedeklemeleri sık sık alınır ve /hana/logbackups biriminde veya Azure'da depolanır. Ayrı bir anlık görüntü almak için hareket günlüğü yedeklemeleri içeren /hana/logbackups birimini tetikleyebilirsiniz. Bu durumda, bir HANA anlık çalıştırmak gerekmez.
- Üretim kesintisi için bir veritabanını belirli bir noktaya geri yüklemeniz gerekiyorsa, Azure'daki Microsoft Azure Desteği veya SAP HANA'nın belirli bir depolama anlık görüntüsüne geri yüklemesini isteyin. Bir örnek, bir kum havuzu sisteminin özgün durumuna planlı bir şekilde geri yüklemesidir.
- Depolama anlık görüntüsüne dahil edilen SAP HANA anlık görüntüsü, depolama anlık görüntüsü alındıktan sonra çalışan ve depolanan hareket günlüğü yedeklemelerini uygulamak için bir mahsup noktasıdır.
- Bu işlem günlüğü yedeklemeleri, veritabanını belirli bir noktaya geri yüklemek için alınır.

Üç birim sınıfını hedefleyen depolama anlık görüntülerini gerçekleştirebilirsiniz:

- /hana/data ve /hana/shared üzerinde /usr/sap içeren birleşik bir anlık görüntü. Bu anlık görüntü, depolama anlık görüntüsü için hazırlık olarak bir SAP HANA anlık oluşturmayı gerektirir. SAP HANA anlık görüntüsü, veritabanının depolama açısından tutarlı bir durumda olmasını sağlar. Geri yükleme işlemi için, bu üzerinde kurmak için bir noktadır.
- /hana/logbackups üzerinde ayrı bir anlık görüntü.
- İşletim sistemi bölümü.

En son anlık görüntü komut dosyalarını ve belgelerini almak için [GitHub'a](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)bakın. [GitHub'dan](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/release.md)anlık görüntü komut dosyası paketini indirdiğinizde, üç dosya alırsınız. Dosyalardan biri, sağlanan işlevsellik için PDF olarak belgelenmiştir. Araç kümesini indirdikten sonra "Anlık görüntü araçlarını alın" yönergelerini izleyin.

## <a name="storage-snapshot-considerations"></a>Depolama anlık görüntü değerlendirmeleri

>[!NOTE]
>Depolama anlık görüntüleri HANA Büyük Örnek birimlerine ayrılan depolama alanını tüketir. Depolama anlık görüntülerini zamanlamanın aşağıdaki yönlerini ve kaç tane depolama anlık görüntüsünü saklayın. 

Azure'daki SAP HANA (Büyük Örnekler) için depolama anlık görüntülerinin belirli mekaniği şunlardır:

- Belirli bir depolama anlık görüntüsünün alındığı zaman diliminde çok az depolama alanı tüketir.
- Veri içeriği değiştikçe ve SAP HANA veri dosyalarındaki içerik depolama hacminde değiştikçe, anlık görüntünün özgün blok içeriğini ve veri değişikliklerini depolaması gerekir.
- Sonuç olarak, depolama anlık boyutu artar. Anlık görüntü ne kadar uzun olursa, depolama anlık görüntüsü o kadar büyük olur.
- Bir depolama anlık görüntüsünün ömrü boyunca SAP HANA veritabanı hacminde ne kadar çok değişiklik yapılırsa, depolama anlık görüntüsünün alan tüketimi de o kadar büyük olur.

Azure'daki SAP HANA (Büyük Örnekler) SAP HANA verileri ve günlük hacimleri için sabit hacim boyutlarıyla birlikte gelir. Bu birimlerin anlık görüntülerini gerçekleştirmek, ses alanınıza yer. Şunları yapmanız gerekir:

- Depolama anlık görüntülerini ne zaman zamanlayışla belirleyin.
- Depolama birimlerinin alan tüketimini izleyin. 
- Depoladığınız anlık görüntü sayısını yönetin. 

Veri yığınlarını içe aktardığınızda veya HANA veritabanında diğer önemli değişiklikleri gerçekleştirdiğinizde depolama anlık görüntülerini devre dışı kullanabilirsiniz. 


Aşağıdaki bölümler, bu anlık görüntüleri gerçekleştirmek için bilgi sağlar ve genel öneriler içerir:

- Donanım birim başına 255 anlık görüntü yenebilse de, bu sayının çok altında kalmak istiyorsunuz. Tavsiye 250 veya daha azdır.
- Depolama anlık görüntülerini gerçekleştirmeden önce boş alanı izleyin ve takip edin.
- Boş alana dayalı depolama anlık görüntü lerinin sayısını azla. Sakladığınız anlık görüntü sayısını düşürebilir veya birimleri uzatabilirsiniz. 1 terabaytlık ünitelerde ek depolama alanı sipariş edebilirsiniz.
- SAP platformu geçiş araçlarıyla (R3load) sap HANA'ya veri taşıma veya SAP HANA veritabanlarını yedeklemelerden geri yükleme gibi etkinlikler sırasında, /hana/veri hacmindeki depolama anlık görüntülerini devre dışı kılabilir. 
- SAP HANA tablolarının daha büyük yeniden yapılanmaları sırasında, mümkünse depolama anlık görüntülerinden kaçının.
- Depolama anlık görüntüleri, SAP HANA'nın Azure'daki olağanüstü durum kurtarma özelliklerinden (Büyük Örnekler) yararlanmanın ön koşuludur.

## <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Self servis depolama anlık görüntülerini kullanmak için ön koşullar

Anlık görüntü komut dosyasının başarılı bir şekilde çalıştığından emin olmak için, Perl'in HANA Large Instances sunucusundaki Linux işletim sistemine yüklü olduğundan emin olun. Perl, HANA Büyük Örnek biriminize önceden yüklenmiş olarak gelir. Perl sürümünü denetlemek için aşağıdaki komutu kullanın:

`perl -v`

![Ortak anahtar bu komutu çalıştırarak kopyalanır](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


## <a name="set-up-storage-snapshots"></a>Depolama anlık görüntülerini ayarlama

HANA Büyük Örnekleri ile depolama anlık görüntülerini ayarlamak için aşağıdaki adımları izleyin.
1. Perl'in HANA Large Instances sunucusundaki Linux işletim sistemine yüklü olduğundan emin olun.
1. /etc/ssh/ssh\_config'i değiştirerek _MACs hmac-sha1_satırını ekleyin.
1. Varsa çalıştırdığınız her SAP HANA örneği için ana düğümde bir SAP HANA yedekleme kullanıcı hesabı oluşturun.
1. SAP HANA HDB istemcisini tüm SAP HANA Büyük Örnekler sunucularına yükleyin.
1. Her bölgenin ilk SAP HANA Büyük Örnekler sunucusunda, anlık görüntü oluşturmayı kontrol eden temel depolama altyapısına erişmek için ortak bir anahtar oluşturun.
1. Komut dosyalarını ve yapılandırma dosyasını [GitHub'dan](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/release.md) SAP HANA yüklemesinde **hdbsql** konumuna kopyalayın.
1. *HANABackupDetails.txt* dosyasını uygun müşteri özellikleri için gerektiği gibi değiştirin.

[GitHub'dan](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/release.md)en son anlık görüntü komut dosyalarını ve belgeleri alın. Daha önce listelenen adımlar [için Azure'da SAP HANA için Microsoft anlık görüntü araçlarına](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)bakın.

### <a name="consideration-for-mcod-scenarios"></a>MCOD senaryoları için göz önünde bulundurulması
Bir HANA Büyük Örnek biriminde birden çok SAP HANA örneği içeren bir [MCOD senaryosu](https://launchpad.support.sap.com/#/notes/1681092) çalıştırıyorsanız, SAP HANA örneklerinin her biri için ayrı depolama birimleri sağlanmış olur. MDC ve diğer hususlar hakkında daha fazla bilgi için, [Azure'daki SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Hatırlanması gereken önemli şeyler" konusuna bakın.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Adım 1: SAP HANA HDB istemcisini yükleyin

Azure'da SAP HANA'ya (Büyük Örnekler) yüklenen Linux işletim sistemi, yedekleme ve olağanüstü durum kurtarma amacıyla SAP HANA depolama anlık görüntülerini çalıştırmak için gereken klasörleri ve komut dosyalarını içerir. [GitHub'da](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/release.md)daha yeni sürümler olup yok. 

SAP HANA'yı yüklerken SAP HANA HDB istemcisini HANA Büyük Örnek birimlerine yüklemek sizin sorumluluğunuzdadır.

### <a name="step-2-change-the-etcsshssh_config"></a>Adım 2: /etc/ssh/ssh\_config'i değiştirin

Bu adım, [Azure'daki SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Depolama ile iletişimi etkinleştir" adlı nda açıklanmıştır.


### <a name="step-3-create-a-public-key"></a>Adım 3: Ortak anahtar oluşturma

HANA Büyük Örnek kiracınızın depolama anlık görüntü arabirimlerine erişimi etkinleştirmek için ortak bir anahtar aracılığıyla oturum açma yordamı belirleyin. 

Kiracınızdaki Azure 'daki ilk SAP HANA (Büyük Örnekler) sunucusunda, depolama altyapısına erişmek için ortak bir anahtar oluşturun. Ortak anahtarla, depolama anlık görüntü arabirimlerinde oturum açmanın bir parolası gerekmez. Ayrıca, ortak bir anahtarla parola kimlik bilgilerini korumanız gerekmez. 

Ortak bir anahtar oluşturmak için Azure'daki [SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Depolama yla iletişimi etkinleştir" bölümüne bakın.


### <a name="step-4-create-an-sap-hana-user-account"></a>Adım 4: SAP HANA kullanıcı hesabı oluşturma

SAP HANA anlık görüntülerini oluşturmaya başlamak için SAP HANA'da depolama anlık görüntü komut dosyalarının kullanabileceği bir kullanıcı hesabı oluşturun. Bu amaçla SAP HANA Studio içinde bir SAP HANA kullanıcı hesabı oluşturun. Kullanıcı SYSTEMDB altında oluşturulmalıdır, MDC için SID veritabanı altında *değil.* Tek kapsayıcı ortamında, kullanıcı kiracı veritabanında oluşturulur. Bu hesabın **Yedekleme Yöneticisi** ve **Katalog Okuma** ayrıcalıkları olmalıdır. 

Bir kullanıcı hesabı kurmak ve kullanmak için [GitHub'da](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"SAP HANA ile iletişimi etkinleştir" bölümüne bakın.


### <a name="step-5-authorize-the-sap-hana-user-account"></a>Adım 5: SAP HANA kullanıcı hesabını yetkilendirme

Bu adımda, oluşturduğunuz SAP HANA kullanıcı hesabını, komut dosyalarının çalışma zamanında parola göndermesine gerek kalmaması için yetkilendirin. SAP HANA `hdbuserstore` komutu, SAP HANA kullanıcı anahtarıoluşturulmasını sağlar. Anahtar bir veya daha fazla SAP HANA düğümünde depolanır. Kullanıcı anahtarı, komut dosyası oluşturma işlemi içinden parolaları yönetmek zorunda kalmadan kullanıcının SAP HANA'ya erişmesini sağlar. Komut dosyası işlemi daha sonra bu makalede ele alınmıştır.

>[!IMPORTANT]
>Bu yapılandırma komutlarını anlık görüntü komutlarının çalıştırıldığı kullanıcı bağlamıyla çalıştırın. Aksi takdirde, anlık görüntü komutları düzgün çalışmaz.


### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Adım 6: Anlık görüntü komut dosyalarını alın, anlık görüntüleri yapılandırın ve yapılandırmayı ve bağlantıyı test edin

Komut dosyalarının en son sürümünü [GitHub'dan](https://github.com/Azure/hana-large-instances-self-service-scripts/tree/master/snapshot_tools_v4.1)indirin. Komut dosyalarının yüklenme şekli, komut dosyalarının 4.1 sürümüyle değiştirildi. Daha fazla bilgi için Azure'daki SAP [HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"SAP HANA ile iletişimi etkinleştir" bölümüne bakın.

Tam komut sırası için, [Azure'daki SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Anlık görüntü araçlarının kolay yüklenmesi (varsayılan)" bölümüne bakın. Varsayılan yüklemenin kullanılmasını öneririz. 

Sürüm 3.x'ten 4.1'e yükseltmek için Azure'daki [SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Varolan bir yüklemeyi yükseltme" konusuna bakın. 4.1 araç kümesini kaldırmak [için, Azure'daki SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Anlık görüntü araçlarının yüklenmesini kaldırma" bölümüne bakın.

[Azure'da SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Anlık görüntü araçlarının tam kurulumu" olarak açıklanan adımları çalıştırmayı unutmayın.

Farklı komut dosyalarının ve dosyaların yüklenirkenki amacı "Bu anlık görüntü araçları nedir?" [Azure'da SAP HANA için Microsoft anlık görüntü araçlarında.](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)

Anlık görüntü araçlarını yapılandırmadan önce, HANA yedekleme konumlarını ve ayarlarını doğru şekilde yapılandırdığınızdan emin olun. Daha fazla bilgi için Azure'daki [SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"SAP HANA Yapılandırması" adlı yapıya bakın.

Anlık görüntü aracı kümesinin yapılandırması, [Azure'daki SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Config dosyası - HANABackupCustomerDetails.txt" olarak açıklanmıştır.

#### <a name="test-connectivity-with-sap-hana"></a>SAP HANA ile bağlantı testi

Tüm yapılandırma verilerini *HANABackupCustomerDetails.txt* dosyasına koyduktan sonra, HANA örnek verileri için yapılandırmaların doğru olup olmadığını kontrol edin. SAP HANA ölçeklendirme veya ölçeklendirme yapılandırmalarından bağımsız olan komut dosyasını `testHANAConnection`kullanın.

Daha fazla bilgi için Azure'daki [SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"SAP HANA ile bağlantıyı kontrol edin - testHANAConnection" adlı bölüme bakın.

#### <a name="test-storage-connectivity"></a>Depolama bağlantısını test edin

Bir sonraki test adımı, *HANABackupCustomerDetails.txt* yapılandırma dosyasına koyduğunuz verilere dayanarak depolamaya bağlantının kontrol etmektir. Ardından bir test anlık görüntüsü çalıştırın. Komutu çalıştırmadan `azure_hana_backup` önce, bu testi çalıştırmalısınız. Bu test için komut sırası için, [Azure'daki SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Depolama SnapshotBağlantısı ile bağlantıyı denetleyin - testStorageSnapshotConnection" bölümüne bakın.

Depolama sanal makine arabirimlerinde başarılı bir oturum açma sonra, komut dosyası aşama 2 ile devam eder ve bir test anlık oluşturma. Çıktı burada SAP HANA'nın üç düğümlü ölçeklendirme yapılandırması için gösterilmiştir.

Test anlık görüntüsü komut dosyasıyla başarılı bir şekilde çalışırsa, gerçek depolama anlık görüntülerini zamanlayabilirsiniz. Başarılı olmazsa, ilerlemeden önce sorunları araştırın. Test anlık görüntüsü, ilk gerçek anlık görüntüler yapılana kadar etrafta kalmalıdır.


### <a name="step-7-perform-snapshots"></a>Adım 7: Anlık görüntü gerçekleştirme

Hazırlama adımları tamamlandığında, gerçek depolama anlık görüntülerini yapılandırmaya ve zamanlamaya başlayabilirsiniz. Zamanlanacak komut dosyası SAP HANA ölçeklendirme ve ölçeklendirme yapılandırmaları ile çalışır. Yedekleme komut dosyasının periyodik ve düzenli olarak yürütülmesi için, cron yardımcı programını kullanarak komut dosyasını zamanlayın. 

Tam komut sözdizimi ve işlevselliği için Azure'daki [SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Anlık görüntü yedeklemesi yap - azure_hana_backup" adlı bölüme bakın. 

Komut dosyası `azure_hana_backup` çalıştığında, aşağıdaki üç aşamada depolama anlık oluşturur:

1. Sap HANA anlık görüntüsünü çalıştırAr.
1. Bir depolama anlık görüntüsü çalıştırın.
1. Depolama anlık görüntüsü çalıştırıldıktan önce oluşturulan SAP HANA anlık görüntüsünü kaldırır.

Komut dosyasını çalıştırmak için, kopyalandığı HDB çalıştırılabilir klasöründen arayın. 

Bekletme süresi, komut dosyasını çalıştırdığınızda parametre olarak gönderilen anlık görüntü sayısıyla yönetilir. Depolama anlık görüntüleri tarafından kapsanan süre yürütme döneminin bir fonksiyonu ve komut dosyası çalıştığında parametre olarak gönderilen anlık görüntü sayısıdır. 

Tutulan anlık görüntü sayısı komut dosyasının çağrısında parametre olarak adlandırılan sayıyı aşıyorsa, aynı etiketin en eski anlık görüntüsü yeni bir anlık görüntü çalıştırmadan önce silinir. Aramanın son parametresi olarak verdiğiniz sayı, tutulan anlık görüntü sayısını denetlemek için kullanabileceğiniz numaradır. Bu numarayla, anlık görüntüler için kullanılan disk alanını dolaylı olarak da denetleyebilirsiniz. 


## <a name="snapshot-strategies"></a>Anlık görüntü stratejileri
Farklı türler için anlık görüntü sıklığı HANA Büyük Örnek olağanüstü durum kurtarma işlevini kullanıp kullanmadığınıza bağlıdır. Bu işlevsellik, depolama anlık görüntülerinin sıklığı ve yürütme süreleri için özel öneriler gerektirebilecek depolama anlık görüntülerine dayanır. 

Aşağıdaki hususlarda ve önerilerde, HANA Büyük Örnekleri'nin sunduğu olağanüstü durum kurtarma işlevini *kullanmadığınız* varsayımı vardır. Bunun yerine, yedeklemeleri olması ve son 30 gün boyunca zaman içinde zaman kurtarma sağlayabilmek için depolama anlık görüntülerini kullanırsınız. Anlık görüntü ve alan sayısının sınırlamaları göz önüne alındığında, aşağıdaki gereksinimleri göz önünde bulundurun:

- Zaman içinde kurtarma için kurtarma süresi.
- Kullanılan alan.
- Kurtarma noktası ve kurtarma süresi, bir felaketten kurtulma potansiyeline yönelik hedeflerdir.
- HANA tam veritabanı yedeklemelerinin disklere karşı nihai olarak yürütülmesi. Disklere veya **arka** arabirime karşı tam veritabanı yedeklemesi yapıldığında, depolama anlık görüntüleri nin yürütülmesi başarısız olur. Depolama anlık görüntülerinin üstünde tam veritabanı yedeklemeleri çalıştırmayı planlıyorsanız, bu süre içinde depolama anlık görüntülerinin yürütülmesinin devre dışı bırakıldığından emin olun.
- Birim başına anlık görüntü sayısı, 250 ile sınırlıdır.

<!-- backint is term for a SAP HANA interface and not a spelling error not spelling errors -->

HANA Büyük Örnekleri'nin olağanüstü durum kurtarma işlevini kullanmazsanız, anlık görüntü süresi daha az sıklıktadır. Bu gibi durumlarda, /hana/data ve /hana/shared,/usr/sap içeren birleşik anlık görüntüleri 12 saatlik veya 24 saatlik periyotlarda gerçekleştirin. Anlık görüntüleri bir ay boyunca saklayın. Aynı durum günlük yedekleme biriminin anlık görüntüleri için de geçerlidir. SAP HANA hareket günlüğü yedeklemelerinin günlük yedekleme hacmine karşı yürütülmesi 5 dakika ila 15 dakikalık dönemlerde gerçekleşir.

Zamanlanmış depolama anlık görüntüleri en iyi cron kullanılarak gerçekleştirilir. Tüm yedeklemeler ve olağanüstü durum kurtarma gereksinimleri için aynı komut dosyasını kullanın. Komut dosyası girişlerini, istenen çeşitli yedekleme sürelerine uyacak şekilde değiştirin. Bu anlık görüntüler, yürütme sürelerine bağlı olarak farklı olarak planlanır. Her 12 saatte bir, günlük veya haftalık olarak saatlik olabilir. 

Aşağıdaki örnekte /etc/crontab'da bir cron zamanlamasını gösterilmektedir:
```
00 1-23 * * * ./azure_hana_backup --type=hana --prefix=hourlyhana --frequency=15min --retention=46
10 00 * * *  ./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
00,05,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
22 12 * * *  ./azure_hana_backup --type=logs --prefix=dailylogback --frequncy=3min --retention=28
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeI --prefix=dailyboot --frequncy=15min --retention=28
```
Önceki örnekte, saatlik birleştirilmiş anlık görüntü/hana/veri ve /hana/shared/SID içeren ve /usr/sap konumlarını içeren birimleri kapsar. Son iki gün içinde daha hızlı bir zaman noktası kurtarma için bu anlık görüntü türünü kullanın. Ayrıca bu ciltlerde günlük bir anlık görüntü var. Yani, saatlik anlık görüntülerle iki günlük kapsama alanınız ve günlük anlık görüntülerle dört haftalık kapsama alanınız vardır. Hareket günlüğü yedekleme hacmi de günlük yedeklenir. Bu yedekler dört hafta saklanır. 

Crontab'ın üçüncü satırında gördüğünüz gibi, HANA hareket günlüğünün yedeklemesi her 5 dakikada bir çalışacak şekilde zamanlanır. Depolama anlık görüntülerini çalıştıran farklı cron işlerinin başlangıç saatleri şaşırtıcıdır. Bu şekilde, anlık görüntüler belirli bir zamanda tek seferde çalışmaz. 

Aşağıdaki örnekte, /hana/veri ve /hana/shared/SID içeren ve /usr/sap konumlarını içeren birimleri kapsayan birleştirilmiş anlık görüntü gerçekleştirebilirsiniz. Bu anlık görüntüleri iki gün saklarsın. Hareket günlüğü yedekleme birimlerinin anlık görüntüleri 5 dakikalık olarak çalışır ve dört saat saklanır. Daha önce olduğu gibi, HANA hareket günlüğü dosyasının yedeklemeher 5 dakikada bir çalışacak şekilde zamanlanır. 

Hareket günlüğü yedekleme biriminin anlık görüntüsü, hareket günlüğü yedeklemesi başladıktan sonra 2 dakikalık bir gecikmeyle gerçekleştirilir. Normal şartlar altında, SAP HANA hareket günlüğü yedeklemesi bu 2 dakika içinde tamamlar. Daha önce olduğu gibi, önyükleme LUN içeren ses bir depolama anlık görüntü tarafından günde bir kez yedeklenir ve dört hafta saklanır.

```
10 0-23 * * * ./azure_hana_backup --type=hana ==prefix=hourlyhana --frequency=15min --retention=48
0,5,10,15,20,25,30,35,40,45,50,55 * * * * ./azure_hana_backup --type=logs --prefix=regularlogback --frequency=3min --retention=28
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup --type=logs --prefix=logback --frequency=3min --retention=48
30 00 * * *  ./azure_hana_backup --type=boot --boottype=TypeII --prefix=dailyboot --frequency=15min --retention=28
```

Aşağıdaki grafik, önceki örneğin dizilerini göstermektedir. ÖNYÜKLEME LUN hariç.

![Yedeklemeler ve anlık görüntüler arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA veritabanında taahhüt edilen değişiklikleri belgelemek için /hana/log birimine karşı düzenli yazılar gerçekleştirir. Sap HANA düzenli olarak /hana/veri hacmine bir tasarruf noktası yazar. Crontab'ta belirtildiği gibi, sap HANA hareket günlüğü yedeklemesi her 5 dakikada bir çalışır. 

Ayrıca, /hana/data ve /hana/shared/SID birimleri üzerinde birleştirilmiş depolama anlık görüntüsünü tetikleme sonucunda bir SAP HANA anlık görüntüsünün her saat çalıştığını da görürsünüz. HANA anlık görüntüsü başarılı olduktan sonra, birleştirilmiş depolama anlık görüntüsü çalışır. Crontab'ta belirtildiği gibi, /hana/logbackup birimindeki depolama görüntüsü HANA işlem günlüğü yedeklemeden yaklaşık 2 dakika sonra her 5 dakikada bir çalışır.

> 

>[!IMPORTANT]
> SAP HANA yedeklemeleri için depolama anlık görüntülerinin kullanımı, yalnızca anlık görüntüler SAP HANA işlem günlüğü yedeklemeleriyle birlikte gerçekleştirildiğinde değerlidir. Bu işlem günlüğü yedeklemelerinin depolama anlık görüntüleri arasındaki zaman aralıklarını kapsaması gerekir. 

30 günlük bir zaman diliminde kurtarma kullanıcısına bir taahhüt tespedseniz, şunları yapmanız gerekir:

- /hana/data ve /hana/shared/SID üzerinden 30 günlük, aşırı durumlarda birleştirilmiş depolama anlık görüntüsüne erişin. 
- Birleştirilmiş depolama anlık görüntülerinden herhangi biri arasındaki zamanı kapsayan bitişik hareket günlüğü yedeklemelerine sahip. Bu nedenle, hareket günlüğü yedekleme biriminin en eski anlık görüntüsünün 30 günlük olması gerekir. İşlem günlüğü yedeklemelerini Azure Depolama'da bulunan başka bir NFS paylaşımına kopyalarsanız durum böyle değildir. Bu durumda, bu NFS paylaşımından eski hareket günlüğü yedeklemelerini çekebilirsiniz.

Depolama anlık görüntülerinden ve işlem günlüğü yedeklemelerinin nihai depolama çoğaltmasından yararlanmak için SAP HANA'nın işlem günlüğü yedeklemelerini yazdığı konumu değiştirin. Bu değişikliği HANA Studio'da yapabilirsiniz. 

SAP HANA tam günlük kesimlerini otomatik olarak yedeklese de, deterministic olmak için bir günlük yedekleme aralığı belirtin. Genellikle deterministik bir dönemle günlük yedeklemelerini çalıştırmak istediğinizden, olağanüstü durum kurtarma seçeneğini kullandığınızda bu özellikle doğrudur. Aşağıdaki durumda, 15 dakika günlük yedekleme aralığı olarak ayarlanır.

![SAP HANA Studio'da SAP HANA yedekleme günlüklerini zamanlama](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Ayrıca, her 15 dakikada bir daha sık olan yedeklemeleri de seçebilirsiniz. Daha sık ayar genellikle HANA Büyük Örnekleri olağanüstü durum kurtarma işlevselliği ile birlikte kullanılır. Bazı müşteriler her 5 dakikada bir işlem günlüğü yedeklemeleri gerçekleştirir.

Veritabanı hiç yedeklenmediyse, son adım, yedekleme kataloğunda bulunması gereken tek bir yedekleme girişi oluşturmak için dosya tabanlı bir veritabanı yedeklemesi gerçekleştirmektir. Aksi takdirde, SAP HANA belirtilen günlük yedeklemelerinizi başlatamaz.

![Tek bir yedekleme girişi oluşturmak için dosya tabanlı yedekleme yapma](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


İlk başarılı depolama anlık görüntüleriniz çalıştırdıktan sonra, adım 6'da çalışan test anlık görüntüsünü silin. Daha fazla bilgi için Azure'daki [SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Test anlık görüntülerini kaldırma - silmeTestStorageSnapshot" adlı bölüme bakın. 


### <a name="monitor-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Disk hacmindeki anlık görüntü sayısını ve boyutunu izleme

Belirli bir depolama biriminde anlık görüntü sayısını ve bu anlık görüntünün depolama tüketimini izleyebilirsiniz. Komut `ls` anlık görüntü dizini veya dosyaları göstermez. Linux OS `du` komutu, aynı birimlerde depolandığı için bu depolama anlık görüntüleriyle ilgili ayrıntıları gösterir. Aşağıdaki seçeneklerle komutu kullanın:

- `du –sh .snapshot`: Bu seçenek, anlık görüntü dizini içindeki tüm anlık görüntülerin toplamını sağlar.
- `du –sh --max-depth=1`: Bu seçenek, **.anlık görüntü** klasöründe kaydedilen tüm anlık görüntüleri ve her anlık görüntünün boyutunu listeler.
- `du –hc`: Bu seçenek, tüm anlık görüntüler tarafından kullanılan toplam boyutu sağlar.

Alınan ve depolanan anlık görüntülerin birimlerdeki tüm depolama alanını tüketmediğinden emin olmak için bu komutları kullanın.

>[!NOTE]
>LUN önyüklemesinin anlık görüntüleri önceki komutlarla görünmez.

### <a name="get-details-of-snapshots"></a>Anlık görüntülerle ilgili ayrıntıları alın
Anlık görüntüler hakkında daha fazla bilgi `azure_hana_snapshot_details`almak için komut dosyasını kullanın. Olağanüstü durum kurtarma konumunda etkin bir sunucu varsa, bu komut dosyasını her iki konumda da çalıştırabilirsiniz. Komut dosyası, anlık görüntüler içeren her bir birim tarafından ayrılmış aşağıdaki çıktıyı sağlar: 
   * Bir birimdeki toplam anlık görüntünün boyutu
   * Bu birimdeki her anlık görüntüdeki aşağıdaki ayrıntılar: 
      - Anlık görüntü adı 
      - Zaman yarat 
      - Anlık görüntünün boyutu
      - Anlık görüntünün sıklığı
      - Bu anlık görüntüyle ilişkili HANA Yedekleme Kimliği, ilgiliyse

Komut ve çıktıların sözdizimi için, [Azure'daki SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Liste anlık görüntüleri - azure_hana_snapshot_details" bölümüne bakın. 



### <a name="reduce-the-number-of-snapshots-on-a-server"></a>Sunucudaki anlık görüntü sayısını azaltın

Daha önce açıklandığı gibi, depoladığınız anlık görüntü etiketlerinin sayısını azaltabilirsiniz. Anlık görüntü başlatmak için komutun son iki parametresi etiket ve saklamak istediğiniz anlık görüntü sayısıdır.

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=28
```

Önceki örnekte, anlık etiket **dailyhana**olduğunu. Bu etiketle birlikte tutulacak anlık görüntü sayısı **28'dir.** Disk alanı tüketimine yanıt vererken, depolanan anlık görüntü sayısını azaltmak isteyebilirsiniz. Örneğin, anlık görüntü sayısını 15'e düşürmenin kolay bir yolu, komut dosyasını son parametre **15'e**ayarlanmış olarak çalıştırmaktır:

```
./azure_hana_backup --type=hana --prefix=dailyhana --frequency=15min --retention=15
```

Komut dosyasını bu ayarda çalıştırıyorsanız, yeni depolama anlık görüntüsünü içeren anlık görüntü sayısı 15'tir. En son 15 anlık görüntü tutulur ve 15 eski anlık görüntü silinir.

 >[!NOTE]
 > Bu komut dosyası, anlık görüntü sayısını yalnızca bir saatten eski anlık görüntüler varsa azaltır. Komut dosyası, bir saatten daha eski anlık görüntüleri silmez. Bu kısıtlamalar, sunulan isteğe bağlı olağanüstü durum kurtarma işleviyle ilgilidir.

Sözdizimi örneklerinde **dailyhana** yedekleme önekiyle anlık görüntü kümesini artık korumak istemiyorsanız, komut dosyasını **bekletme** numarası olarak 0 ile çalıştırın. Bu etiketle eşleşen tüm anlık görüntüler kaldırılır. Tüm anlık görüntüleri kaldırmak HANA Büyük Örnekler olağanüstü durum kurtarma işlevinin yeteneklerini etkileyebilir.

Belirli anlık görüntüleri silmek için ikinci `azure_hana_snapshot_delete`bir seçenek komut dosyası kullanmaktır. Bu komut dosyası, HANA Studio'da bulunan HANA yedekleme kimliğini kullanarak veya anlık görüntü adının kendisi aracılığıyla anlık bir görüntü veya anlık görüntü kümesini silmek üzere tasarlanmıştır. Şu anda, yedek kimlik yalnızca **hana** anlık görüntü türü için oluşturulan anlık görüntülere bağlıdır. Tür **günlükleri** ve **önyükleme** anlık görüntüleri anlık görüntüleri bir SAP HANA anlık görüntü gerçekleştirmez, bu nedenle bu anlık görüntüler için bulunacak yedek kimlik yok. Anlık görüntü adı girilirse, girilen anlık görüntü adıyla eşleşen farklı birimlerdeki tüm anlık görüntüleri arar. 

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->

Komut dosyası hakkında daha fazla bilgi için, [Azure'daki SAP HANA için Microsoft anlık görüntü araçlarında](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Anlık görüntü silme - azure_hana_snapshot_delete" bilgisine bakın.

Komut dosyasını kullanıcı **kökü**olarak çalıştırın.

>[!IMPORTANT]
>Yalnızca silmeyi planladığınız anlık görüntüde var olan veriler varsa, anlık görüntü silindikten sonra, bu veriler sonsuza kadar kaybolur.


## <a name="file-level-restore-from-a-storage-snapshot"></a>Depolama anlık görüntüsünden dosya düzeyinde geri yükleme

<!-- hana, logs and boot are no spelling errors as Acrolinx indicates, but terms of parameter values -->
Anlık görüntü türleri **hana** ve **günlükleri**için, anlık görüntülere doğrudan **.anlık görüntü** dizinindeki birimlerden erişebilirsiniz. Anlık görüntülerin her biri için bir alt dizini var. Anlık görüntü noktasında ki durumu, o alt dizinlinden gerçek dizin yapısına kopyalayın. 

Komut dosyasının geçerli sürümünde, anlık görüntü geri yüklemesi için self servis olarak sağlanan geri yükleme komut dosyası *yoktur.* Anlık geri yükleme, başarısız olma sırasında olağanüstü durum kurtarma alanındaki self servis olağanüstü durum kurtarma komut dosyalarının bir parçası olarak gerçekleştirilebilir. Varolan anlık görüntülerden istenen anlık görüntügeri yüklemek için, bir hizmet isteği açarak Microsoft operasyon ekibiyle iletişim emelisiniz.

>[!NOTE]
>Tek dosya geri yükleme hana Büyük Örnek birimlerinin türünden bağımsız önyükleme LUN anlık görüntüleri için çalışmıyor. **.anlık görüntü** dizini, LUN önyüklemesinde açıkta değil. 
 

## <a name="recover-to-the-most-recent-hana-snapshot"></a>En son HANA anlık görüntüsünü kurtarma

Üretim inme senaryosunda, depolama anlık görüntüden kurtarma işlemi Microsoft Azure Desteği ile bir müşteri olayı olarak başlatılabilir. Verilerin bir üretim sisteminde silinip silinmesi çok acil bir durumdur ve onu geri almanın tek yolu üretim veritabanını geri yüklemektir.

Farklı bir durumda, bir nokta-in-time kurtarma düşük aciliyet ve önceden planlanan gün olabilir. Bu kurtarma yı yüksek öncelikli bir bayrak yükseltmek yerine Azure'da SAP HANA ile planlayabilirsiniz. Örneğin, yeni bir geliştirme paketi uygulayarak SAP yazılımını yükseltmeyi planlayabilirsiniz. Daha sonra geliştirme paketi yükseltmesinden önce durumu temsil eden bir anlık görüntüye geri dönmek gerekir.

İsteği göndermeden önce hazırlanmanız gerekir. Azure ekibindeki SAP HANA daha sonra isteği işleyebilir ve geri yüklenen birimleri sağlayabilir. Daha sonra, anlık görüntülere dayalı OLARAK HANA veritabanını geri yükleyin.

Yeni araç kümesiyle anlık görüntü alma olanakları için, [Azure'daki SAP HANA için El ile kurtarma kılavuzunda bir depolama anlık görüntüsünden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Anlık görüntü nasıl geri yüklenir" başlıklı bölüme bakın.

İsteğe hazırlanmak için aşağıdaki adımları izleyin.

1. Hangi anlık görüntüye geri yüklenececeğine karar verin. Aksi belirtilmedikçe yalnızca hana/veri hacmi geri yüklenir. 

1. HANA örneğini kapatın.

   ![HANA örneğini kapatma](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Her HANA veritabanı düğümündeki veri birimlerini sökün. Veri birimleri hala işletim sistemine monte edilmişse, anlık görüntünün geri yüklemesi başarısız olur.

   ![Her HANA veritabanı düğümündeki veri birimlerini sökme](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Azure destek isteği açın ve belirli bir anlık görüntünün geri yüklemesi ile ilgili yönergeler ekleyin:

   - Geri yükleme sırasında: Azure Hizmetindeki SAP HANA, doğru depolama anlık görüntüsünün geri yüklenmesini koordine etmek, doğrulamak ve onaylamak için bir konferans çağrısına katılmanızı isteyebilir. 

   - Geri yüklemeden sonra: Azure Hizmeti'ndeki SAP HANA, depolama görüntüsü geri yüklendiğinde sizi size onaylar.

1. Geri yükleme işlemi tamamlandıktan sonra, tüm veri hacimlerini yeniden monte edin.

   ![Tüm veri hacimlerini yeniden monte edin](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)



Örneğin, bir depolama anlık görüntüsünden kurtarılan SAP HANA veri dosyalarını almak için başka bir olasılık, [bir depolama anlık görüntüsünden Azure'daki SAP HANA için El ile kurtarma kılavuzunda](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)7.

Anlık görüntü yedeklemesinden geri yüklemek [için, azure'daki SAP HANA için El ile kurtarma kılavuzuna](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)bir depolama anlık görüntüsünden bakın. 

>[!Note]
>Anlık görüntünüz Microsoft işlemleri tarafından geri yüklendiyse, adım 7 yapmanız gerekmez.


### <a name="recover-to-another-point-in-time"></a>Zaman içinde başka bir noktaya kurtarma
Belirli bir noktaya geri yüklemek için, [Azure'daki SAP HANA için El ile kurtarma kılavuzunda bir depolama anlık görüntüsünden](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/latest/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20Guide.md)"Veritabanını zaman içinde aşağıdaki noktaya kurtarın" adlı kitabına bakın. 


## <a name="next-steps"></a>Sonraki adımlar
- Bkz. [Olağanüstü durum kurtarma ilkeleri ve hazırlığı.](hana-concept-preparation.md)
