---
title: "Detection strategies across cloud and identities against infiltrating IT workers"
url: "https://www.microsoft.com/en-us/security/blog/2026/04/21/detection-strategies-cloud-identities-against-infiltrating-it-workers/"
date: "Tue, 21 Apr 2026 16:03:09 +0000"
author: "Microsoft Defender Security Research Team and Microsoft Threat Intelligence"
feed_url: "https://www.microsoft.com/en-us/security/blog/feed/"
---
<aside class="table-of-contents-block accordion wp-block-bloginabox-theme-table-of-contents" id="accordion-755feea4-0e4a-42bf-81a8-ad6aa05ddb04">
	<button class="btn btn-collapse" type="button">
		<span class="table-of-contents-block__label">In this article</span>
		<span class="table-of-contents-block__current"></span>

		<svg class="table-of-contents-block__arrow" fill="none" height="11" viewBox="0 0 18 11" width="18" xmlns="http://www.w3.org/2000/svg">
			<path d="M15.7761 11L18 8.82043L9 0L0 8.82043L2.22394 11L9 4.35913L15.7761 11Z" fill="currentColor">
		</svg>
	</button>
	<div class="table-of-contents-block__collapse-wrapper collapse show" id="accordion-collapse-755feea4-0e4a-42bf-81a8-ad6aa05ddb04">
		<div class="table-of-contents-block__content">
			<ol class="table-of-contents-block__list"><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#attack-chain-overview">Attack chain overview</a><ol class="table-of-contents-block__list"><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#activities-in-pre-recruitment-phase">Activities in pre-recruitment phase</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#activities-in-recruiting-phase">Activities in recruiting phase</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#activities-in-post-recruitment-phase">Activities in post-recruitment phase</a></li></ol></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#mitigation-and-protection-guidance">Mitigation and protection guidance</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#microsoft-defender-xdr-detections">Microsoft Defender XDR detections</a></li></ol>		</div>
	</div>
	<span class="table-of-contents-block__progress-bar"></span>
</aside>



<p class="wp-block-paragraph">The shift to remote and hybrid work since the pandemic expanded global hiring and accelerated digital onboarding, increasing reliance on online identity verification and remote access. Threat actors such as <a href="https://www.microsoft.com/en-us/security/blog/2025/06/30/jasper-sleet-north-korean-remote-it-workers-evolving-tactics-to-infiltrate-organizations/">Jasper Sleet</a>, a North Korea-aligned threat actor, exploit this model by posing as legitimate hires using stolen or fabricated identities and AI-assisted deception to gain trusted access, generate revenue, and in some cases enable data theft, extortion, or follow-on compromise.</p>



<p class="wp-block-paragraph">In the initial job-discovery phase, these fraudulent applicants posing as remote IT workers systematically survey organization career sites and external hiring portals to identify active technical roles and recruitment workflows. A previously published <a href="https://www.microsoft.com/en-us/security/blog/2026/03/06/ai-as-tradecraft-how-threat-actors-operationalize-ai/">Microsoft Threat Intelligence blog</a> highlights how these actors use generative AI at scale to analyze job postings and extract role‑specific language, required skills, certifications, and tooling expectations. They then use those insights to construct tailored fake digital personas and submit highly convincing job applications, increasing their likelihood of passing screening and entering legitimate hiring pipelines, and even onboarding once hired into the targeted roles successfully.</p>



<p class="wp-block-paragraph">Organizations using common and widely adopted human resources (HR) software as a service (SaaS) platforms like Workday often expose their job postings through external career sites for applicants to submit job applications. These job listing sites are often targeted by this threat actor to find open job roles. While this activity might be hard to detect from usual job hunting behavior, knowing the threat actor’s interests and objectives to infiltrate into the target organization might present an opportunity for defenders to look for anomalous patterns in a hiring candidate’s behaviors by leveraging the access to the right telemetry and available threat actor intelligence being published.</p>



<p class="wp-block-paragraph">While these activities could happen on any HR SaaS platform, this blog focuses on Workday as an example due to its widespread adoption and rich event logs, which are useful for hunting and detection, that are available to customers. The discussion highlights how customers using Microsoft Defender for Cloud Apps can monitor and detect fraudulent remote IT worker activity in pre-recruitment and post-recruitment phases, offering guidance on threat hunting and relevant threat detection strategies to help security and HR teams surface suspicious candidates early and detect risky onboarding activity after hire.</p>



<h2 class="wp-block-heading" id="attack-chain-overview">Attack chain overview</h2>



<p class="wp-block-paragraph">In the observed campaigns, the threat actors leverage routine HR workflows like external-facing career sites with open job postings to help with their job search and application process. Once they’re successfully contacted, interviewed, and hired, they complete typical new-hire onboarding formalities like setting up payroll accounts, which are also through the HR SaaS platform like Workday.</p>


<figure class="wp-block-image size-full"><img alt="Jasper Sleet attack chain" class="wp-image-146718 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/Jasper-Sleet-attack-chain.webp" /><figcaption class="wp-element-caption">Figure 1. Timeline of events through the recruitment phases.</figcaption></figure>



<h3 class="wp-block-heading" id="activities-in-pre-recruitment-phase">Activities in pre-recruitment phase</h3>



<p class="wp-block-paragraph">In the pre-recruitment phase, Microsoft has observed Jasper Sleet accessing Workday Recruiting Web Service endpoints exposed through external career sites from known actor infrastructure and email accounts, indicating a discovery phase of open roles and recruitment workflows.</p>



<p class="wp-block-paragraph">Workday lets organizations use internal, non-public APIs such as Recruiting Web Service to allow programmatic access to apply for jobs in these organizations. These APIs are used to connect to external career sites involved in talent management and applicant tracking systems and allow applicants to browse and apply for open job roles. To access these APIs, an organization has to allow setting up of OAuth clients and associated OAuth tokens, and expose the APIs so that the organization’s external career sites can use them.</p>



<p class="wp-block-paragraph">Microsoft has observed API call events coming from known Jasper Sleet infrastructure in Workday telemetry to <em>hrrecruiting/*</em> API endpoints. These events access information about job postings, applications, and related questionnaires, and to submit job applications and questionnaires.</p>



<p class="wp-block-paragraph">Some common API calls being made by the threat actor’s activity when using the Workday portal include the following:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">hrrecruiting/accounts/*</li>



<li class="wp-block-list-item">hrrecruiting/jobApplicationPackages/*</li>



<li class="wp-block-list-item">hrrecruiting/validateJobApplication/*</li>



<li class="wp-block-list-item">hrrecruiting/resumes/*</li>
</ul>


<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-146684 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-54.webp" style="width: 563px; height: auto;" /><figcaption class="wp-element-caption">Figure 2. Sample view of API call events indicating access to hrrecruiting API endpoints on an organization&rsquo;s Workday instance from an external account.</figcaption></figure>



<p class="wp-block-paragraph">It’s important to note here that these API calls could also be made by legitimate job applicants. However, Microsoft has observed the Jasper Sleet threat actor using multiple external accounts suspiciously to access the same set of API calls in a consistent, repeating pattern, as shown in Figure 2, indicating a possible job discovery phase activity on open job roles and following up on job applications submitted. This anomaly sets the threat actor behavior apart from legitimate job applicants.</p>



<p class="wp-block-paragraph">Defender for Cloud Apps’ <a href="https://learn.microsoft.com/en-us/defender-cloud-apps/protect-workday">Workday connector</a> enables organizations to view and track API activity to their <em>/hrrecruiting </em>endpoints. The connector also lets them identify external accounts and their corresponding infrastructure metadata. Organizations can match this information against any available threat intelligence feeds on Jasper Sleet so they can identify fraudulent applications early in the recruiting process.</p>



<h3 class="wp-block-heading" id="activities-in-recruiting-phase">Activities in recruiting phase</h3>



<p class="wp-block-paragraph">In the recruiting phase, signals outside of Workday could help with investigation of threat actor behavior. The threat actor communicates with the target organization’s hiring team using emails and meeting conferencing platforms like Microsoft Teams, Zoom, or Cisco Webex for scheduling interviews. Using advanced hunting tables in Microsoft Defender, organizations can track suspicious communications (for example, email and Teams messages with external accounts originating from suspicious IP addresses or email addresses that could possibly be associated with the threat actor) and raise a red flag early in the hiring process. Additionally, organizations that use Zoom or Cisco Webex must leverage Defender for Cloud Apps’ <a href="https://learn.microsoft.com/en-us/defender-cloud-apps/protect-zoom">Zoom</a> or <a href="https://learn.microsoft.com/en-us/defender-cloud-apps/protect-webex">Cisco Webex</a> connectors to detect malicious external accounts in the interviewing process.</p>



<p class="wp-block-paragraph">Organizations can also leverage Defender for Cloud Apps’ <a href="https://learn.microsoft.com/en-us/defender-cloud-apps/protect-docusign">DocuSign connector</a>, which enables them to monitor activity related to hiring documentation, like offer letter signing from suspicious external sources.</p>



<h3 class="wp-block-heading" id="activities-in-post-recruitment-phase">Activities in post-recruitment phase</h3>



<p class="wp-block-paragraph">When Jasper Sleet is hired for a role in the organization, a legitimate account is created and assigned to them as part of the onboarding process. In organizations that use HR workflows in Workday for onboarding new hires, we’ve observed sign-ins to the newly created Workday profile and setting up of payroll details originating from known Jasper Sleet infrastructure.</p>


<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-146685 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-55.webp" style="width: 677px; height: auto;" /><figcaption class="wp-element-caption">Figure 3. A sample event indicating a payroll account change operation by a new hire.</figcaption></figure>



<p class="wp-block-paragraph">The threat actor now has legitimate access to organization data, and they can access internal SaaS applications like Teams, SharePoint, OneDrive, and Exchange Online. Hence, it’s important to investigate any alerts associated with new hire accounts, especially alerts that are related to access to organization data from different locations and anonymous proxies performing search and downloads on Microsoft 365 suite or other third-party SaaS applications. Microsoft has observed a spike in impossible travel alerts for such new hires, indicating suspicious remote IT worker behavior in the initial months of onboarding.</p>


<figure class="wp-block-image size-full is-resized"><img alt="" class="wp-image-146686 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-56.webp" style="width: 608px; height: auto;" /><figcaption class="wp-element-caption">Figure 4. Frequent impossible travel alerts on a new hire in the first two months since joining.</figcaption></figure>



<h2 class="wp-block-heading" id="mitigation-and-protection-guidance">Mitigation and protection guidance</h2>



<p class="wp-block-paragraph">Microsoft recommends leveraging access to telemetry coming from multiple data sources and monitoring behavioral anomalies in hiring candidates as part of background verification in HR recruitment processes. Organizations can also leverage threat intelligence as an aid, when available, to strengthen confidence in these anomalies.</p>



<p class="wp-block-paragraph">These recommendations draw from established Defender blog guidance patterns and align with protections offered across Microsoft Defender XDR.&nbsp;</p>



<p class="wp-block-paragraph">Organizations can follow these recommendations to mitigate threats associated with this threat actor:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>



<p class="wp-block-paragraph">Enable connectors in Microsoft Defender for Cloud Apps to gain visibility and track activity from external user accounts associated with fraudulent candidates. Investigate events of both external users and newly hired internal users originating from malicious infrastructure. For more information, see the following articles in Microsoft Learn:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><a href="https://learn.microsoft.com/en-us/defender-cloud-apps/protect-workday">How Defender for Cloud Apps helps protect your Workday environment</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/en-us/defender-cloud-apps/protect-docusign">How Defender for Cloud Apps helps protect your DocuSign environment</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/en-us/defender-cloud-apps/protect-webex">How Defender for Cloud Apps helps protect your Cisco Webex environment</a></li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/en-us/defender-cloud-apps/protect-zoom">Connect Zoom to Microsoft Defender for Cloud Apps (Preview)</a></li>
</ul>



<p class="wp-block-paragraph">Educate users on social engineering. Train employees to recognize suspicious behaviors during hiring process and in new hires. For more information on the threat actor behavior, read this blog: <a href="https://www.microsoft.com/en-us/security/blog/2025/06/30/jasper-sleet-north-korean-remote-it-workers-evolving-tactics-to-infiltrate-organizations/">Jasper Sleet: North Korean remote IT workers’ evolving tactics to infiltrate organizations</a></p>



<h2 class="wp-block-heading" id="microsoft-defender-xdr-detections">Microsoft Defender XDR detections</h2>



<p class="wp-block-paragraph">Microsoft Defender XDR customers can refer to the list of applicable detections below. Microsoft Defender XDR coordinates detection, prevention, investigation, and response across endpoints, identities, email, and apps to provide integrated protection against attacks like the threat discussed in this blog.</p>



<p class="wp-block-paragraph">Customers with provisioned access can also use <a href="https://learn.microsoft.com/en-us/defender-xdr/security-copilot-in-microsoft-365-defender">Microsoft Security Copilot in Microsoft Defender</a> to investigate and respond to incidents, hunt for threats, and protect their organization with relevant threat intelligence.</p>



<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td><strong>Tactic</strong><strong>&nbsp;</strong></td><td><strong>Observed activity</strong><strong>&nbsp;</strong></td><td><strong>Microsoft Defender coverage</strong><strong>&nbsp;</strong></td></tr><tr><td>Resource Development &nbsp;</td><td>Threat actors accessing external facing Workday sites to research job postings and submit job applications.</td><td><strong>Microsoft Defender for Cloud Apps</strong><br />&#8211; Possible Jasper Sleet threat actor activity in Workday Recruiting Web Service  </td></tr><tr><td>Resource Development &nbsp;</td><td>Once hired and onboarded, the threat actor signs in to the newly created Workday account to update payroll details from known Jasper Sleet infrastructure</td><td><strong>Microsoft Defender for Cloud Apps</strong><br />&#8211; Suspicious Payroll and Finance related activity in Workday</td></tr><tr><td>Initial Access</td><td>Anomalous sign-ins and access to internal resources by newly hired threat actor</td><td><strong>Microsoft Defender XDR</strong><br />&#8211; Impossible travel <br />&#8211; Sign-in activity by suspected North Korean entity Jasper Sleet</td></tr></tbody></table></figure>



<h4 class="wp-block-heading" id="threat-intelligence-reports">Threat intelligence reports</h4>



<p class="wp-block-paragraph">Microsoft Defender XDR customers can use the following threat analytics reports in the Defender portal (requires license for at least one Defender XDR product) to get the most up-to-date information about the threat actor, malicious activity, and techniques discussed in this blog. These reports provide the intelligence, protection information, and recommended actions to prevent, mitigate, or respond to associated threats found in customer environments.</p>



<ul class="wp-block-list">
<li class="wp-block-list-item"><a href="https://security.microsoft.com/threatanalytics3/3ed6ce54-7d04-4819-aa9a-7f3085ea4a49/overview">Actor profile: Jasper Sleet</a></li>
</ul>



<p class="wp-block-paragraph">Microsoft Security Copilot customers can also use the <a href="https://learn.microsoft.com/defender/threat-intelligence/security-copilot-and-defender-threat-intelligence?bc=%2Fsecurity-copilot%2Fbreadcrumb%2Ftoc.json&amp;toc=%2Fsecurity-copilot%2Ftoc.json#turn-on-the-security-copilot-integration-in-defender-ti">Microsoft Security Copilot integration</a> in Microsoft Defender Threat Intelligence, either in the Security Copilot standalone portal or in the <a href="https://learn.microsoft.com/defender/threat-intelligence/using-copilot-threat-intelligence-defender-xdr">embedded experience</a> in the Microsoft Defender portal to get more information about this threat actor.</p>



<h4 class="wp-block-heading" id="hunting-queries">Hunting queries</h4>



<p class="wp-block-paragraph">Microsoft Defender XDR customers can run the following queries to find related activity to any suspicious indicators in their networks:</p>



<p class="wp-block-paragraph"><strong>Access to Workday Recruiting Web Service API by external users</strong></p>



<p class="wp-block-paragraph"></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; auto-links: false; gutter: false; title: ; quick-code: false; notranslate">
let api_endpoint_regex = 'hrrecruiting/*';
CloudAppEvents
| where Application == 'Workday'
| where IsExternalUser
| where ActionType matches regex api_endpoint_regex
| where IPAddress in (<​suspiciousips>) or AccountId in (<​suspicious_emailids>);
| summarize make_set(ActionType) by AccountId, IPAddress, bin(Timestamp, 1d)
</pre></div>


<p class="wp-block-paragraph"><strong>Emails and Teams communications related to interviews</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; auto-links: false; gutter: false; title: ; quick-code: false; notranslate">
//Email communications

EmailEvents 
| where SenderMailFromAddress == "<​suspicious_emailids>" or RecipientEmailAddress == "<​suspicious_emailids>"
| where Subject has "Interview"
| project Timestamp, SenderMailFromAddress, SenderDisplayName, SenderIPv4, SenderIPv6, RecipientEmailAddress, Subject, DeliveryAction, DeliveryLocation

EmailEvents 
| where SenderIPv4 == "<​suspiciousIPs>" or SenderIPv6 == "<​ suspiciousIPs>"
| where Subject has "Interview"
| project Timestamp, SenderMailFromAddress, SenderDisplayName, SenderIPv4, SenderIPv6, RecipientEmailAddress, Subject, DeliveryAction, DeliveryLocation

//Microsoft Teams communications

CloudAppEvents
| where Application == "Microsoft Teams"
| where IsExternalUser
| where AccountId == "<​suspicious_emailids>" or AccountDisplayName == "<​suspicious_emailids>"
| summarize make_set(ActionType) by IPAddress, AccountId, bin(Timestamp, 1d)

CloudAppEvents
| where Application == "Microsoft Teams"
| where IsExternalUser
| where IPAddress == "<​suspiciousIPs​>" 
| summarize make_set(ActionType) by IPAddress, AccountId, bin(Timestamp, 1d)

//Zoom or Cisco Webex communication events after enabling the Microsoft Defender for Cloud apps connectors

CloudAppEvents
| where Application == "Zoom"
| where IsExternalUser
| where IPAddress == "<​suspiciousIP​s>" 
| summarize make_set(ActionType) by IPAddress, AccountId, bin(Timestamp, 1d)

CloudAppEvents
| where Application == "Cisco Webex"
| where IsExternalUser
| where IPAddress == "<​suspiciousIPs​>"
| summarize make_set(ActionType) by IPAddress, AccountId, bin(Timestamp, 1d)
</pre></div>


<p class="wp-block-paragraph"><strong>Hiring phase involving accessing and signing of agreements through DocuSign</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; auto-links: false; gutter: false; title: ; quick-code: false; notranslate">
CloudAppEvents
| where Application == "DocuSign"
| where IsExternalUser
| where ActionType == "ENVELOPE SIGNED"
| where IPAddress in ("<​suspiciousIPs>") or AccountId == "<​suspicious_emailids>"
</pre></div>


<p class="wp-block-paragraph"><strong>New hire onboarding and payroll activities originating from known Jasper Sleet infrastructure</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; auto-links: false; gutter: false; title: ; quick-code: false; notranslate">
CloudAppEvents
| where Application == "Workday"
| where AccountId == "<​NewHireWorkdayId>"
| where ActionType has_any ("Add", "Change", "Assign", "Create", "Modify") and ActionType has_any ("Account", "Bank", "Payment", "Tax")
| where IPAddress in ("<​suspiciousIPs>")
| summarize make_set(ActionType) by IPAddress, bin(Timestamp, 1d)
</pre></div>


<h4 class="wp-block-heading" id="references">References</h4>



<ul class="wp-block-list">
<li class="wp-block-list-item"><a href="https://www.microsoft.com/en-us/security/blog/2025/06/30/jasper-sleet-north-korean-remote-it-workers-evolving-tactics-to-infiltrate-organizations/">Jasper Sleet: North Korean remote IT workers’ evolving tactics to infiltrate organizations | Microsoft Security Blog</a></li>



<li class="wp-block-list-item"><a href="https://www.microsoft.com/en-us/security/blog/2026/03/06/ai-as-tradecraft-how-threat-actors-operationalize-ai/">AI as tradecraft: How threat actors operationalize AI | Microsoft Security Blog</a></li>
</ul>



<p class="wp-block-paragraph"><em>This research is provided by Microsoft Defender Security Research with contributions from  members of Microsoft Threat Intelligence.</em></p>



<h4 class="wp-block-heading" id="learn-more">Learn more</h4>



<p class="wp-block-paragraph">For the latest security research from the Microsoft Threat Intelligence community, check out the <a href="https://aka.ms/threatintelblog">Microsoft Threat Intelligence Blog</a>.</p>



<p class="wp-block-paragraph">To get notified about new publications and to join discussions on social media, follow us on <a href="https://www.linkedin.com/showcase/microsoft-threat-intelligence">LinkedIn</a>, <a href="https://x.com/MsftSecIntel">X (formerly Twitter)</a>, and <a href="https://bsky.app/profile/threatintel.microsoft.com">Bluesky</a>.</p>



<p class="wp-block-paragraph">To hear stories and insights from the Microsoft Threat Intelligence community about the ever-evolving threat landscape, listen to the <a href="https://thecyberwire.com/podcasts/microsoft-threat-intelligence">Microsoft Threat Intelligence podcast</a>.</p>



<p class="wp-block-paragraph">Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Learn more about <a href="https://learn.microsoft.com/en-us/defender-cloud-apps/ai-agent-protection" rel="noreferrer noopener" target="_blank">securing Copilot Studio agents with Microsoft Defender</a> &nbsp;</li>



<li class="wp-block-list-item">Evaluate your AI readiness with our latest&nbsp;<a href="https://microsoft.github.io/zerotrustassessment/" rel="noreferrer noopener" target="_blank">Zero Trust for AI workshop</a>.</li>



<li class="wp-block-list-item">Learn more about <a href="https://learn.microsoft.com/en-us/defender-cloud-apps/real-time-agent-protection-during-runtime" rel="noreferrer noopener" target="_blank">Protect your agents in real-time during runtime (Preview)</a></li>



<li class="wp-block-list-item">Explore <a href="https://eurppc-word-edit.officeapps.live.com/we/%E2%80%A2%09https:/learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/copilot-studio-agent-builder" rel="noreferrer noopener" target="_blank">how to build and customize agents with Copilot Studio Agent Builder</a>&nbsp;</li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/en-us/copilot/microsoft-365/microsoft-365-copilot-ai-security" rel="noreferrer noopener" target="_blank">Microsoft 365 Copilot AI security documentation</a>&nbsp;</li>



<li class="wp-block-list-item"><a href="https://www.microsoft.com/en-us/security/blog/2024/04/11/how-microsoft-discovers-and-mitigates-evolving-attacks-against-ai-guardrails/" rel="noreferrer noopener" target="_blank">How Microsoft discovers and mitigates evolving attacks against AI guardrails</a>&nbsp;</li>
</ul>



<p class="wp-block-paragraph"></p>
<p>The post <a href="https://www.microsoft.com/en-us/security/blog/2026/04/21/detection-strategies-cloud-identities-against-infiltrating-it-workers/">Detection strategies across cloud and identities against infiltrating IT workers</a> appeared first on <a href="https://www.microsoft.com/en-us/security/blog">Microsoft Security Blog</a>.</p>
