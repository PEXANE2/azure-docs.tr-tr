---
title: Kullanılabilirlik grubunu yapılandırma (Azure hızlı başlatma şablonu)
description: Windows başarısız kümesini oluşturmak, SQL Server VM'leri kümeye katılmak, dinleyiciyi oluşturmak ve Azure'da dahili yük bakiyesini yapılandırmak için Azure hızlı başlatma şablonlarını kullanın.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/04/2019
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: edf810dfc975eebaf261eac7b89106c9e29c759c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022376"
---
# <a name="use-azure-quickstart-templates-to-configure-an-availability-group-for-sql-server-on-an-azure-vm"></a>Sql Server için kullanılabilirlik grubunu Azure VM'de yapılandırmak için Azure hızlı başlatma şablonlarını kullanma
Bu makalede, Azure'daki SQL Server sanal makineleri için Her Zaman Kullanılabilirlik grubu yapılandırmasının dağıtımını kısmen otomatikleştirmek için Azure hızlı başlatma şablonlarının nasıl kullanılacağı açıklanmaktadır. Bu işlemde iki Azure hızlı başlatma şablonu kullanılır: 

   | Şablon | Açıklama |
   | --- | --- |
   | [101-sql-vm-ag-kurulumu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Windows failover kümesini oluşturur ve SQL Server VM'leri ona katılır. |
   | [101-sql-vm-aglistener-kurulumu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | Kullanılabilirlik grubu dinleyicisini oluşturur ve dahili yük dengeleyicisini yapılandırır. Bu şablon yalnızca Windows failover kümesi **101-sql-vm-ag-setup** şablonuyla oluşturulduğunda kullanılabilir. |
   | &nbsp; | &nbsp; |

Kullanılabilirlik grubu yapılandırmasının kullanılabilirlik grubu yapılandırmasının kullanılabilirlik grubunu oluşturma ve dahili yük dengeleyicisini oluşturma gibi diğer bölümleri el ile yapılmalıdır. Bu makalede, otomatik ve el ile adımlar dizisi sağlar.
 

## <a name="prerequisites"></a>Ön koşullar 
Hızlı başlatma şablonlarını kullanarak her zaman kullanılabilirlik grubunun kurulumlarını otomatikleştirmek için aşağıdaki ön koşullara sahip olmalısınız: 
- [Azure aboneliği.](https://azure.microsoft.com/free/)
- Etki alanı denetleyicisi olan bir kaynak grubu. 
- Aynı kullanılabilirlik kümesinde veya kullanılabilirlik bölgesinde bulunan ve [SQL VM kaynak sağlayıcısına kayıtlı](virtual-machines-windows-sql-register-with-resource-provider.md)olan SQL Server [2016 (veya daha sonra) Enterprise sürümünü çalıştıran Azure'da](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) bir veya daha fazla etki alanı birleştirilmiş VM'ler.  
- Kullanılabilir iki (herhangi bir varlık tarafından kullanılmaz) IP adresi: biri dahili yük dengeleyicisi için, diğeri de kullanılabilirlik grubuyla aynı alt ağdaki kullanılabilirlik grubu dinleyicisi için. Varolan bir yük dengeleyici kullanılıyorsa, yalnızca bir kullanılabilir IP adresine ihtiyacınız vardır.  

## <a name="permissions"></a>İzinler
Azure hızlı başlatma şablonlarını kullanarak Her Zaman kullanılabilirlik grubunu yapılandırmak için aşağıdaki izinler gereklidir: 

- Etki alanında **Bilgisayar Nesnesi Oluşturma** izni olan varolan bir etki alanı kullanıcı hesabı.  Örneğin, bir etki alanı yöneticisi hesabı genellikle yeterli account@domain.comizne sahiptir (örneğin: ). _Bu hesap, küme oluşturmak için her VM'deki yerel yönetici grubunun da bir parçası olmalıdır._
- SQL Server hizmetini kontrol eden etki alanı kullanıcı hesabı. 


## <a name="step-1-create-the-failover-cluster-and-join-sql-server-vms-to-the-cluster-by-using-a-quickstart-template"></a>Adım 1: Failover kümesini oluşturun ve hızlı başlangıç şablonu kullanarak SQL Server VM'leri kümeye katılın 
SQL Server VM'leriniz SQL VM kaynak sağlayıcısına kaydolduktan sonra SQL Server VM'lerinizi *SqlVirtualMachineGroups'a*katılabilirsiniz. Bu kaynak, Windows failover kümesinin meta verilerini tanımlar. Meta veriler, hem kümeyi hem de SQL Server hizmetini yönetmek için sürüm, sürüm, tam nitelikli alan adı, Active Directory hesaplarını ve bulut tanığı olarak depolama hesabını içerir. 

*SQLVirtualMachineGroups* kaynak grubuna SQL Server VM'ler eklemek, küme oluşturmak için Windows Failover Cluster Service'i yakalar ve bu kümeye bu SQL Server VM'leri birleştirir. Bu adım **101-sql-vm-ag-setup** quickstart şablonu ile otomatikleştirilir. Aşağıdaki adımları kullanarak uygulayabilirsiniz:

1. [**101-sql-vm-ag-setup**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) quickstart şablonuna gidin. Ardından, Azure portalındaki hızlı başlangıç şablonuna açmak için **Azure'a Dağıt'ı** seçin.
1. Windows failover kümesi için meta verileri yapılandırmak için gerekli alanları doldurun. İsteğe bağlı alanları boş bırakabilirsiniz.

   Aşağıdaki tablo şablon için gerekli değerleri gösterir: 

   | **Alan** | Değer |
   | --- | --- |
   | **Abonelik** |  SQL Server VM'lerinizin bulunduğu abonelik. |
   |**Kaynak grubu** | SQL Server VM'lerinizin bulunduğu kaynak grubu. | 
   |**Failover Küme Adı** | Yeni Windows başarısız kümeniz için istediğiniz ad. |
   | **Mevcut Vm Listesi** | Kullanılabilirlik grubuna katılmak ve bu yeni kümenin bir parçası olmak istediğiniz SQL Server VM'leri. Bu değerleri virgül ve boşlukla ayırın (örneğin: *SQLVM1, SQLVM2).* |
   | **SQL Server Sürümü** | SQL Server VM'lerinizin SQL Server sürümü. Açılan listeden seçin. Şu anda yalnızca SQL Server 2016 ve SQL Server 2017 görüntüleri desteklenmiştir. |
   | **Mevcut Tam Nitelikli Alan Adı** | SQL Server VM'lerinizin bulunduğu etki alanı için varolan FQDN. |
   | **Mevcut Etki Alanı Hesabı** | Şablon dağıtımı sırasında [CNO](/windows-server/failover-clustering/prestage-cluster-adds) oluşturulurken etki alanında **Bilgisayar Nesnesi Oluşturma** iznine sahip varolan bir etki alanı kullanıcı hesabı. Örneğin, bir etki alanı yöneticisi hesabı genellikle yeterli account@domain.comizne sahiptir (örneğin: ). *Bu hesap, küme oluşturmak için her VM'deki yerel yönetici grubunun da bir parçası olmalıdır.*| 
   | **Etki Alanı Hesap Şifresi** | Daha önce bahsedilen etki alanı kullanıcı hesabının parolası. | 
   | **Mevcut Sql Hizmet Hesabı** | Kullanılabilirlik grubu dağıtımı sırasında [SQL Server hizmetini](/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions) kontrol eden account@domain.cometki alanı kullanıcı hesabı (örneğin: ). |
   | **Sql Servis Şifresi** | SQL Server hizmetini kontrol eden etki alanı kullanıcı hesabı tarafından kullanılan parola. |
   | **Bulut Tanık Adı** | Bulut tanığı için oluşturulacak ve kullanılacak yeni bir Azure depolama hesabı. Bu adı değiştirebilirsiniz. |
   | **\_eserler Yer** | Bu alan varsayılan olarak ayarlanır ve değiştirilmemelidir. |
   | **\_eserler Yer Sas Belirteci** | Bu alan kasıtlı olarak boş bırakılır. |
   | &nbsp; | &nbsp; |

1. Hüküm ve koşulları kabul ederseniz, **onay kutusunun üzerinde belirtilen hüküm ve koşulları Kabul** Edin'i seçin. Ardından, hızlı başlangıç şablonunun dağıtımını tamamlamak için **Satın Al'ı** seçin. 
1. Dağıtımınızı izlemek için, en üst gezinti başlığındaki **Bildirimler** zili simgesinden dağıtımı seçin veya Azure portalındaki **Kaynak Grubu'na** gidin. **Ayarlar**altında **Dağıtımlar'ı** seçin ve **Microsoft.Template** dağıtımını seçin. 

>[!NOTE]
> Şablon dağıtımı sırasında sağlanan kimlik bilgileri yalnızca dağıtım uzunluğu için depolanır. Dağıtım bittikten sonra, bu parolalar kaldırılır. Kümeye daha fazla SQL Server VM eklerseniz bunları yeniden sağlamanız istenir. 


## <a name="step-2-manually-create-the-availability-group"></a>Adım 2: Kullanılabilirlik grubunu el ile oluşturma 
[SQL Server Management Studio,](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio) [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell)veya [Transact-SQL'i](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql)kullanarak, normalde yaptığınız gibi kullanılabilirlik grubunu el ile oluşturun. 

>[!IMPORTANT]
> **101-sql-vm-aglistener-setup** quickstart şablonu adım 4'te otomatik olarak yaptığından, şu anda bir dinleyici *oluşturmayın.* 

## <a name="step-3-manually-create-the-internal-load-balancer"></a>Adım 3: Dahili yük dengeleyicisini el ile oluşturun
Always On kullanılabilirlik grubu dinleyicisi, Azure Yük Bakiyesi'nin dahili bir örneğini gerektirir. Dahili yük dengeleyicisi, kullanılabilirlik grubu dinleyicisi için daha hızlı arıza ve yeniden bağlantı sağlayan bir "kayan" IP adresi sağlar. Kullanılabilirlik grubundaki SQL Server VM'leri aynı kullanılabilirlik kümesinin bir parçasıysa, Temel yük dengeleyicisi kullanabilirsiniz. Aksi takdirde, standart yük dengeleyici kullanmanız gerekir. 

> [!IMPORTANT]
> Dahili yük dengeleyicisi SQL Server VM örnekleri ile aynı sanal ağda olmalıdır. 

Sadece iç yük dengeleyici oluşturmanız gerekir. Adım 4'te, **101-sql-vm-aglistener-setup** quickstart şablonu yapılandırmanın geri kalanını (arka uç havuzu, sistem durumu sondası ve yük dengeleme kuralları gibi) işler. 

1. Azure portalında, SQL Server sanal makinelerini içeren kaynak grubunu açın. 
2. Kaynak grubunda **Ekle'yi**seçin.
3. Yük **dengeleyicisi**arayın. Arama sonuçlarında, **Microsoft**tarafından yayınlanan **Yük Dengeleyici'ni**seçin.
4. Yük **Dengeleyici** silik üzerinde **Oluştur'u**seçin.
5. Yük **bakiyesi oluştur** iletişim kutusunda, yük bakiyesini aşağıdaki gibi yapılandırın:

   | Ayar | Değer |
   | --- | --- |
   | **Adı** |Yük bakiyesini temsil eden bir metin adı girin. Örneğin, **sqlLB**girin. |
   | **Tür** |**Dahili**: Çoğu uygulama, aynı sanal ağ daki uygulamaların kullanılabilirlik grubuna bağlanmasını sağlayan bir dahili yük dengeleyicisi kullanır.  </br> **Harici**: Uygulamaların genel internet bağlantısı üzerinden kullanılabilirlik grubuna bağlanmasını sağlar. |
   | **Sanal ağ** | SQL Server örneklerinin içinde olduğu sanal ağı seçin. |
   | **Alt ağ** | SQL Server örneklerinin içinde olduğu alt ağı seçin. |
   | **IP adresi ataması** |**Statik** |
   | **Özel IP adresi** | Alt ağdan kullanılabilir bir IP adresi belirtin. |
   | **Abonelik** |Birden çok aboneliğiniz varsa, bu alan görünebilir. Bu kaynakla ilişkilendirmek istediğiniz aboneliği seçin. Normalde kullanılabilirlik grubu için tüm kaynaklarla aynı abonelik. |
   | **Kaynak grubu** |SQL Server örneklerinin içinde olduğu kaynak grubunu seçin. |
   | **Konum** |SQL Server örneklerinin bulunduğu Azure konumunu seçin. |
   | &nbsp; | &nbsp; |

6. **Oluştur'u**seçin. 


>[!IMPORTANT]
> Her SQL Server VM için ortak IP kaynağının Standart yük dengeleyicisiyle uyumlu olması için bir Standart SKU'su olmalıdır. VM'nizin genel IP kaynağının SKU'unu belirlemek için **Kaynak Grubu'na**gidin, SQL Server VM için **Ortak IP Adresi** kaynağınızı seçin ve Genel **Bakış** bölmesinde **SKU** altındaki değeri bulun. 

## <a name="step-4-create-the-availability-group-listener-and-configure-the-internal-load-balancer-by-using-the-quickstart-template"></a>Adım 4: Kullanılabilirlik grubu dinleyicisini oluşturun ve quickstart şablonu kullanarak dahili yük dengeleyicisini yapılandırın

Kullanılabilirlik grubu dinleyicisini oluşturun ve **101-sql-vm-aglistener-setup** quickstart şablonu kullanarak dahili yük bakiyesini otomatik olarak yapılandırın. Şablon, Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/AvailabilityGroupListener kaynağını karşılar. **101-sql-vm-aglistener-setup** quickstart şablonu, SQL VM kaynak sağlayıcısı aracılığıyla aşağıdaki işlemleri yapar:

- Dinleyici için yeni bir önuç IP kaynağı (dağıtım sırasında sağlanan IP adresi değerine göre) oluşturur. 
- Küme ve dahili yük dengeleyicisi için ağ ayarlarını yapılandırır. 
- İç yük dengeleyicisi, sistem durumu sondası ve yük dengeleme kuralları için arka uç havuzunu yapılandırır.
- Verilen IP adresi ve adı ile kullanılabilirlik grubu dinleyicioluşturur.
 
>[!NOTE]
> **101-sql-vm-aglistener-setup'ı** ancak Windows failover kümesi **101-sql-vm-ag-setup** şablonu yla oluşturulmuşsa kullanabilirsiniz.
   
   
Dahili yük dengeleyicisini yapılandırmak ve kullanılabilirlik grubu dinleyicisini oluşturmak için aşağıdakileri yapın:
1. [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) hızlı başlat şablonuna gidin ve Azure portalında hızlı başlangıç şablonuna başlamak için **Azure'a Dağıt'ı** seçin.
1. İç yük dengeleyicisini yapılandırmak ve kullanılabilirlik grubu dinleyicisini oluşturmak için gerekli alanları doldurun. İsteğe bağlı alanları boş bırakabilirsiniz. 

   Aşağıdaki tablo şablon için gerekli değerleri gösterir: 

   | **Alan** | Değer |
   | --- | --- |
   |**Kaynak grubu** | SQL Server VM'lerinizin ve kullanılabilirlik grubunuzun bulunduğu kaynak grubu. | 
   |**Varolan Failover Küme Adı** | SQL Server VM'lerinizin birleştiği kümenin adı. |
   | **Varolan Sql Kullanılabilirlik Grubu**| SQL Server VM'lerinizin parçası olduğu kullanılabilirlik grubunun adı. |
   | **Mevcut Vm Listesi** | Daha önce bahsedilen kullanılabilirlik grubunun bir parçası olan SQL Server VM'lerinin adları. Adları virgül ve boşlukla ayırın (örneğin: *SQLVM1, SQLVM2).* |
   | **Dinleyici** | Dinleyiciye atamak istediğiniz DNS adı. Varsayılan olarak, bu şablon "aglistener" adını belirtir, ancak değiştirebilirsiniz. Ad 15 karakteri geçmemelidir. |
   | **Dinleyici Bağlantı Noktası** | Dinleyicinin kullanmasını istediğiniz bağlantı noktası. Genellikle, bu bağlantı noktası 1433 varsayılan olmalıdır. Bu, şablonun belirttiği bağlantı noktası numarasıdır. Ancak varsayılan bağlantı noktası değiştirildiyse, dinleyici bağlantı noktası bunun yerine bu değeri kullanmalıdır. | 
   | **Dinleyici IP** | Dinleyicinin kullanmasını istediğiniz IP adresi. Bu adres şablon dağıtımı sırasında oluşturulacaktır, bu nedenle zaten kullanılmamış bir adres sağlayın.  |
   | **Varolan Alt Ağ** | SQL Server VM'lerinizin dahili alt netinin adı (örneğin: *varsayılan).* Bu değeri **Kaynak Grubu'na**giderek, sanal ağınızı seçerek, **Ayarlar** bölmesinde **Alt Ağlar'ı** seçerek ve **Değeri Ad**altında kopyalayarak belirleyebilirsiniz. |
   | **Mevcut Dahili Yük Dengeleyici** | Adım 3'te oluşturduğunuz dahili yük dengeleyicisinin adı. |
   | **Sonda Bağlantı Noktası** | İç yük dengeleyicisinin kullanmasını istediğiniz prob bağlantı noktası. Şablon varsayılan olarak 59999 kullanır, ancak bu değeri değiştirebilirsiniz. |
   | &nbsp; | &nbsp; |

1. Hüküm ve koşulları kabul ederseniz, **onay kutusunun üzerinde belirtilen hüküm ve koşulları Kabul** Edin'i seçin. Hızlı başlangıç şablonunun dağıtımını tamamlamak için **Satın Alma'yı** seçin. 
1. Dağıtımınızı izlemek için, en üst gezinti başlığındaki **Bildirimler** zili simgesinden dağıtımı seçin veya Azure portalındaki **Kaynak Grubu'na** gidin. **Ayarlar**altında **Dağıtımlar'ı** seçin ve **Microsoft.Template** dağıtımını seçin. 

>[!NOTE]
>Dağıtımınız yarı yolda başarısız olursa, **101-sql-vm-aglistener-setup** quickstart şablonunu yeniden dağıtmadan önce PowerShell'i kullanarak [yeni oluşturulan dinleyiciyi](#remove-the-availability-group-listener) el ile kaldırmanız gerekir. 

## <a name="remove-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisini kaldırma
Daha sonra şablonun yapılandırDığı kullanılabilirlik grubu dinleyicisini kaldırmanız gerekiyorsa, SQL VM kaynak sağlayıcısından geçmeniz gerekir. Dinleyici SQL VM kaynak sağlayıcısı üzerinden kayıtlı olduğundan, sql server management studio üzerinden silmesi yeterli değildir. 

En iyi yöntem, PowerShell'de aşağıdaki kod parçacıklarını kullanarak SQL VM kaynak sağlayıcısı aracılığıyla silmektir. Bunu yapmak, kullanılabilirlik grubu dinleyici meta verilerini SQL VM kaynak sağlayıcısından kaldırır. Ayrıca, dinleyiciyi kullanılabilirlik grubundan fiziksel olarak siler. 

```PowerShell
# Remove the availability group listener
# example: Remove-AzResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="common-errors"></a>Sık karşılaşılan hatalar
Bu bölümde bilinen bazı sorunlar ve bunların olası çözümü tartışılmaktadır. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>Kullanılabilirlik grubu 'AG-Name\<>' için kullanılabilirlik grubu dinleyicisi zaten var
Kullanılabilirlik grubu dinleyicisi için Azure hızlı başlatma şablonunda kullanılan seçili kullanılabilirlik grubu zaten bir dinleyici içerir. Fiziksel olarak kullanılabilirlik grubundadır veya meta verileri SQL VM kaynak sağlayıcısında kalır. **101-sql-vm-aglistener-setup** quickstart şablonuna yeniden dağıtmadan önce [PowerShell'i](#remove-the-availability-group-listener) kullanarak dinleyiciyi çıkarın. 

### <a name="connection-only-works-from-primary-replica"></a>Bağlantı yalnızca birincil yinelemeden çalışır
Bu davranış, iç yük dengeleyicisinin yapılandırmasını tutarsız bir durumda bırakan başarısız bir **101-sql-vm-aglistener-kurulum** şablonu dağıtımından büyük olasılıkla gelir. Arka uç havuzunun kullanılabilirlik kümesini listelediğini ve sistem durumu sondası ve yük dengeleme kuralları için kuralların mevcut olduğunu doğrulayın. Bir şey eksikse, iç yük dengeleyicisinin yapılandırması tutarsız bir durumdur. 

Bu davranışı gidermek için [PowerShell'i](#remove-the-availability-group-listener)kullanarak dinleyiciyi çıkarın, Azure portalı üzerinden dahili yük bakiyesini silin ve 3. 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - Sadece SQL sanal makine listesi güncellenebilir
Dinleyici SQL Server Management Studio (SSMS) aracılığıyla silinmişse, ancak SQL VM kaynak sağlayıcısından silinmemişse, bu hata **101-sql-vm-aglistener-setup** şablonuna dağıtırken oluşabilir. Dinleyicinin SSMS ile silmesi, dinleyicinin meta verilerini SQL VM kaynak sağlayıcısından kaldırmaz. Dinleyici [PowerShell](#remove-the-availability-group-listener)aracılığıyla kaynak sağlayıcıdan silinmelidir. 

### <a name="domain-account-does-not-exist"></a>Etki alanı hesabı yok
Bu hatanın iki nedeni olabilir. Belirtilen etki alanı hesabı yok veya [Kullanıcı Anaadı (UPN)](/windows/desktop/ad/naming-properties#userprincipalname) verileri eksik. **101-sql-vm-ag-setup** şablonu UPN formunda bir etki alanı *user@domain.com*hesabı bekler (diğer bir deyişle), ancak bazı etki alanı hesapları eksik olabilir. Bu genellikle, sunucu bir etki alanı denetleyicisine yükseltildiğinde veya powershell aracılığıyla bir kullanıcı oluşturulduğunda, yerel bir kullanıcı ilk etki alanı yöneticisi hesabı olarak geçirildiğinde gerçekleşir. 

Hesabın var olduğunu doğrulayın. Eğer olursa, ikinci bir durumla karşı karşıya kalabilirsiniz. Bunu çözmek için aşağıdakileri yapın:

1. Etki alanı denetleyicisinde, **Server Manager'daki** **Araçlar** seçeneğinden **Etkin Dizin Kullanıcıları ve Bilgisayarlar** penceresini açın. 
2. Sol bölmedeki **Kullanıcıları** seçerek hesaba gidin.
3. Hesabı sağ tıklatın ve **Özellikler'i**seçin.
4. **Hesap** sekmesini seçin. Kullanıcı **oturum açma adı** kutusu boşsa, hatanızın nedeni budur. 

    ![Boş kullanıcı hesabı eksik UPN'yi gösterir](media/virtual-machines-windows-sql-availability-group-quickstart-template/account-missing-upn.png)

5. **Kullanıcının** adını eşleştirmek için Kullanıcı oturum açma adı kutusunu doldurun ve açılan listeden uygun etki alanını seçin. 
6. Değişikliklerinizi kaydetmek için **Uygula'yı** seçin ve **Tamam'ı**seçerek iletişim kutusunu kapatın. 

Bu değişiklikleri yaptıktan sonra Azure hızlı başlatma şablonuna bir kez daha dağıtmayı deneyin. 



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [SQL Server VM'lere Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server VM'ler için SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server VM'ler için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server VM'leri için sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [SQL Server VM için lisans lama modellerini değiştirme](virtual-machines-windows-sql-ahb.md)



