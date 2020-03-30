---
title: 'Azure VPN Ağ Geçidi: P2S için & dışa aktarma sertifikaları oluşturun: MakeCert'
description: Kendi imzalı kök sertifikası oluşturun, ortak anahtarı dışa aktarın ve MakeCert'i kullanarak istemci sertifikaları oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/05/2018
ms.author: cherylmc
ms.openlocfilehash: ad2ab31e6771efc54238d5747863fa2a9bb2f356
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75833983"
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>MakeCert kullanarak Site'ye Nokta lı bağlantılar için sertifika oluşturma ve dışa aktarma

Noktadan Siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede, makecert kullanarak kendi imzalı bir kök sertifika oluşturmak ve istemci sertifikaları oluşturmak nasıl gösterilmektedir. Farklı sertifika yönergeleri arıyorsanız, [bkz.](vpn-gateway-certificates-point-to-site.md) [Certificates - Linux](vpn-gateway-certificates-point-to-site-linux.md)

Sertifikalarınızı oluşturmak için [Windows 10 PowerShell adımlarını](vpn-gateway-certificates-point-to-site.md) kullanmanızı öneririz, ancak bu MakeCert yönergelerini isteğe bağlı bir yöntem olarak sayılyız. Her iki yöntemi kullanarak oluşturduğunuz sertifikalar [desteklenen istemci işletim sistemine](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq)yüklenebilir. Ancak, MakeCert aşağıdaki sınırlamaya sahiptir:

* MakeCert amortismana hazır. Bu, bu aracın herhangi bir noktada kaldırılabileceği anlamına gelir. MakeCert kullanarak zaten oluşturduğunuz sertifikalar, MakeCert artık kullanılamadığında etkilenmez. MakeCert yalnızca sertifikaları oluşturmak için kullanılır, doğrulama mekanizması olarak değil.

## <a name="create-a-self-signed-root-certificate"></a><a name="rootcert"></a>Kendi imzalı kök sertifikası oluşturma

Aşağıdaki adımlar, MakeCert'i kullanarak kendi imzalı bir sertifikayı nasıl oluşturabileceğinizi gösterir. Bu adımlar dağıtım modeline özgü değildir. Hem Kaynak Yöneticisi hem de klasik için geçerlidir.

1. [MakeCert'i](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx)indirin ve kurun.
2. Yüklemeden sonra, genellikle bu yol altında makecert.exe yardımcı programı bulabilirsiniz: 'C:\Program Files (x86)\Windows Kitleri\10\bin\<kemer>'. Ancak, başka bir konuma yüklenmiş olabilir. Yönetici olarak bir komut istemi açın ve MakeCert yardımcı programının konumuna gidin. Uygun konumu ayarlamak için aşağıdaki örneği kullanabilirsiniz:

   ```cmd
   cd C:\Program Files (x86)\Windows Kits\10\bin\x64
   ```
3. Bilgisayarınızdaki Kişisel sertifika deposunda bir sertifika oluşturun ve yükleyin. Aşağıdaki örnekte, P2S'yi yapılandırırken Azure'a yüklediğiniz karşılık gelen bir *.cer* dosyası oluşturulur. 'P2SRootCert' ve 'P2SRootCert.cer'i sertifika için kullanmak istediğiniz adla değiştirin. Sertifika 'Sertifikalar - Geçerli Kullanıcı\Kişisel\Sertifikalar' içinde yer alır.

   ```cmd
   makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
   ```

## <a name="export-the-public-key-cer"></a><a name="cer"></a>Ortak anahtarı dışa aktarma (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Dışa aktarılan.cer dosyası Azure'a yüklenmelidir. Yönergeler için [bkz.](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile) Ek bir güvenilen kök sertifikası eklemek için makalenin [bu bölümüne](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) bakın.

### <a name="export-the-self-signed-certificate-and-private-key-to-store-it-optional"></a>Kendi imzalı sertifikayı ve özel anahtarı depolamak için dışa aktarma (isteğe bağlı)

Kendi imzalı kök sertifikasını dışa aktarmak ve güvenli bir şekilde depolamak isteyebilirsiniz. Gerekirse, daha sonra başka bir bilgisayara yükleyebilir ve daha fazla istemci sertifikası oluşturabilir veya başka bir .cer dosyası dışa aktarabilirsiniz. Kendi imzalı kök sertifikasını .pfx olarak dışa aktarmak için kök sertifikayı seçin ve [istemci sertifikası](#clientexport)dışa aktarma'da açıklandığı gibi aynı adımları kullanın.

## <a name="create-and-install-client-certificates"></a>İstemci sertifikaları oluşturma ve yükleme

Kendi imzalı sertifikayı doğrudan istemci bilgisayara yüklemezsiniz. Kendi imzalı sertifikadan bir istemci sertifikası oluşturmanız gerekir. Daha sonra istemci sertifikasını istemci bilgisayarına dışa aktarır ve yüklersiniz. Aşağıdaki adımlar dağıtım modeline özgü değildir. Hem Kaynak Yöneticisi hem de klasik için geçerlidir.

### <a name="generate-a-client-certificate"></a><a name="clientcert"></a>İstemci sertifikası oluşturma

Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. Kendi imzalı kök sertifikasından bir istemci sertifikası oluşturursunuz ve ardından istemci sertifikasını dışa aktarıp yüklersiniz. İstemci sertifikası yüklenmezse kimlik doğrulama başarısız olur. 

Aşağıdaki adımlar, kendi imzalı kök sertifikadan istemci sertifikası oluşturmanıza yol açmaktadır. Aynı kök sertifikadan birden çok istemci sertifikası oluşturabilirsiniz. Aşağıdaki adımları kullanarak istemci sertifikaları oluşturduğunuzda, istemci sertifikası sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. Başka bir istemci bilgisayara istemci sertifikası yüklemek istiyorsanız, sertifikayı dışa aktarabilirsiniz.
 
1. Kendi imzalı sertifikayı oluşturmak için kullandığınız aynı bilgisayarda, yönetici olarak bir komut istemi açın.
2. İstemci sertifikası oluşturmak için örneği değiştirin ve çalıştırın.
   * *"P2SRootCert"i,* istemci sertifikasını oluşturmakta olduğunuz kendi imzalı kökün adıyla değiştirin. Kendi imzalı kökü oluşturduğunuzda belirttiğiniz 'CN=' değeri ne olursa olsun, kök sertifikasının adını kullandığınızdan emin olun.
   * *P2SChildCert'i,* bir istemci sertifikası oluşturmak istediğiniz ada değiştirin.

   Aşağıdaki örneği değiştirmeden çalıştırıyorsanız, sonuç, Kişisel sertifika deposunuzda P2SRootCert adlı kök sertifikap p2SRootCert'ten oluşturulan bir istemci sertifikasıdır.

   ```cmd
   makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
   ```

### <a name="export-a-client-certificate"></a><a name="clientexport"></a>İstemci sertifikası nı dışa aktarma

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install-an-exported-client-certificate"></a><a name="install"></a>Dışarı aktarılan bir istemci sertifikasını yükleme

İstemci sertifikası yüklemek için [bkz.](point-to-site-how-to-vpn-client-install-azure-cert.md)

## <a name="next-steps"></a>Sonraki adımlar

Noktadan Siteye yapılandırmanıza devam edin. 

* **Kaynak Yöneticisi** dağıtım modeli adımları için [bkz.](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* **Klasik** dağıtım modeli adımları için bkz: [Bir VNet'e (klasik) Bir Noktadan Siteye VPN bağlantısını yapılandırın.](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

P2S sorun giderme bilgileri için [Azure noktadan siteye bağlantıları sorununu giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).