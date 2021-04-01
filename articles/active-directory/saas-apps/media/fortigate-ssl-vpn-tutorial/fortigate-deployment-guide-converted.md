---
title: FortiGate dağıtım kılavuzu | Microsoft Docs
description: Fortinet FortiGate yeni nesil güvenlik duvarı ürününü kurun ve bunlarla çalışın.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/30/2020
ms.author: jeedes
ms.openlocfilehash: cdaa6a9601452100ab90ef8b0f2191002f256b74
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "95025524"
---
# <a name="fortigate-azure-virtual-machine-deployment-guide"></a>FortiGate Azure sanal makine dağıtım kılavuzu

Bu dağıtım kılavuzunu kullanarak, bir Azure sanal makinesi olarak dağıtılan bir sonraki nesil güvenlik duvarı ürünü olan Fortinet FortiGate 'i ayarlamayı ve bunlarla nasıl çalışacağınızı öğreneceksiniz. Ek olarak, Azure Active Directory aracılığıyla VPN kimlik doğrulaması sağlamak için FortiGate SSL VPN Azure AD Galerisi uygulamasını yapılandıracaksınız.

## <a name="redeem-the-fortigate-license"></a>FortiGate lisansını kullanma

Fortinet FortiGate yeni nesil güvenlik duvarı ürünü, Azure hizmet olarak altyapı (IaaS) içinde bir sanal makine olarak kullanılabilir. Bu sanal makine için iki lisans modu vardır: Kullandıkça öde ve kendi lisansını getir (KLG).

KLG sanal makine dağıtımı seçeneği ile, Fortinet 'ten bir FortiGate lisansı satın aldıysanız, bunu Fortinet 'in ürün etkinleştirme sayfasından kullanın: https://support.fortinet.com . Elde edilen lisans dosyası. lik dosya uzantısına sahip olacaktır.

## <a name="download-firmware"></a>Üretici yazılımını indirme

Yazma sırasında, Fortinet FortiGate Azure VM 'si SAML kimlik doğrulaması için gerekli olan bellenim sürümü ile birlikte gelmez. En son sürüm, Fortinet 'ten alınmalıdır.

1. ' Da oturum açın https://support.fortinet.com/ .
2.   >  **Bellenim görüntülerini** indirme bölümüne gidin.
3. **Sürüm notlarının** sağında **İndir**' i seçin.
4. **V 6.00**  >  **6,4**  >  **6.4.2** öğesini seçin.
5. Aynı satırdaki **https** bağlantısını seçerek **FGT_VM64_AZURE-v6-build1723-FORTINET. out** öğesini indirin.
6. Dosyayı daha sonra kaydedin.

## <a name="deploy-the-fortigate-vm"></a>FortiGate sanal makinesini dağıtma

1. Azure portal gidin ve FortiGate sanal makinesini dağıtacağınız abonelikte oturum açın.
2. Yeni bir kaynak grubu oluşturun veya FortiGate sanal makinesini dağıtacaksınız kaynak grubunu açın.
3. **Add (Ekle)** seçeneğini belirleyin.
4. **Markette ara** bölümünde *FORTI* girin. **Fortinet FortiGate yeni nesil güvenlik duvarı**' nı seçin.
5. Yazılım planını seçin (lisansınız varsa, kendi lisansını getir veya yoksa Kullandıkça öde). **Oluştur**’u seçin.
6. VM yapılandırmasını doldurun.

    ![Sanal makine oluşturma ekranının ekran görüntüsü.](virtual-machine.png)

7. **Kimlik doğrulama türünü** **parola** olarak ayarlayın ve VM için yönetici kimlik bilgilerini sağlayın.
8. **Gözden geçir ve oluştur** > **Oluştur**'u seçin.
9. VM dağıtımının tamamlanmasını bekleyin.


### <a name="set-a-static-public-ip-address-and-assign-a-fully-qualified-domain-name"></a>Statik bir genel IP adresi ayarlayın ve tam etki alanı adı atayın

Tutarlı bir kullanıcı deneyimi için, FortiGate sanal makinesine atanan genel IP adresini statik olarak atanacak şekilde ayarlayın. Ayrıca, tam etki alanı adı (FQDN) ile eşleyin.

1. Azure portal gidin ve FortiGate sanal makinesi için ayarları açın.
2. **Genel bakış** EKRANıNDA genel IP adresini seçin.

    ![Fortigate SSL VPN ekran görüntüsü.](public-ip-address.png)

3. **Statik**  >  **Kaydet**' i seçin.

FortiGate VM 'nin dağıtıldığı ortam için genel olarak yönlendirilebilir bir etki alanı adınız varsa, VM için bir konak (A) kaydı oluşturun. Bu kayıt, statik olarak atanan önceki genel IP adresiyle eşleşir.

### <a name="create-a-new-inbound-network-security-group-rule-for-tcp-port-8443"></a>TCP bağlantı noktası 8443 için yeni bir gelen ağ güvenlik grubu kuralı oluşturma

1. Azure portal gidin ve FortiGate sanal makinesi için ayarları açın.
2. Soldaki menüden **ağ iletişimi**' ni seçin. Ağ arabirimi listelenir ve gelen bağlantı noktası kuralları gösterilir.
3. **Gelen bağlantı noktası kuralı ekle**' yi seçin.
4. TCP 8443 için yeni bir gelen bağlantı noktası kuralı oluşturun.

    ![Gelen güvenlik kuralı Ekle ekran görüntüsü.](port-rule.png)

5. **Add (Ekle)** seçeneğini belirleyin.

## <a name="create-a-second-virtual-nic-for-the-vm"></a>VM için Ikinci bir sanal NIC oluşturma

Dahili kaynakların kullanıcılara kullanılabilir hale getirilmeleri için, FortiGate sanal makinesine ikinci bir sanal NIC eklenmelidir. Sanal NIC 'nin bulunduğu sanal Ağa gelen Azure 'un bu iç kaynaklarla yönlendirilebilir bağlantısı olması gerekir.

1. Azure portal gidin ve FortiGate sanal makinesi için ayarları açın.
2. FortiGate VM zaten durdurulmamışsa **Durdur** ' u SEÇIN ve VM 'nin kapatılmasını bekleyin.
3. Soldaki menüden **ağ iletişimi**' ni seçin.
4. **Ağ arabirimi Ekle**' yi seçin.
5. **Oluştur ve ağ arabirimi Ekle '** yi seçin.
6. Yeni ağ arabiriminin özelliklerini yapılandırın ve ardından **Oluştur**' u seçin.

    ![Ağ arabirimi oluşturma ekranının ekran görüntüsü.](new-network-interface.png)

7. FortiGate sanal makinesini başlatın.


## <a name="configure-the-fortigate-vm"></a>FortiGate VM 'sini yapılandırma

Aşağıdaki bölümler, FortiGate sanal makinesini ayarlama konusunda size rehberlik sağlar.

### <a name="install-the-license"></a>Lisansı yükler

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Dağıtım kendi lisansını getir modelini kullanıyorsa, lisans yüklemek için bir istem görürsünüz. Daha önce oluşturulan lisans dosyasını seçin ve karşıya yükleyin. **Tamam** ' ı seçin ve FortiGate sanal makinesini yeniden başlatın.

    ![FortiGate VM lisansının ekran görüntüsü.](license.png)

5. Yeniden başlatma işleminden sonra, lisansı doğrulamak için yönetici kimlik bilgileriyle yeniden oturum açın.

### <a name="update-firmware"></a>Üretici yazılımını güncelleştirme

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Sol menüde **sistem**  >  **bellenimi**' nu seçin.
5. **Üretici yazılımı yönetimi**' nde, **Araştır**' ı seçin ve daha önce indirilen bellenim dosyasını seçin.
6. Uyarıyı yoksayın ve **yedekleme yapılandırması ve yükseltme '** yi seçin.

    ![Üretici yazılımı yönetiminin ekran görüntüsü.](backup-configure-upgrade.png)

7. **Devam**’ı seçin.
8. FortiGate yapılandırmasını (. conf dosyası olarak) kaydetmeniz istendiğinde **Kaydet**' i seçin.
9. Üretici yazılımının karşıya yüklenmesini ve uygulanmasını bekleyin. FortiGate sanal makinesinin yeniden başlatılmasını bekleyin.
10. FortiGate sanal makinesi yeniden başlatıldıktan sonra, yönetici kimlik bilgileriyle tekrar oturum açın.
11. Panoyu ayarlamanız istendiğinde **daha sonra** öğesini seçin.
12. Öğretici videosu başladığında **Tamam**' ı seçin.

### <a name="change-the-management-port-to-tcp-8443"></a>Yönetim bağlantı noktasını TCP 8443 olarak değiştirme

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Sol menüde **sistem**' i seçin.
5. **Yönetim ayarları** altında HTTPS bağlantı noktasını **8443** olarak değiştirin ve **Uygula**' yı seçin.
6. Değişiklik uygulandıktan sonra tarayıcı, yönetim sayfasını yeniden yüklemeye çalışır, ancak başarısız olur. Şu andan itibaren yönetim sayfası adresi `https://<address>:8443` .

    ![Uzak sertifika yükleme ekran görüntüsü.](certificate.png)

### <a name="upload-the-azure-ad-saml-signing-certificate"></a>Azure AD SAML Imzalama sertifikasını karşıya yükleme

1. `https://<address>:8443` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Sol menüde **sistem**  >  **sertifikaları**' nı seçin.
5. Uzak **sertifikayı içeri aktar**' ı seçin  >  .
6. Azure kiracısındaki FortiGate özel uygulama dağıtımından indirilen sertifikaya gidin. Seçin ve **Tamam**' ı seçin.

### <a name="upload-and-configure-a-custom-ssl-certificate"></a>Özel bir SSL sertifikası yükleme ve yapılandırma

FortiGate sanal makinesini, kullanmakta olduğunuz FQDN 'yi destekleyen kendi SSL sertifikanız ile yapılandırmak isteyebilirsiniz. PFX biçiminde özel anahtarla paketlenmiş bir SSL sertifikasına erişiminiz varsa, bu amaçla kullanılabilir.

1. `https://<address>:8443` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Sol menüde **sistem**  >  **sertifikaları**' nı seçin.
5. Yerel **sertifikayı içeri aktar**  >    >  **PKCS #12 sertifikası**' nı seçin.
6. Öğesine gidin. SSL sertifikasını ve özel anahtarı içeren PFX dosyası.
7. Sağlayın. PFX parolası ve sertifika için anlamlı bir ad. Ardından **Tamam**’ı seçin.
8. Sol menüde **sistem**  >  **ayarları**' nı seçin.
9. **Yönetim ayarları** altında, **https sunucu sertifikası**' nın yanındaki listeyi genişletin ve daha önce içeri aktarılan SSL sertifikasını seçin.
10. **Uygula**’yı seçin.
11. Tarayıcı penceresini kapatın ve adresine gidin `https://<address>:8443` .
12. FortiGate yönetici kimlik bilgileriyle oturum açın. Artık kullanımda olan doğru SSL sertifikasını görmeniz gerekir.

### <a name="configure-authentication-timeout"></a>Kimlik doğrulama zaman aşımını yapılandırma

1. Azure portal gidin ve FortiGate sanal makinesi için ayarları açın.
2. Sol taraftaki menüden **seri konsolu**' nu seçin.
3. Kimliği, sanal makine yöneticisi kimlik bilgileriyle seri konsolunda oturum açın.
4. Seri konsolunda aşağıdaki komutları çalıştırın:

    ```
    config system global
    set remoteauthtimeout 60
    end
    ```

### <a name="ensure-network-interfaces-are-obtaining-ip-addresses"></a>Ağ arabirimlerinin IP adreslerini aldığından emin olun

1. `https://<address>:8443` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
3. Soldaki menüden **ağ iletişimi**' ni seçin.
4. Ağ altında, **arabirimler**' i seçin.
5. Doğru Azure alt ağından bir IP adresi aldığından emin olmak için PORT1 (dış arabirim) ve PORT2 (iç arabirim) ' i inceleyin.
    a. Her iki bağlantı noktası da alt ağdan (DHCP aracılığıyla) bir IP adresi alsa, bağlantı noktasına sağ tıklayıp **Düzenle**' yi seçin.
    b. Adresleme modu ' nun yanında **DHCP** ' nin seçili olduğundan emin olun.
    c. Seelct **Tamam**.

    ![Ağ arabirimi adreslemesinin ekran görüntüsü.](addressing.png)

### <a name="ensure-fortigate-vm-has-correct-route-to-on-premises-corporate-resources"></a>FortiGate VM 'sinin şirket Içi kurumsal kaynaklara doğru rota içerdiğinden emin olun

Çoklu bilgisayarlı Azure VM 'Leri aynı sanal ağ üzerindeki tüm ağ arabirimlerine sahiptir (ancak belki de farklı alt ağlar). Bu, genellikle her iki ağ arabiriminin de FortiGate aracılığıyla yayımlanmakta olan şirket içi kurumsal kaynaklarla bağlantısı olduğu anlamına gelir. Bu nedenle, şirket içi kurumsal kaynaklara yönelik istekler yapıldığında trafiğin doğru arabirimden çıkış yaptığından emin olan özel yol girişleri oluşturmak gerekir.

1. `https://<address>:8443` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
3. Soldaki menüden **ağ iletişimi**' ni seçin.
4. Ağ altında **statik yollar**' ı seçin.
5. **Yeni Oluştur**’u seçin.
6. Hedef ' in yanında **alt ağ** seçin.
7. Alt ağ altında, şirket içi kurumsal kaynakların bulunduğu alt ağ bilgilerini belirtin (örn. 10.1.0.0/255.255.255.0)
8. Ağ geçidi adresi ' nin yanında, PORT2 bağlı olduğu Azure alt ağında ağ geçidini belirtin (örn. Bu genellikle 1 benzer 10.6.1.1)
9. Arabirim ' in yanındaki iç ağ arabirimini seçin, PORT2
10. **Tamam**’ı seçin.

    ![Rota yapılandırma ekran görüntüsü.](route.png)

## <a name="configure-fortigate-ssl-vpn"></a>FortiGate SSL VPN 'yi yapılandırma

İçinde özetlenen adımları izleyin https://docs.microsoft.com/azure/active-directory/saas-apps/fortigate-ssl-vpn-tutorial
