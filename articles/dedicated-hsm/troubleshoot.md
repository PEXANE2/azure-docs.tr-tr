---
title: Ayrılmış HSM nedir? - Azure Özel HSM | Microsoft Dokümanlar
description: Azure Özel HSM'ye genel bakış, Azure'da FIPS 140-2 Düzey 3 sertifikasını karşılayan önemli depolama özellikleri sağlar
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc, seodec18
ms.date: 12/07/2018
ms.author: mbaldwin
ms.openlocfilehash: ad3c9d702384b8a32a9d4f0c8aebe44de4bb526e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80337179"
---
# <a name="troubleshooting"></a>Sorun giderme

Azure Özel HSM hizmetinin iki farklı yönü vardır. İlk olarak, HSM aygıtlarının temel ağ bileşenleriyle birlikte Azure'da kaydedilmesi ve dağıtımı. İkinci olarak, belirli bir iş yükü veya uygulama ile kullanıma/entegrasyona hazırlık ta olan HSM aygıtlarının yapılandırması. Thales Luna Network HSM aygıtları Azure'da doğrudan Thales'ten satın aldığınızla aynı olsa da, Azure'da bir kaynak olmaları bazı benzersiz hususlar oluşturur. Bu hususlar ve ortaya çıkan sorun giderme öngörüleri veya en iyi uygulamalar, yüksek görünürlük ve kritik bilgilere erişim sağlamak için burada belgelenmiştir. Hizmet kullanıma sunulduktan sonra, kesin bilgiler doğrudan Microsoft veya Thales'e destek istekleri aracılığıyla kullanılabilir. 

> [!NOTE]
> Yeni dağıtılan bir HSM aygıtında herhangi bir yapılandırma gerçekleştirmeden önce, ilgili yamalar ile güncelleştirilmesi gerektiğini belirtmek gerekir. Belirli bir gerekli yama [KB0019789](https://supportportal.gemalto.com/csm?id=kb_article_view&sys_kb_id=19a81c8bdb9a1fc8d298728dae96197d&sysparm_article=KB0019789) Thales destek portalı olan bir yeniden başlatma askı sorunu giderilmesidir.

## <a name="hsm-registration"></a>HSM Kaydı

Özel HSM, bulutta donanım kaynakları sunduğu ndan serbestçe kullanılamaz ve bu nedenle korunması gereken değerli bir kaynaktır. Bu nedenle kullanarak e-posta HSMrequest@microsoft.comyoluyla bir beyaz liste işlemi kullanın. 

### <a name="getting-access-to-dedicated-hsm"></a>Özel HSM'ye erişim

Özel HSM'nin temel depolama gereksinimlerinize uyacağını düşünüyorsanız, erişim isteğinde bulunmak için e-posta gönder. HSMrequest@microsoft.com Uygulamanızı, istediğiniz bölgeleri ve aradığınız HSM'lerin hacmini anahat. Örneğin Bir Hesap Yöneticisi veya Bulut Çözüm Mimarı gibi bir Microsoft temsilcisiyle çalışıyorsanız, bunları herhangi bir isteğe ekleyin.

## <a name="hsm-provisioning"></a>HSM Temini

Azure'da bir HSM aygıtı nın sağlanması CLI veya PowerShell üzerinden yapılabilir. Hizmete kaydolurken, örnek bir ARM şablonu sağlanacaktır ve ilk özelleştirme için yardım verilecektir. 

### <a name="hsm-deployment-failure-information"></a>HSM Dağıtım Hatası Bilgileri

Özel HSM dağıtım için CLI ve PowerShell'i destekler, böylece portal tabanlı hata bilgileri sınırlıdır ve ayrıntılı değildir. Kaynak Gezgini kullanılarak daha iyi bilgiler bulunabilir. Portal ana sayfasında bunun için bir simge vardır ve daha ayrıntılı hata bilgileri mevcuttur. Bu bilgiler, dağıtım sorunlarıyla ilgili bir destek isteği oluştururken yapıştırılırsa çok yardımcı olur.

![Hata Bilgileri](./media/troubleshoot/failure-information.png)

### <a name="hsm-subnet-delegation"></a>HSM Subnet Delegasyonu
Dağıtım hatalarının bir numaralı nedeni, HSM'lerin sağlandığı müşteri tanımlı alt ağı için uygun delegasyonu ayarlamayı unutmaktır. Bu delegasyonun ayarlanması VNet ve dağıtım için alt net ön koşullarının bir parçasıdır ve öğreticilerde daha fazla ayrıntı bulunabilir.

![Subnet Delegasyonu](./media/troubleshoot/subnet-delegation.png)

### <a name="hsm-deployment-race-condition"></a>HSM Dağıtım Yarış Durumu

Dağıtım için sağlanan standart ARM şablonu HSM ve ExpressRoute ağ geçidi ile ilgili kaynaklara sahiptir. Ağ kaynakları başarılı HSM dağıtımı için bir bağımlılıktır ve zamanlama çok önemli olabilir.  Bazen, bağımlılık sorunlarıyla ilgili dağıtım hataları gördük ve dağıtımı yeniden çalıştırmak genellikle sorunu çözer. Değilse, kaynakları siler ve sonra yeniden dağıtmak genellikle başarılı olur. Bunu denedikten ve sorun bulmaya devam ettikten sonra, Azure portalında "Azure kurulumlarını yapılandıran sorunlar" sorununu seçerek bir destek isteği yükseltin.

### <a name="hsm-deployment-using-terraform"></a>Terraform Kullanarak HSM Dağıtımı

Birkaç müşteri, bu hizmete kaydolurken sağlanan ARM şablonları yerine Terraform'u otomasyon ortamı olarak kullanmaktadır. HSM'ler bu şekilde dağıtılamaz, ancak bağımlı ağ kaynakları dağıtılabilir. Terraform jut HSM dağıtım vardır minimal ARM şablonu için çağırmak için bir modül vardır.  Bu durumda, HSM'leri dağıtmadan önce gerekli ExpressRoute Ağ Geçidi gibi ağ kaynaklarının tam olarak dağıtıldığından emin olmak için dikkatli olunmalıdır. Aşağıdaki CLI komutu, tamamlanmış dağıtım için test etmek ve gerektiğinde tümleştirilebilmek için kullanılabilir. Belirli adlandırma için açı braketi yer tutucularını değiştirin. "ProvisioningState Başarılı" bir sonucu aramak gerekir

```azurecli
az resource show --ids /subscriptions/<subid>/resourceGroups/<myresourcegroup>/providers/Microsoft.Network/virtualNetworkGateways/<myergateway>
```

### <a name="deployment-failure-based-on-quota"></a>Kotaya dayalı dağıtım hatası
Damga başına 2 HSM ve bölge başına 4 HSM'yi aşarsanız dağıtımlar başarısız olabilir. Bu durumu önlemek için, yeniden dağıtmadan önce kaynakları daha önce başarısız olan dağıtımlardan sildiğinizden emin olun. Kaynakları denetlemek için aşağıdaki "HSM'leri nasıl görürüm" öğesine bakın. Eğer öncelikle bir güvenlik olarak orada olan bu kotayı aşmanız HSMrequest@microsoft.com gerektiğini düşünüyorsanız, o zaman ayrıntıları ile e-posta lütfen.

### <a name="deployment-failure-based-on-capacity"></a>Kapasiteye dayalı dağıtım hatası
Belirli bir damga veya bölge dolduğunda, yani neredeyse tüm ücretsiz HSM'ler sağlandığında, bu durum dağıtım hatalarına neden olabilir. Her damganın müşteriler için 11 HSM'si vardır, bu da bölge başına 22 hsm anlamına gelir. Ayrıca her pulda 3 yedek parça ve 1 test cihazı bulunmaktadır. Bir sınıra uymuş olabileceğinizi HSMrequest@microsoft.com düşünüyorsanız, belirli pulların dolum düzeyi hakkında bilgi için e-posta gönderin.

###  <a name="how-do-i-see-hsms-when-provisioned"></a>Sağlandığında HSM'leri nasıl görebiliyorum?
Özel HSM'nin beyaz listeye alınmış bir hizmet olması nedeniyle, Azure portalında "Gizli Tür" olarak kabul edilir. HSM kaynaklarını görmek için, aşağıda gösterildiği gibi "Gizli türleri göster" onay kutusunu denetlemeniz gerekir. NIC kaynağı her zaman HSM izler ve bağlanmak için SSH kullanmadan önce HSM IP adresini bulmak için iyi bir yerdir.

![Subnet Delegasyonu](./media/troubleshoot/hsm-provisioned.png)

## <a name="networking-resources"></a>Ağ Kaynakları

Özel HSM dağıtımı ağ kaynaklarına ve bunun sonucunda dikkat edilmesi gereken bazı sınırlamalara bağımlıdır.

### <a name="provisioning-expressroute"></a>ExpressRoute Sağlama

Özel HSM, ExpressRoute Ağ Geçidi'ni, müşteri özel IP adres alanı ile Azure veri merkezindeki fiziksel HSM arasındaki iletişim için bir "tünel" olarak kullanır.  Vnet başına bir ağ geçidi kısıtlaması olduğu göz önüne alındığında, ExpressRoute üzerinden şirket içi kaynaklarına bağlantı gerektiren müşterilerin bu bağlantı için başka bir Vnet kullanması gerekir.  

### <a name="hsm-private-ip-address"></a>HSM Özel IP Adresi

Özel HSM için sağlanan örnek şablonlar, HSM IP'sinin otomatik olarak belirli bir alt ağ aralığından alınacağını varsayar. ARM şablonundaki "NetworkInterfaces" özniteliği aracılığıyla HSM için açık bir IP adresi belirtebilirsiniz. 

![Subnet Delegasyonu](./media/troubleshoot/private-ip-address.png)

## <a name="hsm-initialization"></a>HSM Başlatma

Başlatma, kullanım için yeni bir HSM veya yeniden kullanmak için varolan bir HSM hazırlar. Nesneleri oluşturabilmek veya depolamak, istemcilerin bağlanmasına veya şifreleme işlemleri gerçekleştirmesine izin vermeden önce HSM'nin başlatılması nın tamamlanması gerekir.

### <a name="lost-credentials"></a>Kayıp Kimlik Bilgileri

Shell yönetici parolasının kaybolması HSM anahtar materyalinin kaybolmasına neden olur. HSM'yi sıfırlamak için bir destek isteği yapılmalıdır.
HSM'yi başlatmada, kimlik bilgilerini güvenli bir şekilde depolayın. Shell ve HSM kimlik bilgileri şirketinizin politikalarına uygun olarak tutulmalıdır.

### <a name="failed-logins"></a>Başarısız Girişler

HSM'lere yanlış kimlik bilgileri sağlamak yıkıcı sonuçlardoğurabilir. HSM Rolleri için varsayılan davranışlar aşağıda veda eder.

| Rol | Eşik (# deneme) | Çok fazla kötü giriş denemesinin sonucu | Kurtarma |
|--|--|--|--|
| HSM SO | 3 |  HSM sıfırlanır (tüm HSM nesneleri kimlikleri ve tüm bölümler gitti)  |  HSM yeniden başlatılması gerekir. İçerikler yedekleme(ler)'den geri yüklenebilir. | 
| Bölüm SO | 10 |  Bölüm sıfırlanır. |  Bölüm yeniden başlatılması gerekir. İçerikler yedeklemeden geri yüklenebilir. |  
| Denetim | 10 | Kilitleme | 10 dakika sonra otomatik olarak kilidi açılır. |  
| Kripto Subay | 10 (azaltılabilir) | HSM ilkesi 15: Enable SO sıfırlama bölümü PIN 1 (etkin) olarak ayarlanırsa, CO ve CU rolleri kilitlenir.<br>HSM ilkesi 15: Enable SO reset of partition PIN 0 (devre dışı) olarak ayarlanırsa, CO ve CU rolleri kalıcı olarak kilitlenir ve bölüm içeriğine artık erişilemez. Bu varsayılan ayardır. | CO rolü kilidi ve bölüm SO tarafından kimlik bilgisi sıfırlanır, kullanarak `role resetpw -name co`.<br>Bölüm yeniden başlatılması ve anahtar malzemenin yedek aygıttan geri yüklenmesi gerekir. |  

## <a name="hsm-configuration"></a>HSM Yapılandırması 

Aşağıdaki öğeler, yapılandırma hatalarının yaygın olduğu veya çağrılmaya değer bir etkiye sahip olduğu durumlardır:

### <a name="hsm-documentation-and-software"></a>HSM Dokümantasyon ve Yazılım
Thales SafeNet Luna 7 HSM cihazlarıiçin yazılım ve dokümantasyon Microsoft tarafından kullanılamaz ve doğrudan Thales'ten indirilmelidir. Kayıt işlemi sırasında alınan Thales Müşteri Kimliği kullanılarak kayıt gereklidir. Cihazlar, Microsoft tarafından sağlanan, yazılım sürümü 7.2 ve firmware sürümü 7.0.3 var. Erken 2020 Thales belgeleri kamu yaptı ve [burada](https://thalesdocs.com/gphsm/luna/7.2/docs/network/Content/Home_network.htm)bulunabilir.  

### <a name="hsm-networking-configuration"></a>HSM Ağ Yapılandırması

HSM içindeki ağı yapılandırırken dikkatli olun.  HSM, ExpressRoute Ağ Geçidi üzerinden bir müşteri özel IP adres alanından doğrudan HSM'ye bağlantı kurar.  Bu iletişim kanalı yalnızca müşteri iletişimi içindir ve Microsoft'un erişimi yoktur. HSM, bu ağ yolunun etkiedilen bir şekilde yapılandırılması, HSM ile tüm iletişimin kaldırıldığı anlamına gelir.  Bu durumda, tek seçenek aygıtısıfırlamak için Azure portalı üzerinden bir Microsoft destek isteği yükseltmektir. Bu sıfırlama yordamı HSM'yi ilk durumuna geri ayarlar ve tüm yapılandırma ve anahtar malzeme kaybolur.  Yapılandırma yeniden oluşturulmalıdır ve aygıt HA grubuna katıldığında anahtar malzeme çoğaltılır alırsınız.  

### <a name="hsm-device-reboot"></a>HSM Cihazı yeniden başlatma

Bazı yapılandırma değişiklikleri, HSM'nin güç döngüsüne dönüştürülmesini veya yeniden başlatılmasını gerektirir. Azure'daki HSM'nin Microsoft testi, bazı durumlarda yeniden başlatmanın askıda kalabileceğini belirlemiştir. Bunun anlamı, Azure portalında zor yeniden başlatma isteğinde bulunan bir destek isteğinin oluşturulması ve azure veri merkezinde el ile bir işlem olduğu düşünüldüğünde tamamlanmasının 48 saat kadar süreceğin dir.  Bu durumu önlemek için, Thales'ten gelen yeniden başlatma düzeltme ekini doğrudan dağıttığınızdan emin olun. Thales Luna Network HSM 7.2 Yeniden başlatma sorunu için önerilen bir yama için indirmelerde [KB0019789'a](https://supportportal.gemalto.com/csm?sys_kb_id=d66911e2db4ffbc0d298728dae9619b0&id=kb_article_view&sysparm_rank=1&sysparm_tsqueryId=d568c35bdb9a4850d6b31f3b4b96199e&sysparm_article=KB0019789) bakın (Not: Indirmek için Thales destek portalına kaydolmanız gerekir).

### <a name="ntls-certificates-out-of-sync"></a>NTLS Sertifikaları eşitlenmemiş
Bir istemci, sertifikanın süresi dolduğunda veya yapılandırma güncelleştirmeleri aracılığıyla üzerine yazıldığında HSM'ye bağlantı kaybedebilir. Sertifika değişimi istemci yapılandırması her HSM ile yeniden uygulanmalıdır.
Örnek Geçersiz sertifika ile NTLS günlüğe kaydetme:

> NTLS[8508]: info : 0 : Gelen bağlantı isteği... : 192.168.50.50.2/59415 NTLS[8508]: SSLAccept'den gelen hata mesajı : hata:14094418:SSL rutinleri:ssl3_read_bytes:tlsv1 uyarısı bilinmiyor ca NTLS[8508]: SSL sırasında hata kabul edin ( RC_SSL_ERROR ) NTLS[8508]: info : 0xc0000711 : İstemci ile güvenli bir kanal kuramamak : 192.168.50.2/59415 : RC_SSL_FAILED_HANDSHAKE NTLS[8508]: info : 0 : NTLS İstemci "Bilinmeyen ana bilgisayar adı" Bağlantı : 192.168.50.2/59415

### <a name="failed-tcp-communication"></a>Başarısız TCP İletişimi

Luna Client yüklemesinden HSM'ye iletişim için en az TCP bağlantı noktası 1792 gerekir. Ortamdaki ağ yapılandırmaları değiştikçe bunu göz önünde bulundurun.

### <a name="failed-ha-group-member-doesnt-recover"></a>Başarısız HA Grup Üyesi Kurtarılmıyor

Başarısız bir HA Group üyesi kurtarılamazsa, hagroup kurtarma komutunu kullanarak Luna istemcisinden el ile kurtarılmalıdır.
Otomatik kurtarmayı etkinleştirmek için bir HA grubu için yeniden deneme sayısını yapılandırmak gerekir. Varsayılan olarak bir HA grubu kurtarıldığında bir HA üyesini gruba kurtarmagirişiminde bulunmaz.

### <a name="ha-group-doesnt-sync"></a>HA Grubu Eşitleme Yapmaz

Üye bölümlerin aynı klonlama etki alanına sahip olmadığı durumlarda, ha eşitleme komutu aşağıdakileri görüntüler: Uyarı: Eşitleme başarısız olabilir.  Yuva 0 ve yuva 1'deki üyelerin özel anahtar klonlama için çakışan ayarları vardır.
Doğru klonlama etki alanına sahip yeni bir bölüm HA grubuna eklenmeli ve ardından yanlış yapılandırılmış bölüm kaldırılmalıdır.

## <a name="hsm-deprovisioning"></a>HSM Deprovisioning 

Yalnızca bir HSM ile tamamen tamamlandığında bu hükümden arındırılabilir ve ardından Microsoft bunu sıfırlayıp ücretsiz bir havuza döndürür. 

### <a name="how-to-delete-an-hsm-resource"></a>HSM kaynağı nasıl silinir?

HSM için Azure kaynağı, HSM "sıfırlanmış" durumda olmadığı sürece silinemez.  Bu nedenle, kaynak olarak silmeye çalışmadan önce tüm anahtar malzeme silinmiş olmalıdır. Sıfırlamanın en hızlı yolu HSM yönetici parolasını yanlış 3 kez almaktır (not: bu, cihaz düzeyindeki yöneticiye değil, HSM yöneticisine başvurur). Luna kabuk sıfırlar `hsm -factoryreset` bir komut var ama sadece seri bağlantı noktasında konsol üzerinden yürütülebilir ve müşterilerin bu erişimi yok.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, HSM dağıtım yaşam döngüsü boyunca sorunlar olabilir veya sorun giderme veya dikkatli değerlendirme gerektiren alanlar hakkında bilgi verilmiştir. Umarım bu makale, gereksiz gecikmeleri ve hayal kırıklığını önlemenize yardımcı olur ve ilgili eklemeler veya değişiklikleriniz varsa, Microsoft ile bir destek isteği yükseltin ve bize bildirin. 
