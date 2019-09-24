---
title: Azure Güvenlik Merkezi 'ndeki önerileri düzeltin | Microsoft Docs
description: Bu belgede, Azure Güvenlik Merkezi 'nde Azure kaynaklarınızı korumanıza ve güvenlik ilkeleriyle uyumlu kalmanıza yardımcı olması için önerilerin nasıl düzeltileceği açıklanmaktadır.
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
ms.date: 08/18/2019
ms.author: memildin
ms.openlocfilehash: 9bd1586193d2e36c370217e37b77409298821a67
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71201008"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi 'ndeki önerileri düzeltin

Öneriler, kaynaklarınızın daha iyi güvenliğini sağlamaya yönelik öneriler sağlar.  Öneri bölümünde belirtilen düzeltme adımlarını izleyerek bir öneri uygulıyordu. 

## Düzeltme adımları<a name="remediation-steps"></a>

Tüm önerileri inceledikten sonra, hangisi önce düzeltileceğine karar verin. Öncelikle ne yapacaklarınızı önceliklendirmeye yardımcı olması için [güvenli puan etkisini](security-center-recommendations.md#monitor-recommendations) kullanmanızı öneririz.

1. Listeden öneriye tıklayın.
1. **Düzeltme adımları** bölümündeki yönergeleri izleyin. Her öneri kendi yönerge kümesine sahiptir. Aşağıda, uygulamaları yalnızca HTTPS üzerinden trafiğe izin verecek şekilde yapılandırmaya yönelik düzeltme adımları gösterilmektedir.

    ![Öneri ayrıntıları](./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png)

1. Tamamlandıktan sonra, düzeltme başarılı olursa bildirim görüntülenir.

## Tek tıklama düzeltme düzeltmesi (Önizleme)<a name="one-click"></a>

Tek tıklamayla yapılan bir düzeltme, tek bir tıklama ile bir kaynak toplu için öneriyi düzeltmenizi sağlar. Yalnızca belirli önerilerin kullanımına sunulan bir seçenektir. Tek tıklamayla düzeltme, düzeltmeyi basitleştirir ve ortamınızdaki güvenliği artırarak güvenli Puanınızı artırabilir.

Tek tıklamalı düzeltme uygulamak için:

1. **1 tıkladım** etiketi olan öneriler listesinden öneriye tıklayın.  

   ![Tek tıklama düzeltmesini seçin](./media/security-center-remediate-recommendations/security-center-one-click-fix-select.png)

2. **Sağlıksız kaynaklar** sekmesinden, öneriyi uygulamak istediğiniz kaynakları seçin ve **Düzelt**' e tıklayın. 

    > [!NOTE]
    > Listelenen bazı kaynaklar devre dışı bırakılabilir, çünkü bunları değiştirmek için uygun izinleriniz yok.

3. Onay kutusunda düzeltme ayrıntılarını ve etkilerini okuyun. 

   ![Tek tıklamayla düzeltilmesi](./media/security-center-remediate-recommendations/security-center-one-click-fix-view.png)

    > [!NOTE]
    > Bu etkiler, **Düzelt**' i tıkladıktan sonra açılan **kaynakları Düzelt** penceresindeki gri kutusunda listelenir. 1-tıklamayı devam etmekte olan değişikliklerin ne olduğunu listeler.

4. Gerekirse ilgili parametreleri ekleyin ve düzeltmeyi onaylayın.

    > [!NOTE]
    > **Sağlıklı kaynaklar** sekmesindeki kaynakları görmek için düzeltme tamamlandıktan sonra işlem birkaç dakika sürebilir. Düzeltme eylemlerini görüntülemek için [etkinlik günlüğünü](#activity-log)kontrol edin.

5. Tamamlandıktan sonra, düzeltme başarılı olursa bildirim görüntülenir.

## Etkinlik günlüğünde tek tıklamayla düzeltme günlüğü<a name="activity-log"></a>

Düzeltme işlemi, kaynak üzerinde yapılandırmayı uygulamak için bir şablon dağıtımı veya REST PATCH API çağrısı kullanır. Bu işlemler [Azure etkinlik günlüğünde](../azure-resource-manager/resource-group-audit.md)günlüğe kaydedilir.


## <a name="recommendations-with-one-click-remediation"></a>Tek tıklama düzeltmeli öneriler

|Öneri|Olsa|
|---|---|
|SQL Server 'lar üzerinde denetim etkinleştirilmelidir|Bu eylem, bu sunucularda ve veritabanlarında SQL denetimini etkinleştirir. <br>**Not**: <ul><li>Seçili SQL sunucularının her bir bölgesi için, Denetim günlüklerini kaydetmek üzere bir depolama hesabı bu bölgedeki tüm sunucular tarafından oluşturulur ve paylaşılır.</li><li>Doğru denetim sağlamak için kaynak grubunu veya depolama hesaplarını silmeyin veya yeniden adlandırmayın.</li></ul>|
|Gelişmiş veri güvenliği, SQL yönetilen örneklerinizin üzerinde etkinleştirilmelidir|Bu eylem, seçili SQL yönetilen örneklerinde SQL gelişmiş veri güvenliğini (ADS) etkinleştirir. <br>**Not**: <ul><li>Seçilen SQL yönetilen örneklerinin her bölgesi ve kaynak grubu için, tarama sonuçlarını kaydetmek üzere bir depolama hesabı bu bölgedeki tüm örnekler tarafından oluşturulur ve paylaşılır.</li><li> REKLAMLARı SQL tarafından yönetilen örnek başına $15 üzerinden ücretlendirilir.</li></ul>|
|SQL yönetilen örneklerinizin üzerinde güvenlik açığı değerlendirmesi etkinleştirilmelidir|Bu eylem, seçili SQL yönetilen örneklerinde SQL güvenlik açığı değerlendirmesini etkinleştirir. <br>**Not**:<ul><li>SQL güvenlik açığı değerlendirmesi, SQL gelişmiş veri güvenliği (ADS) paketinin bir parçasıdır. REKLAMLAR etkin değilse, yönetilen örnekte otomatik olarak etkinleştirilir.</li><li>Seçilen SQL yönetilen örneklerinin her bir bölgesi ve kaynak grubu için, tarama sonuçlarını depolamak için bir depolama hesabı bu bölgedeki tüm örnekler tarafından oluşturulur ve paylaşılır.</li><li>REKLAMLARı SQL Server başına $15 üzerinden ücretlendirilir.</li></ul>||
|Gelişmiş veri güvenliği SQL sunucularınızda etkinleştirilmelidir|Bu eylem, bu seçili sunucularda ve veritabanlarında gelişmiş veri güvenliği (ADS) sağlar. <br>**Not**:<ul><li>Seçili SQL sunucularının her bir bölgesi ve kaynak grubu için, tarama sonuçlarını depolamak için bir depolama hesabı bu bölgedeki tüm sunucular tarafından oluşturulur ve paylaşılır. <</li><li>REKLAMLARı SQL Server başına $15 üzerinden ücretlendirilir.</li></ul>||
|Güvenlik açığı değerlendirmesi SQL sunucularınızda etkinleştirilmelidir|Bu eylem, bu seçili sunucularda ve veritabanlarında SQL güvenlik açığı değerlendirmesini etkinleştirir. <br>**Not**:<ul><li>SQL güvenlik açığı değerlendirmesi, SQL gelişmiş veri güvenliği (ADS) paketinin bir parçasıdır. REKLAMLARı zaten etkinleştirilmemişse, SQL Server 'da otomatik olarak etkinleştirilir.</li><li>Seçili SQL sunucularının her bir bölgesi ve kaynak grubu için, tarama sonuçlarını depolamak için bir depolama hesabı bu bölgedeki tüm örnekler tarafından oluşturulur ve paylaşılır.</li><li>REKLAMLARı SQL Server başına $15 üzerinden ücretlendirilir.</li></ul>||
|SQL veritabanlarında saydam veri şifrelemesi etkinleştirilmelidir|Bu eylem, seçili veritabanlarında SQL veritabanı Saydam Veri Şifrelemesi (TDE) etkinleştirilir. <br>**Not**: Varsayılan olarak, hizmet tarafından yönetilen TDE anahtarları kullanılacaktır. 
|Depolama hesaplarına Güvenli aktarım etkinleştirilmelidir|Bu eylem, depolama hesabı güveninizi yalnızca güvenli bağlantılar tarafından isteklere izin verecek şekilde güncelleştirir. (HTTPS). <br>**Not**:<ul><li>HTTP kullanan istekler reddedilir.</li><li>Azure dosyaları hizmetini kullanırken, SMB 2,1, şifreleme olmadan SMB 3,0 ve Linux SMB istemcisinin bazı türleri dahil olmak üzere şifreleme olmadan bağlantı başarısız olur.  Daha fazla bilgi.</li></ul>|
|Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu eylem, tüm trafiği seçili kaynaklarda HTTP 'den HTTPS 'ye yönlendirir. <br>**Not**:<ul><li>SSL sertifikası olmayan bir HTTPS uç noktası tarayıcıda ' Gizlilik hatası ' ile görünür. Bu nedenle, özel bir etki alanı olan kullanıcıların bir SSL sertifikası ayarladıklarından emin olması gerekir.</li><li>Uygulama hizmetini koruyan paket ve Web uygulaması güvenlik duvarlarının HTTPS oturumlarının iletilmesine izin ver ' i doğrulayın.</li></ul>|
|İşlev uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu eylem, tüm trafiği seçili kaynaklarda HTTP 'den HTTPS 'ye yönlendirir. <br>**Not**:<ul><li>SSL sertifikası olmayan bir HTTPS uç noktası tarayıcıda ' Gizlilik hatası ' ile görünür. Bu nedenle, özel bir etki alanı olan kullanıcıların bir SSL sertifikası ayarladıklarından emin olması gerekir.</li><li>Uygulama hizmetini koruyan paket ve Web uygulaması güvenlik duvarlarının HTTPS oturumlarının iletilmesine izin ver ' i doğrulayın.</li></ul>|
|API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Bu eylem, tüm trafiği seçili kaynaklarda HTTP 'den HTTPS 'ye yönlendirir. <br>**Not**:<ul><li>SSL sertifikası olmayan bir HTTPS uç noktası tarayıcıda ' Gizlilik hatası ' ile görünür. Bu nedenle, özel bir etki alanı olan kullanıcıların bir SSL sertifikası ayarladıklarından emin olması gerekir.</li><li>Uygulama hizmetini koruyan paket ve Web uygulaması güvenlik duvarlarının HTTPS oturumlarının iletilmesine izin ver ' i doğrulayın.</li></ul>|
|Uzaktan hata ayıklama için Web uygulaması kapalı olmaları|Bu eylem, uzaktan hata ayıklamayı devre dışı bırakır.|
|İşlev Uygulaması için uzaktan hata ayıklama kapatılmalıdır|Bu eylem, uzaktan hata ayıklamayı devre dışı bırakır.|
|API uygulaması için uzaktan hata ayıklama kapatılmalıdır|Bu eylem, uzaktan hata ayıklamayı devre dışı bırakır.|
|CORS, her kaynağın Web uygulamanıza erişmesine izin vermemelidir|Bu eylem, diğer etki alanlarının Web uygulamanıza erişmesini engeller. Belirli etki alanlarına izin vermek için, bu kaynakları Izin verilen Kaynaklar alanına (virgülle ayırarak) girin. <br>**Not**: Alanı boş bırakmak, tüm çapraz kaynak çağrılarını engeller. ' param alan başlığı: ' İzin verilen çıkış '|
|CORS her kaynağın işlev uygulamanıza erişmek izin vermemelidir|Bu eylem, diğer etki alanlarının Işlev uygulamanıza erişmesini engeller. Belirli etki alanlarına izin vermek için, bu kaynakları Izin verilen Kaynaklar alanına (virgülle ayırarak) girin. <br>**Not**: Alanı boş bırakmak, tüm çapraz kaynak çağrılarını engeller. ' param alan başlığı: ' İzin verilen çıkış '|
|CORS, her kaynağın API uygulamanıza erişmesine izin vermemelidir|Bu eylem, diğer etki alanlarının API uygulamanıza erişmesini engeller. Belirli etki alanlarına izin vermek için, bu kaynakları Izin verilen Kaynaklar alanına (virgülle ayırarak) girin. <br>**Not**: Alanı boş bırakmak, tüm çapraz kaynak çağrılarını engeller. ' param alan başlığı: ' İzin verilen çıkış '|
|İzleme Aracısı sanal makinelerinizde etkinleştirilmelidir|Bu eylem, seçili sanal makinelere bir izleme Aracısı kurar. Aracının raporlamak için bir çalışma alanı seçin.<ul><li>Güncelleştirme ilkeniz otomatik olarak ayarlandıysa, yeni mevcut örneklere dağıtılır.</li><li>Güncelleştirme ilkeniz el ile olarak ayarlandıysa ve aracıyı mevcut örneklere yüklemek isterseniz onay kutusu seçeneğini belirleyin.  [Daha fazla bilgi edinin](../virtual-machine-scale-sets/virtual-machine-scale-sets-faq.md#how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set)</li></ul>|
|Key Vault tanılama günlükleri etkinleştirilmelidir|Bu eylem, anahtar kasalarında tanılama günlüklerini sunar. Tanılama günlükleri ve ölçümleri seçili çalışma alanına kaydedilir.|
|Service Bus 'daki tanılama günlükleri etkinleştirilmelidir|Bu eylem, Service Bus 'da tanılama günlüklerini sunar. Tanılama günlükleri ve ölçümleri seçili çalışma alanına kaydedilir.|

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, güvenlik merkezi 'nde önerilerin nasıl düzeltileceği gösterildi. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md): Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md): Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
