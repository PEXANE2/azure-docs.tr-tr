---
title: Azure'da onaylanan Linux dağıtımları
description: Ubuntu, CentOS, Oracle ve SUSE yönergeleri de dahil olmak üzere Azure tarafından onaylanan dağıtımlarda Linux hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: gbowerman
manager: gwallace
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 11/04/2019
ms.author: guybo
ms.openlocfilehash: ed7755251feb04a5f811d6ed96b00a347fba8994
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605918"
---
# <a name="endorsed-linux-distributions-on-azure"></a>Azure'da onaylanan Linux dağıtımları
İş ortakları Azure Marketi'nde Linux görüntüleri sağlar. Onaylanan Dağıtım listesine daha da fazla tat eklemek için çeşitli Linux topluluklarıyla birlikte çalışıyoruz. Bu arada, Market'ten kullanılamayan dağıtımlar için, Create'deki yönergeleri izleyerek her zaman kendi Linux'unuzu getirebilir [ve Linux işletim sistemini içeren sanal bir sabit disk yükleyebilirsiniz.](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

## <a name="supported-distributions-and-versions"></a>Desteklenen dağıtımlar ve sürümler
Aşağıdaki tabloda Azure'da desteklenen Linux dağıtımları ve sürümleri listelenir. Azure'da Linux desteği ve açık kaynak teknolojisi hakkında daha ayrıntılı bilgi için [Microsoft Azure'daki Linux görselleri için Destek'e](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) bakın.

Hyper-V ve Azure için Linux Tümleştirme Hizmetleri (LIS) sürücüleri, Microsoft'un doğrudan yukarı linux çekirdeğine katkıda bulunan çekirdek modülleridir.  Bazı LIS sürücüleri varsayılan olarak dağıtım Çekirdeğine yerleşiktir. Red Hat Enterprise (RHEL)/CentOS tabanlı eski dağıtımlar [Hyper-V ve Azure için Linux Entegrasyon Hizmetleri Sürüm 4.2'de](https://www.microsoft.com/download/details.aspx?id=55106)ayrı bir indirme olarak indirilebilir. LIS sürücüleri hakkında daha fazla bilgi için [Linux çekirdeği gereksinimlerine](create-upload-generic.md#linux-kernel-requirements) bakın.

Azure Linux Aracısı Azure Marketi resimlerine önceden yüklenmiş ve genellikle dağıtımın paket deposundan edinilebilir. Kaynak kodu [GitHub](https://github.com/azure/walinuxagent)bulunabilir.


| Dağıtım | Sürüm | Sürücüler | Aracı |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3+, 7.0+, 8.0+ |CentOS 6.3: [LIS indir](https://www.microsoft.com/download/details.aspx?id=55106)<p>CentOS 6.4+: Çekirdek olarak |Paket: "WALinuxAgent" altında [repo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) olarak <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |Çekirdek olarak |Kaynak kodu: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7.9+, 8.2+, 9, 10 |Çekirdek olarak |Paket: "waagent" altında repo olarak <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |Çekirdek olarak |Paket: "WALinuxAgent" altında repo olarak <br/>Kaynak kodu: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 7.1+, 8.0+ |Çekirdek olarak |Paket: "WALinuxAgent" altında repo olarak <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Kurumsal |SAP için SLES/SLES<br>11 SP4<br>12 SP1+<br>15|Çekirdek olarak |Paket:<p> Cloud 11 [için:Araçlar](https://build.opensuse.org/project/show/Cloud:Tools) repo<br>"Python-azure-agent" altında "Genel Bulut" Modülüne dahil edilen 12<br/>Kaynak kodu: [GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE Leap 42.2+ |Çekirdek olarak |Paket: [Bulutta:"python-azure-agent"](https://build.opensuse.org/project/show/Cloud:Tools) altında araçlar repo <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04+ ** <sup>1</sup>** |Çekirdek olarak |Paket: "walinuxagent" altında repo olarak <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |

  - **<sup>Ubuntu</sup>** 12.04 ve 14.04 için genişletilmiş destek hakkında bilgi burada bulabilirsiniz: [Ubuntu Genişletilmiş Güvenlik Bakım](https://www.ubuntu.com/esm).


## <a name="image-update-cadence"></a>Görüntü güncelleme cadence
Azure, onaylanan Linux dağıtımlarının yayıncılarının, azure marketindeki resimlerini üç aylık veya daha hızlı bir şekilde en son düzeltmelerle ve güvenlik düzeltmeleriyle düzenli olarak güncellemelerini gerektirir. Azure Marketi'nde güncelleştirilen görüntüler, bir resim SKU'nun yeni sürümleri olarak müşterilertarafından otomatik olarak kullanılabilir. Linux görsellerini bulma hakkında daha fazla bilgi: [Azure Marketi'nde Linux VM görüntülerini bulun.](https://docs.microsoft.com/azure/virtual-machines/linux/cli-ps-findimage)

### <a name="additional-links"></a>Ek bağlantılar
 - [SUSE Genel Bulut Görüntü Yaşam Döngüsü](https://www.suse.com/c/suse-public-cloud-image-life-cycle/)

## <a name="azure-tuned-kernels"></a>Azure ayarlı çekirdekler

Azure, Azure Marketi'nde yayınladıkları görüntüleri optimize etmek için onaylanan çeşitli Linux dağıtımlarıyla yakın çalışır. Bu işbirliğinin bir yönü, Azure platformu için optimize edilmiş ve Linux dağıtımının tam olarak desteklenen bileşenleri olarak teslim edilen "ayarlanmış" Linux çekirdeklerinin geliştirilmesidir. Azure Ayarlı çekirdekler, dağıtımdan elde edilebilen varsayılan veya genel çekirdeklere kıyasla yeni özellikler ve performans iyileştirmeleri ve daha hızlı (genellikle üç aylık) bir değer sunar.

Çoğu durumda bu çekirdekleri Azure Marketi'ndeki varsayılan görüntülerde önceden yüklenmiş olarak bulursunuz ve böylece Azure müşterileri bu optimize edilmiş çekirdeklerden hemen yararlanır. Bu Azure Ayarlı çekirdekler hakkında daha fazla bilgiyi aşağıdaki bağlantılarda bulabilirsiniz:

 - CentOS Azure Ayarlı Çekirdek - CentOS Sanallaştırma SIG üzerinden kullanılabilir - [Daha Fazla Bilgi](https://wiki.centos.org/SpecialInterestGroup/Virtualization)
 - Debian Cloud Kernel - Azure'da Debian 10 ve Debian 9 "backports" görüntüsüyle kullanılabilir - [Daha Fazla Bilgi](https://wiki.debian.org/Cloud/MicrosoftAzure)
 - SLES Azure Ayarlı Çekirdek - [Daha Fazla Bilgi](https://www.suse.com/c/a-different-builtin-kernel-for-azure-on-demand-images/)
 - Ubuntu Azure-Tuned Çekirdek - [Daha Fazla Bilgi](https://blog.ubuntu.com/2017/09/21/microsoft-and-canonical-increase-velocity-with-azure-tailored-kernel)


## <a name="partners"></a>İş Ortakları

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

CoreOS web sitesinden:

*CoreOS güvenlik, tutarlılık ve güvenilirlik için tasarlanmıştır. CoreOS, yum veya apt üzerinden paket yüklemek yerine, hizmetlerinizi daha yüksek bir soyutlama düzeyinde yönetmek için Linux kaplarını kullanır. Tek bir hizmetin kodu ve tüm bağımlılıklar, bir veya birden çok CoreOS makinesinde çalıştırılabilen bir kapsayıcı içinde paketlenir.*

### <a name="credativ"></a>Credativ
[https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ, özgür yazılım kullanarak profesyonel çözümlerin geliştirilmesi ve uygulanması konusunda uzmanlaşmış bağımsız bir danışmanlık ve hizmet şirketidir. Önde gelen açık kaynak uzmanları olarak Credativ, desteğini kullanan birçok BT departmanıyla uluslararası alanda tanınmaya sahiptir. Microsoft ile birlikte, Credativ şu anda Debian 8 (Jessie) ve Debian 7 (Wheezy) önce karşılık gelen Debian görüntüleri hazırlanıyor. Her iki resim de Azure'da çalışacak şekilde özel olarak tasarlanmıştır ve platform üzerinden kolayca yönetilebilir. Credativ ayrıca Açık Kaynak Destek Merkezleri aracılığıyla Azure için Debian görüntülerinin uzun süreli bakımını ve güncellenmesini destekleyecektir.

### <a name="oracle"></a>Oracle
[https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle'ın stratejisi, kamu ve özel bulutlar için geniş bir çözüm portföyü sunmaktır. Strateji, müşterilere Oracle bulutlarında ve diğer bulutlarda Oracle yazılımını nasıl dağıttıkları konusunda seçenek ve esneklik sağlar. Oracle'ın Microsoft ile ortaklığı, müşterilerin Oracle'ın sertifika ve desteğinin güveniyle Microsoft genel ve özel bulutlarda Oracle yazılımını dağıtmalarına olanak tanır.  Oracle'ın Oracle'ın genel ve özel bulut çözümlerine olan bağlılığı ve yatırımı değişmedi.

### <a name="red-hat"></a>Red Hat
[https://www.redhat.com/en/partners/strategic-alliance/microsoft](https://www.redhat.com/en/partners/strategic-alliance/microsoft)

Dünyanın lider açık kaynak çözümleri sağlayıcısı Red Hat, Fortune 500 şirketlerinin %90'ından fazlasının iş zorluklarını çözmelerine, BT ve iş stratejilerini uyumlu hale getirmeye ve teknolojinin geleceğine hazırlanmalarına yardımcı olur. Red Hat bunu, açık bir iş modeli ve uygun fiyatlı, öngörülebilir abonelik modeli aracılığıyla güvenli çözümler sunarak yapar.

### <a name="suse"></a>SUSE
[https://www.suse.com/suse-linux-enterprise-server-on-azure](https://www.suse.com/suse-linux-enterprise-server-on-azure)

Azure'daki SUSE Linux Enterprise Server, bulut bilgi işlem için üstün güvenilirlik ve güvenlik sağlayan kanıtlanmış bir platformdur. SUSE'nin çok yönlü Linux platformu, kolayca yönetilebilen bir bulut ortamı sunmak için Azure bulut hizmetleriyle sorunsuz bir şekilde entegre olur. SUSE Linux Enterprise Server için 1.800'den fazla bağımsız yazılım satıcısının 9.200'den fazla sertifikalı uygulamasıyla SUSE, veri merkezinde desteklenen iş yüklerinin Azure'da güvenle dağıtılabilmesini sağlar.

### <a name="canonical"></a>Canonical
[https://www.ubuntu.com/cloud/azure](https://www.ubuntu.com/cloud/azure)

Kanonik mühendislik ve açık topluluk yönetimi, Ubuntu'nun tüketiciler için kişisel bulut hizmetlerini içeren istemci, sunucu ve bulut bilgi işlemdeki başarısını yönlendirebilir. Kanoni'nin Ubuntu'da telefondan buluta birleşik ve özgür bir platform vizyonu, telefon, tablet, TV ve masaüstü için tutarlı arayüzler ailesi sağlar. Bu vizyon, Ubuntu'yu kamu bulut sağlayıcılarından tüketici elektroniği üreticilerine kadar çeşitli kurumlar için ilk tercih ve bireysel teknoloji uzmanları arasında favori haline getiriyor.

Dünyanın dört bir yanındaki geliştiriciler ve mühendislik merkezleriyle Canonical, bilgisayar, sunucu ve el cihazları için Ubuntu çözümlerini pazara sunmak için donanım üreticileri, içerik sağlayıcıları ve yazılım geliştiricileri ile iş ortaklığı yapmak üzere benzersiz bir konuma sahiptir.
