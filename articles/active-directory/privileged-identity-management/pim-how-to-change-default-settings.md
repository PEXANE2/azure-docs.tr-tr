---
title: PıM-Azure Active Directory 'de Azure AD rol ayarlarını yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rol ayarlarını yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 05/31/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9252e3bb8ccddb810074b485f6f073f1bda3f05
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804446"
---
# <a name="configure-azure-ad-role-settings-in-pim"></a>PıM 'de Azure AD rol ayarlarını yapılandırma

Ayrıcalıklı rol yöneticisi, uygun rol atamasını etkinleştiren bir kullanıcının deneyimini değiştirme da dahil olmak üzere, kuruluşlarındaki Azure Active Directory (Azure AD) Privileged Identity Management (PıM) özelleştirebilir.

## <a name="open-role-settings"></a>Rol ayarlarını aç

Bir Azure AD rolünün ayarlarını açmak için aşağıdaki adımları izleyin.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Ayarlar**' a tıklayın.

    ![Azure AD rolleri-ayarlar](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. **Roller**' e tıklayın.

1. Ayarlarını yapılandırmak istediğiniz role tıklayın.

    ![Azure AD rolleri-ayarlar roller](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Her rolün ayarlar sayfasında yapılandırabilmeniz için çeşitli ayarlar vardır. Bu ayarlar yalnızca **uygun** atamalar olan kullanıcıları etkiler, **kalıcı** atamalar değildir.

## <a name="activations"></a>Etkinleştirmeler

Rolün süresi dolmadan önce etkin kalacak en uzun süreyi saat cinsinden ayarlamak için **etkinleştirmeler** kaydırıcısını kullanın. Bu değer 1 ila 72 saat arasında olabilir.

## <a name="notifications"></a>Bildirimler

Roller etkinleştirildiğinde yöneticilerin e-posta bildirimleri alıp almamayacağını belirtmek için **Bildirimler** anahtarını kullanın. Bu, yetkisiz veya ılyasal etkinleştirmeleri saptamak için yararlı olabilir.

**Etkin**olarak ayarlandığında, bildirimlerin gönderileceği yer:

- Ayrıcalıklı Rol Yöneticisi
- Güvenlik Yöneticisi
- Genel Yönetici

Daha fazla bilgi için bkz. [PIM 'de e-posta bildirimleri](pim-email-notifications.md).

## <a name="incidentrequest-ticket"></a>Olay/Istek bileti

Uygun yöneticilerin rolünü etkinleştirdiklerinde bilet numarası içerip içermediğini belirtmek için **olay/istek bileti** anahtarını kullanın. Bu, rol erişim denetimleri gerçekleştirdiğinizde yararlı olabilir.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Kullanıcıların rollerini etkinleştirebilmeleri için önce MFA ile kimliklerini doğrulamaları gerekip gerekmediğini belirtmek için **Multi-Factor Authentication** anahtarını kullanın. Her bir rolü etkinleştirdiklerinde değil, her oturum için yalnızca bir kez doğrulaması gerekir. MFA 'yı etkinleştirdiğinizde göz önünde bulundurmanız gereken iki ipucu vardır:

* Kendi e-posta adresleri (genellikle @outlook.com, ancak her zaman değil) için Microsoft hesabı olan kullanıcılar, Azure MFA için kayıt yaptıramaz. Kullanıcılara Microsoft hesaplarıyla roller atamak istiyorsanız, bunları kalıcı Yöneticiler yapmanız ya da bu rol için MFA 'yı devre dışı bırakmanız gerekir.
* Azure AD ve Office365 için yüksek ayrıcalıklı roller için MFA 'yı devre dışı bırakayükleyemezsiniz. Bu roller dikkatle Korunabileceğinden bu bir güvenlik özelliğidir:  
  
  * Azure Information Protection Yöneticisi
  * Faturalama Yöneticisi
  * Bulut Uygulaması Yöneticisi
  * Uyumluluk Yöneticisi
  * Koşullu Erişim Yöneticisi
  * CRM Hizmet Yöneticisi
  * Müşteri Kasası Erişimi Onaylayıcısı
  * Dizin Yazıcıları
  * Exchange Yöneticisi
  * Genel Yönetici
  * Intune Hizmet Yöneticisi
  * Power BI Hizmet Yöneticisi
  * Ayrıcalıklı Rol Yöneticisi
  * Güvenlik Yöneticisi
  * SharePoint Hizmet Yöneticisi
  * Skype Kurumsal Yöneticisi
  * Kullanıcı Yöneticisi

Daha fazla bilgi için bkz. [Multi-Factor Authentication (MFA) ve PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Onay iste

Bir rolü etkinleştirmek için onay gerektirmek isterseniz, aşağıdaki adımları izleyin.

1. **Onay gerektir** anahtarını **etkin**olarak ayarlayın. Bölme, onaylayanları seçmek için seçeneklerle birlikte genişletilir.

    ![Azure AD rolleri-ayarlar-onay gerektir](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Herhangi bir **onaylayan belirtmezseniz,** ayrıcalıklı rol yöneticileri varsayılan onaylayanlar olur. Ayrıcalıklı rol yöneticilerinin bu rolün **Tüm** etkinleştirme isteklerini onaylaması gerekir.

1. Onaylayanları belirtmek için **onaylayanları Seç**' e tıklayın.

    ![Azure AD rolleri-ayarlar-onay gerektir](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Bir veya daha fazla onaylayan seçin ve ardından **Seç**' e tıklayın. Kullanıcılar veya gruplar ' ı seçebilirsiniz. En az 2 onaylayan önerilir. Kendi kendine onayına izin verilmez.

    Seçimleriniz seçili onaylayanlar listesinde görünür.

1. Tüm rol ayarlarınızı belirledikten sonra değişikliklerinizi kaydetmek için **Kaydet** ' e tıklayın.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rolleri atama](pim-how-to-add-role-to-user.md)
- [PıM 'de Azure AD rolleri için güvenlik uyarılarını yapılandırma](pim-how-to-configure-security-alerts.md)
