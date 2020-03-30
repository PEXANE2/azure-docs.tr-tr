---
title: Özel alan adları ekleme ve doğrulama - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Active Directory'de bir etki alanı adını yönetmek için yönetim kavramları ve nasıl yapılacağını
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2395aa5984de2a9fe41e4778d16aba69bfef5192
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77559242"
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Azure Etkin Dizininizde özel alan adlarını yönetme

Etki alanı adı, birçok dizin kaynağı için tanımlayıcının önemli bir parçasıdır: bir kullanıcı için kullanıcı adının veya e-posta adresinin, bir grubun adresinin bir parçasıdır ve bazen bir uygulama için uygulama kimliği URI'nin bir parçasıdır. Azure Etkin Dizini'ndeki (Azure AD) bir kaynak, kaynağı içeren dizine ait bir etki alanı adı içerebilir. Azure AD'deki etki alanlarını yalnızca bir Global Administrator yönetebilir.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Azure REKLAM dizininizin birincil etki alanı adını ayarlama

Dizininiz oluşturulduğunda, 'contoso.onmicrosoft.com' gibi ilk etki alanı adı da birincil etki alanı adıdır. Birincil etki alanı, yeni bir kullanıcı oluşturduğunuzda yeni bir kullanıcıiçin varsayılan etki alanı adıdır. Birincil etki alanı adı ayarlamak, bir yöneticinin portalda yeni kullanıcılar oluşturması işlemini kolaylaştırır. Birincil etki alanı adını değiştirmek için:

1. Dizin için Global Administrator olan bir hesapla [Azure portalında](https://portal.azure.com) oturum açın.
2. **Azure Etkin Dizini'ni**seçin.
3. **Özel etki alanı adları**'nı seçin.
  
   ![Kullanıcı yönetimi sayfasını açma](./media/domains-manage/add-custom-domain.png)
4. Birincil etki alanı olmak istediğiniz etki alanının adını seçin.
5. Birincil komutu **yap'ı** seçin. İstendiğinde seçiminizi onaylayın.
  
   ![Etki alanı adını birincil ad yapma](./media/domains-manage/make-primary-domain.png)

Dizininizin birincil etki alanı adını, federe olmayan doğrulanmış özel etki alanı olarak değiştirebilirsiniz. Dizininizin birincil etki alanını değiştirmek, varolan kullanıcıların kullanıcı adını değiştirmez.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Azure AD kiracınıza özel alan adları ekleme

En fazla 900 yönetilen alan adı ekleyebilirsiniz. Tüm etki alanlarınızı şirket içi Active Directory ile federasyon için yapılandırıyorsanız, her dizinde en fazla 450 etki alanı adı ekleyebilirsiniz.

## <a name="add-subdomains-of-a-custom-domain"></a>Özel bir etki alanının alt etki alanlarını ekleme

Dizininize 'europe.contoso.com' gibi üçüncü düzey bir etki alanı adı eklemek istiyorsanız, önce contoso.com gibi ikinci düzey etki alanını eklemeniz ve doğrulamanız gerekir. Alt etki alanı Azure AD tarafından otomatik olarak doğrulanır. Eklediğiniz alt etki alanının doğrulanmış olduğunu görmek için tarayıcıdaki etki alanı listesini yenileyin.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Özel alan adınız için DNS kayıt şirketini değiştirirseniz ne yapmalıdır?

DNS kayıt şirketlerini değiştirirseniz, Azure AD'de ek yapılandırma görevi yoktur. Alan adını Azure AD ile kesintisiz olarak kullanmaya devam edebilirsiniz. Özel alan adınızı Office 365, Intune veya Azure AD'deki özel alan adlarına dayanan diğer hizmetlerle kullanıyorsanız, bu hizmetlerin belgelerine bakın.

## <a name="delete-a-custom-domain-name"></a>Özel alan adı silme

Kuruluşunuz artık bu etki alanı adını kullanmıyorsa veya bu etki alanı adını başka bir Azure REKLAMıyla kullanmanız gerekiyorsa Azure REKLAMınızdan özel bir etki alanı adını silebilirsiniz.

Özel bir etki alanı adını silmek için öncelikle dizininizdeki hiçbir kaynağın etki alanı adına dayanmadığından emin olmalısınız. Şuanda varsa dizininizden bir etki alanı adını silemezsiniz:

* Herhangi bir kullanıcının etki alanı adını içeren bir kullanıcı adı, e-posta adresi veya proxy adresi vardır.
* Herhangi bir grubun etki alanı adını içeren bir e-posta adresi veya proxy adresi vardır.
* Azure AD'nizdeki herhangi bir uygulamada etki alanı adını içeren bir uygulama kimliği URI vardır.

Özel alan adını silmeden önce Azure REKLAM dizininizdeki bu tür kaynakları değiştirmeniz veya silmeniz gerekir.

### <a name="forcedelete-option"></a>ForceDelete seçeneği

[Azure AD Yönetici Merkezi'nde](https://aad.portal.azure.com) veya Microsoft [Graph API'sini](https://docs.microsoft.com/graph/api/domain-forcedelete?view=graph-rest-beta)kullanarak bir etki alanı adını **Zorla Silebilirsiniz.** Bu seçenekler bir eşzamanlı işlem kullanır ve " "user@contoso.comgibi özel etki alanı adındaki tümuser@contoso.onmicrosoft.combaşvuruları ilk varsayılan etki alanı adına güncelleştirin. 

Azure portalında **ForceDelete'i** aramak için, alan adına 1000'den az başvuru olduğundan ve Exchange'in sağlama hizmeti olduğu tüm başvuruların [Exchange Yönetici Merkezi'nde](https://outlook.office365.com/ecp/)güncellenmesi veya kaldırılması gerektiğinden emin olmalısınız. Buna Exchange Mail Özellikli Güvenlik Grupları ve dağıtılmış listeler dahildir; daha fazla bilgi için [bkz. posta yla etkin güvenlik gruplarını kaldırma.](https://technet.microsoft.com/library/bb123521(v=exchg.160).aspx#Remove%20mail-enabled%20security%20groups) Ayrıca, aşağıdakilerden biri doğruysa **ForceDelete** işlemi başarılı olmaz:

* Office 365 etki alanı abonelik hizmetleri aracılığıyla bir etki alanı satın aldınız
* Başka bir müşteri kiracısı adına yöneten bir iş ortağısınız

**ForceDelete** işleminin bir parçası olarak aşağıdaki işlemler gerçekleştirilir:

* İlk varsayılan etki alanı adına özel alan adı referansları ile kullanıcıların UPN, E-posta Adresi ve ProxyAddress yeniden adlandırır.
* Özel alan adı için başvuruiçeren grupların E-posta Adresini ilk varsayılan etki alanı adına yeniden adlandırır.
* Özel etki alanı adına yapılan atıflarla uygulamaların tanımlayıcılarını ilk varsayılan etki alanı adına yeniden adlandırır.

Bir hata döndürülür:

* Yeniden adlandırılacak nesne sayısı 1000'den büyüktür
* Yeniden adlandırılacak uygulamalardan biri çok kiracılı bir uygulamadır

### <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Neden etki alanı silme ben bu etki alanı adı üzerinde Exchange hakim gruplar olduğunu belirten bir hata ile başarısız oluyor?** <br>
**A:** Bugün, Posta Destekli Güvenlik grupları ve dağıtılmış listeler gibi belirli gruplar Exchange tarafından sağlanmaktadır ve [Exchange Yönetici Merkezi'nde (EAC)](https://outlook.office365.com/ecp/)el ile temizlenmesi gerekir. Özel etki alanı adına dayanan ve başka bir etki alanı adına el ile güncellenmesi gereken proxyadresleri olabilir. 

**S: Ben yönetici\@contoso.com olarak giriş yapıyorum ama etki alanı adı "contoso.com silemez miyim?**<br>
**A:** Kullanıcı hesabı adınızda silmeye çalıştığınız özel alan adı için başvuruda bulunamazsınız. Global Administrator hesabının ilk varsayılan etki alanı adını (.onmicrosoft.com) kullandığından emin admin@contoso.onmicrosoft.comolun. Farklı bir Global Administrator hesabı yla admin@contoso.onmicrosoft.com oturum açın, örneğin veya hesabın bulunduğu admin@fabrikam.com"fabrikam.com" gibi başka bir özel etki alanı adı.

**S: Etki alanını sil düğmesini `In Progress` tıklattım ve Sil işleminin durumunu gördüm. Ne kadar sürer? Başarısız olursa ne olur?**<br>
**A:** Silme etki alanı işlemi, etki alanı adına yapılan tüm başvuruları yeniden adlandıran eşzamanlı bir arka plan görevidir. Bir iki dakika içinde tamamlar. Etki alanı silme başarısız olursa, şuna sahip olmadığınızı sağlayın:

* AppIdentifierURI ile alan adı üzerinde yapılandırılan uygulamalar
* Özel etki alanı adına başvuran posta özellikli herhangi bir grup
* Etki alanı adına 1000'den fazla başvuru

Koşullardan herhangi birinin yerine getirilmediğini fark ederseniz, başvuruları el ile temizleyin ve etki alanını yeniden silmeyi deneyin.

## <a name="use-powershell-or-the-microsoft-graph-api-to-manage-domain-names"></a>Etki alanı adlarını yönetmek için PowerShell veya Microsoft Graph API'yi kullanma

Azure Etkin Dizini'ndeki alan adları için çoğu yönetim görevi Microsoft PowerShell kullanılarak veya programlı olarak Microsoft Graph API kullanılarak tamamlanabilir.

* [Azure AD'de alan adlarını yönetmek için PowerShell'i kullanma](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#domains)
* [Etki alanı kaynak türü](https://docs.microsoft.com/graph/api/resources/domain?view=graph-rest-1.0)

## <a name="next-steps"></a>Sonraki adımlar

* [Özel etki alanı adı ekleme](/azure/active-directory/fundamentals/add-custom-domain?context=azure/active-directory/users-groups-roles/context/ugr-context)
* [Exchange Yönetici Merkezi'ndeki Exchange posta özellikli güvenlik gruplarını Azure AD'deki özel bir etki alanı adında kaldırma](https://technet.microsoft.com/library/bb123521(v=exchg.160).aspx#Remove%20mail-enabled%20security%20groups)
* [ForceDelete Microsoft Graph API ile özel bir etki alanı adı](https://docs.microsoft.com/graph/api/domain-forcedelete?view=graph-rest-beta)
