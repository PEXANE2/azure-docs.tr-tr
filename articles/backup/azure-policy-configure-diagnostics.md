---
title: Kasa Tanılama ayarlarını ölçekte yapılandırın
description: Azure İlkesi'ni kullanarak belirli bir kapsamdaki tüm kasalar için Log Analytics Diagnostics ayarlarını yapılandırın
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: c92957cab3e1ed745e7031e3c6f32e7ecda550a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77584532"
---
# <a name="configure-vault-diagnostics-settings-at-scale"></a>Kasa Tanılama ayarlarını ölçekte yapılandırın

Azure Yedekleme tarafından sağlanan raporlama çözümü Log Analytics'ten (LA) yararlanır. Herhangi bir kasanın verilerinin LOS Angeles'a gönderilmesi için, bu kasa için bir [tanılama ayarının](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events) oluşturulması gerekiyor.

Genellikle, kasa başına el ile bir tanılama ayarı eklemek hantal bir görev olabilir. Buna ek olarak, oluşturulan herhangi bir yeni kasa da bu tonoz için raporları görüntülemek edebilmek için tanılama ayarları etkin olması gerekir. 

Azure Yedekleme, tanılama ayarlarının ölçek olarak oluşturulmasını kolaylaştırmak için (hedef olarak LA ile birlikte), yerleşik bir [Azure İlkesi](https://docs.microsoft.com/azure/governance/policy/)sağlar. Bu ilke, belirli bir abonelik veya kaynak grubundaki tüm kasalara bir LA tanılama ayarı ekler. Aşağıdaki bölümlerde bu iipolitikasın nasıl kullanılacağına ilişkin yönergeler sağlanmıştır.

## <a name="supported-scenarios"></a>Desteklenen Senaryolar

* Bu ilke, belirli bir abonelikteki tüm Kurtarma Hizmetleri kasalarına (veya abonelik içindeki bir kaynak grubuna) aynı anda uygulanabilir. İlkeyi atayan kullanıcının, ilkenin atandığı aboneliğe 'Sahip' erişimine sahip olması gerekir.

* Kullanıcı tarafından belirtilen LA Çalışma Alanı (tanılama verilerinin gönderilebildiği) ilkenin atandığı kasalardan farklı bir abonelikte olabilir. Kullanıcının belirtilen LA Çalışma Alanı'nın bulunduğu aboneliğe 'Reader', 'Katkıda Bulunan' veya 'Sahip' erişimine sahip olması gerekir.

* Yönetim Grubu kapsamı şu anda desteklenmiş.

* Yerleşik ilke şu anda ulusal bulutlarda kullanılamıyor.

## <a name="assigning-the-built-in-policy-to-a-scope"></a>Yerleşik ilkeyi bir kapsama atama

Gerekli kapsamdaki kasalar için ilke atamak için aşağıdaki adımları izleyin:

1. Azure portalında oturum açın ve **İlke** Panosu'na gidin.
2. Azure Kaynakları'ndaki tüm yerleşik ilkelerin listesini almak için sol menüdeki **Tanımlar'ı** seçin.
3. Kategori=İzleme için listeye filtre **uygulayın.** [Önizleme] adlı ilkeyi **bulun: Kurtarma Hizmetleri Kasası için Tanılama Ayarlarını Kaynak özel kategorileri için Analytics çalışma alanını günlüğe kaydetmek için dağıtın.**

![İlke Tanımı Blade](./media/backup-azure-policy-configure-diagnostics/policy-definition-blade.png)

4. İlkenin adını tıklatın. Bu ilke için ayrıntılı tanıma yönlendirilirsiniz.

![Ayrıntılı İlke Tanımı](./media/backup-azure-policy-configure-diagnostics/detailed-policy-definition.png)

5. Bıçağın üst kısmındaki **Atla** düğmesine tıklayın. Bu, sizi Atama **İlkesi** bıçağına yönlendirir.

6. **Temel bilgiler**altında, **Kapsam** alanının yanındaki üç noktanın üzerine tıklayın. Bu, uygulanacak ilke için aboneliği seçebileceğiniz doğru bir bağlam bıçağı açar. Ayrıca isteğe bağlı olarak bir kaynak grubu seçebilirsiniz, böylece ilke yalnızca belirli bir kaynak grubundaki kasalar için uygulanır.

![İlke Atama Temelleri](./media/backup-azure-policy-configure-diagnostics/policy-assignment-basics.png)

7. **Parametreler**altında, aşağıdaki bilgileri girin:

* **Profil Adı** - İlke tarafından oluşturulan tanılama ayarlarına atanacak ad.
* **Günlük Analizi Çalışma Alanı** - Tanılama ayarının ilişkilendirilmesi gereken Log Analytics Çalışma Alanı. İlke ataması kapsamındaki tüm kasaların tanılama verileri belirtilen LA Çalışma alanına itilir.

* **Dışlama Etiket Adı (isteğe bağlı) ve Dışlama Etiket Değeri (isteğe bağlı)** - İlke atamasından belirli bir etiket adı ve değer içeren kasaları hariç tutmayı seçebilirsiniz. Örneğin, 'evet' değerine 'isTest' kümesi ayarlanmış olan kasalara bir tanılama ayarı eklenmesini **istemiyorsanız,** Dışlama Etiket Adı alanına 'isTest' ve **Dışlama** **Etiketi** Değeri alanına 'evet' girmeniz gerekir. Bu iki alandan herhangi biri (veya her ikisi) boş bırakılırsa, ilke içerdikleri etiketlere bakılmaksızın ilgili tüm kasalara uygulanır.

![İlke Atama Parametreleri](./media/backup-azure-policy-configure-diagnostics/policy-assignment-parameters.png)

8. **Bir düzeltme görevi oluşturun** - İlke bir kapsama atandıktan sonra, bu kapsamda oluşturulan yeni kasalar otomatik olarak LA tanılama ayarlarını yapılandırılır (kasanın oluşturulduğu andan itibaren 30 dakika içinde). Kapsamdaki varolan kasalara tanılama ayarı eklemek için, ilke ataması zamanında bir düzeltme görevi tetikleyebilirsiniz. Düzeltme görevini tetiklemek için onay kutusunu seçin **Düzeltme görevi oluştur.** 

![İlke Atama Düzeltme](./media/backup-azure-policy-configure-diagnostics/policy-assignment-remediation.png)

9. **Gözden Geçir+Oluştur** sekmesine gidin ve **Oluştur'u**tıklatın.

## <a name="under-what-conditions-will-the-remediation-task-apply-to-a-vault"></a>Düzeltme görevi hangi koşullar altında kasaya uygulanacak?

Düzeltme görevi, ilkeğin tanımına göre uyumlu olmayan kasalara uygulanır. Bir kasa, aşağıdaki koşullardan herhangi birini karşılarsa uygun değildir:

* Kasa için tanılama ayarı yok.
* Tanılama ayarları kasa için hazırdır, ancak ayarların hiçbirinde hedef olarak LA ile etkinleştirilen Kaynağa özgü olayların **tümü** ve geçişte seçilen **Kaynağa özgü** olaylar vardır. 

Bu nedenle, bir kullanıcının AzureDiagnostics modunda etkinleştirilen AzureBackupReport etkinliğine sahip bir kasası olsa bile (Yedekleme Raporları tarafından desteklenir), düzeltme görevi yine de bu kasa için geçerli olacaktır, çünkü Kaynak özel modu tanılama ayarları oluşturmanın önerilen yolu olduğundan, [ileriye doğru](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events#legacy-event).

Ayrıca, bir kullanıcının yalnızca altı Kaynağa özgü olayın bir alt kümesine sahip bir kasası varsa, Yedekleme Raporları yalnızca altı Kaynağa özgü olayın tümü etkinleştirildiğinde beklendiği gibi çalışacağından, düzeltme görevi bu kasa için geçerli olacaktır.

> [!NOTE]
>
> Bir kasada, belirli bir LA Çalışma alanına veri göndermek üzere yapılandırılan, belirli bir LA Çalışma alanına veri göndermek üzere yapılandırılan, etkinleştirilmiş bir Kaynak özel kategorisi **alt kümesiyle** varolan bir tanılama ayarı varsa, 'Çalışma Alanı X' deyin, Çünkü ilke atamasında sağlanan hedef LA Workspace **aynı** 'Workspace X' ise düzeltme görevi başarısız olur (yalnızca bu kasa için). 
>
>Bunun nedeni, aynı kaynak üzerinde iki farklı tanılama ayarı tarafından etkinleştirilen olaylar bazı formda **çakışıyorsa,** ayarların hedefle aynı LA Çalışma alanına sahip olamayacağıdır. İlgili kasaya gidip hedef olarak farklı bir LA Çalışma alanı yla tanılama ayarını yapılandırarak bu hatayı el ile çözmeniz gerekir.
>
> Hedef olarak Workspace X ile yalnızca AzureBackupReport olarak etkinleştirilen varolan tanılama ayarı başarısız **olmayacak,** çünkü bu durumda, varolan ayar tarafından etkinleştirilen olaylar la düzeltme görevi tarafından oluşturulan ayar tarafından etkinleştirilen olaylar arasında çakışma olmayacaktır.

## <a name="next-steps"></a>Sonraki Adımlar

* [Yedekleme Raporlarını nasıl kullanacağınızı öğrenin](https://docs.microsoft.com/azure/backup/configure-reports)
* [Azure İlkesi hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/governance/policy/)
* [Bir verme kapsamındaki tüm VM'ler için yedeklemeyi otomatik olarak etkinleştirmek için Azure İlkesi'ni kullanın](https://docs.microsoft.com/azure/backup/backup-azure-auto-enable-backup)
