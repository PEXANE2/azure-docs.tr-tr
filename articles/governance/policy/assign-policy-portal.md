---
title: 'Hızlı başlangıç: Portal ile yeni ilke ataması'
description: Bu hızlı başlangıçta, uyumlu olmayan kaynakları belirlemek üzere bir Azure Ilkesi ataması oluşturmak için Azure portal kullanırsınız.
ms.date: 01/29/2021
ms.topic: quickstart
ms.openlocfilehash: e5cbf31e897b5be404327efa254eb90ead990f5f
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99220896"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources"></a>Hızlı başlangıç: uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile _uyumsuzdur_.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup yönetilen diskler ilke tanımı _kullanmayan VM 'leri_ atayacaksınız.

1. **Tüm hizmetler**' i seçip **ilke** arayıp ' yi seçerek Azure Portal Azure ilke hizmetini başlatın.

   :::image type="content" source="./media/assign-policy-portal/search-policy.png" alt-text="Tüm hizmetlerde Ilke aramanın ekran görüntüsü." border="false":::

1. Azure İlkesi sayfasının sol tarafından **Atamalar**'ı seçin. Atama, belirli bir kapsamda gerçekleşmesi için atanmış olan bir ilkedir.

   :::image type="content" source="./media/assign-policy-portal/select-assignments.png" alt-text="Ilkeye genel bakış sayfasından atamalar sayfasını seçme ekran görüntüsü." border="false":::

1. **İlke - Atamalar** sayfasının üst kısmından **İlke Ata**'yı seçin.

   :::image type="content" source="./media/assign-policy-portal/select-assign-policy.png" alt-text="Atamalar sayfasından ' ilke atama ' seçeneğinin ekran görüntüsü." border="false":::

1. **Ilke ata** sayfasında, üç noktayı seçip bir yönetim grubu veya abonelik seçerek **kapsamı** ayarlayın. İsterseniz bir kaynak grubu seçin. Kapsam, ilke atamasının hangi kaynaklarda veya kaynak gruplarında uygulanacağını belirler. Ardından **kapsam** sayfasının alt kısmındaki **Seç** düğmesini kullanın.

   Bu örnekte **Contoso** aboneliği kullanılır. Sizin aboneliğiniz farklı olacaktır.

1. Kaynaklar **Kapsam**’a göre dışlanabilir. **Dışlamalar**, **Kapsam**’dan bir düzey aşağıda başlatılır. **Dışlamalar** isteğe bağlıdır, bu yüzden şimdilik boş bırakın.

1. **İlke tanımı** üç nokta öğesini seçerek kullanılabilen tanımların listesini açın. Azure İlkesi, kullanabileceğiniz yerleşik ilke tanımlarıyla birlikte gelir. Aşağıdakiler gibi çok sayıda ilke mevcuttur:

   - Etiketi ve değerini zorla
   - Etiketi ve değerini uygula
   - Eksikse, kaynak grubundan bir etiketi devralma

   Kullanılabilir yerleşik ilkelerin kısmi listesi için bkz. [Azure ilke örnekleri](./samples/index.md).

1. İlke tanımları listenizde _Yönetilen disk kullanmayan VM'leri denetle_ tanımını arayın. Bu ilkeyi seçin ve ardından **Seç** düğmesini kullanın.

   :::image type="content" source="./media/assign-policy-portal/select-available-definition.png" alt-text="Kullanılabilir tanımları filtrelemeye yönelik ekran görüntüsü." border="false":::

1. **Atama adı** otomatik olarak seçtiğiniz ilke adıyla doldurulur, ancak bunu değiştirebilirsiniz. Bu örnek için _Yönetilen disk kullanmayan VM'leri denetle_ ayarını değiştirmeyin. İsteğe bağlı bir **Açıklama** da ekleyebilirsiniz. Açıklama, bu ilke atamasıyla ilgili ayrıntıları sağlar.
   **Atayan**, oturum açmış kişiye göre otomatik olarak doldurulur. Bu alan isteğe bağlı olduğu için özel değerler girilebilir.

1. İlke zorlamayı _etkin_ bırakın. Daha fazla bilgi için bkz. [ilke atama-zorlama modu](./concepts/assignment-structure.md#enforcement-mode).

1. Atama sihirbazının sonraki segmentine gitmek için sayfanın en altında bulunan **İleri ' yi** veya sayfanın en üstündeki **Parametreler** sekmesini seçin.

1. **Temel kurallar** sekmesi dahil olmak üzere ilke tanımı seçili ise, bu sekmede yapılandırılır. _Yönetilen diskler kullanmayan denetim VM_ 'lerinin hiçbir parametresi olmadığından, atama sihirbazının sonraki kesimine gitmek için sayfanın alt kısmındaki **İleri** ' yi veya sayfanın en üstündeki **Düzeltme** sekmesini seçin.

1. **Yönetilen Kimlik Oluşturun** seçeneğini işaretsiz bırakın. İlke veya girişim, [Deployifnotexists](./concepts/effects.md#deployifnotexists) veya [değişiklik](./concepts/effects.md#modify) efektine sahip bir ilke içerdiğinde bu kutu _denetlenmelidir_ . Bu hızlı başlangıç için kullanılan ilke olmadığından boş bırakın. Daha fazla bilgi için [yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md) ve [düzeltme güvenliğinin işleyişi](./how-to/remediate-resources.md#how-remediation-security-works) bölümlerine bakın.

1. Atama sihirbazının bir sonraki segmentine gitmek için sayfanın en altında bulunan **İleri ' yi** veya sayfanın üst kısmındaki **uyumsuz iletiler** sekmesini seçin.

1. **Uyumsuz olmayan Iletiyi** _sanal makineler için bir yönetilen disk kullanması gerekir_. Bu özel ileti, düzenli değerlendirme sırasında bir kaynak reddedildiğinde veya uyumlu olmayan kaynaklar için görüntülenir.

1. Atama sihirbazının bir sonraki segmentine gitmek için sayfanın alt kısmındaki **İleri ' yi** veya sayfanın en üstündeki **gözden geçir + oluştur** sekmesini seçin.

1. Seçilen seçenekleri gözden geçirin ve ardından sayfanın alt kısmındaki **Oluştur** ' u seçin.

Artık ortamınızın uyumluluk durumunu anlamak için uyumlu olmayan kaynakları belirlemek için hazırsınız demektir.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Sayfanın sol tarafındaki **Uyumluluk** ' i seçin. Ardından, oluşturduğunuz _yönetilen diskler ilke atamasını kullanmayan VM 'Leri denetle_ seçeneğini bulun.

:::image type="content" source="./media/assign-policy-portal/policy-compliance.png" alt-text="Ilke uyumluluğu sayfasındaki uyumluluk ayrıntılarının ekran görüntüsü." border="false":::

Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa **uyumlu olmayan kaynaklar** altında görünürler.

Bir koşul mevcut kaynaklarınıza göre değerlendirilip true sonucunu verdiğinde, bu kaynaklar ilkeyle uyumlu değil olarak işaretlenir. Aşağıdaki tabloda, elde edilen uyumluluk durumu için farklı ilke etkilerinin koşul değerlendirmesi ile nasıl çalıştığı gösterilmektedir. Azure portal değerlendirme mantığını görmeseniz de, uyumluluk durumu sonuçları gösterilir. Uyumluluk durumu sonucu uyumlu veya uyumsuz şeklindedir.

| Kaynak Durumu | Etki | İlke Değerlendirmesi | Uyumluluk Durumu |
| --- | --- | --- | --- |
| Yeni veya Güncelleştirilmiş | Denetim, değiştirme, Auditınotexist | Doğru | Uyumlu Değil |
| Yeni veya Güncelleştirilmiş | Denetim, değiştirme, Auditınotexist | Yanlış | Uyumlu |
| Var | Reddet, denetle, Ekle, Değiştir, DeployIfNotExist, AuditIfNotExist | Doğru | Uyumlu Değil |
| Var | Reddet, denetle, Ekle, Değiştir, DeployIfNotExist, AuditIfNotExist | Yanlış | Uyumlu |

> [!NOTE]
> DeployIfNotExist ve Auditınotexist etkileri, IF ifadesinin TRUE olmasını ve varlık koşulunun uyumsuz olması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturulan atamayı kaldırmak için şu adımları izleyin:

1. Azure İlkesi sayfasının sol tarafından **Uyumluluk**’u (veya **Atamalar**’ı) seçin ve oluşturduğunuz _Yönetilen disk kullanmayan VM'leri denetle_ ilke atamasını bulun.

1. Yönetilen diskler ilkesi atamasını _kullanmayan VM 'Leri denetle_ ' ye sağ tıklayın ve **atamayı Sil**' i seçin.

   :::image type="content" source="./media/assign-policy-portal/delete-assignment.png" alt-text="Uyumluluk sayfasından bir atamayı silmek için bağlam menüsünü kullanmanın ekran görüntüsü." border="false":::

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir kapsama ilke tanımı atadınız ve bunun uyumluluk raporunu değerlendirdiniz.
İlke tanımı, kapsamdaki tüm kaynakların uyumlu olduğunu doğrular ve hangilerinin olduğunu tanımlar.

Yeni kaynakların uyumlu olduğunu doğrulamak üzere ilke atama hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)