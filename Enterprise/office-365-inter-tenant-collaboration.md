---
title: Office 365 테넌트 간 공동 작업
ms.author: chrfox
author: chrfox
manager: laurawi
ms.date: 6/28/2018
ms.audience: Admin
ms.topic: overview
ms.service: o365-administration
localization_priority: Normal
ms.collection: Ent_O365
search.appverid:
- MET150
- MOE150
ms.assetid: eb45fd8b-1d5d-4b0c-9c5a-479dbb176e7d
description: 테 넌 트 및 조직 전체에서 Office 365 공동 작업을 작동 하는 방법을 설명 합니다.
ms.openlocfilehash: 932c837f9dc09dd0469a17ad4e6a05f09966d29c
ms.sourcegitcommit: 69d60723e611f3c973a6d6779722aa9da77f647f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "22541973"
---
# <a name="office-365-inter-tenant-collaboration"></a>Office 365 테넌트 간 공동 작업

이 문서에서는 Office 365 테 넌 트를 두 간의 공동 작업을 수행 하는 여러 방법에 설명 합니다. Office 365 관리자를 위한 것이 것뿐입니다.
  
두, Fabrikam 및 Contoso, 각 조직은 비즈니스 테 넌 트에 대 한 Office 365를 여러 프로젝트; 공동 작업 하려는 생각나지 제한 된 시간 동안 실행 중 일부는 하 고 일부는 진행 중인 키를 누릅니다. 어떻게 Fabrikam 및 Contoso 사용 하도록 설정할 수의 사용자 및 팀이 자신의 다른 Office 365 테 넌 트 간에 안전한 방식으로 보다 효과적으로 공동 작업을 수행할? Office 365, Azure Active Directory B2B 공동 작업을 함께 여러 옵션을 제공 합니다. 이 문서에서는 Fabrikam 및 Contoso 고려할 수 있는 몇가지 주요 시나리오에 설명 합니다.
  
Office 365 테 넌 트 공동 작업 옵션 포함 하는 중앙 위치를 사용 하 여 파일 및 일정 공유, IM을 사용 하 여 대화에 대 한, 오디오/비디오 통신 및 리소스 및 응용 프로그램에 대 한 액세스를 보호에 대 한 호출입니다. 다음 표를 사용 하 여 솔루션을 선택 하 고 자세한 정보를 합니다.
  
## <a name="exchange-online-collaboration-options"></a>Exchange 온라인 공동 작업 옵션

|**공유 목표**|**관리 작업**|**사용 방법 정보**|
|:-----|:-----|:-----|
|다른 Office 365 조직과 일정 공유  <br/> |관리자를 설정할 수 다양 한 수준의 일정 액세스 Exchange Online 기업용 다른 비즈니스와 일정 (약속 있음/없음 정보)를 공유 하는 사용자가 다른 사용자와 공동 작업을 수행할 수 있도록  <br/> |[Exchange Online의 공유](https://technet.microsoft.com/en-us/library/jj916670%28v=exchg.150%29.aspx) <br/> [Exchange Online에서 조직 관계](https://technet.microsoft.com/en-us/library/jj916658%28v=exchg.150%29.aspx) <br/> [Exchange Online에서 조직 관계 만들기](https://technet.microsoft.com/en-us/library/jj916671%28v=exchg.150%29.aspx) <br/> [수정 및 Exchange Online에서 조직 관계](https://technet.microsoft.com/en-us/library/jj916659%28v=exchg.150%29.aspx) <br/> [Exchange Online에서 조직 관계 제거](https://technet.microsoft.com/en-us/library/jj916657%28v=exchg.150%29.aspx) <br/> [외부 사용자와 일정 공유](https://support.office.com/article/fb00dd4e-2d5f-4e8d-8ff4-94b2cf002bdd) <br/> |
|사용자가 자신의 일정 조직 외부의 사용자와 공유 하는 방법을 제어 합니다.  <br/> |관리자가 사용자와 공유할 수 있는 및 부여 된 액세스 수준을 제어 하는 사서함에 공유 정책 적용  <br/> |[Exchange Online에서 정책 공유](https://technet.microsoft.com/en-us/library/jj916673%28v=exchg.150%29.aspx) <br/> [Exchange Online에서 공유 정책 만들기](https://technet.microsoft.com/en-us/library/jj916676%28v=exchg.150%29.aspx) <br/> [Exchange Online의 사서함에 공유 정책 적용](https://technet.microsoft.com/en-us/library/jj916672%28v=exchg.150%29.aspx) <br/> [수정, 해제 또는 Exchange Online에서 공유 정책 제거](https://technet.microsoft.com/en-us/library/jj916674%28v=exchg.150%29.aspx) <br/> |
|안전한 전자 메일 채널을 구성 하 고 파트너 조직을 사용 하 여 메일 흐름을 제어 합니다.  <br/> |관리자가 파트너 조직 또는 서비스 공급자와 메일 교환에 보안을 적용 하려면 커넥터를 만듭니다. 커넥터 제한 도메인 이름에 만들 수 있으며 전송 계층 보안 (TLS)를 통해 암호화 적용 또는 IP 주소 범위를 파트너 보내기 전자 메일을 합니다.  <br/> |[Office 365의 전자 메일 연결 보안을 위해 Exchange Online에서 TLS를 사용하는 방법](https://technet.microsoft.com/en-us/library/mt163898.aspx) <br/> [Configure mail flow using connectors in Office 365](https://technet.microsoft.com/en-us/library/ms.exch.eac.connectorselection%28v=exchg.150%29.aspx) <br/> [Exchange Online의 원격 도메인](https://technet.microsoft.com/en-us/library/jj966211%28v=exchg.150%29.aspx) <br/> [파트너 조직의 보안 메일 흐름에 대 한 커넥터 설정](https://technet.microsoft.com/en-us/library/dn751021%28v=exchg.150%29.aspx) <br/> [Exchange Online 및 Office 365 (개요)에 대 한 메일 흐름 모범 사례](https://technet.microsoft.com/en-us/library/0e6cd9d5-ad3e-418a-8ea9-3bf33332c491%28v=exchg.150%29) <br/> |
   
## <a name="sharepoint-online-and-onedrive-for-business-collaboration-options"></a>SharePoint Online 및 비즈니스 공동 작업 옵션에 대 한 OneDrive

|**공유 목표**|**관리 작업**|**사용 방법 정보**|
|:-----|:-----|:-----|
|외부 사용자와 사이트 및 문서 공유  <br/> |인증 된 Microsoft 계정, 회사 또는 학교에 대 한 사이트 모음 수준 계정 인증 또는 게스트 계정 또는 관리자는 테 넌 트에 공유 구성  <br/> |
  [SharePoint Online 환경에 대해 외부 공유 관리](https://support.office.com/en-US/article/Manage-external-sharing-for-your-SharePoint-Online-environment-C8A462EB-0723-4B0B-8D0A-70FEAFE4BE85?ui=en-US&amp;rs=en-US&amp;ad=US) <br/> [SharePoint Online 및 OneDrive 비즈니스를 위한 Office 365에서에서 공유 하는 제한 된 도메인](https://support.office.com/en-US/article/Restricted-Domains-Sharing-in-Office-365-SharePoint-Online-and-OneDrive-for-Business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9) <br/> [SharePoint Online 비즈니스 (B2B) 익스트라넷 솔루션으로 사용 하 여](https://support.office.com/article/7b087413-165a-4e94-8871-4393e0b9c037) <br/> |
|추적 및 최종 사용자에 대 한 외부 공유 제어  <br/> |OneDrive 비즈니스 파일 소유자 및 SharePoint Online 최종 사용자에 대 한 사이트 및 문서 공유를 구성 하 고 공유를 추적 하는 알림 설정  <br/> |[비즈니스용 OneDrive에 대 한 외부 공유에 대 한 알림을 구성합니다](https://support.office.com/en-US/article/Configure-notifications-for-external-sharing-for-OneDrive-for-Business-b640c693-f170-4227-b8c1-b0a7e0fa876b) <br/> [Office 365에서 SharePoint 파일 또는 폴더 공유](https://support.office.com/article/1fe37332-0f9a-4719-970e-d2578da4941c) <br/> |
   
## <a name="skype-for-business-collaboration-options"></a>비즈니스 공동 작업 옵션에 대 한 Skype

|**공유 목표**|**관리 작업**|**사용 방법 정보**|
|:-----|:-----|:-----|
|비즈니스 사용자를 위한 다른 Skype와 비즈니스 온라인-IM, 통화 및 현재 상태에 대 한 Skype  <br/> |관리자가 IM 온라인 비즈니스 사용자가 자신의 Skype를 설정 하 고 오디오/비디오 통화를 확인 하 고 Office 365 테 넌 트를 다른 사용자와 현재 상태를 볼 수 있습니다.  <br/> |[비즈니스 사용자를 위한 외부 Skype에 연락 하는 사용자를 허용 합니다.](https://support.office.com/article/b414873a-0059-4cd5-aea1-e5d0857dbc94) <br/> |
|비즈니스 온라인-IM, 통화 및 Skype (소비자) 사용자와 현재 상태에 대 한 Skype  <br/> |관리자는 IM 온라인 비즈니스 사용자가 자신의 Skype를 설정 하 고 전화를 걸 Skype (소비자) 사용자와 현재 상태를 볼 수 있습니다.  <br/> |[Skype 연락처를 추가 하는 비즈니스 사용자를 위한 Skype 사용](https://support.office.com/article/08666236-1894-42ae-8846-e49232bbc460) <br/> |
   
## <a name="azure-ad-b2b-collaboration-options"></a>Azure AD B2B 공동 작업 옵션

|**공유 목표**|**관리 작업**|**사용 방법 정보**|
|:-----|:-----|:-----|
|Azure AD B2B 공동 작업-외부 사용자가 조직의 디렉터리에 그룹에 추가 하 여 공유 콘텐츠  <br/> |그룹에 게 외부 사용자를 추가 한 Office 365 테 넌 트에 대 한 전역 관리자가 디렉터리에 참가 하려면 다른 Office 365 테 넌 트에 초대할 수 하 고 SharePoint 사이트 및 그룹에 대 한 라이브러리와 같은 콘텐츠에 대 한 액세스 권한을 부여 합니다.  <br/> |[Azure AD B2B 공동 작업 미리 보기는 무엇입니까?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) <br/> [Azure AD B2B: 새 업데이트가 보다 쉽게 교차 비즈니스 공동 작업](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/) <br/> [Office 365 외부 공유 및 Azure Active Directory B2B 공동 작업](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-o365-external-user) <br/> [Azure Active Directory B2B 공동 작업 API 및 사용자 지정](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-api) <br/> [Azure AD 및 Identity 쇼: Azure AD B2B 공동 작업 (비즈니스에 비즈니스](https://channel9.msdn.com/Series/Azure-AD-Identity/AzureADB2B) <br/> |
   
## <a name="office-365-collaboration-options"></a>Office 365 공동 작업 옵션

|**공유 목표**|**관리 작업**|**사용 방법 정보**|
|:-----|:-----|:-----|
|Office 365 그룹-전자 메일, 일정, OneNote, 및 중앙에서 공유 된 파일  <br/> |그룹은 비즈니스 Essentials, 프리미엄, 교육 및 Enterprise E1, E3 및 e 5 계획에서 지원 됩니다. Office 365 테 넌 트를 하나에 사용자 그룹을 만들고 게스트 사용자로 다른 Office 365 테 넌 트에 사용자를 초대할 수 있습니다. Dynamics CRM에도 적용합니다.  <br/> |[Office 365 그룹에 대 한 설명](https://support.office.com/article/b565caa1-5c40-40ef-9915-60fdb2d97fa2) <br/> [Office 365 그룹에 대 한 게스트 액세스](https://support.office.com/article/bfc7a840-868f-4fd6-a390-f347bf51aff6) <br/> [Office 365 그룹을 배포](https://technet.microsoft.com/en-us/library/dn896591.aspx) <br/> |
   
## <a name="yammer-collaboration-options"></a>Yammer 공동 작업 옵션

|**공유 목표**|**관리 작업**|**사용 방법 정보**|
|:-----|:-----|:-----|
|Yammer-엔터프라이즈 소셜 미디어를 통해 공동 작업  <br/> |외부 그룹을 만드는 기능을 사용 하지 않으면 Yammer 관리 하 여 하지 않으면 사용자가 대화를 선택 하 고 게시물에 따라, 파일을 공유 및 온라인 채팅 하는 기능을 통해 Yammer의 공동 작업을 수행할 외부 그룹을 만들 수 있습니다.  <br/> |[Yammer의 외부 그룹 만들기 및 관리](https://support.office.com/article/9ccd15ce-0efc-4dc1-81bc-4a424ab6f92a) <br/> |
   
## <a name="teams-collaboration-options"></a>팀 공동 작업 옵션

|**공유 목표**|**관리 작업**|**사용 방법 정보**|
|:-----|:-----|:-----|
|조직 외부의 사용자와 팀에서 공동 작업  <br/> |초대 Office 365 테 넌 트에 대 한 전역 관리자 팀에서 외부 공동 작업을 사용 하도록 설정 해야 합니다. 이제 전역 관리자 및 팀 소유자는 팀에서 공동 작업을 수행 하는 전자 메일 주소를 가진 모든 사용자를 초대할 수 있습니다.  <br/> 또한 Admins 관리 하 고 자신의 테 넌 트에 이미 존재 하는 게스트를 편집할 수 있습니다.  <br/> |[게스트 액세스 권한을 부여합니다](https://docs.microsoft.com/en-us/microsoftteams/teams-dependencies) <br/> [팀에 대 한 게스트 액세스 설정 또는 해제](https://docs.microsoft.com/en-us/microsoftteams/set-up-guests) <br/> [PowerShell을 사용 하 여 게스트 액세스를 제어 하려면](https://docs.microsoft.com/en-us/microsoftteams/guest-access-powershell) <br/> [게스트 액세스 검사 목록](https://docs.microsoft.com/en-us/microsoftteams/guest-access-checklist) <br/> [게스트 사용자 보기](https://docs.microsoft.com/en-us/microsoftteams/view-guests) <br/> [게스트 사용자 정보 편집](https://docs.microsoft.com/en-us/microsoftteams/edit-guests-information) <br/> |
|팀 소유자 초대 하 고 게스트 해당 팀 내에서 공동 작업 하는 방법을 관리할 수 있습니다.  <br/> |팀 소유자는 게스트 해당 팀 내에서 수행할 수 있는 추가 컨트롤을 포함 합니다.  <br/> |[게스트를 추가 합니다.](https://support.office.com/en-us/article/teams-and-channels-df38ae23-8f85-46d3-b071-cb11b9de5499?ui=en-US&amp;rs=en-US&amp;ad=US#bkmk_addingguests&amp;ID0EAABAAA=Add_guests) <br/> [팀에 추가 되는 게스트](https://docs.microsoft.com/en-us/microsoftteams/add-guests) <br/> [팀에 대 한 게스트 액세스 관리](https://docs.microsoft.com/en-us/microsoftteams/manage-guests) <br/> [채널 또는 팀에서 사용 하는 참조](https://support.office.com/en-us/article/see-who-s-on-a-team-or-in-a-channel-5c6be9be-9c45-4a0f-a1a0-f332b23cb6b7?ui=en-US&amp;rs=en-US&amp;ad=US) <br/> |
|다른 테 넌 트에서 게스트 팀의 콘텐츠를 보려면 및 다른 구성원과 공동 작업을 수행할 수 있습니다.  <br/> |없음  <br/> |[게스트 액세스 환경](https://docs.microsoft.com/en-us/microsoftteams/guest-experience) <br/> |
   
## <a name="points-to-be-aware-of-about-office-365-inter-tenant-collaboration"></a>Office 365 테 넌 트 공동 작업에 대 한 주의 해야하는 포인트

### <a name="sharing-of-user-accounts-licenses-subscriptions-and-storage"></a>사용자 계정, 라이선스, 구독 및 저장소의 공유

각 조직에는 자체 사용자 계정, id가, 보안 그룹, 구독, 라이선스 및 저장소 유지 관리합니다. 회사 자산 제어를 유지 하는 동안 필요한 정보에 대 한 액세스를 제공 하려면 정책 및 보안 설정을 공유와 함께 Office 365에서 공동 작업 기능을 사용 하는 사용자입니다.
  
- **사용자 계정:** 계정을 공유할 수 없습니다 하 고 계정 테 넌 트 또는 온-프레미스 Active Directory 디렉터리 서비스에서 파티션 간에 중복 될 수 없습니다. 
    
- **라이선스 &amp; 구독:** Office 365에서 라이선스를 제공 계획 라이선스를 (또한 호출된 Sku 또는 Office 365 계획) 사용자가 이러한 계획에 대해 정의 된 Office 365 서비스에 액세스 권한을 부여 합니다. 
    
- **저장소:** Office 365 계획에서 소프트웨어 경계 및 SharePoint Online에 대 한 제한은 별도로 관리에서 사서함 저장소 제한 합니다. 사서함 저장소 제한 설정 하 고 Exchange Online을 사용 하 여 관리 합니다. 두 시나리오에서 테 넌 트 교차 저장소를 공유할 수 없습니다. 
    
### <a name="can-we-share-domain-namespaces-across-office-365-tenants"></a>Office 365 테 넌 트 간에 도메인 네임 스페이스를 공유할 수는?

아니요. 예: fabrikam.com 또는 tailspintoys.com, 베 니 티 도메인 연결 된 및는 한번에 하나의 테 넌 트 함께 사용만 수 있습니다. 각 테 넌 트에는 자체 네임 스페이스; 있어야 합니다. 테 넌 트 간에 UPN, SMTP 및 SIP 네임 스페이스를 공유할 수 없습니다.
  
### <a name="what-about-hybrid-components-and-office-365-inter-tenant-collaboration"></a>하이브리드 구성 요소 및 Office 365 테 넌 트 공동 작업 하는 방법에 대 한 무엇입니까?

여러 테 넌 트 간에 Exchange 조직 및 Azure AD 연결 등의 온-프레미스 하이브리드 구성 요소를 분할할 수 없습니다.
  
