---
title: OPC Vault sertifika hizmeti nasıl yönetilir - Azure | Microsoft Dokümanlar
description: OPC Vault kök CA sertifikalarını ve kullanıcı izinlerini yönetin.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 890a25ed2cf11d657cad930815d78dbf968cc9f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71203659"
---
# <a name="manage-the-opc-vault-certificate-service"></a>OPC Vault sertifika hizmetini yönetme

Bu makalede, Azure'daki OPC Vault sertifika yönetimi hizmetinin yönetim görevleri açıklanmaktadır. İhraççı CA sertifikalarının nasıl yenilenileöğretilen, Sertifika İptal Listesinin (CRL) nasıl yenilenilip yenilenilene ve kullanıcı erişiminin nasıl verilip iptal edilene ilişkin bilgileri içerir.

## <a name="create-or-renew-the-root-ca-certificate"></a>Kök CA sertifikası oluşturma veya yenileme

OPC Vault'u dağıttıktan sonra kök CA sertifikasını oluşturmanız gerekir. Geçerli bir Veren CA sertifikası olmadan, uygulama sertifikalarını imzalayamaz veya düzenleyemezsiniz. Sertifikalarınızı makul ve güvenli ömürlerle yönetmek için [Sertifikalara](howto-opc-vault-secure-ca.md#certificates) bakın. Bir Veren CA sertifikasını ömrünün yarısından sonra yenileyin. Yenilerken, yeni imzalanmış bir uygulama sertifikasının yapılandırılan ömrünün, Veren CA sertifikasının ömrünü aşmaması gerektiğini de göz önünde bulundurun.
> [!IMPORTANT]
> Yönetici rolü, İhraççı CA sertifikasını oluşturmak veya yenilemek için gereklidir.

1. Sertifika hizmetinizi `https://myResourceGroup-app.azurewebsites.net`şu anda açın ve oturum açın.
2. Sertifika **Gruplarına**gidin.
3. Listelenen bir varsayılan sertifika grubu vardır. **Edit'i**seçin.
4. **Sertifika Grubu Ayrıntılarını Edit'te,** CA ve uygulama sertifikalarınızın özadını ve kullanım ömrünü değiştirebilirsiniz. Konu ve yaşam ömürleri ilk CA sertifikası verilmeden önce yalnızca bir kez ayarlanmalıdır. İşlemler sırasında ki yaşam boyu değişiklikler, verilen sertifikaların ve CRL'lerin tutarsız ömürleri ile sonuçlanabilir.
5. Geçerli bir konu girin `CN=My CA Root, O=MyCompany, OU=MyDepartment`(örneğin, ).<br>
   > [!IMPORTANT]
   > Konuyu değiştirirseniz, İhraççı sertifikasını yenilemeniz gerekir, yoksa hizmet uygulama sertifikalarını imzalamaz. Yapılandırma konusu etkin Veren sertifikasının konusuna göre kontrol edilir. Konular eşleşmiyorsa, sertifika imzalama reddedilir.
6. **Kaydet'i**seçin.
7. Bu noktada bir "yasak" hatayla karşılaşırsanız, kullanıcı kimlik bilgilerinizin yeni bir kök sertifikasını değiştirmek veya oluşturmak için yönetici izni yoktur. Varsayılan olarak, hizmeti dağıtan kullanıcının yöneticisi vardır ve hizmetle rolleri imzalar. Azure Etkin Dizin (Azure AD) uygulama kaydında uygun olduğu şekilde diğer kullanıcıların Onaylayıcı, Yazar veya Yönetici rollerine eklenmesi gerekir.
8. **Ayrıntılar'ı**seçin. Bu güncelleştirilmiş bilgileri göstermelidir.
9. İlk Veren CA sertifikasını vermek veya İhraççı sertifikasını yenilemek için **CA Sertifikasını** Yenile'yi seçin. Sonra **Tamam**’ı seçin.
10. Birkaç saniye sonra **Sertifika Ayrıntılarını**görürsünüz. OPC UA uygulamalarınıza dağıtım için en son CA sertifikasını ve CRL'yi indirmek **için, Veren** veya **Crl'yi**seçin.

Artık OPC UA sertifika yönetimi hizmeti, OPC UA uygulamaları için sertifika vermeye hazırdır.

## <a name="renew-the-crl"></a>CRL'yi yenile

CRL'nin yenilenmesi, uygulamalara düzenli aralıklarla dağıtılması gereken bir güncelleştirmedir. CRL Dağıtım Noktası X509 uzantısını destekleyen OPC UA aygıtları, CRL'yi mikrohizmet bitiş noktasından doğrudan güncelleyebilir. Diğer OPC UA aygıtları el ile güncelleştirmegerektirebilir veya güven listelerini sertifikalar ve CRL'lerle güncelleştirmek için GDS sunucu itme uzantıları (*) kullanılarak güncellenebilir.

Aşağıdaki iş akışında, silinen eyaletlerdeki tüm sertifika istekleri, verildiği Veren CA sertifikasına karşılık gelen CRL'lerde iptal edilir. CRL'nin sürüm numarası 1 ile artımlanır. <br>
> [!NOTE]
> Verilen tüm CRL'ler, İhraççı CA sertifikasının süresi dolana kadar geçerlidir. Bunun nedeni, OPC UA belirtiminin CRL için zorunlu, deterministik bir dağıtım modeli gerektirmemesidir.

> [!IMPORTANT]
> Yönetici rolü, Veren CRL'yi yenilemek için gereklidir.

1. Sertifika hizmetinizi `https://myResourceGroup.azurewebsites.net`şu anda açın ve oturum açın.
2. **Sertifika Grupları** sayfasına gidin.
3. **Ayrıntılar'ı**seçin. Bu, geçerli sertifikayı ve CRL bilgilerini göstermelidir.
4. OPC Vault depolama sındaki tüm etkin Veren sertifikalar için güncelleştirilmiş bir CRL vermek için **CRL İptal Listesini (CRL)** güncelleştir'i seçin.
5. Birkaç saniye sonra **Sertifika Ayrıntılarını**görürsünüz. OPC UA uygulamalarınıza dağıtım için en son CA sertifikasını ve CRL'yi indirmek **için, Veren** veya **Crl'yi**seçin.

## <a name="manage-user-roles"></a>Kullanıcı rollerini yönetme

Azure AD Kurumsal Uygulaması'ndaki OPC Vault mikro hizmetinin kullanıcı rollerini yönetebilirsiniz. Rol tanımlarının ayrıntılı bir açıklaması için [Bkz.](howto-opc-vault-secure-ca.md#roles)

Varsayılan olarak, kiracıdaki kimliği doğrulanmış bir kullanıcı hizmeti Okuyucu olarak oturum açabilir. Daha yüksek ayrıcalıklı roller, Azure portalında veya PowerShell'i kullanarak manuel yönetim gerektirir.

### <a name="add-user"></a>Kullanıcı ekle

1. Azure portalı açın.
2. Azure **Active Directory** > **Enterprise uygulamalarına**gidin.
3. OPC Vault microservice'in kaydını seçin `resourceGroupName-service`(varsayılan olarak, sizin).
4. **Kullanıcılara ve Gruplara**gidin.
5. **Kullanıcı Ekle'yi**seçin.
6. Belirli bir role atama için kullanıcıyı seçin veya davet edin.
7. Kullanıcılar için rolü seçin.
8. **Ata**'yı seçin.
9. Yönetici veya Onaylayıcı rolündeki kullanıcılar için Azure Anahtar Kasası erişim ilkeleri eklemeye devam edin.

### <a name="remove-user"></a>Kullanıcı kaldırma

1. Azure portalı açın.
2. Azure **Active Directory** > **Enterprise uygulamalarına**gidin.
3. OPC Vault microservice'in kaydını seçin `resourceGroupName-service`(varsayılan olarak, sizin).
4. **Kullanıcılara ve Gruplara**gidin.
5. Kaldıracak rolü olan bir kullanıcı seçin ve ardından **Kaldır'ı**seçin.
6. Yönetici veya Onaylayıcı rolündeki kaldırılan kullanıcılar için, bunları Azure Anahtar Vault ilkelerinden de kaldırın.

### <a name="add-user-access-policy-to-azure-key-vault"></a>Azure Anahtar Kasası'na kullanıcı erişim ilkesi ekleme

Onaylayıcılar ve Yöneticiler için ek erişim ilkeleri gereklidir.

Varsayılan olarak, hizmet kimliğinin, kullanıcı kimliğe bürünmeden gerçekleşen yüksek işlemleri veya değişiklikleri önlemek için Key Vault'a erişmek için yalnızca sınırlı izinleri vardır. Temel hizmet izinleri, hem sırlar hem de sertifikalar için Al ve Liste'dir. Sırlar için tek bir özel durum vardır: hizmet, kullanıcı tarafından kabul edildikten sonra gizli mağazadan özel bir anahtarı silebilir. Diğer tüm işlemler kullanıcı kimliğine bürünme izinleri gerektirir.

#### <a name="for-an-approver-role-the-following-permissions-must-be-added-to-key-vault"></a>Onaylayıcı rolü için Key Vault'a aşağıdaki izinlerin eklenmesi gerekir

1. Azure portalı açın.
2. Dağıtım sırasında kullanılan `resourceGroupName`OPC Vault'unuza gidin.
3. Anahtar Kasası'na `resourceGroupName-xxxxx`git.
4. Erişim **İlkeleri'ne**gidin.
5. **Yeni Ekle'yi**seçin.
6. Şablonu atlayın. Gereksinimlerle eşleşen bir şablon yok.
7. **Asılat'ı seçin**ve eklenecek kullanıcıyı seçin veya yeni bir kullanıcıyı kiracıya davet edin.
8. Aşağıdaki **Anahtar izinlerini**seçin : **Al**, **Listele**ve **İmzala**.
9. Aşağıdaki **Gizli izinleri**seçin: **Al**, **Liste**, **Set**, ve **Sil**.
10. Aşağıdaki **Sertifika izinlerini**seçin: **Alın** ve **Listele**.
11. **Tamam'ı**seçin ve **Kaydet'i**seçin.

#### <a name="for-an-administrator-role-the-following-permissions-must-be-added-to-key-vault"></a>Yönetici rolü için Key Vault'a aşağıdaki izinlerin eklenmesi gerekir

1. Azure portalı açın.
2. Dağıtım sırasında kullanılan `resourceGroupName`OPC Vault'unuza gidin.
3. Anahtar Kasası'na `resourceGroupName-xxxxx`git.
4. Erişim **İlkeleri'ne**gidin.
5. **Yeni Ekle'yi**seçin.
6. Şablonu atlayın. Gereksinimlerle eşleşen bir şablon yok.
7. **Asılat'ı seçin**ve eklenecek kullanıcıyı seçin veya yeni bir kullanıcıyı kiracıya davet edin.
8. Aşağıdaki **Anahtar izinlerini**seçin : **Al**, **Listele**ve **İmzala**.
9. Aşağıdaki **Gizli izinleri**seçin: **Al**, **Liste**, **Set**, ve **Sil**.
10. Aşağıdaki **Sertifika izinlerini**seçin: **Al,** **Listele,** **Güncelleştir,** **Oluştur**ve **İçe Aktar.**
11. **Tamam'ı**seçin ve **Kaydet'i**seçin.

### <a name="remove-user-access-policy-from-azure-key-vault"></a>Azure Anahtar Atlama'dan kullanıcı erişim ilkesini kaldırma

1. Azure portalı açın.
2. Dağıtım sırasında kullanılan `resourceGroupName`OPC Vault'unuza gidin.
3. Anahtar Kasası'na `resourceGroupName-xxxxx`git.
4. Erişim **İlkeleri'ne**gidin.
5. Kaldırılacak kullanıcıyı bulun ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Artık OPC Vault sertifikalarını ve kullanıcılarını nasıl yönetebileceğinizi öğrendiğiniz için şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [OPC cihazlarının güvenli iletişimi](howto-opc-vault-secure.md)
