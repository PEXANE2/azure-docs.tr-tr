---
title: Azure HDInsight'ta kimlik doğrulama sorunları
description: Azure HDInsight'ta kimlik doğrulama sorunları
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 11/08/2019
ms.openlocfilehash: 26eec9cdd327ceb51e72deb1d6f40d585ce368fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75896132"
---
# <a name="authentication-issues-in-azure-hdinsight"></a>Azure HDInsight'ta kimlik doğrulama sorunları

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

Etki alanı kullanıcıları HDI Ağ Geçidi üzerinden küme hizmetlerinde oturum açtıklarında (Apache Ambari portalında oturum açma gibi) Azure Veri Gölü (Gen1 veya Gen2) tarafından desteklenen güvenli kümelerde HDI Ağ Geçidi, önce Azure Active Directory'den (Azure AD) bir OAuth belirteci elde etmeye çalışacaktır. ve ardından Azure AD DS'den Kerberos bileti alın. Kimlik doğrulama bu aşamalardan herhangi birinde başarısız olabilir. Bu makalede, bu sorunlardan bazılarını hata ayıklama amaçlanmaktadır.

Kimlik doğrulaması başarısız olduğunda, kimlik bilgileri için istenir. Bu iletişim kutusunu iptal ederseniz, hata iletisi yazdırılır. Sık karşılaşılan hata iletilerinden bazıları şunlardır:

## <a name="invalid_grant-or-unauthorized_client-50126"></a>invalid_grant veya unauthorized_client, 50126

### <a name="issue"></a>Sorun

Hata kodu 50126 olan federe kullanıcılar için oturum açma başarısız olur (bulut kullanıcıları için başarılı oturum açın). Hata iletisi benzer:

```
Reason: Bad Request, Detailed Response: {"error":"invalid_grant","error_description":"AADSTS70002: Error validating credentials. AADSTS50126: Invalid username or password\r\nTrace ID: 09cc9b95-4354-46b7-91f1-efd92665ae00\r\n Correlation ID: 4209bedf-f195-4486-b486-95a15b70fbe4\r\nTimestamp: 2019-01-28 17:49:58Z","error_codes":[70002,50126], "timestamp":"2019-01-28 17:49:58Z","trace_id":"09cc9b95-4354-46b7-91f1-efd92665ae00","correlation_id":"4209bedf-f195-4486-b486-95a15b70fbe4"}
```

### <a name="cause"></a>Nedeni

Azure AD hata kodu 50126, ilkenin `AllowCloudPasswordValidation` kiracı tarafından ayarlanmadığı anlamına gelir.

### <a name="resolution"></a>Çözüm

Azure AD kiracısının Şirket Yöneticisi, Azure AD'nin ADFS destekli kullanıcılar için parola hashes'lerini kullanmasını etkinleştirmelidir.  HdInsight'ta kurumsal güvenlik paketini kullan makalede gösterildiği `AllowCloudPasswordValidationPolicy` gibi [uygulayın.](../domain-joined/apache-domain-joined-architecture.md)

---

## <a name="invalid_grant-or-unauthorized_client-50034"></a>invalid_grant veya unauthorized_client, 50034

### <a name="issue"></a>Sorun

Oturum açma hata kodu 50034 ile başarısız olur. Hata iletisi benzer:

```
{"error":"invalid_grant","error_description":"AADSTS50034: The user account Microsoft.AzureAD.Telemetry.Diagnostics.PII does not exist in the 0c349e3f-1ac3-4610-8599-9db831cbaf62 directory. To sign into this application, the account must be added to the directory.\r\nTrace ID: bbb819b2-4c6f-4745-854d-0b72006d6800\r\nCorrelation ID: b009c737-ee52-43b2-83fd-706061a72b41\r\nTimestamp: 2019-04-29 15:52:16Z", "error_codes":[50034],"timestamp":"2019-04-29 15:52:16Z","trace_id":"bbb819b2-4c6f-4745-854d-0b72006d6800", "correlation_id":"b009c737-ee52-43b2-83fd-706061a72b41"}
```

### <a name="cause"></a>Nedeni

Kullanıcı adı yanlıştır (yok). Kullanıcı, Azure portalında kullanılan kullanıcı adını kullanmıyor.

### <a name="resolution"></a>Çözüm

Bu portalda çalışan kullanıcı adını kullanın.

---

## <a name="invalid_grant-or-unauthorized_client-50053"></a>invalid_grant veya unauthorized_client, 50053

### <a name="issue"></a>Sorun

Kullanıcı hesabı kilitlendi, hata kodu 50053. Hata iletisi benzer:

```
{"error":"unauthorized_client","error_description":"AADSTS50053: You've tried to sign in too many times with an incorrect user ID or password.\r\nTrace ID: 844ac5d8-8160-4dee-90ce-6d8c9443d400\r\nCorrelation ID: 23fe8867-0e8f-4e56-8764-0cdc7c61c325\r\nTimestamp: 2019-06-06 09:47:23Z","error_codes":[50053],"timestamp":"2019-06-06 09:47:23Z","trace_id":"844ac5d8-8160-4dee-90ce-6d8c9443d400","correlation_id":"23fe8867-0e8f-4e56-8764-0cdc7c61c325"}
```

### <a name="cause"></a>Nedeni

Yanlış parolayla denemelerde çok fazla oturum açma.

### <a name="resolution"></a>Çözüm

30 dakika kadar bekleyin, kimlik doğrulaması yapmaya çalışan uygulamaları durdurun.

---

## <a name="invalid_grant-or-unauthorized_client-50053"></a>invalid_grant veya unauthorized_client, 50053

### <a name="issue"></a>Sorun

Parola süresi doldu, hata kodu 50053. Hata iletisi benzer:

```
{"error":"user_password_expired","error_description":"AADSTS50055: Password is expired.\r\nTrace ID: 241a7a47-e59f-42d8-9263-fbb7c1d51e00\r\nCorrelation ID: c7fe4a42-67e4-4acd-9fb6-f4fb6db76d6a\r\nTimestamp: 2019-06-06 17:29:37Z","error_codes":[50055],"timestamp":"2019-06-06 17:29:37Z","trace_id":"241a7a47-e59f-42d8-9263-fbb7c1d51e00","correlation_id":"c7fe4a42-67e4-4acd-9fb6-f4fb6db76d6a","suberror":"user_password_expired","password_change_url":"https://portal.microsoftonline.com/ChangePassword.aspx"}
```

### <a name="cause"></a>Nedeni

Parolanın süresi doldu.

### <a name="resolution"></a>Çözüm

Azure portalındaki (şirket içi sisteminizde) parolayı değiştirin ve ardından eşitlemenin yetişmesini 30 dakika bekleyin.

---

## <a name="interaction_required"></a>interaction_required

### <a name="issue"></a>Sorun

Hata iletisi `interaction_required`alın.

### <a name="cause"></a>Nedeni

Kullanıcıya koşullu erişim ilkesi veya MFA uygulanıyor. Etkileşimli kimlik doğrulaması henüz desteklenmediğinden, kullanıcı veya küme MFA'dan / Koşullu erişimden muaf tutulmalıdır. Kümeden (IP adresi tabanlı muafiyet ilkesi) muaf olmayı seçerseniz, bu vnet için AD'nin `ServiceEndpoints` etkin olduğundan emin olun.

### <a name="resolution"></a>Çözüm

Koşullu erişim ilkesini kullanın ve [Azure Active Directory Domain Services'ı kullanarak KURUMSAL Güvenlik Paketi ile HDInsight kümesini Yapılandır'da](./apache-domain-joined-configure-using-azure-adds.md)gösterildiği gibi HDInisght kümelerini MFA'dan muaf tutarak kullanın.

---

## <a name="sign-in-denied"></a>Oturum açma reddedildi

### <a name="issue"></a>Sorun

Oturum açma reddedildi.

### <a name="cause"></a>Nedeni

Bu aşamaya ulaşmak için, OAuth kimlik doğrulama nız bir sorun değildir, ancak Kerberos kimlik doğrulamasıdır. Bu küme ADLS tarafından desteklenen ise, Kerberos auth girişiminden önce OAuth işareti başarılı olmuştur. WASB kümelerinde OAuth oturum açma girişiminde bulunulmadı. Kerberos hatasının birçok nedeni olabilir - parola kalıpları eşitlenmemiş, kullanıcı hesabı Azure AD DS'de kilitli, ve saire. Parola, yalnızca kullanıcı parolayı değiştirdiğinde eşitleme leri. Azure AD DS örneğini oluşturduğunuzda, oluşturmadan sonra değiştirilen parolaları eşitlemeye başlar. Başlangıcından önce ayarlanmış parolaları geriye dönük olarak eşitlemez.

### <a name="resolution"></a>Çözüm

Parolaların eşitli olmayabileceğini düşünüyorsanız, parolayı değiştirmeyi deneyin ve eşitlemek için birkaç dakika bekleyin.

Bir SSH'e katılmayı deneyin Etki alanına birleştirilmiş bir makineden aynı kullanıcı kimlik bilgilerini kullanarak kimlik doğrulaması (kinit) denemesi gerekir. Yerel bir kullanıcı ile baş / kenar düğümü içine SSH ve sonra kinit çalıştırın.

---

## <a name="kinit-fails"></a>kinit başarısız olur

### <a name="issue"></a>Sorun

Kinit başarısız olur.

### <a name="cause"></a>Nedeni

Değişir.

### <a name="resolution"></a>Çözüm

Kinit başarılı olmak için, (bu `sAMAccountName` alem olmadan kısa hesap adıdır) bilmeniz gerekir. `sAMAccountName`genellikle hesap önekidir (bob `bob@contoso.com`in gibi). Bazı kullanıcılar için, farklı olabilir. Eğer göz atmak / öğrenmek için dizin arama `sAMAccountName`yeteneğine ihtiyacınız olacak.

Bulma `sAMAccountName`yolları:

* Yerel Ambari yöneticisini kullanarak Ambari'de oturum açabiliyorsanız, kullanıcı listesine bakın.

* Windows [machine'e katılan](../../active-directory-domain-services/manage-domain.md)bir etki alanınız varsa, göz atmak için standart Windows AD araçlarını kullanabilirsiniz. Bu etki alanında çalışan bir hesap gerektirir.

* Kafa düğümünden, aramak için SAMBA komutlarını kullanabilirsiniz. Bu geçerli bir Kerberos oturumu (başarılı bir akraba) gerektirir. net reklam arama "(userPrincipalName=bob*)"

    Arama / gözatma sonuçları size `sAMAccountName` özniteliği göstermelidir. Ayrıca, bu özellikleri beklediğiniz `pwdLastSet` `badPasswordTime`eşleşip eşleşmediğini görmek için , , `userPrincipalName` vb gibi diğer öznitelikleri bakabilirsiniz.

---

## <a name="kinit-fails-with-preauthentication-failure"></a>kinit Preauthentication hatası ile başarısız

### <a name="issue"></a>Sorun

Kinit başarısızlıkla başarısız `Preauthentication` olur.

### <a name="cause"></a>Nedeni

Yanlış kullanıcı adı veya parola.

### <a name="resolution"></a>Çözüm

Kullanıcı adınızı ve şifrenizi kontrol edin. Ayrıca yukarıda açıklanan diğer özellikleri kontrol edin. Ayrıntılı hata ayıklamayı etkinleştirmek `export KRB5_TRACE=/tmp/krb.log` için, bükülme denemeden önce oturumdan çalıştırın.

---

## <a name="job--hdfs-command-fails-due-to-tokennotfoundexception"></a>İş / HDFS komutu TokenNotFoundException nedeniyle başarısız olur

### <a name="issue"></a>Sorun

İş / HDFS komutu nedeniyle başarısız `TokenNotFoundException`olur.

### <a name="cause"></a>Nedeni

Gerekli OAuth erişim belirteci iş / komut başarılı olmak için bulunamadı. ADLS / ABFS sürücüsü depolama isteklerini yapmadan önce kimlik bilgileri hizmetinden OAuth erişim belirteci almak için çalışacağız. Ambari portalında aynı kullanıcıyı kullanarak oturum açtığınızda bu belirteç kaydedilir.

### <a name="resolution"></a>Çözüm

Ambari portalına, işi çalıştırmak için kimliği kullanılan kullanıcı adı üzerinden başarılı bir şekilde giriş yaptığınızdan emin olun.

---

## <a name="error-fetching-access-token"></a>Erişim belirteci alma hatası

### <a name="issue"></a>Sorun

Kullanıcı hata `Error fetching access token`iletisi alır.

### <a name="cause"></a>Nedeni

Bu hata, kullanıcılar AK'lar kullanarak ADLS Gen2'ye erişmeye çalıştıklarında ve Kerberos belirteci süresi dolduğunda zaman zaman oluşur.

### <a name="resolution"></a>Çözüm

* Azure Veri Gölü Depolama Gen1 için tarayıcı önbelleğini temizleyin ve Ambari'ye yeniden giriş yapın.

* Azure Veri Gölü Depolama Gen2 için, Kullanıcı olarak oturum açmaya çalıştığı kullanıcı `/usr/lib/hdinsight-common/scripts/RegisterKerbWithOauth.sh <upn>` için çalıştırın

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
