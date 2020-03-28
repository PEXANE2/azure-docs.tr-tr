---
title: Öğretici - Azure Active Directory Etki Alanı Hizmetleri için LDAPS'yi yapılandırın | Microsoft Dokümanlar
description: Bu eğitimde, Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanı için güvenli hafif dizin erişim protokolü (LDAPS) yapılandırmayı öğrenirsiniz.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 10/30/2019
ms.author: iainfou
ms.openlocfilehash: 6db2c907abc495ca3c88e1e73e885043a8f19997
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79481543"
---
# <a name="tutorial-configure-secure-ldap-for-an-azure-active-directory-domain-services-managed-domain"></a>Öğretici: Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen etki alanı için güvenli LDAP'yi yapılandırma

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanınızla iletişim kurmak için Hafif Dizin Erişim Protokolü (LDAP) kullanılır. Varsayılan olarak, LDAP trafiği şifrelenmez ve bu da birçok ortam için bir güvenlik sorunudur. Azure AD DS ile yönetilen etki alanını güvenli Hafif Dizin Erişim Protokolü 'nü (LDAPS) kullanacak şekilde yapılandırabilirsiniz. Güvenli LDAP kullandığınızda, trafik şifrelenir. Secure LDAP, Güvenli Soketler Katmanı (SSL) / Taşıma Katmanı Güvenliği (TLS) üzerinden LDAP olarak da bilinir.

Bu öğretici, Azure AD DS yönetilen bir etki alanı için LDAPS'yi nasıl yapılandırabileceğinizi gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure AD DS ile kullanılmak üzere dijital sertifika oluşturma
> * Azure AD DS için güvenli LDAP'yi etkinleştirme
> * Genel internet üzerinden kullanım için güvenli LDAP yapılandırma
> * Azure AD DS yönetilen bir etki alanı için güvenli LDAP'yi bağlama ve test edin

Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.][create-azure-ad-ds-instance]
* *LDP.exe* aracı bilgisayarınızda yüklü.
    * Gerekirse, *Active Directory Domain Services ve LDAP*için Uzak Sunucu Yönetim Araçlarını [(RSAT) yükleyin.][rsat]

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu öğreticide, Azure portalını kullanarak Azure AD DS yönetilen etki alanı için güvenli LDAP'yi yapılandırmış olursunuz. Başlamak için önce [Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-a-certificate-for-secure-ldap"></a>Güvenli LDAP için sertifika oluşturma

Güvenli LDAP kullanmak için, iletişimi şifrelemek için dijital bir sertifika kullanılır. Bu dijital sertifika Azure AD DS yönetilen etki alanınıza uygulanır ve *LDP.exe* gibi araçların verileri sorgularken güvenli şifreli iletişim kullanmasına olanak tanır. Yönetilen etki alanına güvenli LDAP erişimi için sertifika oluşturmanın iki yolu vardır:

* Bir kamu sertifika yetkilisi (CA) veya bir kuruluş CA'dan alınan sertifika.
    * Kuruluşunuz ortak bir CA'dan sertifika alıyorsa, bu ortak CA'dan güvenli LDAP sertifikasını alın. Kuruluşunuzdaki bir kurumsal CA kullanıyorsanız, kurumsal CA'dan güvenli LDAP sertifikası alın.
    * Ortak CA yalnızca Azure AD DS yönetilen etki alanınızla özel bir DNS adı kullandığınızda çalışır. Yönetilen etki alanınızın DNS etki alanı adı *.onmicrosoft.com*ile biterse, bu varsayılan etki alanıyla bağlantıyı güvence altına almak için dijital sertifika oluşturamazsınız. Microsoft *.onmicrosoft.com* etki alanının sahibidir, bu nedenle ortak bir CA sertifika vermez. Bu senaryoda, kendi imzalı bir sertifika oluşturun ve güvenli LDAP yapılandırmak için kullanın.
* Kendi oluşturduğunuz kendi imzalı sertifika.
    * Bu yaklaşım, test amaçları için iyidir ve bu öğreticinin gösterdiği şeydir.

İstediğiniz veya oluşturduğunuz sertifika aşağıdaki gereksinimleri karşılamalıdır. Yönetilen etki alanınız, geçersiz bir sertifikayla güvenli LDAP'yi etkinleştiriniz durumunda sorunlarla karşılaşır:

* **Güvenilir veren -** Sertifika, güvenli LDAP kullanarak yönetilen etki alanına bağlanan bilgisayarlar tarafından güvenilen bir yetkili tarafından düzenlenmelidir. Bu yetki, bu bilgisayarlar tarafından güvenilen ortak bir CA veya Enterprise CA olabilir.
* **Ömür Boyu** - Sertifika en az 3-6 ay boyunca geçerli olmalıdır. Sertifikanın süresi dolduğunda yönetilen etki alanınıza güvenli LDAP erişimi kesintiye uğrar.
* **Konu adı** - Sertifikadaki konu adı yönetilen etki alanınız olmalıdır. Örneğin, etki alanınızın adı *aaddscontoso.com*ise, sertifikanın özadı **.aaddscontoso.com*olmalıdır.
    * Güvenli LDAP'nin Azure AD Etki Alanı Hizmetleri ile düzgün çalıştığından emin olmak için, Sertifikanın DNS adı veya özne alternatif adı joker karakter sertifikası olmalıdır. Etki Alanı Denetleyicileri rasgele adlar kullanır ve hizmetin kullanılabilir kalmasını sağlamak için kaldırılabilir veya eklenebilir.
* **Anahtar kullanımı** - Sertifika dijital *imzalar* ve *anahtar şifreleme*için yapılandırılmalıdır.
* **Sertifika amacı** - Sertifika TLS sunucu kimlik doğrulaması için geçerli olmalıdır.

OpenSSL, Keytool, MakeCert, [New-SelfSignedCertificate][New-SelfSignedCertificate] cmdlet vb. gibi kendi imzalı sertifika oluşturmak için çeşitli araçlar mevcuttur. Bu öğreticide, [Yeni Self-SelfSignedCertificate][New-SelfSignedCertificate] cmdlet kullanarak güvenli LDAP için kendi imzalı bir sertifika oluşturalım. **PowerShell** penceresini Yönetici olarak açın ve aşağıdaki komutları çalıştırın. *$dnsName* değişkenini, yönetilen etki alanınız tarafından kullanılan DNS adı ile değiştirme *aaddscontoso.com:*

```powershell
# Define your own DNS name used by your Azure AD DS managed domain
$dnsName="aaddscontoso.com"

# Get the current date to set a one-year expiration
$lifetime=Get-Date

# Create a self-signed certificate for use with Azure AD DS
New-SelfSignedCertificate -Subject *.$dnsName `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.$dnsName, $dnsName
```

Aşağıdaki örnek çıktı, sertifikanın başarıyla oluşturulduğunu ve yerel sertifika deposunda depolandığını gösterir (*LocalMachine\MY):*

```output
PS C:\WINDOWS\system32> New-SelfSignedCertificate -Subject *.$dnsName `
>>   -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
>>   -Type SSLServerAuthentication -DnsName *.$dnsName, $dnsName.com

   PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\MY

Thumbprint                                Subject
----------                                -------
959BD1531A1E674EB09E13BD8534B2C76A45B3E6  CN=aaddscontoso.com
```

## <a name="understand-and-export-required-certificates"></a>Gerekli sertifikaları anlama ve dışa aktarma

Güvenli LDAP kullanmak için ağ trafiği ortak anahtar altyapısı (PKI) kullanılarak şifrelenir.

* Azure AD DS yönetilen etki alanına **özel** bir anahtar uygulanır.
    * Bu özel anahtar, güvenli LDAP trafiğinin *şifresini çözmek* için kullanılır. Özel anahtar yalnızca Azure AD DS yönetilen etki alanına uygulanmalı ve istemci bilgisayarlara yaygın olarak dağıtılmamalıdır.
    * Özel anahtarı içeren bir sertifika. * PFX* dosya biçimi.
* **Ortak** anahtar istemci bilgisayarlara uygulanır.
    * Bu ortak anahtar, güvenli LDAP trafiğini *şifrelemek* için kullanılır. Ortak anahtar istemci bilgisayarlara dağıtılabilir.
    * Özel anahtarı olmayan sertifikalar *. CER* dosya biçimi.

Bu iki anahtar, *özel* ve *ortak* anahtarlar, yalnızca uygun bilgisayarların birbirleriyle başarılı bir şekilde iletişim kurabileceğinden emin olun. Ortak bir CA veya kurumsal CA kullanıyorsanız, size özel anahtarı içeren ve Azure AD DS yönetilen bir etki alanına uygulanabilen bir sertifika verilir. Ortak anahtar zaten bilinmeli ve istemci bilgisayarlar tarafından güvenilen. Bu öğreticide, özel anahtarla kendi imzalı bir sertifika oluşturdunuz, bu nedenle uygun özel ve ortak bileşenleri dışa aktarmanız gerekir.

### <a name="export-a-certificate-for-azure-ad-ds"></a>Azure AD DS için sertifika verme

Azure AD DS yönetilen etki alanınızla önceki adımda oluşturulan dijital sertifikayı kullanmadan önce, sertifikayı bir *. *Özel anahtarı içeren PFX sertifika dosyası.

1. *Çalıştır* iletişim kutusunu açmak için **Windows** ve **R** tuşlarını seçin.
1. *Çalıştır* iletişim kutusuna **mmc** girerek Microsoft Yönetim Konsolu'nu (MMC) açın ve **ardından Tamam'ı**seçin.
1. Kullanıcı **Hesabı Denetimi** isteminde, MMC'yi yönetici olarak başlatmak için **Evet'i** tıklatın.
1. **Dosya** menüsünden **Ekle/Kaldır'ı tıklatın...**
1. **Sertifikalar'a tutturma** sihirbazında **Bilgisayar hesabı'nı**seçin ve **ardından İleri'yi**seçin.
1. Bilgisayarı **Seç** sayfasında **Yerel bilgisayarı seçin: (bu konsolun üzerinde çalışan bilgisayar)** ve **Ardından Bitir'i**seçin.
1. Eklenti **veya Kaldır Girişler** iletişim kutusunda, sertifikaları MMC'ye eklemek için **Tamam'ı** tıklatın.
1. MMC penceresinde Konsol **Kökünü**genişletin. **Sertifikalar 'ı (Yerel Bilgisayar)** seçin, ardından **Kişisel** düğümü genişletin ve ardından **Sertifika** düğümü.

    ![Microsoft Yönetim Konsolu'ndaki kişisel sertifika mağazasını açın](./media/tutorial-configure-ldaps/open-personal-store.png)

1. Önceki adımda oluşturulan kendi imzalı *sertifika, aaddscontoso.com*gibi gösterilir. Bu sertifikayı sağ seçin, ardından **Tüm Görevler > Dışa Aktarma'yı seçin...**

    ![Microsoft Yönetim Konsolunda dışa aktarma sertifikası](./media/tutorial-configure-ldaps/export-cert.png)

1. Sertifika **Dışa Aktarma Sihirbazı'nda** **İleri'yi**seçin.
1. Sertifikaiçin özel anahtar dışa aktarılmalıdır. Özel anahtar dışa aktarılan sertifikaya dahil edilmezse, yönetilen etki alanınız için güvenli LDAP'yi etkinleştirme eylemi başarısız olur.

    Özel **Anahtarı Dışa Aktar** sayfasında **Evet'i seçin, özel anahtarı dışa aktarın**ve **Ardından 'yi**seçin.
1. Azure AD DS yönetilen etki alanları yalnızca *. *Özel anahtarı içeren PFX sertifika dosya biçimi. Sertifikayı " olarak dışa aktarmayın. * Özel* anahtar olmadan CER sertifika dosya biçimi.

    **Dışa Aktarma Dosya Biçimi** **sayfasında, Kişisel Bilgi Alışverişi - PKCS #12 (. PFX)** dışa aktarılan sertifika için dosya biçimi olarak. *Mümkünse tüm sertifikaları sertifika yoluna ekle*kutusunu işaretleyin:

    ![Sertifikayı PKCS 12'de dışa aktarma seçeneğini seçin (. PFX) dosya biçimi](./media/tutorial-configure-ldaps/export-cert-to-pfx.png)

1. Bu sertifika verilerin şifresini çözmek için kullanıldığından, erişimi dikkatle denetlemeniz gerekir. Sertifikanın kullanımını korumak için parola kullanılabilir. Doğru parola olmadan, sertifika bir hizmete uygulanamaz.

    **Güvenlik** sayfasında, 'yi korumak için **Parola** seçeneğini *seçin. PFX* sertifika dosyası. Parola girin ve onaylayın, ardından **İleri'yi**seçin. Bu parola, Azure AD DS yönetilen etki alanınız için güvenli LDAP'yi etkinleştirmek için bir sonraki bölümde kullanılır.
1. **Dışa Aktar** sayfasında, *C:\Users\accountname\azure-ad-ds.pfx*gibi sertifikayı dışa aktarmak istediğiniz dosya adını ve konumu belirtin. Parolanın ve 'nin konumunun notunu *tutun. PFX* dosyası olarak bu bilgiler sonraki adımlarda gerekli olacaktır.
1. İnceleme sayfasında, sertifikayı bir 'e aktarmak için Bitir'i'yi **Finish** *seçin. PFX* sertifika dosyası. Sertifika başarıyla dışa aktarıldığında bir onay iletişim kutusu görüntülenir.
1. Aşağıdaki bölümde kullanmak üzere MMC'yi açık bırakın.

### <a name="export-a-certificate-for-client-computers"></a>İstemci bilgisayarlar için sertifika verme

İstemci bilgisayarlar, LDAPS kullanarak yönetilen etki alanına başarılı bir şekilde bağlanabilmek için güvenli LDAP sertifikasının verenine güvenmelidir. İstemci bilgisayarların Azure AD DS tarafından şifresi çözülen verileri başarıyla şifrelemek için bir sertifikaya ihtiyacı vardır. Ortak bir CA kullanıyorsanız, bilgisayar bu sertifikayı verenlere otomatik olarak güvenmeli ve karşılık gelen bir sertifikaya sahip olmalıdır. Bu öğreticide kendi imzalı bir sertifika kullanın ve önceki adımda özel anahtarı içeren bir sertifika oluşturdunuz. Şimdi dışa aktaralım ve ardından kendi imzalı sertifikayı istemci bilgisayardaki güvenilir sertifika deposuna yükleyelim:

1. *Kişisel > Sertifikaları deposu> Sertifikalar* için MMC'ye (Yerel Bilgisayar) geri dön. Önceki adımda oluşturulan kendi imzalı *sertifika, aaddscontoso.com*gibi gösterilir. Bu sertifikayı sağ seçin, ardından **Tüm Görevler > Dışa Aktarma'yı seçin...**
1. Sertifika **Dışa Aktarma Sihirbazı'nda** **İleri'yi**seçin.
1. Müşteriler için özel anahtara ihtiyacınız olmadığından, **Dışa** Aktar Özel Anahtar sayfasında **Hayır'ı seçin, özel anahtarı dışa aktarmayın,** sonra **İleri'yi**seçin.
1. Dosya **Biçimini Dışa Aktar** **sayfasında, Kodlanmış X.509 (. CER)** dışa aktarılan sertifika için dosya biçimi olarak:

    ![Sertifikayı Base-64 kodlanmış X.509'da dışa aktarma seçeneğini seçin (. CER) dosya biçimi](./media/tutorial-configure-ldaps/export-cert-to-cer-file.png)

1. **Dışa Aktar sayfasında,** *C:\Users\accountname\azure-ad-ds-client.cer*gibi sertifikayı dışa aktarmak istediğiniz dosya adını ve konumu belirtin.
1. İnceleme sayfasında, sertifikayı bir 'e aktarmak için Bitir'i'yi **Finish** *seçin. CER* sertifika dosyası. Sertifika başarıyla dışa aktarıldığında bir onay iletişim kutusu görüntülenir.

*Şey. CER* sertifika dosyası artık Azure AD DS yönetilen etki alanına güvenli LDAP bağlantısına güvenmesi gereken istemci bilgisayarlara dağıtılabilir. Sertifikayı yerel bilgisayara yükleyelim.

1. Dosya Gezgini'ni açın ve 'yi kaydettiğiniz konuma göz *atın. Cer* sertifika dosyası, *C:\Users\accountname\azure-ad-ds-client.cer*gibi.
1. Sağ *seçin. CER* sertifika dosyası, ardından **Sertifika Yı yükle'yi**seçin.
1. Sertifika **Alma Sihirbazı'nda,** sertifikayı *Yerel makinede*depolamayı seçin, ardından **Sonraki'ni**seçin:

    ![Sertifikayı yerel makine deposuna alma seçeneğini seçin](./media/tutorial-configure-ldaps/import-cer-file.png)

1. İstendiğinde, bilgisayarın değişiklik yapmasına izin vermek için **Evet'i** seçin.
1. Sertifika **türüne göre sertifika deposunu otomatik olarak seçin,** ardından **İleri'yi**seçin.
1. İnceleme sayfasında, 'yi almak için Bitir'i'yi **Finish** *seçin. CER* sertifikası. sertifika başarıyla içe aktarıldığında bir onay iletişim kutusu görüntülenir.

## <a name="enable-secure-ldap-for-azure-ad-ds"></a>Azure AD DS için güvenli LDAP'yi etkinleştirme

Özel anahtarı içeren oluşturulan ve dışa aktarılan dijital sertifika ve bağlantıya güvenecek istemci bilgisayar kümesi yle, artık Azure AD DS yönetilen etki alanınızda güvenli LDAP'yi etkinleştirin. Azure AD DS yönetilen bir etki alanında güvenli LDAP'yi etkinleştirmek için aşağıdaki yapılandırma adımlarını gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com) **Arama kaynakları** kutusuna etki *alanı hizmetlerini* girin. Arama **sonucundan Azure AD Etki Alanı Hizmetlerini** seçin.

    ![Azure portalında Azure AD DS yönetilen etki alanınızı arayın ve seçin](./media/tutorial-configure-ldaps/search-for-domain-services.png)

1. yönetilen etki alanınızı seçin( *örneğin aaddscontoso.com.*
1. Azure AD DS penceresinin sol **tarafında, Güvenli LDAP'yi**seçin.
1. Varsayılan olarak, yönetilen etki alanınıza güvenli LDAP erişimi devre dışı bırakılır. **Etkinle'ye Güvenli LDAP'yi** Geçiş . **Enable**
1. Yönetilen etki alanınıza Internet üzerinden güvenli LDAP erişimi varsayılan olarak devre dışı bırakılır. Kamu güvenli LDAP erişimini etkinleştirdiğinizde, etki alanınız internet üzerinden parola kaba kuvvet saldırılarına karşı hassastır. Bir sonraki adımda, bir ağ güvenlik grubu yalnızca gerekli kaynak IP adresi aralıklarına erişimi kilitlemek için yapılandırılır.

    Geçiş **Etkinleştirmek**için **internet üzerinden güvenli LDAP erişimine izin** ver.

1. yanındaki klasör simgesini **seçin. Güvenli LDAP sertifikası ile PFX dosyası.** ' nin yoluna göz *atın. PFX* dosyası, ardından özel anahtarı içeren önceki bir adımda oluşturulan sertifikayı seçin.

    Sertifika gereksinimleriyle ilgili önceki bölümde belirtildiği gibi, varsayılan *.onmicrosoft.com* etki alanına sahip ortak bir CA'dan bir sertifika kullanamazsınız. Microsoft *.onmicrosoft.com* etki alanının sahibidir, bu nedenle ortak bir CA sertifika vermez. Sertifikanızın uygun biçimde olduğundan emin olun. Değilse, güvenli LDAP'yi etkinleştirdiğinizde Azure platformu sertifika doğrulama hataları oluşturur.

1. **Şifresini çözmek için Parola'yı girin. PFX dosyası,** sertifika nın bir ' ye dışa aktarıldığında önceki bir adımda ayarlanmış * PFX* dosyası.
1. Güvenli LDAP'yi etkinleştirmek için **Kaydet'i** seçin.

    ![Azure portalında Azure AD DS yönetilen bir etki alanı için güvenli LDAP'yi etkinleştirme](./media/tutorial-configure-ldaps/enable-ldaps.png)

Yönetilen etki alanı için güvenli LDAP yapılandırıldığını belirten bir bildirim görüntülenir. Bu işlem tamamlanana kadar yönetilen etki alanının diğer ayarlarını değiştiremezsiniz.

Yönetilen etki alanınız için güvenli LDAP'yi etkinleştirmek birkaç dakika sürer. Sağladığınız güvenli LDAP sertifikası gerekli ölçütlere uymuyorsa, yönetilen etki alanı için güvenli LDAP'yi etkinleştirme eylemi başarısız olur. Hatanın bazı yaygın nedenleri, alan adı yanlışsa veya sertifikanın süresi yakında doluyorsa veya süresi dolmuşsa. Geçerli parametrelerle sertifikayı yeniden oluşturabilir, ardından bu güncelleştirilmiş sertifikayı kullanarak güvenli LDAP'yi etkinleştirebilirsiniz.

## <a name="lock-down-secure-ldap-access-over-the-internet"></a>Internet üzerinden güvenli LDAP erişimini kilitleme

Azure AD DS yönetilen etki alanınıza internet üzerinden güvenli LDAP erişimi sağladığınızda, bu bir güvenlik tehdidi oluşturur. Yönetilen etki alanına TCP bağlantı noktası 636'daki internetten ulaşılabilir. Yönetilen etki alanına erişimi ortamınız için bilinen belirli IP adresleriyle sınırlamak önerilir. Güvenli LDAP erişimi sınırlamak için bir Azure ağ güvenlik grubu kuralı kullanılabilir.

Belirli bir IP adresi kümesinden TCP bağlantı noktası 636 üzerinden gelen güvenli LDAP erişimine izin veren bir kural oluşturalım. Daha düşük önceliğe sahip varsayılan *Bir DenyAll* kuralı Internet'ten gelen diğer tüm trafik için geçerlidir, böylece yalnızca belirtilen adresler güvenli LDAP kullanarak Azure AD DS yönetilen etki alanınıza erişebilir.

1. Azure portalında, sol taraftaki gezinmede *Kaynak gruplarını* seçin.
1. *MyResourceGroup*gibi kaynak grubunu seçin, ardından *aaads-nsg*gibi ağ güvenlik grubunuzu seçin.
1. Varolan gelen ve giden güvenlik kurallarının listesi görüntülenir. Ağ güvenlik grubu pencerelerinin sol tarafında, **Gelen güvenlik kuralları> Ayarlar'ı**seçin.
1. **Ekle'yi**seçin, ardından *TCP* bağlantı noktasına *636'ya*izin verecek bir kural oluşturun. Daha iyi güvenlik için kaynağı *IP Adresleri* olarak seçin ve ardından kuruluşunuz için kendi geçerli IP adresinizi veya aralığınızı belirtin.

    | Ayar                           | Değer        |
    |-----------------------------------|--------------|
    | Kaynak                            | IP Adresleri |
    | Kaynak IP adresleri / CIDR aralıkları | Ortamınız için geçerli bir IP adresi veya aralığı |
    | Kaynak bağlantı noktası aralıkları                | *            |
    | Hedef                       | Herhangi biri          |
    | Hedef bağlantı noktası aralıkları           | 636          |
    | Protokol                          | TCP          |
    | Eylem                            | İzin Ver        |
    | Öncelik                          | 401          |
    | Adı                              | İzin Ver   |

1. Hazır olduğunuzda, kaydetmek ve kuralı uygulamak için **Ekle'yi** seçin.

    ![Internet üzerinden LDAPS erişimini güvence altına almak için bir ağ güvenliği grubu kuralı oluşturma](./media/tutorial-configure-ldaps/create-inbound-nsg-rule-for-ldaps.png)

## <a name="configure-dns-zone-for-external-access"></a>Dış erişim için DNS bölgesini yapılandırma

Internet üzerinden etkinleştirilmiş güvenli LDAP erişimiyle, istemci bilgisayarların yönetilen bu etki alanını bulabilmeleri için DNS bölgesini güncelleştirin. *Güvenli LDAP harici IP adresi,* Azure AD DS yönetilen etki alanınızın **Özellikleri** sekmesinde listelenir:

![Azure portalında Azure AD DS yönetilen etki alanınız için güvenli LDAP harici IP adresini görüntüleyin](./media/tutorial-configure-ldaps/ldaps-external-ip-address.png)

Bu harici IP adresini çözmek için harici DNS sağlayıcınızı *ldaps*gibi bir ana bilgisayar kaydı oluşturacak şekilde yapılandırın. Önce makinenizde yerel olarak test etmek için, Windows ana bilgisayarları dosyasında bir giriş oluşturabilirsiniz. Ana bilgisayar dosyasını yerel makinenizde başarıyla düzenlemek için *Notepad'i* yönetici olarak açın ve *ardından C:\Windows\System32\drivers\etc* dosyasını açın

Aşağıdaki örnek DNS girişi, harici DNS sağlayıcınızla veya yerel ana bilgisayarlar dosyasında, *40.121.19.239'un*harici IP adresine *ldaps.aaddscontoso.com* trafiğini çözer:

```
40.121.19.239    ldaps.aaddscontoso.com
```

## <a name="test-queries-to-the-managed-domain"></a>Yönetilen etki alanına sorgu test

Azure AD DS yönetilen etki alanınıza bağlanmak ve LDAP üzerinden arama yapmak için *LDP.exe* aracını kullanırsınız. Bu araç Uzak Sunucu Yönetim Araçları (RSAT) paketine dahildir. Daha fazla bilgi için uzaktan [sunucu yönetim araçlarını yükleyin'][rsat]e bakın.

1. *LDP.exe'yi* açın ve yönetilen etki alanına bağlanın. **Bağlantı'yı**seçin, ardından **Bağlan'ı seçin... seçeneğini belirleyin.**
1. yönetilen etki alanınızın güvenli LDAP DNS etki alanı adını *girin,* örneğin önceki adımda oluşturulan ldaps.aaddscontoso.com. Güvenli LDAP kullanmak **için,** *Port'u 636*olarak ayarlayın, ardından **SSL**için kutuyu işaretleyin.
1. Yönetilen etki alanına bağlanmak için **Tamam'ı** seçin.

Ardından, Azure AD DS yönetilen etki alanınıza bağlanın. Azure AD DS örneğiniz üzerinde NTLM şifre karma eşitlemedevre dışı bıraktıysanız, kullanıcılar (ve hizmet hesapları) LDAP basit bağlamalarını gerçekleştiremez. NTLM şifre karma senkronizasyonunu devre dışı bırakma hakkında daha fazla bilgi için [Azure AD DS yönetilen etki alanınızı güvenli][secure-domain]hale bakın.

1. **Bağlantı** menüsü seçeneğini seçin ve ardından **Bind... seçeneğini belirleyin.**
1. *Contosoadmin*gibi *AAD DC Yöneticileri* grubuna ait bir kullanıcı hesabının kimlik bilgilerini sağlayın. Kullanıcı hesabının parolasını girin, ardından *aaddscontoso.com*gibi etki alanınızı girin.
1. **Bind türü için,** *kimlik bilgileri ile Bind*seçeneğini seçin.
1. Azure AD DS yönetilen etki alanınıza bağlamak için **Tamam'ı** seçin.

Azure AD DS yönetilen etki alanınızda depolanan nesneleri görmek için:

1. **Görünüm** menüsü seçeneğini seçin ve ardından **Ağaç'ı**seçin.
1. *BaseDN* alanını boş bırakın, ardından **Tamam'ı**seçin.
1. *AADDC Kullanıcıları*gibi bir kapsayıcı seçin, ardından kapsayıcıyı sağ seçin ve **Ara'yı**seçin.
1. Önceden doldurulmuş alanları kümesini bırakın, ardından **Çalıştır'ı**seçin. Sorgunun sonuçları sağ pencerede gösterilir.

    ![LDP.exe'yi kullanarak Azure AD DS yönetilen etki alanınızdaki nesneleri arama](./media/tutorial-configure-ldaps/ldp-query.png)

Belirli bir kapsayıcıyı doğrudan sorgulamak **için, Ağacı > Görüntüle** menüsünden *OU=AADDC Users,DC=AADDSCONTOSO,DC=COM* veya *OU=AADDC Computers,DC=AADDSCONTOSO,DC=COM*gibi bir **BaseDN** belirtebilirsiniz. Sorguları biçimlendirme ve oluşturma hakkında daha fazla bilgi için [LDAP sorgu temellerine][ldap-query-basics]bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici için bağlantıyı test etmek için bilgisayarınızın yerel ana bilgisayarları dosyasına bir DNS girişi eklediyseniz, bu girişi kaldırın ve DNS bölgenize resmi bir kayıt ekleyin. Yerel ana bilgisayarlar dosyasından girişi kaldırmak için aşağıdaki adımları tamamlayın:

1. Yerel makinenizde, *yönetici* olarak Not Defteri'ni açın
1. *C:\Windows\System32\drivers\etc* dosyasına göz atın ve açın
1. Eklediğiniz kaydın satırını silme, örneğin`40.121.19.239    ldaps.aaddscontoso.com`

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD DS ile kullanılmak üzere dijital sertifika oluşturma
> * Azure AD DS için güvenli LDAP'yi etkinleştirme
> * Genel internet üzerinden kullanım için güvenli LDAP yapılandırma
> * Azure AD DS yönetilen bir etki alanı için güvenli LDAP'yi bağlama ve test edin

> [!div class="nextstepaction"]
> [Karma Azure REKLAM ortamı için parola karma eşitleme yapılandırma](tutorial-configure-password-hash-sync.md)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[secure-domain]: secure-your-domain.md

<!-- EXTERNAL LINKS -->
[rsat]: /windows-server/remote/remote-server-administration-tools
[ldap-query-basics]: /windows/desktop/ad/creating-a-query-filter
[New-SelfSignedCertificate]: /powershell/module/pkiclient/new-selfsignedcertificate
