---
title: Azure Güvenlik Merkezi öğreticisi-Azure Güvenlik Merkezi ile kaynaklarınızı koruyun | Microsoft Docs
description: Bu öğreticide, tam zamanında VM erişim ilkesinin ve uygulama denetim ilkesinin nasıl yapılandırılacağı gösterilir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/03/2018
ms.author: memildin
ms.openlocfilehash: 8cb07f3447e50528a94811f33a2142086f698586
ms.sourcegitcommit: 9f330c3393a283faedaf9aa75b9fcfc06118b124
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71996330"
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Öğretici: Azure Güvenlik Merkezi ile kaynaklarınızı koruyun
Güvenlik Merkezi, kötü amaçlı etkinlikleri engellemek için erişim ve uygulama denetimlerini kullanarak tehditlere maruz kalmayı sınırlandırır. Tam zamanında (JıT) sanal makine (VM) erişimi, VM 'lere kalıcı erişimi reddetmenizi sağlayarak saldırılara maruz kalmayı azaltır. Bunun yerine, VM 'lere yalnızca gerektiğinde denetimli ve denetlenen erişim sağlarsınız. Uyarlamalı uygulama denetimleri, VM 'leriniz üzerinde hangi uygulamaların çalıştırılacağını denetleyerek VM 'Lerin kötü amaçlı yazılımlara karşı korunmasına yardımcı olur. Güvenlik Merkezi, VM 'de çalışan işlemi çözümlemek için makine öğrenimini kullanır ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur.

Bu öğreticide aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Tam zamanında VM erişimi ilkesi yapılandırma
> * Uygulama denetim ilkesi yapılandırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Prerequisites
Bu öğreticide ele alınan özellikler arasında ilerlemek için, güvenlik merkezi 'nin standart fiyatlandırma katmanında olmanız gerekir. Güvenlik Merkezi Standart 'ı ücretsiz olarak deneyebilirsiniz. Daha fazla bilgi edinmek için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/security-center/)bakın. [Azure aboneliğinizi Güvenlik Merkezi standardına](security-center-get-started.md) ekleme hızlı başlangıç, standart sürümüne yükseltme konusunda size yol gösterir.

## <a name="manage-vm-access"></a>VM erişimini yönetme
JıT VM erişimi, Azure VM 'lerinize gelen trafiği kilitlemek ve gerektiğinde VM 'lere bağlanmak için kolay erişim sağlarken saldırılara maruz kalma olasılığını azaltmak için kullanılabilir.

Yönetim bağlantı noktalarının her zaman açık olması gerekmez. Yalnızca VM 'ye bağlıyken açık olmaları gerekir, örneğin, yönetim veya bakım görevlerini gerçekleştirmek için. Tam zamanında etkinleştirildiğinde Güvenlik Merkezi, ağ güvenlik grubu (NSG) kurallarını kullanır ve bu sayede, saldırganlar tarafından hedeflenememesi için yönetim bağlantı noktalarına erişimi kısıtlar.

1. Güvenlik Merkezi ana menüsünde **GELIŞMIŞ bulut savunması**altında **tam zamanında VM erişimi** ' ni seçin.

   ![Tam zamanında VM erişimi][1]

   **Tam ZAMANıNDA VM erişimi** , VM 'lerinizin durumu hakkında bilgi sağlar:

   - **Yapılandırıldı** -tam zamanında VM erişimini destekleyecek şekilde yapılandırılmış VM 'ler.
   - Tam zamanında VM erişimini destekleyebilen, ancak olarak yapılandırılmamış VM 'ler **önerilir** .
   - **Öneri yok** -bir VM 'nin Önerilmemesine neden olabilecek nedenler şunlardır:

     - Eksik NSG-tam zamanında çözüm, bir NSG 'nin yerinde olmasını gerektirir.
     - Klasik VM-Güvenlik Merkezi tam zamanında VM erişimi şu anda yalnızca Azure Resource Manager aracılığıyla dağıtılan VM 'Leri desteklemektedir.
     - Diğer-bir sanal makine, aboneliğin veya kaynak grubunun güvenlik ilkesinde tam zamanında çözüm kapatılmışsa veya VM 'nin genel IP 'si eksikse ve bir NSG yoksa, bu kategoride olur.

2. Önerilen bir VM seçin ve bu VM için tam zamanında bir ilke yapılandırmak üzere **1 VM 'de JIT 'ı etkinleştir** ' e tıklayın:

   Güvenlik Merkezi 'nin önerdiği varsayılan bağlantı noktalarını kaydedebilir veya tam zamanında çözümü etkinleştirmek istediğiniz yeni bir bağlantı noktası ekleyip yapılandırabilirsiniz. Bu öğreticide **Ekle**' yi seçerek bir bağlantı noktası ekleyelim.

   ![Bağlantı noktası yapılandırması Ekle][2]

3. **Bağlantı noktası yapılandırması Ekle**altında şunları belirlersiniz:

   - Bağlantı noktası
   - Protokol türü
   - İzin verilen kaynak IP 'Leri-onaylanan bir istek üzerine erişim izni verilen IP aralıkları
   - Belirli bir bağlantı noktasının açıladüğü maksimum istek süresi (maksimum zaman penceresi

4. Kaydetmek için **Tamam ' ı** seçin.

## <a name="harden-vms-against-malware"></a>VM 'Lerin kötü amaçlı yazılımlara karşı sağlamlaştırılmasına
Uyarlamalı uygulama denetimleri, yapılandırılmış kaynak gruplarında çalışmasına izin verilen bir uygulamalar kümesi tanımlamanıza yardımcı olur. Bu, diğer avantajlar arasında sanal makinelerinizin kötü amaçlı yazılımlara karşı korunmasına yardımcı olur. Güvenlik Merkezi, VM 'de çalışan işlemi çözümlemek için makine öğrenimini kullanır ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur.

1. Güvenlik Merkezi ana menüsüne geri dönün. **GELIŞMIŞ bulut savunması**altında **Uyarlamalı uygulama denetimleri**' ni seçin.

   ![Uyarlamalı uygulama denetimleri][3]

   **Kaynak grupları** bölümünde üç sekme bulunur:

   - **Yapılandırıldı**: uygulama denetimiyle yapılandırılan VM 'leri içeren kaynak gruplarının listesidir.
   - **Önerilen**: uygulama denetiminin önerildiği kaynak gruplarının listesidir.
   - **Öneri yok**: herhangi bir uygulama denetimi önerisi olmadan VM 'leri içeren kaynak gruplarının listesi. Örneğin, uygulamaların her zaman değişen ve kararlı olmayan bir duruma ulaşmamış VM 'Ler.

2. Uygulama denetimi önerilerini içeren kaynak gruplarının listesi için **Önerilen** sekmeyi seçin.

   ![Uygulama denetimi önerileri][4]

3. **Uygulama denetim kuralları oluştur** seçeneğini açmak için bir kaynak grubu seçin. **VM 'Leri Seç**bölümünde, önerilen VM 'lerin listesini gözden geçirin ve uygulama denetimini uygulamak istemediğiniz herhangi bir onay işaretini kaldırın. **Beyaz listeye yönelik Işlem seçme kuralları**' nda, önerilen uygulamaların listesini gözden geçirin ve uygulamak istemediğiniz her türlü onay işaretini kaldırın. Liste şunları içerir:

   - **Ad**: tam uygulama yolu
   - **Süreçler**: her yol içinde kaç uygulama bulunur
   - **Ortak**: "Evet", bu işlemlerin bu kaynak grubundaki çoğu VM üzerinde yürütüldüğünü belirtir
   - **Açıktan yararlanma**: bir uyarı simgesi, uygulamaların bir saldırgan tarafından uygulama beyaz listesini atlayıp kullanılamayacağını gösterir. Bu uygulamaların onayını önce gözden geçirilmesi önerilir.

4. Seçimlerinizi tamamladıktan sonra **Oluştur**' u seçin.

## <a name="clean-up-resources"></a>Kaynakları Temizleme
Bu koleksiyondaki diğer hızlı başlangıçler ve öğreticiler bu hızlı başlangıç üzerine inşa ediyor. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, standart katmanı çalıştırmaya devam edin ve otomatik sağlamayı etkin tutun. Devam etmeyi planlamıyorsanız veya ücretsiz katmana dönmek istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **güvenlik ilkesi**' ni seçin.
2. Ücretsiz olarak geri dönmek istediğiniz aboneliği veya ilkeyi seçin. **Güvenlik ilkesi** açılır.
3. **Ilke bileşenleri**altında **fiyatlandırma katmanı**' nı seçin.
4. Standart katmandan ücretsiz katmana abonelik değiştirmek için **ücretsiz** ' ı seçin.
5. **Kaydet**' i seçin.

Otomatik sağlamayı devre dışı bırakmak istiyorsanız:

1. Güvenlik Merkezi ana menüsüne dönüp **güvenlik ilkesi**' ni seçin.
2. Otomatik sağlamayı devre dışı bırakmak istediğiniz aboneliği seçin.
3. **Güvenlik ilkesi – veri toplama**altında, otomatik sağlamayı devre dışı bırakmak için **ekleme** altında **kapalı** ' yı seçin.
4. **Kaydet**' i seçin.

>[!NOTE]
> Otomatik sağlamayı devre dışı bırakmak, aracının sağlandığı Azure VM 'lerinden Microsoft Monitoring Agent kaldırmaz. Otomatik sağlama devre dışı bırakıldığında kaynaklarınız için güvenlik izleme sınırı vardır.
>

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, tehditlere maruz kalma riskini nasıl sınırlandırmayı öğrendiniz:

> [!div class="checklist"]
> * Yalnızca gerektiğinde VM 'lere denetimli ve denetlenen erişim sağlamak için tam zamanında VM erişim ilkesi yapılandırma
> * Bir uyarlamalı uygulama denetimleri ilkesini, sanal makinelerinizdeki hangi uygulamaların çalıştırılacağını denetlemek için yapılandırma

Güvenlik olaylarına yanıt verme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: güvenlik olaylarına yanıt verme](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png
