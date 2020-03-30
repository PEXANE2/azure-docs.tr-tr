---
title: Azure Güvenlik Merkezi'nde düzeltici öneriler | Microsoft Dokümanlar
description: Bu makalede, kaynaklarınızı korumak ve güvenlik ilkelerine uymak için Azure Güvenlik Merkezi'ndeki önerileri nasıl düzeltiniz açıklanmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 8be947cc-cc86-421d-87a6-b1e23077fd50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 6863f4bbfc6aabc4e7bf4314ad26dd4590d31362
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603507"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde düzeltme önerileri

Öneriler, kaynaklarınızı daha iyi nasıl güvence altına alabildiğiniz konusunda öneriler de sunar. Öneride sağlanan düzeltme adımlarını izleyerek bir öneri uygularsınız.

## <a name="remediation-steps"></a>Düzeltme adımları<a name="remediation-steps"></a>

Tüm önerileri gözden geçirdikten sonra, ilk olarak hangisini düzelteceğine karar verin. Önce ne yapmanız gerektiğine öncelik vermek için [Güvenli Puan etkisini](security-center-recommendations.md#monitor-recommendations) kullanmanızı öneririz.

1. Listeden, öneriyi tıklatın.

1. **Düzeltme adımları** bölümündeki yönergeleri izleyin. Her önerinin kendi yönergeleri vardır. Aşağıdaki ekran görüntüsü, uygulamaları yalnızca HTTPS üzerinden trafiğe izin verecek şekilde yapılandırmak için düzeltme adımlarını gösterir.

    ![Öneri ayrıntıları](./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png)

1. Tamamlandıktan sonra, düzeltme başarılı olursa sizi bilgilendiren bir bildirim görüntülenir.

## <a name="quick-fix-remediation"></a>Hızlı Düzeltme düzeltmesi<a name="one-click"></a>

Hızlı Düzeltme, birden çok kaynakla ilgili bir öneriyi hızlı bir şekilde düzeltmenizi sağlar. Yalnızca belirli öneriler için kullanılabilir. Hızlı Düzeltme, düzeltmeyi basitleştirir ve güvenli puanınızı hızla artırarak ortamınızın güvenliğini artırmanızı sağlar.

Hızlı Düzeltme düzeltmesini uygulamak için:

1. Hızlı Düzeltme olan öneriler **listesinden!** etiket, tavsiye üzerine tıklayın.

    [![Hızlı Düzeltme'yi seçin!](media/security-center-remediate-recommendations/security-center-one-click-fix-select.png)](media/security-center-remediate-recommendations/security-center-one-click-fix-select.png#lightbox)

1. Sağlıksız **kaynaklar** sekmesinden, öneriyi uygulamak istediğiniz kaynakları seçin ve **Düzelt'i**tıklatın.

    > [!NOTE]
    > Listelenen kaynaklardan bazıları devre dışı bırakılmış olabilir, çünkü bunları değiştirmek için uygun izinlere sahip değilsiniz.

1. Onay kutusunda, düzeltme ayrıntılarını ve sonuçlarını okuyun.

    ![Hızlı Düzeltme](./media/security-center-remediate-recommendations/security-center-one-click-fix-view.png)

    > [!NOTE]
    > Etkileri, **Düzelt'i**tıklattıktan sonra açılan **Düzelt kaynakları** penceresindeki gri kutuda listelenir. Hızlı Düzeltme düzeltmesi ile devam ederken ne değişikliklerin olduğunu listelerler.

1. Gerekirse ilgili parametreleri ekleyin ve düzeltmeyi onaylayın.

    > [!NOTE]
    > Düzeltme tamamlandıktan sonra **Sağlıklı kaynaklar** sekmesindeki kaynakları görmek birkaç dakika sürebilir. Düzeltme eylemlerini görüntülemek için [etkinlik günlüğünü](#activity-log)denetleyin.

1. Tamamlandıktan sonra, düzeltme başarılı olursa sizi bilgilendiren bir bildirim görüntülenir.

## <a name="quick-fix-remediation-logging-in-the-activity-log"></a>Etkinlik günlüğünde Hızlı Düzeltme düzeltme günlüğü<a name="activity-log"></a>

Düzeltme işlemi, yapılandırmayı kaynağa uygulamak için bir şablon dağıtımı veya REST PATCH API çağrısı nı kullanır. Bu işlemler Azure [etkinlik günlüğüne](../azure-resource-manager/management/view-activity-logs.md)kaydedilir.


## <a name="recommendations-with-quick-fix-remediation"></a>Hızlı Düzeltme düzeltmesi ile öneriler

|Öneri|Dolaylı|
|---|---|
|SQL sunucularında denetim etkinleştirilmelidir|Bu eylem, bu sunucularda ve veritabanlarında SQL denetimine olanak sağlar. <br>**Not**: <ul><li>Seçili SQL sunucularının her bölgesi için denetim günlüklerini kaydetmek için bir depolama hesabı oluşturulur ve o bölgedeki tüm sunucular tarafından paylaşılır.</li><li>Uygun denetimi sağlamak için kaynak grubunu veya depolama hesaplarını silmeyin veya yeniden adlandırmayın.</li></ul>|
|SQL yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir|Bu eylem, seçilen SQL yönetilen örneklerinde SQL Advanced Data Security (ADS) sağlar. <br>**Not**: <ul><li>Seçili SQL yönetilen örneklerinin her bölge ve kaynak grubu için, scan sonuçlarını kaydetmek için bir depolama hesabı oluşturulur ve bu bölgedeki tüm örnekler tarafından paylaşılır.</li><li> ADS, SQL yönetilen örnek başına 15 TL olarak ücretlendirilir.</li></ul>|
|SQL yönetilen örneklerinizde güvenlik açığı değerlendirmesi etkinleştirilmelidir|Bu eylem, seçilen SQL yönetilen örneklerinde SQL Güvenlik Açığı Değerlendirmesi'ni etkinleştirecektir. <br>**Not**:<ul><li>SQL Güvenlik Açığı Değerlendirmesi, SQL Advanced Data Security (ADS) paketinin bir parçasıdır. ADS zaten etkinleştirilmezse, yönetilen örnekte otomatik olarak etkinleştirilir.</li><li>Seçili SQL yönetilen örneklerinin her bölge ve kaynak grubu için, scan sonuçlarını depolamak için bir depolama hesabı oluşturulur ve o bölgedeki tüm örnekler tarafından paylaşılır.</li><li>ADS, SQL sunucusu başına 15 TL olarak ücretlendirilir.</li></ul>||
|Gelişmiş Veri Güvenliği SQL sunucularınızda etkinleştirilmelidir|Bu eylem, bu seçili sunucularda ve veritabanlarında Gelişmiş Veri Güvenliği 'ni (ADS) etkinleştirecektir. <br>**Not**:<ul><li>Seçili SQL sunucularının her bölge ve kaynak grubu için, taraya sonuçları depolamak için bir depolama hesabı oluşturulacak ve o bölgedeki tüm sunucular tarafından paylaşılacaktır.<</li><li>ADS, SQL sunucusu başına 15 TL olarak ücretlendirilir.</li></ul>||
|SQL sunucularınızda Güvenlik Açığı Değerlendirmesi etkinleştirilmelidir|Bu eylem, bu seçili sunucularda ve veritabanlarında SQL Güvenlik Açığı Değerlendirmesi'ni etkinleştirecektir. <br>**Not**:<ul><li>SQL Güvenlik Açığı Değerlendirmesi, SQL Advanced Data Security (ADS) paketinin bir parçasıdır. ADS zaten etkinleştirilmezse, SQL sunucusunda otomatik olarak etkinleştirilir.</li><li>Seçili SQL sunucularının her bölge ve kaynak grubu için, scan sonuçlarını depolamak için bir depolama hesabı oluşturulur ve o bölgedeki tüm örnekler tarafından paylaşılır.</li><li>ADS, SQL sunucusu başına 15 TL olarak ücretlendirilir.</li></ul>||
|SQL veritabanlarında saydam veri şifrelemesi etkinleştirilmelidir|Bu eylem, seçili veritabanlarında SQL Veritabanı Saydam Veri Şifreleme (TDE) sağlar. <br>**Not**: Varsayılan olarak, hizmet yönetilen TDE tuşları kullanılacaktır.
|Depolama hesaplarına güvenli aktarım etkinleştirilmelidir|Bu eylem, depolama hesabı güvenliğinizi yalnızca güvenli bağlantılarla isteklere izin verecek şekilde güncelleştirir. (HTTPS) olarak tanımlanır. <br>**Not**:<ul><li>HTTP kullanan tüm istekler reddedilecektir.</li><li>Azure dosyaları hizmetini kullanırken, SMB 2.1, SMB 3.0 şifrelemesiz senaryolar ve Linux SMB istemcisinin bazı tatları da dahil olmak üzere şifrelemesiz bağlantı başarısız olur. Daha fazla bilgi edinin.</li></ul>|
|Web Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu eylem, seçilen kaynaklardaki TÜM trafiği HTTP'den HTTPS'ye yönlendirir. <br>**Not**:<ul><li>SSL sertifikası olmayan bir HTTPS bitiş noktası tarayıcıda 'Gizlilik Hatası' ile birlikte gösterilecektir. Bu nedenle, özel bir etki alanına sahip kullanıcıların bir SSL sertifikası kurduklarını doğrulamaları gerekir.</li><li>Uygulama hizmetini koruyan paket ve web uygulaması güvenlik duvarlarını koruyun, HTTPS oturumlarının iletilmesine izin verin.</li></ul>|
|Fonksiyon Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu eylem, seçilen kaynaklardaki TÜM trafiği HTTP'den HTTPS'ye yönlendirir. <br>**Not**:<ul><li>SSL sertifikası olmayan bir HTTPS bitiş noktası tarayıcıda 'Gizlilik Hatası' ile birlikte gösterilecektir. Bu nedenle, özel bir etki alanına sahip kullanıcıların bir SSL sertifikası kurduklarını doğrulamaları gerekir.</li><li>Uygulama hizmetini koruyan paket ve web uygulaması güvenlik duvarlarını koruyun, HTTPS oturumlarının iletilmesine izin verin.</li></ul>|
|API Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu eylem, seçilen kaynaklardaki TÜM trafiği HTTP'den HTTPS'ye yönlendirir. <br>**Not**:<ul><li>SSL sertifikası olmayan bir HTTPS bitiş noktası tarayıcıda 'Gizlilik Hatası' ile birlikte gösterilecektir. Bu nedenle, özel bir etki alanına sahip kullanıcıların bir SSL sertifikası kurduklarını doğrulamaları gerekir.</li><li>Uygulama hizmetini koruyan paket ve web uygulaması güvenlik duvarlarını koruyun, HTTPS oturumlarının iletilmesine izin verin.</li></ul>|
|Web Uygulaması için uzaktan hata ayıklama kapatılmalıdır|Bu eylem uzaktan hata ayıklama devre dışı kalmaktadır.|
|İşlev Uygulaması için uzaktan hata ayıklama kapatılmalıdır|Bu eylem uzaktan hata ayıklama devre dışı kalmaktadır.|
|API Uygulaması için uzaktan hata ayıklama kapatılmalıdır|Bu eylem uzaktan hata ayıklama devre dışı kalmaktadır.|
|CORS her kaynağın Web Uygulamanıza erişmesine izin vermemelidir|Bu eylem, diğer etki alanlarının Web Uygulamanıza erişmelerini engeller. Belirli etki adlarına izin vermek için, bunları İzin Verilen başlangıçlar alanına girin (virgülle ayrılmıştır). <br>**Not**: Alanı boş bırakmak tüm çapraz orijinli çağrıları engeller.'Param alan başlığı: 'İzin verilen başlangıçlar'|
|CORS, her kaynağın İşlev Uygulamanıza erişmesine izin vermemelidir|Bu eylem, diğer etki alanlarının İşlev Uygulamanıza erişmelerini engeller. Belirli etki adlarına izin vermek için, bunları İzin Verilen başlangıçlar alanına girin (virgülle ayrılmıştır). <br>**Not**: Alanı boş bırakmak tüm çapraz orijinli çağrıları engeller.'Param alan başlığı: 'İzin verilen başlangıçlar'|
|CORS, her kaynağın API Uygulamanıza erişmesine izin vermemelidir|Bu eylem, diğer etki alanlarının API Uygulamanıza erişmelerini engeller. Belirli etki adlarına izin vermek için, bunları İzin Verilen başlangıçlar alanına girin (virgülle ayrılmıştır). <br>**Not**: Alanı boş bırakmak tüm çapraz orijinli çağrıları engeller.'Param alan başlığı: 'İzin verilen başlangıçlar'|
|Sanal makinelerinizde izleme aracısı etkinleştirilmelidir|Bu eylem, seçili sanal makinelere bir izleme aracısı yükler. Aracının rapor etmesi için bir çalışma alanı seçin.<ul><li>Güncelleştirme ilkeniz otomatik olarak ayarlanmışsa, varolan yeni örneklerde dağıtılır.</li><li>Güncelleştirme ilkeniz el ile ayarlanmışsa ve aracıyı varolan örneklere yüklemek istiyorsanız, onay kutusu seçeneğini belirleyin. [Daha fazlasını öğrenin](../virtual-machine-scale-sets/virtual-machine-scale-sets-faq.md#how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set)</li></ul>|
|Key Vault'taki tanılama günlükleri etkinleştirilmeli|Bu eylem, anahtar tonozlarında tanılama günlükleri sağlar. Tanılama günlükleri ve ölçümleri seçili çalışma alanına kaydedilir.|
|Servis veri kutusundaki tanılama günlükleri etkinleştirilmeli|Bu eylem, servis veri yolundatanı günlükleri sağlar. Tanılama günlükleri ve ölçümleri seçili çalışma alanına kaydedilir.|

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Güvenlik Merkezi'ndeki önerileri nasıl düzeltebildiğiniz gösterilmiştir. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkeleri belirleme](tutorial-security-policy.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini nasıl yapılandırıştıracaklarınızı öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
