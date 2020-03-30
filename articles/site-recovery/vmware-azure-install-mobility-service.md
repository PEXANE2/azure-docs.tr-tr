---
title: VMware VM'lerin ve fiziksel sunucuların Azure'a olağanüstü kurtarma sıyrık yüklemesi yoluyla Mobilite Hizmetini yüklemek için kaynak makineleri hazırlayın | Microsoft Dokümanlar
description: Azure Site Kurtarma hizmetini kullanarak VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü kurtarma hizmeti için push yükleme yoluyla Mobilite aracısını yüklemek için sunucunuzu nasıl hazırlayacağınızı öğrenin.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: a2f4bdb96b8d1ecb23ddcec844726439ec46fff2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74186446"
---
# <a name="prepare-source-machine-for-push-installation-of-mobility-agent"></a>Mobilite aracısının itme montajı için kaynak makinesini hazırlayın

[Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak VMware VM'leri ve fiziksel sunucular için olağanüstü durum kurtarma ayarlarken, [Site Kurtarma Mobilite hizmetini](vmware-physical-mobility-service-overview.md) şirket içi VMware VM ve fiziksel sunucuya yüklersiniz.  Mobilite hizmeti, makinedeki verileri yakalar ve bunları Site Kurtarma işlem sunucusuna iletir.

## <a name="install-on-windows-machine"></a>Windows makinesine yükleme

Korumak istediğiniz her Windows makinesinde aşağıdakileri yapın:

1. Makine ve işlem sunucusu arasında ağ bağlantısı olduğundan emin olun. Ayrı bir işlem sunucusu ayarlamadıysanız, varsayılan olarak yapılandırma sunucusunda çalışıyor demektir.
1. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesabın yerel veya etki alanı olan yönetici hakları olmalıdır. Bu hesabı yalnızca itme yüklemesi ve aracı güncelleştirmeleri için kullanın.
2. Bir etki alanı hesabı kullanmıyorsanız, yerel bilgisayardaki Uzaktan Kullanıcı Erişimi denetimini aşağıdaki gibi devre dışı bırakın:
    - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System kayıt defteri anahtarı altında, yeni bir DWORD ekleyin: **LocalAccountTokenFilterPolicy**. Değeri **1**olarak ayarlayın.
    -  Bunu komut isteminde yapmak için aşağıdaki komutu çalıştırın:  
   'REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d
3. Korumak istediğiniz makinedeki Windows Güvenlik **Duvarı'nda, Güvenlik Duvarı aracılığıyla bir uygulamaya veya özelliğe izin ver'i**seçin. **Dosya ve Yazıcı Paylaşımı nı** ve Windows Yönetimi **Enstrümantasyonunu (WMI)** etkinleştirin. Etki alanına ait bilgisayarlariçin, grup ilkesi nesnesi (GPO) kullanarak güvenlik duvarı ayarlarını yapılandırabilirsiniz.

   ![Güvenlik duvarı ayarları](./media/vmware-azure-install-mobility-service/mobility1.png)

4. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin. Bunu yapmak için yapılandırma sunucunuzda oturum açın.
5. **cspsconfigtool.exe** dosyasını açın. Masaüstünde ve %ProgramData%\ASR\home\svsystems\bin klasöründe kısayol olarak kullanılabilir.
6. Hesapları **Yönet** sekmesinde **Hesap Ekle'yi**seçin.
7. Oluşturduğunuz hesabı ekleyin.
8. Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.

## <a name="install-on-linux-machine"></a>Linux makinesine yükleyin

Korumak istediğiniz her Linux makinesinde aşağıdakileri yapın:

1. Linux makinesi ile işlem sunucusu arasında ağ bağlantısı olduğundan emin olun.
2. İşlem sunucusunun bilgisayara erişmek için kullanabileceği bir hesap oluşturun. Hesap, kaynak Linux sunucusu üzerindeki bir **kök** kullanıcı olmalıdır. Bu hesabı yalnızca itme yüklemesi ve güncelleştirmeler için kullanın.
3. Kaynak Linux sunucusundaki /etc/hosts dosyasında, yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler olduğunu denetleyin.
4. Çoğaltmak istediğiniz bilgisayara en son openssh, openssh-server, openssl paketlerini yükleyin.
5. Secure Shell’in (SSH) etkin olduğundan ve bağlantı noktası 22’de çalıştırıldığından emin olun.
4. sshd_config dosyasında SFTP alt sistemini ve parola kimlik doğrulamasını etkinleştirin. Bunu yapmak **için, kök**olarak oturum açın.
5. **/etc/ssh/sshd_config** dosyasında **PasswordAuthentication**ile başlayan satırı bulun.
6. Satırı açıklamayı bırakın ve değeri **evet**olarak değiştirin.
7. **Alt sistemle**başlayan satırı bulun ve satırı açıklamayı bırakın.

      ![Linux](./media/vmware-azure-install-mobility-service/mobility2.png)

8. **sshd** hizmetini yeniden başlatın.
9. Oluşturduğunuz hesabı CSPSConfigtool içine ekleyin. Bunu yapmak için yapılandırma sunucunuzda oturum açın.
10. **cspsconfigtool.exe** dosyasını açın. Masaüstünde ve %ProgramData%\home\svsystems\bin klasöründe kısayol olarak kullanılabilir.
11. Hesapları **Yönet** sekmesinde **Hesap Ekle'yi**seçin.
12. Oluşturduğunuz hesabı ekleyin.
13. Bir bilgisayar için çoğaltmayı etkinleştirdiğinizde kullandığınız kimlik bilgilerini girin.

## <a name="anti-virus-on-replicated-machines"></a>Çoğaltılan makinelerde anti-virüs

Çoğaltmak istediğiniz makinelerde etkin anti-virüs yazılımı çalışıyorsa, Mobilite hizmeti yükleme klasörünü virüsten koruma işlemlerinden *(C:\ProgramData\ASR\agent)* hariç tutturduğınızdan emin olun. Bu, çoğaltmanın beklendiği gibi çalışmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Mobilite Hizmeti yüklendikten sonra Azure portalında bu VM'leri korumaya başlamak için **+ Çoğaltma'yı** seçin. [VMware VM'ler](vmware-azure-enable-replication.md) ve [fiziksel sunucular](physical-azure-disaster-recovery.md#enable-replication)için çoğaltmayı etkinleştirme hakkında daha fazla bilgi edinin.


