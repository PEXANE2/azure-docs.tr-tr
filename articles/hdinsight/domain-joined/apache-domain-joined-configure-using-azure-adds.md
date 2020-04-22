---
title: Azure AD DS ile Kurumsal Güvenlik - Azure HDInsight
description: Azure Active Directory Domain Services'ı kullanarak bir HDInsight Kurumsal Güvenlik Paketi kümesini nasıl ayarlayıp yapılandırabileceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seodec18
ms.date: 04/17/2020
ms.openlocfilehash: c045378b6e69a9bb1b696d3390dadf84a50bd3b7
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687283"
---
# <a name="enterprise-security-package-configurations-with-azure-active-directory-domain-services-in-hdinsight"></a>HDInsight'ta Azure Active Directory Domain Services ile Kurumsal Güvenlik Paketi yapılandırmaları

Kurumsal Güvenlik Paketi (ESP) kümeleri Azure HDInsight kümelerinde çok kullanıcılı erişim sağlar. ESP'li HDInsight kümeleri bir etki alanına bağlanır. Bu bağlantı, etki alanı kullanıcılarının kümelerle kimlik doğrulaması yapmak ve büyük veri işleri çalıştırmak için etki alanı kimlik bilgilerini kullanmasına olanak tanır.

Bu makalede, Azure Active Directory Domain Services (Azure AD DS) kullanarak BIR HDInsight kümesini ESP ile nasıl yapılandırabileceğinizi öğreneceksiniz.

> [!NOTE]  
> ESP genellikle HDInsight 3.6 ve 4.0'da bu küme türleri için kullanılabilir: Apache Spark, Interactive, Hadoop ve HBase. Apache Kafka küme türü için ESP önizlemede yalnızca en iyi çaba desteği ile birliktedir. ESP GA tarihinden önce oluşturulan ESP kümeleri (1 Ekim 2018) desteklenmez.

## <a name="enable-azure-ad-ds"></a>Azure AD DS’yi etkinleştirme

> [!NOTE]  
> Azure AD DS'yi etkinleştirme ayrıcalıklarına yalnızca kiracı yöneticiler sahiptir. Küme depolama alanı Azure Veri Gölü Depolama Gen1 veya Gen2 ise, Yalnızca temel Kerberos kimlik doğrulaması kullanarak kümeye erişmesi gereken kullanıcılar için Azure Çok Faktörlü Kimlik Doğrulaması'nı devre dışı bırakmanız gerekir.
>
> Belirli *kullanıcılar* için Çok Faktörlü Kimlik Doğrulamayı yalnızca HDInsight kümesinin sanal ağının IP aralığına erişirken devre dışı bırakmaları için [güvenilir IP'leri](../../active-directory/authentication/howto-mfa-mfasettings.md#trusted-ips) veya [Koşullu Erişimi](../../active-directory/conditional-access/overview.md) kullanabilirsiniz. Koşullu Erişim kullanıyorsanız, ACTIVE Directory hizmeti bitiş noktasının HDInsight sanal ağında etkin olduğundan emin olun.
>
> Küme depolama alanı Azure Blob depolama alanıysa, Çok Faktörlü Kimlik Doğrulama'yı devre dışı bırakmayın.

ESP ile bir HDInsight kümesi oluşturmadan önce Azure AD DS'yi etkinleştirmek bir ön koşuldur. Daha fazla bilgi için Azure [portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini Etkinleştir'e](../../active-directory-domain-services/tutorial-create-instance.md)bakın.

Azure AD DS etkinleştirildiğinde, tüm kullanıcılar ve nesneler varsayılan olarak Azure Active Directory'den (Azure AD) Azure AD DS'ye eşitlenmeye başlar. Eşitleme işleminin uzunluğu Azure AD'deki nesne sayısına bağlıdır. Eşitleme yüz binlerce nesne için birkaç gün sürebilir.

HDInsight ile çalışmak için Azure AD DS ile kullandığınız alan adı 39 karakter veya daha az olmalıdır.

Yalnızca HDInsight kümelerine erişimi gereken grupları eşitlemeyi seçebilirsiniz. Yalnızca belirli grupları eşitleme bu seçeneği *kapsamlı eşitleme*denir. Yönergeler için bkz: [Azure AD'den yönetilen etki alanınıza kapsamlı eşitleme yapılandırma.](../../active-directory-domain-services/scoped-synchronization.md)

Güvenli LDAP'yi etkinleştirirken, alan adını konu adına koyun. Ve sertifikada konu alternatif adı. Alan adınız *contoso100.onmicrosoft.com*ise, sertifika konu adınızda ve konu alternatif adınızda tam adın bulunduğundan emin olun. Daha fazla bilgi için bkz: [Azure AD DS yönetilen etki alanı için güvenli LDAP'yi yapılandırın.](../../active-directory-domain-services/tutorial-configure-ldaps.md)

Aşağıdaki örnekte kendi imzalı bir sertifika oluşturulur. alan adı *contoso100.onmicrosoft.com* hem `Subject` (konu adı) hem `DnsName` de (özne alternatif adı) bulunmaktadır.

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
```

## <a name="check-azure-ad-ds-health-status"></a>Azure AD DS sistem durumu durumunu denetleme

**Yönet** kategorisinde **Sistem Durumu'nu** seçerek Azure Etkin Dizin Etki Alanı Hizmetlerinin sistem durumu durumunu görüntüleyin. Azure AD DS'nin durumunun yeşil (çalışıyor) ve eşitlemenin tamamlandığından emin olun.

![Azure AD DS sistem durumu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="create-and-authorize-a-managed-identity"></a>Yönetilen bir kimlik oluşturma ve yetkilendirme

Güvenli etki alanı hizmetleri işlemlerini basitleştirmek için *kullanıcı tarafından atanmış yönetilen bir kimlik* kullanın. **HDInsight Etki Alanı Hizmetleri Katılımcısı** rolünü yönetilen kimliğe atadığınızda, etki alanı hizmetleri işlemlerini okuyabilir, oluşturabilir, değiştirebilir ve silebilir.

HDInsight Kurumsal Güvenlik Paketi için, OS'ler ve hizmet ilkeleri oluşturma gibi belirli etki alanı hizmetleri işlemleri gereklidir. Herhangi bir abonelikte yönetilen kimlikler oluşturabilirsiniz. Genel olarak yönetilen kimlikler hakkında daha fazla bilgi için Azure [kaynakları için Yönetilen kimlikler'e](../../active-directory/managed-identities-azure-resources/overview.md)bakın. Azure HDInsight'ta yönetilen kimliklerin nasıl çalıştığı hakkında daha fazla bilgi için [Azure HDInsight'ta Yönetilen kimlikler'e](../hdinsight-managed-identities.md)bakın.

ESP kümeleri kurmak için, zaten bir tane yoksa, kullanıcı tarafından atanmış yönetilen bir kimlik oluşturun. Bkz. [`Create, list, delete, or assign a role to a user-assigned managed identity by using the Azure portal`](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

Ardından, Azure AD DS için **Access denetiminde** yönetilen kimliğe **HDInsight Etki Alanı Hizmetleri Katılımcısı** rolünü atayın. Bu rol atamasını yapmak için Azure AD DS yönetici ayrıcalıklarına ihtiyacınız vardır.

![Azure Active Directory Etki Alanı Hizmetleri Erişim denetimi](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

**HDInsight Etki Alanı Hizmetleri Katılımcısıfatı** rolünün atanması,`on behalf of`bu kimliğin Azure AD DS etki alanında etki alanı hizmetleri işlemleri yapmak için uygun erişime sahip olmasını sağlar. Bu işlemler, OSB oluşturma ve silme içerir.

Yönetilen kimliğe rol verildikten sonra, Azure AD DS yöneticisi bu rolü kimin kullandığını yönetir. İlk olarak, yönetici portalda yönetilen kimliği seçer. Ardından **Genel Bakış**altında Erişim **Denetimi 'ni (IAM)** seçer. Yönetici, Yönetilen **Kimlik İşlemi** rolünü ESP kümeleri oluşturmak isteyen kullanıcılara veya gruplara atar.

Örneğin, Azure AD DS yöneticisi bu rolü **sjmsi** yönetilen kimlik için **MarketingTeam** grubuna atayabilir. Aşağıdaki resimde bir örnek gösterilmiştir. Bu atama, kuruluştaki doğru kişilerin ESP kümeleri oluşturmak için yönetilen kimliği kullanabilmesini sağlar.

![HDInsight Yönetilen Kimlik Operatörü Rol Atama](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)

## <a name="network-considerations"></a>Ağ konuları

> [!NOTE]  
> Azure AD DS'nin Azure Kaynak Yöneticisi tabanlı bir sanal ağda dağıtılması gerekir. Azure AD DS için klasik sanal ağlar desteklenmez. Daha fazla bilgi için Azure [portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini Etkinleştir'e](../../active-directory-domain-services/tutorial-create-instance-advanced.md#create-and-configure-the-virtual-network)bakın.

Azure AD DS'yi etkinleştirin. Ardından, Active Directory sanal makinelerde (VM) yerel bir Etki Alanı Adı Sistemi (DNS) sunucusu çalışır. Azure AD DS sanal ağınızı bu özel DNS sunucularını kullanacak şekilde yapılandırın. Doğru IP adreslerini bulmak için **Yönet** kategorisindeki **Özellikleri** seçin ve **SANAL AĞDA IP ADRESİnİn ALTINA**BAKıN.

![Yerel DNS sunucuları için IP adreslerini bulma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns1.png)

Azure AD DS sanal ağındaki DNS sunucularının yapılandırmasını değiştirin. Bu özel IP'leri kullanmak için **Ayarlar** kategorisinde **DNS sunucularını** seçin. Ardından **Özel** seçeneğini seçin, metin kutusuna ilk IP adresini girin ve **Kaydet'i**seçin. Aynı adımları kullanarak daha fazla IP adresi ekleyin.

![Sanal ağ DNS yapılandırmasını güncelleştirme](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

Hem Azure AD DS örneğini hem de HDInsight kümesini aynı Azure sanal ağına yerleştirmek daha kolaydır. Farklı sanal ağlar kullanmayı planlıyorsanız, etki alanı denetleyicisinin HDInsight VM'leri tarafından görülebilmesi için bu sanal ağlara bakmanız gerekir. Daha fazla bilgi için [Sanal ağ eşleme'ye](../../virtual-network/virtual-network-peering-overview.md)bakın.

Sanal ağlar eşlendikten sonra, HDInsight sanal ağını özel bir DNS sunucusu kullanacak şekilde yapılandırın. Ayrıca, DNS sunucu adresleri olarak Azure AD DS özel IP'lerini girin. Her iki sanal ağ da aynı DNS sunucularını kullandığında, özel alan adınız doğru IP'ye çözülür ve HDInsight'tan erişilebilir. Örneğin, etki alanınız `contoso.com`bu adımdan sonra `ping contoso.com` doğru Azure AD DS IP'sine çözülmelidir.

![Eşli bir sanal ağ için özel DNS sunucularını yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

HDInsight alt netinizde ağ güvenliği grubu (NSG) kurallarını kullanıyorsanız, hem gelen hem de giden trafik için [gerekli IP'lere](../hdinsight-management-ip-addresses.md) izin vermelisiniz.

Ağ kurulumunuzu test etmek için, BIR Windows VM'ye HDInsight sanal ağına/alt ağına katılın ve etki alanı adını pingleyin. (Bir IP'ye çözülmelidir.) Azure AD DS etki alanına erişmek için **ldp.exe'yi** çalıştırın. Ardından, istemci ve sunucu arasında gerekli tüm RPC çağrılarının başarılı olduğunu doğrulamak için bu Windows VM'yi etki alanına katılın.

Depolama hesabınıza ağ erişimini onaylamak için **nslookup'u** kullanın. Veya kullanabileceğiniz herhangi bir harici veritabanı (örneğin, harici Hive metastore veya Ranger DB). Bir NSG Azure AD DS'yi güvence altına alıyorsa, [Gerekli bağlantı noktalarına](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) Azure AD DS alt netinin NSG kurallarında izin verildiğinden emin olun. Bu Windows VM'nin etki alanına katılması başarılı olursa, bir sonraki adıma devam edebilir ve ESP kümeleri oluşturabilirsiniz.

## <a name="create-an-hdinsight-cluster-with-esp"></a>ESP ile HDInsight kümesi oluşturma

Önceki adımları doğru ayarladıktan sonra, bir sonraki adım ESP etkin leştirilmiş HDInsight kümesini oluşturmaktır. Bir HDInsight kümesi oluşturduğunuzda, Güvenlik + **ağ** sekmesinde Kurumsal Güvenlik Paketi'ni etkinleştirebilirsiniz. Dağıtım için bir Azure Kaynak Yöneticisi şablonu için portal deneyimini bir kez kullanın. Ardından, gözden **geçirme + gelecekteki** yeniden kullanım için oluştur sayfasında önceden doldurulmuş şablonu indirin.

Küme oluşturma sırasında [HDInsight ID Aracısı](identity-broker.md) özelliğini de etkinleştirebilirsiniz. ID Broker özelliği, Multi-Factor Authentication kullanarak Ambari'de oturum açmanızı ve Azure AD DS'de parola kalıplarına gerek kalmadan gerekli Kerberos biletlerini almanızı sağlar.

> [!NOTE]  
> ESP küme adlarının ilk altı karakteri ortamınızda benzersiz olmalıdır. Örneğin, farklı sanal ağlarda birden çok ESP kümeniz varsa, küme adlarında ilk altı karakterin benzersiz olmasını sağlayan bir adlandırma kuralı seçin.

![Azure HDInsight Kurumsal Güvenlik Paketi için alan adı doğrulaması](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp.png)

ESP'yi etkinleştirdikten sonra, Azure AD DS ile ilgili yaygın yanlış yapılandırmalar otomatik olarak algılanır ve doğrulanır. Bu hataları düzelttinden sonra, bir sonraki adımla devam edebilirsiniz.

![Azure HDInsight Kurumsal Güvenlik Paketi başarısız etki alanı doğrulaması](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp-error.png)

ESP ile bir HDInsight kümesi oluşturduğunuzda, aşağıdaki parametreleri sağlamanız gerekir:

* **Cluster admin kullanıcısı**: Senkronize Azure AD DS örneğiniz den kümeniz için bir yönetici seçin. Bu etki alanı hesabı zaten eşitlenmiş ve Azure AD DS'de kullanılabilmelidir.

* **Küme erişim grupları**: Kullanıcıları eşitlemek istediğiniz ve kümeye erişimi olan güvenlik grupları Azure AD DS'de kullanılabilir olmalıdır. Bir örnek HiveUsers grubudur. Daha fazla bilgi için [bkz.](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

* **LDAPS URL**: `ldaps://contoso.com:636`Bir örnek .

Oluşturduğunuz yönetilen kimlik, yeni bir küme oluştururken **Kullanıcı tarafından atanan yönetilen kimlik** açılır listesinden seçilebilir.

![Azure HDInsight ESP Active Directory Etki Alanı Hizmetleri yönetilen kimlik](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-identity.png).

## <a name="next-steps"></a>Sonraki adımlar

* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak [için, ESP ile HDInsight kümeleri için Apache Hive ilkelerini yapılandırın' a](apache-domain-joined-run-hive.md)bakın.
* ESP ile HDInsight kümelerine bağlanmak için SSH'yi kullanmak için [Linux, Unix veya OS X'ten HDInsight'ta Linux tabanlı Apache Hadoop ile SSH'yi kullanın'a](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight)bakın.
