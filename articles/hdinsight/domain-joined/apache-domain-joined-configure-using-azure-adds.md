---
title: Azure AD DS ile kurumsal güvenlik-Azure HDInsight
description: Azure Active Directory Domain Services kullanarak HDInsight Kurumsal Güvenlik Paketi kümesi ayarlamayı ve yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seodec18
ms.date: 02/03/2020
ms.openlocfilehash: cf239cbf69f3816e5ec03e07e2bd5fe370308f22
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78196867"
---
# <a name="enterprise-security-package-configurations-with-azure-active-directory-domain-services-in-hdinsight"></a>HDInsight 'ta Azure Active Directory Domain Services olan Kurumsal Güvenlik Paketi konfigürasyonları

Kurumsal Güvenlik Paketi (ESP) kümeleri, Azure HDInsight kümelerinde çok kullanıcılı erişim sağlar. Etki alanı kullanıcılarının kümelerle kimlik doğrulamak ve büyük veri işleri çalıştırmak için etki alanı kimlik bilgilerini kullanabilmesi için, ESP içeren HDInsight kümeleri bir etki alanına bağlıdır.

Bu makalede, Azure Active Directory Domain Services (Azure AD DS) kullanarak bir HDInsight kümesini ESP ile yapılandırmayı öğreneceksiniz.

> [!NOTE]  
> ESP, şu küme türleri için HDInsight 3,6 ve 4,0 ' de genel kullanıma sunulmuştur: Apache Spark, Interactive, Hadoop ve HBase. Apache Kafka küme türü için ESP yalnızca en iyi çaba desteğiyle önizlemededir. ESP GA tarihi (1 Ekim 2018) öncesinde oluşturulan ESP kümeleri desteklenmez.

## <a name="enable-azure-ad-ds"></a>Azure AD DS etkinleştirme

> [!NOTE]  
> Yalnızca kiracı yöneticileri Azure AD DS etkinleştirme ayrıcalıklarına sahiptir. Küme depolama alanı Azure Data Lake Storage 1. veya Gen2 ise, yalnızca temel Kerberos kimlik doğrulamasını kullanarak kümeye erişmesi gereken kullanıcılar için Azure Multi-Factor Authentication 'yi devre dışı bırakmanız gerekir. 
>
> Belirli kullanıcılar için Multi-Factor Authentication devre dışı bırakmak için [Güvenilen IP](../../active-directory/authentication/howto-mfa-mfasettings.md#trusted-ips) veya [koşullu erişimi](../../active-directory/conditional-access/overview.md) , *yalnızca* HDInsight kümesinin sanal ağı için IP aralığına erişirken kullanabilirsiniz. Koşullu erişim kullanıyorsanız, HDInsight sanal ağında Active Directory Hizmeti uç noktasının etkinleştirildiğinden emin olun.
>
> Küme depolama alanı Azure Blob depoise Multi-Factor Authentication devre dışı bırakın.

Azure AD DS etkinleştirilmesi, ESP ile bir HDInsight kümesi oluşturabilmeniz için bir önkoşuldur. Daha fazla bilgi için bkz. [Azure Portal kullanarak Azure Active Directory Domain Services etkinleştirme](../../active-directory-domain-services/tutorial-create-instance.md). 

Azure AD DS etkinleştirildiğinde, tüm kullanıcılar ve nesneler, varsayılan olarak Azure Active Directory (Azure AD) ile Azure AD DS arasında eşitlemeye başlar. Eşitleme işleminin uzunluğu, Azure AD 'deki nesne sayısına bağlıdır. Eşitleme, yüzlerce binlerce nesne için birkaç gün sürebilir. 

Azure AD DS ile birlikte kullandığınız etki alanı adı, HDInsight ile çalışmak için 39 karakter veya daha az olmalıdır.

Yalnızca HDInsight kümelerine erişmesi gereken grupları eşitlemeyi tercih edebilirsiniz. Yalnızca belirli grupları eşitlemeye yönelik bu seçenek *kapsamlı eşitleme*olarak adlandırılır. Yönergeler için bkz. [Azure AD 'den yönetilen etki alanınızı kapsamlı eşitlemeyi yapılandırma](../../active-directory-domain-services/scoped-synchronization.md).

Güvenli LDAP 'yi etkinleştirirken, etki alanı adını sertifikaya konu adı ve konu alternatif adı ' na koyun. Örneğin, etki alanı adınız *contoso100.onmicrosoft.com*ise, sertifikanın konu adı ve konu diğer adında tam adın bulunduğundan emin olun. Daha fazla bilgi için bkz. [Azure AD DS yönetilen etki alanı için GÜVENLI LDAP yapılandırma](../../active-directory-domain-services/tutorial-configure-ldaps.md). 

Aşağıdaki örnek, kendinden imzalı bir sertifika oluşturur. *Contoso100.onmicrosoft.com* etki alanı adı `Subject` (konu adı) ve `DnsName` (konu alternatif adı).

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
```

## <a name="check-azure-ad-ds-health-status"></a>Azure AD DS sistem durumunu denetle
**Yönet** kategorisinde **sistem** durumu ' nu seçerek Azure Active Directory Domain Services sistem durumunu görüntüleyin. Azure AD DS durumunun yeşil (çalışıyor) olduğundan ve eşitlemenin tamamlantığınızdan emin olun.

![Azure AD DS sistem durumu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="create-and-authorize-a-managed-identity"></a>Yönetilen kimlik oluşturma ve yetkilendirme

Etki alanı Hizmetleri işlemlerini basitleştirmek ve güvenli hale getirmek için *Kullanıcı tarafından atanan yönetilen kimlik* kullanabilirsiniz. **HDInsight etki alanı Hizmetleri katılımcısı** rolünü yönetilen kimliğe atadığınızda, etki alanı Hizmetleri işlemlerini okuyabilir, oluşturabilir, değiştirebilir ve silebilir. 

Kurumsal Güvenlik Paketi, OU 'Lar ve hizmet sorumluları oluşturma gibi bazı etki alanı Hizmetleri işlemleri, HDInsight için de gereklidir. Herhangi bir abonelikte Yönetilen kimlikler oluşturabilirsiniz. Genel olarak yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md). Yönetilen kimliklerin Azure HDInsight 'ta nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure HDInsight 'Ta Yönetilen kimlikler](../hdinsight-managed-identities.md).

ESP kümelerini ayarlamak için, henüz bir tane yoksa, Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Yönergeler için, bkz. [Azure Portal kullanarak Kullanıcı tarafından atanan yönetilen kimliğe rol oluşturma, listeleme, silme veya atama](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md). 

Ardından, **HDInsight etki alanı Hizmetleri katılımcısı** rolünü Azure AD DS **erişim denetimindeki** yönetilen kimliğe atayın. Bu rol atamasını yapmak için Azure AD DS yönetici ayrıcalıklarına sahip olmanız gerekir.

![Azure Active Directory Domain Services Access Control](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

**HDInsight etki alanı Hizmetleri katılımcısı** rolünü atamak, bu kimliğin Azure AD DS etki alanında etki alanı Hizmetleri işlemleri gerçekleştirmek için uygun (adına) erişimi olmasını sağlar. Bu işlemler OU 'Ları oluşturmayı ve silmeyi içerir.

Yönetilen kimlik oluşturulduktan ve doğru rol verildiğinde, Azure AD DS Yöneticisi bu yönetilen kimliği kimlerin kullanacağınızı ayarlayabilir. Önce, yönetici portalda yönetilen kimliği seçer ve ardından **genel bakış**altında **Access Control (IAM)** öğesini seçer. Ardından, sağ tarafta, yönetici, HDInsight ESP kümeleri oluşturmak isteyen kullanıcılara veya gruplara **yönetilen kimlik operatörü** rolünü atar. 

Örneğin, Azure AD DS Yöneticisi, aşağıdaki görüntüde gösterildiği gibi, bu rolü **sjmsi** tarafından yönetilen kimlik Için **Pazarlama ekibi** grubuna atayabilirler. Bu atama, kuruluştaki doğru kişilerin ESP kümeleri oluşturmak için yönetilen kimliği kullanmasını sağlar.

![HDInsight Yönetilen kimlik Işleci rol ataması](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)

## <a name="network-considerations"></a>Ağ ile ilgili önemli noktalar

> [!NOTE]  
> Azure AD DS Azure Resource Manager tabanlı bir sanal ağda dağıtılmalıdır. Klasik sanal ağlar Azure AD DS için desteklenmez. Daha fazla bilgi için bkz. [Azure Portal kullanarak Azure Active Directory Domain Services etkinleştirme](../../active-directory-domain-services/tutorial-create-instance-advanced.md#create-and-configure-the-virtual-network).

Azure AD DS etkinleştirdikten sonra, Active Directory sanal makinelerde (VM 'Ler) bir yerel etki alanı adı sistemi (DNS) sunucusu çalışır. Azure AD DS Sanal ağınızı bu özel DNS sunucularını kullanacak şekilde yapılandırın. Doğru IP adreslerini bulmak için, **Yönet** kategorisinde **Özellikler** ' i seçin ve **sanal ağdaki IP adresi**bölümüne bakın.

![Yerel DNS sunucularının IP adreslerini bulma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns1.png)

Azure AD DS sanal ağındaki DNS sunucularının yapılandırmasını, **Ayarlar** kategorisinde **DNS sunucuları** ' nı seçerek bu özel IP 'leri kullanacak şekilde değiştirin. Ardından **özel** seçeneğini belirleyin, metin kutusuna ilk IP adresini girin ve **Kaydet**' i seçin. Aynı adımları kullanarak daha fazla IP adresi ekleyin.

![Sanal ağ DNS yapılandırması güncelleştiriliyor](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

Aynı Azure sanal ağına hem Azure AD DS örneğini hem de HDInsight kümesini yerleştirmek daha kolay. Farklı sanal ağlar kullanmayı planlıyorsanız, etki alanı denetleyicisinin HDInsight VM 'lerine görebilmesi için bu sanal ağları eşdüzey yapmanız gerekir. Daha fazla bilgi için bkz. [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md). 

Sanal ağlar eşlendikten sonra, HDInsight sanal ağını özel bir DNS sunucusu kullanacak şekilde yapılandırın ve Azure AD DS özel IP 'lerini DNS sunucusu adresleri olarak girin. Her iki sanal ağ da aynı DNS sunucularını kullandıklarında, özel etki alanı adınız doğru IP 'ye çözümlenir ve HDInsight 'tan erişilecektir. Örneğin, etki alanı adınız `contoso.com`, bu adımdan sonra, `ping contoso.com` doğru Azure AD DS IP 'ye çözümlenmelidir.

![Eşlenmiş bir sanal ağ için özel DNS sunucuları yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

HDInsight alt ağınızda Ağ güvenlik grubu (NSG) kuralları kullanıyorsanız, hem gelen hem de giden trafik için [gerekli IP 'lere](../hdinsight-management-ip-addresses.md) izin vermeniz gerekir.

Ağınızın doğru şekilde ayarlanmış olup olmadığını test etmek için HDInsight sanal ağına/alt ağına bir Windows sanal makinesi ekleyin ve etki alanı adına ping gönderin. (Bir IP 'ye çözümlenmelidir.) Azure AD DS etki alanına erişmek için **Ldp. exe** ' yi çalıştırın. Ardından, istemci ve sunucu arasında tüm gerekli RPC çağrılarının başarılı olduğunu onaylamak için bu Windows VM 'sini etki alanına katın. 

Ayrıca, depolama hesabınıza veya kullanabileceğiniz herhangi bir dış veritabanına (örneğin, dış Hive meta veri deposu veya Ranger DB) ağ erişimini onaylamak için **nslookup** ' ı da kullanabilirsiniz. Bir NSG Azure AD DS güvenliğinin sağlanmasına yardımcı olduysa, Azure AD DS alt ağının NSG kurallarında [gerekli tüm bağlantı noktalarına](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) izin verildiğinden emin olun. Bu Windows VM 'ye katılan etki alanı başarılı olursa, sonraki adıma devam edebilir ve ESP kümeleri oluşturabilirsiniz.

## <a name="create-an-hdinsight-cluster-with-esp"></a>ESP ile HDInsight kümesi oluşturma

Önceki adımları doğru ayarladıktan sonra, bir sonraki adım, HDInsight kümesini ESP etkin olarak oluşturmaktır. Bir HDInsight kümesi oluşturduğunuzda, **güvenlik + ağ** sekmesinde kurumsal güvenlik paketi etkinleştirebilirsiniz. Dağıtım için bir Azure Resource Manager şablonu kullanmayı tercih ediyorsanız, Portal deneyimini bir kez daha kullanın ve daha sonra yeniden kullanmak üzere **gözden geçir + oluştur** sayfasında önceden doldurulmuş şablonu indirin. 

Ayrıca, küme oluşturma sırasında [HDıNSIGHT kimlik Aracısı](identity-broker.md) özelliğini de etkinleştirebilirsiniz. KIMLIK Aracısı özelliği, Multi-Factor Authentication kullanarak ve Azure AD DS parola karmaları gerekmeden gerekli Kerberos biletlerini alarak, ambarı 'nda oturum açmanızı sağlar.

> [!NOTE]  
> ESP küme adlarının ilk altı karakteri ortamınızda benzersiz olmalıdır. Örneğin, farklı sanal ağlarda birden çok ESP kümeniz varsa, küme adlarında ilk altı karakterin benzersiz olmasını sağlayan bir adlandırma kuralı seçin.

![Azure HDInsight için etki alanı doğrulama Kurumsal Güvenlik Paketi](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp.png)

ESP 'yi etkinleştirdikten sonra, Azure AD DS ile ilgili ortak yapılandırma hataları otomatik olarak algılanır ve onaylanır. Bu hataları düzelttikten sonra, bir sonraki adımla devam edebilirsiniz.

![Azure HDInsight Kurumsal Güvenlik Paketi etki alanı doğrulaması başarısız oldu](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp-error.png)

ESP ile bir HDInsight kümesi oluşturduğunuzda, aşağıdaki parametreleri sağlamanız gerekir:

- **Küme Yöneticisi kullanıcısı**: eşitlenmiş Azure AD DS örneğinden kümeniz için bir yönetici seçin. Bu etki alanı hesabı zaten eşitlenmiş ve Azure AD DS kullanılabilir olmalıdır.

- **Küme erişim grupları**: kullanıcılarını eşitlemek istediğiniz ve kümeye erişebilen güvenlik grupları Azure AD DS 'da kullanılabilir olmalıdır. HiveUsers grubu bir örnektir. Daha fazla bilgi için bkz. [Grup oluşturma ve Azure Active Directory üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **LDAPS URL 'si**: bir örnek `ldaps://contoso.com:636`.

Oluşturduğunuz yönetilen kimlik, yeni bir küme oluştururken, **Kullanıcı tarafından atanan yönetilen kimlik** listesinden seçilebilir.

![Azure HDInsight ESP Active Directory Domain Services yönetilen kimliği](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-identity.png)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [HDInsight kümeleri için Apache Hive ILKELERINI ESP Ile yapılandırma](apache-domain-joined-run-hive.md).
* ESP ile HDInsight kümelerine bağlanmak üzere SSH kullanmak için bkz. [Linux, Unix veya OS X 'Ten HDInsight 'Ta Linux tabanlı Apache Hadoop Ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight).
