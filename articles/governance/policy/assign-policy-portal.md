---
title: 'Hızlı başlangıç: Portal ile yeni ilke ataması'
description: Bu hızlı başlangıçta, uyumlu olmayan kaynakları belirlemek üzere bir Azure Ilkesi ataması oluşturmak için Azure portal kullanırsınız.
ms.date: 08/17/2020
ms.topic: quickstart
ms.openlocfilehash: 956ec05b5a7fac862eeea86cf96a2db37f1c0536
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651972"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources"></a>Hızlı başlangıç: uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile _uyumsuzdur_.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup yönetilen diskler ilke tanımı _kullanmayan VM 'leri_ atayacaksınız.

1. **Tüm hizmetler**' i seçip **ilke**arayıp ' yi seçerek Azure Portal Azure ilke hizmetini başlatın.

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

1. **Yönetilen Kimlik Oluşturun** seçeneğini işaretsiz bırakın. İlke veya girişim, [Deployifnotexists](./concepts/effects.md#deployifnotexists) efektli bir ilke içerdiğinde bu kutu _denetlenmelidir_ . Bu hızlı başlangıç için kullanılan ilke olmadığından boş bırakın. Daha fazla bilgi için [yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md) ve [düzeltme güvenliğinin işleyişi](./how-to/remediate-resources.md#how-remediation-security-works) bölümlerine bakın.

1. **Ata**’yı seçin.

Artık ortamınızın uyumluluk durumunu anlamak için uyumlu olmayan kaynakları belirlemek için hazırsınız demektir.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları belirleme

Sayfanın sol tarafındaki **Uyumluluk** ' i seçin. Ardından, oluşturduğunuz _yönetilen diskler ilke atamasını kullanmayan VM 'Leri denetle_ seçeneğini bulun.

:::image type="content" source="./media/assign-policy-portal/policy-compliance.png" alt-text="Ilke uyumluluğu sayfasındaki uyumluluk ayrıntılarının ekran görüntüsü." border="false":::

Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa **uyumlu olmayan kaynaklar**altında görünürler.

Bir koşul mevcut kaynaklarınıza göre değerlendirilip true sonucunu verdiğinde, bu kaynaklar ilkeyle uyumlu değil olarak işaretlenir. Aşağıdaki tabloda, elde edilen uyumluluk durumu için farklı ilke etkilerinin koşul değerlendirmesi ile nasıl çalıştığı gösterilmektedir. Azure portal değerlendirme mantığını görmeseniz de, uyumluluk durumu sonuçları gösterilir. Uyumluluk durumu sonucu uyumlu veya uyumsuz şeklindedir.

| **Kaynak Durumu** | **Etki** | **İlke Değerlendirmesi** | **Uyumluluk durumu** |
| --- | --- | --- | --- |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | Doğru | Uyumlu Değil |
| Var | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | Yanlış | Uyumlu |
| Yeni | Audit, AuditIfNotExist\* | Doğru | Uyumlu Değil |
| Yeni | Audit, AuditIfNotExist\* | Yanlış | Uyumlu |

\* Append, DeployIfNotExist ve AuditIfNotExist etkileri IF deyiminin TRUE olmasını gerektirir.
Etkiler ayrıca varlık koşulunun uyumlu olmaması için FALSE olmasını gerektirir. TRUE olduğunda, IF koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

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