---
title: Windows güvenlik olay verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Windows güvenlik olay verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/22/2020
ms.author: yelevin
ms.openlocfilehash: 30055ba1befc68d015e3e3162d8db11a2916f3d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124977"
---
# <a name="connect-windows-security-events"></a>Windows güvenlik olaylarını bağlama 

Güvenlik Olayları bağlayıcısı, Windows sistemlerinizdeki tüm güvenlik olaylarını (sunucular ve iş istasyonları, fiziksel ve sanal) Azure Sentinel çalışma alanınıza aktarmanızı sağlar. Bu, panolarınızdaki Windows güvenlik olaylarını görüntülemenize, bunları özel uyarılar oluştururken kullanmanıza ve araştırmalarınızı geliştirmek için bunlara güvenmenize olanak sağlayarak kuruluşunuzun ağı hakkında daha fazla bilgi edinmenizi ve güvenlik işlemlerinizi genişletmenizi sağlar Yetenek -lerini. Aşağıdaki kümeler arasından hangi olayların aksatı çıkarılalalalabiliyorum:<a name="event-sets"></a>

- **Tüm etkinlikler** - Tüm Windows güvenlik ve AppLocker etkinlikleri.
- **Ortak** - Denetim amaçları için standart olaylar kümesi. Tam bir kullanıcı denetim izi bu kümeye dahildir. Örneğin, hem kullanıcı oturum açma hem de kullanıcı oturum açma olaylarını (olay 4624, 4634) içerir. Güvenlik grubu değişiklikleri, önemli etki alanı denetleyicisi Kerberos işlemleri ve kabul edilen en iyi uygulamalara uygun diğer olay türleri gibi denetim eylemleri de vardır.

    **Ortak** olay kümesi, çok yaygın olmayan bazı olay türleri içerebilir.  Bunun nedeni, **Ortak** kümenin ana noktasının olayların hacmini daha yönetilebilir bir düzeye düşürmek ve tam denetim izi yeteneğini korumak olmasıdır.

- **Minimal** - Olası tehditleri gösterebilecek küçük olaylar kümesi. Bu küme tam bir denetim izi içermez. Yalnızca başarılı bir ihlali gösteren olayları ve çok düşük oluşum oranlarına sahip diğer önemli olayları kapsar. Örneğin, başarılı ve başarısız kullanıcı oturum açmaları (olay işleri 4624, 4625) içerir, ancak denetim için ünemli olmakla birlikte ihlal algılaması için anlamlı olmamakla birlikte geçici olarak yüksek ses düzeyine sahip oturum danışma bilgileri (4634) içermez. Bu kümenin veri hacminin çoğu oturum açma olayları ve işlem oluşturma olaylarından (olay kimliği 4688) oluşur.

- **Yok** - Güvenlik veya AppLocker olayları yok. (Bu ayar bağlayıcıyı devre dışı kılabilir.)

    Aşağıdaki liste, her set için Güvenlik ve Uygulama Dolabı olay tam dökümünü sağlar:

    | Olay seti | Toplanan olay iT'leri |
    | --- | --- |
    | **En az** | 1102, 4624, 4625, 4657, 4663, 4688, 4700, 4702, 4719, 4720, 4722, 4723, 4724, 4727, 4728, 4732, 4735, 4737, 4739, 4740, 4754, 4755, 4756, 4767, 4799, 4825, 4946, 4948, 4956, 5024, 5033, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222 |
    | **Common** | 1, 299, 300, 324, 340, 403, 404, 410, 411, 412, 413, 431, 500, 501, 1100, 1102, 1107, 1108, 4608, 4610, 4611, 4614, 4622, 4624, 4625, 4634, 4647, 4648, 4649, 4657, 4661, 4662, 4663, 4665, 4666, 4667, 4688, 4670, 4672, 4673, 4674, 4675, 4689, 4697, 4700, 4702 , 4704, 4705, 4716, 4717, 4718, 4719, 4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4733, 4732, 4735, 4737, 4738, 4739, 4740, 4742, 4744, 4745, 4746, 4750, 4751, 4752, 4754, 4755, 4756, 4757, 4760, 4761, 4762, 4764, 4767, 4768, 4771, 4774, 4778, 4779, 4781, 4793 , 4797, 4798, 4799, 4800, 4801, 4802, 4803, 4825, 4826, 4870, 4886, 4887, 4888, 4893, 4898, 4902, 4904, 4905, 4907, 4931, 4932, 4933, 4946, 4948, 4956, 4985, 5024, 5033, 5059, 5136, 5137, 5140, 5145, 5632, 6144, 6145, 6272, 6273, 6278, 6416, 6423, 6424, 8001, 8002, 8003 , 8004, 8005, 8006, 8007, 8222, 26401, 30004 |

> [!NOTE]
> Tek bir çalışma alanı bağlamında Güvenlik Etkinlikleri koleksiyonu Azure Güvenlik Merkezi veya Azure Sentinel'den yapılandırılabilir, ancak her ikisi birden yapılandırılamaz. Azure Güvenlik Merkezi'ni zaten çalıştıran bir çalışma alanında Azure Sentinel'e biniyorsanız ve Güvenlik Etkinlikleri'ni toplamaya ayarlanmışsa, iki seçeneğiniz vardır:
> - Güvenlik Olayları koleksiyonunu olduğu gibi Azure Güvenlik Merkezi'nde bırakın. Bu etkinlikleri Azure Sentinel'in yanı sıra Azure Güvenlik Merkezi'nde sorgulayıp analiz edebilirsiniz. Ancak, bağlayıcının bağlantı durumunu izleyebilirsiniz veya Azure Sentinel'deki yapılandırmasını değiştiremezsiniz. Bu sizin için önemliyse, ikinci seçeneği göz önünde bulundurun.
>
> - Azure Güvenlik Merkezi'ndeki [Güvenlik Etkinlikleri koleksiyonunu devre dışı bırakıp,](../security-center/security-center-enable-data-collection.md) Yalnızca o zaman Azure Sentinel'e Güvenlik Olayları bağlayıcısı ekleyin. İlk seçenekte olduğu gibi, hem Azure Sentinel hem de Azure Güvenlik Merkezi'nde olayları sorgulayabilir ve çözümleyebilirsiniz, ancak artık bağlayıcının bağlantı durumunu izleyebilir veya yapılandırmasını Azure Sentinel'de ve yalnızca Azure Sentinel'de değiştirebilirsiniz.

## <a name="set-up-the-windows-security-events-connector"></a>Windows Güvenlik Olayları bağlayıcısını ayarlama

Windows güvenlik etkinliklerinizi Azure Sentinel'de toplamak için:

1. Azure Sentinel gezinti menüsünden **Veri bağlayıcılarını**seçin. Bağlayıcılar listesinden **Güvenlik Olayları'na**ve ardından sağ alttaki **Açık bağlayıcı sayfası** düğmesine tıklayın. Ardından, bu bölümün geri kalanında açıklandığı **gibi, Talimatlar** sekmesi altındaki ekrandaki yönergeleri izleyin.

1. **Önkoşullar**altında açıklandığı şekilde uygun izinlere sahip olduğunuzu doğrulayın.

1. Güvenlik olaylarını Azure Sentinel'e aktarmak istediğiniz makinelere [Log Analytics aracısını](../azure-monitor/platform/log-analytics-agent.md) (Microsoft İzleme Aracısı veya MMA olarak da bilinir) indirin ve yükleyin.

    Azure Sanal Makineler için:
    
    1. Azure **Windows Sanal Makine'deki Yükle aracısını**ve ardından aşağıdaki bağlantıyı tıklatın.
    1. Bağlanmak istediğiniz her sanal makine için, sağda görünen listede adını tıklayın ve sonra **Bağlan'ı**tıklatın.

    Azure olmayan Windows makineleri için (başka bir bulutta fiziksel, sanal önlisans veya sanal):

    1. Azure **Olmayan Windows Machine'deki Yükle aracısını**ve ardından aşağıdaki bağlantıyı tıklatın.
    1. **Windows Computers'ın**altında sağda görünen uygun indirme bağlantılarına tıklayın.
    1. İndirilen yürütülebilir dosyayı kullanarak aracıyı seçtiğiniz Windows sistemlerine yükleyin ve yukarıda belirtilen indirme bağlantılarının altında görünen **Çalışma Alanı Kimliği ve Tuşlarını** kullanarak yapılandırın.

    > [!NOTE]
    >
    > Gerekli internet bağlantısı olmayan Windows sistemlerinin olayları Azure Sentinel'e aktarmaya devam etmesine izin vermek için, sağ alttaki bağlantıyı kullanarak **OMS Ağ Geçidi'ni** ayrı bir makineye indirin ve yükleyin.  Etkinliklerini toplamak istediğiniz her Windows sistemine Log Analytics aracısını yüklemeniz gerekir.
    >
    > Bu senaryo hakkında daha fazla bilgi [ **için, Log Analytics ağ geçidi** belgelerine](../azure-monitor/platform/gateway.md)bakın.

    Ek kurulum seçenekleri ve daha fazla ayrıntı için [ **Log Analytics aracısı** belgelerine](../azure-monitor/platform/agent-windows.md)bakın.

1. Hangi olay kümesini[(Tümü, Ortak veya Minimal)](#event-sets)akışı istediğinizi seçin.

1. **Güncelleştir**’e tıklayın.

1. Windows güvenlik olayları için Log Analytics'teki ilgili `SecurityEvent` şemayı kullanmak için sorgu penceresine yazın.

## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Günlüklerinizin Log Analytics'te görünmeye başlaması yaklaşık 20 dakika sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Windows güvenlik olaylarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Yerleşik veya [özel](tutorial-detect-threats-custom.md) kurallar kullanarak Azure Sentinel ile tehditleri [algılamaya](tutorial-detect-threats-built-in.md) başlayın.

