---
title: Kurumsal Güvenlik Paketi ile Azure HDInsight mimarisi
description: Kurumsal Güvenlik Paketi ile Azure HDInsight güvenliğini nasıl planlayamanızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: omidm
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/11/2020
ms.openlocfilehash: 452a3b04637126b40aca907178bebd6f74ec4481
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365806"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>HDInsight'ta Kurumsal Güvenlik Paketini Kullanma

Standart Azure HDInsight kümesi tek kullanıcılı bir kümedir. Büyük veri iş yükleri oluşturan daha küçük uygulama ekiplerine sahip çoğu şirket için uygundur. Her kullanıcı isteğe bağlı özel bir küme oluşturabilir ve artık ihtiyaç duyulmadığında kümeyi yok edebilir.

Birçok kuruluş, BT ekiplerinin kümeleri yönettiği bir modele doğru hareket etti ve birden çok uygulama ekibi kümeleri paylaşıyor. Bu büyük işletmelerin Azure HDInsight'taki her kümeye çok kullanıcılı erişimi gerekir.

HDInsight, yönetilen bir şekilde popüler bir kimlik sağlayıcısı olan Active Directory'ye dayanır. HDInsight'ı [Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/overview.md)ile tümleştirerek, etki alanı kimlik bilgilerinizi kullanarak kümelere erişebilirsiniz.

HDInsight'taki sanal makineler (VM'ler) sağlanan etki alanınıza katılır. Bu nedenle, HDInsight'ta çalışan tüm hizmetler (Apache Ambari, Apache Hive sunucusu, Apache Ranger, Apache Spark tutumlu sunucusu ve diğerleri) kimlik doğrulaması yapılan kullanıcı için sorunsuz bir şekilde çalışır. Yöneticiler daha sonra kümedeki kaynaklar için rol tabanlı erişim denetimi sağlamak için Apache Ranger'ı kullanarak güçlü yetkilendirme ilkeleri oluşturabilir.

## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight’ı Active Directory ile tümleştirin

Açık kaynak kodlu Apache Hadoop kimlik doğrulama ve güvenlik için Kerberos protokolüne dayanır. Bu nedenle, Kurumsal Güvenlik Paketi (ESP) ile HDInsight küme düğümleri Azure AD DS tarafından yönetilen bir etki alanına birleştirilir. Kerberos güvenliği kümedeki Hadoop bileşenleri için yapılandırılmıştır.

Aşağıdaki şeyler otomatik olarak oluşturulur:

- Her Hadoop bileşeni için bir servis ilkesi
- Etki alanına katılan her makine için bir makine müdürü
- Bu hizmet ve makine ilkelerini depolamak için her küme için bir Organizasyonbirimi (OU)

Özetlemek gerekirse, şu ile bir ortam ayarlamanız gerekir:

- Etkin Dizin etki alanı (Azure AD DS tarafından yönetilir). **Azure HDInsight ile çalışmak için alan adı 39 karakter veya daha az olmalıdır.**
- Azure AD DS'de güvenli LDAP (LDAPS) etkinleştirildi.
- HdInsight sanal ağından Azure AD DS sanal ağına doğru ağ bağlantısı, onlar için ayrı sanal ağlar seçerseniz. HDInsight sanal ağının içindeki bir VM,sanal ağ üzerinden Azure AD DS'ye bir görüş açısına sahip olmalıdır. HDInsight ve Azure AD DS'leri aynı sanal ağda dağıtılırsa, bağlantı otomatik olarak sağlanır ve başka bir işlem gerekmez.

## <a name="set-up-different-domain-controllers"></a>Farklı etki alanı denetleyicileri ayarlama

HDInsight şu anda kümenin Kerberos iletişimi için kullandığı ana etki alanı denetleyicisi olarak yalnızca Azure AD DS'yi destekler. Ancak, böyle bir kurulum HDInsight erişimi için Azure AD DS'yi etkinleştirmeye yol açtığı sürece, diğer karmaşık Active Directory kurulumları da mümkündür.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services

[Azure AD DS,](../../active-directory-domain-services/overview.md) Windows Server Active Directory ile tam uyumlu yönetilen bir etki alanı sağlar. Microsoft, yüksek kullanılabilir (HA) bir kurulumda etki alanını yönetme, düzeltme ve izleme ile ilgilenir. Etki alanı denetleyicilerini koruma konusunda endişelenmeden kümenizi dağıtabilirsiniz.

Kullanıcılar, gruplar ve parolalar Azure AD'den eşitlenir. Azure AD örneğinizdeki tek yönlü eşitleme, kullanıcıların aynı şirket kimlik bilgilerini kullanarak kümede oturum açmalarına olanak tanır.

Daha fazla bilgi için bkz: [Azure AD DS kullanarak ESP ile HDInsight kümelerini yapılandırın.](./apache-domain-joined-configure-using-azure-adds.md)

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>IaaS VM'lerde Şirket Içi Aktif Dizin veya Active Directory

Etki alanınız için şirket içi Active Directory örneğiniz veya daha karmaşık Active Directory kurulumlarınız varsa, Azure AD Connect'i kullanarak bu kimlikleri Azure AD ile senkronize edebilirsiniz. Daha sonra, Bu Etkin Dizin kiracıüzerinde Azure AD DS etkinleştirebilirsiniz.

Kerberos parola karmalarına dayandığından, [Azure AD DS'de parola karma eşitlemesini etkinleştirmeniz](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md)gerekir.

Active Directory Federation Services (AD FS) ile federasyon kullanıyorsanız, parola karma eşitlemesini etkinleştirmeniz gerekir. (Önerilen kurulum için [bu videoyu](https://youtu.be/qQruArbu2Ew)izleyin.) Parola karma eşitleme, AD FS altyapınızın başarısız olması durumunda olağanüstü durum kurtarmaile yardımcı olur ve ayrıca sızdırılmış kimlik bilgisi koruması sağlamaya da yardımcı olur. Daha fazla bilgi için bkz: [Azure AD Connect eşitlemi ile parola karma eşitlemesini etkinleştir.](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md)

Azure AD ve Azure AD DS olmadan sadece IaaS VM'lerde şirket içi Active Directory veya Active Directory'yi kullanmak, ESP'li HDInsight kümeleri için desteklenen bir yapılandırma değildir.

Federasyon kullanılıyorsa ve parola kalıpları doğru eşitleniyorsa, ancak kimlik doğrulama hataları alıyorsanız, PowerShell hizmet sorumlusu için bulut parolası kimlik doğrulamasının etkin olup olmadığını denetleyin. Değilse, Azure AD kiracınız için bir [Home Realm Discovery (HRD) ilkesi](../../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) ayarlamanız gerekir. HRD ilkesini denetlemek ve ayarlamak için:

1. Önizleme [Azure AD PowerShell modüllerini](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2)yükleyin.

   ```powershell
   Install-Module AzureAD
   ```

2. Genel yönetici (kiracı yönetici) kimlik bilgilerini kullanarak bağlanın.

   ```powershell
   Connect-AzureAD
   ```

3. Microsoft Azure PowerShell hizmet sorumlusunun zaten oluşturulıp oluşturulmadığını denetleyin.

   ```powershell
   Get-AzureADServicePrincipal -SearchString "Microsoft Azure Powershell"
   ```

4. Yoksa, hizmet ilkesini oluşturun.

   ```powershell
   $powershellSPN = New-AzureADServicePrincipal -AppId 1950a258-227b-4e31-a9cf-717495945fc2
   ```

5. İlkeyi oluşturun ve bu hizmet ilkesine iliştirin.

   ```powershell
    # Determine whether policy exists
    Get-AzureADPolicy | Where {$_.DisplayName -eq "EnableDirectAuth"}

    # Create if not exists
    $policy = New-AzureADPolicy `
        -Definition @('{"HomeRealmDiscoveryPolicy":{"AllowCloudPasswordValidation":true}}') `
        -DisplayName "EnableDirectAuth" `
        -Type "HomeRealmDiscoveryPolicy"

    # Determine whether a policy for the service principal exist
    Get-AzureADServicePrincipalPolicy `
        -Id $powershellSPN.ObjectId

    # Add a service principal policy if not exist
    Add-AzureADServicePrincipalPolicy `
        -Id $powershellSPN.ObjectId `
        -refObjectID $policy.ID
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight kümelerini ESP ile yapılandırın](apache-domain-joined-configure-using-azure-adds.md)
- [ESP ile HDInsight kümeleri için Apache Hive ilkelerini yapılandırın](apache-domain-joined-run-hive.md)
- [ESP ile HDInsight kümelerini yönetme](apache-domain-joined-manage.md)
