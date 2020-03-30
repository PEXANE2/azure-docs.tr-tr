---
title: Windows Sanal Masaüstü ana bilgisayar havuzu PowerShell oluşturma - Azure
description: PowerShell cmdlets ile Windows Sanal Masaüstü'nde bir ana bilgisayar havuzu oluşturmak için nasıl.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: b390c0beb20b7557294c18f889a0f41023513e2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246968"
---
# <a name="create-a-host-pool-with-powershell"></a>PowerShell ile ana bilgisayar havuzu oluşturma

Ana bilgisayar havuzları, Windows Sanal Masaüstü kiracı ortamlarında bir veya daha fazla aynı sanal makineden oluşan bir koleksiyondur. Her ana bilgisayar havuzu, kullanıcıların fiziksel bir masaüstünde olduğu gibi etkileşim kurabileceği bir uygulama grubu içerebilir.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>Ana bilgisayar havuzu oluşturmak için PowerShell istemcinizi kullanın

İlk olarak, PowerShell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/)

Windows Sanal Masaüstü ortamında oturum açabilmek için aşağıdaki cmdlet'i çalıştırın

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Ardından, Windows Sanal Masaüstü kiracınızda yeni bir ana bilgisayar havuzu oluşturmak için bu cmdlet'i çalıştırın:

```powershell
New-RdsHostPool -TenantName <tenantname> -Name <hostpoolname>
```

Bir oturum ana bilgisayarını ana bilgisayara katılmaya ve yerel bilgisayarınızdaki yeni bir dosyaya kaydetmek için yetkilendirmek için bir kayıt belirteci oluşturmak için bir sonraki cmdlet'i çalıştırın. -ExpirationHours parametresini kullanarak kayıt belirtecinin ne kadar süre geçerli olduğunu belirtebilirsiniz.

```powershell
New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours <number of hours> | Select-Object -ExpandProperty Token | Out-File -FilePath <PathToRegFile>
```

Bundan sonra, azure active directory kullanıcılarını ana bilgisayar havuzu için varsayılan masaüstü uygulama grubuna eklemek için bu cmdlet'i çalıştırın.

```powershell
Add-RdsAppGroupUser -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName "Desktop Application Group" -UserPrincipalName <userupn>
```

**Add-RdsAppGroupUser** cmdlet güvenlik grupları eklemeyi desteklemez ve uygulama grubuna aynı anda yalnızca bir kullanıcı ekler. Uygulama grubuna birden çok kullanıcı eklemek istiyorsanız, cmdlet'i uygun kullanıcı anaadı adlarıyla yeniden çalıştırın.

Daha sonra sanal [makineleri Windows Sanal Masaüstü ana bilgisayar havuzuna kaydet'te](#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool)kullanacağınız kayıt belirteci'ni bir değişkene aktarmak için aşağıdaki cmdlet'i çalıştırın.

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Ana bilgisayar havuzu için sanal makineler oluşturma

Artık Windows Sanal Masaüstü ana bilgisayar havuzunuza katılabilen bir Azure sanal makinesi oluşturabilirsiniz.

Sanal bir makineyi birden çok şekilde oluşturabilirsiniz:

- [Azure Galerisi görüntüsünden sanal makine oluşturma](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Yönetilen görüntüden sanal makine oluşturma](../virtual-machines/windows/create-vm-generalized-managed.md)
- [Yönetilmeyen bir görüntüden sanal makine oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-user-image-data-disks)

>[!NOTE]
>Ana bilgisayar işletim sistemi olarak Windows 7'yi kullanarak sanal bir makine dağıtıyorsanız, oluşturma ve dağıtım işlemi biraz farklı olacaktır. Daha fazla bilgi için Windows [Sanal Masaüstü'nde Windows 7 sanal makine dağıtın'](deploy-windows-7-virtual-machine.md)a bakın.

Oturum ana bilgisayar sanal makinelerinizi oluşturduktan sonra, windows veya Windows Server sanal makinelerinizi başka bir lisans için ödeme yapmadan çalıştırmak için [oturum ana bilgisayar VM'sine](./apply-windows-license.md#apply-a-windows-license-to-a-session-host-vm) Windows lisansı uygulayın. 

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-agent-installations"></a>Windows Sanal Masaüstü aracı yüklemeleri için sanal makineleri hazırlama

Windows Sanal Masaüstü aracılarını yüklemeden ve sanal makineleri Windows Sanal Masaüstü ana bilgisayar havuzuna kaydettirmeden önce sanal makinelerinizi hazırlamak için aşağıdaki leri yapmanız gerekir:

- Makineye etki alanına katılmanız gerekir. Bu, gelen Windows Sanal Masaüstü kullanıcılarının Azure Active Directory hesaplarından Active Directory hesabına eşlenmelerine ve sanal makineye başarıyla erişmelerine izin verilmesine olanak tanır.
- Sanal makine windows server işletim sistemi çalıştırıyorsa Uzak Masaüstü Oturum Ana Bilgisayar (RDSH) rolünü yüklemeniz gerekir. RDSH rolü, Windows Sanal Masaüstü aracılarını düzgün bir şekilde yüklemesini sağlar.

Etki alanına başarılı bir şekilde katılmak için her sanal makinede aşağıdaki şeyleri yapın:

1. [Sanal makineoluştururken](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) sağladığınız kimlik bilgileriyle sanal makineye bağlanın.
2. Sanal makinede, **Kontrol Paneli'ni** başlatın ve **System'i**seçin.
3. **Bilgisayar adını**seçin, **ayarları değiştir'i**seçin ve sonra **Değiştir'i seçin...**
4. **Etki Alanı'nı** seçin ve ardından sanal ağdaki Active Directory etki alanına girin.
5. Etki alanına katılma makineleri için ayrıcalıkları olan bir etki alanı hesabıyla kimlik doğrulaması.

    >[!NOTE]
    > VM'lerinizi Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) ortamına katılıyorsanız, etki alanı birleştirme kullanıcınızın aynı zamanda [AAD DC Yöneticileri grubunun](../active-directory-domain-services/tutorial-create-instance-advanced.md#configure-an-administrative-group)da bir üyesi olduğundan emin olun.

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool"></a>Sanal makineleri Windows Sanal Masaüstü ana bilgisayar havuzuna kaydedin

Sanal makineleri Windows Sanal Masaüstü ana bilgisayar havuzuna kaydetmek, Windows Sanal Masaüstü aracılarını yüklemek kadar kolaydır.

Windows Sanal Masaüstü aracılarını kaydetmek için her sanal makinede aşağıdakileri yapın:

1. [Sanal makineoluştururken](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) sağladığınız kimlik bilgileriyle sanal makineye bağlanın.
2. Windows Sanal Masaüstü Aracısını indirin ve yükleyin.
   - Windows [Sanal Masaüstü Aracısını](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv)İndirin.
   - İndirilen yükleyiciye sağ tıklayın, **Özellikler'i**seçin , **Engellemeyi Kaldır'ı**seçin, ardından **Tamam'ı**seçin. Bu, sisteminizin yükleyiciye güvenmesine olanak sağlar.
   - Yükleyiciyi çalıştırın. Yükleyici sizden kayıt jetonunu istediğinde, **Export-RdsRegistrationInfo** cmdlet'ten aldığınız değeri girin.
3. Windows Virtual Desktop Agent Bootloader'ı indirin ve kurun.
   - Windows [Sanal Masaüstü Aracıbootloader](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH)indirin.
   - İndirilen yükleyiciye sağ tıklayın, **Özellikler'i**seçin , **Engellemeyi Kaldır'ı**seçin, ardından **Tamam'ı**seçin. Bu, sisteminizin yükleyiciye güvenmesine olanak sağlar.
   - Yükleyiciyi çalıştırın.

>[!IMPORTANT]
>Azure'da Windows Sanal Masaüstü ortamınızın güvenliğini sağlamak için, VM'lerinizde gelen bağlantı noktası 3389'u açmamanızı öneririz. Windows Sanal Masaüstü, kullanıcıların ana bilgisayar havuzunun VM'lerine erişmesi için açık bir gelen bağlantı noktası 3389 gerektirmez. Sorun giderme amacıyla 3389 bağlantı noktasını açmanız gerekiyorsa, [tam zamanında VM erişimi](../security-center/security-center-just-in-time.md)kullanmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir ana bilgisayar havuzu yaptığınıza göre, uzaktan kumandayla doldurabilirsiniz. Windows Sanal Masaüstü'ndeki uygulamaları nasıl yönetireceksiniz hakkında daha fazla bilgi edinmek için uygulama gruplarını yönet öğreticisine bakın.

> [!div class="nextstepaction"]
> [Uygulama grupları öğreticiyi yönetme](./manage-app-groups.md)
