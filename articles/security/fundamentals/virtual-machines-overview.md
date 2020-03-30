---
title: Azure VM'lerde kullanılan güvenlik özellikleri
titleSuffix: Azure security
description: Bu makalede, Azure Sanal Makineleri ile kullanılabilecek temel Azure güvenlik özelliklerine genel bir bakış sağlanmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/2/2019
ms.author: terrylan
ms.openlocfilehash: a1726e18ea8c1ba86d77d7b9ca3d50c444620361
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657172"
---
# <a name="azure-virtual-machines-security-overview"></a>Azure Sanal Makineler güvenlik genel bakış
Bu makalede, sanal makinelerle kullanılabilecek temel Azure güvenlik özelliklerine genel bir bakış sağlanmaktadır.

Çok çeşitli bilgi işlem çözümlerini çevik bir şekilde dağıtmak için Azure Sanal Makineleri'ni kullanabilirsiniz. Hizmet Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP ve Azure BizTalk Hizmetlerini destekler. Böylece, herhangi bir iş yükünü ve herhangi bir dili hemen hemen her işletim sistemine dağıtabilirsiniz.

Bir Azure sanal makinesi, sanal makineyi çalıştıran fiziksel donanımı satın almanıza ve muhafaza etmenize gerek kalmadan size sanallaştırma esnekliği sunar. Verilerinizin yüksek güvenlikli veri merkezlerinde korunduğu ve güvenli olduğu güvencesiyle uygulamalarınızı oluşturabilir ve dağıtabilirsiniz.

Azure ile, güvenlikle geliştirilmiş, uyumlu çözümler oluşturabilirsiniz:

* Sanal makinelerinizi virüslerden ve kötü amaçlı yazılımlardan koruyun.
* Hassas verilerinizi şifreleyin.
* Güvenli ağ trafiği.
* Tehditleri tanımlayın ve tespit edin.
* Uyumluluk gereksinimlerini karşılayın.  

## <a name="antimalware"></a>Antimalware

Azure ile Microsoft, Symantec, Trend Micro ve Kaspersky gibi güvenlik satıcılarının kötü amaçlı yazılımdan koruma yazılımlarını kullanabilirsiniz. Bu yazılım, sanal makinelerinizi kötü amaçlı dosyalara, reklam yazılımlarına ve diğer tehditlerden korumaya yardımcı olur.

Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma, virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olan gerçek zamanlı bir koruma yeteneğidir.  Azure için Microsoft Kötü Amaçlı Yazılımdan Koruma, bilinen kötü amaçlı veya istenmeyen yazılımlar azure sistemlerinizde kendisini yüklemeye veya çalıştırmaya çalıştığında yapılandırılabilir uyarılar sağlar.

Azure için Microsoft Kötü Amaçlı Yazılımdan Koruma, uygulamalar ve kiracı ortamları için tek aracılı bir çözümdür. İnsan müdahalesi olmadan arka planda çalışmak için tasarlandı. Kötü amaçlı yazılımdan koruma izleme de dahil olmak üzere temel güvenli veya gelişmiş özel yapılandırmayla uygulama iş yüklerinizin gereksinimlerine göre koruma dağıtabilirsiniz.

[Azure için Microsoft Kötü Amaçlı Yazılımdan Koruma](antimalware.md) ve kullanılabilen temel özellikler hakkında daha fazla bilgi edinin.

Sanal makinelerinizi korumaya yardımcı olmak için kötü amaçlı yazılımdan koruma yazılımı hakkında daha fazla bilgi edinin:

* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Trend Micro Deep Security'yi Windows VM'de hizmet olarak yükleme ve yapılandırma](/azure/virtual-machines/windows/classic/install-trend)
* [Windows VM'de Symantec Endpoint Protection nasıl yüklenir ve yapılandırılır?](/azure/virtual-machines/windows/classic/install-symantec)
* [Azure Marketinde güvenlik çözümleri](https://azure.microsoft.com/marketplace/?term=security)

Daha güçlü bir koruma için [Windows Defender Gelişmiş Tehdit Koruması](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection)kullanmayı düşünün. Windows Defender ATP ile şunları elde elabilirsiniz:

* [Saldırı yüzeyi azaltma](/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction)  
* [Yeni nesil koruma](/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)  
* [Uç nokta koruması ve yanıt](/windows/security/threat-protection/windows-defender-atp/overview-endpoint-detection-response)
* [Otomatik araştırma ve düzeltme](/windows/security/threat-protection/windows-defender-atp/automated-investigations-windows-defender-advanced-threat-protection)
* [Güvenli skor](/windows/security/threat-protection/microsoft-defender-atp/configuration-score)
* [Gelişmiş avcılık](/windows/security/threat-protection/windows-defender-atp/overview-hunting-windows-defender-advanced-threat-protection)
* [Yönetim ve API'ler](/windows/security/threat-protection/windows-defender-atp/management-apis)
* [Microsoft Tehdit Koruması](/windows/security/threat-protection/windows-defender-atp/threat-protection-integration)

Daha fazla bilgi edinin:

* [WDATP ile Başlayın](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)  
* [WDATP yeteneklerine genel bakış](/windows/security/threat-protection/windows-defender-atp/overview)  

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Anahtar güvenliğinin iyileştirilmesi şifreleme ve kimlik doğrulama korumalarını artırabilir. Kritik sırlarınızın ve anahtarlarınızın yönetimini ve güvenliğini Azure Key Vault'ta saklayarak basitleştirebilirsiniz.

Key Vault, anahtarlarınızı FIPS 140-2 Düzey 2 standartlarına göre onaylanmış donanım güvenlik modüllerinde (HSM' ler) saklama seçeneği sunar. Yedekleme veya saydam veri [şifreleme](https://msdn.microsoft.com/library/bb934049.aspx) için SQL Server şifreleme anahtarlarınızın tümü, uygulamalarınızdan gelen tüm anahtarlar veya sırlar ile Birlikte Key Vault'ta saklanabilir. İzinler ve bu korumalı öğelere erişim [Azure Etkin Dizin](https://azure.microsoft.com/documentation/services/active-directory/)aracılığıyla yönetilir.

Daha fazla bilgi edinin:

* [Azure Anahtar Kasası nedir?](/azure/key-vault/key-vault-overview)
* [Azure Key Vault blogu](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Sanal makine diski şifrelemesi

Azure Disk Şifreleme, Windows ve Linux sanal makine disklerinizi şifrelemek için yeni bir özelliktir. Azure Disk Şifreleme, işletim sistemi ve veri diskleri için birim şifrelemesağlamak için Windows'un endüstri standardı [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) özelliğini ve Linux'un [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) özelliğini kullanır.

Çözüm, anahtar kasa aboneliğinizdeki disk şifreleme anahtarlarını ve sırlarını kontrol etmenize ve yönetmenize yardımcı olmak için Azure Key Vault ile entegre edilmiştir. Sanal makine disklerideki tüm verilerin Azure Depolama'da dinlenime de şifrelenmesini sağlar.

Daha fazla bilgi edinin:

* [IaaS VM'ler için Azure Disk Şifrelemesi](/azure/security/azure-security-disk-encryption-overview)
* [Hızlı Başlangıç: Azure PowerShell ile bir Windows IaaS VM'ini Şifreleme](../../virtual-machines/linux/disk-encryption-powershell-quickstart.md)

## <a name="virtual-machine-backup"></a>Sanal makine yedekleme

Azure Yedekleme, uygulama verilerinizin sıfır sermaye yatırımı ve minimum işletme maliyetiyle korunmasına yardımcı olan ölçeklenebilir bir çözümdür. Uygulama hataları verilerinizi bozabilir ve insan hataları uygulamalarınıza hata getirebilir. Azure Yedekleme ile Windows ve Linux çalıştıran sanal makineleriniz korunur.

Daha fazla bilgi edinin:

* [Azure Yedekleme nedir?](/azure/backup/backup-introduction-to-azure-backup)
* [Azure Yedekleme hizmeti SSS](/azure/backup/backup-azure-backup-faq)

## <a name="azure-site-recovery"></a>Azure Site Recovery

Kuruluşunuzun BCDR stratejisinin önemli bir parçası, planlı ve planlanmamış kesintiler meydana geldiğinde kurumsal iş yüklerini ve uygulamaları nasıl çalışır durumda tutacağınızı bulmaktır. Azure Site Kurtarma, birincil konumunuz çöktürülmede ikincil bir konumdan kullanılabilmeleri için iş yüklerinin ve uygulamaların çoğaltılması, başarısız olma ve kurtarma işleminin düzenlenmesine yardımcı olur.

Site Kurtarma:

* **BCDR stratejinizi basitleştirir**: Site Kurtarma, birden çok iş yükünün ve uygulamanın tek bir konumdan çoğaltılmasını, başarısız olmasını ve kurtarılmasını kolaylaştırır. Site Kurtarma çoğaltma ve başarısız lık yönetir, ancak uygulama verilerinizi ele geçirmez veya bu konuda herhangi bir bilgiye sahip değildir.
* **Esnek çoğaltma sağlar**: Site Kurtarma'yı kullanarak Hyper-V sanal makinelerde, VMware sanal makinelerde ve Windows/Linux fiziksel sunucularında çalışan iş yüklerini çoğaltabilirsiniz.
* **Arıza ve kurtarmayı destekler**: Site Kurtarma, üretim ortamlarını etkilemeden olağanüstü durum kurtarma matkaplarını desteklemek için test arızaları sağlar. Ayrıca, beklenen kesintilere yönelik olarak sıfır veri kaybı sunan planlanan yük devretmeler veya beklenmeyen olağanüstü durumlar için minimum düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığına bağlı olarak) planlanmamış yük devretmeler çalıştırabilirsiniz. Başarısız olduktan sonra, birincil sitelerinize geri dönebilirsiniz. Yük devretme işlemini özelleştirebilmeniz ve çok katmanlı uygulamaları kurtarabilmeniz için Site Recovery, betikleri ve Azure otomasyonu çalışma kitaplarını içeren kurtarma planları sunar.
* **İkincil veri merkezlerini ortadan kaldırır**: İkincil bir şirket içi siteye veya Azure'a çoğaltabilirsiniz. Azure'un olağanüstü durum kurtarma hedefi olarak kullanılması, ikincil bir siteyi korumanın maliyetini ve karmaşıklığını ortadan kaldırır. Çoğaltılan veriler Azure Depolama'da depolanır.
* **Mevcut BCDR teknolojileri ile tümleştirir**: Site Kurtarma ortakları ve diğer uygulamaların BCDR özellikleri. Örneğin, kurumsal iş yüklerinin SQL Server arka ucunu korumaya yardımcı olmak için Site Kurtarma kullanabilirsiniz. Bu, kullanılabilirlik gruplarının başarısız olmasını yönetmek için SQL Server Always On için yerel desteği içerir.

Daha fazla bilgi edinin:

* [Azure Site Recovery nedir?](/azure/site-recovery/site-recovery-overview)
* [Azure Site Recovery nasıl çalışır?](/azure/site-recovery/site-recovery-components)
* [Hangi iş yükleri Azure Site Kurtarma tarafından korunur?](/azure/site-recovery/site-recovery-workload)

## <a name="virtual-networking"></a>Sanal ağ

Sanal makinelerin ağ bağlantısına ihtiyacı vardır. Bu gereksinimi desteklemek için Azure, sanal makinelerin bir Azure sanal ağına bağlanmasını gerektirir.

Azure sanal ağı, fiziksel Azure ağ dokusunun üzerine inşa edilmiş mantıksal bir yapıdır. Her mantıksal Azure sanal ağı diğer tüm Azure sanal ağlarından izole edilmiştir. Bu yalıtım, dağıtımlarınızdaki ağ trafiğinin diğer Microsoft Azure müşterileri tarafından erişilemeyeceğini niçin sigortalatamanıza yardımcı olur.

Daha fazla bilgi edinin:

* [Azure ağ güvenliğine genel bakış](network-overview.md)
* [Sanal Ağa genel bakış](/azure/virtual-network/virtual-networks-overview)
* [Kurumsal senaryolar için ağ özellikleri ve ortaklıklar](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Güvenlik politikası yönetimi ve raporlama

Azure Güvenlik Merkezi, tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Güvenlik Merkezi, Azure kaynaklarınızın güvenliği yle ilgili daha fazla görünürlük ve denetim sağlar. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Aksi takdirde fark edilmeyecek tehditleri algılamaya yardımcı olur ve geniş bir güvenlik çözümleri ekosistemi yle çalışır.

Güvenlik Merkezi, sanal makinelerinizin güvenliğini aşağıdaki lere göre optimize edip izlemenize yardımcı olur:

* Sanal makineler için [güvenlik önerileri](/azure/security-center/security-center-recommendations) sağlar. Örnek öneriler şunlardır: sistem güncelleştirmelerini uygulayın, ALA'ların bitiş noktalarını yapılandırın, kötü amaçlı yazılımdan korumayı etkinleştirin, ağ güvenlik gruplarını etkinleştirin ve disk şifrelemeuygulayın.
* Sanal makinelerinizin durumunu izleme.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](/azure/security-center/security-center-intro)
* [Azure Güvenlik Merkezi sık sorulan sorular](/azure/security-center/security-center-faq)
* [Azure Güvenlik Merkezi planlama ve işlemleri](/azure/security-center/security-center-planning-and-operations-guide)

## <a name="compliance"></a>Uyumluluk

Azure Sanal Makineler, FISMA, FedRAMP, HIPAA, PCI DSS Düzey 1 ve diğer önemli uyumluluk programları için sertifikalıdır. Bu sertifika, kendi Azure uygulamalarınızın uyumluluk gereksinimlerini karşılamasını ve işletmenizin çok çeşitli yurtiçi ve uluslararası mevzuat gerekliliklerini karşılamasını kolaylaştırır.

Daha fazla bilgi edinin:

* [Microsoft Güven Merkezi: Uyumluluk](https://www.microsoft.com/en-us/trustcenter/compliance)
* [Güvenilir Bulut: Microsoft Azure Güvenliği, Gizlilik ve Uyumluluk](https://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)

## <a name="confidential-computing"></a>Gizli Bilgi İşlem

Gizli bilgi işlem teknik olarak sanal makine güvenliğinin bir parçası olmasa da, sanal makine güvenliği konusu "bilgi işlem" güvenliğinin üst düzey konusuna aittir. Gizli bilgi işlem "bilgi işlem" güvenliği kategorisine aittir.

Gizli bilgi işlem, verilerin verimli işleme için gerekli olan "açık" olduğunda, verilerin güvenilir https://en.wikipedia.org/wiki/Trusted_execution_environment bir yürütme ortamı (TEE - enklav olarak da bilinir) içinde korunmasını sağlar ve bunun bir örneği aşağıdaki şekilde gösterilir.  

TEEs, bir hata ayıklayıcıyla bile verileri veya içerideki işlemleri dışarıdan görüntülemenin bir yolu olmadığından emin olur. Verilere erişmek için yalnızca yetkili kodun izin verildiğinden bile emin olurlar. Kod değiştirilir veya değiştirilirse, işlemler reddedilir ve ortam devre dışı bırakılır. TEE, bu korumaları içinde kodun yürütülmesi boyunca uygular.

Daha fazla bilgi edinin:

* [Azure gizli bilgi işlem ile tanıştırMa](https://azure.microsoft.com/blog/introducing-azure-confidential-computing/)  
* [Azure gizli bilgi işlem](https://azure.microsoft.com/blog/azure-confidential-computing/)  

## <a name="next-steps"></a>Sonraki adımlar

[VM'ler](iaas.md) ve işletim sistemleri için güvenlik en iyi uygulamaları hakkında bilgi edinin.
