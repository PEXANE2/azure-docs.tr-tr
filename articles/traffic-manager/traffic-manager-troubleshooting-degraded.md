---
title: Azure Trafik Yöneticisi'nde bozulan durum giderme
description: Bozulmuş durum olarak gösterdiğinde Trafik Yöneticisi profilleri sorun giderme.
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: kumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: rohink
ms.openlocfilehash: c398763405472c9018a5c30d34fbd3963ecb93b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938375"
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Azure Traffic Manager’da düşürülmüş durum için sorun giderme

Bu makalede, bozulmuş bir durum gösteren bir Azure Trafik Yöneticisi profili sorun giderme nasıl açıklanmaktadır. Bir Azure Trafik Yöneticisi bozulmuş durum sorun giderme ilk adım olarak tanı günlük etkinleştirmektir.  Daha fazla bilgi için [tanı günlüklerini](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-diagnostic-logs) etkinleştirme'ye bakın. Bu senaryo için, cloudapp.net barındırılan hizmetlerden bazılarını işaret eden bir Trafik Yöneticisi profilini yapılandırdığınızı düşünün. Trafik Yöneticinizin durumu **bozulmuş** bir durum görüntülerse, bir veya daha fazla uç noktanın durumu **bozulabilir:**

![bozulmuş uç nokta durumu](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Trafik Yöneticinizin durumu **etkin olmayan** bir durum görüntülerse, her iki uç nokta devre **dışı bırakılmış**olabilir:

![Etkin Olmayan Trafik Yöneticisi durumu](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Trafik Yöneticisi sondalarını anlama

* Trafik Yöneticisi, sonda sonda yolundan bir HTTP 200 yanıtı aldığında bir bitiş noktasının ÇEVRIMIÇI olduğunu düşünür. Uygulamanız başka bir HTTP yanıt kodu döndürürse, bu yanıt kodunu Trafik Yöneticisi profilinizin [Beklenen durum kodu aralıklarına](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring#configure-endpoint-monitoring) eklemeniz gerekir.
* Trafik Yöneticisi profilinizin [Beklenen durum kodu aralıklarında](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring#configure-endpoint-monitoring) geçerli bir yanıt kodu olarak belirtmediğiniz sürece, 30x yönlendirme yanıtı başarısız olarak kabul edilir. Trafik Yöneticisi yeniden yönlendirme hedefini araştırmaz.
* HTTPs sondaları için sertifika hataları yoksayılır.
* 200 döndürülde kadar sonda yolunun gerçek içeriği önemli değil. URL'yi "/favicon.ico" gibi bazı statik içeriğe doğru araştırmak yaygın bir tekniktir. ASP sayfaları gibi dinamik içerik, uygulama sağlıklı olsa bile her zaman 200'e dönmeyebilir.
* En iyi yöntem, sonda yolunu sitenin yukarı veya aşağı olduğunu belirlemek için yeterli mantığa sahip bir şeye ayarlamaktır. Önceki örnekte, "/favicon.ico" yolunu ayarlayarak, yalnızca w3wp.exe'nin yanıt verdiğini test esiniz. Bu sonda, web uygulamanızın sağlıklı olduğunu göstermeyebilir. Daha iyi bir seçenek, sitenin durumunu belirlemek için mantığı olan "/Probe.aspx" gibi bir şeye giden bir yol ayarlamak olacaktır. Örneğin, CPU kullanımı için performans sayaçları kullanabilir veya başarısız isteklerin sayısını ölçebilirsiniz. Veya web uygulamasının çalıştığından emin olmak için veritabanı kaynaklarına veya oturum durumuna erişmeye çalışabilirsiniz.
* Bir profildeki tüm uç noktalar bozulursa, Trafik Yöneticisi tüm uç noktaları sağlıklı olarak davranır ve trafiği tüm uç noktalara yönlendirir. Bu davranış, sondalama mekanizmasıyla ilgili sorunların hizmetinizin tamamen kesintisine neden olmamasını sağlar.

## <a name="troubleshooting"></a>Sorun giderme

Sonda hatasını gidermek için, sonda URL'sinden HTTP durum kodu dönüşünü gösteren bir araca ihtiyacınız vardır. Ham HTTP yanıtını gösteren birçok araç mevcuttur.

* [Fiddler](https://www.telerik.com/fiddler)
* [Curl](https://curl.haxx.se/)
* [Wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Ayrıca, HTTP yanıtlarını görüntülemek için Internet Explorer'daki F12 Hata Ayıklama Araçları'nın Ağ sekmesini kullanabilirsiniz.

Bu örnekiçin, sonda URL'mizden yanıtı görmek\/istiyoruz: http: /watestsdp2008r2.cloudapp.net:80/Probe. Aşağıdaki PowerShell örneği sorunu göstermektedir.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Örnek çıktı:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

Yeniden yönlendirme yanıtı aldığımıza dikkat edin. Daha önce belirtildiği gibi, 200 dışındaki herhangi bir StatusCode bir hata olarak kabul edilir. Trafik Yöneticisi bitiş noktası durumunu Çevrimdışı olarak değiştirir. Sorunu çözmek için, uygun StatusCode'un sonda yolundan döndürülebilmesini sağlamak için web sitesi yapılandırmasını denetleyin. Trafik Yöneticisi sondasını 200'lük bir yolu işaret etmek için yeniden yapılandırın.

Sondanız HTTPS protokolünü kullanıyorsa, test sırasında SSL/TLS hatalarını önlemek için sertifika denetimini devre dışı bmalısınız. Aşağıdaki PowerShell deyimleri geçerli PowerShell oturumu için sertifika doğrulamadevre dışı bırakma:

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Sonraki Adımlar

[Trafik Yöneticisi trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)

[Trafik Yöneticisi Nedir](traffic-manager-overview.md)

[Bulut Hizmetleri](https://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Uygulama Hizmeti](https://azure.microsoft.com/documentation/services/app-service/web/)

[Traffic Manager üzerindeki işlemler (REST API Başvurusu)](https://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Trafik Yöneticisi Cmdlets][1]

[1]: https://docs.microsoft.com/powershell/module/az.trafficmanager
