---
title: Windows güvenlik kullanarak Windows'da çalışan bir kümeyi güvenli hale
description: Windows güvenliğini kullanarak Windows'da çalışan bağımsız bir kümede düğümden düğüme ve istemciden düğüme güvenliği nasıl yapılandıracaklarını öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 46be6acc1ef08770826a2e020c8930eba0787791
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76774438"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Windows güvenliğini kullanarak Windows'da bağımsız bir kümeyi güvenli hale
Hizmet Kumaşı kümesine yetkisiz erişimi önlemek için kümeyi korumanız gerekir. Küme üretim iş yüklerini çalıştırdığında güvenlik özellikle önemlidir. Bu makalede, *ClusterConfig.JSON* dosyasındaki Windows güvenliğini kullanarak düğümden düğüme ve istemciden düğüme güvenliğin nasıl yapılandırılabildiğini açıklanmaktadır.  İşlem, [Windows'ta çalışan bağımsız bir küme oluşturma'nın](service-fabric-cluster-creation-for-windows-server.md)yapılandırma güvenlik adımına karşılık gelir. Service Fabric'in Windows güvenliğini nasıl kullandığı hakkında daha fazla bilgi için [Küme güvenlik senaryolarına](service-fabric-cluster-security.md)bakın.

> [!NOTE]
> Bir güvenlik seçiminden diğerine küme yükseltmesi olmadığından, düğümden düğüme güvenlik seçimini dikkatle düşünmelisiniz. Güvenlik seçimini değiştirmek için tam kümeyi yeniden oluşturmanız gerekir.
>
>

## <a name="configure-windows-security-using-gmsa"></a>GMSA kullanarak Windows güvenliğini yapılandırma  
Microsoft.Azure.ServiceFabric.WindowsServer ile indirilen *clusterConfig.gMSA.Windows.MultiMachine.JSON* yapılandırma dosyası. [\< sürüm>.zip](https://go.microsoft.com/fwlink/?LinkId=730690) bağımsız küme paketi [Grup Yönetilen Hizmet Hesabı (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)kullanarak Windows güvenlik yapılandırmak için bir şablon içerir:  

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {  
        "ClustergMSAIdentity": "[gMSA Identity]",
        "ClusterSPN": "[Registered SPN for the gMSA account]",
        "ClientIdentities": [
            {
                "Identity": "domain\\username",
                "IsAdmin": true
            }
        ]
    }
}
```

| **Yapılandırma ayarı** | **Açıklama** |
| --- | --- |
| ClusterCredentialType |Düğüm düğümü iletişimi için Windows güvenliğini etkinleştirmek için *Windows* olarak ayarlayın.  | 
| ServerCredentialType |İstemci düğümü iletişimi için Windows güvenliğini etkinleştirmek için *Windows* olarak ayarlayın. |
| Windows Kimlikleri |Küme ve istemci kimliklerini içerir. |
| ClustergMSAIdentity |Düğümden düğüme güvenliği yapılandırır. Bir grup yönetilen hizmet hesabı. |
| KümesPN |gMSA hesabı için kayıtlı SPN|
| Müşteri Kimlikleri |İstemciden düğüme güvenliği yapılandırır. Bir dizi istemci kullanıcı hesabı. |
| Kimlik |İstemci kimliği için etki alanı kullanıcısını, etki alanı\kullanıcı adını ekleyin. |
| IsAdmin |Etki alanı kullanıcısının yönetici istemci erişimine sahip olduğunu veya kullanıcı istemci erişimi için yanlış olduğunu belirtmek için doğru olarak ayarlayın. |

> [!NOTE]
> ClustergMSAIdentity değeri " biçimindemysfgmsa@mydomainolmalıdır" ".

[Düğüm güvenliği düğüm,](service-fabric-cluster-security.md#node-to-node-security) hizmet dokusu gMSA altında çalışması gerektiğinde **ClustergMSAIdentity** ayarlayarak yapılandırılır. Düğümler arasında güven ilişkileri kurmak için, birbirlerinin farkında yapılmalıdır. Bu iki farklı şekilde gerçekleştirilebilir: Kümedeki tüm düğümleri içeren Grup Yönetilen Hizmet Hesabı'nı belirtin veya kümedeki tüm düğümleri içeren etki alanı makine grubunu belirtin. Özellikle daha büyük kümeler (10'dan fazla düğüm) veya büyüme veya küçülme olasılığı olan kümeler için [Grup Yönetilen Hizmet Hesabı (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) yaklaşımını kullanmanızı şiddetle öneririz.  
Bu yaklaşım, küme yöneticilerine üye ekleme ve kaldırma erişim hakları verilen bir etki alanı grubu oluşturulmasını gerektirmez. Bu hesaplar otomatik parola yönetimi için de yararlıdır. Daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/jj128431.aspx)  
 
[İstemciden düğüm güvenliği](service-fabric-cluster-security.md#client-to-node-security) **Istemci Kimlikleri**kullanılarak yapılandırılır. İstemci ve küme arasında güven oluşturmak için, kümeyi güvenebileceği istemci kimliklerini bilecek şekilde yapılandırmanız gerekir. Bu iki farklı şekilde yapılabilir: Bağlanabilen etki alanı grubu kullanıcılarını belirtin veya bağlanabilen etki alanı düğüm kullanıcılarını belirtin. Service Fabric, Service Fabric kümesine bağlı istemciler için iki farklı erişim denetim türünü destekler: yönetici ve kullanıcı. Erişim denetimi, küme yöneticisinin farklı kullanıcı grupları için belirli türdeki küme işlemlerine erişimi sınırlaması ve kümeyi daha güvenli hale getirme olanağı sağlar.  Yöneticiler yönetim yeteneklerine (okuma/yazma özellikleri dahil) tam erişime sahiptir. Kullanıcılar, varsayılan olarak, yalnızca yönetim yeteneklerine (örneğin, sorgu yetenekleri) ve uygulamaları ve hizmetleri çözümleme özelliğine erişimi okumuştur. Erişim denetimleri hakkında daha fazla bilgi için Service [Fabric istemcileri için Rol tabanlı erişim denetimine](service-fabric-cluster-security-roles.md)bakın.  
 
Aşağıdaki örnek **güvenlik** bölümü, gMSA kullanarak Windows güvenliğini yapılandırır ve *ServiceFabric.clusterA.contoso.com* gMSA'daki makinelerin kümenin bir parçası olduğunu ve *CONTOSO\usera'nın* yönetici istemci erişimine sahip olduğunu belirtir:  
  
```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",
        "ClusterSPN" : "http/servicefabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
}
```
  
## <a name="configure-windows-security-using-a-machine-group"></a>Bir makine grubu kullanarak Windows güvenliğini yapılandırma  
Bu model küçümsüyor. Öneri yukarıda ayrıntılı olarak gMSA kullanmaktır. Microsoft.Azure.ServiceFabric.WindowsServer ile indirilen *clusterConfig.Windows.MultiMachine.JSON* yapılandırma dosyası [örnek.\< sürüm>.zip](https://go.microsoft.com/fwlink/?LinkId=730690) bağımsız küme paketi, Windows güvenliğini yapılandırmak için bir şablon içerir.  Windows güvenliği **Özellikler** bölümünde yapılandırılır: 

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
        "ClientIdentities": [{
            "Identity": "[domain\username]",
            "IsAdmin": true
        }]
    }
}
```

| **Yapılandırma ayarı** | **Açıklama** |
| --- | --- |
| ClusterCredentialType |Düğüm düğümü iletişimi için Windows güvenliğini etkinleştirmek için *Windows* olarak ayarlayın.  |
| ServerCredentialType |İstemci düğümü iletişimi için Windows güvenliğini etkinleştirmek için *Windows* olarak ayarlayın. |
| Windows Kimlikleri |Küme ve istemci kimliklerini içerir. |
| Kümekimliği |Düğümden düğüme güvenliği yapılandırmak için bir makine grubu adı, etki alanı\makine grubu kullanın. |
| Müşteri Kimlikleri |İstemciden düğüme güvenliği yapılandırır. Bir dizi istemci kullanıcı hesabı. |  
| Kimlik |İstemci kimliği için etki alanı kullanıcısını, etki alanı\kullanıcı adını ekleyin. |  
| IsAdmin |Etki alanı kullanıcısının yönetici istemci erişimine sahip olduğunu veya kullanıcı istemci erişimi için yanlış olduğunu belirtmek için doğru olarak ayarlayın. |  

[Bir](service-fabric-cluster-security.md#node-to-node-security) Etkin Dizin Etki Alanı içinde bir makine grubu kullanmak istiyorsanız, düğüm düğüm güvenliği **ClusterIdentity** kullanılarak ayarlayarak yapılandırılır. Daha fazla bilgi için active [directory'de Bir Makine Grubu Oluştur'a](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx)bakın.

[İstemciden düğüme güvenlik,](service-fabric-cluster-security.md#client-to-node-security) **İstemci Kimlikleri**kullanılarak yapılandırılır. İstemci ve küme arasında güven oluşturmak için kümenin güvenebileceği istemci kimliklerini bilecek şekilde kümeyi yapılandırmanız gerekir. Güveni iki farklı şekilde oluşturabilirsiniz:

- Bağlanabilecek etki alanı grubu kullanıcılarını belirtin.
- Bağlanabilen etki alanı düğümü kullanıcılarını belirtin.

Service Fabric, Service Fabric kümesine bağlı istemciler için iki farklı erişim denetim türünü destekler: yönetici ve kullanıcı. Erişim denetimi, küme yöneticisinin farklı kullanıcı grupları için belirli türdeki küme işlemlerine erişimi sınırlamasını sağlar ve bu da kümeyi daha güvenli hale getirir.  Yöneticiler yönetim yeteneklerine (okuma/yazma özellikleri dahil) tam erişime sahiptir. Kullanıcılar, varsayılan olarak, yalnızca yönetim yeteneklerine (örneğin, sorgu yetenekleri) ve uygulamaları ve hizmetleri çözümleme özelliğine erişimi okumuştur.  

Aşağıdaki örnek **güvenlik** bölümü Windows güvenliğini yapılandırır, *ServiceFabric/clusterA.contoso.com'daki* makinelerin kümenin bir parçası olduğunu belirtir ve *CONTOSO\usera'nın* yönetici istemci erişimine sahip olduğunu belirtir:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric bir etki alanı denetleyicisi üzerinde dağıtılmamalıdır. ClusterConfig.json'ın bir makine grubu veya grup Yönetilen Hizmet Hesabı (gMSA) kullanırken etki alanı denetleyicisinin IP adresini içermediğinden emin olun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
*ClusterConfig.JSON* dosyasında Windows güvenliğini yapılandırdıktan sonra, [Windows'da çalışan bağımsız bir küme oluştur'da](service-fabric-cluster-creation-for-windows-server.md)küme oluşturma işlemine devam edin.

Düğümden düğüme güvenlik, istemciden düğüme güvenlik ve rol tabanlı erişim denetimi hakkında daha fazla bilgi için [küme güvenlik senaryolarına](service-fabric-cluster-security.md)bakın.

PowerShell veya FabricClient kullanarak bağlanma örnekleri için [güvenli bir kümeye bağlan'a](service-fabric-connect-to-secure-cluster.md) bakın.
