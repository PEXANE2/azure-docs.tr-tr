---
title: Portal ile kaynakları denetlemek için ilke oluşturma-Azure Ilkesi
description: Bu makale, uyumlu olmayan kaynakları belirlemek için bir ilke tanımı oluşturma adımlarında size yol gösterir.
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: quickstart
ms.service: azure-policy
ms.openlocfilehash: 6433c5f90b56489e92ec76aab5c9a0d0c6aeb508
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72254833"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources"></a>Hızlı başlangıç: uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma

Azure 'da uyumluluğu anlamak için ilk adım kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen diskleri kullanmayan sanal makineleri belirlemek için bir ilke ataması oluşturma işleminde size kılavuzluk edecek.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarıyla tanımlayacaksınız. İlke atamasıyla *uyumlu değildir* .

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup yönetilen diskler ilke tanımı *kullanmayan VM 'leri* atayacaksınız.

1. **Tüm hizmetler**' e tıklayıp **ilke**arayıp ' yi seçerek Azure Portal Azure ilke hizmetini başlatın.

   ![Tüm hizmetlerde Ilke ara](./media/assign-policy-portal/search-policy.png)

1. Azure Ilkesi sayfasının sol tarafındaki **atamalar** ' ı seçin. Atama, belirli bir kapsamda gerçekleşmesi için atanmış bir ilkedir.

   ![Ilkeye genel bakış sayfasından atamaları seçin sayfası](./media/assign-policy-portal/select-assignments.png)

1. İlke **-atamalar** sayfasının en üstünden **ilke ata** ' yı seçin.

   ![Atamalar sayfasından bir ilke tanımı atama](./media/assign-policy-portal/select-assign-policy.png)

1. **Ilke ata** sayfasında, üç nokta simgesine tıklayarak ve bir yönetim grubu ya da abonelik seçerek **kapsamı** seçin. İsteğe bağlı olarak bir kaynak grubu seçin. Kapsam, ilke atamasının hangi kaynaklarda veya gruplandırmaya uygulanacağını belirler. Ardından **kapsam** sayfasının alt kısmındaki **Seç** ' e tıklayın.

   Bu örnek **contoso** aboneliğini kullanır. Aboneliğiniz farklı olacak.

1. Kaynaklar, **kapsamı**temel alınarak dışarıda bırakılabilirler. **Dışlamalar** , **kapsam**düzeyinden daha düşük bir düzeyde başlar. **Dışlamalar** isteğe bağlıdır, bu nedenle şimdilik boş bırakın.

1. Kullanılabilir tanımlar listesini açmak için **ilke tanımı** üç nokta simgesini seçin. Azure Ilkesi, kullanabileceğiniz yerleşik ilke tanımlarıyla birlikte gelir. Birçok tane mevcuttur, örneğin:

   - Etiketi ve değerini zorla
   - Etiketi ve değerini Uygula
   - SQL Server sürüm 12,0 gerektir

   Kullanılabilir yerleşik ilkelerin kısmi listesi için bkz. [Azure ilke örnekleri](./samples/index.md).

1. *Yönetilen diskler tanımını kullanmayan VM 'Leri denetleme* hakkında bilgi edinmek için ilke tanımları listesinde arama yapın. Bu ilkeye tıklayın ve **Seç**' e tıklayın.

   ![Doğru ilke tanımını bulun](./media/assign-policy-portal/select-available-definition.png)

1. **Atama adı** , seçtiğiniz ilke adı ile otomatik olarak doldurulur, ancak bunu değiştirebilirsiniz. Bu örnek için, *yönetilen disk kullanmayan VM 'Leri denetle*seçeneğini bırakın. İsteğe bağlı bir **Açıklama**da ekleyebilirsiniz. Açıklama Bu ilke atamasının ayrıntılarını sağlar.
   **Tarafından atanan** , oturum açmış olan kim temelinde otomatik olarak doldurulur. Bu alan isteğe bağlıdır, bu nedenle özel değerler girilebilir.

1. **Yönetilen kimlik oluşturma** işaretini işaretsiz bırakın. İlke veya girişim, [Deployifnotexists](./concepts/effects.md#deployifnotexists) efektli bir ilke içerdiğinde bu kutu _denetlenmelidir_ . Bu hızlı başlangıç için kullanılan ilke olmadığından boş bırakın. Daha fazla bilgi için bkz. [Yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md) ve [Düzeltme güvenliğinin nasıl çalıştığı](./how-to/remediate-resources.md#how-remediation-security-works).

1. **Ata**' ya tıklayın.

Artık ortamınızın uyumluluk durumunu anlamak için uyumlu olmayan kaynakları belirlemek için hazırsınız demektir.

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları tanımla

Sayfanın sol tarafındaki **Uyumluluk** ' i seçin. Ardından, oluşturduğunuz **yönetilen diskler ilke atamasını kullanmayan VM 'Leri denetle** seçeneğini bulun.

![Ilke uyumluluk sayfasındaki uyumluluk ayrıntıları](./media/assign-policy-portal/policy-compliance.png)

Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa **uyumlu olmayan kaynaklar**altında görünürler.

Bir koşul mevcut kaynaklarınıza göre değerlendirildiğinde ve doğru bulunursa, bu kaynaklar ilkeyle uyumlu değil olarak işaretlenir. Aşağıdaki tabloda, farklı ilke efektlerinin, sonuçta elde edilen uyumluluk durumu için koşul değerlendirmesiyle nasıl çalıştığı gösterilmektedir. Azure portal değerlendirme mantığını görmeseniz de, uyumluluk durumu sonuçları gösterilir. Uyumluluk durumu sonucu uyumlu veya uyumlu değil.

| **Kaynak durumu** | **Etkinleşmesi** | **İlke değerlendirmesi** | **Uyumluluk durumu** |
| --- | --- | --- | --- |
| Bulunur | Deny, Audit, append @ no__t-0, DeployIfNotExist @ no__t-1, Auditınotexist @ no__t-2 | Doğru | Uyumlu değil |
| Bulunur | Deny, Audit, append @ no__t-0, DeployIfNotExist @ no__t-1, Auditınotexist @ no__t-2 | False | uyumlu |
| Yeni | Denetim, Auditınotexist @ no__t-0 | Doğru | Uyumlu değil |
| Yeni | Denetim, Auditınotexist @ no__t-0 | False | uyumlu |

\* Append, DeployIfNotExist ve Auditınotexist etkileri, IF ifadesinin TRUE olmasını gerektirir.
Etkiler Ayrıca varlık koşulunun uyumsuz olması için yanlış olmasını gerektirir. TRUE olduğunda, If koşulu ilgili kaynaklar için varlık koşulunun değerlendirilmesini tetikler.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Oluşturulan atamayı kaldırmak için şu adımları izleyin:

1. Azure Ilkesi sayfasının sol tarafındaki **Uyumluluk** (veya **atamalar**) ' ı seçin ve oluşturduğunuz **yönetilen diskleri kullanmayan VM 'leri denetle** ilke atamasını bulun.

1. Yönetilen diskler ilkesi atamasını **kullanmayan VM 'Leri denetle** ' ye sağ tıklayın ve **atamayı Sil** ' i seçin.

   ![Uyumluluk sayfasından bir atamayı silme](./media/assign-policy-portal/delete-assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kapsama ilke tanımı atadınız ve uyumluluk raporunu değerlendirdiniz.
İlke tanımı, kapsamdaki tüm kaynakların uyumlu olduğunu doğrular ve hangilerinin olduğunu tanımlar.

Yeni kaynakların uyumlu olduğunu doğrulamak üzere ilke atama hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)