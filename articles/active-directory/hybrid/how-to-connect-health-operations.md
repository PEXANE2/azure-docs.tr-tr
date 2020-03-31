---
title: Azure Active Directory Connect Health işlemleri
description: Bu makalede, Azure AD Bağlantı Durumu dağıtıldıktan sonra gerçekleştirilebilecek ek işlemler açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2017
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef908429d359020282920d73480a472bfde0aa60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261521"
---
# <a name="azure-active-directory-connect-health-operations"></a>Azure Active Directory Connect Health işlemleri
Bu konu, Azure Etkin Dizin (Azure AD) Bağlantı Durumu'nu kullanarak gerçekleştirebileceğiniz çeşitli işlemleri açıklar.

## <a name="enable-email-notifications"></a>E-posta bildirimlerini etkinleştirme
Uyarılar kimlik altyapınızın sağlıklı olmadığını gösterdiğinde, Azure AD Connect Health hizmetini e-posta bildirimleri gönderecek şekilde yapılandırabilirsiniz. Bu, bir uyarı oluşturulduğunda ve çözüldüğünde oluşur.

![Azure AD Connect Sağlık e-posta bildirim ayarlarının ekran görüntüsü](./media/how-to-connect-health-operations/email_noti_discover.png)

> [!NOTE]
> E-posta bildirimleri varsayılan olarak etkinleştirilir.
>

### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure AD Connect Sistem e-posta bildirimlerini etkinleştirmek için
1. E-posta bildirimi almak istediğiniz hizmet için **Uyarılar** bıyığını açın.
2. Eylem çubuğundan **Bildirim Ayarları'nı**tıklatın.
3. E-posta bildirim anahtarında **AIŞ'ı**seçin.
4. Tüm genel yöneticilerin e-posta bildirimleri almasını istiyorsanız onay kutusunu seçin.
5. E-posta bildirimlerini diğer e-posta adreslerinde almak istiyorsanız, bunları **Ek E-posta Alıcıları** kutusuna belirtin. Bu listeden bir e-posta adresini kaldırmak için girişi sağ tıklatın ve **Sil'i**seçin.
6. Değişiklikleri sonuçlandırmak için **Kaydet'i**tıklatın. Değişiklikler yalnızca siz kaydedindikten sonra etkili olur.

>[!NOTE] 
> Arka uç hizmetimizde eşitleme isteklerini işleyen sorunlar olduğunda, bu hizmet hatanın ayrıntılarını içeren bir bildirim e-postasını kiracınızın yönetim iletişim e-posta adresine (es) gönderir. Müşterilerden, bazı durumlarda bu iletilerin hacminin son derece büyük olduğu yönündeki geri bildirimler duyduk, bu nedenle bu iletileri gönderme şeklimizi değiştiriyoruz. 
>
> Her meydana geldiğinde her eşitleme hatası için bir ileti göndermek yerine, arka uç hizmetinin döndürüldettiği tüm hataların günlük özetini göndeririz. Bu, müşterilerin bu hataları daha verimli bir şekilde işlemesini sağlar ve yinelenen hata iletilerinin sayısını azaltır.
>
> Bu değişikliğin 15 Ocak 2020'de hayata geçirilmesini planlıyoruz.

## <a name="delete-a-server-or-service-instance"></a>Sunucu veya hizmet örneğini silme

>[!NOTE] 
> Silme adımları için Azure AD premium lisansı gereklidir.

Bazı durumlarda, bir sunucunun izlenmesini kaldırmak isteyebilirsiniz. Bir sunucuyu Azure AD Connect Health hizmetinden kaldırmak için bilmeniz gerekenler aşağıda veda edebilirsiniz.

Bir sunucuyu silerken aşağıdakilerden haberdar olun:

* Bu eylem, bu sunucudan daha fazla veri toplamayı durdurur. Bu sunucu izleme hizmetinden kaldırılır. Bu eylemden sonra, bu sunucu için yeni uyarıları, izlemeveya kullanım analizi verilerini görüntüleyemeniz mümkün değildir.
* Bu eylem, sunucunuzdan Sistem Durumu Aracısı'nı kaldırmaz. Bu adımı gerçekleştirmeden önce Sağlık Aracısı'nı kaldırmadıysanız, sunucuda Sistem Durumu Temsilcisiyle ilgili hatalar görebilirsiniz.
* Bu eylem, bu sunucudan zaten toplanan verileri silmez. Bu veriler Azure veri saklama ilkesine uygun olarak silinir.
* Bu eylemi yaptıktan sonra, aynı sunucuyu yeniden izlemeye başlamak istiyorsanız, bu sunucudaki Sistem Durumu Aracısını kaldırmanız ve yeniden yüklemeniz gerekir.

### <a name="delete-a-server-from-the-azure-ad-connect-health-service"></a>Azure AD Connect Sistem Durumu hizmetinden bir sunucuyu silme

>[!NOTE] 
> Silme adımları için Azure AD premium lisansı gereklidir.

Azure AD, Etkin Dizin Federasyonu Hizmetleri (AD FS) ve Azure AD Bağlantısı (Eşitleme) için Sistem Durumu'na Bağlanır:

1. Kaldırılacak sunucu adını seçerek **Sunucu Listesi'nden** **Sunucu** bıçağını açın.
2. **Sunucu** çubuğunda, eylem çubuğundan **Sil'i**tıklatın.
![Azure AD Connect Health silme sunucusunun ekran görüntüsü](./media/how-to-connect-health-operations/DeleteServer2.png)
3. Onay kutusuna sunucu adını yazarak onaylayın.
4. **Sil'i**tıklatın.

Azure Etkin Dizin Etki Alanı Hizmetleri için Azure AD Bağlantı Durumu:

1. Etki **Alanı Denetleyicileri** panosunu açın.
2. Kaldırılacak etki alanı denetleyicisini seçin.
3. Eylem çubuğundan **Seçili Sil'i**tıklatın.
4. Sunucuyu silmek için eylemi onaylayın.
5. **Sil'i**tıklatın.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Azure AD Connect Sistem Durumu hizmetinden bir hizmet örneğini silme
Bazı durumlarda, bir hizmet örneğini kaldırmak isteyebilirsiniz. Azure AD Connect Health hizmetinden bir hizmet örneğini kaldırmak için bilmeniz gerekenler aşağıda veda edebilirsiniz.

Bir hizmet örneğini silerken aşağıdakilerden haberdar olun:

* Bu eylem, geçerli hizmet örneğini izleme hizmetinden kaldırır.
* Bu eylem, Bu hizmet örneğinin bir parçası olarak izlenen sunucuların hiçbirinden Sistem Durumu Aracısı'nı kaldırmaz veya kaldırmaz. Bu adımı gerçekleştirmeden önce Sistem Durumu Aracısı'nı kaldırmadıysanız, sunucularda Sağlık Aracısı ile ilgili hatalar görebilirsiniz.
* Bu hizmet örneğindeki tüm veriler Azure veri saklama ilkesine uygun olarak silinir.
* Bu eylemi gerçekleştirdikten sonra, hizmeti izlemeye başlamak istiyorsanız, Tüm sunucularda Sistem Durumu Aracısı'nı kaldırın ve yeniden yükleyin. Bu eylemi yaptıktan sonra, aynı sunucuyu yeniden izlemeye başlamak istiyorsanız, Sistem Durumu Aracısını bu sunucuda kaldırın, yeniden yükleyin ve kaydedin.

#### <a name="to-delete-a-service-instance-from-the-azure-ad-connect-health-service"></a>Azure AD Bağlantı Durumu hizmetinden bir hizmet örneğini silmek için
1. Kaldırmak istediğiniz servis tanımlayıcısını (çiftlik adı) seçerek **Servis Listesi'nden** **Servis** bıçağını açın. 
2. **Servis** bıçağında, eylem çubuğundan **Sil'i**tıklatın. 
![Azure AD Connect Health silme hizmetinin ekran görüntüsü](./media/how-to-connect-health-operations/DeleteServer.png)
3. Onay kutusuna servis adını yazarak onaylayın (örneğin: sts.contoso.com).
4. **Sil'i**tıklatın.
   <br><br>

[//]: # (RBAC bölümünün başlangıcı)
## <a name="manage-access-with-role-based-access-control"></a>Rol Tabanlı Erişim Denetimi ile erişimi yönetme
Azure AD Connect Health için [Rol Tabanlı Erişim Denetimi (RBAC),](../../role-based-access-control/role-assignments-portal.md) genel yöneticiler dışındaki kullanıcılara ve gruplara erişim sağlar. RBAC, hedeflenen kullanıcılara ve gruplara roller atar ve dizininizdeki genel yöneticileri sınırlamak için bir mekanizma sağlar.

### <a name="roles"></a>Roller
Azure AD Connect Health aşağıdaki yerleşik rolleri destekler:

| Rol | İzinler |
| --- | --- |
| Sahip |Sahipler *erişimi yönetebilir* (örneğin, bir kullanıcıya veya gruba bir rol atayabilir), portaldaki tüm bilgileri (örneğin, uyarıları görüntülemek) *görüntüleyebilir* ve Azure AD Connect Health'teki ayarları (örneğin, e-posta bildirimleri) *değiştirebilir.* <br>Varsayılan olarak, Azure AD global yöneticilerine bu rol atanır ve bu değiştirilemez. |
| Katılımcı |Katılımcılar portaldaki tüm bilgileri (örneğin, uyarıları görüntüleyebilir) *görüntüleyebilir* ve Azure AD Connect Health içindeki ayarları (örneğin, e-posta bildirimleri) *değiştirebilir.* |
| Okuyucu |Okuyucular, Azure AD Connect Health içindeki portaldaki tüm bilgileri (örneğin, uyarıları görüntüleyebilir) *görüntüleyebilir.* |

Diğer tüm roller (Kullanıcı Erişim Yöneticileri veya DevTest Labs Kullanıcıları gibi) roller portal deneyiminde mevcut olsa bile Azure AD Connect Health'e erişmeye hiçbir etkisi yoktur.

### <a name="access-scope"></a>Erişim kapsamı
Azure AD Connect Health erişimi iki düzeyde yönetmeyi destekler:

* **Tüm hizmet örnekleri**: Bu, çoğu durumda önerilen yoldur. Azure AD Connect Health tarafından izlenen tüm rol türlerindeki tüm hizmet örneklerinin (örneğin, bir AD FS çiftliği) erişimini denetler.
* **Hizmet örneği**: Bazı durumlarda, rolün türlerine veya bir hizmet örneğine göre erişimi ayırmanız gerekebilir. Bu durumda, hizmeti örnek düzeyinde erişimi yönetebilirsiniz.  

Son kullanıcının dizin veya hizmet örneği düzeyinde erişimi varsa izin verilir.

### <a name="allow-users-or-groups-access-to-azure-ad-connect-health"></a>Kullanıcıların veya grupların Azure AD Bağlantı Durumu'na erişmesine izin verme
Aşağıdaki adımlar, erişime nasıl izin verilebildiğini gösterir.
#### <a name="step-1-select-the-appropriate-access-scope"></a>Adım 1: Uygun erişim kapsamını seçin
Bir kullanıcının Azure AD Connect Health içindeki *tüm hizmet örnekleri* düzeyinde erişmesine izin vermek için, Azure AD Connect Health'te ana bıçağı açın.<br>

#### <a name="step-2-add-users-and-groups-and-assign-roles"></a>Adım 2: Kullanıcı ve gruplar ekleme ve roller atama
1. **Yapılilen** bölümünden **Kullanıcılar'ı**tıklatın.<br>
   ![Azure AD Connect Sistem kaynak kenar çubuğunun ekran görüntüsü](./media/how-to-connect-health-operations/startRBAC.png)
2. **Ekle'yi**seçin.
3. Rol bölmesini **seç'te** bir rol seçin (örneğin, **Sahip).**<br>
   ![Azure AD Connect Health RBAC Kullanıcıları penceresinin ekran görüntüsü](./media/how-to-connect-health-operations/RBAC_add.png)
4. Hedeflenen kullanıcının veya grubun adını veya tanımlayıcısını yazın. Aynı anda bir veya daha fazla kullanıcı veya grup seçebilirsiniz. **Seç'i**tıklatın.
   ![Azure AD Connect Health RBAC Kullanıcıları penceresinin ekran görüntüsü](./media/how-to-connect-health-operations/RBAC_select_users.png)
5. **Tamam'ı**seçin.<br>
6. Rol ataması tamamlandıktan sonra, kullanıcılar ve gruplar listede görünür.<br>
   ![Azure AD Connect Health RBAC Users penceresinin ekran görüntüsü, yeni kullanıcılar vurgulanır](./media/how-to-connect-health-operations/RBAC_user_list.png)

Artık listelenen kullanıcılar ve gruplar, atanan rollerine göre erişime sahip.

> [!NOTE]
> * Genel yöneticiler her zaman tüm işlemlere tam erişime sahiptir, ancak genel yönetici hesapları önceki listede bulunmaz.
> * Kullanıcıları Davet özelliği Azure AD Connect Health kapsamında desteklenmez.
>
>

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Adım 3: Bıçak konumunu kullanıcılarla veya gruplarla paylaşın
1. İzinleri atadıktan sonra, bir kullanıcı [buraya](https://aka.ms/aadconnecthealth)giderek Azure AD Connect Health'e erişebilir.
2. Bıçakta, kullanıcı bıçağı veya farklı kısımlarını panoya sabitleyebilir. **Pano simgesine Pin'i** tıklamanız yeterlidir.<br>
   ![Azure AD Connect Health RBAC pin blade ekran görüntüsü, pin simgesi vurgulanmış](./media/how-to-connect-health-operations/RBAC_pin_blade.png)

> [!NOTE]
> Reader rolü atanan bir kullanıcı Azure Marketi'nden Azure AD Bağlantı Durumu uzantısını alamaz. Kullanıcı bunu yapmak için gerekli "oluşturma" işlemini gerçekleştiremez. Kullanıcı yine de önceki bağlantıya giderek bıçak alabilirsiniz. Sonraki kullanım için, kullanıcı bıçağı panoya sabitleyebilir.
>
>

### <a name="remove-users-or-groups"></a>Kullanıcıları veya grupları kaldırma
Azure AD Connect Health RBAC'a eklenen bir kullanıcıyı veya grubu kaldırabilirsiniz. Kullanıcıya veya gruba sağ tıklayın ve **Kaldır'ı**seçin.<br>
![Azure AD Connect Health RBAC Users penceresinin ekran görüntüsü, Kaldır vurgulanmış](./media/how-to-connect-health-operations/RBAC_remove.png)

[//]: # (RBAC bölümünün sonu)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Azure AD Connect Health Aracısı yüklemesi](how-to-connect-health-agent-install.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](how-to-connect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](how-to-connect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](how-to-connect-health-adds.md)
* [Azure AD Connect Health ile ilgili SSS](reference-connect-health-faq.md)
* [Azure AD Connect Health sürüm geçmişi](reference-connect-health-version-history.md)
