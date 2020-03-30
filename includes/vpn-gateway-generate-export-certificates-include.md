---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e85dc8c079205484db9b7b7c43a0086f69feb3be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80059970"
---
## <a name="create-a-self-signed-root-certificate"></a><a name="rootcert"></a>Kendi imzalı kök sertifikası oluşturma

Kendi imzalı kök sertifikası oluşturmak için Yeni İmzalı Sertifika cmdlet'i kullanın. Ek parametre bilgileri için [Bkz. Yeni İmzalı Sertifika.](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate)

1. Windows 10 veya Windows Server 2016 çalıştıran bir bilgisayardan, yüksek ayrıcalıklara sahip bir Windows PowerShell konsolu açın. Bu örnekler Azure Bulut Kabuğu "Deneyin"de çalışmaz. Bu örnekleri yerel olarak çalıştırmanız gerekir.
2. Kendi imzalı kök sertifikasını oluşturmak için aşağıdaki örneği kullanın. Aşağıdaki örnek, 'Sertifikalar-Geçerli Kullanıcı\Kişisel\Sertifikalar'da otomatik olarak yüklenen 'P2SRootCert' adlı kendi imzalı kök sertifikası oluşturur. *Sertifikayı sertifika.msc'yi*açarak veya *Kullanıcı Sertifikalarını Yöneterek*görüntüleyebilirsiniz.

   ```powershell
   $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
   -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
   -HashAlgorithm sha256 -KeyLength 2048 `
   -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
   ```
 3. Bu kök sertifikayı oluşturduktan hemen sonra bir istemci sertifikası oluşturmak istiyorsanız PowerShell konsolu açık bırakın.

## <a name="generate-a-client-certificate"></a><a name="clientcert"></a>İstemci sertifikası oluşturma

Noktadan Siteye bağlantı kullanarak bir sanal ağa bağlanan her istemci bilgisayarda bir istemci sertifikası yüklü olmalıdır. Kendi imzalı kök sertifikasından bir istemci sertifikası oluşturursunuz ve ardından istemci sertifikasını dışa aktarıp yüklersiniz. İstemci sertifikası yüklenmezse kimlik doğrulama başarısız olur. 

Aşağıdaki adımlar, kendi imzalı kök sertifikadan istemci sertifikası oluşturmanıza yol açmaktadır. Aynı kök sertifikadan birden çok istemci sertifikası oluşturabilirsiniz. Aşağıdaki adımları kullanarak istemci sertifikaları oluşturduğunuzda, istemci sertifikası sertifikayı oluşturmak için kullandığınız bilgisayara otomatik olarak yüklenir. Başka bir istemci bilgisayara istemci sertifikası yüklemek istiyorsanız, sertifikayı dışa aktarabilirsiniz.

Örnekler, bir yıl içinde süresi dolan bir istemci sertifikası oluşturmak için Yeni İmzalı Sertifika cmdlet'i kullanır. İstemci sertifikası için farklı bir son kullanma değeri ayarlama gibi ek parametre bilgileri için [Bkz. Yeni İmzalı Sertifika.](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate)

### <a name="example-1---powershell-console-session-still-open"></a>Örnek 1 - PowerShell konsol oturumu hala açık

Kendi imzalı kök sertifikasını oluşturduktan sonra PowerShell konsolunuzu kapatmadıysanız bu örneği kullanın. Bu örnek önceki bölümden devam ediyor ve '$cert' olarak bildirilen değişkeni kullanır. Kendi imzalı kök sertifikasını oluşturduktan sonra PowerShell konsolunu kapattıysanız veya yeni bir PowerShell konsol oturumunda ek istemci sertifikaları oluşturuyorsanız, [Örnek 2'deki](#ex2)adımları kullanın.

İstemci sertifikası oluşturmak için örneği değiştirin ve çalıştırın. Aşağıdaki örneği değiştirmeden çalıştırıyorsanız, sonuç 'P2SChildCert' adlı bir istemci sertifikasıdır.  Alt sertifikaya başka bir ad vermek istiyorsanız, CN değerini değiştirin. Bu örneği çalıştırırken TextExtension'ı değiştirmeyin. Oluşturduğunuz istemci sertifikası bilgisayarınıza otomatik olarak 'Sertifikalar - Geçerli Kullanıcı\Kişisel\Sertifikalar' olarak yüklenir.

```powershell
New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="example-2---new-powershell-console-session"></a><a name="ex2"></a>Örnek 2 - Yeni PowerShell konsol oturumu

Ek istemci sertifikaları oluşturuyorsanız veya kendi imzalı kök sertifikanızı oluşturmak için kullandığınız PowerShell oturumunu kullanmıyorsanız, aşağıdaki adımları kullanın:

1. Bilgisayara yüklenen kendi imzalı kök sertifikasını tanımlayın. Bu cmdlet, bilgisayarınıza yüklenen sertifikaların listesini döndürür.

   ```powershell
   Get-ChildItem -Path "Cert:\CurrentUser\My"
   ```
2. Döndürülen listeden özne adını bulun ve yanındaki parmak izini bir metin dosyasına kopyalayın. Aşağıdaki örnekte, iki sertifika vardır. CN adı, alt sertifika oluşturmak istediğiniz kendi imzalı kök sertifikasının adıdır. Bu durumda, 'P2SRootCert'.

   ```
   Thumbprint                                Subject
  
   AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
   7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
   ```
3. Önceki adımdaki parmak izini kullanarak kök sertifikası için bir değişken bildirin. THUMBPRINT'i alt sertifika oluşturmak istediğiniz kök sertifikasının parmak izi ile değiştirin.

   ```powershell
   $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
   ```

   Örneğin, önceki adımda P2SRootCert için parmak izi kullanarak, değişken aşağıdaki gibi görünür:

   ```powershell
   $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
   ```
4. İstemci sertifikası oluşturmak için örneği değiştirin ve çalıştırın. Aşağıdaki örneği değiştirmeden çalıştırıyorsanız, sonuç 'P2SChildCert' adlı bir istemci sertifikasıdır. Alt sertifikaya başka bir ad vermek istiyorsanız, CN değerini değiştirin. Bu örneği çalıştırırken TextExtension'ı değiştirmeyin. Oluşturduğunuz istemci sertifikası bilgisayarınıza otomatik olarak 'Sertifikalar - Geçerli Kullanıcı\Kişisel\Sertifikalar' olarak yüklenir.

   ```powershell
   New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
   -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
   -HashAlgorithm sha256 -KeyLength 2048 `
   -CertStoreLocation "Cert:\CurrentUser\My" `
   -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
   ```

## <a name="export-the-root-certificate-public-key-cer"></a><a name="cer"></a>Kök sertifikası ortak anahtarını dışa aktarma (.cer)

[!INCLUDE [Export public key](vpn-gateway-certificates-export-public-key-include.md)]

### <a name="export-the-self-signed-root-certificate-and-private-key-to-store-it-optional"></a>Kendi imzalı kök sertifikasını ve depolamak için özel anahtarı dışa aktarma (isteğe bağlı)

Kendi imzalı kök sertifikasını dışa aktarmak ve güvenli bir şekilde yedekleme olarak depolamak isteyebilirsiniz. Gerekirse, daha sonra başka bir bilgisayara yükleyebilir ve daha fazla istemci sertifikası oluşturabilirsiniz. Kendi imzalı kök sertifikasını .pfx olarak dışa aktarmak için kök sertifikayı seçin ve [istemci sertifikası](#clientexport)dışa aktarma'da açıklandığı gibi aynı adımları kullanın.

## <a name="export-the-client-certificate"></a><a name="clientexport"></a>İstemci sertifikasını dışarı aktarma

[!INCLUDE [Export client certificate](vpn-gateway-certificates-export-client-cert-include.md)]
