---
title: Arka uç sunucularına izin vermek için gereken sertifikalar
titleSuffix: Azure Application Gateway
description: Bu makalede, TLS/SSL sertifikasının azure uygulama ağ geçidinde arka uç örneklerine izin vermek için gereken kimlik doğrulama sertifikasına ve güvenilen kök sertifikaya nasıl dönüştürülebileceğine dair örnekler verilmektedir
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: absha
ms.openlocfilehash: 20f588639c54b0a8b7cd304f33b5a9d633a73be6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80133041"
---
# <a name="create-certificates-to-allow-the-backend-with-azure-application-gateway"></a>Azure Application Gateway ile arka uca izin vermek için sertifikalar oluşturun

TLS'nin sonuna kadar yapmak için, Uygulama Ağ Geçidi kimlik doğrulama/güvenilen kök sertifikaları yükleyerek arka uç örneklerine izin verilmesini gerektirir. v1 SKU için kimlik doğrulama sertifikaları gereklidir, ancak v2 SKU güvenilen kök sertifikaları için sertifikalara izin vermek gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> - Arka uç sertifikasından kimlik doğrulama sertifikasını dışa aktarma (v1 SKU için)
> - Arka uç sertifikasından güvenilir kök sertifikası nı dışa aktarma (v2 SKU için)

## <a name="prerequisites"></a>Ön koşullar

Uygulama Ağ Geçidi ile arka uç örneklerine izin vermek için kimlik doğrulama sertifikaları veya güvenilen kök sertifikaları oluşturmak için varolan bir arka uç sertifikası gereklidir. Arka uç sertifikası TLS/SSL sertifikası ile aynı olabilir veya ek güvenlik için farklı olabilir. Uygulama Ağ Geçidi, TLS/SSL sertifikası oluşturmak veya satın almak için herhangi bir mekanizma sağlamaz. Sınama amacıyla, kendi imzalı bir sertifika oluşturabilirsiniz, ancak bunu üretim iş yükleri için kullanmamalısınız. 

## <a name="export-authentication-certificate-for-v1-sku"></a>Dışa aktarma kimlik belgesi (v1 SKU için)

Application Gateway v1 SKU'da arka uç örneklerine izin vermek için kimlik doğrulama sertifikası gereklidir. Kimlik doğrulama sertifikası, Base-64 kodlu X.509() kodlanmış arka uç sunucu sertifikalarının ortak anahtarıdır. CER) biçimi. Bu örnekte, arka uç sertifikası için bir TLS/SSL sertifikası kullanır ve kimlik doğrulama sertifikası olarak kullanılmak üzere ortak anahtarını dışa aktarın. Ayrıca, bu örnekte, gerekli sertifikaları dışa aktarmak için Windows Certificate Manager aracını kullanırsınız. Uygun başka bir araç kullanmayı seçebilirsiniz.

TLS/SSL sertifikanızdan ortak anahtar .cer dosyasını (özel anahtar değil) dışa aktarın. Aşağıdaki adımlar, Base-64 kodlu X.509(cer dosyası) dışa aktarmanıza yardımcı olur. SERTIFIKANIZ IÇIN CER) biçimi:

1. Sertifikadan bir .cer dosyası almak için **Kullanıcı sertifikalarını yönet** menüsünü açın. Sertifikayı genellikle 'Sertifikalar - Geçerli Kullanıcı\Kişisel\Sertifikalar' olarak bulun ve sağ tıklatın. **Tüm Görevler**’e tıklayın ve ardından **Dışarı Aktar**’a tıklayın. **Sertifika Dışarı Aktarma Sihirbazı** açılır. Sertifikayı Geçerli Kullanıcı\Kişisel\Sertifikalar altında bulamıyorsanız, yanlışlıkla "Sertifikalar - Geçerli Kullanıcı" yerine yanlışlıkla "Sertifikalar - Yerel Bilgisayar" olarak açmış olabilirsiniz. PowerShell'i kullanarak geçerli kullanıcı kapsamında Sertifika Yöneticisi'ni açmak istiyorsanız, konsol penceresine *certmgr* yazarsınız.

   ![Dışarı Aktarma](./media/certificates-for-backend-authentication/export.png)

2. Sihirbazda **İleri'yi**tıklatın.

   ![Sertifikayı dışarı aktarma](./media/certificates-for-backend-authentication/exportwizard.png)

3. **Hayır, özel anahtarı dışarı aktarma**’yı seçin ve **İleri**’ye tıklayın.

   ![Özel anahtarı dışa aktarmayın](./media/certificates-for-backend-authentication/notprivatekey.png)

4. **Dışarı Aktarma Dosyası Biçimi** sayfasında **Base-64 ile kodlanmış X.509 (.CER)** seçeneğini belirleyin ve **İleri**’ye tıklayın.

   ![Base-64 kodlanmış](./media/certificates-for-backend-authentication/base64.png)

5. **DosyaNın Dışa Aktarılaması**için, sertifikayı dışa aktarmak istediğiniz konuma **göz atın.** **Dosya adı** alanına, sertifika dosyası için bir ad girin. Ardından **İleri'yi**tıklatın.

   ![Göz at](./media/certificates-for-backend-authentication/browse.png)

6. Sertifikayı dışarı aktarmak için **Son**'a tıklayın.

   ![Son](./media/certificates-for-backend-authentication/finish.png)

7. Sertifikanız başarıyla dışa aktarılır.

   ![Başarılı](./media/certificates-for-backend-authentication/success.png)

   Dışa aktarılan sertifika şuna benzer:

   ![Verilen](./media/certificates-for-backend-authentication/exported.png)

8. Dışa aktarılan sertifikayı Not Defteri'ni kullanarak açarsanız, bu örneğe benzer bir şey görürsünüz. Mavi bölüm, uygulama ağ geçidine yüklenen bilgileri içerir. Sertifikanızı Not Defteri ile açarsanız ve buna benzer görünmüyorsa, genellikle bu, Base-64 kodlu X.509() kodlu Kullanarak dışa aktarmadığınız anlamına gelir. CER) biçimi. Ayrıca, farklı bir metin düzenleyicisi kullanmak istiyorsanız, bazı düzenleyicilerin arka planda istenmeyen biçimlendirme ler sunabileceğini anlayın. Bu, bu sertifikadaki metni Azure'a yüklediğinizde sorun yaratabilir.

   ![Notepad ile aç](./media/certificates-for-backend-authentication/format.png)

## <a name="export-trusted-root-certificate-for-v2-sku"></a>Güvenilir kök sertifikasını dışa aktarma (v2 SKU için)

Uygulama ağ geçidi v2 SKU'daki arka uç örneklerini beyaz listeye almak için güvenilir kök sertifikası gereklidir. Kök sertifika, X.509 kodlu bir Base-64 sertifikasıdır.( CER) arka uç sunucu sertifikalarından root sertifikasıbiçimlendirme. Bu örnekte, arka uç sertifikası için bir TLS/SSL sertifikası kullanacağız, ortak anahtarını dışa aktaracağız ve daha sonra güvenilen CA'nın kök sertifikasını base64 kodlanmış biçimdeki ortak anahtardan dışa aktarıp güvenilen kök sertifikasını alacağız. Ara sertifika(lar) sunucu sertifikası ile birlikte yüklenmeli ve arka uç sunucusuna yüklenmelidir.

Aşağıdaki adımlar, sertifikanız için .cer dosyasını dışa aktarmanıza yardımcı olur:

1. Yukarıdaki **arka uç sertifikasından (v1 SKU için)** dışa aktarma kimlik doğrulama sertifikası bölümünde belirtilen 1-9 adımlarını kullanarak public keyi arka uç sertifikanızdan dışa aktarın.

2. Ortak anahtar dışa aktarıldıktan sonra dosyayı açın.

   ![Açık yetki belgesi](./media/certificates-for-backend-authentication/openAuthcert.png)

   ![sertifika hakkında](./media/certificates-for-backend-authentication/general.png)

3. Sertifika yetkilisini görüntülemek için Sertifika Yolu görünümüne geçin.

   ![sertifika ayrıntıları](./media/certificates-for-backend-authentication/certdetails.png)

4. Kök sertifikayı seçin ve **Sertifikayı Görüntüle'yi**tıklatın.

   ![cert yolu](./media/certificates-for-backend-authentication/rootcert.png)

   Kök sertifika ayrıntılarını görmelisiniz.

   ![sertifika bilgileri](./media/certificates-for-backend-authentication/rootcertdetails.png)

5. **Ayrıntılar** görünümüne taşıyın ve **Dosyaya Kopyala'yı tıklatın...**

   ![kopya kök cert](./media/certificates-for-backend-authentication/rootcertcopytofile.png)

6. Bu noktada, arka uç sertifikasından kök sertifikanın ayrıntılarını ayıkladınız. **Sertifika Dışa Aktarma Sihirbazı'nı**görürsünüz. Şimdi, yukarıda bir arka uç **sertifikasından (v1 SKU için) alınan özgünlük sertifikasını** Temel-64 kodlu X.509'da güvenilir kök sertifikasını dışa aktarmak için bölüm 2-9'da belirtilen özgünlük sertifikasını kullanın(. CER) biçimi.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Base-64 kodlanmış X.509(. CER) biçimi. Bunu uygulama ağ geçidine ekleyerek arka uç sunucularınızı uçarak TLS şifrelemesi için beyaz listeye alabilirsiniz. [PowerShell ile Uygulama Ağ Geçidi'ni kullanarak TLS'yi sonuna kadar yapılandırın.](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)

