---
title: "Cross‑tenant helpdesk impersonation to data exfiltration: A human-operated intrusion playbook"
url: "https://www.microsoft.com/en-us/security/blog/2026/04/18/crosstenant-helpdesk-impersonation-data-exfiltration-human-operated-intrusion-playbook/"
date: "Sat, 18 Apr 2026 12:55:45 +0000"
author: "Microsoft Defender Security Research Team and Microsoft Defender Experts"
feed_url: "https://www.microsoft.com/en-us/security/blog/feed/"
---
<aside class="table-of-contents-block accordion wp-block-bloginabox-theme-table-of-contents" id="accordion-9bdb4ecd-a135-4ac9-a25d-958d9f233be5">
	<button class="btn btn-collapse" type="button">
		<span class="table-of-contents-block__label">In this article</span>
		<span class="table-of-contents-block__current"></span>

		<svg class="table-of-contents-block__arrow" fill="none" height="11" viewBox="0 0 18 11" width="18" xmlns="http://www.w3.org/2000/svg">
			<path d="M15.7761 11L18 8.82043L9 0L0 8.82043L2.22394 11L9 4.35913L15.7761 11Z" fill="currentColor">
		</svg>
	</button>
	<div class="table-of-contents-block__collapse-wrapper collapse show" id="accordion-collapse-9bdb4ecd-a135-4ac9-a25d-958d9f233be5">
		<div class="table-of-contents-block__content">
			<ol class="table-of-contents-block__list"><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#risk-to-enterprise-environments">Risk to enterprise environments</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#attack-chain-overview">Attack chain overview</a><ol class="table-of-contents-block__list"><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-1-initial-contact-via-teams-t1566-003-spearphishing-via-service">Stage 1: Initial contact via Teams (T1566.003 Spearphishing via Service)</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-2-remote-assistance-foothold">Stage 2: Remote assistance foothold</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-3-interactive-reconnaissance-and-access-validation">Stage 3: Interactive reconnaissance and access validation</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-4-payload-placement-and-trusted-application-invocation">Stage 4: Payload placement and trusted application invocation</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-5-execution-context-validation-and-registry-backed-loader-state">Stage 5: Execution context validation and registry backed loader state</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-6-command-and-control">Stage 6: Command and control</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-7-internal-discovery-and-lateral-movement-toward-high-value-assets">Stage 7: Internal discovery and lateral movement toward high value assets</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-8-remote-deployment-of-auxiliary-access-tooling-level-rmm">Stage 8: Remote deployment of auxiliary access tooling (Level RMM)</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#stage-9-data-exfiltration">Stage 9: Data exfiltration</a></li></ol></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#mitigation-and-protection-guidance">Mitigation and protection guidance</a><ol class="table-of-contents-block__list"><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#microsoft-protection-outcomes">Microsoft protection outcomes</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#microsoft-defender-xdr-detections">Microsoft Defender XDR detections</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#hunting-queries">Hunting queries</a></li></ol></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#references">References</a></li><li class="table-of-contents-block__list-item"><a class="table-of-contents-block__list-item-link" href="#learn-more">Learn More</a></li></ol>		</div>
	</div>
	<span class="table-of-contents-block__progress-bar"></span>
</aside>



<p class="wp-block-paragraph">Threat actors are initiating cross-tenant Microsoft Teams communications&nbsp;while impersonating IT or helpdesk personnel to socially engineer users into granting remote desktop access. After access is established through Quick Assist or similar remote support tools, attackers often execute trusted vendor-signed applications alongside attacker-supplied modules to enable malicious code execution. </p>



<p class="wp-block-paragraph">This access pathway might be used to perform credential-backed lateral movement using native administrative protocols such as Windows Remote Management (WinRM), allowing threat actors to pivot toward high-value assets including domain controllers. In observed intrusions, follow-on commercial remote management software and data transfer utilities such as Rclone were used to expand access across the enterprise environment and stage business-relevant information for transfer to external cloud storage. This intrusion chain relies heavily on legitimate applications and administrative protocols, allowing threat actors to blend into expected enterprise activity during multiple intrusion phases.</p>



<p class="wp-block-paragraph">Threat actors are increasingly abusing external Microsoft Teams collaboration to impersonate IT or helpdesk personnel and convince users to grant remote assistance access. From this initial foothold, attackers can leverage trusted tools and native administrative protocols to move laterally across the enterprise and stage sensitive data for exfiltration—often blending into routine IT support activity throughout the intrusion lifecycle. Microsoft Defender provides correlated visibility across identity, endpoint, and collaboration telemetry to help detect and disrupt this user‑initiated access pathway before it escalates into broader compromise.</p>



<h2 class="wp-block-heading" id="risk-to-enterprise-environments">Risk to enterprise environments</h2>



<p class="wp-block-paragraph">By abusing enterprise collaboration workflows instead of traditional email based phishing channels, attackers may initiate contact through applications such as Microsoft Teams in a way that appears consistent with routine IT support interactions. </p>



<p class="wp-block-paragraph">Microsoft Teams applies multiple security controls at the point of first external contact &#8211; before any chat, call, or file exchange occurs &#8211; including external tenant labeling, Accept/Block prompts, message previews, and phishing indicators designed to help users assess risk prior to engagement. However, this attack chain relies on convincing users to bypass those warnings and voluntarily grant remote access through legitimate support tools. In observed intrusions, risk is introduced not by external messaging alone, but when a user approves follow on actions — such as launching a remote assistance session — that result in interactive system access.</p>



<p class="wp-block-paragraph">In observed intrusions, risk is introduced not by external messaging alone, but when a user approves follow‑on actions — such as launching a remote assistance session — that result in interactive system access.</p>



<p class="wp-block-paragraph">An approved external Teams interaction might enable threat actors to:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Establish credential-backed interactive system access&nbsp;</li>



<li class="wp-block-list-item">Deploy trusted applications to execute attacker-controlled code&nbsp;</li>



<li class="wp-block-list-item">Pivot toward identity and domain infrastructure using WinRM&nbsp;</li>



<li class="wp-block-list-item">Deploy commercially available remote management tooling&nbsp;</li>



<li class="wp-block-list-item">Stage sensitive business-relevant data for transfer to external cloud infrastructure&nbsp;</li>
</ul>



<p class="wp-block-paragraph">In the campaign, lateral movement and follow-on tooling installation occurred shortly after initial access, increasing the risk of enterprise-wide persistence and targeted data exfiltration. As each environment is different and with potential handoff to different threat actors, stages might differ if not outright bypassed.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146658 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-38.webp" /><figcaption class="wp-element-caption">Figure 1: Attack chain.</figcaption></figure>



<h2 class="wp-block-heading" id="attack-chain-overview">Attack chain overview<a id="_msocom_1"></a></h2>



<h3 class="wp-block-heading" id="stage-1-initial-contact-via-teams-t1566-003-spearphishing-via-service">Stage 1: Initial contact via Teams (T1566.003 Spearphishing via Service)</h3>



<p class="wp-block-paragraph">The intrusion begins with abuse of external collaboration features in Microsoft Teams, where an attacker operating from a separate tenant initiates contact while impersonating internal support personnel as a means to social engineer the user. This activity does not stem from a weakness in Microsoft Teams or its built‑in security protections. Instead, attackers abuse legitimate collaboration features by persuading users to override multiple, clearly presented security warnings, highlighting the broader challenge of defending against attacks driven by social engineering rather than technical exploitation.</p>



<p class="wp-block-paragraph">Because interaction occurs within an enterprise collaboration platform rather than through traditional email‑based phishing vectors, it might bypass initial user skepticism associated with unsolicited external communication. Security features protecting Teams users are detailed <a href="https://learn.microsoft.com/en-us/microsoftteams/teams-security-guide">here</a>, for reference. It’s important to note that this attack relies on users willfully ignoring or overlooking security notices and other protection features.&nbsp; The lure varies and might include “Microsoft Security Update”, “Spam Filter Update”, “Account Verification” but the objective is constant: convince the user to ignore warnings and external contact flags, launch a remote management session, and accept elevation. Voice phishing (vishing) is sometimes layered to increase trust or compliance if voice interactions don’t replace the messaging altogether.</p>



<p class="wp-block-paragraph">Timing matters. We regularly see a “ChatCreated” event to indicate a first contact situation, followed by suspicious chats or vishing, remote management, and other events that commonly produce alerts to include mailbombing or URL click alerts.&nbsp;All of these can be correlated by account and chat thread information in your Defender hunting environment.</p>



<p class="wp-block-paragraph">Teams security warnings:</p>



<p class="wp-block-paragraph">External Accept/Block screens provide notice to users about First Contact events, which prompt the user to inspect the sender’s identity before accepting:</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146659 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-39.webp" /><figcaption class="wp-element-caption">Figure 2: External Accept/Block screens.</figcaption></figure>



<p class="wp-block-paragraph">Higher confidence warnings alert the user of spam or phishing attempts on first contact:<a id="_msocom_1"></a></p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146660 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-40.webp" /><figcaption class="wp-element-caption">Figure 3: spam or phishing alert.</figcaption></figure>



<p class="wp-block-paragraph">External warnings notify users that they are communicating with a tenant/organization other than their own and should be treated with scrutiny:</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146661 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-41.webp" /><figcaption class="wp-element-caption">Figure 4: External warnings.</figcaption></figure>



<p class="wp-block-paragraph">Message warnings alert the user on the risk in clicking the URL:</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146663 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-43.webp" /><figcaption class="wp-element-caption">Figure 5: URL click warning.</figcaption></figure>



<p class="wp-block-paragraph">Safe Links for time-of-click protection warns users when URLs from Teams chat messages are malicious:</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146664 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-44.webp" /><figcaption class="wp-element-caption">Figure 6: time-of-click protection warning.</figcaption></figure>



<p class="wp-block-paragraph">Zero-hour Auto Purge (ZAP) can remove messages that were flagged as malicious after they have been sent:</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146665 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-45.webp" /><figcaption class="wp-element-caption">Figure 7: Removed malicious from ZAP.</figcaption></figure>



<p class="wp-block-paragraph">It’s important to note that the attacker often does not send the URL over a Teams message. Instead, they will navigate to it while on the endpoint during a remote management session. Therefore, the best security is user education on understanding the importance of not ignoring external flags for new helpdesk contacts. See “User education” in the “Defend, harden, and educate (Controls to deploy&nbsp;now)” section for further advice.</p>



<h3 class="wp-block-heading" id="stage-2-remote-assistance-foothold">Stage 2: Remote assistance foothold<a id="_msocom_1"></a></h3>



<p class="wp-block-paragraph">With user consent obtained through social engineering, the attacker gains interactive control of the device using remote support tools such as Quick Assist. This access typically results in the launch of QuickAssist.exe, followed by the display of standard Windows elevation prompts through Consent.exe as the attacker is guided through approval steps.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146666 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-46.webp" /><figcaption class="wp-element-caption">Figure 8: Quick Assist Key Logs.</figcaption></figure>



<p class="wp-block-paragraph">From the user’s perspective, the attacker &nbsp;convinces them to open Quick Assist, enter a short key, the follow all prompts and approvals to grant access.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146667 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-47.webp" /><figcaption class="wp-element-caption">Figure 9 &#8211; Quick Assist Launch.</figcaption></figure>



<p class="wp-block-paragraph">This step is often completed in under a minute. The urgency and interactivity are the signal: a remote‑assist process tree followed immediately by “cmd.exe” or PowerShell on the same desktop.</p>



<h3 class="wp-block-heading" id="stage-3-interactive-reconnaissance-and-access-validation">Stage 3: Interactive reconnaissance and access validation</h3>



<p class="wp-block-paragraph">Immediately after establishing control through Quick Assist, the attacker typically spends the first 30–120 seconds assessing their level of access and understanding the compromised environment. This is often reflected by a brief surge of <em>cmd.exe</em> activity, used to verify user context and privilege levels, gather basic system information such as host identity and operating system details, and confirm domain affiliation. In parallel, the attacker might query registry values to determine OS build and edition, while also performing quick network reconnaissance to evaluate connectivity, reachability, and potential opportunities for lateral movement.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146668 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-48.webp" /><figcaption class="wp-element-caption">Figure 10: Enumeration.</figcaption></figure>



<p class="wp-block-paragraph">On systems with limited privileges—such as kiosks, VDI, or non-corp-joined devices—actors might pause without deploying payloads, leaving only brief reconnaissance activity. They often return later when access improves or pivot to other targets within the same tenant.</p>



<h3 class="wp-block-heading" id="stage-4-payload-placement-and-trusted-application-invocation">Stage 4: Payload placement and trusted application invocation</h3>



<p class="wp-block-paragraph">Once remote access is established, the intrusion transitions from user‑assisted interaction to preparing the environment for persistent execution. At this point, attackers introduce a small staging bundle onto disk using either archive‑based deployment or short‑lived scripting activity. As activity moves beyond initial social engineering, Microsoft security protections shift from user‑facing warnings to behavior‑based detection, correlation, and automated response across identity, endpoint, and network layers.</p>



<p class="wp-block-paragraph">After access is established, attackers stage payloads in locations such as ProgramData and execute them using DLL side‑loading through trusted signed applications. This includes: </p>



<ul class="wp-block-list">
<li class="wp-block-list-item">AcroServicesUpdater2_x64.exe loading a staged msi.dll</li>



<li class="wp-block-list-item">ADNotificationManager.exe loading vcruntime140_1.dll</li>



<li class="wp-block-list-item">DlpUserAgent.exe loading mpclient.dll</li>



<li class="wp-block-list-item">werfault.exe loading Faultrep.dll</li>
</ul>



<p class="wp-block-paragraph">Allowing attacker‑supplied modules to run under a trusted execution context from non‑standard paths.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146669 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-49.webp" /><figcaption class="wp-element-caption">Figure 11: Sample Payload.</figcaption></figure>



<h3 class="wp-block-heading" id="stage-5-execution-context-validation-and-registry-backed-loader-state">Stage 5: Execution context validation and registry backed loader state<a id="_msocom_1"></a></h3>



<p class="wp-block-paragraph">Following payload delivery, the attacker performs runtime checks to validate host conditions before execution. A large encoded value is then written to a user‑context registry location, serving as a staging container for encrypted configuration data to be retrieved later at runtime.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146670 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-50.webp" /><figcaption class="wp-element-caption">Figure 12: Representative commands / actions (sanitized).</figcaption></figure>



<p class="wp-block-paragraph">In this stage, a sideloaded module acting as an intermediary loader decrypts staged registry data in memory to reconstruct execution and C2 configuration without writing files to disk. This behavior aligns with intrusion frameworks such as Havoc, which externalize encrypted configuration to registry storage, allowing trusted sideloaded components to dynamically recover execution context and maintain operational continuity across restarts or remediation events.</p>



<p class="wp-block-paragraph">Microsoft Defender for Endpoint may detect this activity as:</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Unexpected DLL load by trusted application</li>



<li class="wp-block-list-item">Service‑path execution outside vendor installation directory</li>



<li class="wp-block-list-item">Execution from user‑writable directories such as ProgramData</li>
</ul>



<p class="wp-block-paragraph">Attack surface reduction rules and Windows Defender Application Control policies can be used to restrict execution pathways commonly leveraged for sideloaded module activation.<a id="_msocom_3"></a></p>



<h3 class="wp-block-heading" id="stage-6-command-and-control">Stage 6: Command and control</h3>



<p class="wp-block-paragraph">Following successful execution of the sideloaded component, the updater‑themed process <em>AcroServicesUpdater2_x64.exe</em> began initiating outbound HTTPS connections over TCP port 443 to externally hosted infrastructure.</p>



<p class="wp-block-paragraph">Unlike expected application update workflows which are typically restricted to known vendor services these connections were directed toward dynamically hosted cloud‑backed endpoints and unknown external domains. This behavior indicates remote attacker‑controlled infrastructure rather than legitimate update mechanisms.</p>



<p class="wp-block-paragraph">Establishing outbound encrypted communications in this manner enables compromised processes to operate as beaconing implants, allowing adversaries to remotely retrieve instructions and maintain control within the affected environment while blending command traffic into routine HTTPS activity. The use of cloud‑hosted hosting layers further reduces infrastructure visibility and improves the attacker’s ability to modify or rotate communication endpoints without altering the deployed payload.</p>



<p class="wp-block-paragraph">This activity marks the transition from local execution to externally directed command‑and‑control — enabling subsequent stages of discovery and movement inside the enterprise network.</p>



<h3 class="wp-block-heading" id="stage-7-internal-discovery-and-lateral-movement-toward-high-value-assets">Stage 7: Internal discovery and lateral movement toward high value assets</h3>



<p class="wp-block-paragraph">Shortly after external communications were established, the compromised process began initiating internal remote management connections over WinRM (TCP&nbsp;5985) toward additional domain‑joined systems within the enterprise environment.</p>



<p class="wp-block-paragraph">Microsoft Defender may surface these activities as multi‑device incidents reflecting credential‑backed lateral movement initiated from a user‑context remote session.</p>



<p class="wp-block-paragraph">Analysis of WinRM activity indicates that the threat actor used native Windows remote execution to pivot from the initially compromised endpoint toward high‑value infrastructure assets, including identity and domain management systems such as domain controllers. Use of WinRM from a non‑administrative application suggests credential‑backed lateral movement directed by an external operator, enabling remote command execution, interaction with domain infrastructure, and deployment of additional tooling onto targeted hosts.</p>



<p class="wp-block-paragraph">Targeting identity‑centric infrastructure at this stage reflects a shift from initial foothold to broader enterprise control and persistence. Notably, this internal pivot preceded the remote deployment of additional access tooling in later stages, indicating that attacker‑controlled WinRM sessions were subsequently leveraged to extend sustained access across</p>



<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td>Protocol: &#8220;HTTP&#8221; <br />Entity Type: &#8220;IP&#8221; <br />Ip: &lt;IP Address&gt; <br />Target: &#8220;http://host.domain.local:5985/wsman&#8221; <br />RequestUserAgent: &#8220;Microsoft WinRM Client&#8221;</td></tr></tbody></table></figure>



<hr class="wp-block-separator has-alpha-channel-opacity" />



<h3 class="wp-block-heading" id="stage-8-remote-deployment-of-auxiliary-access-tooling-level-rmm">Stage 8: Remote deployment of auxiliary access tooling (Level RMM)<a id="_msocom_1"></a></h3>



<p class="wp-block-paragraph">Subsequent activity revealed the remote installation of an additional management platform across compromised hosts using Windows Installer (msiexec.exe). This introduced an alternate control channel independent of the original intrusion components, reducing reliance on the initial implant and enabling sustained access through standard administrative mechanisms. As a result, attackers could maintain persistent remote control even if earlier payloads were disrupted or removed.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146672 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-51.webp" /></figure>



<h3 class="wp-block-heading" id="stage-9-data-exfiltration">Stage 9: Data exfiltration</h3>



<p class="wp-block-paragraph">Actors used the file‑synchronization tool Rclone to transfer data from internal network locations to an external cloud storage service. File‑type exclusions in the transfer parameters suggest a targeted effort to exfiltrate business‑relevant documents while minimizing transfer size and detection risk.</p>


<figure class="wp-block-image size-full"><img alt="" class="wp-image-146673 webp-format" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2026/04/image-52.webp" /></figure>



<p class="wp-block-paragraph">Microsoft Defender might detect this activity as possible data exfiltration involving uncommon synchronization tooling.</p>



<h2 class="wp-block-heading" id="mitigation-and-protection-guidance">Mitigation and protection guidance</h2>



<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td><strong>Family / Product</strong></td><td><strong>Protection</strong></td><td><strong>Reference documents</strong></td></tr><tr><td>Microsoft Teams</td><td>Review external collaboration policies and ensure users receive clear external sender notifications when interacting with cross‑tenant contacts. Consider device‑ or identity‑based access requirements prior to granting remote support sessions.</td><td><a href="https://learn.microsoft.com/en-us/microsoftteams/trusted-organizations-external-meetings-chat">https://learn.microsoft.com/en-us/microsoftteams/trusted-organizations-external-meetings-chat</a> and <a href="https://learn.microsoft.com/en-us/defender-office-365/mdo-support-teams-about">https://learn.microsoft.com/en-us/defender-office-365/mdo-support-teams-about</a></td></tr><tr><td>Microsoft Defender for Office 365</td><td>Enable Safe Links for Teams conversations with time-of-click verification, and ensure zero-hour auto purge (ZAP) is active to retroactively quarantine weaponized messages.</td><td><a href="https://learn.microsoft.com/en-us/defender-office-365/safe-links-about">https://learn.microsoft.com/en-us/defender-office-365/safe-links-about</a></td></tr><tr><td>Microsoft Defender for Endpoint</td><td>Disable or restrict remote management tools to authorized roles, enable standard ASR rules in block mode, and apply WDAC to prevent DLL sideloading from ProgramData and AppData paths used by these actors.</td><td><a href="https://learn.microsoft.com/en-us/defender-endpoint/attack-surface-reduction-rules-reference">https://learn.microsoft.com/en-us/defender-endpoint/attack-surface-reduction-rules-reference</a></td></tr><tr><td>Microsoft Entra ID</td><td>Enforce Conditional Access requiring MFA and compliant devices for administrative roles, restrict WinRM to authorized management workstations, and monitor for Rclone or similar synchronization utilities used for data exfiltration via hunting or custom alerts tuned to your environment.</td><td><a href="https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview">https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview</a> and <a href="https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-overview">https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-overview</a> and <a href="https://learn.microsoft.com/en-us/defender-xdr/custom-detections-overview">https://learn.microsoft.com/en-us/defender-xdr/custom-detections-overview</a></td></tr><tr><td>Network Controls</td><td>Enable network protection to block implant C2 beaconing to poor-reputation and newly registered domains, and alert on registry modifications to ASEP locations by non-installer processes.&nbsp; Hunting and custom detections tuned to your environment will assist in detecting network threats.</td><td><a href="https://learn.microsoft.com/en-us/defender-endpoint/network-protection">https://learn.microsoft.com/en-us/defender-endpoint/network-protection</a></td></tr><tr><td>Education</td><td>The attackers will often initiate Teams calls with their targets to talk them through completing actions that result in machine compromise. It may be useful to establish a verbal authentication code between IT Helpdesk and employees: a key phrase that an attacker is unlikely to know. Inform employees how IT Helpdesk would normally reach out to them: which medium(s) of communication? Email, Teams, Phone calls, etc. What identifiers would those IT Helpdesk contacts have? Domain names, aliases, phone numbers, etc. Show example images of your Helpdesk vs. an attacker impersonating them over your communication medium. &nbsp;Show examples of how to identify external versus internal Teams communications, block screens, message and call reporting, as well as how to identify a display name vs. the real caller’s name and domain.&nbsp; Inform employees that URLs shared by an external Helpdesk account leading to Safe Links warnings about malicious websites are extremely suspicious. They should report the message as phish and contact your security team. &nbsp; If they receive any URLs from IT Helpdesk that involve going to a webpage for security updates or spam mailbox cleanings, then they should report that to your security team. &nbsp;Treat unsolicited and unexpected external contact from IT Helpdesk as inherently suspicious.</td><td><a href="https://www.microsoft.com/en-us/security/blog/2025/10/07/disrupting-threats-targeting-microsoft-teams/">Disrupting threats targeting Microsoft Teams | Microsoft Security Blog</a></td></tr></tbody></table></figure>



<hr class="wp-block-separator has-alpha-channel-opacity" />



<h3 class="wp-block-heading" id="microsoft-protection-outcomes">Microsoft protection outcomes<a id="_msocom_1"></a></h3>



<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td><strong>Family / Product</strong></td><td><strong>Protection in addition to detections.</strong></td><td><strong>Reference Documents</strong></td></tr><tr><td>AI driven detection &amp; attack disruption</td><td>When Defender detects credential‑backed WinRM lateral movement following a Quick Assist session, Automatic Attack Disruption can suspend the originating user session and contain the users prior to domain‑controller interaction &nbsp;— limiting lateral movement before your SOC engages. Look for incidents tagged “Attack Disruption” in your queue.</td><td><a href="https://learn.microsoft.com/en-us/defender-xdr/automatic-attack-disruption">https://learn.microsoft.com/en-us/defender-xdr/automatic-attack-disruption</a> and <a href="https://learn.microsoft.com/en-us/defender-xdr/configure-attack-disruption">https://learn.microsoft.com/en-us/defender-xdr/configure-attack-disruption</a></td></tr><tr><td>Cross-family / product incident correlation</td><td>Teams/MDO, Entra ID, and MDE signals are automatically correlated into unified incidents. This entire attack chain surfaces as one multi-stage incident — not dozens of disconnected alerts. Review “Multi-stage” incidents for the full story.</td><td><a href="https://learn.microsoft.com/en-us/defender-xdr/incident-queue">https://learn.microsoft.com/en-us/defender-xdr/incident-queue</a></td></tr><tr><td>Threat analytics and continuous tuning</td><td>Threat analytics reports for these TTPs include exposure assessments and mitigations for your environment. Detection logic is continuously updated to reflect evolving tradecraft. Check your Threat Analytics dashboard for reports tagged to these Storm actors.</td><td><a href="https://learn.microsoft.com/en-us/defender-xdr/threat-analytics">https://learn.microsoft.com/en-us/defender-xdr/threat-analytics</a></td></tr><tr><td>Teams external message accept/block controls</td><td>When an external user initiates contact, Teams presents the recipient with a message preview and an explicit Accept or Block prompt before any conversation begins.&nbsp; Blocking prevents future messages and hides your presence status from that sender.</td><td><a href="https://learn.microsoft.com/en-us/microsoftteams/teams-security-best-practices-for-safer-messaging">https://learn.microsoft.com/en-us/microsoftteams/teams-security-best-practices-for-safer-messaging</a></td></tr><tr><td>Security recommendations</td><td>Following security recommendations can help in improving the security posture of the org. Apply UAC restrictions to local accounts on network logonsSafe DLL Search ModeEnable Network ProtectionDisable &#8216;Allow Basic authentication&#8217; for WinRM Client/Service</td><td><a href="https://learn.microsoft.com/en-us/defender-vulnerability-management/tvm-security-recommendation">https://learn.microsoft.com/en-us/defender-vulnerability-management/tvm-security-recommendation</a></td></tr></tbody></table></figure>



<hr class="wp-block-separator has-alpha-channel-opacity" />



<h3 class="wp-block-heading" id="microsoft-defender-xdr-detections">Microsoft Defender XDR detections<a id="_msocom_1"></a></h3>



<p class="wp-block-paragraph">Microsoft Defender provides pre-breach and post-breach coverage for this campaign, supported by the &nbsp;generic and specific alerts listed below.</p>



<figure class="wp-block-table"><table class="has-fixed-layout"><tbody><tr><td><strong>Tactic</strong></td><td><strong>Observed activity</strong></td><td><strong>Microsoft Defender coverage</strong></td></tr><tr><td>Initial Access</td><td>The actor&nbsp;initiates&nbsp;a cross‑tenant Teams chat or&nbsp;call from an often newly created&nbsp;tenant using an IT/Help‑Desk persona</td><td><strong>Microsoft Defender for Office 365</strong> &#8211; Microsoft Teams chat initiated by a suspicious external user &#8211; IT Support Teams Voice phishing following mail bombing activity &#8211; A user clicked through to a potentially malicious URL. &#8211; A potentially malicious URL click was detected. <strong>&nbsp;</strong> <br /><br /><strong>Microsoft Defender for Endpoint</strong> &#8211; Possible initial access from an emerging threat</td></tr><tr><td>Execution&nbsp;</td><td>The attacker gains interactive control via&nbsp;remote management tools&nbsp;to include&nbsp;Quick Assist.</td><td><strong>Microsoft Defender for Endpoint</strong><br />&#8211; Suspicious activity using Quick Assist &#8211; Uncommon remote access software &#8211; Remote monitoring and management software suspicious activity<br /><br /><strong>Microsoft Defender Antivirus</strong><br />&#8211; Trojan:Win64/DllHijack.VGA!MTB &#8211; Trojan:Win64/DllHijack.VGB!MTB &#8211; Trojan:Win64/Tedy!MTB&nbsp; &#8211; Trojan.Win64.Malgent&nbsp; &#8211; Trojan:Win64/Zusy!MTB</td></tr><tr><td>Lateral Movement</td><td>Attacker pivots via WinRM to target high￼value assets (e.g., domain controllers).</td><td><strong>Microsoft Defender for Endpoint</strong><br />&#8211; Suspicious sign-in activity &#8211; Potential human-operated malicious activity &#8211; Hands-on-keyboard attack involving multiple devices</td></tr><tr><td>Persistence</td><td>Runtime environment validated and encoded loader state stored within user registry.</td><td><strong>Microsoft Defender for Endpoint</strong><br />&#8211; Suspicious registry modification</td></tr><tr><td>Defense Evasion &amp; Privilege Escalation</td><td>DLL Side-Loading (e.g., AcroServicesUpdater2_x64.exe, ADNotificationManager.exe,&nbsp;or DlpUserAgent.exe)</td><td><strong>Microsoft Defender for Endpoint</strong><br />&#8211; An executable file loaded an unexpected DLL file <br /><br /><strong>Microsoft Defender Antivirus</strong><br />&#8211; Trojan:Win64/DllHijack.VGA!MTB &#8211; Trojan:Win64/DllHijack.VGB!MTB &#8211; Trojan:Win64/Tedy!MTB&nbsp; &#8211; Trojan.Win64.Malgent&nbsp; &#8211; Trojan:Win64/Zusy!MTB</td></tr><tr><td>Command &amp; Control</td><td>The implant or sideloaded host typically beacons over HTTPS</td><td><strong>Microsoft Defender for Endpoint</strong><br />&#8211; Connection to a custom network indicator &#8211; A file or network connection related to a ransomware-linked emerging threat activity group detected</td></tr><tr><td>Data Exfiltration</td><td>Widely available&nbsp;file‑synchronization&nbsp;utility Rclone to systematically transfer data</td><td><strong>Microsoft Defender for Endpoint</strong><br />&#8211; Possible data exfiltration</td></tr><tr><td>Multi-tactic</td><td>Many alerts span across multiple tactics or stages of an attack and cover many platforms.</td><td><strong>Microsoft Defender (All)</strong> &#8211; Multi-stage incident involving Execution &#8211; Remote management event after suspected Microsoft Teams IT support phishing &#8211; An Office application ran suspicious commands</td></tr></tbody></table></figure>



<h3 class="wp-block-heading" id="hunting-queries">Hunting queries</h3>



<p class="wp-block-paragraph">Security teams can use the advanced hunting capabilities in Microsoft Defender XDR to proactively look for indicators of exploitation.<a id="_msocom_1"></a></p>



<p class="wp-block-paragraph"><strong>A. Teams → RMM correlation</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
let _timeFrame = 30m;
// Teams message signal 
let _teams =
    MessageEvents
    | where Timestamp > ago(14d)
    //| where SenderDisplayName contains "add keyword"
    //          or SenderDisplayName contains "add keyword"
    | extend Recipient = parse_json(RecipientDetails)
    | mv-expand Recipient
    | extend VictimAccountObjectId = tostring(Recipient.RecipientObjectId),
             VictimRecipientDisplayName = tostring(Recipient.RecipientDisplayName)
    | project
        TTime = Timestamp,
        SenderEmailAddress,
        SenderDisplayName,
        VictimRecipientDisplayName,
        VictimAccountObjectId;
// RMM launches on endpoint side
let _rmm =
    DeviceProcessEvents
    | where Timestamp > ago(14d)
    | where FileName in~ ("QuickAssist.exe", "AnyDesk.exe", "TeamViewer.exe")
    | extend VictimAccountObjectId = tostring(InitiatingProcessAccountObjectId)
    | project
        DeviceName,
        QTime = Timestamp,
        RmmTool = FileName,
        VictimAccountObjectId;
_teams
| where isnotempty(VictimAccountObjectId)
| join kind=inner _rmm on VictimAccountObjectId
| where isnotempty(DeviceName)
| where QTime between ((TTime) .. (TTime +(_timeFrame)))
| project DeviceName, SenderEmailAddress, SenderDisplayName, VictimRecipientDisplayName, VictimAccountObjectId, TTime, QTime, RmmTool
| order by QTime desc
</pre></div>


<p class="wp-block-paragraph"><strong>B. Execution</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
DeviceProcessEvents
| where Timestamp > ago(7d)
| where InitiatingProcessFileName =~ "cmd.exe"
| where FileName =~ "cmd.exe"
| where ProcessCommandLine has_all ("/S /D /c", "\" set /p=\"PK\"", "1>")
</pre></div>


<p class="wp-block-paragraph"><strong>C. ZIP → ProgramData service path → signed host sideload</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
let _timeFrame = 10m;
let _armOrDevice =
    DeviceFileEvents
    | where Timestamp > ago(14d)
    | where FolderPath has_any (
        "C:\\ProgramData\\Adobe\\ARM\\", 
        "C:\\ProgramData\\Microsoft\\DeviceSync\\",
        "D:\\ProgramData\\Adobe\\ARM\\", 
        "D:\\ProgramData\\Microsoft\\DeviceSync\\")
      and ActionType in ("FileCreated","FileRenamed")
    | project DeviceName, First=Timestamp, FileName;
let _hostRun =
    DeviceProcessEvents
    | where Timestamp > ago(14d)
    | where FileName in~ ("AcroServicesUpdater2_x64.exe","DlpUserAgent.exe","ADNotificationManager.exe")
    | project DeviceName, Run=Timestamp, Host=FileName;
_armOrDevice
| join kind=inner _hostRun on DeviceName
| where Run between (First .. (First+(_timeFrame)))
| summarize First=min(First), Run=min(Run), Files=make_set(FileName, 10) by DeviceName, Host
| order by Run desc
</pre></div>


<p class="wp-block-paragraph"><strong>D. PowerShell → high‑risk TLD → writes %AppData%/Roaming EXE</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
let _timeFrame = 5m;
let _psNet = DeviceNetworkEvents
| where Timestamp > ago(14d)
| where InitiatingProcessFileName in~ ("powershell.exe","pwsh.exe")
| where RemoteUrl matches regex @"(?i)\.(top|xyz|zip|click)$"
| project DeviceName, NetTime=Timestamp, RemoteUrl, RemoteIP;
let _exeWrite = DeviceFileEvents
| where Timestamp > ago(14d)
| where FolderPath has @"\AppData\Roaming\" and FileName endswith ".exe"
| project DeviceName, WTime=Timestamp, FileName, FolderPath, SHA256;
_psNet
| join kind=inner _exeWrite on DeviceName
| where WTime between (NetTime .. (NetTime+(_timeFrame)))
| project DeviceName, NetTime, RemoteUrl, RemoteIP, WTime, FileName, FolderPath, SHA256
| order by WTime desc
</pre></div>


<p class="wp-block-paragraph"><strong>E. Registry breadcrumbs / ASEP anomalies</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
DeviceRegistryEvents
| where Timestamp > ago(30d)
| where RegistryKey has @"\SOFTWARE\Classes\Local Settings\Software\Microsoft"
| where RegistryValueName in~ ("UCID","UFID","XJ01","XJ02","UXMP")
| project Timestamp, DeviceName, ActionType, RegistryKey, RegistryValueName, PreviousRegistryValueData, InitiatingProcessFileName
| order by Timestamp desc
</pre></div>


<p class="wp-block-paragraph"><strong>F. Non‑browser process → API‑Gateway → internal AD protocols</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
let _timeFrame = 10m;
let _net1 =
    DeviceNetworkEvents
    | where Timestamp > ago(14d)
    | where RemoteUrl has ".execute-api."
    | where InitiatingProcessFileName !in~ ("chrome.exe","msedge.exe","firefox.exe")
    | project DeviceName,
              Proc=InitiatingProcessFileName,
              OutTime=Timestamp,
              RemoteUrl,
              RemoteIP;
let _net2 =
    DeviceNetworkEvents
    | where Timestamp > ago(14d)
    | where RemotePort in (135,389,445,636)
    | project DeviceName,
              Proc=InitiatingProcessFileName,
              InTime=Timestamp,
              RemoteIP,
              RemotePort;
_net1
| join kind=inner _net2 on DeviceName, Proc
| where InTime between (OutTime .. (OutTime+(_timeFrame)))
| project DeviceName, Proc, OutTime, RemoteUrl, InTime, RemotePort
| order by InTime desc
</pre></div>


<p class="wp-block-paragraph"><strong>G. PowerShell history deletion</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
DeviceFileEvents
| where Timestamp > ago(14d)
| where FileName =~ "ConsoleHost_history.txt" and ActionType == "FileDeleted"
| project Timestamp, DeviceName, InitiatingProcessFileName, InitiatingProcessCommandLine, FolderPath
| order by Timestamp desc
</pre></div>


<p class="wp-block-paragraph"><strong>H. Reconnaissance burst (cmd / PowerShell)</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
DeviceProcessEvents
| where Timestamp > ago(14d)
| where FileName in~ ("cmd.exe","powershell.exe","pwsh.exe")
| where ProcessCommandLine has_any (
    "whoami", "whoami /all", "whoami /groups", "whoami /priv",
    "hostname", "systeminfo", "ver", "wmic os get",
    "reg query HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion",
    "query user", "net user", "nltest", "ipconfig /all", "arp -a", "route print",
    "dir", "icacls"
)
| project Timestamp, DeviceName, FileName, InitiatingProcessFileName, ProcessCommandLine
| summarize eventCount = count(), FileNames = make_set(FileName), InitiatingProcessFileNames = make_set(InitiatingProcessFileName), ProcessCommandLines = make_set(ProcessCommandLine, 5) by DeviceName
| where eventCount > 2
</pre></div>


<p class="wp-block-paragraph"><strong>I. Data Exfil</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
DeviceProcessEvents
| where Timestamp > ago(2d)
| where FileName =~ "rclone.exe" or ProcessVersionInfoOriginalFileName =~ "rclone.exe"
| where ProcessCommandLine has_all ("copy ", "--config rclone_uploader.conf", "--transfers 16", "--checkers 16", "--buffer-size 64M", "--max-age=3y", "--exclude *.mdf")
</pre></div>


<p class="wp-block-paragraph"><strong>J. Quick Assist–anchored recon (no staging writes within 10 minutes)</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
let _reconWindow = 10m; // common within 1-5 minutes
let _stageWindow = 15m; // common 1-2 minutes after recon, or less
// Anchor on RMM 
let _rmm =
    DeviceProcessEvents
    | where Timestamp > ago(14d)
    | where FileName in~ ("QuickAssist.exe", "AnyDesk.exe", "TeamViewer.exe")
    | project DeviceName, RMMTime=Timestamp;
// Recon commands within X minutes of RMM start (targeted list)
let _recon =
    DeviceProcessEvents
    | where Timestamp > ago(14d)
    | where FileName in~ ("cmd.exe","powershell.exe","pwsh.exe")
    | where ProcessCommandLine has_any (
        "whoami", "hostname", "systeminfo", "ver", "wmic os get",
        "reg query HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion",
        "query user", "net user", "nltest", "ipconfig /all", "arp -a", "route print",
        "dir", "icacls"
    )
    | project DeviceName, ReconTime=Timestamp, ReconCmd=ProcessCommandLine, ReconProc=FileName;
// Suspect staging writes (ZIP/EXE/DLL)
let _staging =
    DeviceFileEvents
    | where Timestamp > ago(14d)
    | where ActionType in ("FileCreated","FileRenamed")
    | where FileName matches regex @"(?i).*\\.(zip|exe|dll)$"
    | project DeviceName, STime=Timestamp, StageFile=FileName, StagePath=FolderPath;
// Correlate RMM + recon, then exclude cases with staging writes in the next X minutes
let _rmmRecon =
    _rmm
    | join kind=inner _recon on DeviceName
    | where ReconTime between (RMMTime .. (RMMTime+(_reconWindow)))
    | project DeviceName, RMMTime, ReconTime, ReconProc, ReconCmd;
_rmmRecon
| join kind=leftouter _staging on DeviceName
| extend HasStagingInWindow = iff(STime between (RMMTime .. (RMMTime+(_stageWindow))), 1, 0)
| summarize HasStagingInWindow=max(HasStagingInWindow) by DeviceName, RMMTime, ReconTime, ReconProc, ReconCmd
| where HasStagingInWindow == 0
| project DeviceName, RMMTime, ReconTime, ReconProc, ReconCmd
</pre></div>


<p class="wp-block-paragraph"><strong>K. Sample Correlation Query Between Chat, First Contact, and Alerts</strong></p>


<div class="wp-block-syntaxhighlighter-code "><pre class="brush: plain; title: ; notranslate">
Note. Please modify or tune for your specific environment.

let _timeFrame = 30m;      // Tune: how long after the Teams event to look for matching alerts
let _huntingWindow = 4d;   // Tune: broader lookback increases coverage but also cost
// Seed Teams message activity and normalize the victim/join fields you want to carry forward
let _teams = materialize (
    MessageEvents
    | where Timestamp > ago(_huntingWindow)
    | extend Recipient = parse_json(RecipientDetails)
    // Optional tuning: add sender/name/content filters here first to reduce volume early
    //| where SenderDisplayName contains "add keyword"
    //          or SenderDisplayName contains "add keyword"
    // add other hunting terms 
    | mv-expand Recipient
    | extend VictimAccountObjectId = tostring(Recipient.RecipientObjectId),
             VictimUPN = tostring(Recipient.RecipientSmtpAddress)
    | project
        TTime = Timestamp,
        SenderUPN = SenderEmailAddress,
        SenderDisplayName,
        VictimUPN,
        VictimAccountObjectId,
        ChatThreadId = ThreadId
);
// Distinct key sets used to prefilter downstream tables before joining
let _VictimAccountObjectId = materialize(
    _teams
    | where isnotempty(VictimAccountObjectId)
    | distinct VictimAccountObjectId
);
let _VictimUPN = materialize(
    _teams
    | where isnotempty(VictimUPN)
    | distinct VictimUPN
);
let _ChatThreadId = materialize(
    _teams
    | where isnotempty(ChatThreadId)
    | distinct ChatThreadId
);
// Find first-seen chat creation events for the chat threads already present in _teams
// Tune: add more CloudAppEvents filters here if you want to narrow to external / one-on-one / specific chat types
let _firstContact = materialize(
    CloudAppEvents
    | where Timestamp > ago(_huntingWindow)
    | where Application has "Teams"
    | where ActionType == "ChatCreated"
    | extend Raw = todynamic(RawEventData)
    | extend ChatThreadId = tostring(Raw.ChatThreadId)
    | where isnotempty(ChatThreadId)
    | join kind=innerunique (_ChatThreadId) on ChatThreadId
    | summarize FCTime = min(Timestamp) by ChatThreadId
);
// Alert branch 1: match by victim object ID
// Usually the cleanest identity join if the field is populated consistently
let _alerts_by_oid = materialize(
    AlertEvidence
    | where Timestamp > ago(_huntingWindow)
    | where AccountObjectId in (_VictimAccountObjectId)
    | project
        ATime = Timestamp,
        AlertId,
        Title,
        AccountName,
        AccountObjectId,
        AccountUpn = "",
        SourceId = "",
        ChatThreadId = ""
);
// Alert branch 2: match by victim UPN
// Useful when ObjectId is missing or alert evidence is only populated with UPN
let _alerts_by_upn = materialize(
    AlertEvidence
    | where Timestamp > ago(_huntingWindow)
    | where AccountUpn in (_VictimUPN)
    | project
        ATime = Timestamp,
        AlertId,
        Title,
        AccountName,
        AccountObjectId,
        AccountUpn,
        SourceId = "",
        ChatThreadId = ""
);
// Alert branch 3: match by chat thread ID
// Tune: this is typically the most expensive branch because it inspects AdditionalFields
let _alerts_by_thread = materialize(
    AlertEvidence
    | where Timestamp > ago(_huntingWindow)
    | where AdditionalFields has_any (_ChatThreadId)
    | extend AdditionalFields = todynamic(AdditionalFields)
    | extend
        SourceId = tostring(AdditionalFields.SourceId),
        ChatThreadIdRaw = tostring(AdditionalFields.ChatThreadId)
    | extend ChatThreadId = coalesce(
        ChatThreadIdRaw,
        extract(@"/(?:chats|channels|conversations|spaces)/([^/]+)/", 1, SourceId)
    )
    | where isnotempty(ChatThreadId)
    | join kind=innerunique (_ChatThreadId) on ChatThreadId
    | project
        ATime = Timestamp,
        AlertId,
        Title,
        AccountName,
        AccountObjectId,
        AccountUpn = "",
        SourceId,
        ChatThreadId
);
//
// add branch 4 to corrilate with host events
//
// Add first-contact context back onto the Teams seed set
let _teams_fc = materialize(
    _teams
    | join kind=leftouter _firstContact on ChatThreadId
    | extend FirstContact = isnotnull(FCTime)
);
// Join path 1: Teams victim object ID -> alert AccountObjectId
let _matches_oid =
    _teams_fc
    | where isnotempty(VictimAccountObjectId)
    | join hint.strategy=broadcast kind=leftouter (
        _alerts_by_oid
    ) on $left.VictimAccountObjectId == $right.AccountObjectId
    // Time bound keeps only alerts near the Teams activity; widen/narrow _timeFrame to tune sensitivity
    | where isnull(ATime) or ATime between (TTime .. TTime + _timeFrame)
    | extend MatchType = "ObjectId";
// Join path 2: Teams victim UPN -> alert AccountUpn
let _matches_upn =
    _teams_fc
    | where isnotempty(VictimUPN)
    | join hint.strategy=broadcast kind=leftouter (
        _alerts_by_upn
    ) on $left.VictimUPN == $right.AccountUpn
    | where isnull(ATime) or ATime between (TTime .. TTime + _timeFrame)
    | extend MatchType = "VictimUPN";
// Join path 3: Teams chat thread -> alert chat thread
let _matches_thread =
    _teams_fc
    | where isnotempty(ChatThreadId)
    | join hint.strategy=broadcast kind=leftouter (
        _alerts_by_thread
    ) on ChatThreadId
    | where isnull(ATime) or ATime between (TTime .. TTime + _timeFrame)
    | extend MatchType = "ChatThreadId";
//
// add branch 4 for host events
//
// Merge all match paths and collapse multiple alert hits per Teams event into one row
union _matches_oid, _matches_upn, _matches_thread
| summarize
    AlertTitles = make_set(Title, 50),
    AlertIds = make_set(AlertId, 50),
    MatchTypes = make_set(MatchType, 10),
    FirstAlertTime = min(ATime)
    by
        TTime,
        SenderUPN,
        SenderDisplayName,
        VictimUPN,
        VictimAccountObjectId,
        ChatThreadId
</pre></div>


<p class="wp-block-paragraph">Protecting your organization from collaboration‑based impersonation attacks as demonstrated throughout this intrusion chain, cross‑tenant helpdesk impersonation campaigns rely less on platform exploitation and more on persuading users to initiate trusted remote access workflows within legitimate enterprise collaboration tools such as Microsoft Teams. </p>



<p class="wp-block-paragraph">Organizations should treat any unsolicited external support contact as inherently suspicious and implement layered defenses that limit credential‑backed remote sessions, enforce Conditional Access with MFA and compliant device requirements, and restrict the use of administrative protocols such as WinRM to authorized management workstations. At the endpoint and identity layers, enabling Attack Surface Reduction (ASR) rules, Zero‑hour Auto Purge (ZAP), Safe Links for Teams messages, and network protection can reduce opportunities for sideloaded execution and outbound command‑and‑control activity that blend into routine HTTPS traffic. </p>



<p class="wp-block-paragraph">Finally, organizations should reinforce user education—such as establishing internal helpdesk authentication phrases and training employees to verify external tenant indicators—to prevent adversaries from converting legitimate collaboration workflows into attacker‑guided remote access and staged data exfiltration pathways. As attackers adapt their impersonation tactics, Microsoft Defender Experts continues to strengthen protections across Teams, identity, and endpoint security to help reduce risk as threats shift.</p>



<h2 class="wp-block-heading" id="references">References</h2>



<ul class="wp-block-list">
<li class="wp-block-list-item"><a href="https://techcommunity.microsoft.com/blog/MicrosoftDefenderforOffice365Blog/protection-against-email-bombs-with-microsoft-defender-for-office-365/4418048" rel="noreferrer noopener" target="_blank">Protection Against Email Bombs with Microsoft Defender for Office 365 | Microsoft Community Hub</a></li>



<li class="wp-block-list-item"><a href="https://techcommunity.microsoft.com/blog/microsoftdefenderforoffice365blog/protection-against-multi-modal-attacks-with-microsoft-defender/4438786" rel="noreferrer noopener" target="_blank">Protection against multi-modal attacks with Microsoft Defender | Microsoft Community Hub</a></li>



<li class="wp-block-list-item"><a href="https://www.microsoft.com/en-us/security/blog/2026/03/16/help-on-the-line-how-a-microsoft-teams-support-call-led-to-compromise/" rel="noreferrer noopener" target="_blank">Help on the line: How a Microsoft Teams support call led to compromise | Microsoft Security Blog</a></li>



<li class="wp-block-list-item"><a href="https://www.microsoft.com/en-us/security/blog/2025/10/07/disrupting-threats-targeting-microsoft-teams/" rel="noreferrer noopener" target="_blank">Disrupting threats targeting Microsoft Teams | Microsoft Security Blog</a></li>
</ul>



<p class="wp-block-paragraph"><em>This research is provided by Microsoft Defender Security Research with contributions from Jesse Birch, Sagar Patil, Balaji Venkatesh S (DEX), Eric Hopper, Charu Puhazholi</em>,&nbsp;<em>and other members of Microsoft Threat Intelligence.</em></p>



<h2 class="wp-block-heading" id="learn-more">Learn More</h2>



<p class="wp-block-paragraph">Review our documentation to learn more about our real-time protection capabilities and see how to enable them within your organization.  &nbsp;</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Learn more about <a href="https://learn.microsoft.com/en-us/defender-cloud-apps/ai-agent-protection" rel="noreferrer noopener" target="_blank">securing Copilot Studio agents with Microsoft Defender</a> &nbsp;<a id="_msocom_1"></a></li>



<li class="wp-block-list-item">Evaluate your AI readiness with our latest&nbsp;<a href="https://microsoft.github.io/zerotrustassessment/" rel="noreferrer noopener" target="_blank">Zero Trust for AI workshop</a>.</li>



<li class="wp-block-list-item">Learn more about <a href="https://learn.microsoft.com/en-us/defender-cloud-apps/real-time-agent-protection-during-runtime" rel="noreferrer noopener" target="_blank">Protect your agents in real-time during runtime (Preview)</a></li>



<li class="wp-block-list-item">Explore <a href="https://eurppc-word-edit.officeapps.live.com/we/%E2%80%A2%09https:/learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/copilot-studio-agent-builder" rel="noreferrer noopener" target="_blank">how to build and customize agents with Copilot Studio Agent Builder</a>&nbsp;</li>



<li class="wp-block-list-item"><a href="https://learn.microsoft.com/en-us/copilot/microsoft-365/microsoft-365-copilot-ai-security" rel="noreferrer noopener" target="_blank">Microsoft 365 Copilot AI security documentation</a>&nbsp;</li>



<li class="wp-block-list-item"><a href="https://www.microsoft.com/en-us/security/blog/2024/04/11/how-microsoft-discovers-and-mitigates-evolving-attacks-against-ai-guardrails/" rel="noreferrer noopener" target="_blank">How Microsoft discovers and mitigates evolving attacks against AI guardrails</a>&nbsp;</li>
</ul>



<p class="wp-block-paragraph"></p>
<p>The post <a href="https://www.microsoft.com/en-us/security/blog/2026/04/18/crosstenant-helpdesk-impersonation-data-exfiltration-human-operated-intrusion-playbook/">Cross‑tenant helpdesk impersonation to data exfiltration: A human-operated intrusion playbook</a> appeared first on <a href="https://www.microsoft.com/en-us/security/blog">Microsoft Security Blog</a>.</p>
