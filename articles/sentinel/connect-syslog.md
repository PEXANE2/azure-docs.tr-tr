---
title: Syslog verilerini Azure Sentinel önizlemesine bağlama | Microsoft Docs
description: Syslog verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2019
ms.author: rkarlin
ms.openlocfilehash: fef9fa128d2ebb84fb82579f254735fdb9aa7ee2
ms.sourcegitcommit: 1b7b0e1c915f586a906c33d7315a5dc7050a2f34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67881059"
---
# <a name="connect-your-external-solution-using-syslog"></a>Syslog kullanarak dış çözümünüzü bağlama

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Syslog 'yi destekleyen şirket içi gereçlerden Azure Sentinel 'e bağlanabilirsiniz. Bu, Gereç ve Azure Sentinel arasında Linux makinesine dayalı bir aracı kullanılarak yapılır. Linux makineniz Azure 'da ise, cihazınızdan veya uygulamanızdan günlükleri Azure 'da oluşturduğunuz ayrılmış bir çalışma alanına akışa alabilir ve bağlayabilirsiniz. Linux makineniz Azure 'da değilse, Aracıınızdan günlükleri Linux için aracıyı yüklediğiniz bir adanmış şirket içi VM 'ye veya makineye akışını sağlayabilirsiniz. 

> [!NOTE]
> Gereciniz Syslog CEF 'yi destekliyorsa, bağlantı daha tamamlanmıştır ve bu seçeneği seçmeniz ve [CEF 'deki verileri bağlama](connect-common-event-format.md)bölümündeki yönergeleri izlemeniz gerekir.

## <a name="how-it-works"></a>Nasıl çalışır?

Syslog Linux için ortak olan olay günlüğü protokolüdür. Uygulamaları, yerel makinede depolanan veya bir Syslog Toplayıcıya teslim olabilir iletileri gönderir. Linux için Log Analytics Aracısı yüklendiğinde, iletileri aracıya iletmek için yerel Syslog Daemon programını yapılandırır. Aracı daha sonra iletiyi ilgili kaydın oluşturulduğu Azure Izleyici 'ye gönderir.

Daha fazla bilgi için bkz. [Azure izleyici 'de Syslog veri kaynakları](../azure-monitor/platform/data-sources-syslog.md).

> [!NOTE]
> Aracı, birden fazla kaynaktaki günlükleri toplayabilir, ancak adanmış ara sunucu makinesine yüklenmiş olmalıdır.

## <a name="connect-your-syslog-appliance"></a>Syslog gerecinizi bağlama

1. Azure Sentinel portalında **veri bağlayıcıları** ' nı seçin ve tablodaki **Syslog** satırını seçin ve sağdaki Syslog bölmesinde **bağlayıcı sayfasını aç**' a tıklayın.
2. Linux makineniz Azure içindeyse, **indir ve aracıyı Azure Linux sanal makinesine**Yükle ' yi seçin. Sanal makineler penceresinde, aracıyı yüklemek istediğiniz makineleri seçin ve üst kısımdaki **Bağlan** ' a tıklayın.
1. Linux makineniz Azure 'da değilse, **Linux 'U Linux olmayan makineye indir ve yükle**' yi seçin. **Doğrudan aracı** penceresinde, **Linux için indirme ve ekleme Aracısı** altındaki komutu kopyalayın ve makinenizde çalıştırın. 
1. Günlükleri, syslog Bağlayıcısı Kurulumu penceresinde **bağlanacak şekilde yapılandırın** altında şu yönergeleri izleyin:
    1. **Çalışma alanı Gelişmiş ayarları yapılandırmanızı açmak**için bağlantıya tıklayın. 
    1. **Verileri**seçin ve sonra **Syslog**' ı seçin.
    1. Ardından, tabloda Syslog 'ın toplamasını istediğiniz tesislerin toplanmasını istediğinizi belirleyin. Syslog gerecinizin günlük üst bilgilerinde içerdiği tesisleri eklemeniz ya da seçmeniz gerekir. Bu yapılandırmayı, şu klasörde Syslog-d ' d e n Syslog:/etc/rsyslog.exe klasöründe ve/etc/syslog-ng/Security-config-omsagent.exe altındaki r-Syslog içinde görebilirsiniz. 
       > [!NOTE]
       > **Aşağıdaki yapılandırmayı makinelerime uygulamak**için onay kutusunu seçerseniz bu yapılandırma, bu çalışma alanına bağlı tüm Linux makinelere uygulanır. Bu yapılandırmayı Syslog makinenizde şu şekilde görebilirsiniz: 
1. **Yapılandırma dikey penceresini açmak için buraya bas**' ye tıklayın.
1. **Veri** ve ardından **Syslog**öğesini seçin.
   - Syslog tarafından gönderdiğiniz her bir özelliğin tabloda olduğundan emin olun. İzlenecek her tesis için bir önem derecesi belirleyin. **Uygula**'ya tıklayın.
1. Syslog makinenizde bu tesisleri gönderdiğinizden emin olun. 

1. Syslog günlükleri için Log Analytics ilgili şemayı kullanmak için **Syslog**araması yapın.
1. Syslog iletilerinizi ayrıştırmak ve sonra yeni bir Log Analytics işlevi olarak kaydetmek ve sonra işlevi yeni bir veri türü olarak kullanmak için [Azure izleyici günlük sorgularındaki Işlevleri kullanma](../azure-monitor/log-query/functions.md) bölümünde açıklanan kusto işlevini kullanabilirsiniz.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, syslog şirket içi gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
