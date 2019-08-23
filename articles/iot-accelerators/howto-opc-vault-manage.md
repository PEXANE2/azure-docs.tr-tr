---
title: OPC Kasası sertifika yönetimi hizmetini yönetme-Azure | Microsoft Docs
description: OPC Kasası kök CA sertifikalarını ve Kullanıcı izinlerini yönetin.
author: mregen
ms.author: mregen
ms.date: 8/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 6e53914b16bc126cdab7300d918dbb7b6b868728
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69973811"
---
# <a name="how-to-manage-the-opc-ua-certificate-service"></a>OPC UA sertifika hizmetini yönetme

Bu makalede, Azure 'daki OPC UA sertifika yönetimi hizmeti için yönetim görevleri, veren CA sertifikalarını yenileme, sertifika Iptal listesinin (CRL) nasıl yenileneceği ve Kullanıcı erişiminin nasıl verileceği ve iptal edileceği açıklanmaktadır.

## <a name="create-or-renew-the-root-ca-certificate"></a>Kök CA sertifikası oluşturma veya yenileme

Kök CA sertifikasını oluşturmak için dağıtımdan sonra zorunlu bir adımdır. Geçerli bir veren CA sertifikası olmadan, hiçbir uygulama sertifikası imzalanamıyor ve verilemez.<br>Sertifikaları makul, güvenli ömürlerle yönetmek için [sertifika ömürleri](howto-opc-vault-secure-ca.md#certificates) hakkında bölümüne bakın. Veren CA sertifikası, yaşam süresinin yarısından sonra yenilenmelidir, ancak daha sonra yeni imzalanmış bir uygulama sertifikasının yapılandırılan yaşam süresi, verenin sertifikasının ömrünü aşmayacak.<br>
> [!IMPORTANT]
> Veren CA sertifikasını oluşturmak veya yenilemek için ' Yönetici ' rolü gereklidir.

1. Sertifika hizmetinizi `https://myResourceGroup-app.azurewebsites.net` açın ve oturum açın.
2. `Certificate Groups` Sayfasına gidin.
3. Listelenen bir `Default` sertifika grubu vardır. `Edit` öğesine tıklayın.
4. İçinde `Edit Certificate Group Details` , CA ve uygulama sertifikalarınızın konu adını ve yaşam süresini değiştirebilirsiniz.<br>Konu ve yaşam süreleri yalnızca ilk CA sertifikası verilmeden önce bir kez ayarlanmalıdır. İşlemler sırasında yaşam süresi değişiklikleri, verilen sertifikaların ve CRL 'lerin tutarsız ömürleri oluşmasına neden olabilir.
5. Geçerli bir konu girin, örneğin, `CN=My CA Root, O=MyCompany, OU=MyDepartment`.<br>
   > [!IMPORTANT]
   > Konunun değiştirilmesinin, verenin sertifikasını yenilemesi gerekir veya hizmet uygulama sertifikalarını imzalamaz. Yapılandırmanın konusu, etkin veren sertifikanın konusuyla ilgili olarak kontrol edilir. Konular eşleşmezse, sertifika imzalama reddedilir.
6. `Save` Düğmeye tıklayın.
7. Bu noktada bir ' yasak ' hatası alırsanız, Kullanıcı kimlik bilgilerinizin yeni bir kök sertifikayı değiştirme veya oluşturma izni yoktur. Varsayılan olarak, hizmeti dağıtan kullanıcının hizmeti ile yönetici ve imzalama rolleri varsa, diğer kullanıcıların AzureAD uygulama kaydına uygun şekilde ' onaylayan ', ' yazıcı ' veya ' Yönetici ' rollerine eklenmesi gerekir.
8. `Details` Düğmeye tıklayın. , `View Certificate Group Details` Güncelleştirilmiş bilgileri görüntülemelidir.
9. İlk veren CA sertifikasını vermek veya veren sertifikasını yenilemek için düğmeyetıklayın.`Renew CA Certificate` Devam `Ok` etmek için tuşuna basın.
10. Birkaç saniye `Certificate Details` sonra görüntülenir. OPC `Crl` UA uygulamalarınıza dağıtmak üzere en son CA sertifikasını ve CRL 'yi indirmek için veya ' a basın `Issuer` .
11. Artık OPC UA sertifika yönetimi hizmeti OPC UA uygulamalarına yönelik sertifikalar vermek için hazırdır.

## <a name="renew-the-crl"></a>CRL 'YI yenileme

Sertifika Iptal listesinin (CRL) yenilenmesi, düzenli aralıklarla uygulamalara dağıtılması gereken bir güncelleştirmedir. CRL dağıtım noktası x509 uzantısını destekleyen OPC UA cihazları, CRL 'YI mikro hizmet uç noktasından doğrudan güncelleştirebilir. Diğer OPC UA cihazları el ile güncelleştirmeler gerektirebilir veya en iyi durumda, güven listelerini sertifikalar ve CRL 'Ler ile güncelleştirmek için GDS sunucu gönderme uzantıları (*) kullanılarak güncellenebilir.

Aşağıdaki iş akışında, silinen durumlardaki tüm sertifika istekleri, için verilen veren CA sertifikasına karşılık gelen CRL 'lerde iptal edilir. CRL 'nin sürüm numarası 1 artırılır. <br>
> [!NOTE]
> OPC UA belirtimi, CRL için zorunlu ve belirleyici bir dağıtım modeli gerektirmediğinden, tüm verilen CRL 'Ler veren CA sertifikasının süresi dolmadan geçerli değildir.

> [!IMPORTANT]
> Veren CRL 'YI yenilemek için ' Yönetici ' rolü gereklidir.

1. Sertifika hizmetinizi `https://myResourceGroup.azurewebsites.net` açın ve oturum açın.
2. `Certificate Groups` Sayfasına gidin.
3. `Details` Düğmeye tıklayın. `View Certificate Group Details` Geçerli sertifika ve CRL bilgilerini görüntülemelidir.
4. OPC kasa depolama alanındaki tüm etkin veren sertifikaları için güncelleştirilmiş bir CRL vermek üzere düğmeyetıklayın.`Update CRL Revocation List(CRL)`
5. Birkaç saniye `Certificate Details` sonra görüntülenir. OPC `Crl` UA uygulamalarınıza dağıtmak üzere en son CA sertifikasını ve CRL 'yi indirmek için veya ' a basın `Issuer` .

## <a name="manage-user-roles"></a>Kullanıcı rollerini yönetme

OPC Kasası mikro hizmeti için Kullanıcı rolleri Azure Active Directory Kurumsal uygulamada yönetilir.

Rol tanımlarının ayrıntılı bir açıklaması için [Roller](howto-opc-vault-secure-ca.md#roles) bölümüne bakın.

Varsayılan olarak, Kiracıdaki kimliği doğrulanmış bir Kullanıcı hizmette ' okuyucu ' olarak oturum açabilir. Daha yüksek ayrıcalıklı roller, Azure portal veya PowerShell kullanarak el ile yönetim gerektirir.

### <a name="add-user"></a>Kullanıcı Ekle

1. Azure portal `portal.azure.com`açın.
2. Öğesine `Azure Active Directory` /gidin. `Enterprise applications`
3. OPC Kasası mikro hizmetinin kaydını varsayılan `resourceGroupName-service`olarak seçin.
4. `Users and Groups` sayfasına gidin.
5. `Add User` öğesine tıklayın.
6. Kullanıcıyı belirli bir role atamayı seçin veya davet edin.
7. Kullanıcılar için rol seçin.
8. `Assign` Düğmeye basın.
9. `Administrator` Veya`Approver` rolündeki kullanıcılar için Azure Key Vault erişim ilkeleri eklemeye devam edin.

### <a name="remove-user"></a>Kullanıcı Kaldır

1. Azure portal `portal.azure.com`açın.
2. Öğesine `Azure Active Directory` /gidin. `Enterprise applications`
3. OPC Kasası mikro hizmetinin kaydını varsayılan `resourceGroupName-service`olarak seçin.
4. `Users and Groups` sayfasına gidin.
5. Kaldırılacak rolü olan bir kullanıcı seçin.
6. `Remove` Düğmeye basın.
7. Kaldırılan yöneticileri ve onaylayanları Azure Key Vault ilkelerden kaldırın.

### <a name="add-user-access-policy-to-azure-key-vault"></a>Azure Key Vault Kullanıcı erişim Ilkesi Ekle

**Onaylayanlar** ve **Yöneticiler**için ek erişim ilkeleri gerekir.

Varsayılan olarak, hizmet kimliğinin, yükseltilmiş işlemleri veya değişikliklerin kullanıcı kimliğine bürünme olmadan gerçekleşmesini engellemek için Key Vault erişim için yalnızca sınırlı izinlere sahip olması gerekir. Temel hizmet izinleri `Get` `List` hem gizli dizi hem de sertifikalara yöneliktir. Gizli diziler için, yalnızca bir istisna vardır. hizmet, bir `Delete` Kullanıcı tarafından kabul edildiğinde gizli olmayan özel bir anahtar olabilir. Tüm diğer işlemler için kullanıcı kimliğine bürünme izinleri gerekir.<br>

#### <a name="for-an-approver-role-the-following-permissions-must-be-added-to-key-vault"></a>Bir **onaylayan rolü** için aşağıdaki izinler Key Vault eklenmelidir:

1. Azure portal `portal.azure.com`açın.
2. Dağıtım sırasında kullanılan OPC kasanıza `resourceGroupName`gidin.
3. Key Vault `resourceGroupName-xxxxx`gidin.
4. Öğesine `Access Policies`gidin.
5. `Add new` öğesine tıklayın.
6. Şablonu atla, gereksinimlerle eşleşen şablon yok.
7. Açık `Select Principal` ' a tıklayın ve eklenecek kullanıcıyı seçin veya kiracıya yeni bir kullanıcı davet edin.
8. Denetim `Key permissions` :`Get`ve en önemlisi`Sign`. `List`
9. Denetim `Secret permissions`: `Get`, `List`, ve.`Set` `Delete`
10. Denetle `Certificate permissions`: `Get`ve .`List`
11. Tıklatın `Ok`.
12. `Save`değişikliklerine.

#### <a name="for-an-administrator-role-the-following-permissions-must-be-added-to-key-vault"></a>**Yönetici rolü** için aşağıdaki izinler Key Vault eklenmelidir:

1. Azure portal `portal.azure.com`açın.
2. Dağıtım sırasında kullanılan OPC kasanıza `resourceGroupName`gidin.
3. Key Vault `resourceGroupName-xxxxx`gidin.
4. Öğesine `Access Policies`gidin.
5. `Add new` öğesine tıklayın.
6. Şablonu atla, gereksinimlerle eşleşen şablon yok.
7. Açık `Select Principal` ' a tıklayın ve eklenecek kullanıcıyı seçin veya kiracıya yeni bir kullanıcı davet edin.
8. Denetim `Key permissions` :`Get`ve en önemlisi`Sign`. `List`
9. Denetim `Secret permissions`: `Get`, `List`, ve.`Set` `Delete`
10. Denetim `Certificate permissions`: `Get`, `List`, ,ve.`Update` `Create` `Import`
11. Tıklatın `Ok`.
12. `Save`değişikliklerine.

### <a name="remove-user-access-policy-from-azure-key-vault"></a>Azure Key Vault Kullanıcı erişim Ilkesini kaldırma

1. Azure portal `portal.azure.com`açın.
2. Dağıtım sırasında kullanılan OPC kasanıza `resourceGroupName`gidin.
3. Key Vault `resourceGroupName-xxxxx`gidin.
4. Öğesine `Access Policies`gidin.
5. Kaldırılacak kullanıcıyı bulun ve Kullanıcı erişimini silmek `... / Delete` için tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Artık OPC Kasası sertifikalarını ve kullanıcılarını nasıl yönetebileceğinizi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [OPC cihazlarıyla güvenli iletişim](howto-opc-vault-secure.md)
