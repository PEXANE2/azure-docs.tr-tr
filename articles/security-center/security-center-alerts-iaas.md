---
title: Azure Güvenlik Merkezi 'nde VM 'Ler ve sunucular için tehdit algılama | Microsoft Docs
description: Bu konuda, Azure Güvenlik Merkezi 'nde bulunan VM ve sunucu uyarıları sunulmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.topic: conceptual
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: a5ed91cef6e49fcb71c35f2262479be45a018651
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754313"
---
# <a name="threat-detection-for-vms-and-servers-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde VM 'Ler ve sunucular için tehdit algılama

Bu konu, aşağıdaki işletim sistemlerine sahip VM 'Ler ve sunucular için kullanılabilen farklı algılama yöntemi türlerini ve uyarılarını gösterir. Desteklenen sürümlerin listesi için bkz. [Azure Güvenlik Merkezi tarafından desteklenen platformlar ve Özellikler](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Pencerelerin<a name="windows-machines"></a>

Azure Güvenlik Merkezi, Windows tabanlı makinelerinizi izlemek ve korumak için Azure hizmetleri ile tümleşir. Güvenlik Merkezi, tüm bu hizmetlerden gelen uyarıları ve düzeltme önerilerini kullanımı kolay bir biçimde sunar.

* **Microsoft Defender ATP** <a name="windows-atp"></a> -Güvenlik Merkezi, Microsoft Defender Gelişmiş tehdit koruması (ATP) ile tümleştirerek bulut iş yükü koruma platformlarını genişletir. Bu, kapsamlı uç nokta algılama ve yanıt (EDR) özellikleri sağlar.

    > [!NOTE]
    > Microsoft Defender ATP algılayıcısı, güvenlik merkezi kullanan Windows sunucularında otomatik olarak etkinleştirilir.

    Microsoft Defender ATP bir tehdit algıladığında bir uyarı tetikler. Uyarı, güvenlik merkezi panosunda gösterilir. Panodan, Microsoft Defender ATP konsoluna Pivot verebilir ve saldırının kapsamını açığa çıkarmak için ayrıntılı bir araştırma gerçekleştirebilirsiniz. Microsoft Defender ATP hakkında daha fazla bilgi için bkz. [Microsoft Defender ATP hizmetine sunucu](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints)ekleme.

* **Kilitlenme bilgi dökümü Analizi** <a name="windows-dump"></a> -yazılım kilitlenirse, kilitlenme bilgi döküm kilitlenme sırasında belleğin bir kısmını yakalar.

    Kötü amaçlı yazılımdan koruma veya kötü amaçlı yazılım içeren bir kilitlenme oluşmuş olabilir. Güvenlik ürünleri tarafından algılanmayı önlemek için çeşitli kötü amaçlı yazılım biçimleri, diske yazmayı veya diske yazılmış yazılım bileşenlerini şifrelemeyi önleyen bir dosya daha az saldırı kullanır. Bu tür bir saldırı geleneksel disk tabanlı yaklaşımlar kullanılarak algılanarak tespit edilir.

    Ancak, bellek analizini kullanarak bu tür bir saldırıyı tespit edebilirsiniz. Güvenlik Merkezi, kilitlenme dökümündeki belleği çözümleyerek saldırının kullandığı teknikleri algılayabilir. Örneğin, saldırı yazılımda güvenlik açıklarından yararlanmaya, gizli verilere erişebilmeye ve güvenliği aşılmış bir makinede sürekli olarak devam ediyor olabilir. Güvenlik Merkezi, ana bilgisayarlara en az performans etkisi ile çalışır.

    Kilitlenme bilgi döküm Analizi uyarılarının ayrıntıları için, [Uyarı tablosuna](alerts-reference.md#alerts-windows)bakın.

* **Dosya daha az saldırı algılama** <a name="windows-fileless"></a> -uç noktalarınızı hedefleyen dosya daha az saldırı yaygındır. Algılamayı önlemek için, dosya daha az saldırı kötü amaçlı yükleri belleğe ekler. Saldırgan yükleri, güvenliği aşılmış işlemlerin belleğinde kalır ve çok çeşitli kötü amaçlı etkinlikler gerçekleştirir.

    Dosya daha az saldırı algılaması sayesinde, otomatik bellek oluşturma teknikleri, dosya daha az saldırı araç takımları, teknikler ve davranışları belirler. Bu çözüm, çalışma zamanında makinenizi düzenli olarak tarar ve öngörüleri doğrudan güvenlik açısından kritik işlemlerin belleğinden ayıklar.

    Yararlanma, kod ekleme ve kötü amaçlı yükleri yürütme kanıtlarını bulur. Dosya daha az saldırı algılama, uyarı önceliklendirme, bağıntı ve aşağı akış yanıt süresini hızlandırmak için ayrıntılı güvenlik uyarıları üretir. Bu yaklaşım, daha fazla algılama kapsamı sağlayan olay tabanlı EDR çözümlerini tamamlar.

    Dosya daha az saldırı algılama uyarılarına ilişkin ayrıntılar için, [Uyarı tablosuna](alerts-reference.md#alerts-windows)bakın.

> [!NOTE]
> [Azure Güvenlik Merkezi PlayBook: güvenlik uyarılarını](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046)indirerek Windows uyarılarının benzetimini yapabilirsiniz.

## 'Un<a name="linux-machines"></a>

Güvenlik Merkezi, en yaygın Linux denetim çerçevelerinden biri olan **auditd**'Yi kullanarak Linux makinelerden gelen denetim kayıtlarını toplar. auditd, ana hat çekirdekte yer yaşar. 

* **Linux auditd uyarıları ve Microsoft Monitoring Agent (MMA) Tümleştirmesi** <a name="linux-auditd"></a> -auditd sistemi, sistem çağrılarını izlemekten sorumlu olan çekirdek düzeyindeki bir alt sistemden oluşur. Bu, belirli bir kural kümesi tarafından filtreleyerek filtre uygular ve iletileri bir yuvaya yazar. Güvenlik Merkezi, Microsoft Monitoring Agent (MMA) içindeki auditd paketinden işlevleri tümleştirir. Bu tümleştirme, tüm desteklenen Linux dağıtımlarına hiçbir önkoşul olmadan sestd olaylarının toplanmasını mümkün bir şekilde sunar.  

    auditd kayıtları toplanır, zenginleştirir ve Linux MMA Aracısı kullanılarak olaylara toplanır. Güvenlik Merkezi, bulut ve şirket içi Linux makinelerinde kötü amaçlı davranışları algılamak için Linux sinyalleri kullanan yeni çözümlemeler ekler. Windows özelliklerine benzer şekilde, bu analizler şüpheli işlemlere, dubmerak eden oturum açma girişimlerini, çekirdek modülünü yüklemeyi ve diğer etkinlikleri kapsar. Bu etkinlikler, bir makinenin saldırıya açık ya da ihlal edilmiş olduğunu gösterebilir.  

    Aşağıda, saldırı yaşam döngüsünün farklı aşamalarını kapsayan çeşitli analizler örnekleri verilmiştir.

    Linux uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-linux)bakın.

> [!NOTE]
> [Azure Güvenlik Merkezi PlayBook: Linux algılamalarını](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef)indirerek Linux uyarılarının benzetimini yapabilirsiniz.

 
 ## <a name="next-steps"></a>Sonraki adımlar

Örnekler ve güvenlik merkezi algılama hakkında daha fazla bilgi için bkz.:

* [Azure Güvenlik Merkezi, siber saldırısı algılamasını nasıl otomatikleştirir](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)
* [Azure Güvenlik Merkezi 'nin yönetimsel araçları kullanarak güvenlik açıklarını nasıl algıladığı](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)
* [Güvenliği tehlikeye giren Linux makinelerinin saldırı sırasında algılamak için Azure Güvenlik Merkezi 'Nden yararlanın](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/)
* [Azure Güvenlik Merkezi, Linux 'ta gelişen güvenlik açıklarını algılayabilir](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/)