---
title: Yapılandırma Azure NetApp Files NFS birim erişimi için genişletilmiş gruplarla LDAP ekler | Microsoft Docs
description: Azure NetApp Files kullanarak bir NFS birimi oluşturduğunuzda, genişletilmiş gruplarla LDAP 'yi etkinleştirmeye yönelik konuları ve adımları açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 04/05/2021
ms.author: b-juche
ms.openlocfilehash: 2031cbf07d700307ae1e11c516f9fc736bce5080
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2021
ms.locfileid: "106499019"
---
# <a name="configure-adds-ldap-with-extended-groups-for-nfs-volume-access"></a>Yapılandırma, NFS birim erişimi için genişletilmiş gruplarla LDAP ekler

[BIR NFS birimi oluşturduğunuzda](azure-netapp-files-create-volumes.md), birim için genişletilmiş gruplar ile LDAP özelliğini ( **LDAP** seçeneği) etkinleştirme seçeneğiniz vardır. Bu özellik Active Directory LDAP kullanıcılarının ve genişletilmiş grupların (1024 gruba kadar) birime erişmesini sağlar.  

Bu makalede, bir NFS birimi oluştururken genişletilmiş gruplarla LDAP 'yi etkinleştirmeye yönelik konular ve adımlar açıklanmaktadır.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Azure Active Directory Domain Services (AEKLEMELERI) kullanıyorsanız TLS üzerinden LDAP *etkinleştirilmemelidir.*  

* Genişletilmiş gruplar ile LDAP özelliğini etkinleştirirseniz LDAP özellikli [Kerberos birimleri](configure-kerberos-encryption.md) , LDAP kullanıcıları için dosya sahipliğini doğru şekilde görüntülemez. Bir LDAP kullanıcısı tarafından oluşturulan bir dosya veya dizin, `root` gerçek LDAP kullanıcısı yerine varsayılan olarak sahip olur. Ancak, `root` Hesap, komutunu kullanarak dosya sahipliğini el ile değiştirebilir `chown <username> <filename>` . 

* Birimi oluşturduktan sonra LDAP seçenek ayarını (etkin veya devre dışı) değiştiremezsiniz.  

* Aşağıdaki tabloda LDAP önbelleğinin yaşam süresi (TTL) ayarları açıklanmaktadır. İstemci aracılığıyla bir dosyaya veya dizine erişmeyi denemeden önce önbelleğin yenilenmesini beklemeniz gerekir. Aksi takdirde, istemcide erişim engellendi iletisi görüntülenir. 

    |     Hata koşulu    |     Çözüm    |
    |-|-|
    | Önbellek |  Varsayılan Zaman Aşımı |
    | Grup üyeliği listesi  | 24 saat TTL  |
    | UNIX grupları  | 24 saat TTL, 1 dakikalık negatif TTL  |
    | UNIX kullanıcıları  | 24 saat TTL, 1 dakikalık negatif TTL  |

    Önbelleklerin *yaşam süresi* olarak adlandırılan belirli bir zaman aşımı süresi vardır. Zaman aşımı süresi dolduktan sonra, eski girdilerin devam edebilmesi için girişlerin süresi doldu. *Negatıf TTL* değeri, var olmayan nesneler için LDAP sorguları nedeniyle performans sorunlarından kaçınmak için, başarısız olan bir aramanın yer aldığı yerdir. "        

## <a name="steps"></a>Adımlar

1. Genişletilmiş gruplar ile LDAP özelliği şu anda önizlemededir. Bu özelliği ilk kez kullanmadan önce, özelliği kaydetmeniz gerekir:  

    1. Özelliği kaydedin:   

        ```azurepowershell-interactive
        Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapExtendedGroups
        ```

    2. Özellik kaydının durumunu denetleyin: 

        > [!NOTE]
        > **Registrationstate** , ' a `Registering` değiştirilmeden önce 60 dakikaya kadar bir durumda olabilir `Registered` . Devam etmeden önce durum olana kadar bekleyin `Registered` .

        ```azurepowershell-interactive
        Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapExtendedGroups
        ```
        
    Ayrıca, [Azure CLI komutlarını](/cli/azure/feature) kullanarak `az feature register` `az feature show` özelliği kaydedebilir ve kayıt durumunu görüntüleyebilirsiniz. 

2. LDAP birimleri LDAP sunucu ayarları için bir Active Directory yapılandırması gerektirir. [Active Directory bağlantıları Için gereksinimlerde](create-active-directory-connections.md#requirements-for-active-directory-connections) yer alan yönergeleri izleyin ve Azure Portal Active Directory bağlantıları yapılandırmak için [Active Directory bağlantısı oluşturun](create-active-directory-connections.md#create-an-active-directory-connection) .  

3. Active Directory LDAP sunucusunun Active Directory açık ve çalışıyor olduğundan emin olun. Bunu, AD makinesine [Active Directory Basit Dizin Hizmetleri (AD LDS)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831593(v=ws.11)) rolünü yükleyip yapılandırarak yapabilirsiniz.

4. LDAP NFS kullanıcılarının, LDAP sunucusunda belirli POSIX özniteliklerinin olması gerekir. Gerekli öznitelikleri ayarlamak için [LDAP POSIX özniteliklerini Yönet](create-volumes-dual-protocol.md#manage-ldap-posix-attributes) ' i izleyin.  

5. LDAP ile tümleşik bir Linux istemcisini yapılandırmak istiyorsanız, bkz. [NFS Istemcisini yapılandırma Azure NetApp Files](configure-nfs-clients.md).

6.  NFS birimi oluşturmak için [Azure NetApp Files IÇIN NFS birimi oluşturma](azure-netapp-files-create-volumes.md) bölümündeki adımları izleyin. Birim oluşturma işlemi sırasında **protokol** sekmesi altında **LDAP** seçeneğini etkinleştirin.   

    ![LDAP seçeneğiyle bir birim oluştur sayfası gösteren ekran görüntüsü.](../media/azure-netapp-files/create-nfs-ldap.png)  

7. İsteğe bağlı-genişletilmiş gruplar etkinleştirilmiş LDAP içeren bir NFS birimine erişmek için Windows LDAP sunucusunda mevcut olmayan yerel NFS istemci kullanıcılarını etkinleştirebilirsiniz. Bunu yapmak için, **Yerel NFS KULLANıCıLARıNA LDAP Ile Izin ver** seçeneğini şu şekilde etkinleştirin:
    1. **Active Directory bağlantılar**' a tıklayın.  Var olan bir Active Directory bağlantısında bağlam menüsüne (üç nokta `…` ) tıklayın ve **Düzenle**' yi seçin.  
    2. Görüntülenen **Active Directory Ayarları Düzenle** penceresinde, **Yerel NFS kullanıcılarına LDAP ile izin ver** seçeneğini belirleyin.  

    ![Yerel NFS kullanıcılarına LDAP ile Izin ver seçeneğini gösteren ekran görüntüsü](../media/azure-netapp-files/allow-local-nfs-users-with-ldap.png)  

## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files için NFS birimi oluşturma](azure-netapp-files-create-volumes.md)
* [Active Directory bağlantıları oluşturma ve yönetme](create-active-directory-connections.md)
* [LDAP birimi sorunlarını giderme](troubleshoot-ldap-volumes.md)
