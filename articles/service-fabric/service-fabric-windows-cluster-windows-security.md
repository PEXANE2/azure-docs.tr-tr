---
title: Windows güvenliğini kullanarak Windows üzerinde çalışan bir kümenin güvenliğini sağlama
description: Windows güvenliği kullanarak Windows üzerinde çalışan tek başına kümede düğümden düğüme ve istemciden düğüme güvenliği yapılandırmayı öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 46be6acc1ef08770826a2e020c8930eba0787791
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76774438"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Windows güvenliğini kullanarak Windows 'da tek başına kümeyi güvenli hale getirme
Service Fabric kümesine yetkisiz erişimi engellemek için, kümeyi güvenli hale getirin. Küme üretim iş yüklerini çalıştırdığında güvenlik özellikle önemlidir. Bu makalede, *clusterConfig. JSON* dosyasındaki Windows güvenliği kullanılarak düğümden düğüme ve istemciden düğüme güvenliğin nasıl yapılandırılacağı açıklanır.  İşlem, [Windows üzerinde çalışan tek başına küme oluşturma](service-fabric-cluster-creation-for-windows-server.md)güvenlik adımını yapılandırma adımına karşılık gelir. Service Fabric Windows güvenliğini nasıl kullandığı hakkında daha fazla bilgi için bkz. [küme güvenliği senaryoları](service-fabric-cluster-security.md).

> [!NOTE]
> Tek bir güvenlik seçiminden diğerine küme yükseltmesi olmadığından, düğümden düğüme güvenliği dikkatlice düşünmeniz gerekir. Güvenlik seçimini değiştirmek için, tam kümeyi yeniden oluşturmanız gerekir.
>
>

## <a name="configure-windows-security-using-gmsa"></a>GMSA kullanarak Windows güvenliğini yapılandırma  
Microsoft. Azure. ServiceFabric. WindowsServer ile indirilen örnek *clusterConfig. gMSA. Windows. MultiMachine. JSON* yapılandırma dosyası [.\< sürüm>. zip](https://go.microsoft.com/fwlink/?LinkId=730690) tek başına küme paketi, [Grup yönetilen hizmet hesabı (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)kullanarak Windows güvenliğini yapılandırmak için bir şablon içerir:  

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
| ClusterCredentialType |Düğüm düğümü iletişimi için Windows güvenliği 'ni etkinleştirmek üzere *Windows* 'a ayarlayın.  | 
| ServerCredentialType |İstemci düğümü iletişimi için Windows güvenliğini etkinleştirmek üzere *Windows* 'a ayarlayın. |
| Windowsıdentities |Küme ve istemci kimliklerini içerir. |
| Clustergmsakimliği |Düğümden düğüme güvenliği yapılandırır. Grup tarafından yönetilen hizmet hesabı. |
| Kümespn |GMSA hesabı için SPN kaydettirildi|
| Clienentidentities |İstemciden düğüme güvenliği yapılandırır. İstemci kullanıcı hesapları dizisi. |
| Kimlik |İstemci kimliği için etki alanı \ Kullanıcı adı ' nı ekleyin. |
| IsAdmin |Etki alanı kullanıcısının yönetici istemci erişimi olduğunu veya Kullanıcı istemci erişimi için yanlış değerini belirtmek için true olarak ayarlayın. |

> [!NOTE]
> Clustergmsaıdentity değeri "mysfgmsa@mydomain" biçiminde olmalıdır.

Node [-node güvenliğine](service-fabric-cluster-security.md#node-to-node-security) , Service Fabric 'In gMSA altında çalışması gerektiğinde **Clustergmsaıdentity** ayarlanarak yapılandırılır. Düğümler arasında güven ilişkisi oluşturmak için bunların birbirleriyle uyumlu hale getirilmeleri gerekir. Bu iki farklı şekilde gerçekleştirilebilir: kümedeki tüm düğümleri içeren grup tarafından yönetilen hizmet hesabını belirtin veya kümedeki tüm düğümleri içeren etki alanı makine grubunu belirtin. [Grup yönetilen hizmet hesabı (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) yaklaşımını özellikle daha büyük kümeler (10 ' dan fazla düğüm) için veya büyümekte veya küçülebilecek kümeler için kullanmanızı önemle öneririz.  
Bu yaklaşım, küme yöneticilerine üye eklemek ve kaldırmak için erişim hakları verilen bir etki alanı grubu oluşturulmasını gerektirmez. Bu hesaplar otomatik parola yönetimi için de kullanışlıdır. Daha fazla bilgi için bkz. [Grup tarafından yönetilen hizmet hesapları Ile çalışmaya](https://technet.microsoft.com/library/jj128431.aspx)başlama.  
 
[İstemciden düğüme güvenliği](service-fabric-cluster-security.md#client-to-node-security) , **Clienentidentities**kullanılarak yapılandırılır. Bir istemciyle küme arasında güven sağlamak için, kümeyi güveneceği istemci kimliklerini bilmek üzere yapılandırmanız gerekir. Bu iki farklı şekilde yapılabilir: bağlanabilecek etki alanı grubu kullanıcılarını veya bağlanabilecek etki alanı düğüm kullanıcılarını belirtebilir. Service Fabric, bir Service Fabric kümesine bağlı istemciler için iki farklı erişim denetimi türünü destekler: yönetici ve Kullanıcı. Erişim denetimi, küme yöneticisinin farklı Kullanıcı grupları için belirli türdeki küme işlemlerine erişimi sınırlayabilmesini ve kümenin daha güvenli olmasını sağlar.  Yöneticiler, yönetim özelliklerine (okuma/yazma özellikleri dahil) tam erişime sahiptir. Varsayılan olarak kullanıcılar, yönetim özelliklerine (örneğin, sorgu özellikleri) yalnızca okuma erişimine ve uygulama ve Hizmetleri çözme imkanına sahiptir. Erişim denetimleri hakkında daha fazla bilgi için bkz. [Service Fabric istemcileri Için rol tabanlı erişim denetimi](service-fabric-cluster-security-roles.md).  
 
Aşağıdaki örnek **güvenlik** bölümü, gMSA kullanarak Windows güvenliğini yapılandırır ve *ServiceFabric.clusterA.contoso.com* gMSA 'daki makinelerin kümenin bir parçası olduğunu ve *contoso\usera* 'nın yönetici istemci erişimi olduğunu belirtir:  
  
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
  
## <a name="configure-windows-security-using-a-machine-group"></a>Makine grubunu kullanarak Windows güvenliğini yapılandırma  
Bu model kullanım dışı bırakılıyor. Bu öneri, yukarıda ayrıntılı olarak gMSA 'yı kullanmaktır. Microsoft. Azure. ServiceFabric. WindowsServer ile indirilen örnek *clusterConfig. Windows. MultiMachine. JSON* yapılandırma dosyası [.\< sürüm>. zip](https://go.microsoft.com/fwlink/?LinkId=730690) tek başına küme paketi, Windows güvenliğini yapılandırmak için bir şablon içerir.  Windows güvenliği **Özellikler** bölümünde yapılandırılır: 

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
| ClusterCredentialType |Düğüm düğümü iletişimi için Windows güvenliği 'ni etkinleştirmek üzere *Windows* 'a ayarlayın.  |
| ServerCredentialType |İstemci düğümü iletişimi için Windows güvenliğini etkinleştirmek üzere *Windows* 'a ayarlayın. |
| Windowsıdentities |Küme ve istemci kimliklerini içerir. |
| Clusterıdentity |Düğümden düğüme güvenliği yapılandırmak için, domain\machinegroup adlı bir makine grubu adı kullanın. |
| Clienentidentities |İstemciden düğüme güvenliği yapılandırır. İstemci kullanıcı hesapları dizisi. |  
| Kimlik |İstemci kimliği için etki alanı \ Kullanıcı adı ' nı ekleyin. |  
| IsAdmin |Etki alanı kullanıcısının yönetici istemci erişimi olduğunu veya Kullanıcı istemci erişimi için yanlış değerini belirtmek için true olarak ayarlayın. |  

Düğüm, düğüm [güvenliği](service-fabric-cluster-security.md#node-to-node-security) Active Directory etki alanı içinde bir makine grubu kullanmak Istiyorsanız **clusterıdentity** kullanılarak yapılandırılır. Daha fazla bilgi için bkz. [Active Directory makine grubu oluşturma](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[İstemci-düğüm güvenliği](service-fabric-cluster-security.md#client-to-node-security) , **Clienentidentities**kullanılarak yapılandırılır. Bir istemciyle küme arasında güven oluşturmak için, kümeyi, kümenin güvenebileceği istemci kimliklerini bildirmek üzere yapılandırmanız gerekir. Güveni iki farklı yolla oluşturabilirsiniz:

- Bağlanabilecek etki alanı grubu kullanıcılarını belirtin.
- Bağlanabilecek etki alanı düğümü kullanıcılarını belirtin.

Service Fabric, bir Service Fabric kümesine bağlı istemciler için iki farklı erişim denetimi türünü destekler: yönetici ve Kullanıcı. Erişim denetimi, küme yöneticisinin farklı Kullanıcı grupları için belirli türdeki küme işlemlerine erişimi sınırlandırmasına olanak sağlar ve bu da kümeyi daha güvenli hale getirir.  Yöneticiler, yönetim özelliklerine (okuma/yazma özellikleri dahil) tam erişime sahiptir. Varsayılan olarak kullanıcılar, yönetim özelliklerine (örneğin, sorgu özellikleri) yalnızca okuma erişimine ve uygulama ve Hizmetleri çözme imkanına sahiptir.  

Aşağıdaki örnek **güvenlik** bölümünde Windows güvenliği yapılandırılır, *Servicefabric/ClusterA. contoso. com* ' daki makinelerin kümenin bir parçası olduğunu ve *contoso\usera* 'nın yönetici istemcisi erişiminin olduğunu belirtir:

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
> Service Fabric bir etki alanı denetleyicisine dağıtılmamalıdır. ClusterConfig. json ' ın bir makine grubu veya grup yönetilen hizmet hesabı (gMSA) kullanırken etki alanı denetleyicisinin IP adresini içermediğinden emin olun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
*ClusterConfig. JSON* dosyasında Windows güvenliği yapılandırıldıktan sonra, [Windows üzerinde çalışan tek başına küme oluşturma](service-fabric-cluster-creation-for-windows-server.md)' da küme oluşturma işlemini sürdürür.

Düğümden düğüme güvenliği, istemciden düğüme güvenliği ve rol tabanlı erişim denetimi hakkında daha fazla bilgi için bkz. [küme güvenliği senaryoları](service-fabric-cluster-security.md).

Bkz. PowerShell veya FabricClient kullanarak bağlanma örnekleri için [güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md) .
