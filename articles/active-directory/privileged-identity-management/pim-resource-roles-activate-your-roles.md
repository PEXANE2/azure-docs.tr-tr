---
title: Azure Kaynak rollerimi PıM-Azure Active Directory 'de etkinleştir | Microsoft Docs
description: Azure Kaynak rollerinizi Azure AD Privileged Identity Management (PıM) ' de nasıl etkinleştireceğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 06/28/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e2bf1253f1c2f53b839a73034fcdb33f3aab463
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804342"
---
# <a name="activate-my-azure-resource-roles-in-pim"></a>Azure Kaynak rollerimi PıM 'de etkinleştir

Azure kaynakları için uygun rol üyeleri, Azure Active Directory (Azure AD) Privileged Identity Management (PıM) kullanarak etkinleştirmeyi gelecekteki bir tarih ve saate göre zamanlayabilir. Ayrıca, en fazla (yöneticiler tarafından yapılandırılan) belirli bir etkinleştirme süresi arasından seçim yapabilir.

Bu makale, PıM 'de Azure Kaynak rolünü etkinleştirmeleri gereken üyelere yöneliktir.

## <a name="activate-a-role"></a>Rol etkinleştirme

Bir Azure Kaynak rolünü gerçekleştirmeniz gerektiğinde, PıM ' de **rollerim** gezinti seçeneğini kullanarak etkinleştirme isteğinde bulunabilir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın. Panonuza PıM kutucuğunu ekleme hakkında daha fazla bilgi için bkz. PıM 'yi [kullanmaya başlama](pim-getting-started.md).

1. **Rollerim**' e tıklayın.

    ![Etkinleştirebilecekleri rolleri gösteren roller sayfası](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Uygun Azure Kaynak rollerinizin listesini görmek için **Azure Kaynak rolleri** ' ne tıklayın.

   ![Rollerim-Azure Kaynak rolleri sayfası](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png) 

1. **Azure Kaynak rolleri** listesinde, etkinleştirmek istediğiniz rolü bulun.

    ![Azure Kaynak rolleri-uygun rollerim listesi](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Etkinleştir bölmesini açmak için **Etkinleştir** ' e tıklayın.

1. Rolünüz Multi-Factor Authentication (MFA) gerektiriyorsa, **devam etmeden önce kimliğinizi doğrula**' ya tıklayın. Her oturum için yalnızca bir kez kimlik doğrulaması yapmanız gerekir.

    ![Rol etkinleştirmeden önce MFA ile kimliğimi doğrula](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. **Kimliğimi doğrula** ' ya tıklayın ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![PIN kodu gibi güvenlik doğrulaması sağlayan ekran](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Azaltılmış kapsam belirtmek isterseniz, kaynak Filtre bölmesini açmak için **kapsam** ' a tıklayın.

    Yalnızca ihtiyacınız olan kaynaklara erişim istemek için en iyi uygulamadır. Kaynak filtre bölmesinde, erişmeniz gereken kaynak gruplarını veya kaynakları belirtebilirsiniz.

    ![Kapsam belirtmek için etkinleştir-kaynak filtre bölmesi](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Gerekirse, özel bir etkinleştirme başlangıç saati belirtin. Üye, seçili saatten sonra etkinleştirilecektir.

1. **Neden** kutusunda, etkinleştirme isteğinin nedenini girin.

    ![Kapsam, başlangıç zamanı, süre ve nedenle etkinleştirme bölmesi tamamlandı](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. **Etkinleştir**' e tıklayın.

    Rol onay gerektirmiyorsa, etkinleştirilir ve etkin roller listesine eklenir. Rolünü kullanmak istiyorsanız sonraki bölümdeki adımları izleyin.

    Rol etkinleştirme için [onay gerektiriyorsa](pim-resource-roles-approval-workflow.md) , tarayıcınızın sağ üst köşesinde, isteğin onay beklendiğini bildiren bir bildirim görüntülenir.

    ![Etkinleştirme isteği onay bildirimi bekliyor](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Etkinleştirdikten hemen sonra bir rol kullan

Etkinleştirdikten sonra herhangi bir gecikme olması durumunda, Azure Kaynak rollerinizi hemen kullanmaya başladıktan sonra bu adımları izleyin.

1. Azure AD Privileged Identity Management açın.

1. Uygun Azure AD rollerinizin ve Azure Kaynak rollerinin bir listesini görmek için **rollerim** ' e tıklayın.

1. **Azure Kaynak rolleri**' ne tıklayın.

1. **Etkin roller** sekmesine tıklayın.

1. Rol etkin olduktan sonra portalın oturumunu kapatın ve yeniden oturum açın.

    Rol artık kullanılabilir olmalıdır.

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleyin

Etkinleştirme için bekleyen isteklerinizin durumunu görüntüleyebilirsiniz.

1. Azure AD Privileged Identity Management açın.

1. Azure AD rolünüzün ve Azure Kaynak rolü isteklerinizin listesini görmek için **Isteklerim** ' e tıklayın.

    ![İsteklerim-bekleyen isteklerinizi gösteren Azure Kaynak sayfası](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. **Istek durumu** sütununu görüntülemek için sağa kaydırın.

## <a name="cancel-a-pending-request"></a>Bekleyen bir isteği iptal etme

Onay gerektiren bir rolün etkinleştirilmesini gerektirmiyorsa, bekleyen bir isteği dilediğiniz zaman iptal edebilirsiniz.

1. Azure AD Privileged Identity Management açın.

1. **Isteklerim**' e tıklayın.

1. İptal etmek istediğiniz rol için **iptal** bağlantısına tıklayın.

    Iptal ' e tıkladığınızda istek iptal edilir. Rolü yeniden etkinleştirmek için, etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![Iptal eylemi vurgulanmış istek listem](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot"></a>Sorun giderme

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Rol etkinleştirildikten sonra izinler verilmiyor

PıM 'de bir rolü etkinleştirdiğinizde, etkinleştirme ayrıcalıklı rol gerektiren tüm portallara anında yaymayabilir. Bazı durumlarda değişiklik yayılsa bile portalda web önbelleği değişikliğin anında geçerlilik kazanmamasına yol açabilir. Etkinleştirme gecikirse, yapmanız gerekenler aşağıda verilmiştir.

1. Azure portalında oturumunuzu kapatın ve sonra yeniden oturum açın.

    Bir Azure Kaynak rolünü etkinleştirdiğinizde, etkinleştirme aşamalarını görürsünüz. Tüm aşamalar tamamlandıktan sonra bir **Oturumu kapat** bağlantısı görürsünüz. Bu bağlantıyı kullanarak oturumu kapatabilirsiniz. Bu işlem çoğu durumda etkinleştirme gecikmesi sorununu çözecektir.

1. PıM 'de rolün üyesi olarak listelendiğinizi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rollerini genişletme veya yenileme](pim-resource-roles-renew-extend.md)
- [PIM'de Azure AD rollerimi etkinleştirme](pim-how-to-activate-role.md)
