---
title: Azure 'da desteklenen Linux dağıtımları
description: Ubuntu, CentOS, Oracle ve SUSE için yönergeler de dahil olmak üzere, Azure tarafından onaylanan dağıtımlara yönelik Linux hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: gbowerman
manager: gwallace
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: guybo
ms.openlocfilehash: fd21170c4edc1ed0587ea4d4e067e61590530623
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87283325"
---
# <a name="endorsed-linux-distributions-on-azure"></a>Azure 'da desteklenen Linux dağıtımları

İş ortakları, Azure Marketi 'nde Linux görüntüleri sağlar. Microsoft, onaylı dağıtım listesine daha da fazla bilgi eklemek için çeşitli Linux topluluklarıyla birlikte çalışmaktadır. Market 'ten kullanılamayan dağıtımlar için, [Linux işletim sistemini içeren bir sanal sabit disk oluşturma ve karşıya yükleme](./create-upload-generic.md)konusundaki yönergeleri izleyerek her zaman kendi Linux 'u getirebilirsiniz.

## <a name="supported-distributions-and-versions"></a>Desteklenen dağıtımlar ve sürümler

Aşağıdaki tablo, Azure 'da desteklenen Linux dağıtımlarını ve sürümlerini listelemektedir. Azure 'da Linux ve açık kaynaklı teknoloji desteği hakkında daha ayrıntılı bilgi için [Microsoft Azure Içindeki Linux görüntüleri Için destek](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) bölümüne bakın.

Hyper-V ve Azure için Linux Integration Services (LIS) sürücüleri, Microsoft 'un doğrudan yukarı akış Linux çekirdeğine katkıda bulunduğu çekirdek modüllerdir. Bazı LIS sürücüleri, varsayılan olarak dağıtımın çekirdeğine yerleştirilmiştir. Red Hat Enterprise (RHEL)/CentOS tabanlı eski dağıtımlar, [Hyper-V ve Azure Için Linux Integration Services sürüm 4,2](https://www.microsoft.com/download/details.aspx?id=55106)' de ayrı bir indirme olarak sunulmaktadır. LIS sürücüleri hakkında daha fazla bilgi için bkz. [Linux çekirdek gereksinimleri](create-upload-generic.md#linux-kernel-requirements) .

Azure Linux Aracısı, Azure Marketi görüntülerinde önceden yüklenmiş olur ve genellikle dağıtımın paket deposundan kullanılabilir. Kaynak kodu [GitHub](https://github.com/azure/walinuxagent)' da bulunabilir.

| Dağıtım | Sürüm | Sürücüler | Aracı |
| --- | --- | --- | --- |
| Standart dışı bir Wave Software CentOS |CentOS 6. x, 7. x, 8. x |CentOS 6,3: [LIS Download](https://www.microsoft.com/download/details.aspx?id=55106)<p>CentOS 6.4 +: çekirdekte |Paket: "Walınuxagent" altında [Depo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) içinde <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/)<p> CoreOS, 26 Mayıs 2020 itibariyle artık [yaşam süresi sona](https://coreos.com/os/eol/) erecek. |Artık kullanılamıyor | | |
| Credavtiv tarafından ortaya çıkarıldı |8.x, 9.x |Çekirdekte |Paket: "waagent" altında depo Içinde <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
|Kinvolk tarafından yataylı kapsayıcı Linux| Stable, Edge| | |
| Oracle tarafından Oracle Linux |6.x, 7.x, 8.x |Çekirdekte |Paket: "Walınuxagent" altında depo Içinde <br/>Kaynak kodu: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat ile Red Hat Enterprise Linux |6.x, 7.x, 8.x |Çekirdekte |Paket: "Walınuxagent" altında depo Içinde <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise SUSE |SLES/SLES for SAP 11. x, 12. x, 15. x <br/> [SUSE genel bulut görüntüsü yaşam döngüsü](https://www.suse.com/c/suse-public-cloud-image-life-cycle/) |Çekirdekte |Leyebilir<p> Bulutta 11 için [: Araçlar](https://build.opensuse.org/project/show/Cloud:Tools) deposu<br>"genel bulut" modülünde "Python-Azure-Agent" altında bulunan 12 için<br/>Kaynak kodu: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| SUSE tarafından openSUSE |openSUSE Leap 15.x |Çekirdekte |Paket: [bulutta:](https://build.opensuse.org/project/show/Cloud:Tools) "Python-Azure-Agent" ın altındaki araçlar deposu <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Kurallı olarak Ubuntu |Ubuntu Server ve Pro. 16. x, 18. x, 20. x<p>Ubuntu 12,04 ve 14,04 için genişletilmiş destek hakkında bilgi burada bulunabilir: [Ubuntu genişletilmiş güvenlik Bakımı](https://www.ubuntu.com/esm). |Çekirdekte |Paket: "walınuxagent" altında depo Içinde <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |

## <a name="image-update-cadence"></a>Görüntü güncelleştirme temposunda

Azure, onaylı Linux dağıtımlarının yayımcılarının, üç aylık veya daha hızlı bir temposunda en son düzeltme ekleri ve güvenlik düzeltmeleri ile Azure Marketi 'ndeki görüntülerini düzenli olarak güncelleştirilmesini gerektirir. Azure Marketi 'ndeki güncelleştirilmiş görüntüler, bir görüntü SKU 'sunun yeni sürümleri olarak müşteriler tarafından otomatik olarak kullanılabilir. Linux görüntülerini bulma hakkında daha fazla bilgi: [Azure Marketi 'Nde LINUX VM görüntülerini bulun](./cli-ps-findimage.md).

## <a name="azure-tuned-kernels"></a>Azure tarafından ayarlanan çekirdekler

Azure, Azure Market 'Te yayımladıkları görüntüleri iyileştirmek için, çeşitli desteklenen linux dağıtımlarıyla yakından birlikte çalışmaktadır. Bu işbirliğinin bir yönü, Azure platformu için optimize edilmiş ve Linux dağıtımının tamamen desteklenen bileşenleri olarak sunulan "ayarlanmış" Linux çekirdekleri geliştirmektedir. Azure tarafından ayarlanmış çekirdekler yeni özellikler ve performans iyileştirmeleri ve dağıtım tarafından kullanılabilen varsayılan ya da genel çekirdekler ile karşılaştırıldığında daha hızlı (genellikle üç ayda) temposunda.

Çoğu durumda, müşteriler bu iyileştirilmiş çekirdekler için hemen faydalanabilmeleri için, bu çekirdekler 'yi Azure Marketi 'ndeki varsayılan görüntülerde önceden yüklenmiş olarak bulacaksınız. Azure tarafından ayarlanan bu çekirdekler hakkında daha fazla bilgi aşağıdaki bağlantılarda bulunabilir:

- [CentOS Azure tarafından ayarlanmış çekirdek-CentOS sanallaştırma SıG ile kullanılabilir](https://wiki.centos.org/SpecialInterestGroup/Virtualization)
- [Bulut çekirdeğini devre dışı bırak-Azure 'da detem 10 ve detem 9 "backports" görüntüsü ile kullanılabilir](https://wiki.debian.org/Cloud/MicrosoftAzure)
- [SLES Azure tarafından ayarlanan çekirdek](https://www.suse.com/c/a-different-builtin-kernel-for-azure-on-demand-images/)
- [Ubuntu Azure tarafından ayarlanan çekirdek](https://blog.ubuntu.com/2017/09/21/microsoft-and-canonical-increase-velocity-with-azure-tailored-kernel)

## <a name="partners"></a>İş Ortakları

### <a name="coreos"></a>CoreOS

CoreOS, 26 Mayıs 2020 ' e kadar [yaşam bitişi](https://coreos.com/os/eol/) olarak zamanlanır.
Microsoft, CoreOS kullanıcıları için iki (2) geçiş kanalına sahiptir.

- Kinvolk tarafından yataylı ("Kinvolk by Yataykapsayıcı Linux" girdisine bakın.)
- [Fedora çekirdek işletim sistemi](https://docs.fedoraproject.org/en-US/fedora-coreos/provisioning-azure/) (müşteriler kendi görüntüsünü karşıya yüklemelidir. [Geçiş belgeleri](https://docs.fedoraproject.org/en-US/fedora-coreos/migrate-cl/)aşağıda verilmiştir.

### <a name="credativ"></a>Credavtiv

[https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credadtiv, ücretsiz yazılım kullanarak profesyonel çözümlerin geliştirilmesi ve uygulanması konusunda uzmanlaşmış bağımsız bir danışmanlık ve hizmetler şirketidir. Önde gelen açık kaynaklı uzmanlar olarak, Crefetiv, desteğini kullanan pek çok BT departmanının kullanıldığı Uluslararası tanıma sahiptir. Credadtiv, Microsoft ile birlikte, şu anda Demallik 8 (Jesan) ve 7 ' den önce (Wheezy) ilgili Dekamu görüntülerini hazırlıyor. Her iki görüntü de Azure 'da çalışmak üzere özel olarak tasarlanmıştır ve platform aracılığıyla kolayca yönetilebilir. Credavtiv Ayrıca, Azure için açık kaynak destek merkezleri aracılığıyla uzun süreli bakım ve güncelleştirme görüntülerinin güncelleştirilmesini de destekleyecektir.

### <a name="oracle"></a>Oracle

[https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle stratejisi, genel ve özel bulutlara yönelik kapsamlı bir çözüm portföyü sunmamaktadır. Strateji, müşterilere Oracle bulutlarında ve diğer bulutlarda Oracle yazılımı dağıtma konusunda seçim ve esneklik sağlar. Oracle 'ın Microsoft 'un iş ortaklığı, müşterilerin Microsoft ortak ve özel bulutlarda Oracle yazılımlarını, Oracle 'dan sertifika ve destek sağlamak için dağıtmalarını sağlar.  Oracle 'ın, Oracle ortak ve özel bulut çözümlerinde taahhüt ve yatırımı değiştirilmez.

### <a name="red-hat"></a>Red Hat

[https://www.redhat.com/en/partners/strategic-alliance/microsoft](https://www.redhat.com/en/partners/strategic-alliance/microsoft)

Dünya genelindeki açık kaynaklı çözümlerin önde gelen sağlayıcısı, Red Hat Fortune 500 şirketinin %90 ' ünden fazlasına yardımcı olur. iş sorunlarını çözmesine, BT ve iş stratejilerini hizalamasına ve daha sonra teknoloji için hazırlık yapmanıza olanak sağlar. Red Hat Bu, açık iş modeli ve uygun maliyetli, öngörülebilir bir abonelik modeli aracılığıyla güvenli çözümler sunarak bunu yapar.

### <a name="suse"></a>SUSE

[https://www.suse.com/suse-linux-enterprise-server-on-azure](https://www.suse.com/suse-linux-enterprise-server-on-azure)

Azure 'da SUSE Linux Enterprise Server, bulut bilgi işlem için üstün güvenilirlik ve güvenlik sağlayan, kendini kanıtlamış bir platformdur. SUSE 'in çok yönlü Linux platformu, kolayca yönetilebilir bir bulut ortamı sunmak için Azure bulut hizmetleriyle sorunsuz bir şekilde tümleşir. SUSE Linux Enterprise Server için 9.200 ' 1.800 den fazla bağımsız yazılım satıcısından daha fazla sertifikalı uygulama sayesinde, SUSE, veri merkezinde desteklenen çalışan iş yüklerinin Azure 'da güvenle dağıtılmasını sağlar.

### <a name="canonical"></a>Canonical

[https://www.ubuntu.com/cloud/azure](https://www.ubuntu.com/cloud/azure)

Kurallı mühendislik ve açık topluluk idare sürücüsü Ubuntu, müşteriler için kişisel bulut hizmetleri içeren istemci, sunucu ve bulut bilgi işlem başarısı. Ubuntu 'da bulunan Birleşik, ücretsiz bir platformun, telefonlardan buluta kadar, telefon, tablet, TV ve Masaüstü için bir dizi tutarlı arabirim sağlar. Bu vizyon, genel bulut sağlayıcılarından farklı kurumlar için birinci seçimi, tüketici elektronileri ve bireysel teknolojik vericiler arasında bir sık kullanılan olarak sunar.

Dünyanın dört bir yanındaki geliştiriciler ve mühendislik merkezleri sayesinde, yaygın olarak donanım üreticileri, içerik sağlayıcıları ve yazılım geliştiricileri olan iş ortaklarına, bilgisayarlar, sunucular ve avuçiçi cihazlar için de Ubuntu çözümlerini pazara sunma olanağı sağlar.
