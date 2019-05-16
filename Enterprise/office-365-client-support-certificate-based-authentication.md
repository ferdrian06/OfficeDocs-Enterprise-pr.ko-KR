---
title: Office 365 클라이언트 앱 지원-인증서 기반 인증
ms.author: robmazz
author: robmazz
manager: laurawi
audience: ITPro
ms.topic: article
ms.service: Office 365 Administration
localization_priority: Normal
ms.collection:
- Strat_O365_Enterprise
- M365-subscription-management
search.appverid:
- MET150
description: 인증서 기반 인증에 대 한 Office 365 클라이언트 앱 지원
ms.openlocfilehash: 88bc59934399588f0a5c682c6b136ad0948ecd52
ms.sourcegitcommit: 9c6e31204aa326c31d60befe80e610f702e65800
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2019
ms.locfileid: "33990929"
---
# <a name="office-365-client-app-support--certificate-based-authentication"></a><span data-ttu-id="14c21-103">Office 365 클라이언트 앱 지원-인증서 기반 인증</span><span class="sxs-lookup"><span data-stu-id="14c21-103">Office 365 Client App Support — Certificate-based Authentication</span></span>

<span data-ttu-id="14c21-104">인증서 기반 인증을 사용 하면 Windows, Android 또는 iOS 장치에서 클라이언트 인증서를 사용 하 여 Azure Active Directory에 인증할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="14c21-104">Certificate-based authentication enables you to authenticate to Azure Active Directory with a client certificate on Windows, Android, or iOS devices.</span></span> <span data-ttu-id="14c21-105">이 기능을 구성 하면 모바일 장치에서 특정 메일 및 Microsoft Office 응용 프로그램에 대 한 사용자 이름 및 암호 조합을 입력할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="14c21-105">Configuring this feature eliminates the need to enter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span>

<span data-ttu-id="14c21-106">자세한 내용은 [인증서 기반 인증](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started)을 확인 하세요.</span><span class="sxs-lookup"><span data-stu-id="14c21-106">Learn more about [certificate-based authentication](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started).</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="14c21-107">지원되는 플랫폼</span><span class="sxs-lookup"><span data-stu-id="14c21-107">Supported platforms</span></span>

 - <span data-ttu-id="14c21-108">Windows 10 데스크톱<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="14c21-108">Windows 10 Desktop<sup>2</sup></span></span>
 - <span data-ttu-id="14c21-109">Windows 10 최신 앱</span><span class="sxs-lookup"><span data-stu-id="14c21-109">Windows 10 Modern Apps</span></span>
 - <span data-ttu-id="14c21-110">웹 브라우저</span><span class="sxs-lookup"><span data-stu-id="14c21-110">Web browsers</span></span>
 - <span data-ttu-id="14c21-111">Android</span><span class="sxs-lookup"><span data-stu-id="14c21-111">Android</span></span>
 - <span data-ttu-id="14c21-112">iOS</span><span class="sxs-lookup"><span data-stu-id="14c21-112">iOS</span></span>
 - <span data-ttu-id="14c21-113">macOS<sup>1</sup> <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="14c21-113">macOS<sup>1</sup> <sup>2</sup></span></span>

<span data-ttu-id="14c21-114">Office 365의 플랫폼 지원에 대 한 자세한 내용은 [office 365에 대 한 시스템 요구 사항을](https://products.office.com/office-system-requirements)참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="14c21-114">For more information about platform support in Office 365, see [System requirements for Office 365](https://products.office.com/office-system-requirements).</span></span>

## <a name="supported-clients"></a><span data-ttu-id="14c21-115">지원되는 클라이언트</span><span class="sxs-lookup"><span data-stu-id="14c21-115">Supported clients</span></span>

<span data-ttu-id="14c21-116">다음 클라이언트의 최신 버전에서는 인증서 기반 인증을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="14c21-116">The latest versions of the following clients support certificate-based authentication:</span></span>

| | | | | | |
|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="14c21-117">![액세스 아이콘](media/o365-access-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-117">![Access icon](media/o365-access-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-118">액세스</span><span class="sxs-lookup"><span data-stu-id="14c21-118">Access</span></span>](https://products.office.com/access) | <span data-ttu-id="14c21-119">![Azure 아이콘](media/o365-azure-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-119">![Azure icon](media/o365-azure-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-120">Azure AD <br> 포털</span><span class="sxs-lookup"><span data-stu-id="14c21-120">Azure AD <br> Portal </span></span>](https://azure.microsoft.com/features/azure-portal/) | <span data-ttu-id="14c21-121">![회사 포털 아이콘](media/o365-microsoft-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-121">![Company portal icon](media/o365-microsoft-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-122">회사 <br> 포털</span><span class="sxs-lookup"><span data-stu-id="14c21-122">Company <br> Portal </span></span>](https://docs.microsoft.com/intune-user-help/sign-in-to-the-company-portal) | <span data-ttu-id="14c21-123">![Delve 아이콘](media/o365-delve-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-123">![Delve icon](media/o365-delve-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-124">Delve</span><span class="sxs-lookup"><span data-stu-id="14c21-124">Delve</span></span>](https://products.office.com/business/intelligent-search) | <span data-ttu-id="14c21-125">![Dynamics 365 아이콘](media/o365-dynamics365-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-125">![Dynamics 365 icon](media/o365-dynamics365-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-126">Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="14c21-126">Dynamics 365</span></span>](https://dynamics.microsoft.com) 
| <span data-ttu-id="14c21-127">![에 지 아이콘](media/o365-edge-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-127">![Edge icon](media/o365-edge-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-128">면</span><span class="sxs-lookup"><span data-stu-id="14c21-128">Edge</span></span>](https://www.microsoft.com/windows/microsoft-edge) | <span data-ttu-id="14c21-129">![Excel 아이콘](media/o365-excel-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-129">![Excel icon](media/o365-excel-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-130">Excel</span><span class="sxs-lookup"><span data-stu-id="14c21-130">Excel</span></span>](https://products.office.com/excel) | <span data-ttu-id="14c21-131">![흐름 아이콘](media/o365-flow-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-131">![Flow icon](media/o365-flow-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-132">Flow</span><span class="sxs-lookup"><span data-stu-id="14c21-132">Flow</span></span>](https://flow.microsoft.com) | <span data-ttu-id="14c21-133">![양식 아이콘](media/o365-forms-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-133">![Forms icon](media/o365-forms-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-134">Forms</span><span class="sxs-lookup"><span data-stu-id="14c21-134">Forms</span></span>](https://flow.microsoft.com/connectors/shared_microsoftforms/microsoft-forms/) | <span data-ttu-id="14c21-135">![Kaizala 아이콘](media/o365-kaizala-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-135">![Kaizala icon](media/o365-kaizala-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-136">Kaizala</span><span class="sxs-lookup"><span data-stu-id="14c21-136">Kaizala</span></span>](https://products.office.com/en/business/microsoft-kaizala) 
| <span data-ttu-id="14c21-137">![Office 365 관리 아이콘](media/o365-o365admin-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-137">![Office 365 Admin icon](media/o365-o365admin-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-138">Office 365 <br> 관리자</span><span class="sxs-lookup"><span data-stu-id="14c21-138">Office 365 <br> Admin</span></span>](https://products.office.com/business/manage-office-365-admin-app) | <span data-ttu-id="14c21-139">![렌즈 아이콘](media/o365-lens-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-139">![Lens icon](media/o365-lens-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-140">Office Lens</span><span class="sxs-lookup"><span data-stu-id="14c21-140">Office Lens</span></span>](https://www.microsoft.com/p/office-lens/9wzdncrfj3t8?activetab=pivot%3Aoverviewtab) | <span data-ttu-id="14c21-141">![비즈니스용 OneDrive 아이콘](media/o365-OneDrive-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-141">![OneDrive for Business icon](media/o365-OneDrive-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-142">OneDrive<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="14c21-142">OneDrive<sup>1</sup></span></span>](https://products.office.com/onedrive-for-business/online-cloud-storage) |  <span data-ttu-id="14c21-143">![OneNote 아이콘](media/o365-OneNote-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-143">![OneNote icon](media/o365-OneNote-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-144">OneNote</span><span class="sxs-lookup"><span data-stu-id="14c21-144">OneNote</span></span>](https://products.office.com/onenote) | <span data-ttu-id="14c21-145">![Outlook 아이콘](media/o365-outlook-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-145">![Outlook icon](media/o365-outlook-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-146">Outlook</span><span class="sxs-lookup"><span data-stu-id="14c21-146">Outlook</span></span>](https://products.office.com/outlook) 
| <span data-ttu-id="14c21-147">![Planner 아이콘](media/o365-planner-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-147">![Planner icon](media/o365-planner-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-148">Planner</span><span class="sxs-lookup"><span data-stu-id="14c21-148">Planner</span></span>](https://products.office.com/business/task-management-software) | <span data-ttu-id="14c21-149">![PowerApps 아이콘](media/o365-powerapps-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-149">![PowerApps icon](media/o365-powerapps-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-150">PowerApps</span><span class="sxs-lookup"><span data-stu-id="14c21-150">PowerApps </span></span>](https://powerapps.microsoft.com) | <span data-ttu-id="14c21-151">![PowerBI 아이콘](media/o365-powerbi-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-151">![PowerBI icon](media/o365-powerbi-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-152">Power BI</span><span class="sxs-lookup"><span data-stu-id="14c21-152">Power BI</span></span>](https://powerbi.microsoft.com)| <span data-ttu-id="14c21-153">![PowerPoint 아이콘](media/o365-powerpoint-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-153">![PowerPoint icon](media/o365-powerpoint-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-154">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="14c21-154">PowerPoint</span></span>](https://products.office.com/powerpoint) | <span data-ttu-id="14c21-155">![프로젝트 아이콘](media/o365-project-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-155">![Project icon](media/o365-project-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-156">Project</span><span class="sxs-lookup"><span data-stu-id="14c21-156">Project</span></span>](https://products.office.com/project) 
| <span data-ttu-id="14c21-157">![Publisher 아이콘](media/o365-publisher-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-157">![Publisher icon](media/o365-publisher-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-158">Publisher</span><span class="sxs-lookup"><span data-stu-id="14c21-158">Publisher</span></span>](https://products.office.com/publisher) | <span data-ttu-id="14c21-159">![SharePoint 아이콘](media/o365-sharepoint-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-159">![SharePoint icon](media/o365-sharepoint-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-160">Sharepoint</span><span class="sxs-lookup"><span data-stu-id="14c21-160">Sharepoint</span></span>](https://products.office.com/sharepoint) | <span data-ttu-id="14c21-161">![비즈니스용 Skype 아이콘](media/o365-skypeforbusiness-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-161">![Skype for Business icon](media/o365-skypeforbusiness-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-162"><br> 비즈니스용 Skype</span><span class="sxs-lookup"><span data-stu-id="14c21-162">Skype for <br> Business</span></span>](https://www.skype.com/business/) | <span data-ttu-id="14c21-163">![스티커 메모 아이콘](media/o365-stickynotes-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-163">![Sticky Notes icon](media/o365-stickynotes-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-164">스티커 메모</span><span class="sxs-lookup"><span data-stu-id="14c21-164">Sticky Notes</span></span>](https://www.microsoft.com/p/microsoft-sticky-notes/9nblggh4qghw) | <span data-ttu-id="14c21-165">![스트림 아이콘](media/o365-stream-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-165">![Stream icon](media/o365-stream-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-166">Stream</span><span class="sxs-lookup"><span data-stu-id="14c21-166">Stream</span></span>](https://stream.microsoft.com) 
| <span data-ttu-id="14c21-167">![Sway 아이콘](media/o365-sway-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-167">![Sway icon](media/o365-sway-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-168">Sway</span><span class="sxs-lookup"><span data-stu-id="14c21-168">Sway</span></span>](https://sway.com) | <span data-ttu-id="14c21-169">![팀 아이콘](media/o365-teams-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-169">![Teams icon](media/o365-teams-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-170">Teams</span><span class="sxs-lookup"><span data-stu-id="14c21-170">Teams</span></span>](https://products.office.com/microsoft-teams/group-chat-software) | <span data-ttu-id="14c21-171">![할 일 아이콘](media/o365-todo-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-171">![To-Do icon](media/o365-todo-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-172">To-Do</span><span class="sxs-lookup"><span data-stu-id="14c21-172">To-Do</span></span>](https://todo.microsoft.com) | <span data-ttu-id="14c21-173">![Visio 아이콘](media/o365-visio-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-173">![Visio icon](media/o365-visio-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-174">Visio</span><span class="sxs-lookup"><span data-stu-id="14c21-174">Visio</span></span>](https://products.office.com/visio/flowchart-software) | <span data-ttu-id="14c21-175">![Word 아이콘](media/o365-word-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-175">![Word icon](media/o365-word-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-176">Word</span><span class="sxs-lookup"><span data-stu-id="14c21-176">Word</span></span>](https://products.office.com/word) 
| <span data-ttu-id="14c21-177">![Yammer 아이콘](media/o365-yammer-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-177">![Yammer icon](media/o365-yammer-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-178">Yammer<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="14c21-178">Yammer<sup>2</sup></span></span>](https://products.office.com/yammer/yammer-overview) |

## <a name="supported-powershell-modules"></a><span data-ttu-id="14c21-179">지원 되는 PowerShell 모듈</span><span class="sxs-lookup"><span data-stu-id="14c21-179">Supported PowerShell modules</span></span>

| | | | | | |
|:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="14c21-180">![Azure 아이콘](media/o365-azure-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-180">![Azure icon](media/o365-azure-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-181">Azure AD <br> PowerShell</span><span class="sxs-lookup"><span data-stu-id="14c21-181">Azure AD <br> PowerShell</span></span>](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0) | <span data-ttu-id="14c21-182">![Exchange 아이콘](media/o365-exchange-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-182">![Exchange icon](media/o365-exchange-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-183">Exchange Online <br> PowerShell</span><span class="sxs-lookup"><span data-stu-id="14c21-183">Exchange Online <br> PowerShell</span></span>](https://docs.microsoft.com/powershell/exchange/exchange-online/exchange-online-powershell?view=exchange-ps) | <span data-ttu-id="14c21-184">![SharePoint 아이콘](media/o365-sharepoint-64x64.png)</span><span class="sxs-lookup"><span data-stu-id="14c21-184">![SharePoint icon](media/o365-sharepoint-64x64.png)</span></span> <br> [<span data-ttu-id="14c21-185">SharePoint Online <br> PowerShell</span><span class="sxs-lookup"><span data-stu-id="14c21-185">SharePoint Online <br> PowerShell</span></span>](https://docs.microsoft.com/sharepoint/manage-team-and-communication-sites-in-powershell)

> [!NOTE]
> <span data-ttu-id="14c21-186"><sup>1</sup> Macos의 OneDrive에 대 한 지원이 곧 제공 될 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="14c21-186"><sup>1</sup> Support for OneDrive on macOS available soon.</span></span> <br>
> <span data-ttu-id="14c21-187"><sup>2</sup> Windows 데스크톱의 Yammer 지원 및 macos가 곧 제공 될 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="14c21-187"><sup>2</sup> Support for Yammer on Windows Desktop and macOS available soon.</span></span>