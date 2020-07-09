---
title: Mobility hizmetinin Azure Site Recovery otomatik güncelleştirmesi
description: Azure Site Recovery kullanarak Azure VM 'Leri çoğaltılırken Mobility hizmetinin otomatik güncelleştirmesine genel bakış.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/02/2020
ms.author: rajanaki
ms.openlocfilehash: b57ce89979225015dc87bbfb17f9603897ef6d6b
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135847"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Azure 'dan Azure 'a çoğaltma için Mobility hizmetini otomatik güncelleştirme

Azure Site Recovery, herhangi bir sorunu gidermek ve mevcut özellikleri geliştirmek ya da yenilerini eklemek için aylık bir sürüm temposunda kullanır. Hizmetle güncel kalmasını sağlamak için her ay yama dağıtımını planlamanız gerekir. Her yükseltmeyle ilişkili ek yükün önüne geçmek için Site Recovery bileşen güncelleştirmelerini yönetmesine izin verebilirsiniz.

[Azure 'Dan Azure 'a olağanüstü durum kurtarma mimarisinde](azure-to-azure-architecture.md)belirtildiği gibi, Mobility hizmeti bir Azure bölgesinden diğerine çoğaltma özelliği etkinleştirilmiş olan tüm Azure sanal makinelerine (VM) yüklenir. Otomatik güncelleştirmeleri kullandığınızda, her yeni sürüm Mobility hizmeti uzantısını güncelleştirir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-automatic-updates-work"></a>Otomatik güncelleştirmeler nasıl çalışır?

Güncelleştirmeleri yönetmek için Site Recovery kullandığınızda, kasa ile aynı abonelikte oluşturulan bir Otomasyon hesabı aracılığıyla küresel bir runbook (Azure hizmetleri tarafından kullanılır) dağıtır. Her kasa bir Otomasyon hesabı kullanır. Bir kasadaki her VM için Runbook etkin otomatik güncelleştirmeleri denetler. Mobility hizmeti uzantısının daha yeni bir sürümü varsa, güncelleştirme yüklenir.

Varsayılan runbook zamanlaması, çoğaltılan sanal makinenin coğrafi bölge saat diliminde 12:00 ile günde bir kez gerçekleşir. Ayrıca, Otomasyon hesabı aracılığıyla runbook zamanlamasını değiştirebilirsiniz.

> [!NOTE]
> [Güncelleştirme paketi 35](site-recovery-whats-new.md#updates-march-2019)' den başlayarak, güncelleştirmeler için kullanılacak mevcut bir Otomasyon hesabı seçebilirsiniz. Güncelleştirme paketi 35 ' dan önce varsayılan olarak otomasyon hesabını Site Recovery. Yalnızca bir VM için çoğaltmayı etkinleştirdiğinizde bu seçeneği belirleyebilirsiniz. Zaten çoğaltma özelliği etkin olan bir VM için kullanılamaz. Seçtiğiniz ayar aynı kasada korunan tüm Azure sanal makineleri için geçerlidir.

Otomatik güncelleştirmeleri açmak, Azure sanal makinelerinizin yeniden başlatılmasını gerektirmez veya devam eden çoğaltmayı etkiler.

Otomasyon hesabı 'nda iş faturalaması, bir ayda kullanılan iş çalışma zamanı dakikalarının sayısına bağlıdır. İş yürütme, her gün yaklaşık bir dakika boyunca birkaç saniye sürer ve serbest birimler olarak ele alınmıştır. Varsayılan olarak, aşağıdaki tabloda gösterildiği gibi, 500 dakika bir Otomasyon hesabı için boş birimler olarak dahil edilir:

| Dahil edilen ücretsiz birimler (her ay) | Fiyat |
|---|---|
| İş çalışma zamanı 500 dakika | ₹ 0.14/dakika

## <a name="enable-automatic-updates"></a>Otomatik güncelleştirmeleri etkinleştir

Site Recovery uzantı güncelleştirmelerini yönetebilmenin birkaç yolu vardır:

- [Çoğaltmayı etkinleştir adımının bir parçası olarak yönetin](#manage-as-part-of-the-enable-replication-step)
- [Kasa içindeki uzantı güncelleştirme ayarlarını değiştirin](#toggle-the-extension-update-settings-inside-the-vault)
- [Güncelleştirmeleri el ile yönetin](#manage-updates-manually)

### <a name="manage-as-part-of-the-enable-replication-step"></a>Çoğaltmayı etkinleştir adımının bir parçası olarak yönetin

VM veya [Kurtarma Hizmetleri kasasından](azure-to-azure-how-to-enable-replication.md) [başlayan bir](azure-to-azure-quickstart.md) VM için çoğaltmayı etkinleştirdiğinizde, Site Recovery Site Recovery uzantısı için güncelleştirmeleri yönetmesine veya el ile yönetmeye izin verebilirsiniz.

:::image type="content" source="./media/azure-to-azure-autoupdate/enable-rep.png" alt-text="Uzantı ayarları":::

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Kasa içindeki uzantı güncelleştirme ayarlarını değiştirin

1. Kurtarma Hizmetleri kasasından **Manage**  >  **Site Recovery altyapıyı**Yönet ' e gidin.
1. **Azure sanal makineler**  >  **uzantı güncelleştirme ayarları**' nın altında  >  **Site Recovery yönetmesine izin ver**' **On**i seçin.

   Uzantıyı el ile yönetmek için **kapalı**' yı seçin.

1. **Kaydet**'i seçin.

:::image type="content" source="./media/azure-to-azure-autoupdate/vault-toggle.png" alt-text="Uzantı güncelleştirme ayarları":::

> [!IMPORTANT]
> **Yönetilecek Site Recovery Izin ver**' i seçtiğinizde, bu ayar kasadaki tüm sanal makinelere uygulanır.

> [!NOTE]
> Her iki seçenek de güncelleştirmeleri yönetmek için kullanılan Otomasyon hesabını bilgilendirir. Bu özelliği bir kasada ilk kez kullanıyorsanız, varsayılan olarak yeni bir Otomasyon hesabı oluşturulur. Alternatif olarak, ayarı özelleştirebilir ve var olan bir Otomasyon hesabı seçebilirsiniz. Aynı kasada çoğaltmayı etkinleştirmek için sonraki tüm işlemler, daha önce oluşturulan Otomasyon hesabını kullanır. Şu anda, açılan menü yalnızca kasala aynı kaynak grubundaki Otomasyon hesaplarını listeler.

### <a name="manage-updates-manually"></a>Güncelleştirmeleri el ile yönetin

1. Sanal makinelerinize yüklü Mobility hizmeti için yeni güncelleştirmeler varsa, aşağıdaki bildirimi görürsünüz: **yeni Site Recovery çoğaltma Aracısı güncelleştirmesi var. Yüklemek için tıklayın.**

   :::image type="content" source="./media/vmware-azure-install-mobility-service/replicated-item-notif.png" alt-text="Çoğaltılan Öğeler penceresi":::

1. VM seçim sayfasını açmak için bildirimi seçin.
1. Yükseltmek istediğiniz VM 'Leri seçin ve ardından **Tamam**' ı seçin. Mobility hizmetini Güncelleştir seçili her VM için başlatılır.

   :::image type="content" source="./media/vmware-azure-install-mobility-service/update-okpng.png" alt-text="Çoğaltılan öğeler VM listesi":::

## <a name="common-issues-and-troubleshooting"></a>Yaygın sorunlar ve sorun giderme

Otomatik güncelleştirmelerle ilgili bir sorun varsa, kasa panosundaki **yapılandırma sorunları** altında bir hata bildirimi görürsünüz.

Otomatik güncelleştirmeleri etkinleştiremediğimiz takdirde aşağıdaki yaygın hatalara ve önerilen eylemlere bakın:

- **Hata**: Azure farklı çalıştır hesabı (hizmet sorumlusu) oluşturma ve hizmet sorumlusuna katkıda bulunan rolü verme izinleriniz yok.

  **Önerilen eylem**: oturum açan hesabın katkı olarak atandığından emin olun ve yeniden deneyin. İzinleri atama hakkında daha fazla bilgi için bkz. [nasıl yapılır: Azure AD uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için portalı kullanma](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app).

  Otomatik güncelleştirmeleri etkinleştirdikten sonra birçok sorunu gidermek için, **Onar**' ı seçin. Onar düğmesi kullanılamıyorsa, uzantı güncelleştirme ayarları bölmesinde hata iletisine bakın.

  :::image type="content" source="./media/azure-to-azure-autoupdate/repair.png" alt-text="Uzantı güncelleştirme ayarlarındaki Site Recovery hizmeti onarma düğmesi":::

- **Hata**: farklı çalıştır hesabının kurtarma hizmetleri kaynağına erişim izni yok.

  **Önerilen eylem**: [Farklı Çalıştır hesabını silin ve yeniden oluşturun](../automation/manage-runas-account.md). Ya da Otomasyon farklı çalıştır hesabının Azure Active Directory uygulamasının kurtarma hizmetleri kaynağına erişebildiğinizden emin olun.

- **Hata**: Run as hesabı bulunamadı. Bunlardan biri silinmiş veya oluşturulmamış-Azure Active Directory uygulama, hizmet sorumlusu, rol, Otomasyon sertifikası varlığı, Otomasyon bağlantı varlığı veya Parmak Izi sertifika ile bağlantı arasında aynı değil.

  **Önerilen eylem**: [Farklı Çalıştır hesabını silin ve yeniden oluşturun](../automation/manage-runas-account.md).

- **Hata**: Otomasyon hesabı tarafından kullanılan Azure farklı çalıştır sertifikası sona ermek üzere.

  Farklı Çalıştır hesabı için oluşturulan otomatik olarak imzalanan sertifika, oluşturma tarihinden itibaren bir yıl dolar. Sertifikayı süresi dolmadan önce herhangi bir zamanda yenileyebilirsiniz. E-posta bildirimlerine kaydolduysanız, sizin tarafınızdan bir eylem gerektiğinde e-postalar da alacaksınız. Bu hata, sona erme tarihinden iki ay önce gösterilir ve sertifikanın süresi dolmuşsa kritik bir hataya değişir. Sertifikanın süresi dolduktan sonra, aynı şekilde yenileene kadar otomatik güncelleştirme işlevsel olmayacaktır.

  **Önerilen eylem**: Bu sorunu çözmek için **Onar** ve **Sertifikayı Yenile**' yi seçin.

  :::image type="content" source="./media/azure-to-azure-autoupdate/automation-account-renew-runas-certificate.PNG" alt-text="Yenile-CERT":::

  > [!NOTE]
  > Sertifikayı yeniledikten sonra, geçerli durumu göstermek için sayfayı yenileyin.
