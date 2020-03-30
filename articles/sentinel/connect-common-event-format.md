---
title: CEF verilerini Azure Sentinel Preview'a bağlayın| Microsoft Dokümanlar
description: CEF verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588357"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Ortak Olay Biçimini kullanarak harici çözümünüzü bağlayın


CEF iletileri gönderen harici bir çözüme bağlandığınızda, Azure Sentinel'e bağlanmanın üç adımı vardır:

ADIM 1: [AracıYı dağıtarak CEF'i bağlayın](connect-cef-agent.md) STEP 2: [Çözüme özel adımları gerçekleştirIN](connect-cef-solution-config.md) STEP 3: [Bağlantıyı doğrulayın](connect-cef-verify.md)

Bu makalede, bağlantının nasıl çalıştığı açıklanır, ön koşullar sağlar ve aracıyı Syslog'un üstüne Ortak Olay Biçimi (CEF) iletileri gönderen güvenlik çözümlerine dağıtmak için gereken adımları sağlar. 

> [!NOTE] 
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

Bu bağlantıyı kurmak için, cihaz ve Azure Sentinel arasındaki iletişimi desteklemek için özel bir Linux makinesine (VM veya şirket içi) bir aracı dağıtmanız gerekir. Aşağıdaki diyagramda, Azure'da bir Linux VM durumunda kurulum açıklanmaktadır.

 ![Azure'da CEF](./media/connect-cef/cef-syslog-azure.png)

Alternatif olarak, başka bir bulutta veya şirket içi makinede VM kullanırsanız, bu kurulum bulunur. 

 ![CEF tesislerinde](./media/connect-cef/cef-syslog-onprem.png)


## <a name="security-considerations"></a>Güvenlik konuları

Makinenin güvenliğini kuruluşunuzun güvenlik ilkesine göre yapılandırdığından emin olun. Örneğin, ağınızı kurumsal ağ güvenlik politikanızla uyumlu olacak şekilde yapılandırabilir ve daemon'daki bağlantı noktalarını ve protokollerini gereksinimlerinize uygun olacak şekilde değiştirebilirsiniz. Makine güvenliği yapılandırmanızı geliştirmek için aşağıdaki yönergeleri kullanabilirsiniz:  [Azure'da Güvenli VM](../virtual-machines/linux/security-policy.md), [Ağ güvenliği için en iyi uygulamalar.](../security/fundamentals/network-best-practices.md)

Güvenlik çözümü ve Syslog makinesi arasındaki TLS iletişimini kullanmak için, TLS'de iletişim kurmak için Syslog daemon 'u (rsyslog veya syslog-ng) yapılandırmanız gerekir: [TlS ile Syslog Trafiğini şifreleme -rsyslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [günlük iletileri TLS ile şifreleme –syslog-ng](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).

 
## <a name="prerequisites"></a>Ön koşullar
Proxy olarak kullandığınız Linux makinesinin aşağıdaki işletim sistemlerinden birini çalıştırdığından emin olun:

- 64 bit
  - CentOS 6 ve 7
  - Amazon Linux 2017.09
  - Oracle Linux 6 ve 7
  - Red Hat Enterprise Linux Server 6 ve 7
  - Debian GNU/Linux 8 ve 9
  - Ubuntu Linux 14.04 LTS, 16.04 LTS ve 18.04 LTS
  - SUSE Linux Enterprise Server 12
- 32 bit
   - CentOS 6
   - Oracle Linux 6
   - Red Hat Enterprise Linux Server 6
   - Debian GNU/Linux 8 ve 9
   - Ubuntu Linux 14.04 LTS ve 16.04 LTS
 
 - Daemon sürümleri
   - Syslog-ng: 2.1 - 3.22.1
   - Rsyslog: v8
  
 - Syslog RFC'ler desteklendi
   - Syslog RFC 3164
   - Syslog RFC 5424
 
Makinenizin aşağıdaki gereksinimleri de karşıladığından emin olun: 
- İzinler
    - Makinenizde yüksek izinler (sudo) olmalıdır. 
- Yazılım gereksinimleri
    - Python'un makinenizde çalıştırdığından emin olun



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, CEF cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

