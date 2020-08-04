---
title: Azure NetApp Files için NFSv 4.1 Kerberos şifrelemesini yapılandırma | Microsoft Docs
description: Azure NetApp Files için NFSv 4.1 Kerberos şifrelemesini ve performans etkisini nasıl yapılandıracağınızı açıklar.
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
ms.date: 07/27/2020
ms.author: b-juche
ms.openlocfilehash: 05d173b715a8bc060e2f4d9cdcc7e3aef5630109
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87535563"
---
# <a name="configure-nfsv41-kerberos-encryption-for-azure-netapp-files"></a>Azure NetApp Files için NFSv 4.1 Kerberos şifrelemesini yapılandırma

Azure NetApp Files, Kerberos modlarında (krb5, krb5i ve krb5p), AES-256 şifrelemesi ile NFS istemci şifrelemesini destekler. Bu makalede, bir NFSv 4.1 birimini Kerberos şifrelemesi ile kullanmak için gereken yapılandırma açıklanmaktadır.

## <a name="requirements"></a>Gereksinimler

Aşağıdaki gereksinimler NFSv 4.1 istemci şifrelemesi için geçerlidir: 

* Kerberos anahtar oluşturmayı kolaylaştırmak için Active Directory Domain Services (AD DS) bağlantısı 
* Hem istemci hem de Azure NetApp Files NFS sunucusu IP adresleri için DNS A/PTR kayıt oluşturma
* Bir Linux istemcisi  
    Bu makalede RHEL ve Ubuntu istemcileri için rehberlik sunulmaktadır.  Diğer istemciler benzer yapılandırma adımlarıyla çalışacaktır. 
* NTP sunucusu erişimi  
    Yaygın olarak kullanılan Active Directory Etki Alanı Denetleyicisi (AD DC) etki alanı denetleyicilerinden birini kullanabilirsiniz.

## <a name="create-an-nfs-kerberos-volume"></a>NFS Kerberos birimi oluşturma

1.  NFSv 4.1 birimini oluşturmak için [Azure NetApp Files IÇIN NFS birimi oluşturma](azure-netapp-files-create-volumes.md) bölümündeki adımları izleyin.   

    Birim Oluştur sayfasında, NFS sürümünü **Nfsv 4.1**olarak ayarlayın ve Kerberos ' u **etkin**olarak ayarlayın.

    > [!IMPORTANT] 
    > Birim oluşturulduktan sonra Kerberos etkinleştirme seçimini değiştiremezsiniz.

    ![NFSv 4.1 Kerberos birimi oluştur](../media/azure-netapp-files/create-kerberos-volume.png)  

2. Birim için istenen erişim düzeyi ve güvenlik seçeneği (Kerberos 5, Kerberos 5i veya Kerberos 5p) ile eşleşecek şekilde **dışa aktarma ilkesini** seçin.   

    Kerberos 'un performans etkisi için bkz. [NFSv 4.1 üzerinde Kerberos 'un performans etkisi](#kerberos_performance).  

    Ayrıca, Azure NetApp Files gezinti bölmesinde Ilkeyi dışarı aktar ' a tıklayarak birimin Kerberos güvenlik yöntemlerini değiştirebilirsiniz.

3.  NFSv 4.1 birimini oluşturmak için **gözden geçir + oluştur** ' a tıklayın.

## <a name="configure-the-azure-portal"></a>Azure portal yapılandırma 

1.  [Active Directory bağlantı oluşturma](azure-netapp-files-create-volumes-smb.md#create-an-active-directory-connection)bölümündeki yönergeleri izleyin.  

    Kerberos, Active Directory ' de en az bir makine hesabı oluşturmanızı gerektirir. Sağladığınız hesap bilgileri hem SMB *hem* de nfsv 4.1 Kerberos birimlerine yönelik hesaplar oluşturmak için kullanılır. Bu makine hesap, birim oluşturma sırasında otomatik olarak oluşturulur.

2.  **Kerberos bölgesi**altında **ad sunucusu adını** ve **KDC IP** adresini girin.

    AD sunucusu ve KDC IP aynı sunucu olabilir. Bu bilgiler, Azure NetApp Files tarafından kullanılan SPN makinesi hesabını oluşturmak için kullanılır. Makine hesabı oluşturulduktan sonra, Azure NetApp Files gerektiğinde ek KDC sunucularının yerini bulmak için DNS sunucusu kayıtlarını kullanacaktır. 

    ![Kerberos bölgesi](../media/azure-netapp-files/kerberos-realm.png)
 
3.  Yapılandırmayı kaydetmek için **Birleştir** ' e tıklayın.

## <a name="configure-active-directory-connection"></a>Active Directory bağlantısını yapılandırma 

NFSv 4.1 Kerberos yapılandırması, Active Directory iki bilgisayar hesabı oluşturur:
* SMB paylaşımları için bir bilgisayar hesabı
* NFSv 4.1 için bir bilgisayar hesabı--bu hesabı öneki ile tanımlayabilirsiniz `NFS-` . 

İlk NFSv 4.1 Kerberos birimini oluşturduktan sonra, aşağıdaki PowerShell komutunu kullanarak şifreleme türünü veya bilgisayar hesabını ayarlayın:

`Set-ADComputer $NFSCOMPUTERACCOUNT -KerberosEncryptionType AES256`

## <a name="configure-the-nfs-client"></a>NFS istemcisini yapılandırma 

NFS istemcisini yapılandırmak için [Azure NetApp FILES NFS Istemcisi yapılandırma](configure-nfs-clients.md) konusundaki yönergeleri izleyin.  

## <a name="mount-the-nfs-kerberos-volume"></a><a name="kerberos_mount"></a>NFS Kerberos birimini bağlama

1. **Birimler** sayfasında, bağlamak istediğiniz NFS birimini seçin.

2. Yönergeleri göstermek için birimden **bağlama yönergeleri** ' ni seçin.

    Örneğin: 

    ![Kerberos birimleri için bağlama yönergeleri](../media/azure-netapp-files/mount-instructions-kerberos-volume.png)  

3. Yeni birim için dizin (bağlama noktası) oluşturun.  

4. Bilgisayar hesabı için varsayılan şifreleme türünü AES 256 olarak ayarlayın:  
    `Set-ADComputer $COMPUTERACCOUNT -KerberosEncryptionType AES256 -Credential $ANFSERVICEACCOUNT`

    * Her bilgisayar hesabı için bu komutu yalnızca bir kez çalıştırmanız gerekir.
    * Bu komutu, bir etki alanı denetleyicisinden veya [rsat](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems) yüklü bir bilgisayardan çalıştırabilirsiniz. 
    * `$COMPUTERACCOUNT`Değişkeni, Kerberos birimini dağıtırken Active Directory oluşturulan bilgisayar hesabıdır. Bu, ön eki olan hesaptır `NFS-` . 
    * `$ANFSERVICEACCOUNT`Değişken, bilgisayar hesabının oluşturulduğu kuruluş birimi üzerinde atanmış denetimleri olan ayrıcalıksız bir Active Directory kullanıcı hesabıdır. 

5. Birimi konağa bağlayın: 

    `sudo mount -t nfs -o sec=krb5p,rw,hard,rsize=1048576,wsize=1048576,vers=4.1,tcp $ANFEXPORT $ANFMOUNTPOINT`

    * `$ANFEXPORT`Değişkeni, `host:/export` bağlama yönergelerinde bulunan yoldur.
    * `$ANFMOUNTPOINT`Değişkeni, Linux ana bilgisayarındaki kullanıcı tarafından oluşturulan klasördür.

## <a name="performance-impact-of-kerberos-on-nfsv41"></a><a name="kerberos_performance"></a>NFSv 4.1 üzerinde Kerberos 'un performans etkisi 

Bu bölüm, NFSv 4.1 üzerinde Kerberos 'un performans etkisini anlamanıza yardımcı olur.

### <a name="available-security-options"></a>Kullanılabilir güvenlik seçenekleri 

Şu anda NFSv 4.1 birimleri için kullanılabilir olan güvenlik seçenekleri şunlardır: 

* **sec = sys** , NFS işlemlerinin kimliğini doğrulamak için AUTH_SYS kullanarak yerel UNIX UID 'Leri ve GID 'leri kullanır.
* **sec = krb5** , kullanıcıların kimliğini doğrulamak IÇIN yerel UNIX uid 'Leri ve GID 'Ler yerine Kerberos V5 kullanır.
* **sec = krb5i** , Kullanıcı kimlik doğrulaması Için Kerberos V5 kullanır ve verilerin değiştirilmesini engellemek için güvenli sağlama TOPLAMı kullanarak NFS işlemlerinin bütünlük denetimini gerçekleştirir.
* **sec = krb5p** Kullanıcı kimlik doğrulaması ve bütünlük denetimi Için Kerberos V5 kullanır. Trafik algılaması 'nı engellemek için NFS trafiğini şifreler. Bu seçenek en güvenli ayardır, ancak aynı zamanda en fazla performans yükünden de oluşur.

### <a name="performance-vectors-tested"></a>Performans vektörlerini test edildi

Bu bölümde çeşitli seçeneklerin tek istemci tarafı performans etkisi açıklanmaktadır `sec=*` .

* Performans etkisi iki düzeyde test edildi: düşük eşzamanlılık (düşük yük) ve yüksek eşzamanlılık (g/ç ve aktarım hızı üst sınırı).  
* Üç tür iş yükü test edilmiştir:  
    * Küçük işlem rastgele okuma/yazma (FIO kullanarak)
    * Büyük işlem sıralı okuma/yazma (FIO kullanarak)
    * Git gibi uygulamalar tarafından oluşturulan meta veriler ağır iş yükü

### <a name="expected-performance-impact"></a>Beklenen performans etkisi 

İki odak alanı vardır: açık yük ve üst sınır. Aşağıdaki listede güvenlik ayarı ve senaryo tarafından senaryoya göre performans etkisi güvenlik ayarı açıklanır. Tüm karşılaştırmalar güvenlik parametresine göre yapılır `sec=sys` .

Krb5 'in performans etkisi:

* Düşük eşzamanlılık (r/w):
    * Sıralı gecikme artan 0,3 MS.
    * Rastgele g/ç gecikme süresi arttı 0,2 ms.
    * Meta veri g/ç gecikme süresi 0,2 ms.
* Yüksek eşzamanlılık (r/w): 
    * En fazla sıralı üretilen iş krb5 tarafından etkilenmedi.
    * İş yükü saf yazma 'ya kaydıkça, tamamen etkinin sıfıra düşürüldüğü, saf okuma iş yükleri için en yüksek rastgele g/ç değeri %30 oranında azaltılır. 
    * Maksimum meta veri iş yükü %30 oranında azaltıldı.

Krb5i 'in performans etkisi: 

* Düşük eşzamanlılık (r/w):
    * Sıralı gecikme artan 0,5 ms.
    * Rastgele g/ç gecikme süresi arttı 0,2 ms.
    * Meta veri g/ç gecikme süresi 0,2 ms.
* Yüksek eşzamanlılık (r/w): 
    * İş yükü karışımından bağımsız olarak en fazla sıralı üretilen iş yükü %70 oranında azaltılır.
    * İş yükü saf yazmaya kaydıkça, saf okuma iş yükleri için %50 oranında en yüksek rastgele g/ç azaltılır. 
    * Maksimum meta veri iş yükü %30 oranında azaltıldı.

Krb5p 'in performans etkisi:

* Düşük eşzamanlılık (r/w):
    * Sıralı gecikme artan 0,8 MS.
    * Rastgele g/ç gecikme süresi arttı 0,2 ms.
    * Meta veri g/ç gecikme süresi 0,2 ms.
* Yüksek eşzamanlılık (r/w): 
    * İş yükü karışımından bağımsız olarak en fazla sıralı üretilen iş yükü %85 oranında azaltılır. 
    * İş yükü saf yazmaya kaydıkça, saf okuma 43 iş yükleri için %65 oranında en yüksek rastgele g/ç azalır. 
    * Maksimum meta veri iş yükü %30 oranında azaltıldı.

## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files hakkında SSS](azure-netapp-files-faqs.md)
* [Azure NetApp Files için NFS birimi oluşturma](azure-netapp-files-create-volumes.md)
* [Active Directory bağlantısı oluşturma](azure-netapp-files-create-volumes-smb.md#create-an-active-directory-connection)
* [Azure NetApp Files için NFS istemcisini yapılandırma](configure-nfs-clients.md) 
