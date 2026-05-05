---
title: "Making opportunistic cyberattacks harder by design"
url: "https://www.microsoft.com/en-us/security/blog/2026/04/20/making-opportunistic-cyberattacks-harder-by-design/"
date: "Mon, 20 Apr 2026 16:00:00 +0000"
author: "Ilya Grebnov"
feed_url: "https://www.microsoft.com/en-us/security/blog/feed/"
---
<p class="wp-block-paragraph"><em>This is part of a series of blogs and interviews conducted with our </em><a href="https://www.microsoft.com/en-us/security/blog/topic/office-of-the-ciso/" rel="noreferrer noopener" target="_blank"><em>Microsoft Deputy CISOs</em></a><em>, in which we surface a number of mission-critical security recommendations and best practices that businesses can enact right now and derive real meaningful benefits from. In this article, Ilya Grebnov, Deputy CISO for Microsoft Dynamics 365 and Power Platform at Microsoft dives into cyberattacks of opportunity and how to prevent them.</em></p>



<p class="wp-block-paragraph">When your infrastructure powers thousands of organizations and millions of users, security is not a feature. It is the foundation you build everything else upon. I’m the Deputy CISO for Microsoft Dynamics 365 and Microsoft Power Platform. You may know Dynamics 365 as a cloud-based suite of intelligent business applications that unify customer relationship management (CRM) and enterprise resource planning (ERP) capabilities to help organizations manage sales, customer service, finance, supply chain, and operations more effectively. Power Platform is a low-code suite of tools that empowers both technical and non-technical users to analyze data, build custom applications, automate workflows, and create intelligent virtual agents. It does this by connecting to various data sources through Microsoft Dataverse and integrating seamlessly with not only Dynamics 365 but Microsoft 365 as well.</p>



<p class="wp-block-paragraph">What might be a little less obvious is that together, these two suites make up what is quite possibly the largest internal business group fully running on Azure at Microsoft. With such a large cloud footprint of our own, and as an important part of the broader Microsoft cloud offering, it’s highly important that we take our digital security seriously. We must remain vigilant against all manner of threats and align our defenses with <a href="https://www.microsoft.com/en-us/trust-center/security/secure-future-initiative" rel="noreferrer noopener" target="_blank">Secure Future Initiative (SFI)</a> and One Microsoft principles. </p>



<p class="wp-block-paragraph">
  I could talk for quite some time about many aspects of security, but I want to focus in on a topic I see mentioned less often than it should: avoiding attacks of opportunity. These are attacks launched by individuals who find ways into systems adjacent to our domains and who move laterally into our space. Maybe they’re looking for our data itself, or maybe they want to use our space as a means locate the company’s crown jewels elsewhere. 
</p>



<p class="wp-block-paragraph">
  To start with, I’d like to cover credential elimination, endpoint reduction, and identity controls. These are strong security practices that everyone can pick up right away. After that, I want to cover the benefits of platform engineering, which delivers some very important security advantages to organizations ready to take it on.  
</p>



<div class="wp-block-buttons is-content-justification-center is-layout-flex wp-container-core-buttons-is-layout-a89b3969 wp-block-buttons-is-layout-flex">
<div class="wp-block-button"><a class="wp-block-button__link wp-element-button" href="https://info.microsoft.com/ww-landing-subscribe-to-ciso-digest.html?lcid=en-us" rel="noreferrer noopener" target="_blank">Join the&nbsp;Microsoft CISO Digest distribution list</a></div>
</div>



<h2 class="wp-block-heading" id="credential-elimination-and-the-benefits-of-managed-identities"><strong>Credential elimination and the benefits of managed identities</strong>
</h2>



<p class="wp-block-paragraph">
  Most attackers don’t break into your network. They log in with stolen credentials. While good password hygiene helps reduce this behavior, a more reliable solution is removing credentials from the system entirely. Internally, we rely on a simple principle: if a workload can authenticate without a secret, it should. In following this principle, we have redesigned standards, retired legacy patterns, and eliminated large classes of passwords, client secrets, and API keys across our environment. The fewer credentials that exist, the fewer there are to phish, guess, reuse, or leak.
</p>



<p class="wp-block-paragraph">In practice, credential elimination is predominantly a design choice. Workloads prove who they are without a shared secret. On Azure, the primary mechanisms we use to accomplish this are managed identities (workload identities issued by <a href="https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id" rel="noreferrer noopener" target="_blank">Microsoft Entra ID</a>) and federated identity patterns that mint tokens just-in-time, with just-enough-access for a specific resource or scope. There’s nothing to store, rotate, accidentally commit to a repo, or forget to expire—which removes a significant portion of potential incident root causes tied to leaked or stale secrets. </p>



<p class="wp-block-paragraph">Because so many organizations build on our platforms, eliminating secrets in our own infrastructure is just the beginning. We have lent significant focus to making credential-free patterns available end-to-end for customers too.<strong> </strong><a href="https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/overview" rel="noreferrer noopener" target="_blank">Power Platform Managed Identity</a> (PPMI) gives Power Platform components like Dataverse plugins and Power Automate a tenant-owned identity that authenticates to Azure resources using federated credentials instead of embedded passwords or client secrets. This reduces outages from expired secrets and unblocks makers who previously needed app registrations they didn’t have permission to create. And Microsoft Entra Agent ID treats AI agents, like those created in Copilot Studio, as first class identities so they can be inventoried, governed, and bound to a human sponsor for accountability.</p>



<p class="wp-block-paragraph">
  Credential elimination pairs naturally with endpoint elimination, which is the process of reducing or removing public, inbound-reachable endpoints wherever possible. In Azure, when workloads authenticate using managed identities and call out to services, you can:
</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Front your data plane with private endpoints/private link, keeping services off the public internet.</li>



<li class="wp-block-list-item">Disable inbound administrative ports (RDP/SSH) in favor of brokered access like just-in-time, bastion, or serial console, and rely on service-to-service OAuth instead of IP-based allowlists.</li>



<li class="wp-block-list-item">Enforce least privileged access at the token level, minimizing the blast radius of misused tokens.</li>
</ul>



<p class="wp-block-paragraph">
  Together, these tactics result in fewer places for an opportunistic attacker to reach. By replacing secrets with managed identities and collapsing public surfaces, you remove the easiest paths in. There are no passwords to stuff, no shared API keys to reuse, and far fewer public surfaces to probe. Even when an attacker gets a foothold nearby, lateral movement is harder because there’s nothing reusable to log in with, and every agent/workload has a distinct, auditable identity you can shut down in seconds. 
</p>



<h2 class="wp-block-heading" id="platform-engineering-for-security"><strong>Platform engineering for security</strong>
</h2>



<p class="wp-block-paragraph">
  Opportunistic attackers thrive on inconsistency. Every exception we grant, whether it’s “this team is special,” or “that pattern is fine just this once,” spawns a snowflake architecture with unique configs, unique libraries, and unique failure modes. At small scale, those choices feel harmless. At organizational scale, they can very likely multiply risk and slow incident response. To win long term, we make opinionated decisions centrally and remove room for interpretation, transforming “do the right thing” from advice to policy.
</p>



<p class="wp-block-paragraph">
  In practice, that means standardizing on common compute and communications, disallowing brittle patterns, and enforcing the same controls everywhere. When we do that, there are fewer places for misconfigurations to hide, and far fewer opportunities for a nearby compromise to pivot laterally into our environment. 
</p>



<p class="wp-block-paragraph">
  Platform engineering delivers the most benefit at scale. I would estimate the most opportune time to take it on is when you reach 500 engineers. Start too early and you risk dampening healthy experimentation; start too late and migration, coordination, and cleanup get exponentially harder. The inflection point is when the cost of fragmentation overtakes the creative benefits of local team autonomy. That’s the moment to set paved paths, publish the guardrails, and commit to consistency.
</p>



<p class="wp-block-paragraph">
  What to line up before you flip the switch:
</p>



<ul class="wp-block-list">
<li class="wp-block-list-item">Paved paths worth choosing, including secure-by-default runtimes, libraries, and pipelines.</li>



<li class="wp-block-list-item">Policy-as-code that blocks deprecated patterns and enforces identity-based auth and networking.</li>



<li class="wp-block-list-item">Executive sponsorship to hold the line on exceptions and keep platform friction low enough that teams see the benefits of using it. </li>
</ul>



<p class="wp-block-paragraph">
  Once you’ve decided the time is right for platform engineering, the next question is “who shapes the platform and how do we make trade‑offs?” This is where security and architecture step in—not as blockers, but as partners in defining the paved paths.
</p>



<p class="wp-block-paragraph">
  Broadly speaking, product and feature teams tend to <strong>optimize for success</strong>. They want to add capabilities, integrate faster, and ship value. Platform engineering and security, on the other hand, largely focus on minimizing<strong> risk</strong>. They want to reduce dependencies, question complexity, and enforce patterns that scale safely. Here’s the important part: neither side is wrong. They’re just solving different problems. The key is finding a deliberate balance that satisfies everyone’s needs. You need enough flexibility for innovation within the right number of guardrails to prevent fragmentation and reduce your attack surface.
</p>



<p class="wp-block-paragraph">
  This mindset shift is critical because it reframes security from “the team that says no” to “the team that designs the defaults everyone can trust.” When security and platform engineering work together, the result is a foundation where controls are <strong>baked in</strong> rather than bolted on, and one where every service inherits the same protections by design.
</p>



<p class="wp-block-paragraph">
  I’ll use my own team’s process as an illustrative example of the process. We standardized compute through “core services,” the backbone our application teams use for execution and communications. The tradeoff is intentional: we get a bit less local flexibility for a lot more global safety and speed. When a new defense is needed, one team lands it in core services and over 450 services inherit it, without the need for a service-by-service campaign. That saves time, reduces duplication, and because the evidence is centralized at the platform level, it’s easier for us to both approve changes and demonstrate compliance. We applied the same approach to partitioning and disallowed patterns, to a common communication library (uniform auth, mTLS, retries, telemetry, and policy hooks), and to centralized resource management and telemetry.
</p>



<h2 class="wp-block-heading" id="resilience-consistency-and-fewer-weak-links"><strong>Resilience, consistency, and fewer weak links</strong>
</h2>



<p class="wp-block-paragraph">
  Credential elimination and platform engineering aren’t quick wins. They’re foundational moves that reshape how you can defend at scale. They demand long term coordination, but the payoff is resilience, consistency, and a dramatically smaller attack surface. Microsoft continues to innovate in this space as well. 
</p>



<p class="wp-block-paragraph">
  Concerning identity, we have delivered PPMI so organizations can securely access their Azure resources without juggling secrets or certificates—and they can use either bring-your-own or platform-managed identities. Next, we’re moving to platform provisioned identities, which are automatically created for each service, partitioned at the cell level, and scoped to the minimum privileges the service needs. Together, these steps materially reduce blast radius if an attacker gains a foothold.
</p>



<p class="wp-block-paragraph">
  Standardization is the force multiplier for platform security. Because our core services enforce consistent patterns, one change in the platform can protect all of our 450+ services. This saves time, reduces duplication, makes it easier to approve changes, and helps us demonstrate compliance because evidence is centralized at the platform level. This same uniformity is also enabling agent driven automation to help services meet SFI goals at scale—work that would be impractical in a fragmented environment. 
</p>



<p class="wp-block-paragraph">
  Underpinning all of this is the idea of paved paths: opinionated defaults that make the secure choice the easy choice. That’s how we turn security from a checklist into an enabler, and how we make attacks of opportunity far harder to pull off.
</p>



<div class="is-style-inline-centered wp-block-bloginabox-theme-promotional">
	
<div class="promotional promotional--has-media promotional--media-right">
	<div class="promotional__wrapper">
		<div class="promotional__content-wrapper">
			<div class="promotional__content">
				

<h2 class="wp-block-heading has-h-2-font-size" id="microsoftdeputy-cisos">Microsoft<br />Deputy CISOs</h2>



<p class="has-h-5-font-size wp-block-paragraph"><strong>To hear more from Microsoft Deputy CISOs, check out the&nbsp;<a href="https://www.microsoft.com/en-us/security/blog/topic/office-of-the-ciso/" rel="noreferrer noopener" target="_blank">OCISO blog series</a></strong>.</p>



<p class="wp-block-paragraph">To stay on top of important security industry updates, explore resources specifically designed for CISOs, and learn best practices for improving your organization’s security posture, join the&nbsp;<a href="https://info.microsoft.com/ww-landing-subscribe-to-ciso-digest.html?lcid=en-us" rel="noreferrer noopener" target="_blank">Microsoft CISO Digest distribution list</a>.</p>

			</div>
		</div>
					<div class="promotional__media-wrapper">
				<div class="promotional__media">
											<img alt="Man with smile on face working with laptop" class="attachment-full size-full" height="370" src="https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2025/10/Man-with-smile-on-face-working-with-laptop-2.webp" width="748" />									</div>
			</div>
			</div>
</div>
</div>



<p class="wp-block-paragraph">To learn more about Microsoft Security solutions, visit our&nbsp;<a href="https://www.microsoft.com/en-us/security/business" target="_blank">website.</a>&nbsp;Bookmark the&nbsp;<a href="https://www.microsoft.com/security/blog/" target="_blank">Security blog</a>&nbsp;to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (<a href="https://www.linkedin.com/showcase/microsoft-security/">Microsoft Security</a>) and X (<a href="https://twitter.com/@MSFTSecurity" target="_blank">@MSFTSecurity</a>)&nbsp;for the latest news and updates on cybersecurity.</p>
<p>The post <a href="https://www.microsoft.com/en-us/security/blog/2026/04/20/making-opportunistic-cyberattacks-harder-by-design/">Making opportunistic cyberattacks harder by design</a> appeared first on <a href="https://www.microsoft.com/en-us/security/blog">Microsoft Security Blog</a>.</p>
