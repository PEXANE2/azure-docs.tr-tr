---
title: Azure sanal makineler ile kullanılan güvenlik özellikleri-Azure güvenliği | Microsoft Docs
description: Bu makalede, Azure sanal makinelerle kullanılabilecek çekirdek Azure Güvenlik özelliklerine genel bir bakış sunulmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2019
ms.author: terrylan
ms.openlocfilehash: 186a0b0d4025f7d8a8888a9a0d2d0bd8832cf2d9
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726486"
---
# <a name="azure-virtual-machines-security-overview"></a>Azure sanal makineler güvenliğine genel bakış
Bu makale, sanal makinelerle kullanılabilecek çekirdek Azure Güvenlik özelliklerine genel bir bakış sağlar.

Azure sanal makinelerini, çok çeşitli bilgi işlem çözümlerini çevik bir şekilde dağıtmak için kullanabilirsiniz. Hizmet Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP ve Azure BizTalk Services destekler. Böylece, herhangi bir iş yükünü ve herhangi bir dili neredeyse tüm işletim sistemlerinde dağıtabilirsiniz.

Bir Azure sanal makinesi, sanal makineyi çalıştıran fiziksel donanımı satın almanıza ve muhafaza etmenize gerek kalmadan size sanallaştırma esnekliği sunar. Uygulamalarınızın yüksek güvenlikli veri merkezlerinde korunduğu ve güvende olduğundan emin olmak için uygulamalarınızı derleyip dağıtabilirsiniz.

Azure sayesinde, şu şekilde güvenliğe yönelik gelişmiş, uyumlu çözümler oluşturabilirsiniz:

* Sanal makinelerinizi virüslerden ve kötü amaçlı yazılımlardan koruyun.
* Hassas verilerinizi şifreleyin.
* Güvenli ağ trafiği.
* Tehditleri belirleyip algılayın.
* Uyumluluk gereksinimlerini karşılayın.  

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma Yazılımı

Azure ile Microsoft, Symantec, Trend Micro ve Kaspersky gibi güvenlik satıcılarından kötü amaçlı yazılımdan koruma yazılımı kullanabilirsiniz. Bu yazılım, sanal makinelerinizi kötü amaçlı dosyalardan, reklam yazılımlarından ve diğer tehditlerden korumanıza yardımcı olur.

Azure Cloud Services için Microsoft Antimalware ve sanal makineler, virüsler, casus yazılım ve diğer kötü amaçlı yazılımların tanımlanmasına ve kaldırılmasına yardımcı olan gerçek zamanlı bir koruma özelliğidir.  Azure için Microsoft kötü amaçlı yazılımdan koruma, bilinen kötü amaçlı veya istenmeyen yazılımlar kendisini yüklemeyi veya Azure sistemlerinizde çalıştırmayı denediğinde yapılandırılabilir uyarılar sağlar.

Azure için Microsoft kötü amaçlı yazılımdan koruma, uygulamalar ve kiracı ortamları için tek bir aracı çözümüdür. Bu, bir kullanıcı müdahalesi olmadan arka planda çalışacak şekilde tasarlanmıştır. Kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere temel güvenli veya gelişmiş özel yapılandırma ile uygulama iş yüklerinizin ihtiyaçlarına göre koruma dağıtabilirsiniz.

Azure için Microsoft Antimalware 'i dağıtırken ve etkinleştirdiğinizde aşağıdaki temel özellikler kullanılabilir:

* **Gerçek zamanlı koruma**: Kötü amaçlı yazılım yürütmeyi algılamak ve engellemek için Cloud Services ve sanal makinelerde etkinlik izler.
* **Zamanlanmış tarama**: , Etkin olarak çalışan programlar da dahil olmak üzere, kötü amaçlı yazılımları algılamak için hedeflenen taramayı
* **Kötü amaçlı yazılım düzeltmesi**: , Kötü amaçlı dosyaları silme veya karantinaya alma ve kötü amaçlı kayıt defteri girişlerini temizleme gibi algılanan kötü amaçlı yazılımlara otomatik olarak işlem gerçekleştirir.
* **İmza güncelleştirmeleri**: , Korumanın önceden belirlenmiş bir sıklıkta güncel olduğundan emin olmak için en son koruma imzalarını (virüs tanımları) otomatik olarak kurar.
* **Kötü amaçlı yazılımdan koruma altyapısı güncelleştirmeleri**: , Azure için Microsoft kötü amaçlı yazılımdan koruma altyapısını otomatik olarak güncelleştirir.
* **Kötü amaçlı yazılımdan koruma platformu güncelleştirmeleri**: , Azure için Microsoft kötü amaçlı yazılımdan koruma platformunu otomatik olarak güncelleştirir.
* **Etkin koruma**: Hızlı yanıt sağlamak için, algılanan tehditler ve şüpheli kaynaklar hakkında telemetri meta verilerini Azure 'a bildirir. Microsoft etkin koruma sistemi (HARITALAR) aracılığıyla gerçek zamanlı zaman uyumlu imza teslimini mümkün bir şekilde sunar.
* **Örnek raporlama**: Hizmeti iyileştirmenize ve sorun gidermeye yardımcı olmak üzere Azure için Microsoft kötü amaçlı yazılımdan koruma hizmetine örnekler sağlar ve rapor verir.
* **Dışlamaları**: Uygulama ve hizmet yöneticilerinin belirli dosya, işlem ve sürücüleri, performansı ve diğer nedenlerle koruma ve tarama dışında bırakacak şekilde yapılandırmasına izin verir.
* **Kötü amaçlı yazılımdan koruma olayı koleksiyonu**: Kötü amaçlı yazılımdan koruma hizmeti durumunu, şüpheli etkinlikleri ve düzeltme eylemlerini, işletim sistemi olay günlüğünde kaydeder ve bunları Azure depolama hesabınızda toplar.

Sanal makinelerinizi korumaya yardımcı olmak için kötü amaçlı yazılımdan koruma yazılımları hakkında daha fazla bilgi

* [Azure Cloud Services ve sanal makineler için Microsoft kötü amaçlı yazılımdan koruma](antimalware.md)
* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Windows VM 'de bir hizmet olarak Trend mikro derin güvenliği 'ni yüklemek ve yapılandırmak](/azure/virtual-machines/windows/classic/install-trend)
* [Windows VM 'de Symantec Endpoint Protection 'i yüklemek ve yapılandırmak](/azure/virtual-machines/windows/classic/install-symantec)
* [Azure Marketi 'nde güvenlik çözümleri](https://azure.microsoft.com/marketplace/?term=security)

Daha da güçlü koruma için, [Windows Defender Gelişmiş tehdit koruması](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection)'nı kullanmayı göz önünde bulundurun. Windows Defender ATP ile şunları alırsınız:

* [Saldırı yüzeyi azaltma](/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction)  
* [Yeni nesil koruma](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)  
* [Endpoint Protection ve yanıt](/windows/security/threat-protection/windows-defender-atp/overview-endpoint-detection-response)
* [Otomatik araştırma ve düzeltme](/windows/security/threat-protection/windows-defender-atp/automated-investigations-windows-defender-advanced-threat-protection)
* [Güvenli puan](/windows/security/threat-protection/windows-defender-atp/overview-secure-score-windows-defender-advanced-threat-protection)
* [Gelişmiş arama](/windows/security/threat-protection/windows-defender-atp/overview-hunting-windows-defender-advanced-threat-protection)
* [Yönetim ve API 'Ler](/windows/security/threat-protection/windows-defender-atp/management-apis)
* [Microsoft tehdit koruması](/windows/security/threat-protection/windows-defender-atp/threat-protection-integration)

Daha fazla bilgi:

* [WDADTP 'yi kullanmaya başlama](/windows/security/threat-protection/windows-defender-atp/get-started)  
* [WDADTP özelliklerine genel bakış](/windows/security/threat-protection/windows-defender-atp/overview)  

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Anahtar güvenliğini iyileştirmek, şifreleme ve kimlik doğrulama korumalarını geliştirebilir. Kritik gizli dizilerlerinizin ve anahtarların yönetim ve güvenliğini Azure Key Vault içinde depolayarak kolaylaştırabilirsiniz.

Key Vault, anahtarlarınızı FIPS 140-2 2. Düzey standartlarıyla sertifikalanmış olan donanım güvenlik modüllerinde (HSM'ler) depolama seçeneği sunar. Yedekleme veya [Saydam veri şifrelemesi](https://msdn.microsoft.com/library/bb934049.aspx) için SQL Server şifreleme anahtarlarınızın tümü, uygulamalarınızda herhangi bir anahtar veya gizli dizi ile Key Vault depolanabilir. Bu korumalı öğelere izinler ve erişim [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)üzerinden yönetilir.

Daha fazla bilgi:

* [Azure Key Vault nedir?](/azure/key-vault/key-vault-overview)
* [Azure Key Vault blogu](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Sanal makine disk şifrelemesi

Azure disk şifrelemesi, Windows ve Linux sanal makine disklerinizi şifrelemek için yeni bir özelliktir. Azure disk şifrelemesi, işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak üzere Windows 'un sektör standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) özelliğini ve Linux 'un [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğini kullanır.

Çözüm, Anahtar Kasası aboneliğinizdeki disk şifreleme anahtarlarını ve gizli dizileri denetlemenize ve yönetmenize yardımcı olmak için Azure Key Vault ile tümleşiktir. Sanal makine disklerindeki tüm verilerin Azure Storage 'da bekleyen olarak şifrelenmesini sağlar.

Daha fazla bilgi:

* [IaaS VM 'Leri için Azure disk şifrelemesi](/azure/security/azure-security-disk-encryption-overview)
* [Hızlı Başlangıç: Windows IaaS VM 'sini Azure PowerShell ile şifreleme](../azure-disk-encryption-linux-powershell-quickstart.md)

## <a name="virtual-machine-backup"></a>Sanal makine yedekleme

Azure Backup, uygulama verilerinizi sıfır sermaye yatırımı ve en az işletim maliyetleriyle korumanıza yardımcı olan ölçeklenebilir bir çözümdür. Uygulama hataları verilerinizi bozabilir ve insan hataları, uygulamalarınızda hatalar oluşturabilir. Azure Backup, Windows ve Linux çalıştıran sanal makineleriniz korunur.

Daha fazla bilgi:

* [Azure Backup nedir?](/azure/backup/backup-introduction-to-azure-backup)
* [Azure Backup hizmeti SSS](/azure/backup/backup-azure-backup-faq)

## <a name="azure-site-recovery"></a>Azure Site Recovery

Kuruluşunuzun BCDR stratejisinin önemli bir kısmı, planlı ve plansız kesintiler gerçekleştiğinde kurumsal iş yüklerini ve uygulamaları nasıl tutabileceğini öğrenmelidir. Azure Site Recovery, iş yüklerinin ve uygulamaların çoğaltılmasını, yük devretmesini ve kurtarılmasına yardımcı olur, böylece birincil konumunuz kapalıysa ikincil bir konumdan kullanılabilir.

Site Recovery:

* **BCDR stratejinizi basitleştirir**: Site Recovery, tek bir konumdan birden çok iş iş yükünün ve uygulamasının çoğaltma, yük devretme ve kurtarma işlemesini kolaylaştırır. Site Recovery, çoğaltma ve yük devretme işlemlerini düzenler, ancak uygulama verilerinizi engellemez veya onunla ilgili herhangi bir bilgi vermez.
* **Esnek çoğaltma sağlar**: Site Recovery kullanarak, Hyper-V sanal makinelerinde, VMware sanal makinelerinde ve Windows/Linux fiziksel sunucularında çalışan iş yüklerini çoğaltabilirsiniz.
* **Yük devretmeyi ve kurtarmayı destekler**: Site Recovery, üretim ortamlarını etkilemeden olağanüstü durum kurtarma detaylarını desteklemek için test yük devretmeleri sağlar. Ayrıca, beklenen kesintilere yönelik olarak sıfır veri kaybı sunan planlanan yük devretmeler veya beklenmeyen olağanüstü durumlar için minimum düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) planlanmamış yük devretmeler çalıştırabilirsiniz. Yük devretmeden sonra, birincil sitelerinize geri dönebilirsiniz. Yük devretme işlemini özelleştirebilmeniz ve çok katmanlı uygulamaları kurtarabilmeniz için Site Recovery, betikleri ve Azure otomasyonu çalışma kitaplarını içeren kurtarma planları sunar.
* **İkincil veri merkezlerini ortadan kaldırır**: İkincil bir şirket içi siteye veya Azure 'a çoğaltabilirsiniz. Olağanüstü durum kurtarma için Azure 'un bir hedef olarak kullanılması, ikincil bir sitenin korunmasının maliyetini ve karmaşıklığını ortadan kaldırır. Çoğaltılan veriler Azure depolama 'da depolanır.
* **Mevcut BCDR teknolojileriyle tümleştirilir**: Diğer uygulamaların BCDR özelliklerine sahip iş ortakları Site Recovery. Örneğin, kurumsal iş yüklerinin SQL Server arka ucunun korunmasına yardımcı olmak için Site Recovery kullanabilirsiniz. Bu, kullanılabilirlik gruplarının yük devretmesini yönetmek için SQL Server her zaman açık için yerel destek içerir.

Daha fazla bilgi:

* [Azure Site Recovery nedir?](/azure/site-recovery/site-recovery-overview)
* [Azure Site Recovery nasıl çalışır?](/azure/site-recovery/site-recovery-components)
* [Hangi iş yükleri Azure Site Recovery korumalıdır?](/azure/site-recovery/site-recovery-workload)

## <a name="virtual-networking"></a>Sanal ağ

Sanal makinelerin ağ bağlantısı olması gerekir. Azure, bu gereksinimi desteklemek için sanal makinelerin bir Azure sanal ağına bağlanmasını gerektirir.

Azure sanal ağı, fiziksel Azure ağ dokusunun üzerine oluşturulan mantıksal bir yapıdır. Her mantıksal Azure sanal ağı, diğer tüm Azure sanal ağlarından yalıtılmıştır. Bu yalıtım, dağıtımlarınızdaki ağ trafiğine diğer Microsoft Azure müşterilerin erişimine açık olmadığından emin olmanıza yardımcı olur.

Daha fazla bilgi:

* [Azure ağ güvenliğine genel bakış](network-overview.md)
* [Sanal Ağ’a genel bakış](/azure/virtual-network/virtual-networks-overview)
* [Kurumsal senaryolar için ağ özellikleri ve ortaklıkları](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Güvenlik İlkesi Yönetimi ve raporlama

Azure Güvenlik Merkezi, tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Güvenlik Merkezi, Azure kaynaklarınızın güvenliğini ve üzerinde daha fazla görünürlük elde etmenizi sağlar. Azure abonelikleriniz genelinde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Başka türlü fark edilmemiş tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş bir ekosistemiyle birlikte çalışabilir.

Güvenlik Merkezi, sanal makinelerinizin güvenliğini en uygun hale getirmenize ve izlemenize yardımcı olur:

* Sanal makineler için [güvenlik önerileri](/azure/security-center/security-center-recommendations) sağlama. Örnek öneriler şunlardır: sistem güncelleştirmelerini uygulama, ACL uç noktalarını yapılandırma, kötü amaçlı yazılımdan koruma, ağ güvenlik gruplarını etkinleştirme ve disk şifrelemeyi uygulama.
* Sanal makinelerinizin durumunu izleme.

Daha fazla bilgi:

* [Azure Güvenlik Merkezi'ne Giriş](/azure/security-center/security-center-intro)
* [Azure Güvenlik Merkezi hakkında sık sorulan sorular](/azure/security-center/security-center-faq)
* [Azure Güvenlik Merkezi planlama ve işlemler](/azure/security-center/security-center-planning-and-operations-guide)

## <a name="compliance"></a>Uyumluluk

Azure sanal makineleri, FıMMA, Fedrampa, HIPAA, PCI DSS Level 1 ve diğer önemli uyumluluk programları için sertifikalandırilmiştir. Bu sertifika, kendi Azure uygulamalarınızın uyumluluk gereksinimlerini karşılamasını ve işletmenizin çok çeşitli yurtiçi ve uluslararası mevzuata gereksinimlerini ele abilmesini kolaylaştırır.

Daha fazla bilgi:

* [Microsoft Güven Merkezi: Uyumluluk](https://www.microsoft.com/en-us/trustcenter/compliance)
* [Güvenilir bulut: Güvenlik, gizlilik ve uyumluluk Microsoft Azure](https://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)

## <a name="confidential-computing"></a>Gizli Bilgi Işlem

Gizli bilgi işlem, sanal makine güvenliğinin teknik bir parçası olmadığından, sanal makine güvenliği konusu "işlem" güvenliğinin üst düzey konusuna aittir. Gizli bilgi işlem, "işlem" güvenliği kategorisinde yer alıyor.

Gizli bilgi işlem işlemleri, veriler "açık" olduğunda, verimli işleme için gerekli olduğunda, verilerin bir güvenilir yürütme ortamında https://en.wikipedia.org/wiki/Trusted_execution_environment (t-aynı zamanda bir şifreleme olarak da bilinir), aşağıdaki şekilde gösterildiği bir örnek olarak korunmasını sağlar .  

TEEs, bir hata ayıklayıcıyla birlikte, verileri veya dışarıdaki içindeki işlemleri görüntülemenin bir yolu olmadığından emin olun. Bunlara, yalnızca yetkili kodun verilere erişmesine izin verildiğinden emin olun. Kod değiştirilirse veya üzerinde değişiklik yapılmışsa, işlemler reddedilir ve ortam devre dışı bırakılır. T, bu korumaların içindeki kodun yürütülmesi boyunca uygulanmasını zorlar.

Daha fazla bilgi:

* [Azure gizli bilgi işlem tanıtımı](https://azure.microsoft.com/blog/introducing-azure-confidential-computing/)  
* [Azure gizli bilgi işlem](https://azure.microsoft.com/blog/azure-confidential-computing/)  
