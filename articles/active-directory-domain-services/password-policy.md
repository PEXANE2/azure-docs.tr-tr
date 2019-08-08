---
title: 'Azure Active Directory Domain Services: Parola ilkesi | Microsoft Docs'
description: Yönetilen etki alanlarında parola ilkelerini anlama
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: 8829f16e580d0b926781ce0a3e9f8e6a63cf3110
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68853768"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Yönetilen etki alanlarında parola ve hesap kilitleme ilkeleri
Bu makalede, yönetilen bir etki alanında varsayılan parola ilkeleri açıklanmaktadır. Ayrıca, bu ilkeleri nasıl yapılandırabileceğini de ele alır.

## <a name="fine-grained-password-policies-fgpp"></a>Hassas parola ilkeleri (FGPP)
Tek bir etki alanı içinde çok sayıda parola ilkesi belirtmek için hassas parola ilkeleri kullanın. FGPP, bir etki alanındaki farklı Kullanıcı kümelerine parola ve hesap kilitleme ilkeleri için farklı kısıtlamalar uygulamanıza olanak sağlar. Örneğin, ayrıcalıklı hesaplara katı parola ayarları uygulayabilirsiniz.

FGPP kullanarak aşağıdaki parola ayarlarını yapılandırabilirsiniz:
* Minimum parola uzunluğu
* Parola geçmişi
* Parolaların karmaşıklık gereksinimlerini karşılaması gerekir
* En az parola yaşı
* Maksimum parola yaşı
* Hesap kilitleme ilkesi
    * Hesap kilitleme süresi
    * İzin verilen başarısız oturum açma denemesi sayısı
    * Sonrasında başarısız oturum açma denemesi sayısını Sıfırla


## <a name="default-fine-grained-password-policy-settings-on-a-managed-domain"></a>Yönetilen bir etki alanında varsayılan hassas parola ilkesi ayarları
Aşağıdaki ekran görüntüsünde, Azure AD Domain Services yönetilen bir etki alanında yapılandırılan varsayılan hassas parola ilkesi gösterilmektedir.

![Varsayılan hassas parola ilkesi](./media/how-to/default-fgpp.png)

> [!NOTE]
> Varsayılan yerleşik hassas parola ilkesini değiştiremez veya silemezsiniz. ' AAD DC Administrators ' grubunun üyeleri özel FGPP oluşturabilir ve varsayılan yerleşik FGPP 'yi geçersiz kılmak (öncelikli hale getirmeleri) için onu yapılandırabilir.
>
>

## <a name="password-policy-settings"></a>Parola ilkesi ayarları
Yönetilen bir etki alanında aşağıdaki parola ilkeleri varsayılan olarak yapılandırılır:
* En az parola uzunluğu (karakter): 7
* En fazla parola yaşı (ömür): 90 gün
* Parolaların karmaşıklık gereksinimlerini karşılaması gerekir

### <a name="account-lockout-settings"></a>Hesap kilitleme ayarları
Yönetilen bir etki alanında aşağıdaki hesap kilitleme ilkeleri varsayılan olarak yapılandırılır:
* Hesap kilitleme süresi: 30
* İzin verilen başarısız oturum açma girişimi sayısı: 5
* Başarısız oturum açma denemesi sayısı sıfırlama sayısı: 30 dakika

2 dakika içinde beş geçersiz parola kullanılırsa, Kullanıcı hesapları 30 dakika boyunca kilitlidir. Hesaplar 30 dakika sonra otomatik olarak açılır.


## <a name="create-a-custom-fine-grained-password-policy-fgpp-on-a-managed-domain"></a>Yönetilen bir etki alanında özel bir hassas parola ilkesi (FGPP) oluşturma
Özel bir FGPP oluşturup, bunu yönetilen etki alanındaki belirli gruplara uygulayabilirsiniz. Bu yapılandırma, yönetilen etki alanı için yapılandırılmış varsayılan FGPP 'yi etkin bir şekilde geçersiz kılar.

> [!TIP]
> Yalnızca **' AAD DC Administrators '** grubunun üyeleri için özel hassas parola ilkeleri oluşturma izinleri vardır.
>
>

Ayrıca, özel hassas parola ilkeleri oluşturabilir ve bunları, yönetilen etki alanında oluşturduğunuz özel OU 'Lara uygulayabilirsiniz.

Aşağıdaki nedenlerle özel bir FGPP yapılandırabilirsiniz:
* Farklı bir hesap kilitleme ilkesi ayarlamak için.
* Yönetilen etki alanı için varsayılan parola yaşam süresi ayarını yapılandırmak için.

Yönetilen etki alanında özel bir FGPP oluşturmak için:
1. Yönetilen etki alanınızı yönetmek için kullandığınız Windows sanal makinesinde oturum açın (en az Windows Server 2012 R2 olmalıdır). Bir tane yoksa [Azure AD Domain Services etki alanını yönetmek](manage-domain.md)için yönergeleri izleyin.
2. VM üzerinde **Active Directory Yönetim Merkezi** başlatın.
3. Etki alanı adına tıklayın (örneğin, ' contoso100.com ').
4. Sistem kapsayıcısını açmak için **sistem** ' e çift tıklayın.
5. **Parola ayarları kapsayıcısı**çift tıklayın.
6. **AADDSSTFPSO**adlı yönetilen etki alanı için varsayılan yerleşik FGPP 'yi görürsünüz. Bu yerleşik FGPP 'yi değiştiremezsiniz. Bununla birlikte, yeni bir özel FGPP varsayılan FGPP 'yi geçersiz kılabilirsiniz.
7. Sağdaki **Görevler** panelinde, **Yeni** ' ye tıklayın ve **parola ayarları**' na tıklayın.
8. **Parola ayarları oluştur** iletişim kutusunda özel FGPP 'nin bir parçası olarak uygulanacak özel parola ayarlarını belirtin. Varsayılan FGPP 'yi geçersiz kılmak için önceliği uygun şekilde ayarlamayı unutmayın.

   ![Özel FGPP oluştur](./media/how-to/custom-fgpp.png)

   > [!TIP]
   > **Yanlışlıkla silinmeye karşı koru seçeneğinin işaretini kaldırın.** Bu seçenek işaretliyse, FGPP kaydedilemiyor.
   >
   >

9. **Doğrudan öğesine uygulanır**, **Ekle** düğmesine tıklayın. **Kullanıcıları veya grupları seç** Iletişim kutusunda **konumlar** düğmesine tıklayın.

   ![Kullanıcı ve grupları seçin](./media/how-to/fgpp-applies-to.png)

10. **Konumlar** iletişim kutusunda etki alanı adını genişletin ve **Aaddc kullanıcıları**' na tıklayın. Artık FGPP 'nin uygulanacağı yerleşik kullanıcılar OU 'sunda bir grup seçebilirsiniz.

    ![Grubun ait olduğu OU 'yu seçin](./media/how-to/fgpp-container.png)

11. Grubun adını yazın ve grubun var olduğunu doğrulamak için **adları denetle** düğmesine tıklayın.

    ![FGPP 'yi uygulamak için grubu seçin](./media/how-to/fgpp-apply-group.png)

12. Grubun adı **doğrudan** bölümünde görüntülenir. Bu değişiklikleri kaydetmek için **Tamam** düğmesine tıklayın.

    ![FGPP uygulandı](./media/how-to/fgpp-applied.png)

> [!TIP]
> **Özel bir OU 'daki Kullanıcı hesaplarına özel parola ilkeleri uygulamak için:** Hassas parola ilkeleri yalnızca gruplara uygulanabilir. Özel bir parola ilkesini yalnızca özel bir OU 'daki kullanıcılar için yapılandırmak üzere, bu OU 'daki kullanıcıları içeren bir grup oluşturun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Hassas parola ilkeleri Active Directory hakkında bilgi edinin](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [AD Yönetim merkezini kullanarak hassas parola ilkelerini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)
