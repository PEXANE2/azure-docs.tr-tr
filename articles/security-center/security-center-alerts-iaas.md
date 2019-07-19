---
title: Azure Güvenlik Merkezi 'nde VM 'Ler & sunucular için tehdit algılama | Microsoft Docs
description: Bu konuda, Azure Güvenlik Merkezi 'nde bulunan VM ve sunucu uyarıları sunulmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: f23865fc0a1943a5157e4ff8eb8de10a71ef0883
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295785"
---
# <a name="threat-detection-for-vms--servers-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde VM 'Ler & sunucular için tehdit algılama

Bu konuda, aşağıdaki işletim sistemlerine sahip VM 'Ler ve sunucular için kullanılabilen farklı algılama yöntemleri ve uyarıları sunulmaktadır. Desteklenen sürümlerin listesi için bkz. [Azure Güvenlik Merkezi tarafından desteklenen platformlar ve Özellikler](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Pencerelerin<a name="windows-machines"></a>

Güvenlik Merkezi, Windows tabanlı makinelerinizi izlemek ve korumak için Azure hizmetleri ile tümleşir.  Güvenlik Merkezi, tüm bu hizmetlerden gelen uyarıları ve düzeltme önerilerini kullanımı kolay bir biçimde sunar.

### Microsoft Server Defender ATP<a nanme="windows-atp"></a>

Azure Güvenlik Merkezi, Windows Defender Gelişmiş tehdit koruması (ATP) ile tümleştirerek bulut Iş yükü koruma platformlarını genişletir. Bu, kapsamlı uç nokta algılama ve yanıt (EDR) özellikleri sağlar.

> [!NOTE]
> Windows Server Defender ATP algılayıcısı, Azure Güvenlik Merkezi 'nde eklendi Windows sunucularında otomatik olarak etkinleştirilir.

Windows Server Defender ATP bir tehdit algıladığında bir uyarı tetikler. Uyarı, güvenlik merkezi panosunda gösterilir. Panodan, saldırının kapsamını açmaya yönelik ayrıntılı bir araştırma gerçekleştirmek için Windows Defender ATP konsoluna bir özet oluşturabilirsiniz. Windows Server Defender ATP hakkında daha fazla bilgi için bkz. [Windows Defender ATP hizmetine sunucu](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)ekleme.

### Kilitlenme bilgi döküm Analizi<a nanme="windows-dump"></a>

Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar.

Kötü amaçlı yazılımdan koruma veya kötü amaçlı yazılım içeren bir kilitlenme oluşmuş olabilir. Güvenlik ürünleri tarafından algılanmayı önlemek için çeşitli kötü amaçlı yazılım biçimleri, diske yazmayı veya diske yazılmış yazılım bileşenlerini şifrelemeyi önleyen bir dosya daha az saldırı kullanır. Bu tür bir saldırı geleneksel disk tabanlı yaklaşımlar kullanılarak algılanarak tespit edilir.

Ancak, bu tür bir saldırı bellek analizi kullanılarak algılanabilir. Güvenlik Merkezi, kilitlenme dökümündeki belleği analiz ederek, saldırının yazılım içindeki güvenlik açıklarından yararlanmak, gizli verilere erişmek ve güvenliği aşılmış bir makine dahilinde kalıcı olarak devam ettirmek için kullandığı teknikleri algılayabilir. Bu işlem, ana bilgisayarlara en düşük performans etkisi olan güvenlik merkezi arka ucu tarafından yapılır.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Kod ekleme bulundu**|Kod ekleme, çalışmakta olan işlemlere veya iş parçacıklarına yürütülebilir modüllerin eklenmesidir. Bu teknik, kötü amaçlı yazılım tarafından, bulunan ve kaldırılan verileri başarıyla gizlerken, verileri erişmek için kullanılır. <br/>Bu uyarı, eklenen bir modülün kilitlenme bilgi dökümünde mevcut olduğunu belirtir. Güvenlik Merkezi, kötü amaçlı olan ve olmayan eklenen modülleri birbirinden ayırt etmek için eklenen modülün bir şüpheli davranış profiline uygun olup olmadığını denetler.|
|**Şüpheli kod segmenti bulundu**|Bir kod kesiminin, yansıtıcı ekleme ve işlem gizleme gibi standart olmayan yöntemler kullanılarak ayrıldığını gösterir. Uyarı, bildirilen kod segmentinin özellikleri ve davranışları için bağlam sağlamak üzere işlenmiş olan kod kesiminin ek özelliklerini sağlar.|
|**Bulunan shellcode**|Kabuk Kodu, kötü amaçlı yazılım bir yazılım güvenlik açığından yararlandıktan sonra çalıştırılan yüktür.<br/>Bu uyarı, kilitlenme dökümü analizinin kötü amaçlı yükler tarafından yaygın olarak gerçekleştirilen davranışı gösteren yürütülebilir kod algıladığını gösterir. Kötü amaçlı olmayan yazılımlar da bu davranışı gerçekleştirebilir, ancak normal yazılım geliştirme uygulamalarından tipik değildir.|

### Dosya daha az saldırı algılama<a nanme="windows-fileless"></a>

Azure 'da, müşterilerinizin uç noktalarını hedefleyen dosya daha az saldırıları düzenli olarak görebiliyoruz.

Algılamayı önlemek için, dosya daha az saldırı kötü amaçlı yükleri belleğe ekler. Saldırgan yükleri, güvenliği aşılmış işlemlerin belleğinde kalır ve çok çeşitli kötü amaçlı etkinlikler gerçekleştirir.

Dosya daha az saldırı algılaması sayesinde, otomatik bellek oluşturma teknikleri, dosya daha az saldırı araç takımları, teknikler ve davranışları belirler. Bu çözüm, çalışma zamanında makinenizi düzenli olarak tarar ve öngörüleri doğrudan güvenlik açısından kritik işlemlerin belleğinden ayıklar.

Yararlanma, kod ekleme ve kötü amaçlı yükleri yürütme kanıtlarını bulur. Dosya daha az saldırı algılama, uyarı önceliklendirme, bağıntı ve aşağı akış yanıt süresini hızlandırmak için ayrıntılı güvenlik uyarıları üretir. Bu yaklaşım, daha fazla algılama kapsamı sağlayan olay tabanlı EDR çözümlerini tamamlar.

> [!NOTE]
> [Azure Güvenlik Merkezi PlayBook](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef)'U indirerek Windows uyarılarının benzetimini yapabilirsiniz: Güvenlik uyarıları ve belirtilen yönergeleri izleyin

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Dosya Less saldırı tekniği algılandı**|Aşağıda belirtilen işlemin belleği bir dosya daha az saldırı araç seti içerir: Meterpreter. Dosya sisteminde genellikle dosya sisteminde bir varlık yoktur, bu da geleneksel Anti-Virus tarafından algılama işlemini zorlaştırır.|

### <a name="further-reading"></a>Daha fazla bilgi

Örnekler ve Güvenlik Merkezi algılaması hakkında daha fazla bilgi için:

* [Azure Güvenlik Merkezi, siber saldırısı algılamasını nasıl otomatikleştirir](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)
* [Azure Güvenlik Merkezi 'nin yönetimsel araçları kullanarak güvenlik açıklarını nasıl algıladığı](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

## 'Un<a name="linux-machines"></a>

Güvenlik Merkezi, en yaygın Linux denetim çerçevelerinden biri olan **auditd**kullanarak Linux makinelerden denetim kayıtları toplar. auditd, uzun bir süre içinde olma ve ana hat çekirdekte yaşayan avantajlardan yararlanmaktadır. 

### Linux auditd uyarıları ve Microsoft Monitoring Agent (MMA) Tümleştirmesi<a name="linux-auditd"></a>

Auditd sistemi, sistem çağrılarını izlemekten, belirli bir kural kümesine göre filtrelemeden ve bunları bir soket için iletilerle ilgili olan çekirdek düzeyinde bir alt sistemden oluşur. Güvenlik Merkezi, Microsoft Monitoring Agent (MMA) içindeki auditd paketinden işlevleri tümleştirir. Bu tümleştirme, hiçbir önkoşul olmadan desteklenen tüm Linux dağıtımlarında auditd olayları toplamasını mümkün değildir.  

auditd kayıtları toplanır, zenginleştirir ve Linux MMA Aracısı kullanılarak olaylara toplanır. Güvenlik Merkezi, bulut ve şirket içi Linux makinelerinde kötü amaçlı davranışları algılamak için Linux sinyallerine yararlanan yeni analizler ekleme konusunda sürekli olarak çalışmaktadır. Windows özelliklerine benzer şekilde, bu analizler şüpheli işlemlere, dubsin oturum açma girişimlerine, çekirdek modülü yüklemeye ve bir makinenin saldırıya açık ya da ihlal edilmiş olduğunu gösteren diğer etkinliklere yayılır.  

Aşağıda, saldırı yaşam döngüsünün farklı aşamaları boyunca nasıl yayılacağını gösteren çeşitli analiz örnekleri verilmiştir.

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Beklenmedik bir şekilde SSH yetkili anahtarlar dosyasına erişim için işlem görüldü**|Bilinen kötü amaçlı yazılım kampanyalarına benzer bir yöntemde SSH yetkili anahtarları dosyasına erişildi. Bu erişim, bir saldırganın bir makineye kalıcı erişim kazanmaya çalıştığınızı belirtebilir|
|**Algılanan Kalıcılık girişimi**|Konak veri analizi, tek kullanıcılı mod için bir başlangıç betiğinin yüklendiğini algıladı. <br/>Meşru bir işlemin bu modda yürütülmesi gerektiğinden nadir olduğundan, bu durum, bir saldırganın kalıcılığı garanti etmek için her çalışma düzeyine kötü amaçlı bir işlem eklediğini gösterir.|
|**Zamanlanmış görevlerin işlenmesi algılandı**|Ana bilgisayar veri analizi, zamanlanmış görevler için olası bir düzenleme algıladı. Saldırganlar genellikle kalıcı görevleri riskli hale getikleri makinelere ekler.|
|**Şüpheli dosya zaman damgası değişikliği**|Ana bilgisayar veri analizi şüpheli bir zaman damgası değişikliği algıladı. Saldırganlar, bu yeni bırakılan dosyaların algılanmasını önlemek için genellikle mevcut meşru dosyalardaki zaman damgalarını yeni araçlara kopyalar|
|**Sudoers grubuna yeni bir kullanıcı eklendi**|Konak veri analizi, bir kullanıcının sudoers grubuna eklendiğini algıladı ve bu, üyelerinin yüksek ayrıcalıklarla komutları çalıştırmasına olanak sağlar.|
|**DHCP istemcisinde DynoRoot güvenlik açığından büyük olasılıkla faydalanma**|Ana bilgisayar veri analizi, dhclient betiğinin ana işlemiyle olağan dışı bir komutun yürütülmesini algıladı.|
|**Şüpheli çekirdek modülü algılandı**|Ana bilgisayar veri analizi, bir çekirdek modülü olarak yüklenen paylaşılan bir nesne dosyası algıladı. Bu yasal etkinlik olabilir veya makinelerinizden birinin tehlikeye girdiğinin göstergesidir.|
|**Dijital para birimi madenciliği ile ilişkili işlem algılandı**|Ana bilgisayar veri analizi, normalde dijital para birimi madenciliği ile ilişkili bir işlemin yürütülmesini algıladı|
|**Dış IP adresine yönelik olası bağlantı noktası iletme**|Ana bilgisayar veri analizi, bir dış IP adresine bağlantı noktası iletmeyi başlatma işlemini algıladı.|

> [!NOTE]
> Azure Güvenlik Merkezi PlayBook 'u indirerek [Windows uyarılarının benzetimini yapabilirsiniz: Güvenlik uyarıları](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef) ve belirtilen yönergeleri izleyin.


Daha fazla bilgi için şu makalelere bakın:  

* [Güvenliği tehlikeye giren Linux makinelerinin saldırı sırasında algılamak için Azure Güvenlik Merkezi 'Nden yararlanın](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)

* [Azure Güvenlik Merkezi, Linux 'ta gelişen güvenlik açıklarını algılayabilir](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)

 