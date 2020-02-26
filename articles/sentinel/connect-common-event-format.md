---
title: CEF verilerini Azure Sentinel önizlemesine bağlama | Microsoft Docs
description: CEF verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/26/2019
ms.author: yelevin
ms.openlocfilehash: 8314614616c6b8969832d52fc684d47ba1bf0fe3
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588357"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Ortak olay biçimini kullanarak dış çözümünüzü bağlama


CEF iletileri gönderen bir dış çözümü bağladığınızda, Azure Sentinel ile bağlantı kurmak için üç adım vardır:

1\. Adım: [aracıyı dağıtarak CEF 'Yi bağlama](connect-cef-agent.md) adım 2: [çözüme özgü adımları gerçekleştirme](connect-cef-solution-config.md) adım 3: [bağlantıyı doğrulama](connect-cef-verify.md)

Bu makalede bağlantının nasıl çalıştığı açıklanır, ön koşullar sağlanır ve aracıyı, syslog 'nin en üstünde ortak olay biçimi (CEF) iletileri gönderen güvenlik çözümlerinde dağıtmaya yönelik adımlar sağlar. 

> [!NOTE] 
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

Bu bağlantıyı yapmak için, Gereç ve Azure Sentinel arasındaki iletişimi desteklemek üzere adanmış bir Linux makinesine (VM veya şirket içi) bir aracı dağıtmanız gerekir. Aşağıdaki diyagramda, Azure 'daki bir Linux sanal makinesi olayında kurulum açıklanmaktadır.

 ![Azure 'da CEF](./media/connect-cef/cef-syslog-azure.png)

Alternatif olarak, başka bir bulutta veya şirket içi bir makinede bir VM kullanıyorsanız bu kurulum mevcut olacaktır. 

 ![Şirket içi CEF](./media/connect-cef/cef-syslog-onprem.png)


## <a name="security-considerations"></a>Güvenlikle ilgili dikkat edilmesi gerekenler

Makinenin güvenliğini kuruluşunuzun güvenlik ilkesine göre yapılandırdığınızdan emin olun. Örneğin, ağınızı kurumsal ağ güvenlik ilkenize göre olacak şekilde yapılandırabilir ve gereksinimlerinize göre uyum sağlamak için arka plan programındaki bağlantı noktalarını ve protokolleri değiştirmelisiniz. Aşağıdaki yönergeleri kullanarak makinenizin güvenlik yapılandırmasını geliştirebilirsiniz:  [Azure 'Da GÜVENLI VM](../virtual-machines/linux/security-policy.md), [ağ güvenliği için en iyi uygulamalar](../security/fundamentals/network-best-practices.md).

Güvenlik çözümü ve Syslog makinesi arasındaki TLS iletişimini kullanmak için Syslog Daemon (rsyslog veya Syslog-ng) ' i TLS ile iletişim kurmak üzere yapılandırmanız gerekir: TLS [-rsyslog Ile Syslog trafiğini şifreleme](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [günlük iletilerini TLS – Syslog-NG ile şifreleme](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).

 
## <a name="prerequisites"></a>Önkoşullar
Proxy olarak kullandığınız Linux makinenin aşağıdaki işletim sistemlerinden birini çalıştırdığından emin olun:

- 64 bit
  - CentOS 6 ve 7
  - Amazon Linux 2017,09
  - Oracle Linux 6 ve 7
  - Red Hat Enterprise Linux Server 6 ve 7
  - Borçlu GNU/Linux 8 ve 9
  - Ubuntu Linux 14,04 LTS, 16,04 LTS ve 18,04 LTS
  - SUSE Linux Enterprise Server 12
- 32 bit
   - CentOS 6
   - Oracle Linux 6
   - Red Hat Enterprise Linux Server 6
   - Borçlu GNU/Linux 8 ve 9
   - Ubuntu Linux 14,04 LTS ve 16,04 LTS
 
 - Daemon sürümleri
   - Syslog-NG: 2,1-3.22.1
   - Rsyslog: V8
  
 - Syslog RFC 'Leri destekleniyor
   - Syslog RFC 3164
   - Syslog RFC 5424
 
Makinenizin aynı zamanda aşağıdaki gereksinimleri karşıladığından emin olun: 
- İzinler
    - Makinenizde yükseltilmiş izinleriniz (sudo) olmalıdır. 
- Yazılım gereksinimleri
    - Makinenizde Python 'un çalıştığından emin olun



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede CEF gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

